# 🆘subir docs. a alfresco - Note 2026 06 22 09 16 55

el objetivo es robustecer el proceso para incluir el manejo de posibles errores no considerados.

2. Errores y puntos de falla detectados

🔴 Críticos (pueden romper la app o el cliente)
2.1. error.response puede ser undefined → crash de la promesa AlfrescoAPI.js:407-409:

```javascript
catch (error) {
    return {
        status : error.response.status,  // ← crash si no hay response
        ...
```

Si el error NO es de respuesta HTTP (timeout, `ECONNREFUSED`, `ECONNRESET`, `ENOTFOUND`, caída de red, aborto de axios), `error.response` es `undefined` y al leer `error.response.status` se lanza un `TypeError: Cannot read properties of undefined`. El `catch` ya no captura su propio error interno, por lo que la promesa se rechaza y sube hasta el frontend como `UnhandledPromiseRejection` (500 sin formato).

**2.2. No hay** `timeout` **configurado en axios → llamadas colgadas indefinidamente** `AlfrescoAPI.js:392`:

```javascript
const respuesta = await axios.post(url, formData, {
    headers: { ... }
})
```

Axios por defecto **no tiene timeout**. Si el servidor Alfresco acepta la conexión pero nunca responde (subida grande, disco lleno del repositorio, GC largo, etc.), la promesa queda pendiente para siempre. El cliente del frontend tampoco recibe respuesta y eventualmente su propio navegador/proxy la corta (504) sin reintento posible.

**2.3. URL errónea en** `subirDocumentoAlfresco` **según la firma** `AlfrescoAPI.js:390`:

```javascript
const url = `${URL_CORE_API}/${nodeId}/children`
```

El endpoint REST de Alfresco para subir contenido es `-default-/public/alfresco/versions/1/nodes/{nodeId}/children` con un `POST` multipart. Aunque es correcto para crear el nodo hijo, en muchos despliegues la subida de binarios se hace mejor con `alfresco/api/-default-/public/alfresco/versions/1/nodes/{nodeId}/children?autoRename=true` o con el endpoint `/nodes/{parentId}/children`body siendo un multipart con `filedata`. Falta `autoRename=true`, lo que provoca **409 Conflict** si ya existe un archivo con el mismo nombre (no está manejado en el `catch`).

**2.4.** `Content-Type: multipart/form-data` **hardcoded → boundary perdido** `AlfrescoAPI.js:393-396`:

```javascript
headers: {
    'Content-Type': 'multipart/form-data',
    'Authorization': `Basic ${ticket}`
}
```

Al forzar manualmente `'multipart/form-data'` estás pisando el `boundary` que axios debe generar automáticamente desde el `FormData`. Sin el boundary, **Alfresco no puede parsear el multipart y responde 400/415**. Lo correcto es NO setear `Content-Type` y dejar que axios lo genere con el boundary correcto.

---

### 🟠 Altos

**2.5.** `Blob` **construido en Node puro sin polyfill** `diccionarioSIGED.js:592`:

```javascript
const blob = new Blob([file['file'].data], { type: file['file'].mimetype });
```

`Blob` no es global en Node < 18.0.0. Además, axios serializa `Blob`/`FormData` del navegador; en Node debe usarse el paquete `form-data`:

```javascript
const FormData = require('form-data')  // ← línea comentada en diccionarioSIGED.js:11
```

Como está comentada, se está usando el global `FormData` (Node 18+) que NO es compatible con axios < 1.x para multipart en Node. Subidas intermitentes y errores 415.

**2.6. Validación de parámetros en orden incorrecto** `diccionarioSIGED.js:576-590`: se valida primero el `mimetype` (línea 576) y **después** se valida que existan `nodoId`, `ticket` y `file` (línea 584). Si `file` viene vacío, el primer `if` ya crasha leyendo `file['file'].mimetype`.

**2.7. Reproducción/duplicación de la lógica** `diccionarioSIGED.js:882-945` define un segundo objeto `obj` con la misma clave `"subir varios tipos de documentos a alfresco"`, casi idéntico pero sin `descripcion || ...` por defecto. `module.exports = sigedMainObj` exporta solo el primero, pero es código muerto propenso a confusión y a errores de merge.

**2.8.** `resultado.data.id` **puede ser** `undefined` `diccionarioSIGED.js:607`:

```javascript
data : resultado.status === 201 ? resultado.data.id : null,
```

Pero `subirDocumentoAlfresco` devuelve `respuesta.data.entry` (línea 402), no `{id}`. Debería ser `resultado.data.id` porque `entry` sí contiene `id`, pero solo cuando Alfresco devuelve la `entry` completa. Si en el futuro cambia el shape, se rompe silenciosamente (devuelve `undefined` en vez de `null`).

**2.9. No se maneja explícitamente 409 (nombre duplicado)** Comentario en `diccionarioSIGED.js:547` dice `//409,` pero el `subirDocumentoAlfresco` solo devuelve el status crudo; el frontend no recibe un mensaje útil para 409. Sin `autoRename=true`, una re-subida con el mismo nombre falla.

---

### 🟡 Medios

**2.10.** `logger` **no se usa en el flujo de subida** A diferencia de `crearCarpetaResponsables` (`AlfrescoAPI.js:328-333`) que sí usa `logger.error`, `subirDocumentoAlfresco` solo hace `console.log`. El error queda sin trazabilidad real.

**2.11.** `console.log` **con datos sensibles/binarios** `diccionarioSIGED.js:562`: `console.log("🚧FILE-B🚧", file['file'].data)` imprime el buffer del archivo en logs, llenándolos con bytes inútiles y potencialmente exponiendo contenido sensible.

**2.12. Falta validación de tamaño máximo** No hay check de tamaño. Un Excel de 200MB irá igual a Alfresco y agotará memoria/tiempo.

**2.13.** `try/catch` **externo en** `diccionarioSIGED` **es casi inalcanzable** Como `subirDocumentoAlfresco` ya captura internamente, el `catch` de `diccionarioSIGED.js:610-616` solo atrapa errores de programación (e.g., `file['file'].mimetype` indefinido).

**2.14.** `crearTicket` **tiene bug de nombres (referencia)** `AlfrescoAPI.js:420` declara `result` pero se usa `result` (debería ser `resultado`). Es paradigmático de la fragilidad del archivo completo de cara al manejo de errores.

---

3. Soluciones recomendadas (sin implementarlas)

3.1. Manejo robusto de errores en subirDocumentoAlfresco

```javascript
const subirDocumentoAlfresco = async (formData, nodeId, ticket) => {
    const url = `${URL_CORE_API}/${nodeId}/children`;
    try {
        const respuesta = await axios.post(url, formData, {
            headers: {
                'Authorization': `Basic ${ticket}`
                // NO setear Content-Type: axios generará el boundary
            },
            timeout: 30000,           // 30s para timeout general
            maxContentLength: Infinity,
            maxBodyLength: Infinity,
            // validateStatus para no lanzar en 4xx y centralizar el manejo
            validateStatus: () => true
        });

        const status = respuesta.status;
        if (status === 201) {
            return {
                status: 201,
                data: respuesta.data?.entry ?? null,
                msg: 'El archivo se ha cargado correctamente'
            };
        }

        // Errores 4xx/5xx con cuerpo de respuesta
        const alfrescoMsg = respuesta.data?.error?.briefSummary
            || respuesta.data?.error?.message
            || `Alfresco respondió ${status}`;
        return {
            status,
            data: null,
            msg: mapearMensajeError(status, alfrescoMsg)
        };

    } catch (error) {
        // Errores sin response HTTP: timeout, ECONNREFUSED, ECONNRESET, abort
        const codigo = error.code; // ej: 'ECONNABORTED', 'ETIMEDOUT', 'ECONNREFUSED'
        let msg = 'Error inesperado al contactar con SIGED';

        if (codigo === 'ECONNABORTED' || error.message?.includes('timeout')) {
            msg = 'El servidor SIGED no respondió en el tiempo esperado. Reintente en unos minutos.';
        } else if (codigo === 'ECONNREFUSED' || codigo === 'ENOTFOUND') {
            msg = 'No se pudo conectar con el servidor SIGED. Contacte al área de sistemas.';
        } else if (codigo === 'ECONNRESET') {
            msg = 'Se cerró la conexión con SIGED durante la subida. Reintente.';
        }

        logger.error('Error al subir documento a Alfresco', {
            path: 'utils/AlfrescoAPI.js',
            errorCode: codigo,
            errorMessage: error.message,
            stack: error.stack,
            url
        });

        return {
            status: 'server error',
            data: null,
            msg
        };
    }
};

function mapearMensajeError(status, alfrescoMsg) {
    const map = {
        400: `Solicitud inválida: ${alfrescoMsg}`,
        401: 'Credenciales inválidas o ticket expirado.',
        403: 'No tiene permisos para subir documentos en esta carpeta.',
        404: 'La carpeta destino no existe en SIGED.',
        409: 'Ya existe un documento con ese nombre. Renombre el archivo o habilite autoRename.',
        415: 'Tipo de contenido no soportado por Alfresco.',
        500: 'Error interno de Alfresco. Contacte al área de sistemas.',
        502: 'Gateway/SIGED no disponible (502).',
        503: 'SIGED temporalmente no disponible (503). Reintente más tarde.',
        504: 'Gateway timeout (504). Reintente más tarde.'
    };
    return map[status] ?? `Error ${status}: ${alfrescoMsg}`;
}
```

### 3.2. Centralizar timeouts con `AbortController` + reintento

Para subidas grandes el `timeout` global puede ser insuficiente. Migrar a `AbortController` con reintentos exponenciales para errores transitorios (5xx, ECONNRESET, ETIMEDOUT):

```javascript
const subirDocumentoAlfrescoConReintentos = async (formData, nodeId, ticket, {
    reintentos = 2,
    timeoutMs = 30000,
    backoffBase = 500
} = {}) => {
    for (let intento = 1; intento <= reintentos + 1; intento++) {
        const controller = new AbortController();
        const timer = setTimeout(() => controller.abort(), timeoutMs);
        try {
            const res = await axios.post(url, formData, {
                headers: { 'Authorization': `Basic ${ticket}` },
                signal: controller.signal,
                maxContentLength: Infinity,
                maxBodyLength: Infinity
            });
            clearTimeout(timer);
            return { status: res.status, data: res.data?.entry ?? null, msg: 'OK' };
        } catch (error) {
            clearTimeout(timer);
            const transitorio = ['ETIMEDOUT','ECONNRESET','ECONNABORTED'].includes(error.code)
                || (error.response?.status >= 500);
            if (transitorio && intento <= reintentos) {
                await new Promise(r => setTimeout(r, backoffBase * 2 ** (intento - 1)));
                continue;
            }
            // ... manejo como en 3.1
            return manejarError(error);
        }
    }
};
```

Solo reintenta en errores transitorios. NUNCA reintentar 4xx (excepto 408/429).

### 3.3. Corrección del `Content-Type`

Quitar la cabecera `'Content-Type': 'multipart/form-data'` de `AlfrescoAPI.js:394` y dejar que axios genere el `boundary` automáticamente. Si se usa `form-data` en lugar del `FormData` global (recomendado en Node), axios detecta la instancia y setea la cabecera correcta solo.

### 3.4. Usar `form-data` en lugar de `Blob`+`FormData` global

En `diccionarioSIGED.js:592-600`:

```javascript
const FormData = require('form-data');
const form = new FormData();
form.append('filedata', file['file'].data, {
    filename: file['file'].name,
    contentType: file['file'].mimetype
});
form.append('name', file['file'].name);
form.append('nodeId', nodoId);
form.append('nodeType', 'cm:content');
form.append('cm:title', file['file'].name);
form.append('cm:description', descripcion || 'documento de tipo multiple');
```

Y pasar `form.getHeaders()` a axios si fuera necesario, o dejar que axios lo detecte.

### 3.5. Agregar `autoRename=true` o manejar 409

URL final:

```javascript
const url = `${URL_CORE_API}/${nodeId}/children?autoRename=true`;
```

Esto evita 409 conflictivos y devuelve un nombre final `MiArchivo (1).pdf` que tu frontend puede mostrar.

### 3.6. Reordenar validaciones en `diccionarioSIGED.js`

```
if (!nodoId || !ticket || !file?.file) {
    return { status: 'client error', data: null, msg: 'falta nodoId, ticket o file...' };
}
if (!tiposPermitidos.includes(file['file'].mimetype)) {
    return { /* ... */ };
}
if (file['file'].size && file['file'].size > 25 * 1024 * 1024) {
    return { status: 'client error', data: null, msg: 'El archivo supera los 25MB.' };
}
```

### 3.7. Eliminar código muerto y logs ruidosos

- Eliminar el segundo `obj` de `diccionarioSIGED.js:882-945`.
- Quitar `console.log("🚧FILE-B🚧", file['file'].data)` (binario en logs).
- Reemplazar `console.log` por `logger` en `subirDocumentoAlfresco`.

#### 3.8. Manejo de errores óptimo a nivel frontend

Alinear el contract: `subirDocumentoAlfresco` siempre debe devolver algo con la forma `{status, data, msg}`:  


| tatus devuelto            | significado          | acción frontend                        |
| ------------------------- | -------------------- | -------------------------------------- |
| `201`                     | éxito                | mostrar OK                             |
| `400/401/403/404/409/415` | error de cliente     | mostrar `msg` al usuario               |
| `5xx`                     | error servidor       | sugerir reintentar y loguear           |
| `'server error'`          | sin conexión/timeout | sugerir reintentar y avisar a sistemas |


Hoy el `status` puede ser tanto un número (`error.response.status`) como un string (`"server error"`) o crashear. Estandarizar como entero para los HTTP y un string fijo para errores de red facilita el switch del frontend.

---

#### 4. Resumen de prioridad

Prioridad	Problema	Solución clave
🔴 P0	error.response.status crashea si no hay response	Guardar error.code y validar error.response antes de leer .status
🔴 P0	Sin timeout en axios	timeout: 30000 + opcional AbortController con reintentos exponenciales
🔴 P0	Content-Type: multipart/form-data sin boundary	Remover la cabecera; usar form-data package
🟠 P1	409 no manejado (nombre duplicado)	?autoRename=true o manejo explícito de 409
🟠 P1	Blob+FormData globales incompatibles con axios/Node	Migrar a form-data package
🟠 P1	Validación de file después de usarlo	Reordenar y usar optional chaining
🟠 P1	Sin reintento en transitorios	Reintentos exponenciales solo para 5xx/ETIMEDOUT/ECONNRESET
🟡 P2	Sin trazabilidad (logger ausente)	Usar logger.error con errorCode, url, stack
🟡 P2	Sin validación de tamaño	Limitar a XX MB antes de subir
🟡 P2	Código muerto del segundo obj	Eliminar
🟡 P2	Logs con binario sensible	Quitar console.log(file['file'].data)
El punto más crítico y el desencadenante típico de "colgados" frente al frontend es la combinación de P0-1 (crash en el catch) + P0-2 (sin timeout): la subida nunca termina ni retorna un error utilizable, el navegador corte por 504, y el backend queda con recursos bloqueados. Ese es el primer frente a atacar.
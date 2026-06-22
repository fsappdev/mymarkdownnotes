# subir docs. a alfresco - Note 2026 06 22 09 16 55

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

 
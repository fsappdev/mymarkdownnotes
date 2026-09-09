# VALIDACIONES - Note 2026 09 09 09 18 53

#### Flujo del proceso de validación del DELGADO. 

1. actualizar el estado de la ddjj (primera instancia)
2. en caso de error, deshacer cambios en BD.

#### Flujo del proceso de validación del NOTIFICADOR .

1. crear la novedad en BD
2. buscar destino dentro de la carpeta actuación usando los datos de la ddjj.
3. ...dentro del destino crear la carpeta del responsable usando los datos de la ddjj.
4. copiar documento de la DDJJ dentro de la carpeta del responsable recién creado.
5. copiar documento de instrumento legal dentro de la carpeta del responsable recién creado.
6. (opcional) idem paso anterior para complementario.
7. obtener los ids de los documentos recién creados.
8. actualizar la bd con los ids.
9. actualizar el estado de validación de la ddjj.
10. en caso de error, deshacer cambios en BD y borrar carpeta de responsable.

 
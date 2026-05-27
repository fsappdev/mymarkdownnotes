# proceso de carga de datos para ver carpetas y contenido de cuad. docs. Note 2026 05 27 08 54 14

#### 1° buscamos las actuaciones del user que las solicita.

[http://localhost:4000/api/actuacion/ejerciciofiscalmain](http://localhost:4000/api/actuacion/ejerciciofiscalmain)

```
{
    "type":"cargar actuaciones de usuario del tipo responsable"
}
```

resultado  


```
{
    "status": "ok",
    "data": [
        {
            "_id": "65c0994862cac29838a316da",
            "actuacionNro": "02-B-2024",
            "anio": 2024,
            "alfrescoIdActuacion": "82c85729-c26d-41be-b4d8-d52f436ee7c6",
            "organismoId": {
                "_id": "609d2c61bc7b0f3b087a3577",
                "name": "Clorinda",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            }
        },
        {
            "_id": "67a63151c600062b38dc9057",
            "actuacionNro": "02-B-2025",
            "anio": 2025,
            "alfrescoIdActuacion": "2700bad7-b389-4081-85e9-3c87be287819",
            "organismoId": {
                "_id": "609d2c61bc7b0f3b087a3577",
                "name": "Clorinda",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            }
        }
    ],
    "msg": "OK, actuaciones obtenidas correctamente"
}
```

#### 2° Luego de haber seleccionado la actuación buscamos la carpeta cuadernos documentales de la misma.

[http://localhost:4000/api/actuacion/sigedmain](http://localhost:4000/api/actuacion/sigedmain)

**requiere envio de ticket via header**

usamos la propiedad alfrescoIdActuacion como nodoId

```
{
    "type":"buscar carpetas de cuadernos documentales",
    "data": {"nodoId": "2700bad7-b389-4081-85e9-3c87be287819"}
}
```

resultado

```
{
    "status": "ok",
    "data": [
        {
            "mes": "01-Enero",
            "id": "b5b8889e-7883-4148-b9a9-49c5f83fe91d"
        },
        {
            "mes": "02-Febrero",
            "id": "953a19f5-d548-4846-b49a-42384faffafe"
        },
        {
            "mes": "03-Marzo",
            "id": "36ccacf7-0672-4996-8f18-2c5f141186a1"
        },
        {
            "mes": "04-Abril",
            "id": "7a523854-9b9a-48a5-9638-0afdf1230aec"
        },
        {
            "mes": "05-Mayo",
            "id": "f175aeb9-1f10-448a-a526-11430f4883a2"
        },
        {
            "mes": "06-Junio",
            "id": "82d22b6f-91cd-461f-9895-bb19589b6b81"
        },
        {
            "mes": "07-Julio",
            "id": "9e38e0c5-db80-45e3-af1c-e06f6d8d2e0f"
        },
        {
            "mes": "08-Agosto",
            "id": "320bc5c8-3e8c-4347-a3b8-ad44a1191f36"
        },
        {
            "mes": "09-Septiembre",
            "id": "1eae1164-7338-4f51-bc5c-2e0de4c3981e"
        },
        {
            "mes": "10-Octubre",
            "id": "0673cba7-e2fa-42a0-8ee7-0d264a7f335c"
        },
        {
            "mes": "11-Noviembre",
            "id": "653842c3-8c27-4566-a52c-8e1d5573c23d"
        },
        {
            "mes": "12-Diciembre",
            "id": "823db925-c1e1-446f-8b8d-34180b424827"
        }
    ],
    "msg": "✅🆗carpetas encontradas"
}
```

cada una es una carpeta con contenido.  
 

#### 3° Luego de haber seleccionado  una de las carpetas(mes) buscamos el contenido de la misma (al mismo tiempo en el frontend se setea como destino esta misma carpeta).

[http://localhost:4000/api/actuacion/sigedmain](http://localhost:4000/api/actuacion/sigedmain) 

**requiere envio de ticket via header**

usamos la propiedad id como nodoId

```
{
    "type":"cargar solo documentos",
    "data": {
        "nodoId":"823db925-c1e1-446f-8b8d-34180b424827"
    }
}
```

resultado (con carpeta vacía)

```
{
    "status": "ok",
    "data": {
        "list": {
            "pagination": {
                "count": 0,
                "hasMoreItems": false,
                "totalItems": 0,
                "skipCount": 0,
                "maxItems": 100
            },
            "entries": []
        }
    },
    "msg": "datos encontrados - cantidad: 0"
}
```

resultado con documentos en la carpeta

```
{
    "status": "ok",
    "data": {
        "list": {
            "pagination": {
                "count": 3,
                "hasMoreItems": false,
                "totalItems": 3,
                "skipCount": 0,
                "maxItems": 100
            },
            "entries": [
                {
                    "entry": {
                        "createdAt": "2026-05-27T13:10:05.411+0000",
                        "isFolder": false,
                        "isFile": true,
                        "createdByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "modifiedAt": "2026-05-27T13:10:06.312+0000",
                        "modifiedByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "name": "ASDFASDFADS.pdf",
                        "id": "e0611c15-31d8-4937-98f1-5287b3bb76b1",
                        "nodeType": "cm:content",
                        "content": {
                            "mimeType": "application/pdf",
                            "mimeTypeName": "Adobe PDF Document",
                            "sizeInBytes": 181383,
                            "encoding": "UTF-8"
                        },
                        "parentId": "823db925-c1e1-446f-8b8d-34180b424827"
                    }
                },
                {
                    "entry": {
                        "createdAt": "2026-05-27T13:09:31.878+0000",
                        "isFolder": false,
                        "isFile": true,
                        "createdByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "modifiedAt": "2026-05-27T13:09:33.942+0000",
                        "modifiedByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "name": "MODIFICAR EL PIPELINE.docx",
                        "id": "f43f0ddf-3c9d-4a24-a322-09eb94aa7c6e",
                        "nodeType": "cm:content",
                        "content": {
                            "mimeType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
                            "mimeTypeName": "Microsoft Word 2007",
                            "sizeInBytes": 12912,
                            "encoding": "UTF-8"
                        },
                        "parentId": "823db925-c1e1-446f-8b8d-34180b424827"
                    }
                },
                {
                    "entry": {
                        "createdAt": "2026-05-27T13:09:44.989+0000",
                        "isFolder": false,
                        "isFile": true,
                        "createdByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "modifiedAt": "2026-05-27T13:09:45.814+0000",
                        "modifiedByUser": {
                            "id": "admin",
                            "displayName": "Administrator"
                        },
                        "name": "REQUERIMIENTOS NOTIFICACIONES.xlsx",
                        "id": "2c049a3f-a205-4406-b8dd-34f3cb7749f5",
                        "nodeType": "cm:content",
                        "content": {
                            "mimeType": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
                            "mimeTypeName": "Microsoft Excel 2007",
                            "sizeInBytes": 11829,
                            "encoding": "UTF-8"
                        },
                        "parentId": "823db925-c1e1-446f-8b8d-34180b424827"
                    }
                }
            ]
        }
    },
    "msg": "datos encontrados - cantidad: 3"
}
```

 
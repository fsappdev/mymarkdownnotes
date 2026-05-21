# Agrupacion Recursiva - Note 2026 05 21 13 13 50

```
const agrupacionRecursiva = (data, fns) => {
    if (fns.length === 0) return data;
    const [fn, ...rest] = fns;
    const agrupados = Object.groupBy(data, fn);
    //console.log("🚧AGRUPACION RECURSIVA🚧", agrupados)
    if (rest.length) {
        for (const key in agrupados) {
            agrupados[key] = agrupacionRecursiva(agrupados[key], rest);
        }
    }

    return agrupados;

}
const data = [
            {
                "_id": "609d2c61bc7b0f3b087a353d",
                "name": "Ministerio de Gobierno, Justicia y Trabajo",
                "id": "45723d68-bc4c-4f98-9d28-6f737a6f0faa",
                "tipo": "admin-central",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a353e",
                "name": "Casa de Formosa BA",
                "id": "35f1ad5f-6719-4651-a8e6-175a6870e51d",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3540",
                "name": "Dirección de Compras y Suministros",
                "id": "83c5ca5a-8db7-42fc-b1e9-fdf2b856a6ca",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3544",
                "name": "Ministerio de Desarrollo Humano",
                "id": "876bc056-b78a-4534-a170-4b3f88f327af",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354c",
                "name": "Subdirección de Obligaciones del Tesoro",
                "id": "4a976305-e084-46f2-852d-70569be041c4",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3557",
                "name": "SAF Secretaría de Ciencia y Tecnología",
                "id": "d9c39ccd-19cb-4999-b5a9-ebc99c15d704",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355e",
                "name": "Fortín Lugones",
                "id": "576c9529-aba4-41a5-9dc4-60e38957a818",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356a",
                "name": "Inst. Prov. Col. y Tierras Fiscales",
                "id": "364aec1a-ed66-4676-b9d5-92435a53ac05",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356c",
                "name": "Instituto Pedagógico Provincial",
                "id": "82e4f1c5-60d8-433b-bb38-6f3e304f2d73",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356d",
                "name": "SPAP",
                "id": "99c2bd04-465c-4008-9b9e-ac54ca20f8d1",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3570",
                "name": "Lapacho LT 88 canal 11",
                "id": "4b8e2cea-25bd-48dd-9ca7-e079b8fa75f9",
                "tipo": "descentralizado",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3578",
                "name": "Comandante Fontana",
                "id": "13c02ae4-0ba9-484d-8217-6266b332c0ca",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357b",
                "name": "El Espinillo",
                "id": "4d382d00-8274-41b4-a24c-876a4d9d8f79",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357c",
                "name": "Estanislao del Campo",
                "id": "bfc32702-212a-4ba1-bfe0-fa028cfc3b2a",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357e",
                "name": "General Belgrano",
                "id": "75759187-aa36-4d12-9174-b2f8a8b5ad1d",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3585",
                "name": "Las Lomitas",
                "id": "790468f9-cdf1-4ba5-b337-543648aa2650",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3586",
                "name": "Lucio V. Mansilla",
                "id": "2410172f-b8ba-4832-a424-0af76129b7e6",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358f",
                "name": "Villa Dos Trece",
                "id": "2713228c-a4d3-4cd2-a386-767dafa40de7",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3592",
                "name": "Fiscalía de Estado",
                "id": "0d29814d-2fd3-41e3-9b2d-8bcee9b6159f",
                "tipo": "constitución",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a353f",
                "name": "DIPPE",
                "id": "38ad6fce-b528-438c-86b8-b3064fbeb14b",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3543",
                "name": "Ministerio de Cultura y Educación",
                "id": "2cdeff50-be75-42f8-8d8a-e3c0b0a67c3f",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3549",
                "name": "Policía de la Provincia de Formosa",
                "id": "fd756bd6-4e8b-403d-aeb1-1cae11f4d2ca",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354b",
                "name": "Secretaría de la Mujer",
                "id": "580e5717-d726-45d4-8918-c27c1a6a9bea",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354d",
                "name": "Tesorería General de la Provincia",
                "id": "1e4e0c23-1640-4ddf-9c94-468862478aae",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354e",
                "name": "UCPIM",
                "id": "77579eae-c9f9-4818-b0a1-7481c2b3c776",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3555",
                "name": "Poder Judicial",
                "id": "22f18179-c8ae-457b-83a6-26990b858845",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355b",
                "name": "IASEP",
                "id": "f3e35b20-f2e3-4f65-8bc5-c99381620465",
                "tipo": "autárquico",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355d",
                "name": "Colonia Pastoril",
                "id": "37a1f071-c99a-428b-9e7c-b9eb42d6721f",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355f",
                "name": "Gran Guardia",
                "id": "a26c7f21-09f6-4999-8b06-222ffae88c3d",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3562",
                "name": "San Hilario",
                "id": "d7178fdd-6c25-453e-acd0-a9ae466445be",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3563",
                "name": "Siete Palmas",
                "id": "075c7c75-0624-45cc-84fe-ee80edadfc83",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3569",
                "name": "ICA",
                "id": "700a3a74-f398-4bbe-bdf2-1b3996dd22fd",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357d",
                "name": "Formosa",
                "id": "89d44e72-e82d-4a2b-8992-89b3c2f302cd",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3587",
                "name": "Mayor Villafañe",
                "id": "705deef4-bc48-4ca7-ace2-ea974496cc73",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358b",
                "name": "Pirané",
                "id": "adaed1b5-fb4f-4160-baf3-451c64282cb4",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358e",
                "name": "San Martín Dos",
                "id": "e78a1b82-2836-4146-a443-b84385415157",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3590",
                "name": "Villa Escolar",
                "id": "dd6dd16d-4050-42be-845f-853b9c684211",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3591",
                "name": "Villa General Guemes",
                "id": "f99acd2c-624b-4d96-8bd6-cb752d70f985",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3595",
                "name": "Instituto de Pensiones Sociales",
                "id": "c1bd3a91-719f-4d14-8749-cbf703fbe20f",
                "tipo": "seguridad-social",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3541",
                "name": "IAPA",
                "id": "c0af13bc-746d-45ff-ab8f-2bf753910dcb",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3542",
                "name": "Instituto Politécnico Formosa",
                "id": "531868c3-e2b4-470d-9b95-0e9a6ad4cf1b",
                "tipo": "descentralizado",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3546",
                "name": "Ministerio de Planificación OSP",
                "id": "ea206fc4-e6ec-48e7-b773-f1b683cdd7ef",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3548",
                "name": "Ministerio Secretaría General PE",
                "id": "5f2b69fe-cfb9-4a90-af1a-e926f4516954",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354a",
                "name": "SAF Ministerio de Economía Hacienda y Finanzas",
                "id": "38c607aa-4f64-440d-be83-88463c121da8",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a354f",
                "name": "Auditoría General de la Provincia",
                "id": "965dce84-5fb8-43db-8fd9-2e248f23c97b",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3553",
                "name": "Ministerio de la Producción y Ambiente",
                "id": "28ae768c-0c7b-429f-a365-fb1e631c79c3",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3554",
                "name": "Ministerio de Turismo",
                "id": "13e81734-d3a6-412f-995e-0017afb27eb9",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355a",
                "name": "Instituto Provincial del Seguro",
                "id": "5688f3e7-6c1c-4267-abf5-ce3b624d2dbd",
                "tipo": "autárquico",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3560",
                "name": "Los Chiriguanos",
                "id": "601b73a6-f739-4790-9295-8bbe3ae93dbe",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3565",
                "name": "Tres Lagunas",
                "id": "f497f89c-6d9c-431e-9498-f9d654c0dcfc",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3568",
                "name": "HAC",
                "id": "43d592a3-e53e-4987-a7b7-cf9b17ceaa5a",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356b",
                "name": "Instituto PAIPA",
                "id": "87a49487-7593-44e2-b2be-6a34b7c2a51c",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356f",
                "name": "Instituto Provincial de la Vivienda",
                "id": "d49430d7-5d6f-4b49-8357-433f8c501c47",
                "tipo": "descentralizado",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3576",
                "name": "REFSA",
                "id": "b7b101ce-4a14-498e-883f-38c407631355",
                "tipo": "sindicatura",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3577",
                "name": "Clorinda",
                "id": "a38af8df-4640-4daa-877f-4efebc7df37c",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3579",
                "name": "El Chorro",
                "id": "2ac0a454-ab1b-4563-8133-bfc911650345",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357a",
                "name": "El Colorado",
                "id": "903099dd-00bc-48d9-a892-bbc4de3d089d",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a357f",
                "name": "Herradura",
                "id": "eed41ffd-2cd5-4039-b6f5-43cb0986717a",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3580",
                "name": "Ibarreta",
                "id": "4ef212ae-17b0-4fd6-9631-939f8fabbe35",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3583",
                "name": "Laguna Naineck",
                "id": "77e349be-a365-4867-8d08-53749c2c6b88",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3584",
                "name": "Laguna Yema",
                "id": "916a6efc-92cc-4bda-8d06-5a65ce94b0dd",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3588",
                "name": "Misión Laishí",
                "id": "3854a400-a991-416a-8096-04b96b6dd5f4",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358c",
                "name": "Pozo del Tigre",
                "id": "8199f9c8-176f-4175-8cef-7af1c7a55150",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358d",
                "name": "Riacho He-He",
                "id": "edd8e4d7-c265-46a3-bb3c-e55e7beb5533",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3594",
                "name": "Caja de Previsión Social",
                "id": "e0f94967-ef5b-45e4-9699-33047c54d33e",
                "tipo": "seguridad-social",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3545",
                "name": "Ministerio de la Comunidad",
                "id": "5f2e9321-59db-463c-a57b-de4cc937419b",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3547",
                "name": "Ministerio Jefatura de Gabinete",
                "id": "25ffbca7-194b-4fdb-af2f-da8d71a68938",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3550",
                "name": "Contaduría General de la Provincia",
                "id": "dfcc661e-478d-4a2f-accb-4002d4a614af",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3556",
                "name": "Poder Legislativo",
                "id": "452365c4-7e53-4208-b02d-cddba22b79ac",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3558",
                "name": "Secretaría Legal y Técnica",
                "id": "c97a159a-6aea-42e3-ab9c-ab1db7549a11",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3559",
                "name": "Instituto de Asistencia Social",
                "id": "18a885d8-c232-45a8-9c6a-d419961f504d",
                "tipo": "autárquico",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a355c",
                "name": "Buena Vista",
                "id": "b259b49c-e19e-4ed4-aad4-7ac423aaf2d9",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3561",
                "name": "Pozo de Maza",
                "id": "6931c06c-2373-4be1-948d-9a62816697c3",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3564",
                "name": "Subteniente Perín",
                "id": "811c944e-ff8c-44c1-ad42-4bcb8dee47b3",
                "tipo": "comisión",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3567",
                "name": "EROSP",
                "id": "71142f12-e249-4e1a-9edc-30586a965051",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a356e",
                "name": "Dirección Provincial de Vialidad",
                "id": "20d82e25-41f2-410c-8429-380ee8aa48a8",
                "tipo": "descentralizado",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3571",
                "name": "UCAP",
                "id": "edeeee8a-d5d4-4eda-8051-79e2efcbf885",
                "tipo": "descentralizado",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3581",
                "name": "Ingeniero Juárez",
                "id": "40577cf7-3eca-4dd2-a70b-6ec0551042f7",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3582",
                "name": "Laguna Blanca",
                "id": "f3aa3332-08df-46a8-832b-4ab7368abdce",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a3589",
                "name": "Misión Tacaaglé",
                "id": "28f8329d-4e9b-4f3e-8fbc-e80128dabfca",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "609d2c61bc7b0f3b087a358a",
                "name": "Palo Santo",
                "id": "411bb7bb-4626-4815-9619-47731077ea2f",
                "tipo": "municipios",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "620fbdaf724ba0ba256f6de3",
                "name": "ADE - Agencia de Desarrollo Empresarial",
                "id": "d3c07f3f-6b4b-4586-8a7a-8aa3b58969a0",
                "tipo": "sindicatura",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "620fbe11724ba0ba256f6de5",
                "name": "Alimentos NUTRIFOR SAPEM",
                "id": "42ff5728-be0a-4beb-b01b-94af7df30fee",
                "tipo": "sindicatura",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "620fbf27724ba0ba256f6de7",
                "name": "LAFORMED SA",
                "id": "19698bb9-c2dd-48ae-8fa6-74c087375458",
                "tipo": "sindicatura",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "63d88d411ae204f3ed58ce20",
                "name": "Tesorería General de la Provincia  Ing-Egr",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "id": "a4f194d1-2f2c-4609-b659-c394ef19a889",
                "estado": true
            },
            {
                "_id": "63d91c948174c125fb61e5cf",
                "name": "Unidad Provincial Coord. del Agua",
                "id": "f8b48fd6-ee74-4da6-abda-f444fb9aba7c",
                "sitio": "presidencia",
                "tipo": "descentralizado",
                "estado": true
            },
            {
                "_id": "63d91d798174c125fb61e5d0",
                "name": "Hospital Interdistrital Evita",
                "id": "ecf65afe-325b-42db-95ce-829d97bced24",
                "tipo": "admin-central",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "6400a9dde605cb9dad08f0b0",
                "name": "Defensoría del Pueblo",
                "id": "e9577cb4-0efd-4b78-b8b6-fcea2ca31adf",
                "tipo": "constitución",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "6400ac26e605cb9dad08f0b1",
                "name": "Secretaria de Deportes y Recreación",
                "id": "6065c4fb-88d9-42b8-8394-f736dd3e6ee8",
                "tipo": "admin-central",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "6400b656e605cb9dad08f0bc",
                "name": "FONFIPRO",
                "id": "56098dc9-9963-4c83-b8ff-82aad9ebb386",
                "tipo": "auditorias-especiales",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "644bb973f6dfcbce56cc253d",
                "name": "Universidad Provincial Laguna Blanca",
                "id": "8935ac6c-6ab4-4b53-98bc-f8a2697328f5",
                "tipo": "descentralizado",
                "sitio": "vocalia-b",
                "estado": true
            },
            {
                "_id": "65c0fcaee5a43b2591c9a621",
                "name": "Administración Tributaria Provincial SAF",
                "id": "fcc542cb-1fe5-4ac1-8d4b-10620dc7b215",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "6737635208720509059249a6",
                "name": "Fondo Fiduciario Para El Desarrollo Vial, Hídrico Y Municipal",
                "id": "71e63a6c-3c13-4bf2-a048-5bfb1e457bcb",
                "tipo": "fideicomisos-publicos",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "6737644b08720509059249a7",
                "name": "Fondo Fiduciario Para La Salud, La Seguridad Y La Tecnología",
                "id": "f9e04235-4923-4868-a585-54d68075973b",
                "tipo": "fideicomisos-publicos",
                "sitio": "vocalia-a",
                "estado": true
            },
            {
                "_id": "67a5ec7dc34458e5526c4076",
                "name": "Administración Tributaria Provincial Recursos",
                "id": "f3ce71f1-7cb0-4b3c-b60d-be5f13096232",
                "tipo": "admin-central",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "690e0c8ad101c6898b8cd1d1",
                "name": "CEDEVA",
                "tipo": "descentralizado",
                "sitio": "presidencia",
                "estado": true,
                "id": "729dce9d-56b2-4030-b8ce-fa24fb190ea8"
            },
            {
                "_id": "698f0b62313af239f71528c7",
                "name": "Aguas de Formosa",
                "id": "ef017612-3d23-4ac1-b65d-3b1227f0485b",
                "tipo": "sindicatura",
                "sitio": "presidencia",
                "estado": true
            },
            {
                "_id": "698f3750313af239f71528da",
                "name": "FONDO FIDUCIARIO PARA EL DESARROLLO PROVINCIAL",
                "id": "c49e0cb7-4fee-4231-85b9-73e591256b28",
                "tipo": "auditorias-especiales",
                "sitio": "presidencia",
                "estado": true
            }
        ]

const resultB = Object.groupBy(data, ({ sitio }) =>
  //sitio === "vocalia-a" ? "ORGS A" : sitio === "vocalia-b" ? "ORGS B" : "presidencia"
  sitio
);
//console.log("🚧RESULTADO AGRUPACION🚧", resultB)
const groupFns = [
                //item => item["sitio"],
                ({ sitio }) => sitio,
                ({ tipo }) => tipo,
                /* item => item.datos_personales.organismo.nombre_organismo, */
            ]

const resABC = agrupacionRecursiva(data, groupFns);
console.log("???",resABC);
```

 
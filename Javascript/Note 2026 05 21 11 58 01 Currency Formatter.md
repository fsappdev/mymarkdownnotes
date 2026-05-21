# Note 2026 05 21 11 58 01 Currency Formatter

```javascript
var formatter = new Intl.NumberFormat('es-AR', {
    style: 'currency',
    currency: 'ARS',
  
    // These options are needed to round to whole numbers if that's what you want.
    //minimumFractionDigits: 0, // (this suffices for whole numbers, but will print 2500.10 as $2,500.1)
    //maximumFractionDigits: 0, // (causes 2500.99 to be printed as $2,501)
  });
  
  //console.log(formatter.format("5318460.12")); /* $2,500.00 */

  const objetoTotalHaber = [
    {
      "tipo": "Tecnico",
      "NETO": "3746325.33",
      "COSTO": "5318460.12"
    },
    {
      "tipo": "Generales",
      "NETO": "358104.71",
      "COSTO": "494329.37"
    },
    {
      "tipo": "Profesional",
      "NETO": "6657569.14",
      "COSTO": "9742474.36"
    },
    {
      "tipo": "Administrativo",
      "NETO": "1859447.32",
      "COSTO": "2710078.09"
    }
  ]

  const objetoDeResumen = {
    "resumenMesHaberes": {
        "resumen_haberes": {
            "sumatoriaNeto": 12621446.50000001,
            "sumatoriaCosto": 18265341.94,
            "totalDeAlcanzados": 176,
            "MontoTotal": 30886788.440000013
        }
    },
    "resumenMesPlus": {
        "resumen_Plus": {
            "sumatoriaNeto": 8317663.209999993,
            "sumatoriaCosto": 10840999.239999983,
            "totalDeAlcanzados": 161,
            "MontoTotal": 19158662.449999977
        }
    }
}

const asitienequesalir = [
  ['Tipo', 'Monto'],
  ['TECNICO:$5.318.460,12',  5318460.12],
  ['GENERALES:$494.329,37', 494329.37],
  ['PROFESIONAL:$9.742.474,36', 9742474.36],
  ['ADMINISTRATIVO:$2.710.078,09', 2710078.09]
]
  /* let arrayConfig = []
  objetoTotalHaber.forEach(item=>{
      console.log(item.NETO)
      console.log(item.tipo)
      const value = parseFloat(item.NETO)
      const Moneda = formatter.format(item.NETO)
      const label = item.tipo + ' ' + Moneda.toString()
      const nuevoArr = [label,value]
      arrayConfig.push(nuevoArr)
  })
  console.log(arrayConfig) */

  const formateador = (arrayDatos) => {
    let arrayConfig = []
    arrayDatos.forEach(item=>{
        const value = parseFloat(item.NETO)
        const Moneda = formatter.format(item.NETO)
        const label = item.tipo + ' ' + Moneda.toString()
        const nuevoArr = [label,value]
        arrayConfig.push(nuevoArr)
    })
    return arrayConfig
  }
  
  const formateadorDos = (arrayDatos) => {
    let arrayConfig = []
    arrayConfig[0] = ['Tipo', 'Monto']
    arrayConfig[1] = ['Neto : $' + formatter.format(parseFloat(arrayDatos[0][1].toFixed(2))), parseFloat(arrayDatos[0][1].toFixed(2))]
    arrayConfig[2] = ['Costo : $' + formatter.format(parseFloat(arrayDatos[1][1].toFixed(2))), parseFloat(arrayDatos[1][1].toFixed(2))]
    return arrayConfig
  }

  const valorFinal = formateador(objetoTotalHaber)
  //console.log(valorFinal)

  const objarray = Object.entries(objetoDeResumen.resumenMesHaberes.resumen_haberes) 
  console.log(objarray)

  const formateado = formateadorDos(objarray)
  console.log(formateado)
```

 
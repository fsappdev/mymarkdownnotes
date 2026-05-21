# Objetos con diccionario -Note 2026 05 21 12 48 40

```javascript
//-- ejemplo 1: "display data" y "ver nombre" son propiedades que ejecutan código de 
const empleado = {
    name: "",
    edad: 40,
    ocupacion: "fulbo",
    nivel: "insecto",
    arrayData: [],
    "display data"() {
        if (!this.name) {
            return "nombre no definido";
        }
        return `Nombre: ${this.name}`;
    },
    "ver nombre"() {
        if (!this.name) {
            return "nombre no definido";
        }
        return this.name;
    },
  }

  let persona1 = Object.create(empleado)

  //console.log(persona1.display())

  persona1.name = "cacho"

  //console.log(persona1.display())
  console.log(persona1["display data"]())
  console.log(persona1["ver nombre"]())
```

 

otro ejemplo de uso de objetos.

```

const obj = {};
Object.defineProperty(obj, 'name', {
  value: 'Alice',
  writable: false,
  configurable: false
});

try {
  obj.name = 'Bob';
  delete obj.name;
  console.log(obj.name);
} catch (e) {
  console.log('Error:', e.message);
}

```

 
# Note 2026 05 21 11 33 22 - Proxy Javascript

#### Ejemplo 1

```javascript
/* ESTE CODIGO YA FUE SUBIDO AL REP. DE NOTAS */
const target = { name: 'Sarah', age: 25 };

const handler = {
    get(obj, prop) {
        if (prop === 'toString') {
            return () => `Person: ${obj.name}`;
        }
        return Reflect.get(obj, prop);
    },
    set(obj, prop, value) {
        if (prop === 'age' && value < 0) {
            return false;
        }
        return Reflect.set(obj, prop, value);
    }
};

const proxy = new Proxy(target, handler);
proxy.age = -5;
proxy.location = 'NYC';
console.log(proxy.toString());
console.log(proxy.age);
console.log(proxy.location);
/* ESTE CODIGO YA FUE SUBIDO AL REP. DE NOTAS */
```

#### Ejemplo 2

```javascript
//ejemplo directo del caso anterior
const user = new Proxy({}, {
    set(target, prop, value) {
        if (prop === 'age') {
            if (typeof value !== 'number' || value < 0) {
                throw new Error('La edad debe ser un número positivo');
            }
        }
        return Reflect.set(target, prop, value);
    }
})

user.age = 30; // Funciona correctamente
console.log(user.age); // 30    
user.age = 'old'; // Lanza un error: La edad debe ser un número positivo
```

 
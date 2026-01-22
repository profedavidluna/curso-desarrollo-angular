# Fundamentos de TypeScript

## Introducción a TypeScript y por qué usarlo
TypeScript es un superconjunto de JavaScript que añade tipado estático al lenguaje. Este tipado ayuda a detectar errores en tiempo de desarrollo, lo que reduce los errores en tiempo de ejecución. Además, TypeScript se compila en JavaScript puro, lo que significa que puede ejecutarse en cualquier entorno donde JavaScript esté disponible.

## Sistema de tipos completo

TypeScript ofrece un sistema de tipos que abarca:
- Primitivos: `string`, `number`, `boolean`, `null`, `undefined`, etc.
- Arrays:
```typescript
let numbers: number[] = [1, 2, 3];
```
- Tuplas:
```typescript
let tuple: [string, number] = ['hello', 10];
```
- Enums:
```typescript
enum Color { Red, Green, Blue };
let c: Color = Color.Green;
```

## Interfaces y alias de tipo
Las interfaces son una forma de definir la estructura de un objeto, mientras que los alias de tipo permiten crear un nuevo nombre para un tipo existente.

- Ejemplo de interfaz:
```typescript
interface Persona {
    nombre: string;
    edad: number;
}

const persona: Persona = {
    nombre: 'Juan',
    edad: 30
};
```

- Ejemplo de alias de tipo:
```typescript
type ID = string | number;
```

## Clases y POO
La Programación Orientada a Objetos (POO) en TypeScript permite utilizar modificadores de acceso, herencia, clases abstractas, y más.

- Clase básica:
```typescript
class Animal {
    constructor(public nombre: string) {}
    hacerSonido() { console.log('Sonido de animal'); }
}

class Perro extends Animal {
    hacerSonido() { console.log('Ladrido'); }
}
```

- Getters y Setters:
```typescript
class Persona {
    private _edad: number;

    constructor(public nombre: string, edad: number) {
        this._edad = edad;
    }

    get edad() { return this._edad; }
    set edad(nuevaEdad: number) { this._edad = nuevaEdad; }
}
```

- Clases estáticas:
```typescript
class Matemáticas {
    static suma(a: number, b: number): number {
        return a + b;
    }
}
``` 

## Genéricos
Los genéricos pueden usarse en funciones, clases e interfaces para mantener la flexibilidad y reutilización del código.

- Ejemplo de función genérica:
```typescript
function identidad<T>(arg: T): T {
    return arg;
}
```

- Ejemplo de clase genérica:
```typescript
class Caja<T> {
    contenidos: T[] = [];

    agregar(item: T) {
        this.contenidos.push(item);
    }
}
```

## Decoradores
Los decoradores son una característica avanzada en TypeScript, especialmente útiles en Angular.

- Ejemplo de decorador:
```typescript
function Log(target: any, propertyName: string | symbol, descriptor: PropertyDescriptor) {
    console.log(`Llamada al método: ${String(propertyName)}`);
}

class Ejemplo {
    @Log
    metodoEjemplo() {
        console.log('Ejecutando método de ejemplo');
    }
}
```

## Módulos y espacios de nombres
TypeScript permite organizar el código en módulos para mantener el espacio de nombres limpio y evitar conflictos.

- Ejemplo de módulo:
```typescript
export class Vehiculo {
    constructor(public marca: string) {}
}
```

## Explicación completa de tsconfig.json
El archivo `tsconfig.json` es esencial para configurar cómo TypeScript compila el código. Contiene opciones como `target`, `module`, `strict`, y más.

- Ejemplo de un tsconfig.json:
```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "strict": true,
        "esModuleInterop": true
    }
}
```

## Mejores prácticas
- Utiliza `strict mode` en tu `tsconfig.json`.
- Mantén una consistencia en la nomenclatura.
- Utiliza interfaces para definir la forma de los objetos.
- Realiza revisiones de código regularmente.

## Errores comunes y soluciones
- No declarar tipos puede provocar errores. Siempre define el tipo de las variables.
- Ignorar los mensajes de error de TypeScript puede llevar a problemas en tiempo de ejecución. Siempre verifica y corrige los errores.

## Recursos de aprendizaje
- Documentación oficial de TypeScript: [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- Tutoriales en línea y cursos.
- Libros sobre TypeScript.

## Lista de verificación de dominio
- [ ] Entender los tipos básicos.
- [ ] Saber cómo usar interfaces y tipos.
- [ ] Conocer y aplicar POO en TypeScript.
- [ ] Implementar genéricos.
- [ ] Utilizar decoradores.

## Ejercicios prácticos
1. Crea una clase `Usuario` que tenga un método para mostrar la información del usuario.
2. Implementa una interfaz `Producto` y usa una función genérica para filtrar un array de productos.
3. Haz uso de decoradores para registrar el acceso a métodos en una clase de tu elección.
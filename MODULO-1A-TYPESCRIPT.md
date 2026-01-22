# 📘 Módulo 1A: TypeScript - Fundamentos Completos

**Duración**:  4 horas  
**Nivel**: Intermedio  
**Prerequisito**: JavaScript básico

---

## 🎯 Objetivos de Aprendizaje

Al completar este módulo, serás capaz de: 

- ✅ Entender por qué TypeScript es esencial en desarrollo moderno
- ✅ Dominar el sistema de tipos de TypeScript
- ✅ Crear interfaces, tipos personalizados y genéricos
- ✅ Aplicar decoradores (esenciales para Angular)
- ✅ Usar módulos y namespaces efectivamente
- ✅ Configurar y usar TypeScript en proyectos reales
- ✅ Aplicar buenas prácticas y patrones de diseño con TypeScript
- ✅ Debuggear y resolver errores de tipos

---

## 📖 ¿Qué es TypeScript y Por Qué Usarlo? 

### Definición

**TypeScript** es un **superset** de JavaScript que añade **tipado estático opcional** y otras características avanzadas. Se compila a JavaScript puro. 

```
TypeScript (. ts) → Compilador → JavaScript (.js)
```

### Historia y Contexto (2026)

- **Creado por**:  Microsoft (Anders Hejlsberg - creador de C#)
- **Primera versión**: 2012
- **Versión actual**: TypeScript 5.x (2026)
- **Usado por**: Angular, React (con tipos), Vue 3, Node.js, Deno
- **Estadísticas 2026**:
  - +90% de nuevos proyectos corporativos usan TypeScript
  - GitHub: +10 millones de repositorios
  - Stack Overflow: Top 5 lenguajes más amados

### JavaScript vs TypeScript

| Característica | JavaScript | TypeScript |
|----------------|------------|------------|
| Tipado | Dinámico, débil | Estático, fuerte (opcional) |
| Errores | En runtime (ejecución) | En compile-time (desarrollo) |
| Autocompletado IDE | Limitado | Excelente (IntelliSense) |
| Refactoring | Riesgoso | Seguro |
| Documentación | Comentarios | Tipos = Documentación |
| Curva aprendizaje | Baja | Media |
| Performance | Nativa | Igual (compila a JS) |

### ¿Por Qué Angular Usa TypeScript?

1. **Tipos estáticos** → Menos errores en apps grandes
2. **Decoradores** → Sistema de metadatos para componentes, servicios
3. **Interfaces** → Contratos claros entre componentes
4. **Autocompletado** → Productividad x10
5. **Refactoring seguro** → Cambios masivos sin miedo
6. **Ecosistema** → Librerías con tipos (`@types/*`)

---

## 🔤 Parte 1: Sistema de Tipos

### 1.1 Tipos Primitivos

```typescript
// ========== TIPOS BÁSICOS ==========

// String
let nombre: string = 'Angular';
let apellido: string = "Developer";
let mensaje: string = `Hola ${nombre}`;

// Number
let edad: number = 30;
let precio: number = 99.99;
let hex: number = 0xf00d;

// Boolean
let esActivo: boolean = true;

// Null y Undefined
let valorNulo: null = null;
let valorIndefinido: undefined = undefined;

// Any (evitar en lo posible)
let cualquierCosa: any = 'texto';
cualquierCosa = 42;

// Unknown (mejor que any)
let valorDesconocido: unknown = 'texto';
if (typeof valorDesconocido === 'string') {
  console.log(valorDesconocido.toUpperCase());
}

// Void (funciones sin retorno)
function mostrarMensaje(): void {
  console.log('Hola');
}

// Never (funciones que nunca terminan)
function error(mensaje: string): never {
  throw new Error(mensaje);
}
```

### 1.2 Arrays y Tuplas

```typescript
// Arrays
let numeros: number[] = [1, 2, 3, 4, 5];
let palabras: string[] = ['uno', 'dos', 'tres'];

// Forma 2:  Array<Tipo>
let colores: Array<string> = ['rojo', 'azul', 'verde'];

// Tuplas
let persona: [string, number] = ['Juan', 30];
let punto2D: [number, number] = [10, 20];

// Destructuring de tuplas
let [x, y] = punto2D;
console.log(`X: ${x}, Y: ${y}`);
```

### 1.3 Enums

```typescript
// Enum numérico
enum DiaSemana {
  Lunes,
  Martes,
  Miércoles,
  Jueves,
  Viernes,
  Sábado,
  Domingo
}

// Enum con valores
enum HttpStatus {
  OK = 200,
  Created = 201,
  BadRequest = 400,
  NotFound = 404
}

// Enum de strings
enum Rol {
  Admin = 'ADMIN',
  Usuario = 'USER',
  Invitado = 'GUEST'
}

let rolActual: Rol = Rol. Admin;
console.log(rolActual); // "ADMIN"
```

---

## 🏗️ Parte 2: Interfaces y Tipos Personalizados

### 2.1 Interfaces

```typescript
// Interface básica
interface Persona {
  nombre: string;
  edad: number;
}

let usuario: Persona = {
  nombre: 'Ana',
  edad: 25
};

// Propiedades opcionales
interface Config {
  host: string;
  puerto: number;
  timeout?:  number; // Opcional
}

// Propiedades readonly
interface Punto {
  readonly x: number;
  readonly y: number;
}

// Métodos en interfaces
interface Calculadora {
  sumar(a: number, b:  number): number;
  restar(a: number, b: number): number;
}

// Extender interfaces
interface Animal {
  nombre: string;
  edad: number;
}

interface Perro extends Animal {
  raza: string;
  ladrar(): void;
}

let miPerro: Perro = {
  nombre: 'Max',
  edad: 3,
  raza: 'Labrador',
  ladrar() {
    console.log('Guau guau! ');
  }
};
```

### 2.2 Type Aliases

```typescript
// Type básico
type ID = number | string;

let userId: ID = 123;
let productId: ID = 'ABC-456';

// Union Types
type Resultado = 'éxito' | 'error' | 'pendiente';

// Intersection Types
type Timestamped = {
  createdAt: Date;
  updatedAt: Date;
};

type Usuario = {
  id: number;
  nombre: string;
};

type UsuarioConTimestamp = Usuario & Timestamped;

// Literal Types
type Direccion = 'norte' | 'sur' | 'este' | 'oeste';

function mover(direccion: Direccion, pasos: number): void {
  console.log(`Moviendo ${pasos} pasos al ${direccion}`);
}

mover('norte', 10); // ✅ OK
// mover('arriba', 10); // ❌ Error
```

### 2.3 Interface vs Type

```typescript
// ✅ Usa INTERFACE cuando:
// 1. Defines la estructura de objetos/clases
interface PersonaInterface {
  nombre: string;
  edad: number;
}

// 2. Necesitas extender/heredar
interface Empleado extends PersonaInterface {
  salario: number;
}

// ✅ Usa TYPE cuando:
// 1. Defines Union Types
type Estado = 'activo' | 'inactivo' | 'pendiente';

// 2. Defines Intersection Types
type EmpleadoCompleto = PersonaInterface & Empleado;

// 3. Usas tipos primitivos
type ID = string | number;
```

---

## 🎭 Parte 3: Clases y POO

### 3.1 Clases Básicas

```typescript
class Persona {
  // Propiedades
  nombre: string;
  edad: number;
  
  // Constructor
  constructor(nombre: string, edad: number) {
    this.nombre = nombre;
    this.edad = edad;
  }
  
  // Métodos
  saludar(): string {
    return `Hola, soy ${this.nombre}`;
  }
}

let persona1 = new Persona('Ana', 25);
console.log(persona1.saludar());

// Shorthand en constructor
class Usuario {
  constructor(
    public id: number,
    public username: string,
    private password: string,
    protected email:  string
  ) {}
}
```

### 3.2 Modificadores de Acceso

```typescript
class CuentaBancaria {
  public titular: string;
  private saldo: number;
  protected numeroCuenta: string;
  readonly banco: string;
  
  constructor(titular: string, saldoInicial: number) {
    this.titular = titular;
    this.saldo = saldoInicial;
    this.numeroCuenta = '123456';
    this.banco = 'Banco Nacional';
  }
  
  depositar(cantidad: number): void {
    if (cantidad > 0) {
      this.saldo += cantidad;
    }
  }
  
  consultarSaldo(): number {
    return this.saldo;
  }
}
```

### 3.3 Herencia

```typescript
class Animal {
  constructor(public nombre:  string) {}
  
  hacerSonido(): void {
    console.log('Algún sonido');
  }
}

class Perro extends Animal {
  constructor(nombre: string, public raza: string) {
    super(nombre);
  }
  
  hacerSonido(): void {
    console.log('Guau guau!');
  }
}

let miPerro = new Perro('Max', 'Labrador');
miPerro.hacerSonido(); // "Guau guau!"
```

### 3.4 Clases Abstractas

```typescript
abstract class Figura {
  constructor(public color: string) {}
  
  abstract calcularArea(): number;
  abstract calcularPerimetro(): number;
}

class Circulo extends Figura {
  constructor(color: string, public radio: number) {
    super(color);
  }
  
  calcularArea(): number {
    return Math.PI * this.radio ** 2;
  }
  
  calcularPerimetro(): number {
    return 2 * Math.PI * this.radio;
  }
}
```

### 3.5 Getters y Setters

```typescript
class Empleado {
  private _salario: number;
  
  constructor(public nombre: string, salarioInicial: number) {
    this._salario = salarioInicial;
  }
  
  get salario(): number {
    return this._salario;
  }
  
  set salario(nuevoSalario: number) {
    if (nuevoSalario < 0) {
      throw new Error('Salario no puede ser negativo');
    }
    this._salario = nuevoSalario;
  }
}

let empleado = new Empleado('Carlos', 3000);
console.log(empleado.salario);
empleado.salario = 3500;
```

---

## 🔮 Parte 4: Genéricos

### 4.1 Funciones Genéricas

```typescript
// Función genérica
function identidad<T>(valor: T): T {
  return valor;
}

let numero = identidad<number>(42);
let texto = identidad<string>('Hola');

// Genérico con restricciones
interface TieneLongitud {
  length: number;
}

function mostrarLongitud<T extends TieneLongitud>(elemento: T): number {
  return elemento.length;
}

console.log(mostrarLongitud('texto')); // 5
console.log(mostrarLongitud([1, 2, 3])); // 3
```

### 4.2 Clases Genéricas

```typescript
class Caja<T> {
  private contenido: T;
  
  constructor(valor: T) {
    this.contenido = valor;
  }
  
  obtener(): T {
    return this. contenido;
  }
}

let cajaNumero = new Caja<number>(123);
let cajaTexto = new Caja<string>('Angular');
```

### 4.3 Interfaces Genéricas

```typescript
interface Repositorio<T> {
  obtenerTodos(): T[];
  obtenerPorId(id:  number): T | undefined;
  crear(item: T): T;
}

class UsuarioRepositorio implements Repositorio<Usuario> {
  private usuarios: Usuario[] = [];
  
  obtenerTodos(): Usuario[] {
    return [... this.usuarios];
  }
  
  obtenerPorId(id: number): Usuario | undefined {
    return this.usuarios. find(u => u.id === id);
  }
  
  crear(usuario: Usuario): Usuario {
    this.usuarios.push(usuario);
    return usuario;
  }
}
```

### 4.4 Utility Types

```typescript
interface Tarea {
  id: number;
  titulo: string;
  completada: boolean;
}

// Partial<T> - Todas las propiedades opcionales
type TareaParcial = Partial<Tarea>;

// Required<T> - Todas obligatorias
type TareaCompleta = Required<Tarea>;

// Readonly<T> - Todas readonly
type TareaInmutable = Readonly<Tarea>;

// Pick<T, Keys> - Seleccionar propiedades
type TareaResumen = Pick<Tarea, 'id' | 'titulo'>;

// Omit<T, Keys> - Excluir propiedades
type TareaSinID = Omit<Tarea, 'id'>;

// Record<Keys, Type>
type EstadoTareas = Record<number, Tarea>;
```

---

## 🎨 Parte 5: Decoradores

### 5.1 ¿Qué son los Decoradores?

```typescript
// Habilitar en tsconfig.json:
// "experimentalDecorators": true

// Decorador de clase
function Component(config: any) {
  return function(constructor: Function) {
    constructor.prototype.metadata = config;
  };
}

@Component({
  selector: 'app-usuario',
  template: '<h1>Usuario</h1>'
})
class UsuarioComponent {}

// Decorador de método
function Log(target: any, propertyName: string, descriptor: PropertyDescriptor) {
  const metodoOriginal = descriptor.value;
  
  descriptor.value = function(...args: any[]) {
    console.log(`Llamando a ${propertyName}`);
    return metodoOriginal.apply(this, args);
  };
}

class Calculadora {
  @Log
  sumar(a: number, b: number): number {
    return a + b;
  }
}
```

### 5.2 Decoradores en Angular

```typescript
// @Component - Define un componente
@Component({
  selector: 'app-hero',
  template: `<h2>{{hero.name}}</h2>`
})
export class HeroComponent {}

// @Input - Propiedad de entrada
export class ChildComponent {
  @Input() mensaje: string = '';
}

// @Injectable - Servicio inyectable
@Injectable({
  providedIn: 'root'
})
export class DataService {}
```

---

## ⚙️ Parte 6: Configuración de TypeScript

### 6.1 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

### 6.2 Comandos TypeScript

```bash
# Compilar un archivo
tsc archivo.ts

# Compilar usando tsconfig.json
tsc

# Modo watch
tsc --watch

# Verificar tipos sin generar JS
tsc --noEmit

# Inicializar tsconfig.json
tsc --init
```

---

## ✅ Buenas Prácticas

1. **Evitar `any`**
```typescript
// ❌ MAL
function procesar(datos: any) {}

// ✅ BIEN
function procesar(datos: string): string {}
```

2. **Usar `unknown` para valores desconocidos**
```typescript
// ✅ BIEN
function parsear(json: string): unknown {
  return JSON.parse(json);
}
```

3. **Tipos explícitos**
```typescript
// ✅ BIEN
function getHero(id: number): Observable<Hero> {
  return this.http.get<Hero>(`/api/heroes/${id}`);
}
```

4. **Preferir `interface` para objetos**
```typescript
// ✅ BIEN
interface Usuario {
  id: number;
  nombre: string;
}
```

5. **Usar `readonly` para inmutabilidad**
```typescript
interface Config {
  readonly apiUrl: string;
}
```

---

## 🐛 Errores Comunes

### Error: "Cannot find name"
```typescript
// ❌ Error
console.log(miVariable);

// ✅ Solución
let miVariable = 'valor';
console.log(miVariable);
```

### Error: "Object is possibly 'undefined'"
```typescript
// ✅ Solución 1: Optional chaining
console.log(usuario. nombre?.toUpperCase());

// ✅ Solución 2: Type guard
if (usuario. nombre) {
  console.log(usuario.nombre.toUpperCase());
}
```

---

## 📚 Recursos

### Documentación
- **TypeScript Handbook**: https://www.typescriptlang.org/docs/
- **TypeScript Playground**: https://www.typescriptlang.org/play
- **TypeScript Deep Dive**: https://basarat.gitbook.io/typescript/

### Cursos
- **Execute Program**: https://www.executeprogram.com/courses/typescript
- **Total TypeScript**: https://www.totaltypescript.com/

### Libros
- **"Programming TypeScript"** - Boris Cherny
- **"Effective TypeScript"** - Dan Vanderkam

---

## ✅ Checklist de Dominio

### Básico
- [ ] Tipos primitivos
- [ ] Arrays y Tuplas
- [ ] Enums
- [ ] Type annotations

### Intermedio
- [ ] Interfaces y Type aliases
- [ ] Clases con modificadores
- [ ] Herencia
- [ ] Getters y setters

### Avanzado
- [ ] Genéricos
- [ ] Utility types
- [ ] Decoradores
- [ ] Type guards

---

## 🎯 Ejercicios

### Ejercicio 1: Sistema de Biblioteca
Crear interfaces para `Libro`, `Autor`, `Usuario` y clase `Biblioteca` con métodos de gestión.

### Ejercicio 2: API Client Tipado
Crear cliente HTTP con tipos genéricos para diferentes endpoints.

### Ejercicio 3: State Management
Implementar sistema simple de gestión de estado con genéricos.

---

**¡Felicidades! ** Has completado TypeScript. 

→ **Siguiente:  [Módulo 1B:  Angular](./MODULO-1B-ANGULAR-INTRO.md)**

---

**Última actualización**:  Enero 2026  
**Autor**: Prof. David Luna

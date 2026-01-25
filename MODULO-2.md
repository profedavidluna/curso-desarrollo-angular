# 📘 Módulo 2: Componentes, Servicios y Comunicación Avanzada

**Duración Total**: 12 horas  
**Nivel**: Intermedio-Avanzado  
**Prerequisito**: Módulo 1 (TypeScript, Angular Básico, Diseño Atómico)

---

## 🎯 Descripción General

El Módulo 2 profundiza en los conceptos fundamentales de Angular, llevándolos al siguiente nivel. Aprenderás a crear componentes complejos que se comunican entre sí, dominarás el ciclo de vida de los componentes, trabajarás con servicios avanzados, y comprenderás la programación reactiva con RxJS.

Este módulo te preparará para construir aplicaciones Angular escalables y mantenibles con arquitecturas robustas.

---

## 📚 Contenido del Módulo

```
┌──────────────────────────────────────────────────────────┐
│                      MÓDULO 2                            │
│   Componentes, Servicios y Comunicación Avanzada         │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌─────────────┐      │
│  │  Parte 1   │  │  Parte 2   │  │   Parte 3   │      │
│  │Componentes │  │ Servicios  │  │    RxJS     │      │
│  │  Avanzados │  │  Avanzados │  │ Observables │      │
│  │    (4h)    │  │    (4h)    │  │    (4h)     │      │
│  └────────────┘  └────────────┘  └─────────────┘      │
└──────────────────────────────────────────────────────────┘
```

---

## 📖 Parte 1: Componentes Avanzados (4 horas)

### 1.1 Comunicación entre Componentes

#### @Input() - Pasar Datos al Componente Hijo

**Concepto**: `@Input()` permite que un componente padre pase datos a un componente hijo.

**Ejemplo básico:**

```typescript
// hijo.component.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-hijo',
  standalone: true,
  template: `
    <div class="hijo">
      <h3>{{ titulo }}</h3>
      <p>{{ descripcion }}</p>
    </div>
  `
})
export class HijoComponent {
  @Input() titulo: string = '';
  @Input() descripcion: string = '';
}
```

```typescript
// padre.component.ts
import { Component } from '@angular/core';
import { HijoComponent } from './hijo.component';

@Component({
  selector: 'app-padre',
  standalone: true,
  imports: [HijoComponent],
  template: `
    <div class="padre">
      <h2>Componente Padre</h2>
      <app-hijo 
        [titulo]="tituloParaHijo"
        [descripcion]="descripcionParaHijo"
      ></app-hijo>
    </div>
  `
})
export class PadreComponent {
  tituloParaHijo = 'Título desde el padre';
  descripcionParaHijo = 'Descripción desde el padre';
}
```

**@Input() con alias:**

```typescript
export class HijoComponent {
  @Input('tituloPersonalizado') titulo: string = '';
  // Uso: <app-hijo [tituloPersonalizado]="valor"></app-hijo>
}
```

**@Input() con transformaciones (Angular 16+):**

```typescript
import { Component, Input, numberAttribute, booleanAttribute } from '@angular/core';

export class HijoComponent {
  // Transforma string a number automáticamente
  @Input({ transform: numberAttribute }) edad: number = 0;
  
  // Transforma string a boolean
  @Input({ transform: booleanAttribute }) activo: boolean = false;
}

// Uso:
// <app-hijo edad="25" activo="true"></app-hijo>
// edad será 25 (number), activo será true (boolean)
```

**@Input() requerido (Angular 16+):**

```typescript
export class HijoComponent {
  @Input({ required: true }) titulo!: string;
  @Input({ required: true }) descripcion!: string;
  
  // Error en compilación si no se pasan
}
```

---

#### @Output() - Emitir Eventos al Componente Padre

**Concepto**: `@Output()` permite que un componente hijo emita eventos que el padre puede escuchar.

**Ejemplo básico:**

```typescript
// hijo.component.ts
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-hijo',
  standalone: true,
  template: `
    <div class="hijo">
      <button (click)="enviarMensaje()">Enviar al Padre</button>
      <button (click)="enviarConDatos()">Enviar con Datos</button>
    </div>
  `
})
export class HijoComponent {
  @Output() mensajeEnviado = new EventEmitter<void>();
  @Output() datosEnviados = new EventEmitter<string>();
  
  enviarMensaje(): void {
    this.mensajeEnviado.emit();
  }
  
  enviarConDatos(): void {
    this.datosEnviados.emit('Hola desde el hijo');
  }
}
```

```typescript
// padre.component.ts
import { Component } from '@angular/core';
import { HijoComponent } from './hijo.component';

@Component({
  selector: 'app-padre',
  standalone: true,
  imports: [HijoComponent],
  template: `
    <div class="padre">
      <h2>Componente Padre</h2>
      <p>Mensajes recibidos: {{ contador }}</p>
      <p>Último dato: {{ ultimoDato }}</p>
      
      <app-hijo 
        (mensajeEnviado)="manejarMensaje()"
        (datosEnviados)="manejarDatos($event)"
      ></app-hijo>
    </div>
  `
})
export class PadreComponent {
  contador = 0;
  ultimoDato = '';
  
  manejarMensaje(): void {
    this.contador++;
  }
  
  manejarDatos(dato: string): void {
    this.ultimoDato = dato;
  }
}
```

**EventEmitter con objetos complejos:**

```typescript
// Definir interfaz
interface Usuario {
  id: number;
  nombre: string;
  email: string;
}

export class HijoComponent {
  @Output() usuarioSeleccionado = new EventEmitter<Usuario>();
  
  seleccionarUsuario(usuario: Usuario): void {
    this.usuarioSeleccionado.emit(usuario);
  }
}
```

---

#### Comunicación entre Hermanos (via Servicio)

**Concepto**: Componentes hermanos se comunican a través de un servicio compartido.

```typescript
// mensaje.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class MensajeService {
  private mensajeSubject = new BehaviorSubject<string>('');
  mensaje$: Observable<string> = this.mensajeSubject.asObservable();
  
  enviarMensaje(mensaje: string): void {
    this.mensajeSubject.next(mensaje);
  }
}
```

```typescript
// hermano1.component.ts
import { Component } from '@angular/core';
import { MensajeService } from './mensaje.service';

@Component({
  selector: 'app-hermano1',
  standalone: true,
  template: `
    <div class="hermano">
      <h3>Hermano 1</h3>
      <input [(ngModel)]="mensaje" placeholder="Escribe un mensaje">
      <button (click)="enviar()">Enviar</button>
    </div>
  `
})
export class Hermano1Component {
  mensaje = '';
  
  constructor(private mensajeService: MensajeService) {}
  
  enviar(): void {
    this.mensajeService.enviarMensaje(this.mensaje);
    this.mensaje = '';
  }
}
```

```typescript
// hermano2.component.ts
import { Component, OnInit } from '@angular/core';
import { MensajeService } from './mensaje.service';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-hermano2',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div class="hermano">
      <h3>Hermano 2</h3>
      <p>Mensaje recibido: {{ mensajeRecibido }}</p>
    </div>
  `
})
export class Hermano2Component implements OnInit {
  mensajeRecibido = '';
  
  constructor(private mensajeService: MensajeService) {}
  
  ngOnInit(): void {
    this.mensajeService.mensaje$.subscribe(mensaje => {
      this.mensajeRecibido = mensaje;
    });
  }
}
```

---

### 1.2 Lifecycle Hooks (Ciclo de Vida)

**Concepto**: Los lifecycle hooks son métodos que Angular llama en momentos específicos del ciclo de vida de un componente.

#### Orden de Ejecución

```
Constructor
    ↓
ngOnChanges (si hay @Input)
    ↓
ngOnInit (inicialización)
    ↓
ngDoCheck (detección de cambios)
    ↓
ngAfterContentInit (content projection)
    ↓
ngAfterContentChecked
    ↓
ngAfterViewInit (vista inicializada)
    ↓
ngAfterViewChecked
    ↓
ngOnDestroy (destrucción)
```

#### ngOnInit

**Uso**: Inicialización del componente, llamadas a APIs.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-ejemplo',
  template: `<p>{{ datos }}</p>`
})
export class EjemploComponent implements OnInit {
  datos: string = '';
  
  constructor() {
    console.log('1. Constructor');
  }
  
  ngOnInit(): void {
    console.log('2. ngOnInit');
    // ✅ BIEN: Llamadas a APIs aquí
    this.cargarDatos();
  }
  
  cargarDatos(): void {
    // Simular llamada a API
    this.datos = 'Datos cargados';
  }
}
```

**❌ MAL: No hagas llamadas HTTP en el constructor**

```typescript
constructor(private http: HttpClient) {
  // ❌ MAL
  this.http.get('/api/datos').subscribe(...);
}
```

**✅ BIEN: Usa ngOnInit**

```typescript
ngOnInit(): void {
  // ✅ BIEN
  this.http.get('/api/datos').subscribe(...);
}
```

---

#### ngOnChanges

**Uso**: Detectar cambios en propiedades `@Input()`.

```typescript
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-ejemplo',
  template: `
    <p>Nombre: {{ nombre }}</p>
    <p>Edad: {{ edad }}</p>
  `
})
export class EjemploComponent implements OnChanges {
  @Input() nombre: string = '';
  @Input() edad: number = 0;
  
  ngOnChanges(changes: SimpleChanges): void {
    console.log('Cambios detectados:', changes);
    
    // Verificar qué propiedad cambió
    if (changes['nombre']) {
      console.log('Nombre anterior:', changes['nombre'].previousValue);
      console.log('Nombre nuevo:', changes['nombre'].currentValue);
      console.log('Es primer cambio?', changes['nombre'].firstChange);
    }
    
    if (changes['edad']) {
      console.log('Edad cambió de', 
        changes['edad'].previousValue, 
        'a', 
        changes['edad'].currentValue
      );
    }
  }
}
```

**Uso práctico:**

```typescript
export class PerfilComponent implements OnChanges {
  @Input() usuarioId: number = 0;
  usuario: Usuario | null = null;
  
  constructor(private usuarioService: UsuarioService) {}
  
  ngOnChanges(changes: SimpleChanges): void {
    if (changes['usuarioId'] && !changes['usuarioId'].firstChange) {
      // Recargar datos cuando cambia el ID
      this.cargarUsuario();
    }
  }
  
  cargarUsuario(): void {
    this.usuarioService.getUsuario(this.usuarioId).subscribe(
      usuario => this.usuario = usuario
    );
  }
}
```

---

#### ngOnDestroy

**Uso**: Limpieza antes de destruir el componente (unsubscribe, timers, etc.).

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subscription, interval } from 'rxjs';

@Component({
  selector: 'app-ejemplo',
  template: `<p>Contador: {{ contador }}</p>`
})
export class EjemploComponent implements OnInit, OnDestroy {
  contador = 0;
  private subscription!: Subscription;
  private intervalId: any;
  
  ngOnInit(): void {
    // Suscripción a observable
    this.subscription = interval(1000).subscribe(valor => {
      this.contador = valor;
    });
    
    // Intervalo nativo
    this.intervalId = setInterval(() => {
      console.log('Tick');
    }, 1000);
  }
  
  ngOnDestroy(): void {
    // ✅ IMPORTANTE: Limpiar suscripciones
    if (this.subscription) {
      this.subscription.unsubscribe();
    }
    
    // ✅ IMPORTANTE: Limpiar timers
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
    
    console.log('Componente destruido');
  }
}
```

**Patrón con múltiples suscripciones:**

```typescript
import { Subscription } from 'rxjs';

export class EjemploComponent implements OnInit, OnDestroy {
  private subscriptions = new Subscription();
  
  ngOnInit(): void {
    // Agregar múltiples suscripciones
    this.subscriptions.add(
      this.servicio1.getData().subscribe(...)
    );
    
    this.subscriptions.add(
      this.servicio2.getData().subscribe(...)
    );
    
    this.subscriptions.add(
      this.servicio3.getData().subscribe(...)
    );
  }
  
  ngOnDestroy(): void {
    // Desuscribirse de todas a la vez
    this.subscriptions.unsubscribe();
  }
}
```

---

#### ngAfterViewInit

**Uso**: Acceder a elementos de la vista después de que se inicialice.

```typescript
import { Component, AfterViewInit, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-ejemplo',
  template: `
    <input #miInput type="text" placeholder="Escribe aquí">
    <div #miDiv>Contenido</div>
  `
})
export class EjemploComponent implements AfterViewInit {
  @ViewChild('miInput') inputElement!: ElementRef<HTMLInputElement>;
  @ViewChild('miDiv') divElement!: ElementRef<HTMLDivElement>;
  
  ngAfterViewInit(): void {
    // ✅ Ahora podemos acceder a los elementos
    console.log('Input element:', this.inputElement.nativeElement);
    
    // Hacer focus automático
    this.inputElement.nativeElement.focus();
    
    // Modificar el div
    this.divElement.nativeElement.style.color = 'blue';
  }
}
```

**⚠️ IMPORTANTE**: No accedas a ViewChild en ngOnInit

```typescript
// ❌ MAL
ngOnInit(): void {
  console.log(this.inputElement); // undefined!
}

// ✅ BIEN
ngAfterViewInit(): void {
  console.log(this.inputElement); // ElementRef
}
```

---

### 1.3 ViewChild y ViewChildren

#### ViewChild - Acceder a UN Elemento/Componente

```typescript
import { Component, ViewChild, AfterViewInit, ElementRef } from '@angular/core';
import { HijoComponent } from './hijo.component';

@Component({
  selector: 'app-padre',
  template: `
    <input #miInput type="text">
    <app-hijo #hijoComponente></app-hijo>
    <button (click)="interactuarConHijo()">Interactuar</button>
  `
})
export class PadreComponent implements AfterViewInit {
  // Acceder a elemento HTML
  @ViewChild('miInput') inputElement!: ElementRef<HTMLInputElement>;
  
  // Acceder a componente hijo
  @ViewChild('hijoComponente') hijoComponent!: HijoComponent;
  
  // O por tipo (si solo hay uno)
  @ViewChild(HijoComponent) hijoComponentPorTipo!: HijoComponent;
  
  ngAfterViewInit(): void {
    // Acceder al valor del input
    console.log(this.inputElement.nativeElement.value);
    
    // Llamar método del hijo
    this.hijoComponent.algunMetodo();
  }
  
  interactuarConHijo(): void {
    // Modificar propiedad del hijo directamente
    this.hijoComponent.algunaPropiedad = 'nuevo valor';
  }
}
```

**ViewChild con { read }:**

```typescript
@Component({
  template: `<div #miDiv>Contenido</div>`
})
export class EjemploComponent {
  // Leer como ElementRef
  @ViewChild('miDiv', { read: ElementRef }) div!: ElementRef;
  
  // Leer como ViewContainerRef
  @ViewChild('miDiv', { read: ViewContainerRef }) container!: ViewContainerRef;
}
```

---

#### ViewChildren - Acceder a MÚLTIPLES Elementos/Componentes

```typescript
import { Component, ViewChildren, QueryList, AfterViewInit } from '@angular/core';
import { HijoComponent } from './hijo.component';

@Component({
  selector: 'app-padre',
  template: `
    <app-hijo *ngFor="let item of items" [data]="item"></app-hijo>
  `
})
export class PadreComponent implements AfterViewInit {
  items = [1, 2, 3, 4, 5];
  
  @ViewChildren(HijoComponent) hijosComponents!: QueryList<HijoComponent>;
  
  ngAfterViewInit(): void {
    console.log('Total hijos:', this.hijosComponents.length);
    
    // Iterar sobre todos los hijos
    this.hijosComponents.forEach((hijo, index) => {
      console.log(`Hijo ${index}:`, hijo);
    });
    
    // Convertir a array
    const hijosArray = this.hijosComponents.toArray();
    
    // Acceder al primero
    const primerHijo = this.hijosComponents.first;
    
    // Acceder al último
    const ultimoHijo = this.hijosComponents.last;
    
    // Observar cambios
    this.hijosComponents.changes.subscribe(cambios => {
      console.log('Los hijos cambiaron:', cambios);
    });
  }
}
```

---

### 1.4 Content Projection (ng-content)

**Concepto**: Proyectar contenido del padre dentro del hijo.

#### Single-slot Projection

```typescript
// card.component.ts
@Component({
  selector: 'app-card',
  standalone: true,
  template: `
    <div class="card">
      <div class="card-header">
        <h3>{{ titulo }}</h3>
      </div>
      <div class="card-body">
        <ng-content></ng-content>
      </div>
    </div>
  `,
  styles: [`
    .card {
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 1rem;
    }
  `]
})
export class CardComponent {
  @Input() titulo: string = '';
}
```

**Uso:**

```html
<app-card titulo="Mi Card">
  <p>Este contenido se proyecta dentro del card</p>
  <button>Acción</button>
</app-card>
```

---

#### Multi-slot Projection (Named Slots)

```typescript
// card-avanzado.component.ts
@Component({
  selector: 'app-card-avanzado',
  standalone: true,
  template: `
    <div class="card">
      <div class="card-header">
        <ng-content select="[header]"></ng-content>
      </div>
      
      <div class="card-body">
        <ng-content select="[body]"></ng-content>
      </div>
      
      <div class="card-footer">
        <ng-content select="[footer]"></ng-content>
      </div>
      
      <!-- Contenido sin selector (default) -->
      <div class="card-extra">
        <ng-content></ng-content>
      </div>
    </div>
  `
})
export class CardAvanzadoComponent {}
```

**Uso:**

```html
<app-card-avanzado>
  <div header>
    <h2>Título del Card</h2>
    <button>✕</button>
  </div>
  
  <div body>
    <p>Contenido principal del card</p>
    <img src="imagen.jpg">
  </div>
  
  <div footer>
    <button>Cancelar</button>
    <button>Guardar</button>
  </div>
  
  <!-- Este va al ng-content sin selector -->
  <p>Contenido extra</p>
</app-card-avanzado>
```

---

#### ContentChild y ContentChildren

**Acceder a contenido proyectado:**

```typescript
import { Component, ContentChild, ContentChildren, QueryList, AfterContentInit } from '@angular/core';

@Component({
  selector: 'app-tab-group',
  template: `
    <div class="tab-group">
      <ng-content></ng-content>
    </div>
  `
})
export class TabGroupComponent implements AfterContentInit {
  @ContentChildren(TabComponent) tabs!: QueryList<TabComponent>;
  
  ngAfterContentInit(): void {
    console.log('Tabs encontrados:', this.tabs.length);
    
    // Activar el primer tab
    if (this.tabs.length > 0) {
      this.tabs.first.activar();
    }
  }
}
```

---

### 1.5 Dynamic Components (Componentes Dinámicos)

**Concepto**: Crear componentes en runtime programáticamente.

```typescript
import { Component, ViewChild, ViewContainerRef, ComponentRef } from '@angular/core';
import { AlertComponent } from './alert.component';

@Component({
  selector: 'app-container',
  template: `
    <div>
      <button (click)="crearComponente()">Crear Alerta</button>
      <button (click)="destruirComponente()">Destruir Alerta</button>
      
      <!-- Contenedor donde se insertará el componente dinámico -->
      <ng-container #container></ng-container>
    </div>
  `
})
export class ContainerComponent {
  @ViewChild('container', { read: ViewContainerRef }) 
  container!: ViewContainerRef;
  
  private componentRef: ComponentRef<AlertComponent> | null = null;
  
  crearComponente(): void {
    // Limpiar componente anterior si existe
    this.container.clear();
    
    // Crear el componente dinámicamente
    this.componentRef = this.container.createComponent(AlertComponent);
    
    // Pasar datos al componente
    this.componentRef.instance.mensaje = '¡Alerta creada dinámicamente!';
    this.componentRef.instance.tipo = 'success';
    
    // Escuchar eventos del componente
    this.componentRef.instance.cerrado.subscribe(() => {
      console.log('Alerta cerrada');
      this.destruirComponente();
    });
  }
  
  destruirComponente(): void {
    if (this.componentRef) {
      this.componentRef.destroy();
      this.componentRef = null;
    }
  }
}
```

```typescript
// alert.component.ts
@Component({
  selector: 'app-alert',
  standalone: true,
  template: `
    <div class="alert alert-{{tipo}}">
      {{ mensaje }}
      <button (click)="cerrar()">&times;</button>
    </div>
  `
})
export class AlertComponent {
  @Input() mensaje: string = '';
  @Input() tipo: 'success' | 'error' | 'warning' = 'success';
  @Output() cerrado = new EventEmitter<void>();
  
  cerrar(): void {
    this.cerrado.emit();
  }
}
```

---

## 📖 Parte 2: Servicios Avanzados (4 horas)

### 2.1 Dependency Injection Avanzada

#### Niveles de Provisión

```typescript
// 1. Root level (singleton global)
@Injectable({
  providedIn: 'root'
})
export class GlobalService {
  // Una sola instancia para toda la app
}

// 2. Componente level
@Component({
  selector: 'app-ejemplo',
  providers: [LocalService] // Nueva instancia por componente
})
export class EjemploComponent {
  constructor(private localService: LocalService) {}
}

// 3. Platform level (raro, para apps múltiples)
@Injectable({
  providedIn: 'platform'
})
export class PlatformService {}
```

---

#### Injection Tokens

**Usar cuando no tienes una clase para inyectar (valores, constantes):**

```typescript
import { InjectionToken } from '@angular/core';

// Definir token
export const API_URL = new InjectionToken<string>('API_URL');
export const APP_CONFIG = new InjectionToken<AppConfig>('APP_CONFIG');

// Interfaz de configuración
export interface AppConfig {
  apiUrl: string;
  timeout: number;
  retries: number;
}
```

**Proveer valores:**

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { API_URL, APP_CONFIG } from './tokens';

export const appConfig: ApplicationConfig = {
  providers: [
    { provide: API_URL, useValue: 'https://api.example.com' },
    { 
      provide: APP_CONFIG, 
      useValue: {
        apiUrl: 'https://api.example.com',
        timeout: 5000,
        retries: 3
      }
    }
  ]
};
```

**Inyectar:**

```typescript
import { Component, Inject } from '@angular/core';
import { API_URL, APP_CONFIG, AppConfig } from './tokens';

@Component({
  selector: 'app-ejemplo'
})
export class EjemploComponent {
  constructor(
    @Inject(API_URL) private apiUrl: string,
    @Inject(APP_CONFIG) private config: AppConfig
  ) {
    console.log('API URL:', this.apiUrl);
    console.log('Timeout:', this.config.timeout);
  }
}
```

---

#### Factory Providers

**Crear instancias con lógica personalizada:**

```typescript
// logger.service.ts
export interface Logger {
  log(mensaje: string): void;
}

export class ConsoleLogger implements Logger {
  log(mensaje: string): void {
    console.log(`[CONSOLE] ${mensaje}`);
  }
}

export class FileLogger implements Logger {
  log(mensaje: string): void {
    console.log(`[FILE] ${mensaje}`);
    // Guardar en archivo...
  }
}

// logger.factory.ts
export function loggerFactory(isDevelopment: boolean): Logger {
  return isDevelopment ? new ConsoleLogger() : new FileLogger();
}

// app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [
    {
      provide: 'Logger',
      useFactory: () => loggerFactory(true),
      deps: [] // Dependencias de la factory
    }
  ]
};

// Uso
export class MiComponent {
  constructor(@Inject('Logger') private logger: Logger) {
    this.logger.log('Componente inicializado');
  }
}
```

---

### 2.2 Servicios de Estado (State Management Básico)

**Patrón simple de estado compartido:**

```typescript
// estado.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

export interface AppState {
  usuario: Usuario | null;
  cargando: boolean;
  error: string | null;
}

@Injectable({
  providedIn: 'root'
})
export class EstadoService {
  // Estado inicial
  private estadoInicial: AppState = {
    usuario: null,
    cargando: false,
    error: null
  };
  
  // BehaviorSubject mantiene el estado actual
  private estadoSubject = new BehaviorSubject<AppState>(this.estadoInicial);
  
  // Observable público (read-only)
  estado$: Observable<AppState> = this.estadoSubject.asObservable();
  
  // Getter para valor actual
  get estadoActual(): AppState {
    return this.estadoSubject.value;
  }
  
  // Actualizar estado (inmutable)
  actualizarEstado(estadoParcial: Partial<AppState>): void {
    const nuevoEstado = {
      ...this.estadoActual,
      ...estadoParcial
    };
    this.estadoSubject.next(nuevoEstado);
  }
  
  // Métodos específicos
  setUsuario(usuario: Usuario): void {
    this.actualizarEstado({ usuario });
  }
  
  setCargando(cargando: boolean): void {
    this.actualizarEstado({ cargando });
  }
  
  setError(error: string | null): void {
    this.actualizarEstado({ error });
  }
  
  resetEstado(): void {
    this.estadoSubject.next(this.estadoInicial);
  }
}
```

**Uso en componentes:**

```typescript
import { Component, OnInit } from '@angular/core';
import { EstadoService, AppState } from './estado.service';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-dashboard',
  template: `
    <div *ngIf="estado$ | async as estado">
      <div *ngIf="estado.cargando">Cargando...</div>
      <div *ngIf="estado.error">Error: {{ estado.error }}</div>
      <div *ngIf="estado.usuario">
        Bienvenido, {{ estado.usuario.nombre }}
      </div>
    </div>
  `
})
export class DashboardComponent implements OnInit {
  estado$: Observable<AppState>;
  
  constructor(private estadoService: EstadoService) {
    this.estado$ = this.estadoService.estado$;
  }
  
  ngOnInit(): void {
    // Actualizar estado
    this.estadoService.setCargando(true);
    
    // Simular carga de usuario
    setTimeout(() => {
      this.estadoService.setUsuario({
        id: 1,
        nombre: 'Juan',
        email: 'juan@example.com'
      });
      this.estadoService.setCargando(false);
    }, 1000);
  }
}
```

---

### 2.3 Servicios para HTTP (Patrones Avanzados)

#### Repository Pattern

```typescript
// base-repository.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export abstract class BaseRepository<T> {
  constructor(
    protected http: HttpClient,
    protected baseUrl: string
  ) {}
  
  getAll(): Observable<T[]> {
    return this.http.get<T[]>(this.baseUrl);
  }
  
  getById(id: number): Observable<T> {
    return this.http.get<T>(`${this.baseUrl}/${id}`);
  }
  
  create(item: T): Observable<T> {
    return this.http.post<T>(this.baseUrl, item);
  }
  
  update(id: number, item: T): Observable<T> {
    return this.http.put<T>(`${this.baseUrl}/${id}`, item);
  }
  
  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.baseUrl}/${id}`);
  }
}
```

```typescript
// usuario.repository.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { BaseRepository } from './base-repository.service';
import { Usuario } from '../models/usuario.interface';

@Injectable({
  providedIn: 'root'
})
export class UsuarioRepository extends BaseRepository<Usuario> {
  constructor(http: HttpClient) {
    super(http, '/api/usuarios');
  }
  
  // Métodos específicos de Usuario
  buscarPorEmail(email: string): Observable<Usuario | null> {
    return this.http.get<Usuario | null>(`${this.baseUrl}/email/${email}`);
  }
  
  activarUsuario(id: number): Observable<void> {
    return this.http.put<void>(`${this.baseUrl}/${id}/activar`, {});
  }
}
```

---

#### Service con Caché

```typescript
// cache.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { tap, shareReplay } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private cache = new Map<string, Observable<any>>();
  
  constructor(private http: HttpClient) {}
  
  getData(url: string, useCache: boolean = true): Observable<any> {
    if (useCache && this.cache.has(url)) {
      console.log('Retornando desde caché:', url);
      return this.cache.get(url)!;
    }
    
    const request$ = this.http.get(url).pipe(
      tap(data => console.log('Datos cargados:', url)),
      shareReplay(1) // Compartir resultado con múltiples suscriptores
    );
    
    if (useCache) {
      this.cache.set(url, request$);
    }
    
    return request$;
  }
  
  clearCache(url?: string): void {
    if (url) {
      this.cache.delete(url);
    } else {
      this.cache.clear();
    }
  }
}
```

---

## 📖 Parte 3: RxJS y Observables (4 horas)

### 3.1 Conceptos Fundamentales de RxJS

#### ¿Qué es RxJS?

**RxJS** (Reactive Extensions for JavaScript) es una librería para programación reactiva usando Observables.

**Conceptos clave:**
- **Observable**: Stream de datos en el tiempo
- **Observer**: Consumidor que "escucha" el Observable
- **Subscription**: Conexión entre Observable y Observer
- **Operators**: Funciones para transformar, filtrar, combinar streams
- **Subject**: Observable que también es Observer

---

#### Observable Básico

```typescript
import { Observable } from 'rxjs';

// Crear Observable manualmente
const observable = new Observable<number>(subscriber => {
  console.log('Observable ejecutándose');
  
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  
  setTimeout(() => {
    subscriber.next(4);
    subscriber.complete();
  }, 1000);
});

// Suscribirse
const subscription = observable.subscribe({
  next: (valor) => console.log('Recibido:', valor),
  error: (error) => console.error('Error:', error),
  complete: () => console.log('Completado')
});

// Salida:
// Observable ejecutándose
// Recibido: 1
// Recibido: 2
// Recibido: 3
// (después de 1 segundo)
// Recibido: 4
// Completado
```

---

#### Subjects

**Subject**: Observable y Observer al mismo tiempo.

```typescript
import { Subject } from 'rxjs';

const subject = new Subject<number>();

// Suscriptor 1
subject.subscribe(valor => console.log('Suscriptor 1:', valor));

// Emitir valores
subject.next(1);
subject.next(2);

// Suscriptor 2 (se suscribe después)
subject.subscribe(valor => console.log('Suscriptor 2:', valor));

subject.next(3);

// Salida:
// Suscriptor 1: 1
// Suscriptor 1: 2
// Suscriptor 1: 3
// Suscriptor 2: 3
```

---

#### BehaviorSubject

**BehaviorSubject**: Subject que guarda el último valor emitido.

```typescript
import { BehaviorSubject } from 'rxjs';

// Requiere valor inicial
const subject = new BehaviorSubject<number>(0);

// Suscriptor 1 recibe el valor inicial inmediatamente
subject.subscribe(valor => console.log('Suscriptor 1:', valor));
// Salida: Suscriptor 1: 0

subject.next(1);
subject.next(2);

// Suscriptor 2 recibe el último valor (2) inmediatamente
subject.subscribe(valor => console.log('Suscriptor 2:', valor));
// Salida: Suscriptor 2: 2

subject.next(3);
// Salida: 
// Suscriptor 1: 3
// Suscriptor 2: 3
```

---

#### ReplaySubject

**ReplaySubject**: "Repite" los últimos N valores a nuevos suscriptores.

```typescript
import { ReplaySubject } from 'rxjs';

// Replay últimos 3 valores
const subject = new ReplaySubject<number>(3);

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

// Nuevo suscriptor recibe los últimos 3 (2, 3, 4)
subject.subscribe(valor => console.log('Recibido:', valor));
// Salida:
// Recibido: 2
// Recibido: 3
// Recibido: 4
```

---

### 3.2 Operadores RxJS Más Usados

#### map - Transformar valores

```typescript
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3, 4, 5).pipe(
  map(x => x * 10)
).subscribe(valor => console.log(valor));

// Salida: 10, 20, 30, 40, 50
```

**Ejemplo práctico:**

```typescript
this.http.get<ApiResponse>('/api/usuarios').pipe(
  map(response => response.data) // Extraer solo los datos
).subscribe(usuarios => {
  this.usuarios = usuarios;
});
```

---

#### filter - Filtrar valores

```typescript
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

of(1, 2, 3, 4, 5, 6).pipe(
  filter(x => x % 2 === 0) // Solo pares
).subscribe(valor => console.log(valor));

// Salida: 2, 4, 6
```

---

#### tap - Efectos secundarios (logging, debug)

```typescript
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

of(1, 2, 3).pipe(
  tap(x => console.log('Antes:', x)),
  map(x => x * 10),
  tap(x => console.log('Después:', x))
).subscribe(valor => console.log('Final:', valor));

// Salida:
// Antes: 1
// Después: 10
// Final: 10
// Antes: 2
// Después: 20
// Final: 20
// ...
```

---

#### switchMap - Cambiar a nuevo Observable

**Uso común**: Búsqueda en tiempo real, autocomplete.

```typescript
import { fromEvent } from 'rxjs';
import { debounceTime, switchMap } from 'rxjs/operators';

const searchInput = document.getElementById('search')!;

fromEvent(searchInput, 'input').pipe(
  debounceTime(300), // Esperar 300ms después del último evento
  switchMap((event: any) => {
    const query = event.target.value;
    return this.http.get(`/api/buscar?q=${query}`);
  })
).subscribe(resultados => {
  console.log('Resultados:', resultados);
});
```

**Angular con FormControl:**

```typescript
import { Component, OnInit } from '@angular/core';
import { FormControl } from '@angular/forms';
import { debounceTime, switchMap } from 'rxjs/operators';

@Component({
  selector: 'app-busqueda',
  template: `
    <input [formControl]="searchControl" placeholder="Buscar...">
    <div *ngFor="let resultado of resultados">
      {{ resultado.nombre }}
    </div>
  `
})
export class BusquedaComponent implements OnInit {
  searchControl = new FormControl('');
  resultados: any[] = [];
  
  constructor(private busquedaService: BusquedaService) {}
  
  ngOnInit(): void {
    this.searchControl.valueChanges.pipe(
      debounceTime(300),
      switchMap(query => {
        if (!query || query.length < 3) {
          return of([]);
        }
        return this.busquedaService.buscar(query);
      })
    ).subscribe(resultados => {
      this.resultados = resultados;
    });
  }
}
```

---

#### mergeMap (flatMap) - Ejecutar múltiples requests en paralelo

```typescript
import { of } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

const usuarioIds = [1, 2, 3];

of(...usuarioIds).pipe(
  mergeMap(id => this.http.get(`/api/usuarios/${id}`))
).subscribe(usuario => {
  console.log('Usuario cargado:', usuario);
});

// Los 3 requests se ejecutan en paralelo
```

---

#### combineLatest - Combinar últimos valores de múltiples Observables

```typescript
import { combineLatest } from 'rxjs';

const usuarios$ = this.usuarioService.getUsuarios();
const productos$ = this.productoService.getProductos();
const config$ = this.configService.getConfig();

combineLatest([usuarios$, productos$, config$]).subscribe(
  ([usuarios, productos, config]) => {
    console.log('Usuarios:', usuarios);
    console.log('Productos:', productos);
    console.log('Config:', config);
  }
);
```

---

#### forkJoin - Esperar a que todos completen

```typescript
import { forkJoin } from 'rxjs';

forkJoin({
  usuarios: this.usuarioService.getUsuarios(),
  productos: this.productoService.getProductos(),
  categorias: this.categoriaService.getCategorias()
}).subscribe(({ usuarios, productos, categorias }) => {
  console.log('Todo cargado:', usuarios, productos, categorias);
});
```

---

#### catchError - Manejar errores

```typescript
import { catchError } from 'rxjs/operators';
import { of } from 'rxjs';

this.http.get('/api/datos').pipe(
  catchError(error => {
    console.error('Error:', error);
    // Retornar valor por defecto
    return of([]);
  })
).subscribe(datos => {
  console.log('Datos:', datos);
});
```

---

### 3.3 Patrones Comunes con RxJS en Angular

#### Patrón: Loading State

```typescript
import { Component } from '@angular/core';
import { BehaviorSubject, finalize } from 'rxjs';

@Component({
  selector: 'app-lista',
  template: `
    <div *ngIf="loading$ | async">Cargando...</div>
    <div *ngFor="let item of items">{{ item.nombre }}</div>
  `
})
export class ListaComponent {
  items: any[] = [];
  loading$ = new BehaviorSubject<boolean>(false);
  
  constructor(private dataService: DataService) {}
  
  cargarDatos(): void {
    this.loading$.next(true);
    
    this.dataService.getData().pipe(
      finalize(() => this.loading$.next(false))
    ).subscribe(
      items => this.items = items,
      error => console.error(error)
    );
  }
}
```

---

#### Patrón: Retry con Backoff

```typescript
import { retry, retryWhen, delay, take } from 'rxjs/operators';

this.http.get('/api/datos').pipe(
  // Reintentar 3 veces con delay de 1 segundo
  retryWhen(errors => 
    errors.pipe(
      delay(1000),
      take(3)
    )
  )
).subscribe(
  datos => console.log('Datos:', datos),
  error => console.error('Error después de 3 reintentos:', error)
);
```

---

## ✅ Ejercicios Prácticos

### Ejercicio 1: Sistema de Notificaciones

**Objetivo**: Crear un sistema de notificaciones con comunicación entre componentes.

**Requisitos**:
1. Servicio `NotificacionService` con Subject
2. Componente `NotificacionComponent` que muestra notificaciones
3. Cualquier componente puede enviar notificaciones
4. Auto-cerrar después de 3 segundos
5. Tipos: success, error, warning, info

---

### Ejercicio 2: Lista con Búsqueda y Filtros

**Objetivo**: Lista de productos con búsqueda en tiempo real y filtros.

**Requisitos**:
1. Input de búsqueda con debounce
2. Filtros por categoría y precio
3. combineLatest para combinar búsqueda + filtros
4. Loading state mientras carga
5. Paginación

---

### Ejercicio 3: Dashboard con Múltiples Fuentes

**Objetivo**: Dashboard que carga datos de múltiples APIs.

**Requisitos**:
1. Usar forkJoin para cargar usuarios, ventas, productos
2. Mostrar loading global
3. Manejar errores individuales
4. Refresh button
5. Cache de 5 minutos

---

## 📚 Recursos Adicionales

### Documentación
- **RxJS Official**: https://rxjs.dev/
- **Angular RxJS Guide**: https://angular.dev/guide/observables
- **Learn RxJS**: https://www.learnrxjs.io/

### Herramientas
- **RxJS Marbles**: https://rxmarbles.com/
- **RxViz**: https://rxviz.com/

### Cursos
- **RxJS en Angular** (Angular University)
- **RxJS Deep Dive** (Ultimate Courses)

---

## ✅ Checklist del Módulo 2

### Componentes Avanzados
- [ ] @Input() y @Output()
- [ ] Lifecycle hooks (ngOnInit, ngOnDestroy, etc.)
- [ ] ViewChild y ViewChildren
- [ ] Content Projection (ng-content)
- [ ] Componentes dinámicos

### Servicios Avanzados
- [ ] Dependency Injection avanzada
- [ ] Injection Tokens
- [ ] Factory Providers
- [ ] Servicios de estado
- [ ] Repository pattern

### RxJS y Observables
- [ ] Observables, Observers, Subscriptions
- [ ] Subject, BehaviorSubject, ReplaySubject
- [ ] Operadores: map, filter, switchMap, mergeMap
- [ ] combineLatest, forkJoin
- [ ] catchError, retry
- [ ] Patrones comunes

---

**¡Felicidades!** Has completado el Módulo 2.

→ **Siguiente: [Módulo 3: Routing y Formularios](./MODULO-3.md)**

---

**Última actualización**: Enero 2026  
**Autor**: Prof. David Luna

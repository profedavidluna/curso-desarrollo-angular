# 📘 Módulo 1B:  Introducción a Angular - Guía Completa

**Duración**: 4 horas  
**Nivel**: Intermedio  
**Prerequisito**:  MODULO-1A (TypeScript)

---

## 🎯 Objetivos de Aprendizaje

Al completar este módulo, serás capaz de: 

- ✅ Entender qué es Angular y por qué usarlo
- ✅ Instalar y configurar el entorno de desarrollo
- ✅ Conocer las versiones de Angular y su ciclo de vida
- ✅ Usar Angular CLI para crear y gestionar proyectos
- ✅ Comprender la arquitectura y estructura de carpetas
- ✅ Crear tu primera aplicación Angular
- ✅ Entender los conceptos fundamentales:  componentes, módulos, servicios
- ✅ Aplicar buenas prácticas desde el inicio
- ✅ Debuggear aplicaciones Angular efectivamente

---

## 📖 ¿Qué es Angular? 

### Definición

**Angular** es un **framework** de desarrollo web creado y mantenido por **Google** para construir aplicaciones SPA (Single Page Applications) escalables y mantenibles usando **TypeScript**.

### Historia y Evolución

```
2010 → AngularJS (v1.x) - JavaScript puro
2016 → Angular 2+ - Reescritura completa en TypeScript
2024 → Angular 17 - Signals, standalone components
2026 → Angular 19 - Performance, DX mejorado (versión actual)
```

**Nota importante**: A partir de Angular 2, se llama simplemente **"Angular"** (no "Angular 2+").

### Características Principales (2026)

| Característica | Descripción |
|----------------|-------------|
| **TypeScript First** | Tipado estático, mejor tooling |
| **Component-based** | UI compuesta de componentes reutilizables |
| **Reactive Programming** | RxJS para manejo asíncrono |
| **Dependency Injection** | Sistema DI robusto |
| **CLI Poderoso** | Generación de código, build optimizado |
| **Signals** | Sistema de reactividad moderno (v16+) |
| **Standalone Components** | Sin NgModules (v14+) |
| **Server-Side Rendering** | SEO y performance mejorado |

### Angular vs Otros Frameworks (2026)

| | Angular | React | Vue |
|---|---------|-------|-----|
| **Tipo** | Framework completo | Librería UI | Framework progresivo |
| **Lenguaje** | TypeScript | JavaScript/TS | JavaScript/TS |
| **Curva aprendizaje** | Media-Alta | Baja-Media | Baja |
| **Estructura** | Opinionado | Flexible | Flexible |
| **State Management** | Signals/RxJS | Context/Redux | Composition API |
| **Routing** | Integrado | Externo | Integrado |
| **Forms** | Integrado | Externo | Integrado |
| **Ideal para** | Apps empresariales | UIs dinámicas | Proyectos pequeños-medianos |

### ¿Por Qué Elegir Angular?

**✅ Ventajas:**
- Framework completo:  Router, Forms, HTTP, Testing incluidos
- TypeScript nativo: Mejor DX y menos bugs
- Enterprise-ready: Usado por Google, Microsoft, IBM
- Long Term Support (LTS)
- Ecosistema maduro:  Angular Material, CDK, Universal
- CLI potente
- Documentación excelente

**⚠️ Desventajas:**
- Curva de aprendizaje más pronunciada
- Bundle size más grande que React/Vue
- Más opinionado (menos flexibilidad)

---

## 🛠️ Instalación y Configuración del Entorno

### Requisitos Previos

#### 1. Node.js y npm

Angular requiere Node.js LTS (v18 o superior en 2026).

```bash
# Verificar versión de Node. js
node --version
# Salida esperada: v20.11.0 o superior

# Verificar versión de npm
npm --version
# Salida esperada: v10.2.0 o superior
```

**Si no tienes Node.js:**
- Descarga desde:  https://nodejs.org/
- Elige la versión **LTS** (recomendada)
- Instala siguiendo el asistente

**Tip**: Usa **nvm** (Node Version Manager):
```bash
# Instalar nvm (Linux/Mac)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Instalar Node LTS
nvm install --lts
nvm use --lts
```

#### 2. Editor de Código:  Visual Studio Code

**Descarga**:  https://code.visualstudio.com/

**Extensiones recomendadas:**
```
1. Angular Language Service (Angular. ng-template)
2. Angular Snippets (johnpapa.Angular2)
3. ESLint (dbaeumer.vscode-eslint)
4. Prettier (esbenp.prettier-vscode)
5. GitLens (eamodio. gitlens)
6. Auto Rename Tag (formulahendry.auto-rename-tag)
7. Material Icon Theme (PKief.material-icon-theme)
```

**Configuración de VS Code** (`settings.json`):
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter":  "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll. eslint": true
  },
  "files.eol": "\n",
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

### Instalación de Angular CLI

Angular CLI es la herramienta oficial para crear y gestionar proyectos. 

```bash
# Instalar Angular CLI globalmente
npm install -g @angular/cli

# Verificar instalación
ng version

# Salida esperada: 
# Angular CLI:  19.0.0
# Node: 20.11.0
# Package Manager: npm 10.2.4
```

**Si necesitas una versión específica:**
```bash
# Instalar versión específica
npm install -g @angular/cli@19.0.0

# Ver versiones disponibles
npm view @angular/cli versions --json
```

**Actualizar Angular CLI:**
```bash
# Desinstalar versión anterior
npm uninstall -g @angular/cli

# Limpiar caché
npm cache clean --force

# Instalar última versión
npm install -g @angular/cli@latest
```

---

## 📊 Versiones de Angular y LTS

### Ciclo de Liberación

Angular sigue un ciclo predecible: 

```
┌─────────────┬─────────────┬─────────────┬─────────────┐
│   v19.0.0   │   v19.1.0   │   v19.2.0   │   v20.0.0   │
│  (Major)    │  (Minor)    │  (Minor)    │  (Major)    │
└─────────────┴─────────────┴─────────────┴─────────────┘
     │              │              │              │
  Mayo 2025    Julio 2025    Sept 2025     Nov 2025
```

**Calendario:**
- **Major version** (vX.0.0): Cada 6 meses (Mayo y Noviembre)
- **Minor version** (vX.Y.0): Cada 1-2 meses
- **Patch version** (vX.Y.Z): Semanalmente si es necesario

### Versiones Importantes

| Versión | Fecha | Características Clave |
|---------|-------|----------------------|
| **Angular 2** | Sept 2016 | Reescritura completa en TypeScript |
| **Angular 4** | Marzo 2017 | HttpClient, Animations separadas |
| **Angular 9** | Feb 2020 | **Ivy por defecto** |
| **Angular 12** | Mayo 2021 | Ivy everywhere |
| **Angular 14** | Junio 2022 | **Standalone Components** |
| **Angular 16** | Mayo 2023 | **Signals** |
| **Angular 17** | Nov 2023 | Control flow (@if, @for) |
| **Angular 19** | Nov 2025 | **Versión actual** |

### Política LTS

```
Angular v18 (Mayo 2024)
│
├─ Active:  Mayo 2024 - Nov 2024 (6 meses)
│  └─ Nuevas features, bug fixes
│
└─ LTS: Nov 2024 - Mayo 2025 (12 meses)
   └─ Solo critical fixes y seguridad
```

**Recomendaciones:**
- **Proyectos nuevos**: Última versión estable (v19)
- **Producción**: Mantente en LTS, actualiza cada 6-12 meses

---

## 🚀 Angular CLI - Comandos Esenciales

### Crear un Nuevo Proyecto

```bash
# Sintaxis básica
ng new nombre-proyecto

# Con opciones interactivas
ng new mi-app-angular
# ?  Would you like to add Angular routing? (y/N) → y
# ? Which stylesheet format?  → CSS

# Con opciones inline
ng new mi-app-angular \
  --routing=true \
  --style=scss \
  --strict=true \
  --standalone=true
```

**Opciones importantes:**
- `--routing`: Incluir Angular Router
- `--style`: CSS, SCSS, SASS, LESS
- `--strict`: Modo estricto de TypeScript
- `--standalone`: Componentes standalone (sin NgModules)
- `--skip-git`: No inicializar Git
- `--skip-tests`: Sin archivos de tests

**Estructura generada:**
```
mi-app-angular/
├── . angular/                   # Caché de Angular CLI
├── node_modules/              # Dependencias
├── src/                       # Código fuente
│   ├── app/
│   │   ├── app.component.ts
│   │   ├── app.component. html
│   │   ├── app.component.css
│   │   ├── app.config.ts
│   │   └── app.routes.ts
│   ├── assets/
│   ├── index.html
│   ├── main.ts
│   └── styles.css
├── angular.json               # Configuración de CLI
├── package.json
├── tsconfig.json
└── README.md
```

### Servir la Aplicación

```bash
# Servidor de desarrollo básico
ng serve

# Con opciones
ng serve --open              # Abre navegador automáticamente
ng serve --port 4300         # Cambiar puerto
ng serve --host 0.0.0.0      # Accesible desde red local

# Aliases
ng s                         # Alias de ng serve
ng s -o                      # Serve + open
ng s --port 3000 -o

# Accesible en:  http://localhost:4200
```

**Características del servidor:**
- ✅ Hot Module Replacement
- ✅ Live Reload
- ✅ Source Maps para debugging
- ✅ Proxy configuration para CORS

### Generar Componentes y Más

```bash
# ========== COMPONENTES ==========
ng generate component nombre
ng g c nombre                      # Alias

# Ejemplos
ng g c header                      # src/app/header/
ng g c pages/home                  # src/app/pages/home/
ng g c shared/button --standalone

# Opciones
ng g c hero --skip-tests           # Sin . spec.ts
ng g c hero --inline-template      # Template inline
ng g c hero --inline-style         # Estilos inline
ng g c hero --flat                 # Sin carpeta propia

# ========== SERVICIOS ==========
ng generate service nombre
ng g s nombre

ng g s services/data
ng g s auth

# ========== MÓDULOS ==========
ng generate module nombre
ng g m nombre

ng g m feature --routing           # Con routing
ng g m shared

# ========== DIRECTIVAS ==========
ng generate directive nombre
ng g d nombre

ng g d directives/highlight

# ========== PIPES ==========
ng generate pipe nombre
ng g p nombre

ng g p pipes/capitalize

# ========== GUARDS ==========
ng generate guard nombre
ng g g nombre

ng g g guards/auth

# ========== INTERCEPTORS ==========
ng generate interceptor nombre

ng g interceptor interceptors/auth

# ========== INTERFACES ==========
ng generate interface nombre
ng g i nombre

ng g i models/user

# ========== ENUMS ==========
ng generate enum nombre
ng g e nombre

ng g e enums/status
```

### Build para Producción

```bash
# Build básico
ng build

# Build optimizado
ng build --configuration production

# Salida en:  dist/nombre-proyecto/

# Optimizaciones incluidas: 
# - Minificación
# - Tree shaking
# - AOT compilation
# - Bundle optimization
```

### Tests

```bash
# Unit tests
ng test

# Con cobertura
ng test --code-coverage

# Modo headless (CI/CD)
ng test --watch=false --browsers=ChromeHeadless

# E2E tests
ng e2e
```

### Otros Comandos Útiles

```bash
# Ayuda
ng generate --help
ng g c --help

# Actualizar Angular
ng update
ng update @angular/cli @angular/core

# Analizar bundle size
ng build --stats-json
npx webpack-bundle-analyzer dist/mi-app/stats.json

# Agregar librerías
ng add @angular/material
ng add @ngrx/store
ng add @angular/pwa

# Información del proyecto
ng version
ng config
```

---

## 📁 Estructura de Carpetas

### Estructura Completa Explicada

```
mi-app-angular/
│
├── . angular/                   # Caché de Angular CLI
├── node_modules/              # Dependencias (NO subir a Git)
│
├── src/                       # CÓDIGO FUENTE
│   │
│   ├── app/                   # APLICACIÓN ANGULAR
│   │   │
│   │   ├── core/              # Servicios singleton, guards
│   │   │   ├── guards/
│   │   │   ├── interceptors/
│   │   │   └── services/
│   │   │
│   │   ├── shared/            # Componentes reutilizables
│   │   │   ├── components/
│   │   │   ├── directives/
│   │   │   └── pipes/
│   │   │
│   │   ├── features/          # Módulos de funcionalidades
│   │   │   ├── home/
│   │   │   ├── about/
│   │   │   └── dashboard/
│   │   │
│   │   ├── models/            # Interfaces, tipos, enums
│   │   │   ├── user. interface.ts
│   │   │   └── product.interface.ts
│   │   │
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component. css
│   │   ├── app.config.ts
│   │   └── app. routes.ts
│   │
│   ├── assets/                # Archivos estáticos
│   │   ├── images/
│   │   ├── icons/
│   │   └── data/
│   │
│   ├── environments/          # Variables de entorno
│   │   ├── environment.ts
│   │   └── environment.prod.ts
│   │
│   ├── index.html             # HTML principal
│   ├── main.ts                # Bootstrap de la app
│   ├── styles.css             # Estilos globales
│   └── favicon.ico
│
├── . editorconfig
├── .gitignore
├── angular.json               # Configuración de Angular CLI
├── package.json               # Dependencias y scripts
├── tsconfig.json              # Configuración de TypeScript
└── README.md
```

### Archivos Clave

#### 1. `src/main.ts` - Bootstrap

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));
```

#### 2. `src/app/app.component.ts` - Componente Raíz

```typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {
  title = 'mi-app-angular';
}
```

#### 3. `src/app/app.config.ts` - Configuración

```typescript
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';
import { provideHttpClient } from '@angular/common/http';

export const appConfig:  ApplicationConfig = {
  providers:  [
    provideRouter(routes),
    provideHttpClient()
  ]
};
```

#### 4. `src/app/app.routes.ts` - Rutas

```typescript
import { Routes } from '@angular/router';
import { HomeComponent } from './pages/home/home.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: '**', redirectTo: '' }
];
```

#### 5. `angular.json` - Configuración de CLI

```json
{
  "projects": {
    "mi-app-angular": {
      "architect": {
        "build": {
          "options": {
            "outputPath":  "dist/mi-app-angular",
            "index": "src/index.html",
            "main": "src/main. ts",
            "tsConfig": "tsconfig.app. json",
            "assets": ["src/favicon.ico", "src/assets"],
            "styles": ["src/styles.css"],
            "scripts": []
          }
        },
        "serve": {
          "options": {
            "port": 4200
          }
        }
      }
    }
  }
}
```

#### 6. `package.json` - Dependencias

```json
{
  "name": "mi-app-angular",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build":  "ng build",
    "test": "ng test"
  },
  "dependencies": {
    "@angular/animations": "^19.0.0",
    "@angular/common":  "^19.0.0",
    "@angular/compiler": "^19.0.0",
    "@angular/core":  "^19.0.0",
    "@angular/forms": "^19.0.0",
    "@angular/platform-browser":  "^19.0.0",
    "@angular/router": "^19.0.0",
    "rxjs": "~7.8.0",
    "tslib": "^2.3.0",
    "zone.js":  "~0.14.0"
  },
  "devDependencies": {
    "@angular/cli": "^19.0.0",
    "@angular/compiler-cli": "^19.0.0",
    "typescript": "~5.4.0"
  }
}
```

---

## 🏗️ Arquitectura de Angular

### Conceptos Fundamentales

```
┌─────────────────────────────────────┐
│       APLICACIÓN ANGULAR            │
│                                     │
│  ┌──────────┐  ┌──────────┐       │
│  │Component │  │Component │       │
│  │  (Vista) │  │  (Vista) │       │
│  └────┬─────┘  └────┬─────┘       │
│       │             │              │
│       └─────────────┘              │
│              │                     │
│       ┌─────���▼──────┐             │
│       │   Services  │             │
│       │ (Lógica)    │             │
│       └──────┬──────┘             │
│              │                     │
│       ┌──────▼──────┐             │
│       │  HTTP/API   │             │
│       └─────────────┘             │
└─────────────────────────────────────┘
```

### 1. Componentes (Components)

**¿Qué son?**
- Bloques de construcción de la UI
- Tienen:  template (HTML), estilos (CSS), lógica (TypeScript)

**Anatomía de un componente:**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero',
  standalone: true,
  template: `
    <h2>{{hero.name}}</h2>
    <p>Power: {{hero.power}}</p>
  `,
  styles: [`
    h2 { color: blue; }
  `]
})
export class HeroComponent {
  hero = {
    name:  'Superman',
    power: 'Flight'
  };
}
```

**Con archivos separados:**

```typescript
@Component({
  selector: 'app-hero',
  standalone:  true,
  templateUrl: './hero.component.html',
  styleUrls: ['./hero.component. css']
})
export class HeroComponent {
  // Lógica
}
```

### 2. Templates (HTML)

**Data Binding:**

```html
<!-- Interpolation:  {{ }} -->
<h1>{{ title }}</h1>

<!-- Property Binding: [property] -->
<img [src]="imageUrl">
<button [disabled]="isDisabled">Click</button>

<!-- Event Binding:  (event) -->
<button (click)="onClick()">Click me</button>

<!-- Two-way Binding: [(ngModel)] -->
<input [(ngModel)]="name">
<p>Hola, {{ name }}!</p>
```

**Directivas estructurales:**

```html
<!-- *ngIf -->
<div *ngIf="isVisible">Visible</div>

<!-- *ngFor -->
<ul>
  <li *ngFor="let hero of heroes; let i = index">
    {{ i + 1 }}. {{ hero.name }}
  </li>
</ul>

<!-- *ngSwitch -->
<div [ngSwitch]="color">
  <p *ngSwitchCase="'red'">Rojo</p>
  <p *ngSwitchDefault>Otro</p>
</div>

<!-- Angular 17+: @if, @for -->
@if (isVisible) {
  <p>Visible</p>
} @else {
  <p>No visible</p>
}

@for (hero of heroes; track hero.id) {
  <li>{{ hero.name }}</li>
}
```

### 3. Servicios (Services)

**¿Qué son? **
- Clases reutilizables para lógica de negocio
- Compartir datos entre componentes
- Comunicación con APIs

**Ejemplo:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class HeroService {
  private apiUrl = 'https://api.example.com/heroes';

  constructor(private http: HttpClient) {}

  getHeroes(): Observable<Hero[]> {
    return this. http.get<Hero[]>(this.apiUrl);
  }

  getHero(id: number): Observable<Hero> {
    return this.http.get<Hero>(`${this.apiUrl}/${id}`);
  }
}
```

**Inyección en componente:**

```typescript
import { Component, OnInit } from '@angular/core';
import { HeroService } from './hero.service';

@Component({
  selector: 'app-hero-list',
  template: `
    <ul>
      <li *ngFor="let hero of heroes">{{ hero.name }}</li>
    </ul>
  `
})
export class HeroListComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) {}

  ngOnInit(): void {
    this.heroService.getHeroes().subscribe(heroes => {
      this.heroes = heroes;
    });
  }
}
```

### 4. Dependency Injection (DI)

**¿Qué es?**
- Patrón donde las dependencias se inyectan automáticamente
- Angular gestiona la creación de servicios

**Ejemplo:**

```typescript
// ❌ MAL:  Creación manual
export class HeroComponent {
  private heroService = new HeroService();
}

// ✅ BIEN: Inyección
export class HeroComponent {
  constructor(private heroService: HeroService) {}
}
```

### 5. Routing

**Configuración:**

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'heroes', component: HeroesComponent },
  { path: 'heroes/:id', component: HeroDetailComponent },
  { path: '**', redirectTo: '' }
];
```

**En el template:**

```html
<!-- app.component.html -->
<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/about">About</a>
  <a routerLink="/heroes">Heroes</a>
</nav>

<router-outlet></router-outlet>
```

**Navegación programática:**

```typescript
import { Router } from '@angular/router';

constructor(private router: Router) {}

goToHero(id: number): void {
  this.router.navigate(['/heroes', id]);
}
```

---

## ✅ Buenas Prácticas

### 1. Estructura de Archivos

```
✅ BIEN: Organización por features
src/app/
├── core/
├── shared/
├── features/
│   ├── heroes/
│   └── products/

❌ MAL: Todo en la raíz
src/app/
├── hero-list. component.ts
├── product-list.component.ts
└── ...  (20+ archivos)
```

### 2. Nomenclatura

```typescript
// ✅ BIEN: PascalCase para clases
export class HeroDetailComponent {}
export class HeroService {}

// ✅ BIEN: camelCase para variables
const heroName = 'Superman';

// ✅ BIEN: kebab-case para archivos
hero-detail.component.ts
hero. service.ts
```

### 3. Componentes Pequeños

```typescript
// ✅ BIEN: Componentes enfocados
@Component({
  selector: 'app-hero-card',
  template: `
    <div class="card">
      <h3>{{ hero.name }}</h3>
    </div>
  `
})
export class HeroCardComponent {
  @Input() hero!: Hero;
}

// ❌ MAL: Componente gigante
@Component({
  template: `... 500 líneas ...`
})
export class DashboardComponent {
  // 1000 líneas de lógica
}
```

### 4. Servicios con Responsabilidad Única

```typescript
// ✅ BIEN
@Injectable({ providedIn: 'root' })
export class HeroService {
  getHeroes() {}
  getHero(id) {}
}

// ❌ MAL:  Servicio "God Object"
@Injectable({ providedIn: 'root' })
export class AppService {
  getHeroes() {}
  login() {}
  sendEmail() {}
  processPayment() {}
}
```

### 5. Observables y Async Pipe

```typescript
// ✅ BIEN: Usar async pipe
@Component({
  template: `
    <div *ngFor="let hero of heroes$ | async">
      {{ hero. name }}
    </div>
  `
})
export class HeroListComponent {
  heroes$ = this.heroService.getHeroes();
  
  constructor(private heroService:  HeroService) {}
}

// ❌ MAL: Memory leaks
export class HeroDetailComponent {
  ngOnInit() {
    this.heroService.getHero(1).subscribe(...);
    // Nunca se desuscribe
  }
}
```

### 6. TypeScript Tipos Explícitos

```typescript
// ✅ BIEN
getHero(id: number): Observable<Hero> {
  return this.http.get<Hero>(`/api/heroes/${id}`);
}

// ❌ MAL
getHero(id:  any): any {
  return this.http.get(`/api/heroes/${id}`);
}
```

---

## 🐛 Debugging en Angular

### 1. Chrome DevTools

**Angular DevTools extension**
1. F12 → pestaña "Angular"
2. Ver componentes, propiedades, change detection

### 2. Console Logs

```typescript
export class HeroComponent implements OnInit {
  ngOnInit() {
    console.log('HeroComponent initialized');
    console.table(this.heroes);
  }
}
```

### 3. Breakpoints en VS Code

**launch.json:**
```json
{
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Angular",
      "url": "http://localhost:4200",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

---

## 🎯 Tu Primera Aplicación:  Lista de Héroes

### Paso 1: Crear el Proyecto

```bash
ng new heroes-app --routing --style=css --standalone
cd heroes-app
ng serve -o
```

### Paso 2: Crear la Interface

```bash
ng g interface models/hero
```

```typescript
// src/app/models/hero.ts
export interface Hero {
  id: number;
  name: string;
  power: string;
}
```

### Paso 3: Crear el Servicio

```bash
ng g s services/hero
```

```typescript
// src/app/services/hero.service.ts
import { Injectable } from '@angular/core';
import { Hero } from '../models/hero';

@Injectable({
  providedIn: 'root'
})
export class HeroService {
  private heroes: Hero[] = [
    { id: 1, name: 'Superman', power: 'Flight' },
    { id: 2, name: 'Wonder Woman', power: 'Strength' },
    { id: 3, name: 'Flash', power: 'Speed' }
  ];

  getHeroes(): Hero[] {
    return this.heroes;
  }

  getHero(id: number): Hero | undefined {
    return this.heroes.find(h => h.id === id);
  }
}
```

### Paso 4: Crear el Componente

```bash
ng g c components/hero-list
```

```typescript
// src/app/components/hero-list/hero-list.component.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Hero } from '../../models/hero';
import { HeroService } from '../../services/hero.service';

@Component({
  selector: 'app-hero-list',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './hero-list.component.html',
  styleUrl: './hero-list.component. css'
})
export class HeroListComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) {}

  ngOnInit(): void {
    this.heroes = this.heroService.getHeroes();
  }
}
```

```html
<!-- src/app/components/hero-list/hero-list.component.html -->
<h2>Heroes</h2>
<ul class="heroes-list">
  <li *ngFor="let hero of heroes" class="hero-item">
    <span class="hero-id">{{ hero.id }}</span>
    <span class="hero-name">{{ hero.name }}</span>
    <span class="hero-power">{{ hero.power }}</span>
  </li>
</ul>
```

```css
/* src/app/components/hero-list/hero-list.component.css */
.heroes-list {
  list-style: none;
  padding: 0;
}

.hero-item {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  margin-bottom: 0.5rem;
  background:  #f0f0f0;
  border-radius: 4px;
}

.hero-name {
  flex: 1;
  font-size: 1.2rem;
}

.hero-power {
  color: #007bff;
  font-style: italic;
}
```

### Paso 5: Integrar en App Component

```typescript
// src/app/app.component.ts
import { Component } from '@angular/core';
import { HeroListComponent } from './components/hero-list/hero-list.component';

@Component({
  selector:  'app-root',
  standalone: true,
  imports:  [HeroListComponent],
  template: `
    <div class="container">
      <h1>{{ title }}</h1>
      <app-hero-list></app-hero-list>
    </div>
  `,
  styles: [`
    .container {
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem;
    }
    h1 {
      color: #333;
      text-align: center;
    }
  `]
})
export class AppComponent {
  title = 'Heroes App';
}
```

### Paso 6: Ejecutar

```bash
ng serve -o
```

¡Listo! Tienes tu primera app Angular funcionando. 

---

## 📚 Recursos de Aprendizaje

### Documentación Oficial
- **Angular. dev**: https://angular.dev/
- **Angular. io** (legacy): https://angular.io/
- **Angular CLI**: https://angular.io/cli
- **Style Guide**: https://angular.dev/style-guide

### Cursos
- **Angular University**: https://angular-university.io/
- **Ultimate Angular**:  https://ultimatecourses.com/
- **Udemy - Angular Complete Guide** (Maximilian Schwarzmüller)

### Blogs
- **Angular Blog**: https://blog.angular.io/
- **Indepth. dev**: https://indepth.dev/angular
- **This is Angular**: https://www.thisisangular.com/

### Librerías
- **Angular Material**: https://material.angular.io/
- **PrimeNG**: https://primeng.org/
- **NgRx**: https://ngrx.io/

### Comunidades
- **Angular Discord**: https://discord.gg/angular
- **Reddit r/Angular2**: https://www.reddit.com/r/Angular2/
- **Stack Overflow**: [angular] tag

---

## ✅ Checklist de Dominio

### Básico
- [ ] Instalar Node.js, npm y Angular CLI
- [ ] Crear proyecto con `ng new`
- [ ] Servir aplicación con `ng serve`
- [ ] Entender estructura de carpetas
- [ ] Crear componentes
- [ ] Data binding básico
- [ ] Directivas estructurales

### Intermedio
- [ ] Crear servicios
- [ ] Dependency Injection
- [ ] Routing básico
- [ ] Formularios template-driven
- [ ] HTTP Client
- [ ] Observables básicos
- [ ] Lifecycle hooks

### Avanzado
- [ ] Formularios reactivos
- [ ] Guards
- [ ] Interceptors
- [ ] Lazy loading
- [ ] Signals
- [ ] Testing

---

## 🎯 Ejercicios Prácticos

### Ejercicio 1: CRUD de Tareas

**Objetivo**: App de gestión de tareas. 

**Requisitos**:
1. Listar tareas
2. Agregar nueva tarea
3. Marcar como completada
4. Eliminar tarea
5. Filtrar (todas/completadas/pendientes)

### Ejercicio 2: Rick and Morty API

**Objetivo**: Mostrar personajes de la API. 

**API**: https://rickandmortyapi.com/api/character

**Requisitos**:
1. Listar personajes
2. Paginación
3. Búsqueda por nombre
4. Ver detalle

### Ejercicio 3: Autenticación

**Objetivo**: Login/logout con guards.

**Requisitos**:
1. Formulario de login
2. Guard para rutas privadas
3. Servicio de autenticación
4. Token en localStorage
5. Logout

---

**¡Felicidades!** Has completado Angular Intro. 

→ **Siguiente:  [Módulo 1C:  Diseño Atómico](./MODULO-1C-DISENO-ATOMICO.md)**

---

**Última actualización**:  Enero 2026  
**Versión Angular**: 19.x  
**Autor**: Prof. David Luna

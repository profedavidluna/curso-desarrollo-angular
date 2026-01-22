# 📘 Módulo 1C: Diseño Atómico - Arquitectura de Componentes

**Duración**: 3 horas  
**Nivel**: Intermedio  
**Prerequisito**:  MODULO-1B (Angular Intro)

---

## 🎯 Objetivos de Aprendizaje

Al completar este módulo, serás capaz de: 

- ✅ Entender qué es el Diseño Atómico y su origen
- ✅ Identificar los 5 niveles de la metodología atómica
- ✅ Aplicar Diseño Atómico en proyectos Angular
- ✅ Estructurar componentes de forma escalable y mantenible
- ✅ Crear sistemas de diseño consistentes
- ✅ Reconocer anti-patrones y malas prácticas
- ✅ Implementar una arquitectura de componentes profesional
- ✅ Documentar y comunicar decisiones de diseño

---

## 📖 ¿Qué es el Diseño Atómico? 

### Definición

**Atomic Design** (Diseño Atómico) es una **metodología** creada por **Brad Frost** en 2013 para crear sistemas de diseño de interfaces de usuario de manera **jerárquica** y **modular**.

> "We're not designing pages, we're designing systems of components."  
> — Brad Frost

### Inspiración:  Química

El Diseño Atómico se inspira en la química: 

```
QUÍMICA                      →    DISEÑO ATÓMICO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Átomos (H, O, C)            →    Átomos (Button, Input)
    ↓                                   ↓
Moléculas (H₂O)             →    Moléculas (SearchBar)
    ↓                                   ↓
Organismos (Célula)         →    Organismos (Header)
    ↓                                   ↓
Templates (Estructura)      →    Templates (PageLayout)
    ↓                                   ↓
Páginas (Producto final)    →    Páginas (HomePage)
```

### ¿Por Qué Usar Diseño Atómico en Angular?

**✅ Ventajas:**

1. **Reutilización máxima**: Componentes pequeños reutilizables
2. **Mantenibilidad**: Cambiar un átomo afecta todos los usos
3. **Consistencia visual**: Mismo look & feel
4. **Escalabilidad**:  Fácil agregar funcionalidades
5. **Testing simplificado**: Componentes pequeños = tests fáciles
6. **Colaboración**: Diseñadores y devs hablan el mismo idioma
7. **Documentación natural**: La estructura ES la documentación
8. **Onboarding rápido**: Nuevos devs entienden rápido

**⚠️ Desafíos:**

- Requiere planificación inicial
- Puede parecer over-engineering en proyectos pequeños
- Curva de aprendizaje para el equipo
- Tentación de sobre-abstraer

---

## 🧪 Los 5 Niveles del Diseño Atómico

```
┌─────────────────────────────────────────────────┐
│                   5.  PÁGINAS                    │
│          (Instancias específicas)               │
│  HomePage, AboutPage, DashboardPage             │
└───────────────────┬─────────────────────────────┘
                    │
┌───────────────────▼─────────────────────────────┐
│                 4. TEMPLATES                    │
│            (Layouts, estructura)                │
│   PageLayout, AuthLayout, DashboardLayout       │
└───────────────────┬─────────────────────────────┘
                    │
┌───────────────────▼─────────────────────────────┐
│                3. ORGANISMOS                    │
│        (Secciones complejas de UI)              │
│   Header, Footer, Sidebar, ProductCard          │
└───────────────────┬─────────────────────────────┘
                    │
┌───────────────────▼─────────────────────────────┐
│                2. MOLÉCULAS                     │
│        (Grupos de átomos con función)           │
│   SearchBar, FormField, CardHeader              │
└───────────────────┬─────────────────────────────┘
                    │
┌───────────────────▼─────────────────────────────┐
│                  1. ÁTOMOS                      │
│       (Elementos básicos de UI)                 │
│   Button, Input, Label, Icon, Avatar            │
└─────────────────────────────────────────────────┘
```

### Nivel 1: Átomos ⚛️

**Definición**: Los componentes más básicos e indivisibles de la interfaz.  No pueden descomponerse más sin perder su significado.

**Características:**
- ✅ **Autónomos**: Funcionan independientemente
- ✅ **Configurables**: Aceptan inputs para personalización
- ✅ **Sin lógica de negocio**: Solo presentación
- ✅ **Altamente reutilizables**:  Usados en toda la app
- ✅ **Testeables**: Fáciles de testear en aislamiento

**Ejemplos de Átomos:**

| Átomo | Descripción | Props típicos |
|-------|-------------|---------------|
| **Button** | Botón clickeable | `text`, `type`, `disabled`, `size` |
| **Input** | Campo de texto | `value`, `placeholder`, `type` |
| **Label** | Etiqueta de texto | `text`, `for`, `required` |
| **Icon** | Ícono visual | `name`, `size`, `color` |
| **Avatar** | Imagen de usuario | `src`, `alt`, `size` |
| **Badge** | Insignia o etiqueta | `text`, `color`, `variant` |
| **Spinner** | Indicador de carga | `size`, `color` |
| **Checkbox** | Casilla de verificación | `checked`, `label` |

**Ejemplo en Angular - Átomo Button:**

```typescript
// button.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';

type ButtonVariant = 'primary' | 'secondary' | 'danger' | 'ghost';
type ButtonSize = 'sm' | 'md' | 'lg';

@Component({
  selector: 'atom-button',
  standalone: true,
  imports: [CommonModule],
  template: `
    <button
      [class]="getButtonClasses()"
      [disabled]="disabled || loading"
      [type]="type"
      (click)="handleClick($event)"
    >
      @if (loading) {
        <span class="spinner"></span>
      }
      @if (iconLeft) {
        <span class="icon-left">{{ iconLeft }}</span>
      }
      <span class="button-text">{{ text }}</span>
      @if (iconRight) {
        <span class="icon-right">{{ iconRight }}</span>
      }
    </button>
  `,
  styleUrls: ['./button.component.scss']
})
export class ButtonComponent {
  @Input() text: string = '';
  @Input() variant: ButtonVariant = 'primary';
  @Input() size: ButtonSize = 'md';
  @Input() disabled: boolean = false;
  @Input() loading: boolean = false;
  @Input() type: 'button' | 'submit' | 'reset' = 'button';
  @Input() iconLeft?: string;
  @Input() iconRight?: string;
  @Input() fullWidth: boolean = false;
  
  @Output() clicked = new EventEmitter<Event>();

  getButtonClasses(): string {
    return [
      'atom-button',
      `atom-button--${this.variant}`,
      `atom-button--${this.size}`,
      this.fullWidth ? 'atom-button--full' : '',
      this.loading ? 'atom-button--loading' : ''
    ].filter(Boolean).join(' ');
  }

  handleClick(event: Event): void {
    if (! this.disabled && !this.loading) {
      this.clicked. emit(event);
    }
  }
}
```

```scss
// button.component.scss
.atom-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  border: none;
  border-radius: 6px;
  font-weight: 500;
  cursor:  pointer;
  transition: all 0.2s ease;
  font-family: inherit;
  
  &:hover: not(:disabled) {
    transform: translateY(-1px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  }
  
  &:disabled {
    cursor: not-allowed;
    opacity: 0.5;
  }
  
  // Variants
  &--primary {
    background:  #007bff;
    color: white;
    
    &:hover:not(:disabled) {
      background: #0056b3;
    }
  }
  
  &--secondary {
    background: #6c757d;
    color: white;
  }
  
  &--danger {
    background: #dc3545;
    color: white;
  }
  
  &--ghost {
    background: transparent;
    color: #007bff;
    border: 1px solid #007bff;
  }
  
  // Sizes
  &--sm {
    padding: 0.375rem 0.75rem;
    font-size: 0.875rem;
  }
  
  &--md {
    padding: 0.5rem 1rem;
    font-size: 1rem;
  }
  
  &--lg {
    padding: 0.75rem 1.5rem;
    font-size: 1.125rem;
  }
  
  &--full {
    width: 100%;
  }
}
```

**Uso del átomo:**

```html
<atom-button 
  text="Guardar"
  variant="primary"
  (clicked)="onSave()"
></atom-button>

<atom-button 
  text="Eliminar"
  variant="danger"
  iconLeft="🗑️"
  (clicked)="onDelete()"
></atom-button>
```

---

### Nivel 2: Moléculas 🧬

**Definición**: Grupos de **átomos** que trabajan juntos como una unidad con una función específica.

**Características:**
- ✅ **Composición de átomos**: Usan múltiples átomos
- ✅ **Función específica**: Realizan una tarea concreta
- ✅ **Reutilizables**: Usados en diferentes contextos
- ✅ **Lógica simple**: Coordinan átomos

**Ejemplos de Moléculas:**

| Molécula | Átomos que contiene | Función |
|----------|-------------------|---------|
| **SearchBar** | Input + Button | Búsqueda |
| **FormField** | Label + Input + ErrorMessage | Campo de formulario |
| **CardHeader** | Avatar + Title + Subtitle | Encabezado de tarjeta |
| **Pagination** | Button + Text | Navegación por páginas |
| **Alert** | Icon + Text + Button | Mensaje de alerta |

**Ejemplo en Angular - Molécula SearchBar:**

```typescript
// search-bar.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { ButtonComponent } from '../../atoms/button/button.component';
import { InputComponent } from '../../atoms/input/input.component';

@Component({
  selector: 'molecule-search-bar',
  standalone: true,
  imports: [FormsModule, ButtonComponent, InputComponent],
  template: `
    <div class="search-bar">
      <atom-input
        [(value)]="searchValue"
        [placeholder]="placeholder"
        type="search"
        [disabled]="disabled"
        (keyup.enter)="handleSearch()"
      ></atom-input>
      
      <atom-button
        text="Buscar"
        iconLeft="🔍"
        variant="primary"
        [disabled]="disabled || ! searchValue"
        (clicked)="handleSearch()"
      ></atom-button>
      
      @if (searchValue) {
        <atom-button
          text="Limpiar"
          variant="ghost"
          size="sm"
          (clicked)="handleClear()"
        ></atom-button>
      }
    </div>
  `,
  styleUrls: ['./search-bar.component.scss']
})
export class SearchBarComponent {
  @Input() placeholder: string = 'Buscar...';
  @Input() disabled: boolean = false;
  @Input() initialValue: string = '';
  
  @Output() search = new EventEmitter<string>();
  @Output() clear = new EventEmitter<void>();
  
  searchValue: string = '';

  ngOnInit() {
    this.searchValue = this.initialValue;
  }

  handleSearch(): void {
    if (this.searchValue.trim()) {
      this.search.emit(this.searchValue);
    }
  }

  handleClear(): void {
    this.searchValue = '';
    this.clear.emit();
  }
}
```

```scss
// search-bar.component.scss
. search-bar {
  display:  flex;
  gap: 0.5rem;
  align-items: center;
  
  atom-input {
    flex: 1;
  }
}
```

**Uso de la molécula:**

```html
<molecule-search-bar
  placeholder="Buscar héroes..."
  (search)="onSearch($event)"
  (clear)="onClear()"
></molecule-search-bar>
```

---

### Nivel 3: Organismos 🦠

**Definición**:  Componentes complejos formados por **moléculas** y **átomos** que constituyen secciones distintas de la interfaz.

**Características:**
- ✅ **Secciones completas**: Header, Footer, Sidebar
- ✅ **Combinan moléculas y átomos**: Uso intensivo de componentes
- ✅ **Lógica de negocio**: Pueden tener lógica más compleja
- ✅ **Contextuales**: Específicos a ciertos contextos
- ✅ **Independientes**:  Funcionan de manera autónoma

**Ejemplos de Organismos:**

| Organismo | Componentes que contiene | Función |
|-----------|------------------------|---------|
| **Header** | Logo + Navigation + SearchBar + UserMenu | Navegación principal |
| **Footer** | LinkList + SocialIcons + Newsletter | Pie de página |
| **ProductCard** | Image + Title + Price + Rating + Button | Tarjeta de producto |
| **CommentSection** | Avatar + CommentForm + CommentList | Sección de comentarios |
| **DataTable** | TableHeader + TableRow + Pagination | Tabla de datos |

**Ejemplo en Angular - Organismo Header:**

```typescript
// header.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { RouterLink } from '@angular/router';
import { CommonModule } from '@angular/common';
import { ButtonComponent } from '../../atoms/button/button.component';
import { AvatarComponent } from '../../atoms/avatar/avatar.component';
import { SearchBarComponent } from '../../molecules/search-bar/search-bar.component';

export interface NavItem {
  label: string;
  route: string;
  icon?: string;
}

export interface User {
  name: string;
  avatar:  string;
  email: string;
}

@Component({
  selector: 'organism-header',
  standalone: true,
  imports: [
    CommonModule,
    RouterLink,
    ButtonComponent,
    AvatarComponent,
    SearchBarComponent
  ],
  template: `
    <header class="header">
      <div class="header__container">
        <!-- Logo -->
        <div class="header__logo">
          <a [routerLink]="['/']">
            <img [src]="logo" [alt]="appName" />
            <span>{{ appName }}</span>
          </a>
        </div>

        <!-- Navigation -->
        <nav class="header__nav">
          <a 
            *ngFor="let item of navItems" 
            [routerLink]="item.route"
            routerLinkActive="active"
          >
            {{ item.label }}
          </a>
        </nav>

        <!-- Search -->
        @if (showSearch) {
          <div class="header__search">
            <molecule-search-bar
              [placeholder]="searchPlaceholder"
              (search)="onSearch($event)"
            ></molecule-search-bar>
          </div>
        }

        <!-- User Section -->
        <div class="header__user">
          @if (user) {
            <div class="header__user-menu">
              <button 
                class="header__user-button"
                (click)="toggleUserMenu()"
              >
                <atom-avatar
                  [src]="user. avatar"
                  [alt]="user.name"
                  size="md"
                ></atom-avatar>
                <span>{{ user.name }}</span>
              </button>

              @if (showUserMenu) {
                <div class="header__dropdown">
                  <button (click)="onProfile()">👤 Perfil</button>
                  <button (click)="onSettings()">⚙️ Configuración</button>
                  <hr>
                  <button (click)="onLogout()">🚪 Cerrar sesión</button>
                </div>
              }
            </div>
          } @else {
            <atom-button
              text="Iniciar sesión"
              variant="primary"
              (clicked)="onLogin()"
            ></atom-button>
          }
        </div>
      </div>
    </header>
  `,
  styleUrls: ['./header.component.scss']
})
export class HeaderComponent {
  @Input() logo: string = '/assets/logo.svg';
  @Input() appName: string = 'My App';
  @Input() navItems: NavItem[] = [];
  @Input() showSearch: boolean = true;
  @Input() searchPlaceholder: string = 'Buscar...';
  @Input() user: User | null = null;
  
  @Output() search = new EventEmitter<string>();
  @Output() login = new EventEmitter<void>();
  @Output() logout = new EventEmitter<void>();
  @Output() profile = new EventEmitter<void>();
  @Output() settings = new EventEmitter<void>();

  showUserMenu: boolean = false;

  toggleUserMenu(): void {
    this.showUserMenu = ! this.showUserMenu;
  }

  onSearch(query: string): void {
    this.search.emit(query);
  }

  onLogin(): void {
    this.login.emit();
  }

  onLogout(): void {
    this.showUserMenu = false;
    this.logout.emit();
  }

  onProfile(): void {
    this.showUserMenu = false;
    this.profile.emit();
  }

  onSettings(): void {
    this.showUserMenu = false;
    this.settings.emit();
  }
}
```

```scss
// header.component.scss
.header {
  background: white;
  border-bottom: 1px solid #e0e0e0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  z-index:  100;

  &__container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 1rem 2rem;
    display: flex;
    align-items: center;
    gap: 2rem;
  }

  &__logo {
    a {
      display: flex;
      align-items: center;
      gap: 0.75rem;
      text-decoration: none;
      color: inherit;
      font-weight: 600;
    }

    img {
      height: 40px;
    }
  }

  &__nav {
    flex: 1;
    display:  flex;
    gap: 1. 5rem;

    a {
      text-decoration: none;
      color: #666;
      font-weight: 500;
      transition:  color 0.2s;

      &:hover {
        color: #007bff;
      }

      &.active {
        color: #007bff;
      }
    }
  }

  &__search {
    min-width: 300px;
  }

  &__user-menu {
    position: relative;
  }

  &__user-button {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
    border-radius: 8px;

    &:hover {
      background: #f5f5f5;
    }
  }

  &__dropdown {
    position: absolute;
    top: calc(100% + 0.5rem);
    right: 0;
    background: white;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    min-width: 200px;
    padding: 0.5rem;

    button {
      width: 100%;
      padding: 0.75rem 1rem;
      text-align: left;
      background: transparent;
      border: none;
      cursor: pointer;
      border-radius: 4px;

      &:hover {
        background: #f5f5f5;
      }
    }

    hr {
      margin: 0.5rem 0;
      border: none;
      border-top: 1px solid #e0e0e0;
    }
  }
}
```

---

### Nivel 4: Templates 📄

**Definición**: Estructuras de página que definen el **layout** y la distribución de organismos.  No contienen datos reales, solo la estructura.

**Características:**
- ✅ **Solo estructura**:  Definen posiciones, no contenido
- ✅ **Reutilizables**: Un template para múltiples páginas
- ✅ **Layout patterns**: Sidebar, Dashboard, Auth
- ✅ **Sin lógica de negocio**:  Puro layout
- ✅ **Content projection**: Usan `<ng-content>`

**Ejemplos de Templates:**

| Template | Estructura | Casos de uso |
|----------|-----------|--------------|
| **AuthLayout** | Centrado, card, sin header/footer | Login, Register |
| **DashboardLayout** | Header + Sidebar + Main + Footer | Admin, Dashboard |
| **LandingLayout** | Hero + Features + CTA + Footer | Landing pages |
| **FullPageLayout** | Solo contenido | Presentaciones |

**Ejemplo en Angular - Template DashboardLayout:**

```typescript
// dashboard-layout.component.ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'template-dashboard-layout',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div class="dashboard-layout">
      <!-- Header -->
      <div class="dashboard-layout__header">
        <ng-content select="[header]"></ng-content>
      </div>

      <div class="dashboard-layout__body">
        <!-- Sidebar -->
        @if (showSidebar) {
          <aside 
            class="dashboard-layout__sidebar"
            [class.dashboard-layout__sidebar--collapsed]="sidebarCollapsed"
          >
            <ng-content select="[sidebar]"></ng-content>
          </aside>
        }

        <!-- Main Content -->
        <main class="dashboard-layout__main">
          <div class="dashboard-layout__content">
            <ng-content></ng-content>
          </div>
        </main>
      </div>

      <!-- Footer -->
      @if (showFooter) {
        <div class="dashboard-layout__footer">
          <ng-content select="[footer]"></ng-content>
        </div>
      }
    </div>
  `,
  styleUrls: ['./dashboard-layout. component.scss']
})
export class DashboardLayoutComponent {
  @Input() showSidebar: boolean = true;
  @Input() showFooter: boolean = true;
  @Input() sidebarCollapsed: boolean = false;
}
```

```scss
// dashboard-layout.component.scss
.dashboard-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;

  &__header {
    position: sticky;
    top: 0;
    z-index:  100;
  }

  &__body {
    flex: 1;
    display: flex;
  }

  &__sidebar {
    width: 280px;
    background: #f8f9fa;
    border-right: 1px solid #e0e0e0;
    transition: width 0.3s ease;

    &--collapsed {
      width: 80px;
    }
  }

  &__main {
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  &__content {
    flex: 1;
    padding: 2rem;
    background: #f5f5f5;
  }

  &__footer {
    background: white;
    border-top: 1px solid #e0e0e0;
  }
}
```

**Uso del template:**

```html
<template-dashboard-layout [showSidebar]="true">
  <!-- Header -->
  <organism-header 
    header
    [user]="currentUser"
    (logout)="onLogout()"
  ></organism-header>

  <!-- Sidebar -->
  <organism-sidebar 
    sidebar
    [menuItems]="menuItems"
  ></organism-sidebar>

  <!-- Main Content -->
  <h1>Dashboard</h1>
  <p>Contenido principal... </p>
</template-dashboard-layout>
```

---

### Nivel 5: Páginas 📱

**Definición**: Instancias específicas de **templates** con datos reales. Son las páginas que los usuarios finalmente ven.

**Características:**
- ✅ **Datos reales**: Usan información específica
- ✅ **Instancias de templates**: Un template, muchas páginas
- ✅ **Lógica de negocio completa**: Interactúan con servicios, APIs
- ✅ **Enrutables**:  Asociadas a rutas específicas
- ✅ **Context-aware**: Conocen el contexto completo

**Ejemplos de Páginas:**

| Página | Template usado | Organismos específicos |
|--------|---------------|----------------------|
| **HomePage** | LandingLayout | HeroSection, FeaturesGrid |
| **DashboardPage** | DashboardLayout | StatsCards, ChartWidget |
| **ProductListPage** | DashboardLayout | ProductGrid, Filters |
| **LoginPage** | AuthLayout | LoginForm |

**Ejemplo en Angular - Página DashboardPage:**

```typescript
// dashboard. page.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { DashboardLayoutComponent } from '../../templates/dashboard-layout/dashboard-layout.component';
import { HeaderComponent } from '../../organisms/header/header.component';
import { SidebarComponent } from '../../organisms/sidebar/sidebar. component';
import { StatsCardComponent } from '../../organisms/stats-card/stats-card.component';
import { DashboardService } from '../../services/dashboard.service';

@Component({
  selector: 'page-dashboard',
  standalone: true,
  imports: [
    CommonModule,
    DashboardLayoutComponent,
    HeaderComponent,
    SidebarComponent,
    StatsCardComponent
  ],
  template: `
    <template-dashboard-layout>
      <!-- Header -->
      <organism-header
        header
        [user]="currentUser"
        (logout)="handleLogout()"
      ></organism-header>

      <!-- Sidebar -->
      <organism-sidebar
        sidebar
        [menuItems]="sidebarMenu"
      ></organism-sidebar>

      <!-- Main Content -->
      <div class="dashboard-page">
        <h1>Dashboard</h1>
        
        <!-- Stats Cards -->
        <div class="dashboard-page__stats">
          <organism-stats-card
            title="Total Usuarios"
            [value]="stats?. totalUsers"
            icon="👥"
            trend="up"
            [change]="stats?.growth"
          ></organism-stats-card>

          <organism-stats-card
            title="Ingresos"
            [value]="stats?.revenue"
            prefix="$"
            icon="💰"
            trend="up"
          ></organism-stats-card>
        </div>
      </div>
    </template-dashboard-layout>
  `,
  styleUrls: ['./dashboard.page.scss']
})
export class DashboardPage implements OnInit {
  currentUser: any;
  stats: any;

  sidebarMenu = [
    { label: 'Inicio', route: '/dashboard', icon: '🏠' },
    { label: 'Usuarios', route: '/users', icon: '👥' },
    { label: 'Productos', route: '/products', icon: '📦' }
  ];

  constructor(private dashboardService: DashboardService) {}

  ngOnInit(): void {
    this.loadDashboardData();
  }

  loadDashboardData(): void {
    this.dashboardService.getStats().subscribe(stats => {
      this.stats = stats;
    });
  }

  handleLogout(): void {
    // Lógica de logout
  }
}
```

---

## 🏗️ Estructura de Carpetas con Diseño Atómico

### Opción Recomendada (Híbrida para Angular)

```
src/app/
├── core/                          # Servicios singleton
│   ├── services/
│   ├── guards/
│   └── interceptors/
│
├── shared/                        # Componentes reutilizables
│   ├── atoms/
│   │   ├── button/
│   │   ├── input/
│   │   ├── avatar/
│   │   └── index.ts              # Barrel export
│   ├── molecules/
│   │   ├── search-bar/
│   │   ├── form-field/
│   │   └── index.ts
│   ├── pipes/
│   ├── directives/
│   └── models/
│
├── layouts/                       # Templates
│   ├── dashboard-layout/
│   ├── auth-layout/
│   └── index.ts
│
├── features/                      # Módulos por funcionalidad
│   ├── auth/
│   │   ├── components/           # Organismos de auth
│   │   ├── pages/
│   │   ├── services/
│   │   └── auth.routes.ts
│   │
│   ├── dashboard/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── dashboard.routes.ts
│   │
│   └── products/
│       ├── components/
│       ├── pages/
│       ├── services/
│       └── products.routes.ts
│
├── app.component.ts
├── app.config.ts
└── app.routes.ts
```

---

## ⚠️ Anti-Patrones y Errores Comunes

### ❌ Anti-Patrón 1: Átomos con Lógica de Negocio

```typescript
// ❌ MAL: Botón que hace peticiones HTTP
@Component({
  selector: 'atom-button'
})
export class ButtonComponent {
  constructor(private http: HttpClient) {}
  
  onClick() {
    this.http.post('/api/save', data).subscribe(...);
  }
}

// ✅ BIEN:  Botón solo emite eventos
@Component({
  selector: 'atom-button'
})
export class ButtonComponent {
  @Output() clicked = new EventEmitter<void>();
  
  onClick() {
    this.clicked.emit();
  }
}
```

### ❌ Anti-Patrón 2: Moléculas sin Átomos

```typescript
// ❌ MAL: SearchBar que no reutiliza átomos
@Component({
  selector: 'molecule-search-bar',
  template: `
    <input type="text" class="custom-input">
    <button class="custom-button">Buscar</button>
  `
})

// ✅ BIEN:  Componer con átomos
@Component({
  selector: 'molecule-search-bar',
  template: `
    <atom-input></atom-input>
    <atom-button text="Buscar"></atom-button>
  `
})
```

### ❌ Anti-Patrón 3: Sobre-Abstraer

```typescript
// ❌ MAL: Átomo demasiado genérico
@Component({
  selector: 'atom-element'
})
export class ElementComponent {
  @Input() type: string;
  @Input() config: any;
  @Input() behavior: string;
  // 50+ inputs más...
}
```

---

## 📚 Storybook y Documentación

### Instalar Storybook en Angular

```bash
npx storybook@latest init
```

### Ejemplo de Story

```typescript
// button.stories.ts
import type { Meta, StoryObj } from '@storybook/angular';
import { ButtonComponent } from './button.component';

const meta: Meta<ButtonComponent> = {
  title: 'Atoms/Button',
  component: ButtonComponent,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'danger', 'ghost']
    }
  }
};

export default meta;
type Story = StoryObj<ButtonComponent>;

export const Primary: Story = {
  args: {
    text: 'Primary Button',
    variant: 'primary'
  }
};

export const Danger: Story = {
  args:  {
    text: 'Delete',
    variant: 'danger',
    iconLeft: '🗑️'
  }
};

export const Loading: Story = {
  args:  {
    text: 'Loading...',
    loading: true
  }
};
```

---

## ✅ Buenas Prácticas

1. **Responsabilidad Única**: Cada componente una sola responsabilidad
2. **Composición sobre Herencia**: Componer, no heredar
3. **Props Explícitos**: Tipos claros, no `any`
4. **Documentar**: JSDoc en componentes
5. **Prefijos en Selectores**: `atom-`, `molecule-`, `organism-`
6. **Barrel Exports**: Facilitar imports

```typescript
// shared/atoms/index.ts
export * from './button/button.component';
export * from './input/input.component';

// Uso
import { ButtonComponent, InputComponent } from '@shared/atoms';
```

---

## 🎯 Ejercicio Práctico Final

### Proyecto:  Sistema de Diseño para App de Tareas

**Átomos (mínimo 8):**
- [ ] Button (4 variantes, 3 tamaños)
- [ ] Input (text, email, password, search)
- [ ] Checkbox
- [ ] Avatar
- [ ] Badge
- [ ] Icon
- [ ] Spinner
- [ ] Radio button

**Moléculas (mínimo 5):**
- [ ] SearchBar (Input + Button)
- [ ] FormField (Label + Input + Error)
- [ ] CheckboxGroup
- [ ] FilterBar
- [ ] Pagination

**Organismos (mínimo 4):**
- [ ] Header (Logo + Nav + Search + UserMenu)
- [ ] TaskCard (Avatar + Title + Description + Actions)
- [ ] TaskList (múltiples TaskCards + Filters)
- [ ] Sidebar (Logo + Menu + Footer)

**Templates (mínimo 2):**
- [ ] AppLayout (Header + Sidebar + Main)
- [ ] AuthLayout (Centrado)

**Páginas (mínimo 3):**
- [ ] TaskListPage
- [ ] TaskDetailPage
- [ ] LoginPage

---

## 📚 Recursos

### Documentación
- **Atomic Design (libro)**: https://atomicdesign.bradfrost.com/
- **Storybook**: https://storybook.js.org/

### Design Systems de Referencia
- **Material Design**: https://material.io/
- **IBM Carbon**: https://carbondesignsystem.com/
- **Atlassian Design**: https://atlassian.design/
- **Shopify Polaris**: https://polaris.shopify.com/

---

## ✅ Checklist

- [ ] Identificar átomos, moléculas, organismos
- [ ] Estructurar carpetas con metodología atómica
- [ ] Crear componentes reutilizables
- [ ] Implementar composición
- [ ] Documentar en Storybook
- [ ] Evitar anti-patrones

---

## 🎉 Conclusión

El Diseño Atómico te ayuda a: 
- ✅ Pensar en sistemas, no páginas
- ✅ Construir componentes reutilizables
- ✅ Mantener consistencia visual
- ✅ Escalar proyectos sosteniblemente
- ✅ Colaborar efectivamente
- ✅ Reducir deuda técnica

En Angular, combina perfectamente con: 
- Componentes standalone
- Lazy loading
- Dependency injection
- Signals

---

**¡Felicidades! ** Has completado Diseño Atómico. 

→ **Siguiente:  [Módulo 2: Componentes y Servicios](./MODULO-2.md)**

---

**Última actualización**:  Enero 2026  
**Autor**: Prof. David Luna  
**Referencias**: Brad Frost - Atomic Design (2013)

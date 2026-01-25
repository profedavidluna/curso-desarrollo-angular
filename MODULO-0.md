# 📘 Módulo 0: Fundamentos de CSS - Flexbox y Grid Layout

**Duración**:  6 horas  
**Prerequisito para**:  Módulo 1  
**Branch**: [`modulo-0-css-layouts`](../../tree/modulo-0-css-layouts)

---

## 🎯 Objetivos del Módulo

Al completar este módulo, serás capaz de: 

- ✅ Comprender cuándo usar Flexbox vs Grid vs ambos combinados
- ✅ Dominar Flexbox para layouts unidimensionales (filas o columnas)
- ✅ Utilizar CSS Grid para layouts bidimensionales complejos
- ✅ Crear diseños responsive modernos sin frameworks CSS
- ✅ Combinar Flexbox y Grid para layouts profesionales
- ✅ Aplicar mejores prácticas de diseño web moderno
- ✅ Prepararte para estilizar componentes Angular de manera efectiva

---

## 📚 ¿Por qué CSS Layout antes de Angular?

### Importancia en el Desarrollo Web Moderno

En 2026, **Flexbox y Grid** son los pilares fundamentales del diseño web moderno:

**🎨 Ventajas sobre métodos antiguos (float, position)**
- ✅ Código más limpio y semántico
- ✅ Menos hacks y workarounds
- ✅ Responsive por naturaleza
- ✅ Mejor performance
- ✅ Más mantenible

**🚀 Por qué son esenciales para Angular**
- **Componentes reutilizables**:  Cada componente Angular necesita un layout interno bien estructurado
- **Interfaces profesionales**: Sin librerías pesadas (Bootstrap, Tailwind) que aumentan el bundle size
- **Flexibilidad**: Adaptar componentes a diferentes contextos
- **Performance**: CSS puro es más rápido que librerías CSS
- **Control total**: Diseños personalizados sin limitaciones de frameworks

### Casos de Uso Reales en Aplicaciones Angular

| Caso de Uso | Herramienta | Por qué |
|-------------|-------------|---------|
| Navbar con logo, menú y acciones | **Flexbox** | Distribución horizontal, alineación vertical |
| Dashboard con cards | **Grid + Flexbox** | Grid para layout, Flexbox para contenido interno |
| Formularios complejos | **Grid** | Control preciso de columnas |
| Lista de productos/items | **Flexbox** | Wrapping automático |
| Layout de app (header, sidebar, main) | **Grid** | Áreas con nombre, estructura clara |
| Card con header, contenido, footer | **Flexbox** | Distribuir contenido verticalmente |

---

## 🔷 Parte 1: Flexbox - El Layout Unidimensional

### Conceptos Fundamentales

**Flexbox** organiza elementos en **una sola dirección**:  fila (→) o columna (↓).

#### Ejes de Flexbox

```
FLEX-DIRECTION:  ROW (default)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━→ Main Axis (eje principal)

    ┌───────┐  ┌───────┐  ┌───────┐
    │   1   │  │   2   │  │   3   │    ↑
    └───────┘  └───────┘  └───────┘    │ Cross Axis
                                        │ (eje cruzado)
                                        ↓

FLEX-DIRECTION: COLUMN
    ↓ Main Axis
    
    ┌───────────┐
    │     1     │
    └───────────┘
    ┌───────────┐
    │     2     │    ← Cross Axis →
    └───────────┘
    ┌───────────┐
    │     3     │
    └───────────┘
```

### Propiedades del Container (Padre)

```css
.flex-container {
  /* Activar Flexbox */
  display: flex;
  
  /* DIRECCIÓN:  ¿Horizontal o vertical? */
  flex-direction:  row;              /* Por defecto: izq → der */
  flex-direction:  row-reverse;      /* Invertido: der → izq */
  flex-direction: column;           /* Arriba → abajo */
  flex-direction: column-reverse;   /* Abajo → arriba */
  
  /* WRAP: ¿Permitir múltiples líneas? */
  flex-wrap: nowrap;    /* Default: una sola línea */
  flex-wrap: wrap;      /* Múltiples líneas */
  flex-wrap: wrap-reverse;
  
  /* JUSTIFY-CONTENT:  Distribución en el EJE PRINCIPAL */
  justify-content:  flex-start;     /* Al inicio (default) */
  justify-content:  flex-end;       /* Al final */
  justify-content: center;         /* Centrado */
  justify-content: space-between;  /* Espacio entre items */
  justify-content: space-around;   /* Espacio alrededor */
  justify-content: space-evenly;   /* Espacio uniforme */
  
  /* ALIGN-ITEMS: Alineación en el EJE CRUZADO */
  align-items: stretch;     /* Estirar (default) */
  align-items: flex-start;  /* Al inicio */
  align-items:  flex-end;    /* Al final */
  align-items: center;      /* Centrado */
  align-items: baseline;    /* Por línea base de texto */
  
  /* ALIGN-CONTENT:  Múltiples líneas en EJE CRUZADO */
  align-content: stretch;
  align-content: flex-start;
  align-content: center;
  align-content: space-between;
  
  /* GAP: Espaciado moderno (¡no usar margins!) */
  gap: 20px;              /* Entre filas y columnas */
  row-gap: 20px;          /* Solo entre filas */
  column-gap: 20px;       /* Solo entre columnas */
}
```

### Propiedades de los Items (Hijos)

```css
.flex-item {
  /* ORDEN: Cambiar orden visual (sin tocar HTML) */
  order: 0;  /* Default, menor = primero */
  
  /* FLEX-GROW: ¿Puede crecer? */
  flex-grow: 0;   /* Default: NO crece */
  flex-grow:  1;   /* Crece proporcionalmente */
  flex-grow: 2;   /* Crece 2x más que los de grow:  1 */
  
  /* FLEX-SHRINK: ¿Puede encogerse? */
  flex-shrink: 1;  /* Default: SÍ se encoge */
  flex-shrink: 0;  /* NO se encoge nunca */
  
  /* FLEX-BASIS: Tamaño base ANTES de grow/shrink */
  flex-basis: auto;   /* Default: según contenido */
  flex-basis: 200px;  /* Tamaño inicial fijo */
  flex-basis:  50%;    /* Porcentaje del container */
  
  /* FLEX:  Shorthand (grow shrink basis) */
  flex: 0 1 auto;    /* Default completo */
  flex: 1;           /* Equivale a:  1 1 0 */
  flex: 1 1 300px;   /* grow=1, shrink=1, base=300px */
  
  /* ALIGN-SELF:  Alineación individual */
  align-self: auto;        /* Hereda de align-items */
  align-self: flex-start;
  align-self: center;
  align-self: flex-end;
  align-self: stretch;
}
```

### 🎓 Caso de Estudio 1: Navbar Responsive

**Problema real**: Crear una barra de navegación que funcione en desktop y mobile.

**Requisitos**:
- Logo a la izquierda
- Enlaces de navegación al centro
- Botones de acción a la derecha
- En móvil: layout vertical con menú hamburguesa

**Solución con Flexbox**:

```css
/* Container principal */
.navbar {
  display: flex;
  justify-content: space-between;  /* Logo | Nav | Actions */
  align-items: center;              /* Alineación vertical */
  padding: 1rem 2rem;
  background:  #2c3e50;
}

/* Sección de navegación */
.nav-links {
  display: flex;
  gap: 2rem;                        /* Espaciado entre links */
}

/* Acciones (botones) */
.nav-actions {
  display: flex;
  gap: 1rem;
  align-items: center;
}

/* Responsive */
@media (max-width: 768px) {
  .navbar {
    flex-direction: column;         /* Cambiar a vertical */
    align-items: flex-start;
  }
  
  .nav-links {
    flex-direction: column;
    width: 100%;
  }
}
```

**Por qué Flexbox aquí?**
- ✅ Layout unidimensional (horizontal)
- ✅ Distribución automática del espacio
- ✅ Fácil alineación vertical
- ✅ Simple de hacer responsive

---

## 🟦 Parte 2: CSS Grid - El Layout Bidimensional

### Conceptos Fundamentales

**Grid** organiza elementos en **dos dimensiones**: filas Y columnas simultáneamente.

#### Anatomía de Grid

```
Grid Container
┌─────────────────────────────────────────┐
│  Line 1                    Line 4       │
│    ↓                         ↓          │
│  ┌────────┬────────┬────────┐  ← Line 1│
│  │ 1,1    │ 1,2    │ 1,3    │           │
│  │ (cell) │        │        │           │
│  ├────────┼────────┼────────┤  ← Line 2│
│  │ 2,1    │ 2,2    │ 2,3    │           │
│  │        │        │        │           │
│  └────────┴────────┴────────┘  ← Line 3│
│                                         │
└─────────────────────────────────────────┘
    ↑         ↑         ↑
  Col 1     Col 2     Col 3
```

### Propiedades del Container

```css
.grid-container {
  /* Activar Grid */
  display:  grid;
  
  /* COLUMNAS: Definir estructura horizontal */
  grid-template-columns: 200px 400px 200px;        /* 3 columnas fijas */
  grid-template-columns: 1fr 2fr 1fr;              /* Proporciones (fr = fraction) */
  grid-template-columns: repeat(3, 1fr);           /* 3 columnas iguales */
  grid-template-columns: repeat(4, 200px);         /* 4 columnas de 200px */
  grid-template-columns: 200px auto 200px;         /* Auto se ajusta */
  
  /* AUTO-FIT y AUTO-FILL: Grid responsive sin media queries */
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  /* auto-fit: Ajusta columnas al espacio disponible */
  /* auto-fill: Crea todas las columnas posibles */
  /* minmax(250px, 1fr): Mínimo 250px, máximo 1fr */
  
  /* FILAS: Definir estructura vertical */
  grid-template-rows: 100px auto 50px;
  grid-template-rows: repeat(3, 200px);
  
  /* AUTO-ROWS: Tamaño de filas implícitas */
  grid-auto-rows: 100px;
  grid-auto-rows: minmax(100px, auto);
  
  /* GAP: Espaciado entre celdas */
  gap: 20px;
  row-gap: 20px;
  column-gap: 20px;
  
  /* AREAS: Dar nombres a zonas (¡super útil!) */
  grid-template-areas: 
    "header header header"
    "sidebar main main"
    "footer footer footer";
  
  /* ALINEACIÓN DE ITEMS */
  justify-items: start | end | center | stretch;  /* Horizontal */
  align-items: start | end | center | stretch;    /* Vertical */
  
  /* ALINEACIÓN DEL GRID COMPLETO */
  justify-content: start | end | center | space-between;
  align-content: start | end | center | space-between;
}
```

### Propiedades de los Items

```css
.grid-item {
  /* POSICIONAMIENTO POR LÍNEAS */
  grid-column-start: 1;
  grid-column-end: 3;      /* De línea 1 a 3 */
  
  /* SHORTHAND para columnas */
  grid-column:  1 / 3;      /* De línea 1 a 3 */
  grid-column:  1 / span 2; /* Desde 1, ocupar 2 columnas */
  grid-column: span 2;     /* Ocupar 2 columnas (desde donde esté) */
  
  /* SHORTHAND para filas */
  grid-row:  1 / 3;
  grid-row: span 2;
  
  /* ASIGNAR A UN ÁREA */
  grid-area: header;       /* Usa área definida en template-areas */
  
  /* ALINEACIÓN INDIVIDUAL */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

### 🎓 Caso de Estudio 2: Layout de Aplicación Completa

**Problema real**:  Estructura típica de una aplicación web.

**Requisitos**: 
- Header en la parte superior (full width)
- Sidebar lateral (fijo 250px)
- Área de contenido principal (flexible)
- Footer en la parte inferior (full width)

**Solución con Grid**:

```css
. app-layout {
  display:  grid;
  
  /* Definir áreas con nombres */
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  
  /* Columnas:  sidebar fijo, main flexible */
  grid-template-columns: 250px 1fr;
  
  /* Filas: header fijo, main crece, footer fijo */
  grid-template-rows: 60px 1fr 50px;
  
  height: 100vh;  /* Ocupa toda la pantalla */
}

/* Asignar cada elemento a su área */
.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

**Por qué Grid aquí?**
- ✅ Layout bidimensional (filas Y columnas)
- ✅ Áreas con nombres = código legible
- ✅ Control preciso de tamaños
- ✅ Fácil reorganizar para responsive

---

## 🔥 Parte 3: Flexbox + Grid = Layouts Profesionales

### ¿Cuándo usar cada uno?

| Escenario | Usar | Razón |
|-----------|------|-------|
| Navbar horizontal | **Flexbox** | Una fila de elementos |
| Grid de cards/productos | **Grid** | Distribución 2D con wrap automático |
| Card individual (header, body, footer) | **Flexbox** | Distribución vertical interna |
| Layout de página completa | **Grid** | Estructura general 2D |
| Lista de comentarios | **Flexbox** | Secuencia vertical simple |
| Dashboard con widgets | **Grid + Flexbox** | Grid para layout, Flex para widgets |
| Formulario multi-columna | **Grid** | Control preciso de columnas |
| Centering (horizontal y vertical) | **Flexbox** o **Grid** | Ambos funcionan bien |

### 🎓 Caso de Estudio 3: Dashboard Moderno

**Problema real**: Panel de control con diferentes tamaños de widgets.

**Requisitos**: 
- Cards de diferentes tamaños
- Responsive automático
- Algunos widgets ocupan más espacio
- Contenido interno bien organizado

**Solución combinada**:

```css
/* Grid para el layout general del dashboard */
.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-auto-rows: 100px;  /* Filas de 100px para control granular */
  gap: 20px;
}

/* Widgets de diferentes tamaños */
.widget-small  { grid-row: span 2; }  /* 200px alto */
.widget-medium { grid-row: span 3; }  /* 300px alto */
. widget-large  { grid-row: span 4; }  /* 400px alto */
.widget-wide   { grid-column: span 2; } /* 2 columnas de ancho */

/* Flexbox para el contenido INTERNO de cada widget */
.widget {
  display: flex;
  flex-direction: column;
  padding: 20px;
  background: white;
  border-radius: 12px;
}

. widget-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.widget-body {
  flex: 1;  /* Ocupa espacio disponible */
  overflow-y: auto;
}

.widget-footer {
  margin-top: auto;  /* Se pega al fondo */
  padding-top: 16px;
  border-top: 1px solid #eee;
}
```

**Por qué esta combinación? **
- ✅ **Grid**: Layout externo, control 2D, diferentes tamaños
- ✅ **Flexbox**: Contenido interno, distribución vertical, alineación
- ✅ Cada herramienta hace lo que mejor sabe hacer

---

## ✏️ EJERCICIOS INTERMEDIOS

### 📝 Ejercicio 1: Sistema de Cards de Productos (Flexbox)

**Nivel**:  Intermedio  
**Duración estimada**: 45 minutos  
**Tecnología**: Flexbox

#### Contexto
Estás desarrollando una tienda online y necesitas mostrar productos en cards responsivas.

#### Requisitos
1. **Layout de cards**:
   - Mínimo 250px de ancho por card
   - Máximo 3 cards por fila en desktop
   - 1 card por fila en móvil (<768px)
   - Espaciado uniforme de 20px

2. **Estructura de cada card**:
   ```
   ┌─────────────────┐
   │     Imagen      │
   ├─────────────────┤
   │     Título      │
   │  Descripción    │
   │     Precio      │
   ├─────────────────┤
   │  [Añadir cesta] │
   └─────────────────┘
   ```

3. **Características**:
   - Imagen:  aspect-ratio 16:9, cover
   - Botón siempre al fondo (sin importar largo de descripción)
   - Hover: elevación con transform y box-shadow
   - Badge de "NUEVO" o "OFERTA" posicionado sobre imagen

#### Criterios de Éxito
- [ ] Cards se distribuyen automáticamente
- [ ] Responsive funciona correctamente
- [ ] Botón siempre al fondo de la card
- [ ] Hover effect suave (transition)
- [ ] Badge posicionado correctamente

#### Pistas
```css
. products-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.product-card {
  flex: 1 1 250px;        /* grow shrink basis */
  max-width: calc(33.333% - 14px);  /* Max 3 por fila */
  display: flex;
  flex-direction: column;
}

.product-image {
  position: relative;     /* Para el badge */
  aspect-ratio: 16/9;
}

.product-body {
  flex: 1;               /* Ocupa espacio disponible */
}

.product-button {
  margin-top: auto;      /* Se pega al fondo */
}
```

---

### 📝 Ejercicio 2: Formulario de Registro Multi-Columna (Grid)

**Nivel**: Intermedio  
**Duración estimada**:  50 minutos  
**Tecnología**: CSS Grid

#### Contexto
Crear un formulario de registro de usuario con layout profesional.

#### Requisitos

1. **Layout desktop (2 columnas)**:
   ```
   ┌─────────────┬─────────────┐
   │   Nombre    │   Apellido  │
   ├─────────────┴─────────────┤
   │          Email            │
   ├─────────────┬─────────────┤
   │  Teléfono   │    Fecha    │
   ├─────────────┴─────────────┤
   │         Dirección         │
   ├─────────────┬─────────────┤
   │   Ciudad    │     CP      │
   ├─────────────┴─────────────┤
   │        Biografía          │
   │       (textarea)          │
   ├───────────────────────────┤
   │       [Registrar]         │
   └───────────────────────────┘
   ```

2. **Campos que ocupan 2 columnas**: 
   - Email
   - Dirección
   - Biografía
   - Botón submit

3. **Responsive móvil**:
   - 1 columna en pantallas <768px

4. **Validación visual**:
   - Border verde si válido
   - Border rojo si inválido
   - Mensaje de error debajo del campo

#### Criterios de Éxito
- [ ] Grid de 2 columnas funciona correctamente
- [ ] Campos específicos ocupan 2 columnas
- [ ] Botón centrado horizontalmente
- [ ] Responsive a 1 columna en móvil
- [ ] Espaciado consistente (gap)

#### Pistas
```css
.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  max-width: 800px;
  margin: 0 auto;
}

.form-group-full {
  grid-column: span 2;  /* Ocupar 2 columnas */
}

.submit-button {
  grid-column: span 2;
  justify-self: center;  /* Centrado horizontal */
}

@media (max-width: 768px) {
  .form-grid {
    grid-template-columns: 1fr;
  }
  
  .form-group-full,
  .submit-button {
    grid-column: span 1;
  }
}
```

---

### 📝 Ejercicio 3: Galería de Imágenes tipo Pinterest (Grid)

**Nivel**: Intermedio-Avanzado  
**Duración estimada**: 60 minutos  
**Tecnología**: CSS Grid

#### Contexto
Crear una galería tipo "masonry" donde las imágenes tienen diferentes alturas.

#### Requisitos

1. **Layout masonry**:
   - Columnas automáticas (min 250px)
   - Imágenes de diferentes alturas
   - Sin espacios vacíos innecesarios

2. **Cada item**:
   - Imagen
   - Overlay con título al hacer hover
   - Ícono de "corazón" para favoritos
   - Autor y fecha

3. **Interactividad**:
   - Hover: overlay con opacidad y blur
   - Click en corazón:  toggle favorito
   - Modal al click en imagen

#### Criterios de Éxito
- [ ] Grid responsive sin media queries
- [ ] Items de diferentes alturas encajan bien
- [ ] Overlay aparece suavemente (transition)
- [ ] Responsive funciona automáticamente

#### Pistas
```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-auto-rows: 10px;  /* Filas pequeñas para control granular */
  gap:  16px;
}

.gallery-item {
  /* Ocupar X filas según altura de imagen */
  grid-row-end: span 20;  /* Pequeña:  200px */
}

.gallery-item. medium {
  grid-row-end: span 30;  /* Mediana: 300px */
}

.gallery-item.large {
  grid-row-end: span 40;  /* Grande: 400px */
}

/* JavaScript para calcular height dinámicamente */
.gallery-item img {
  width: 100%;
  height:  100%;
  object-fit:  cover;
}
```

---

## 🔥 EJERCICIOS AVANZADOS

### 🚀 Ejercicio 4: Layout de Blog Complejo (Grid + Flexbox)

**Nivel**: Avanzado  
**Duración estimada**: 90 minutos  
**Tecnología**: Grid + Flexbox combinados

#### Contexto
Crear un layout de blog profesional con sidebar, contenido principal, widgets y áreas dinámicas.

#### Requisitos

1. **Estructura desktop**:
   ```
   ┌─────────────────────────────────────────────┐
   │              HEADER (navbar)                │
   ├──────────┬────────────────────┬─────────────┤
   │          │                    │             │
   │ SIDEBAR  │    MAIN CONTENT    │   WIDGETS   │
   │ (250px)  │      (flexible)    │   (300px)   │
   │          │                    │             │
   ├──────────┴────────────────────┴─────────────┤
   │                  FOOTER                     │
   └─────────────────────────────────────────────┘
   ```

2. **Main Content**:
   - Posts en grid de 2 columnas
   - Cada post es un flex container vertical
   - Featured post ocupa 2 columnas

3. **Widgets** (cada uno usa Flexbox interno):
   - Búsqueda
   - Categorías
   - Posts populares (lista)
   - Newsletter (formulario)

4. **Responsive**:
   - Desktop (>1200px): 3 columnas
   - Tablet (768px-1200px): solo main + widgets
   - Mobile (<768px): todo en 1 columna

#### Criterios de Éxito
- [ ] Layout de 3 columnas funciona
- [ ] Grid dentro de main content
- [ ] Flexbox en cada widget
- [ ] 3 breakpoints responsive
- [ ] Sidebar colapsable en tablet
- [ ] Featured post destacado correctamente

#### Código Base
```css
/* Grid para layout principal */
.blog-layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main widgets"
    "footer footer footer";
  grid-template-columns:  250px 1fr 300px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

. header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.widgets { grid-area: widgets; }
.footer  { grid-area: footer; }

/* Grid para posts */
.posts-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}

.post-featured {
  grid-column: span 2;
}

/* Flexbox para cada post */
.post-card {
  display: flex;
  flex-direction: column;
  background: white;
  border-radius: 12px;
  overflow: hidden;
}

.post-image { /* ...  */ }
.post-body { flex: 1; }
.post-footer { margin-top: auto; }

/* Responsive */
@media (max-width: 1200px) {
  .blog-layout {
    grid-template-areas:
      "header header"
      "main widgets"
      "footer footer";
    grid-template-columns: 1fr 300px;
  }
  
  .sidebar { display: none; }
}

@media (max-width: 768px) {
  .blog-layout {
    grid-template-areas:
      "header"
      "main"
      "widgets"
      "footer";
    grid-template-columns: 1fr;
  }
  
  .posts-grid {
    grid-template-columns: 1fr;
  }
  
  .post-featured {
    grid-column: span 1;
  }
}
```

---

### 🚀 Ejercicio 5: Dashboard Analítico Avanzado (Grid Complejo)

**Nivel**: Avanzado  
**Duración estimada**: 120 minutos  
**Tecnología**:  Grid + Flexbox + Positioning

#### Contexto
Dashboard tipo Google Analytics con gráficos, métricas y widgets de diferentes tamaños. 

#### Requisitos

1. **Layout en Grid de 12 columnas**:
   ```
   ┌──────────────────────────────────────────┐
   │     HEADER (12 columnas)                 │
   ├────────┬─────────────────────────┬───────┤
   │ KPI 1  │        KPI 2            │ KPI 3 │
   │ (3)    │         (6)             │  (3)  │
   ├────────┴─────────────────────────┴───────┤
   │         GRÁFICO PRINCIPAL               │
   │              (12)                       │
   ├──────────────────┬──────────────────────┤
   │   Gráfico 2 (6)  │   Gráfico 3 (6)     │
   ├──────────────────┼──────────────────────┤
   │   Lista (4)      │    Mapa (8)          │
   └──────────────────┴──────────────────────┘
   ```

2. **Widgets especiales**:
   - KPIs con tendencia (↑ ↓)
   - Gráficos con placeholder
   - Tabla con scroll interno
   - Mapa interactivo

3. **Funcionalidades**:
   - Drag & drop para reordenar (JS)
   - Resize de widgets
   - Ocultar/mostrar widgets
   - Exportar a PDF

4. **Responsive**:
   - 12 columnas en desktop
   - 6 columnas en tablet
   - 1 columna en móvil

#### Criterios de Éxito
- [ ] Sistema de 12 columnas funciona
- [ ] Widgets de diferentes tamaños
- [ ] Responsive a 6 y 1 columna
- [ ] Scroll interno en widgets donde aplique
- [ ] Flexbox para contenido interno de cada widget

#### Código Base
```css
/* Sistema de grid de 12 columnas */
. dashboard {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-auto-rows: minmax(100px, auto);
  gap: 20px;
  padding:  20px;
}

/* Clases de utilidad para spans */
.col-span-3  { grid-column: span 3; }
.col-span-4  { grid-column: span 4; }
.col-span-6  { grid-column:  span 6; }
.col-span-8  { grid-column: span 8; }
.col-span-12 { grid-column: span 12; }

. row-span-2  { grid-row: span 2; }
.row-span-3  { grid-row: span 3; }

/* Widget base con Flexbox */
.widget {
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  
  display: flex;
  flex-direction: column;
}

. widget-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.widget-body {
  flex: 1;
  overflow-y: auto;
}

/* KPI específico */
.kpi-widget {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.kpi-value {
  font-size: 3rem;
  font-weight: bold;
  color: #2c3e50;
}

.kpi-trend {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #27ae60;
}

. kpi-trend. down {
  color: #e74c3c;
}

/* Responsive */
@media (max-width: 1024px) {
  .dashboard {
    grid-template-columns: repeat(6, 1fr);
  }
  
  /* Reajustar spans */
  .col-span-3  { grid-column: span 3; }
  .col-span-4  { grid-column:  span 3; }
  .col-span-6  { grid-column:  span 6; }
  .col-span-8  { grid-column:  span 6; }
  .col-span-12 { grid-column:  span 6; }
}

@media (max-width:  768px) {
  .dashboard {
    grid-template-columns: 1fr;
  }
  
  .col-span-3,
  .col-span-4,
  .col-span-6,
  .col-span-8,
  .col-span-12 {
    grid-column: span 1;
  }
}
```

---

## 📚 Recursos y Referencias

### Documentación Oficial
- [MDN - Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)
- [MDN - Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
- [CSS Tricks - A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS Tricks - A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

### Herramientas Interactivas
- [Flexbox Froggy](https://flexboxfroggy.com/) - Juego para aprender Flexbox
- [Grid Garden](https://cssgridgarden.com/) - Juego para aprender Grid
- [Flexbox Defense](http://www.flexboxdefense.com/) - Tower defense con Flexbox
- [Grid Critters](https://gridcritters.com/) - Aprende Grid jugando

### Generadores y Visualizadores
- [Flexbox Generator](https://loading.io/flexbox/)
- [Grid Generator](https://grid.layoutit.com/)
- [CSS Grid Generator](https://cssgrid-generator.netlify.app/)

### Patrones y Ejemplos
- [1-Line Layouts](https://1linelayouts.glitch.me/)
- [Grid by Example](https://gridbyexample.com/)
- [Flexbox Patterns](https://www.flexboxpatterns.com/)

---

## 🎯 Casos de Uso Reales en Angular

### 1. Component Layout
```typescript
// character-card.component.css
.card {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.card-body {
  flex: 1; /* Ocupa espacio disponible */
}

. card-actions {
  margin-top: auto; /* Siempre al fondo */
}
```

### 2. Dashboard Grid
```typescript
// dashboard.component.css
.dashboard-grid {
  display: grid;
  grid-template-columns:  repeat(auto-fit, minmax(300px, 1fr));
  gap: 24px;
}
```

### 3. Form Layout
```typescript
// user-form.component.css
.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.form-field-full {
  grid-column: span 2;
}
```

---

## ✅ Checklist de Dominio

Marca cuando puedas hacer lo siguiente sin consultar documentación:

### Flexbox
- [ ] Crear un navbar horizontal con logo, nav y acciones
- [ ] Centrar un elemento horizontal y verticalmente
- [ ] Hacer que un elemento ocupe el espacio restante (flex:  1)
- [ ] Distribuir elementos con espacio entre ellos (space-between)
- [ ] Crear una card con footer pegado al fondo
- [ ] Hacer wrap de elementos y controlar gap
- [ ] Cambiar orden visual sin tocar HTML (order)

### Grid
- [ ] Crear un grid de 3 columnas iguales
- [ ] Hacer un grid responsive sin media queries (auto-fit)
- [ ] Usar grid-template-areas para layout de app
- [ ] Hacer que un elemento ocupe múltiples columnas (span)
- [ ] Crear un grid con columnas fijas y flexibles (200px 1fr)
- [ ] Posicionar un elemento en líneas específicas
- [ ] Controlar el tamaño de filas automáticas (grid-auto-rows)

### Combinados
- [ ] Grid para layout externo, Flexbox para contenido interno
- [ ] Dashboard con widgets de diferentes tamaños
- [ ] Formulario multi-columna con campos que ocupan varias columnas
- [ ] Blog layout con sidebar, main y widgets

---

## 🚀 Proyecto Integrador del Módulo

### Portal de Personajes Rick & Morty - Landing Page

**Objetivo**: Crear una landing page completa usando Flexbox y Grid. 

**Secciones**:
1. **Hero** (Flexbox)
   - Título, descripción, CTA
   - Imagen de fondo
   - Centrado vertical y horizontal

2. **Features Grid** (Grid)
   - 3 columnas en desktop
   - Cards con iconos

3. **Character Showcase** (Grid + Flexbox)
   - Grid de personajes
   - Cards con Flexbox interno

4. **Stats Section** (Flexbox)
   - Números grandes
   - Distribución horizontal

5. **Footer** (Grid)
   - 4 columnas de links
   - Copyright centrado

**Requisitos técnicos**:
- Responsive (mobile-first)
- Sin frameworks CSS
- Smooth scroll
- Animaciones con CSS

---

**¡Éxito en tu aprendizaje!** 💪

Una vez domines Flexbox y Grid, estarás listo para: 
→ [Módulo 1: Introducción a Angular y TypeScript](./MODULO-1.md)

# 📝 Módulo 2: Ejercicios Prácticos - Componentes y Servicios Avanzados

**Duración**: 8-10 horas  
**Nivel**: Básico a Avanzado  
**Prerequisito**: Completar Módulo 2 teórico

---

## 🎯 Objetivo

Esta guía contiene **8 ejercicios** para practicar los conceptos del Módulo 2:
- ✅ **5 ejercicios resueltos** (con código completo)
- ✅ **3 ejercicios para practicar** (solo enunciados)

**Distribución:**
- 1 ejercicio básico
- 4 ejercicios intermedios (2 resueltos, 2 para practicar)
- 3 ejercicios avanzados (2 resueltos, 1 para practicar)

---

## 📚 Índice de Ejercicios

### Ejercicios Resueltos
1. **[BÁSICO]** Sistema de Contador con Componentes Padre-Hijo
2. **[INTERMEDIO]** Lista de Tareas con Comunicación entre Componentes
3. **[INTERMEDIO]** Sistema de Notificaciones con Servicios
4. **[AVANZADO]** Dashboard con Múltiples Fuentes de Datos (RxJS)
5. **[AVANZADO]** Buscador en Tiempo Real con Autocomplete

### Ejercicios para Practicar
6. **[INTERMEDIO]** Sistema de Carrito de Compras
7. **[INTERMEDIO-AVANZADO]** Chat en Tiempo Real
8. **[AVANZADO]** Sistema de Gestión de Usuarios con CRUD Completo

---

# 🟢 EJERCICIOS RESUELTOS

---

## Ejercicio 1: Sistema de Contador con Componentes Padre-Hijo [BÁSICO]

### 📋 Enunciado

Crear una aplicación de contador donde:
- Un componente **padre** muestra el valor actual del contador
- Dos componentes **hijos**:
  - Uno con botones para incrementar/decrementar
  - Otro que muestra el historial de cambios
- Usar `@Input()` y `@Output()` para la comunicación

### 🎯 Objetivos de Aprendizaje
- Comunicación padre-hijo con @Input
- Emisión de eventos con @Output
- EventEmitter
- Pasar datos entre componentes

### 💡 Solución Completa

#### Paso 1: Crear el proyecto

```bash
ng new contador-app --routing=false --style=css --standalone
cd contador-app
```

#### Paso 2: Crear las interfaces

```typescript
// src/app/models/contador-cambio.interface.ts
export interface ContadorCambio {
  valorAnterior: number;
  valorNuevo: number;
  accion: 'incremento' | 'decremento' | 'reset';
  timestamp: Date;
}
```

#### Paso 3: Componente Hijo - Controles

```bash
ng g c components/contador-controles
```

```typescript
// src/app/components/contador-controles/contador-controles.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-contador-controles',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './contador-controles.component.html',
  styleUrls: ['./contador-controles.component.css']
})
export class ContadorControlesComponent {
  @Input() valorActual: number = 0;
  @Input() minimo: number = 0;
  @Input() maximo: number = 100;
  
  @Output() incrementar = new EventEmitter<void>();
  @Output() decrementar = new EventEmitter<void>();
  @Output() reset = new EventEmitter<void>();

  onIncrementar(): void {
    if (this.valorActual < this.maximo) {
      this.incrementar.emit();
    }
  }

  onDecrementar(): void {
    if (this.valorActual > this.minimo) {
      this.decrementar.emit();
    }
  }

  onReset(): void {
    this.reset.emit();
  }

  get puedeIncrementar(): boolean {
    return this.valorActual < this.maximo;
  }

  get puedeDecrementar(): boolean {
    return this.valorActual > this.minimo;
  }
}
```

```html
<!-- src/app/components/contador-controles/contador-controles.component.html -->
<div class="controles">
  <h3>Controles</h3>
  <div class="botones">
    <button 
      (click)="onDecrementar()"
      [disabled]="!puedeDecrementar"
      class="btn btn-danger"
    >
      - Decrementar
    </button>
    
    <button 
      (click)="onReset()"
      class="btn btn-secondary"
    >
      🔄 Reset
    </button>
    
    <button 
      (click)="onIncrementar()"
      [disabled]="!puedeIncrementar"
      class="btn btn-success"
    >
      + Incrementar
    </button>
  </div>
  
  <div class="info">
    <small>Rango: {{ minimo }} - {{ maximo }}</small>
  </div>
</div>
```

```css
/* src/app/components/contador-controles/contador-controles.component.css */
.controles {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  margin: 1rem 0;
}

.botones {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin: 1rem 0;
}

.btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-success {
  background: #28a745;
  color: white;
}

.btn-success:hover:not(:disabled) {
  background: #218838;
  transform: translateY(-2px);
}

.btn-danger {
  background: #dc3545;
  color: white;
}

.btn-danger:hover:not(:disabled) {
  background: #c82333;
  transform: translateY(-2px);
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background: #5a6268;
  transform: translateY(-2px);
}

.info {
  text-align: center;
  color: #6c757d;
  margin-top: 1rem;
}
```

#### Paso 4: Componente Hijo - Historial

```bash
ng g c components/contador-historial
```

```typescript
// src/app/components/contador-historial/contador-historial.component.ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ContadorCambio } from '../../models/contador-cambio.interface';

@Component({
  selector: 'app-contador-historial',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './contador-historial.component.html',
  styleUrls: ['./contador-historial.component.css']
})
export class ContadorHistorialComponent {
  @Input() historial: ContadorCambio[] = [];

  getIcono(accion: string): string {
    switch (accion) {
      case 'incremento': return '⬆️';
      case 'decremento': return '⬇️';
      case 'reset': return '🔄';
      default: return '❓';
    }
  }

  getClaseAccion(accion: string): string {
    switch (accion) {
      case 'incremento': return 'incremento';
      case 'decremento': return 'decremento';
      case 'reset': return 'reset';
      default: return '';
    }
  }
}
```

```html
<!-- src/app/components/contador-historial/contador-historial.component.html -->
<div class="historial">
  <h3>Historial de Cambios</h3>
  
  <div *ngIf="historial.length === 0" class="vacio">
    <p>No hay cambios registrados</p>
  </div>
  
  <div class="lista-historial">
    <div 
      *ngFor="let cambio of historial; let i = index"
      class="item-historial"
      [class]="getClaseAccion(cambio.accion)"
    >
      <div class="icono">{{ getIcono(cambio.accion) }}</div>
      <div class="info">
        <div class="valores">
          <span class="valor-anterior">{{ cambio.valorAnterior }}</span>
          <span class="flecha">→</span>
          <span class="valor-nuevo">{{ cambio.valorNuevo }}</span>
        </div>
        <div class="accion">{{ cambio.accion }}</div>
        <div class="timestamp">
          {{ cambio.timestamp | date:'short' }}
        </div>
      </div>
    </div>
  </div>
</div>
```

```css
/* src/app/components/contador-historial/contador-historial.component.css */
.historial {
  background: #fff;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #dee2e6;
  margin: 1rem 0;
}

h3 {
  margin-bottom: 1rem;
  color: #333;
}

.vacio {
  text-align: center;
  color: #6c757d;
  padding: 2rem;
}

.lista-historial {
  max-height: 400px;
  overflow-y: auto;
}

.item-historial {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  margin-bottom: 0.5rem;
  border-radius: 6px;
  border-left: 4px solid;
  background: #f8f9fa;
}

.item-historial.incremento {
  border-left-color: #28a745;
}

.item-historial.decremento {
  border-left-color: #dc3545;
}

.item-historial.reset {
  border-left-color: #6c757d;
}

.icono {
  font-size: 1.5rem;
}

.info {
  flex: 1;
}

.valores {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 0.25rem;
}

.valor-anterior {
  color: #6c757d;
}

.flecha {
  margin: 0 0.5rem;
  color: #6c757d;
}

.valor-nuevo {
  color: #007bff;
}

.accion {
  text-transform: capitalize;
  color: #495057;
  font-size: 0.9rem;
}

.timestamp {
  font-size: 0.8rem;
  color: #6c757d;
}
```

#### Paso 5: Componente Padre

```typescript
// src/app/app.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ContadorControlesComponent } from './components/contador-controles/contador-controles.component';
import { ContadorHistorialComponent } from './components/contador-historial/contador-historial.component';
import { ContadorCambio } from './models/contador-cambio.interface';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    CommonModule,
    ContadorControlesComponent,
    ContadorHistorialComponent
  ],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  contador: number = 0;
  minimo: number = 0;
  maximo: number = 100;
  historial: ContadorCambio[] = [];

  onIncrementar(): void {
    const valorAnterior = this.contador;
    this.contador++;
    this.registrarCambio(valorAnterior, this.contador, 'incremento');
  }

  onDecrementar(): void {
    const valorAnterior = this.contador;
    this.contador--;
    this.registrarCambio(valorAnterior, this.contador, 'decremento');
  }

  onReset(): void {
    const valorAnterior = this.contador;
    this.contador = 0;
    this.registrarCambio(valorAnterior, this.contador, 'reset');
  }

  private registrarCambio(
    valorAnterior: number,
    valorNuevo: number,
    accion: 'incremento' | 'decremento' | 'reset'
  ): void {
    this.historial.unshift({
      valorAnterior,
      valorNuevo,
      accion,
      timestamp: new Date()
    });
  }

  get porcentaje(): number {
    return (this.contador / this.maximo) * 100;
  }

  get colorBarra(): string {
    if (this.porcentaje < 33) return '#28a745';
    if (this.porcentaje < 66) return '#ffc107';
    return '#dc3545';
  }
}
```

```html
<!-- src/app/app.component.html -->
<div class="app-container">
  <header>
    <h1>🔢 Sistema de Contador</h1>
    <p>Ejercicio de comunicación entre componentes</p>
  </header>

  <main>
    <div class="display-contador">
      <div class="valor-actual">{{ contador }}</div>
      <div class="barra-progreso">
        <div 
          class="barra-relleno"
          [style.width.%]="porcentaje"
          [style.background-color]="colorBarra"
        ></div>
      </div>
      <div class="info-progreso">
        {{ porcentaje | number:'1.0-0' }}% del máximo
      </div>
    </div>

    <app-contador-controles
      [valorActual]="contador"
      [minimo]="minimo"
      [maximo]="maximo"
      (incrementar)="onIncrementar()"
      (decrementar)="onDecrementar()"
      (reset)="onReset()"
    ></app-contador-controles>

    <app-contador-historial
      [historial]="historial"
    ></app-contador-historial>
  </main>
</div>
```

```css
/* src/app/app.component.css */
.app-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  color: #333;
  margin-bottom: 0.5rem;
}

header p {
  color: #6c757d;
}

.display-contador {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  border-radius: 12px;
  text-align: center;
  margin-bottom: 1rem;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.valor-actual {
  font-size: 4rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.barra-progreso {
  background: rgba(255, 255, 255, 0.3);
  height: 20px;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 0.5rem;
}

.barra-relleno {
  height: 100%;
  transition: all 0.3s ease;
}

.info-progreso {
  font-size: 0.9rem;
  opacity: 0.9;
}
```

### ✅ Resultado Esperado

Al ejecutar `ng serve`, deberías ver:
- Un contador grande en el centro con barra de progreso
- Botones para incrementar, decrementar y resetear
- Los botones se deshabilitan al llegar a los límites
- Un historial que muestra todos los cambios con timestamps
- Colores diferentes para cada tipo de acción

### 🎓 Conceptos Aplicados
- ✅ @Input() para pasar datos al hijo
- ✅ @Output() y EventEmitter para emitir eventos
- ✅ Comunicación padre-hijo bidireccional
- ✅ Interfaces TypeScript
- ✅ Bindings: property, event, interpolation
- ✅ Directivas estructurales (*ngFor, *ngIf)
- ✅ Pipes (date, number)

---

## Ejercicio 2: Lista de Tareas con Comunicación entre Componentes [INTERMEDIO]

### 📋 Enunciado

Crear una aplicación de gestión de tareas (TODO List) donde:
- Componente para **agregar tareas**
- Componente para **listar tareas**
- Componente para **cada tarea individual**
- Componente para **filtros** (todas/completadas/pendientes)
- Las tareas se pueden marcar como completadas
- Se pueden eliminar tareas
- Contador de tareas pendientes

### 🎯 Objetivos de Aprendizaje
- Comunicación entre múltiples componentes
- @Input() y @Output() avanzado
- Gestión de estado en el padre
- Componentes reutilizables
- Manejo de arrays

### 💡 Solución Completa

#### Paso 1: Crear el proyecto

```bash
ng new todo-app --routing=false --style=css --standalone
cd todo-app
```

#### Paso 2: Crear las interfaces

```typescript
// src/app/models/tarea.interface.ts
export interface Tarea {
  id: number;
  titulo: string;
  descripcion: string;
  completada: boolean;
  prioridad: 'baja' | 'media' | 'alta';
  fechaCreacion: Date;
}

export type FiltroTarea = 'todas' | 'completadas' | 'pendientes';
```

#### Paso 3: Componente Formulario de Tarea

```bash
ng g c components/tarea-form
```

```typescript
// src/app/components/tarea-form/tarea-form.component.ts
import { Component, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Tarea } from '../../models/tarea.interface';

@Component({
  selector: 'app-tarea-form',
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: './tarea-form.component.html',
  styleUrls: ['./tarea-form.component.css']
})
export class TareaFormComponent {
  @Output() tareaCreada = new EventEmitter<Omit<Tarea, 'id' | 'fechaCreacion' | 'completada'>>();

  titulo: string = '';
  descripcion: string = '';
  prioridad: 'baja' | 'media' | 'alta' = 'media';

  onSubmit(): void {
    if (this.titulo.trim()) {
      this.tareaCreada.emit({
        titulo: this.titulo,
        descripcion: this.descripcion,
        prioridad: this.prioridad
      });
      this.resetForm();
    }
  }

  private resetForm(): void {
    this.titulo = '';
    this.descripcion = '';
    this.prioridad = 'media';
  }
}
```

```html
<!-- src/app/components/tarea-form/tarea-form.component.html -->
<div class="tarea-form">
  <h2>➕ Nueva Tarea</h2>
  <form (ngSubmit)="onSubmit()">
    <div class="form-group">
      <label for="titulo">Título *</label>
      <input
        type="text"
        id="titulo"
        [(ngModel)]="titulo"
        name="titulo"
        placeholder="¿Qué necesitas hacer?"
        required
      >
    </div>

    <div class="form-group">
      <label for="descripcion">Descripción</label>
      <textarea
        id="descripcion"
        [(ngModel)]="descripcion"
        name="descripcion"
        rows="3"
        placeholder="Detalles adicionales (opcional)"
      ></textarea>
    </div>

    <div class="form-group">
      <label for="prioridad">Prioridad</label>
      <select id="prioridad" [(ngModel)]="prioridad" name="prioridad">
        <option value="baja">🟢 Baja</option>
        <option value="media">🟡 Media</option>
        <option value="alta">🔴 Alta</option>
      </select>
    </div>

    <button type="submit" [disabled]="!titulo.trim()" class="btn-submit">
      Agregar Tarea
    </button>
  </form>
</div>
```

```css
/* src/app/components/tarea-form/tarea-form.component.css */
.tarea-form {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

h2 {
  margin-bottom: 1rem;
  color: #333;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #495057;
  font-weight: 500;
}

input, textarea, select {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ced4da;
  border-radius: 4px;
  font-size: 1rem;
  font-family: inherit;
}

input:focus, textarea:focus, select:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
}

.btn-submit {
  width: 100%;
  padding: 0.75rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
}

.btn-submit:hover:not(:disabled) {
  background: #0056b3;
}

.btn-submit:disabled {
  background: #6c757d;
  cursor: not-allowed;
}
```

#### Paso 4: Componente Item de Tarea

```bash
ng g c components/tarea-item
```

```typescript
// src/app/components/tarea-item/tarea-item.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Tarea } from '../../models/tarea.interface';

@Component({
  selector: 'app-tarea-item',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './tarea-item.component.html',
  styleUrls: ['./tarea-item.component.css']
})
export class TareaItemComponent {
  @Input() tarea!: Tarea;
  @Output() toggleCompletada = new EventEmitter<number>();
  @Output() eliminar = new EventEmitter<number>();

  onToggleCompletada(): void {
    this.toggleCompletada.emit(this.tarea.id);
  }

  onEliminar(): void {
    if (confirm(`¿Eliminar tarea "${this.tarea.titulo}"?`)) {
      this.eliminar.emit(this.tarea.id);
    }
  }

  getPrioridadIcon(): string {
    switch (this.tarea.prioridad) {
      case 'alta': return '🔴';
      case 'media': return '🟡';
      case 'baja': return '🟢';
      default: return '';
    }
  }
}
```

```html
<!-- src/app/components/tarea-item/tarea-item.component.html -->
<div class="tarea-item" [class.completada]="tarea.completada">
  <div class="checkbox">
    <input
      type="checkbox"
      [checked]="tarea.completada"
      (change)="onToggleCompletada()"
      [id]="'tarea-' + tarea.id"
    >
    <label [for]="'tarea-' + tarea.id"></label>
  </div>

  <div class="contenido">
    <div class="titulo">
      <span class="prioridad">{{ getPrioridadIcon() }}</span>
      {{ tarea.titulo }}
    </div>
    <div class="descripcion" *ngIf="tarea.descripcion">
      {{ tarea.descripcion }}
    </div>
    <div class="metadata">
      <small>Creada: {{ tarea.fechaCreacion | date:'short' }}</small>
    </div>
  </div>

  <div class="acciones">
    <button class="btn-eliminar" (click)="onEliminar()" title="Eliminar">
      🗑️
    </button>
  </div>
</div>
```

```css
/* src/app/components/tarea-item/tarea-item.component.css */
.tarea-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1rem;
  background: white;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  margin-bottom: 0.75rem;
  transition: all 0.3s ease;
}

.tarea-item:hover {
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transform: translateY(-2px);
}

.tarea-item.completada {
  opacity: 0.7;
  background: #f8f9fa;
}

.checkbox {
  padding-top: 2px;
}

.checkbox input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.contenido {
  flex: 1;
}

.titulo {
  font-weight: 600;
  color: #333;
  margin-bottom: 0.25rem;
}

.tarea-item.completada .titulo {
  text-decoration: line-through;
  color: #6c757d;
}

.prioridad {
  margin-right: 0.5rem;
}

.descripcion {
  color: #6c757d;
  font-size: 0.9rem;
  margin-bottom: 0.5rem;
}

.metadata {
  color: #adb5bd;
  font-size: 0.8rem;
}

.acciones {
  display: flex;
  gap: 0.5rem;
}

.btn-eliminar {
  background: transparent;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  opacity: 0.6;
  transition: opacity 0.3s ease;
}

.btn-eliminar:hover {
  opacity: 1;
}
```

#### Paso 5: Componente Filtros

```bash
ng g c components/tarea-filtros
```

```typescript
// src/app/components/tarea-filtros/tarea-filtros.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FiltroTarea } from '../../models/tarea.interface';

@Component({
  selector: 'app-tarea-filtros',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './tarea-filtros.component.html',
  styleUrls: ['./tarea-filtros.component.css']
})
export class TareaFiltrosComponent {
  @Input() filtroActual: FiltroTarea = 'todas';
  @Input() totalTareas: number = 0;
  @Input() tareasCompletadas: number = 0;
  @Input() tareasPendientes: number = 0;
  
  @Output() cambiarFiltro = new EventEmitter<FiltroTarea>();

  onCambiarFiltro(filtro: FiltroTarea): void {
    this.cambiarFiltro.emit(filtro);
  }
}
```

```html
<!-- src/app/components/tarea-filtros/tarea-filtros.component.html -->
<div class="filtros">
  <div class="estadisticas">
    <div class="stat">
      <div class="stat-valor">{{ totalTareas }}</div>
      <div class="stat-label">Total</div>
    </div>
    <div class="stat">
      <div class="stat-valor">{{ tareasPendientes }}</div>
      <div class="stat-label">Pendientes</div>
    </div>
    <div class="stat">
      <div class="stat-valor">{{ tareasCompletadas }}</div>
      <div class="stat-label">Completadas</div>
    </div>
  </div>

  <div class="botones-filtro">
    <button
      [class.activo]="filtroActual === 'todas'"
      (click)="onCambiarFiltro('todas')"
    >
      Todas
    </button>
    <button
      [class.activo]="filtroActual === 'pendientes'"
      (click)="onCambiarFiltro('pendientes')"
    >
      Pendientes
    </button>
    <button
      [class.activo]="filtroActual === 'completadas'"
      (click)="onCambiarFiltro('completadas')"
    >
      Completadas
    </button>
  </div>
</div>
```

```css
/* src/app/components/tarea-filtros/tarea-filtros.component.css */
.filtros {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.estadisticas {
  display: flex;
  justify-content: space-around;
  margin-bottom: 1.5rem;
  gap: 1rem;
}

.stat {
  text-align: center;
  flex: 1;
}

.stat-valor {
  font-size: 2rem;
  font-weight: bold;
  color: #007bff;
}

.stat-label {
  color: #6c757d;
  font-size: 0.9rem;
}

.botones-filtro {
  display: flex;
  gap: 0.5rem;
}

.botones-filtro button {
  flex: 1;
  padding: 0.75rem;
  border: 1px solid #dee2e6;
  background: white;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.botones-filtro button:hover {
  background: #f8f9fa;
}

.botones-filtro button.activo {
  background: #007bff;
  color: white;
  border-color: #007bff;
}
```

#### Paso 6: Componente Lista de Tareas

```bash
ng g c components/tarea-lista
```

```typescript
// src/app/components/tarea-lista/tarea-lista.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Tarea } from '../../models/tarea.interface';
import { TareaItemComponent } from '../tarea-item/tarea-item.component';

@Component({
  selector: 'app-tarea-lista',
  standalone: true,
  imports: [CommonModule, TareaItemComponent],
  templateUrl: './tarea-lista.component.html',
  styleUrls: ['./tarea-lista.component.css']
})
export class TareaListaComponent {
  @Input() tareas: Tarea[] = [];
  @Output() toggleCompletada = new EventEmitter<number>();
  @Output() eliminar = new EventEmitter<number>();

  onToggleCompletada(id: number): void {
    this.toggleCompletada.emit(id);
  }

  onEliminar(id: number): void {
    this.eliminar.emit(id);
  }
}
```

```html
<!-- src/app/components/tarea-lista/tarea-lista.component.html -->
<div class="tarea-lista">
  <div *ngIf="tareas.length === 0" class="sin-tareas">
    <div class="icono">📝</div>
    <p>No hay tareas para mostrar</p>
  </div>

  <app-tarea-item
    *ngFor="let tarea of tareas"
    [tarea]="tarea"
    (toggleCompletada)="onToggleCompletada($event)"
    (eliminar)="onEliminar($event)"
  ></app-tarea-item>
</div>
```

```css
/* src/app/components/tarea-lista/tarea-lista.component.css */
.tarea-lista {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  min-height: 200px;
}

.sin-tareas {
  text-align: center;
  padding: 3rem;
  color: #6c757d;
}

.sin-tareas .icono {
  font-size: 4rem;
  margin-bottom: 1rem;
  opacity: 0.5;
}

.sin-tareas p {
  font-size: 1.2rem;
}
```

#### Paso 7: Componente Padre (App)

```typescript
// src/app/app.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Tarea, FiltroTarea } from './models/tarea.interface';
import { TareaFormComponent } from './components/tarea-form/tarea-form.component';
import { TareaListaComponent } from './components/tarea-lista/tarea-lista.component';
import { TareaFiltrosComponent } from './components/tarea-filtros/tarea-filtros.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    CommonModule,
    TareaFormComponent,
    TareaListaComponent,
    TareaFiltrosComponent
  ],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  tareas: Tarea[] = [];
  filtroActual: FiltroTarea = 'todas';
  private nextId = 1;

  onTareaCreada(tareaNueva: Omit<Tarea, 'id' | 'fechaCreacion' | 'completada'>): void {
    const tarea: Tarea = {
      ...tareaNueva,
      id: this.nextId++,
      completada: false,
      fechaCreacion: new Date()
    };
    this.tareas.push(tarea);
  }

  onToggleCompletada(id: number): void {
    const tarea = this.tareas.find(t => t.id === id);
    if (tarea) {
      tarea.completada = !tarea.completada;
    }
  }

  onEliminar(id: number): void {
    this.tareas = this.tareas.filter(t => t.id !== id);
  }

  onCambiarFiltro(filtro: FiltroTarea): void {
    this.filtroActual = filtro;
  }

  get tareasFiltradas(): Tarea[] {
    switch (this.filtroActual) {
      case 'completadas':
        return this.tareas.filter(t => t.completada);
      case 'pendientes':
        return this.tareas.filter(t => !t.completada);
      default:
        return this.tareas;
    }
  }

  get tareasCompletadas(): number {
    return this.tareas.filter(t => t.completada).length;
  }

  get tareasPendientes(): number {
    return this.tareas.filter(t => !t.completada).length;
  }
}
```

```html
<!-- src/app/app.component.html -->
<div class="app-container">
  <header>
    <h1>✅ Gestor de Tareas</h1>
    <p>Organiza tu día con eficiencia</p>
  </header>

  <main>
    <app-tarea-form
      (tareaCreada)="onTareaCreada($event)"
    ></app-tarea-form>

    <app-tarea-filtros
      [filtroActual]="filtroActual"
      [totalTareas]="tareas.length"
      [tareasCompletadas]="tareasCompletadas"
      [tareasPendientes]="tareasPendientes"
      (cambiarFiltro)="onCambiarFiltro($event)"
    ></app-tarea-filtros>

    <app-tarea-lista
      [tareas]="tareasFiltradas"
      (toggleCompletada)="onToggleCompletada($event)"
      (eliminar)="onEliminar($event)"
    ></app-tarea-lista>
  </main>
</div>
```

```css
/* src/app/app.component.css */
.app-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  color: #333;
  margin-bottom: 0.5rem;
  font-size: 2.5rem;
}

header p {
  color: #6c757d;
  font-size: 1.1rem;
}
```

### ✅ Resultado Esperado

- Formulario para agregar tareas con título, descripción y prioridad
- Estadísticas mostrando total, pendientes y completadas
- Filtros para ver todas/pendientes/completadas
- Lista de tareas con checkbox, descripción y botón eliminar
- Tareas completadas con estilo tachado
- Confirmación antes de eliminar

### 🎓 Conceptos Aplicados
- ✅ Múltiples componentes comunicándose
- ✅ @Input() y @Output() en varios niveles
- ✅ FormsModule y [(ngModel)]
- ✅ Getters computados
- ✅ Filtrado de arrays
- ✅ Clases CSS dinámicas
- ✅ Confirmaciones de usuario

---

## Ejercicio 3: Sistema de Notificaciones con Servicios [INTERMEDIO]

### 📋 Enunciado

Crear un sistema de notificaciones toast donde:
- Servicio central de notificaciones
- Componente contenedor de notificaciones
- Componente individual para cada notificación
- Tipos: success, error, warning, info
- Auto-cerrar después de N segundos (configurable)
- Posición configurable (top-right, bottom-right, etc.)
- Máximo de notificaciones visibles

### 🎯 Objetivos de Aprendizaje
- Servicios compartidos entre componentes
- BehaviorSubject y Observables
- Lifecycle hooks (ngOnDestroy)
- setTimeout y clearTimeout
- Componentes dinámicos

### 💡 Solución Completa

#### Paso 1: Crear proyecto

```bash
ng new notificaciones-app --routing=false --style=css --standalone
cd notificaciones-app
```

#### Paso 2: Crear interfaces

```typescript
// src/app/models/notificacion.interface.ts
export type TipoNotificacion = 'success' | 'error' | 'warning' | 'info';

export interface Notificacion {
  id: number;
  tipo: TipoNotificacion;
  titulo: string;
  mensaje: string;
  duracion: number; // en milisegundos
  timestamp: Date;
}

export interface ConfigNotificacion {
  tipo: TipoNotificacion;
  titulo: string;
  mensaje: string;
  duracion?: number;
}
```

#### Paso 3: Servicio de Notificaciones

```typescript
// src/app/services/notificacion.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { Notificacion, ConfigNotificacion } from '../models/notificacion.interface';

@Injectable({
  providedIn: 'root'
})
export class NotificacionService {
  private notificaciones: Notificacion[] = [];
  private notificacionesSubject = new BehaviorSubject<Notificacion[]>([]);
  private nextId = 1;
  private maxNotificaciones = 5;
  private duracionPorDefecto = 5000; // 5 segundos

  notificaciones$: Observable<Notificacion[]> = this.notificacionesSubject.asObservable();

  mostrar(config: ConfigNotificacion): void {
    const notificacion: Notificacion = {
      id: this.nextId++,
      tipo: config.tipo,
      titulo: config.titulo,
      mensaje: config.mensaje,
      duracion: config.duracion || this.duracionPorDefecto,
      timestamp: new Date()
    };

    // Agregar notificación
    this.notificaciones.push(notificacion);

    // Limitar número de notificaciones
    if (this.notificaciones.length > this.maxNotificaciones) {
      this.notificaciones.shift(); // Remover la primera
    }

    this.notificacionesSubject.next([...this.notificaciones]);

    // Auto-cerrar después de la duración especificada
    setTimeout(() => {
      this.cerrar(notificacion.id);
    }, notificacion.duracion);
  }

  success(titulo: string, mensaje: string, duracion?: number): void {
    this.mostrar({ tipo: 'success', titulo, mensaje, duracion });
  }

  error(titulo: string, mensaje: string, duracion?: number): void {
    this.mostrar({ tipo: 'error', titulo, mensaje, duracion });
  }

  warning(titulo: string, mensaje: string, duracion?: number): void {
    this.mostrar({ tipo: 'warning', titulo, mensaje, duracion });
  }

  info(titulo: string, mensaje: string, duracion?: number): void {
    this.mostrar({ tipo: 'info', titulo, mensaje, duracion });
  }

  cerrar(id: number): void {
    this.notificaciones = this.notificaciones.filter(n => n.id !== id);
    this.notificacionesSubject.next([...this.notificaciones]);
  }

  cerrarTodas(): void {
    this.notificaciones = [];
    this.notificacionesSubject.next([]);
  }
}
```

#### Paso 4: Componente Notificación Individual

```bash
ng g c components/notificacion-item
```

```typescript
// src/app/components/notificacion-item/notificacion-item.component.ts
import { Component, Input, Output, EventEmitter, OnInit, OnDestroy } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Notificacion } from '../../models/notificacion.interface';

@Component({
  selector: 'app-notificacion-item',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './notificacion-item.component.html',
  styleUrls: ['./notificacion-item.component.css']
})
export class NotificacionItemComponent implements OnInit, OnDestroy {
  @Input() notificacion!: Notificacion;
  @Output() cerrar = new EventEmitter<number>();

  progreso = 100;
  private intervalo: any;

  ngOnInit(): void {
    this.iniciarProgreso();
  }

  ngOnDestroy(): void {
    if (this.intervalo) {
      clearInterval(this.intervalo);
    }
  }

  onCerrar(): void {
    this.cerrar.emit(this.notificacion.id);
  }

  getIcono(): string {
    switch (this.notificacion.tipo) {
      case 'success': return '✅';
      case 'error': return '❌';
      case 'warning': return '⚠️';
      case 'info': return 'ℹ️';
      default: return '';
    }
  }

  private iniciarProgreso(): void {
    const intervaloTiempo = 50; // Actualizar cada 50ms
    const decremento = (intervaloTiempo / this.notificacion.duracion) * 100;

    this.intervalo = setInterval(() => {
      this.progreso -= decremento;
      if (this.progreso <= 0) {
        clearInterval(this.intervalo);
      }
    }, intervaloTiempo);
  }
}
```

```html
<!-- src/app/components/notificacion-item/notificacion-item.component.html -->
<div class="notificacion" [class]="'notificacion--' + notificacion.tipo">
  <div class="notificacion-header">
    <span class="icono">{{ getIcono() }}</span>
    <h4 class="titulo">{{ notificacion.titulo }}</h4>
    <button class="btn-cerrar" (click)="onCerrar()">&times;</button>
  </div>
  
  <div class="notificacion-body">
    <p class="mensaje">{{ notificacion.mensaje }}</p>
  </div>
  
  <div class="progreso">
    <div class="progreso-barra" [style.width.%]="progreso"></div>
  </div>
</div>
```

```css
/* src/app/components/notificacion-item/notificacion-item.component.css */
.notificacion {
  width: 350px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  margin-bottom: 1rem;
  overflow: hidden;
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.notificacion-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  border-bottom: 1px solid #e9ecef;
}

.icono {
  font-size: 1.5rem;
}

.titulo {
  flex: 1;
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
}

.btn-cerrar {
  background: transparent;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: #6c757d;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  transition: background 0.2s;
}

.btn-cerrar:hover {
  background: #f8f9fa;
}

.notificacion-body {
  padding: 1rem;
}

.mensaje {
  margin: 0;
  color: #495057;
  font-size: 0.9rem;
  line-height: 1.5;
}

.progreso {
  height: 4px;
  background: #e9ecef;
  overflow: hidden;
}

.progreso-barra {
  height: 100%;
  transition: width 0.05s linear;
}

/* Variantes de color */
.notificacion--success .notificacion-header {
  background: #d4edda;
  border-bottom-color: #c3e6cb;
}

.notificacion--success .progreso-barra {
  background: #28a745;
}

.notificacion--error .notificacion-header {
  background: #f8d7da;
  border-bottom-color: #f5c6cb;
}

.notificacion--error .progreso-barra {
  background: #dc3545;
}

.notificacion--warning .notificacion-header {
  background: #fff3cd;
  border-bottom-color: #ffeaa7;
}

.notificacion--warning .progreso-barra {
  background: #ffc107;
}

.notificacion--info .notificacion-header {
  background: #d1ecf1;
  border-bottom-color: #bee5eb;
}

.notificacion--info .progreso-barra {
  background: #17a2b8;
}
```

#### Paso 5: Contenedor de Notificaciones

```bash
ng g c components/notificacion-contenedor
```

```typescript
// src/app/components/notificacion-contenedor/notificacion-contenedor.component.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Observable } from 'rxjs';
import { Notificacion } from '../../models/notificacion.interface';
import { NotificacionService } from '../../services/notificacion.service';
import { NotificacionItemComponent } from '../notificacion-item/notificacion-item.component';

@Component({
  selector: 'app-notificacion-contenedor',
  standalone: true,
  imports: [CommonModule, NotificacionItemComponent],
  templateUrl: './notificacion-contenedor.component.html',
  styleUrls: ['./notificacion-contenedor.component.css']
})
export class NotificacionContenedorComponent implements OnInit {
  notificaciones$!: Observable<Notificacion[]>;

  constructor(private notificacionService: NotificacionService) {}

  ngOnInit(): void {
    this.notificaciones$ = this.notificacionService.notificaciones$;
  }

  onCerrar(id: number): void {
    this.notificacionService.cerrar(id);
  }
}
```

```html
<!-- src/app/components/notificacion-contenedor/notificacion-contenedor.component.html -->
<div class="notificaciones-contenedor">
  <app-notificacion-item
    *ngFor="let notificacion of notificaciones$ | async"
    [notificacion]="notificacion"
    (cerrar)="onCerrar($event)"
  ></app-notificacion-item>
</div>
```

```css
/* src/app/components/notificacion-contenedor/notificacion-contenedor.component.css */
.notificaciones-contenedor {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 9999;
  pointer-events: none;
}

.notificaciones-contenedor > * {
  pointer-events: all;
}
```

#### Paso 6: Componente de Prueba (App)

```typescript
// src/app/app.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { NotificacionContenedorComponent } from './components/notificacion-contenedor/notificacion-contenedor.component';
import { NotificacionService } from './services/notificacion.service';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, NotificacionContenedorComponent],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  constructor(private notificacionService: NotificacionService) {}

  mostrarSuccess(): void {
    this.notificacionService.success(
      '¡Éxito!',
      'La operación se completó correctamente'
    );
  }

  mostrarError(): void {
    this.notificacionService.error(
      'Error',
      'Ocurrió un error al procesar la solicitud'
    );
  }

  mostrarWarning(): void {
    this.notificacionService.warning(
      'Advertencia',
      'Esta acción requiere confirmación'
    );
  }

  mostrarInfo(): void {
    this.notificacionService.info(
      'Información',
      'Hay una nueva actualización disponible'
    );
  }

  mostrarPersonalizada(): void {
    this.notificacionService.mostrar({
      tipo: 'info',
      titulo: 'Notificación Personalizada',
      mensaje: 'Esta notificación dura 10 segundos',
      duracion: 10000
    });
  }

  cerrarTodas(): void {
    this.notificacionService.cerrarTodas();
  }
}
```

```html
<!-- src/app/app.component.html -->
<app-notificacion-contenedor></app-notificacion-contenedor>

<div class="app-container">
  <header>
    <h1>🔔 Sistema de Notificaciones</h1>
    <p>Prueba los diferentes tipos de notificaciones</p>
  </header>

  <main>
    <div class="botones">
      <button class="btn btn-success" (click)="mostrarSuccess()">
        ✅ Success
      </button>
      
      <button class="btn btn-error" (click)="mostrarError()">
        ❌ Error
      </button>
      
      <button class="btn btn-warning" (click)="mostrarWarning()">
        ⚠️ Warning
      </button>
      
      <button class="btn btn-info" (click)="mostrarInfo()">
        ℹ️ Info
      </button>
    </div>

    <div class="acciones">
      <button class="btn btn-secondary" (click)="mostrarPersonalizada()">
        ⏱️ Personalizada (10s)
      </button>
      
      <button class="btn btn-secondary" (click)="cerrarTodas()">
        🗑️ Cerrar Todas
      </button>
    </div>

    <div class="instrucciones">
      <h3>📝 Características:</h3>
      <ul>
        <li>✅ Máximo 5 notificaciones visibles</li>
        <li>✅ Auto-cierre después de 5 segundos (por defecto)</li>
        <li>✅ Barra de progreso animada</li>
        <li>✅ Botón para cerrar manualmente</li>
        <li>✅ 4 tipos: Success, Error, Warning, Info</li>
        <li>✅ Duración personalizable</li>
        <li>✅ Animación de entrada suave</li>
      </ul>
    </div>
  </main>
</div>
```

```css
/* src/app/app.component.css */
.app-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

header {
  text-align: center;
  margin-bottom: 3rem;
}

h1 {
  color: #333;
  margin-bottom: 0.5rem;
}

header p {
  color: #6c757d;
}

.botones {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.acciones {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 3rem;
}

.btn {
  padding: 1rem 2rem;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  color: white;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.btn-success {
  background: #28a745;
}

.btn-error {
  background: #dc3545;
}

.btn-warning {
  background: #ffc107;
  color: #333;
}

.btn-info {
  background: #17a2b8;
}

.btn-secondary {
  background: #6c757d;
}

.instrucciones {
  background: #f8f9fa;
  padding: 2rem;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.instrucciones h3 {
  color: #333;
  margin-bottom: 1rem;
}

.instrucciones ul {
  list-style: none;
  padding: 0;
}

.instrucciones li {
  padding: 0.5rem 0;
  color: #495057;
}
```

### ✅ Resultado Esperado

- Notificaciones aparecen en la esquina superior derecha
- Máximo 5 notificaciones visibles simultáneamente
- Barra de progreso que disminuye conforme pasa el tiempo
- Auto-cierre después de 5 segundos
- Botón para cerrar manualmente
- Animación suave de entrada
- 4 estilos diferentes según el tipo

### 🎓 Conceptos Aplicados
- ✅ Servicio singleton compartido
- ✅ BehaviorSubject y Observables
- ✅ setTimeout y clearInterval
- ✅ ngOnDestroy para limpieza
- ✅ Animaciones CSS
- ✅ Position fixed
- ✅ Event emitters entre componentes

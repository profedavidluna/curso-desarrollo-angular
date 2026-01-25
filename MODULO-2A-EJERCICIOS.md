Ejercicio 4: Dashboard con Múltiples Fuentes de Datos (RxJS) [AVANZADO]
📋 Enunciado
Crear un dashboard que carga datos de múltiples fuentes (APIs simuladas) usando RxJS:

Cargar usuarios, productos y ventas simultáneamente
Mostrar loading global mientras carga
Manejar errores individuales sin romper el dashboard
Botón de refresh que recarga todo
Cache de 30 segundos
Estadísticas calculadas de múltiples fuentes
🎯 Objetivos de Aprendizaje
forkJoin para cargar múltiples observables
catchError para manejo de errores
shareReplay para cache
combineLatest para combinar streams
tap para efectos secundarios
finalize para limpiar loading
💡 Solución Completa
Paso 1: Crear proyecto
bash
ng new dashboard-app --routing=false --style=css --standalone
cd dashboard-app
Paso 2: Crear interfaces y modelos
TypeScript
// src/app/models/dashboard.interface.ts
export interface Usuario {
  id: number;
  nombre: string;
  email: string;
  activo: boolean;
}

export interface Producto {
  id: number;
  nombre: string;
  precio: number;
  categoria: string;
  stock: number;
}

export interface Venta {
  id: number;
  productoId: number;
  usuarioId: number;
  cantidad: number;
  total: number;
  fecha: Date;
}

export interface DashboardData {
  usuarios: Usuario[];
  productos: Producto[];
  ventas: Venta[];
}

export interface Estadisticas {
  totalUsuarios: number;
  usuariosActivos: number;
  totalProductos: number;
  productosStockBajo: number;
  totalVentas: number;
  ingresoTotal: number;
  ventaPromedio: number;
}

export interface CargaEstado {
  usuarios: 'idle' | 'loading' | 'success' | 'error';
  productos: 'idle' | 'loading' | 'success' | 'error';
  ventas: 'idle' | 'loading' | 'success' | 'error';
}
Paso 3: Servicio de API Simulada
TypeScript
// src/app/services/api.service.ts
import { Injectable } from '@angular/core';
import { Observable, of, throwError } from 'rxjs';
import { delay } from 'rxjs/operators';
import { Usuario, Producto, Venta } from '../models/dashboard.interface';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  
  // Simular errores aleatoriamente (10% de probabilidad)
  private simularError(): boolean {
    return Math.random() < 0.1;
  }

  getUsuarios(): Observable<Usuario[]> {
    const usuarios: Usuario[] = [
      { id: 1, nombre: 'Juan Pérez', email: 'juan@example.com', activo: true },
      { id: 2, nombre: 'María García', email: 'maria@example.com', activo: true },
      { id: 3, nombre: 'Carlos López', email: 'carlos@example.com', activo: false },
      { id: 4, nombre: 'Ana Martínez', email: 'ana@example.com', activo: true },
      { id: 5, nombre: 'Luis Rodríguez', email: 'luis@example.com', activo: true },
      { id: 6, nombre: 'Elena Sánchez', email: 'elena@example.com', activo: false },
      { id: 7, nombre: 'Pedro Gómez', email: 'pedro@example.com', activo: true },
      { id: 8, nombre: 'Sofía Torres', email: 'sofia@example.com', activo: true },
    ];

    if (this.simularError()) {
      return throwError(() => new Error('Error al cargar usuarios')).pipe(
        delay(1000)
      );
    }

    return of(usuarios).pipe(delay(1500));
  }

  getProductos(): Observable<Producto[]> {
    const productos: Producto[] = [
      { id: 1, nombre: 'Laptop HP', precio: 899.99, categoria: 'Electrónica', stock: 15 },
      { id: 2, nombre: 'Mouse Logitech', precio: 29.99, categoria: 'Accesorios', stock: 50 },
      { id: 3, nombre: 'Teclado Mecánico', precio: 79.99, categoria: 'Accesorios', stock: 3 },
      { id: 4, nombre: 'Monitor Samsung 27"', precio: 299.99, categoria: 'Electrónica', stock: 8 },
      { id: 5, nombre: 'Webcam HD', precio: 59.99, categoria: 'Accesorios', stock: 0 },
      { id: 6, nombre: 'Auriculares Sony', precio: 149.99, categoria: 'Audio', stock: 25 },
      { id: 7, nombre: 'SSD 1TB', precio: 119.99, categoria: 'Almacenamiento', stock: 2 },
      { id: 8, nombre: 'Router WiFi', precio: 89.99, categoria: 'Redes', stock: 12 },
      { id: 9, nombre: 'Impresora Canon', precio: 199.99, categoria: 'Oficina', stock: 6 },
      { id: 10, nombre: 'Tablet iPad', precio: 499.99, categoria: 'Electrónica', stock: 10 },
    ];

    if (this.simularError()) {
      return throwError(() => new Error('Error al cargar productos')).pipe(
        delay(1000)
      );
    }

    return of(productos).pipe(delay(2000));
  }

  getVentas(): Observable<Venta[]> {
    const ventas: Venta[] = [
      { id: 1, productoId: 1, usuarioId: 1, cantidad: 2, total: 1799.98, fecha: new Date('2024-01-15') },
      { id: 2, productoId: 2, usuarioId: 2, cantidad: 5, total: 149.95, fecha: new Date('2024-01-16') },
      { id: 3, productoId: 3, usuarioId: 3, cantidad: 1, total: 79.99, fecha: new Date('2024-01-16') },
      { id: 4, productoId: 4, usuarioId: 1, cantidad: 1, total: 299.99, fecha: new Date('2024-01-17') },
      { id: 5, productoId: 6, usuarioId: 4, cantidad: 3, total: 449.97, fecha: new Date('2024-01-17') },
      { id: 6, productoId: 7, usuarioId: 5, cantidad: 2, total: 239.98, fecha: new Date('2024-01-18') },
      { id: 7, productoId: 8, usuarioId: 6, cantidad: 1, total: 89.99, fecha: new Date('2024-01-18') },
      { id: 8, productoId: 10, usuarioId: 7, cantidad: 1, total: 499.99, fecha: new Date('2024-01-19') },
      { id: 9, productoId: 2, usuarioId: 8, cantidad: 10, total: 299.90, fecha: new Date('2024-01-19') },
      { id: 10, productoId: 9, usuarioId: 2, cantidad: 1, total: 199.99, fecha: new Date('2024-01-20') },
      { id: 11, productoId: 1, usuarioId: 3, cantidad: 1, total: 899.99, fecha: new Date('2024-01-20') },
      { id: 12, productoId: 4, usuarioId: 4, cantidad: 2, total: 599.98, fecha: new Date('2024-01-21') },
    ];

    if (this.simularError()) {
      return throwError(() => new Error('Error al cargar ventas')).pipe(
        delay(1000)
      );
    }

    return of(ventas).pipe(delay(1000));
  }
}
Paso 4: Servicio de Dashboard
TypeScript
// src/app/services/dashboard.service.ts
import { Injectable } from '@angular/core';
import { Observable, forkJoin, BehaviorSubject, of } from 'rxjs';
import { map, catchError, tap, shareReplay } from 'rxjs/operators';
import { ApiService } from './api.service';
import { 
  DashboardData, 
  Estadisticas,
  Usuario,
  Producto,
  Venta 
} from '../models/dashboard.interface';

interface CachedData {
  data: DashboardData;
  timestamp: number;
}

@Injectable({
  providedIn: 'root'
})
export class DashboardService {
  private cache: CachedData | null = null;
  private readonly CACHE_DURATION = 30000; // 30 segundos

  private loadingSubject = new BehaviorSubject<boolean>(false);
  loading$ = this.loadingSubject.asObservable();

  private erroresSubject = new BehaviorSubject<string[]>([]);
  errores$ = this.erroresSubject.asObservable();

  constructor(private apiService: ApiService) {}

  getDashboardData(forzarRecarga: boolean = false): Observable<DashboardData> {
    // Verificar cache
    if (!forzarRecarga && this.cache && this.isCacheValid()) {
      console.log('Retornando datos desde cache');
      return of(this.cache.data);
    }

    console.log('Cargando datos desde API');
    this.loadingSubject.next(true);
    this.erroresSubject.next([]);

    return forkJoin({
      usuarios: this.apiService.getUsuarios().pipe(
        catchError(error => {
          this.agregarError('usuarios', error.message);
          return of([]);
        })
      ),
      productos: this.apiService.getProductos().pipe(
        catchError(error => {
          this.agregarError('productos', error.message);
          return of([]);
        })
      ),
      ventas: this.apiService.getVentas().pipe(
        catchError(error => {
          this.agregarError('ventas', error.message);
          return of([]);
        })
      )
    }).pipe(
      tap(data => {
        // Guardar en cache
        this.cache = {
          data,
          timestamp: Date.now()
        };
        this.loadingSubject.next(false);
      }),
      shareReplay(1)
    );
  }

  calcularEstadisticas(data: DashboardData): Estadisticas {
    const totalUsuarios = data.usuarios.length;
    const usuariosActivos = data.usuarios.filter(u => u.activo).length;
    
    const totalProductos = data.productos.length;
    const productosStockBajo = data.productos.filter(p => p.stock < 5).length;
    
    const totalVentas = data.ventas.length;
    const ingresoTotal = data.ventas.reduce((sum, v) => sum + v.total, 0);
    const ventaPromedio = totalVentas > 0 ? ingresoTotal / totalVentas : 0;

    return {
      totalUsuarios,
      usuariosActivos,
      totalProductos,
      productosStockBajo,
      totalVentas,
      ingresoTotal,
      ventaPromedio
    };
  }

  private isCacheValid(): boolean {
    if (!this.cache) return false;
    const now = Date.now();
    return (now - this.cache.timestamp) < this.CACHE_DURATION;
  }

  private agregarError(fuente: string, mensaje: string): void {
    const erroresActuales = this.erroresSubject.value;
    this.erroresSubject.next([...erroresActuales, `Error en ${fuente}: ${mensaje}`]);
  }

  limpiarCache(): void {
    this.cache = null;
  }
}
Paso 5: Componente Card de Estadística
bash
ng g c components/stat-card
TypeScript
// src/app/components/stat-card/stat-card.component.ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-stat-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './stat-card.component.html',
  styleUrls: ['./stat-card.component.css']
})
export class StatCardComponent {
  @Input() titulo: string = '';
  @Input() valor: number | string = 0;
  @Input() icono: string = '📊';
  @Input() color: string = 'blue';
  @Input() prefijo: string = '';
  @Input() sufijo: string = '';
  @Input() cambio?: number; // Porcentaje de cambio
  @Input() tendencia?: 'up' | 'down';
}
HTML
<!-- src/app/components/stat-card/stat-card.component.html -->
<div class="stat-card" [class]="'stat-card--' + color">
  <div class="stat-card__icon">{{ icono }}</div>
  
  <div class="stat-card__content">
    <div class="stat-card__titulo">{{ titulo }}</div>
    <div class="stat-card__valor">
      <span class="prefijo">{{ prefijo }}</span>
      {{ valor }}
      <span class="sufijo">{{ sufijo }}</span>
    </div>
    
    <div class="stat-card__cambio" *ngIf="cambio !== undefined">
      <span [class]="'tendencia tendencia--' + tendencia">
        <span *ngIf="tendencia === 'up'">↗</span>
        <span *ngIf="tendencia === 'down'">↘</span>
        {{ cambio }}%
      </span>
      <span class="texto-cambio">vs mes anterior</span>
    </div>
  </div>
</div>
CSS
/* src/app/components/stat-card/stat-card.component.css */
.stat-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  gap: 1.5rem;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}

.stat-card__icon {
  font-size: 3rem;
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 12px;
  background: rgba(0,0,0,0.05);
}

.stat-card--blue .stat-card__icon {
  background: rgba(0, 123, 255, 0.1);
}

.stat-card--green .stat-card__icon {
  background: rgba(40, 167, 69, 0.1);
}

.stat-card--orange .stat-card__icon {
  background: rgba(255, 193, 7, 0.1);
}

.stat-card--red .stat-card__icon {
  background: rgba(220, 53, 69, 0.1);
}

.stat-card__content {
  flex: 1;
}

.stat-card__titulo {
  color: #6c757d;
  font-size: 0.875rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.5rem;
}

.stat-card__valor {
  font-size: 2rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 0.25rem;
}

.prefijo, .sufijo {
  font-size: 1.5rem;
  color: #6c757d;
}

.stat-card__cambio {
  font-size: 0.875rem;
  color: #6c757d;
}

.tendencia {
  font-weight: 600;
  margin-right: 0.5rem;
}

.tendencia--up {
  color: #28a745;
}

.tendencia--down {
  color: #dc3545;
}

.texto-cambio {
  color: #adb5bd;
}
Paso 6: Componente Tabla de Datos
bash
ng g c components/data-table
TypeScript
// src/app/components/data-table/data-table.component.ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-data-table',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './data-table.component.html',
  styleUrls: ['./data-table.component.css']
})
export class DataTableComponent {
  @Input() titulo: string = '';
  @Input() datos: any[] = [];
  @Input() columnas: string[] = [];
  @Input() maxFilas: number = 5;

  get datosLimitados() {
    return this.datos.slice(0, this.maxFilas);
  }
}
HTML
<!-- src/app/components/data-table/data-table.component.html -->
<div class="data-table">
  <h3 class="data-table__titulo">{{ titulo }}</h3>
  
  <div class="data-table__wrapper">
    <table class="table">
      <thead>
        <tr>
          <th *ngFor="let col of columnas">{{ col }}</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let item of datosLimitados">
          <td *ngFor="let col of columnas">
            {{ item[col.toLowerCase()] }}
          </td>
        </tr>
        <tr *ngIf="datos.length === 0">
          <td [attr.colspan]="columnas.length" class="sin-datos">
            No hay datos disponibles
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  
  <div class="data-table__footer" *ngIf="datos.length > maxFilas">
    <small>Mostrando {{ maxFilas }} de {{ datos.length }} registros</small>
  </div>
</div>
CSS
/* src/app/components/data-table/data-table.component.css */
.data-table {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

.data-table__titulo {
  padding: 1.5rem;
  margin: 0;
  background: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
  color: #333;
  font-size: 1.25rem;
}

.data-table__wrapper {
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th {
  background: #f8f9fa;
  padding: 1rem;
  text-align: left;
  font-weight: 600;
  color: #495057;
  border-bottom: 2px solid #dee2e6;
  white-space: nowrap;
}

.table td {
  padding: 1rem;
  border-bottom: 1px solid #dee2e6;
  color: #495057;
}

.table tbody tr:hover {
  background: #f8f9fa;
}

.sin-datos {
  text-align: center;
  color: #6c757d;
  font-style: italic;
  padding: 2rem !important;
}

.data-table__footer {
  padding: 1rem 1.5rem;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
  text-align: center;
  color: #6c757d;
}
Paso 7: Componente Principal (Dashboard)
TypeScript
// src/app/app.component.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Observable } from 'rxjs';
import { DashboardService } from './services/dashboard.service';
import { DashboardData, Estadisticas } from './models/dashboard.interface';
import { StatCardComponent } from './components/stat-card/stat-card.component';
import { DataTableComponent } from './components/data-table/data-table.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, StatCardComponent, DataTableComponent],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  dashboardData: DashboardData | null = null;
  estadisticas: Estadisticas | null = null;
  loading$: Observable<boolean>;
  errores$: Observable<string[]>;

  constructor(private dashboardService: DashboardService) {
    this.loading$ = this.dashboardService.loading$;
    this.errores$ = this.dashboardService.errores$;
  }

  ngOnInit(): void {
    this.cargarDatos();
  }

  cargarDatos(forzarRecarga: boolean = false): void {
    this.dashboardService.getDashboardData(forzarRecarga).subscribe({
      next: (data) => {
        this.dashboardData = data;
        this.estadisticas = this.dashboardService.calcularEstadisticas(data);
      },
      error: (error) => {
        console.error('Error al cargar dashboard:', error);
      }
    });
  }

  onRefresh(): void {
    this.dashboardService.limpiarCache();
    this.cargarDatos(true);
  }
}
HTML
<!-- src/app/app.component.html -->
<div class="dashboard">
  <header class="dashboard__header">
    <div class="header-content">
      <div class="header-info">
        <h1>📊 Dashboard Analytics</h1>
        <p>Panel de control con múltiples fuentes de datos</p>
      </div>
      <button 
        class="btn-refresh" 
        (click)="onRefresh()"
        [disabled]="loading$ | async"
      >
        <span class="icono">🔄</span>
        Actualizar
      </button>
    </div>
  </header>

  <!-- Loading Global -->
  <div class="loading-overlay" *ngIf="loading$ | async">
    <div class="spinner"></div>
    <p>Cargando datos del dashboard...</p>
  </div>

  <!-- Errores -->
  <div class="errores" *ngIf="(errores$ | async) as errores">
    <div class="error-card" *ngFor="let error of errores">
      ⚠️ {{ error }}
    </div>
  </div>

  <!-- Estadísticas -->
  <div class="stats-grid" *ngIf="estadisticas">
    <app-stat-card
      titulo="Total Usuarios"
      [valor]="estadisticas.totalUsuarios"
      icono="👥"
      color="blue"
      [cambio]="12.5"
      tendencia="up"
    ></app-stat-card>

    <app-stat-card
      titulo="Usuarios Activos"
      [valor]="estadisticas.usuariosActivos"
      icono="✅"
      color="green"
      [cambio]="8.3"
      tendencia="up"
    ></app-stat-card>

    <app-stat-card
      titulo="Total Productos"
      [valor]="estadisticas.totalProductos"
      icono="📦"
      color="orange"
    ></app-stat-card>

    <app-stat-card
      titulo="Stock Bajo"
      [valor]="estadisticas.productosStockBajo"
      icono="⚠️"
      color="red"
    ></app-stat-card>

    <app-stat-card
      titulo="Total Ventas"
      [valor]="estadisticas.totalVentas"
      icono="💰"
      color="green"
    ></app-stat-card>

    <app-stat-card
      titulo="Ingreso Total"
      [valor]="estadisticas.ingresoTotal | number:'1.2-2'"
      icono="💵"
      color="blue"
      prefijo="$"
      [cambio]="15.7"
      tendencia="up"
    ></app-stat-card>

    <app-stat-card
      titulo="Venta Promedio"
      [valor]="estadisticas.ventaPromedio | number:'1.2-2'"
      icono="📈"
      color="orange"
      prefijo="$"
    ></app-stat-card>
  </div>

  <!-- Tablas -->
  <div class="tables-grid" *ngIf="dashboardData">
    <app-data-table
      titulo="Usuarios Recientes"
      [datos]="dashboardData.usuarios"
      [columnas]="['Nombre', 'Email', 'Activo']"
      [maxFilas]="5"
    ></app-data-table>

    <app-data-table
      titulo="Productos con Stock Bajo"
      [datos]="getProductosStockBajo()"
      [columnas]="['Nombre', 'Stock', 'Precio']"
      [maxFilas]="5"
    ></app-data-table>

    <app-data-table
      titulo="Últimas Ventas"
      [datos]="dashboardData.ventas"
      [columnas]="['Id', 'Total', 'Cantidad']"
      [maxFilas]="5"
    ></app-data-table>
  </div>
</div>
TypeScript
// Agregar este método a app.component.ts
getProductosStockBajo() {
  if (!this.dashboardData) return [];
  return this.dashboardData.productos
    .filter(p => p.stock < 5)
    .sort((a, b) => a.stock - b.stock);
}
CSS
/* src/app/app.component.css */
.dashboard {
  min-height: 100vh;
  background: #f5f7fa;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.dashboard__header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.header-content {
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-info h1 {
  margin: 0 0 0.5rem 0;
  font-size: 2rem;
}

.header-info p {
  margin: 0;
  opacity: 0.9;
}

.btn-refresh {
  background: rgba(255,255,255,0.2);
  color: white;
  border: 2px solid white;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.btn-refresh:hover:not(:disabled) {
  background: white;
  color: #667eea;
}

.btn-refresh:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-refresh .icono {
  font-size: 1.2rem;
}

.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.7);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  color: white;
}

.spinner {
  width: 60px;
  height: 60px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 1rem;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.errores {
  max-width: 1400px;
  margin: 2rem auto;
  padding: 0 2rem;
}

.error-card {
  background: #f8d7da;
  border: 1px solid #f5c6cb;
  color: #721c24;
  padding: 1rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.stats-grid {
  max-width: 1400px;
  margin: 2rem auto;
  padding: 0 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}

.tables-grid {
  max-width: 1400px;
  margin: 2rem auto;
  padding: 0 2rem 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 1.5rem;
}
✅ Resultado Esperado
Dashboard que carga 3 fuentes de datos simultáneamente
Loading global mientras carga
Si una fuente falla, muestra error pero no rompe el dashboard
Cache de 30 segundos (segunda carga es instantánea)
Botón refresh que fuerza recarga
7 tarjetas de estadísticas con colores
3 tablas con datos recientes
Diseño responsive y profesional
🎓 Conceptos Aplicados
✅ forkJoin para cargar múltiples observables
✅ catchError para manejo de errores individual
✅ shareReplay para cache
✅ tap para efectos secundarios
✅ BehaviorSubject para estado
✅ Pipes (number, date)
✅ Grid CSS responsive

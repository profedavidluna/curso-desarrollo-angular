# Consumo de APIs REST y Proyecto Integrador en Angular

## 1. Introducción al Consumo de APIs REST

Una API REST (Interfaz de Programación de Aplicaciones basada en Transferencia de Estado Representacional) permite que el frontend se comunique con un servidor para obtener, crear, actualizar o eliminar datos. En Angular, esta comunicación se realiza principalmente a través del módulo `HttpClient`.

### 1.1. ¿Qué es una API REST?

Una API REST expone recursos a través de URLs (endpoints) y usa los métodos HTTP para definir las operaciones:

| Método HTTP | Operación | Ejemplo de uso |
|-------------|-----------|----------------|
| `GET`       | Obtener datos | Listar personajes |
| `POST`      | Crear un recurso | Registrar un usuario |
| `PUT`       | Reemplazar un recurso completo | Actualizar perfil completo |
| `PATCH`     | Actualizar parte de un recurso | Cambiar solo el nombre |
| `DELETE`    | Eliminar un recurso | Borrar una tarea |

### 1.2. Configuración de HttpClient

En Angular 19 con componentes standalone, se provee `HttpClient` en la configuración de la aplicación:

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient()
  ]
};
```

---

## 2. Uso de HttpClient

### 2.1. Solicitud GET

```typescript
// personajes.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export interface Personaje {
  id: number;
  name: string;
  status: string;
  species: string;
  image: string;
}

export interface RespuestaApi {
  info: {
    count: number;
    pages: number;
    next: string | null;
    prev: string | null;
  };
  results: Personaje[];
}

@Injectable({ providedIn: 'root' })
export class PersonajesService {
  private readonly urlBase = 'https://rickandmortyapi.com/api';

  constructor(private http: HttpClient) {}

  obtenerPersonajes(pagina: number = 1): Observable<RespuestaApi> {
    return this.http.get<RespuestaApi>(`${this.urlBase}/character?page=${pagina}`);
  }

  obtenerPersonajePorId(id: number): Observable<Personaje> {
    return this.http.get<Personaje>(`${this.urlBase}/character/${id}`);
  }
}
```

### 2.2. Solicitud POST

```typescript
// tareas.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export interface Tarea {
  id?: number;
  titulo: string;
  completada: boolean;
}

@Injectable({ providedIn: 'root' })
export class TareasService {
  private readonly urlBase = 'https://api.ejemplo.com/tareas';

  constructor(private http: HttpClient) {}

  crearTarea(tarea: Tarea): Observable<Tarea> {
    return this.http.post<Tarea>(this.urlBase, tarea);
  }

  actualizarTarea(id: number, tarea: Partial<Tarea>): Observable<Tarea> {
    return this.http.patch<Tarea>(`${this.urlBase}/${id}`, tarea);
  }

  eliminarTarea(id: number): Observable<void> {
    return this.http.delete<void>(`${this.urlBase}/${id}`);
  }
}
```

---

## 3. Manejo de Respuestas con RxJS

RxJS proporciona operadores para transformar y gestionar los datos recibidos de la API.

### 3.1. Operador map

Transforma los datos de la respuesta antes de entregarlos al componente:

```typescript
import { map } from 'rxjs/operators';

obtenerNombresPersonajes(): Observable<string[]> {
  return this.http.get<RespuestaApi>(`${this.urlBase}/character`).pipe(
    map(respuesta => respuesta.results.map(p => p.name))
  );
}
```

### 3.2. Operadores catchError y throwError

Permiten capturar y manejar errores de la API de forma controlada:

```typescript
import { catchError, throwError } from 'rxjs';
import { HttpErrorResponse } from '@angular/common/http';

obtenerPersonajes(): Observable<RespuestaApi> {
  return this.http.get<RespuestaApi>(`${this.urlBase}/character`).pipe(
    catchError((error: HttpErrorResponse) => {
      let mensaje = 'Ocurrió un error inesperado.';
      if (error.status === 404) {
        mensaje = 'Recurso no encontrado.';
      } else if (error.status === 500) {
        mensaje = 'Error interno del servidor.';
      }
      console.error(mensaje, error);
      return throwError(() => new Error(mensaje));
    })
  );
}
```

### 3.3. Operador switchMap

Cancela la solicitud anterior cuando llega una nueva (ideal para búsquedas en tiempo real):

```typescript
import { Subject } from 'rxjs';
import { debounceTime, distinctUntilChanged, switchMap } from 'rxjs/operators';

@Component({ /* ... */ })
export class BusquedaComponent implements OnInit {
  private terminoBusqueda$ = new Subject<string>();
  resultados: Personaje[] = [];

  constructor(private servicio: PersonajesService) {}

  ngOnInit(): void {
    this.terminoBusqueda$.pipe(
      debounceTime(400),
      distinctUntilChanged(),
      switchMap(termino => this.servicio.buscarPersonaje(termino))
    ).subscribe(resultados => {
      this.resultados = resultados;
    });
  }

  onBusqueda(termino: string): void {
    this.terminoBusqueda$.next(termino);
  }
}
```

---

## 4. Encabezados HTTP y Autenticación

### 4.1. Enviar encabezados personalizados

```typescript
import { HttpHeaders } from '@angular/common/http';

obtenerDatosProtegidos(): Observable<any> {
  const encabezados = new HttpHeaders({
    'Authorization': `Bearer ${this.authService.obtenerToken()}`,
    'Content-Type': 'application/json'
  });
  return this.http.get<any>(`${this.urlBase}/datos-protegidos`, { headers: encabezados });
}
```

### 4.2. Interceptores HTTP

Los interceptores permiten modificar automáticamente todas las solicitudes o respuestas HTTP (por ejemplo, para agregar el token de autenticación a cada petición):

```typescript
// interceptors/auth.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authService = inject(AuthService);
  const token = authService.obtenerToken();

  if (token) {
    const reqAutenticada = req.clone({
      headers: req.headers.set('Authorization', `Bearer ${token}`)
    });
    return next(reqAutenticada);
  }

  return next(req);
};
```

Registro del interceptor en la configuración:

```typescript
// app.config.ts
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { authInterceptor } from './interceptors/auth.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(withInterceptors([authInterceptor]))
  ]
};
```

---

## 5. Parámetros de Consulta en Solicitudes HTTP

```typescript
import { HttpParams } from '@angular/common/http';

buscarPersonajes(nombre: string, estado: string, pagina: number): Observable<RespuestaApi> {
  const parametros = new HttpParams()
    .set('name', nombre)
    .set('status', estado)
    .set('page', pagina.toString());

  return this.http.get<RespuestaApi>(`${this.urlBase}/character`, { params: parametros });
}
```

---

## 6. Variables de Entorno

Angular permite definir configuraciones por entorno (desarrollo, producción):

```typescript
// src/environments/environment.ts (desarrollo)
export const environment = {
  production: false,
  urlApi: 'https://rickandmortyapi.com/api'
};

// src/environments/environment.prod.ts (producción)
export const environment = {
  production: true,
  urlApi: 'https://rickandmortyapi.com/api'
};
```

Uso en un servicio:

```typescript
import { environment } from '../../environments/environment';

@Injectable({ providedIn: 'root' })
export class PersonajesService {
  private readonly urlBase = environment.urlApi;
  // ...
}
```

---

## 7. Manejo de Estado de Carga y Errores en el Componente

Es buena práctica informar al usuario mientras se cargan los datos o cuando ocurre un error:

```typescript
// personajes.component.ts
import { Component, OnInit } from '@angular/core';
import { AsyncPipe, NgIf, NgFor } from '@angular/common';
import { PersonajesService, Personaje } from './personajes.service';

@Component({
  selector: 'app-personajes',
  standalone: true,
  imports: [NgIf, NgFor, AsyncPipe],
  template: `
    <div *ngIf="cargando">Cargando personajes...</div>
    <div *ngIf="error" class="error">{{ error }}</div>

    <div *ngIf="!cargando && !error">
      <div *ngFor="let personaje of personajes">
        <img [src]="personaje.image" [alt]="personaje.name" />
        <h3>{{ personaje.name }}</h3>
        <p>Estado: {{ personaje.status }} | Especie: {{ personaje.species }}</p>
      </div>
    </div>
  `
})
export class PersonajesComponent implements OnInit {
  personajes: Personaje[] = [];
  cargando = false;
  error: string | null = null;

  constructor(private servicio: PersonajesService) {}

  ngOnInit(): void {
    this.cargando = true;
    this.servicio.obtenerPersonajes().subscribe({
      next: (respuesta) => {
        this.personajes = respuesta.results;
        this.cargando = false;
      },
      error: (err) => {
        this.error = err.message;
        this.cargando = false;
      }
    });
  }
}
```

---

## 8. Paginación

La API de Rick and Morty devuelve los datos paginados. Así se implementa la paginación en Angular:

```typescript
// personajes.component.ts
@Component({ /* ... */ })
export class PersonajesComponent implements OnInit {
  personajes: Personaje[] = [];
  paginaActual = 1;
  totalPaginas = 1;
  cargando = false;

  constructor(private servicio: PersonajesService) {}

  ngOnInit(): void {
    this.cargarPersonajes();
  }

  cargarPersonajes(): void {
    this.cargando = true;
    this.servicio.obtenerPersonajes(this.paginaActual).subscribe({
      next: (respuesta) => {
        this.personajes = respuesta.results;
        this.totalPaginas = respuesta.info.pages;
        this.cargando = false;
      },
      error: () => {
        this.cargando = false;
      }
    });
  }

  paginaAnterior(): void {
    if (this.paginaActual > 1) {
      this.paginaActual--;
      this.cargarPersonajes();
    }
  }

  paginaSiguiente(): void {
    if (this.paginaActual < this.totalPaginas) {
      this.paginaActual++;
      this.cargarPersonajes();
    }
  }
}
```

---

## 9. Proyecto Integrador: Portal de Gestión de Personajes

Este proyecto consolida los aprendizajes de todos los módulos anteriores en una aplicación Angular completa que consume la API de Rick and Morty.

### 9.1. Descripción del Proyecto

Desarrollar un **Portal de Gestión de Personajes** de Rick and Morty que incluya:

- Lista de personajes con paginación y búsqueda en tiempo real
- Vista de detalle de cada personaje
- Sistema de favoritos persistido en `localStorage`
- Autenticación simulada con guardias de navegación
- Formulario de registro e inicio de sesión con validaciones
- Diseño responsivo

### 9.2. Estructura de Carpetas del Proyecto

```
src/
├── app/
│   ├── components/
│   │   ├── atoms/
│   │   │   ├── boton/
│   │   │   └── campo-texto/
│   │   ├── molecules/
│   │   │   ├── tarjeta-personaje/
│   │   │   └── barra-busqueda/
│   │   └── organisms/
│   │       ├── cabecera/
│   │       └── lista-personajes/
│   ├── pages/
│   │   ├── login/
│   │   ├── registro/
│   │   ├── personajes/
│   │   ├── detalle-personaje/
│   │   └── favoritos/
│   ├── services/
│   │   ├── auth.service.ts
│   │   ├── personajes.service.ts
│   │   └── favoritos.service.ts
│   ├── models/
│   │   ├── personaje.model.ts
│   │   └── usuario.model.ts
│   ├── guards/
│   │   └── auth.guard.ts
│   ├── interceptors/
│   │   └── auth.interceptor.ts
│   ├── app.routes.ts
│   └── app.config.ts
└── environments/
    ├── environment.ts
    └── environment.prod.ts
```

### 9.3. Rutas del Proyecto

```typescript
// app.routes.ts
import { Routes } from '@angular/router';
import { authGuard } from './guards/auth.guard';

export const routes: Routes = [
  { path: '', redirectTo: '/personajes', pathMatch: 'full' },
  { path: 'login', loadComponent: () => import('./pages/login/login.component').then(m => m.LoginComponent) },
  { path: 'registro', loadComponent: () => import('./pages/registro/registro.component').then(m => m.RegistroComponent) },
  {
    path: 'personajes',
    canActivate: [authGuard],
    loadComponent: () => import('./pages/personajes/personajes.component').then(m => m.PersonajesComponent)
  },
  {
    path: 'personajes/:id',
    canActivate: [authGuard],
    loadComponent: () => import('./pages/detalle-personaje/detalle-personaje.component').then(m => m.DetallePersonajeComponent)
  },
  {
    path: 'favoritos',
    canActivate: [authGuard],
    loadComponent: () => import('./pages/favoritos/favoritos.component').then(m => m.FavoritosComponent)
  },
  { path: '**', redirectTo: '/personajes' }
];
```

### 9.4. Servicio de Favoritos con localStorage

```typescript
// services/favoritos.service.ts
import { Injectable, signal } from '@angular/core';
import { Personaje } from '../models/personaje.model';

@Injectable({ providedIn: 'root' })
export class FavoritosService {
  private readonly CLAVE = 'favoritos';
  favoritos = signal<Personaje[]>(this.cargarFavoritos());

  private cargarFavoritos(): Personaje[] {
    const datos = localStorage.getItem(this.CLAVE);
    return datos ? JSON.parse(datos) : [];
  }

  private guardar(): void {
    localStorage.setItem(this.CLAVE, JSON.stringify(this.favoritos()));
  }

  agregar(personaje: Personaje): void {
    if (!this.esFavorito(personaje.id)) {
      this.favoritos.update(lista => [...lista, personaje]);
      this.guardar();
    }
  }

  quitar(id: number): void {
    this.favoritos.update(lista => lista.filter(p => p.id !== id));
    this.guardar();
  }

  esFavorito(id: number): boolean {
    return this.favoritos().some(p => p.id === id);
  }
}
```

### 9.5. Servicio de Autenticación Simulada

```typescript
// services/auth.service.ts
import { Injectable, signal } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class AuthService {
  private readonly CLAVE_TOKEN = 'token_sesion';
  estaAutenticado = signal<boolean>(!!localStorage.getItem(this.CLAVE_TOKEN));

  iniciarSesion(correo: string, contrasena: string): boolean {
    // Simulación: cualquier correo válido y contraseña de 6+ caracteres
    if (correo && contrasena.length >= 6) {
      const token = btoa(`${correo}:${Date.now()}`);
      localStorage.setItem(this.CLAVE_TOKEN, token);
      this.estaAutenticado.set(true);
      return true;
    }
    return false;
  }

  cerrarSesion(): void {
    localStorage.removeItem(this.CLAVE_TOKEN);
    this.estaAutenticado.set(false);
  }

  obtenerToken(): string | null {
    return localStorage.getItem(this.CLAVE_TOKEN);
  }
}
```

---

## 10. Despliegue de la Aplicación

### 10.1. Compilación para Producción

```bash
# Compilar la aplicación optimizada para producción
ng build --configuration production
```

Esto genera una carpeta `dist/` con los archivos estáticos listos para desplegar.

### 10.2. Despliegue en GitHub Pages

```bash
# Instalar herramienta de despliegue
npm install -g angular-cli-ghpages

# Compilar y desplegar
ng deploy --base-href=/nombre-del-repositorio/
```

### 10.3. Despliegue en Netlify

1. Sube el código a GitHub
2. Crea una cuenta en [Netlify](https://netlify.com)
3. Conecta tu repositorio de GitHub
4. Configura:
   - **Comando de compilación**: `ng build --configuration production`
   - **Directorio de publicación**: `dist/nombre-proyecto/browser`
5. Haz clic en **Deploy site**

### 10.4. Archivo de Redirección para SPAs

Al desplegar una SPA, es necesario redirigir todas las rutas al `index.html`. En Netlify, crea el archivo `public/_redirects`:

```
/*  /index.html  200
```

---

## 11. Pruebas Unitarias Básicas

Angular incluye Jasmine y Karma para pruebas unitarias.

### 11.1. Prueba de un Servicio

```typescript
// personajes.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { PersonajesService } from './personajes.service';

describe('PersonajesService', () => {
  let servicio: PersonajesService;
  let controladorHttp: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [PersonajesService]
    });
    servicio = TestBed.inject(PersonajesService);
    controladorHttp = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    controladorHttp.verify();
  });

  it('debería obtener personajes de la API', () => {
    const respuestaMock = {
      info: { count: 826, pages: 42, next: null, prev: null },
      results: [{ id: 1, name: 'Rick Sanchez', status: 'Alive', species: 'Human', image: '' }]
    };

    servicio.obtenerPersonajes().subscribe(respuesta => {
      expect(respuesta.results.length).toBe(1);
      expect(respuesta.results[0].name).toBe('Rick Sanchez');
    });

    const solicitud = controladorHttp.expectOne(
      'https://rickandmortyapi.com/api/character?page=1'
    );
    expect(solicitud.request.method).toBe('GET');
    solicitud.flush(respuestaMock);
  });
});
```

### 11.2. Ejecutar las Pruebas

```bash
# Ejecutar pruebas una vez
ng test --watch=false

# Ejecutar pruebas en modo vigilancia
ng test
```

---

## Conclusión

El consumo de APIs REST con `HttpClient` es fundamental para construir aplicaciones Angular dinámicas. Combinando los interceptores para autenticación, los operadores de RxJS para transformar datos y el manejo adecuado de errores y estados de carga, se pueden crear aplicaciones robustas y con buena experiencia de usuario. El proyecto integrador reúne todos los conceptos del curso: componentes, servicios, enrutamiento, formularios reactivos y consumo de APIs, preparándote para el desarrollo profesional con Angular.

# Enrutamiento y Formularios en Angular

## 1. Introducción al Enrutamiento en Angular

El enrutamiento en Angular permite navegar entre diferentes vistas o componentes dentro de una Single Page Application (SPA) sin recargar la página completa. Angular Router es la librería oficial que gestiona la navegación declarativa y programática.

### 1.1. Configuración básica del Router

En Angular 19 con el nuevo enfoque standalone, el router se configura directamente en `app.config.ts`:

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes)
  ]
};
```

Las rutas se definen en `app.routes.ts`:

```typescript
// app.routes.ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', redirectTo: '' }
];
```

### 1.2. RouterOutlet y RouterLink

`<router-outlet>` es el marcador de posición donde Angular renderiza el componente correspondiente a la ruta activa. `RouterLink` permite navegar entre rutas de forma declarativa en las plantillas.

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink, RouterLinkActive } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, RouterLink, RouterLinkActive],
  template: `
    <nav>
      <a routerLink="/" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">Inicio</a>
      <a routerLink="/about" routerLinkActive="active">Acerca de</a>
    </nav>
    <router-outlet />
  `
})
export class AppComponent {}
```

---

## 2. Rutas con Parámetros

### 2.1. Parámetros de ruta (Route Parameters)

Los parámetros de ruta permiten pasar valores dinámicos en la URL.

```typescript
// app.routes.ts
export const routes: Routes = [
  { path: 'personaje/:id', component: PersonajeDetalleComponent }
];
```

Para leer el parámetro en el componente se utiliza `ActivatedRoute`:

```typescript
// personaje-detalle.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-personaje-detalle',
  standalone: true,
  template: `<p>Personaje ID: {{ personajeId }}</p>`
})
export class PersonajeDetalleComponent implements OnInit {
  personajeId: string | null = null;

  constructor(private route: ActivatedRoute) {}

  ngOnInit(): void {
    this.personajeId = this.route.snapshot.paramMap.get('id');
  }
}
```

### 2.2. Query Parameters

Los query parameters son opcionales y se pasan después del signo `?` en la URL.

```typescript
// Navegar con query params
this.router.navigate(['/personajes'], { queryParams: { page: 2, status: 'alive' } });

// Leer query params
this.route.queryParamMap.subscribe(params => {
  const page = params.get('page');
  const status = params.get('status');
});
```

---

## 3. Rutas Anidadas (Child Routes)

Las rutas anidadas permiten definir sub-vistas dentro de un componente padre.

```typescript
// app.routes.ts
export const routes: Routes = [
  {
    path: 'personajes',
    component: PersonajesComponent,
    children: [
      { path: '', component: PersonajesListaComponent },
      { path: ':id', component: PersonajeDetalleComponent }
    ]
  }
];
```

El componente padre debe incluir su propio `<router-outlet>` para renderizar las rutas hijas.

---

## 4. Lazy Loading

El Lazy Loading carga módulos o componentes bajo demanda, mejorando el tiempo de carga inicial de la aplicación.

```typescript
// app.routes.ts
export const routes: Routes = [
  {
    path: 'admin',
    loadComponent: () =>
      import('./admin/admin.component').then(m => m.AdminComponent)
  },
  {
    path: 'personajes',
    loadChildren: () =>
      import('./personajes/personajes.routes').then(m => m.PERSONAJES_ROUTES)
  }
];
```

---

## 5. Guardias de Navegación (Route Guards)

Los guardias permiten controlar el acceso a las rutas según ciertas condiciones (autenticación, permisos, etc.).

### 5.1. canActivate

```typescript
// auth.guard.ts
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from './auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true;
  }

  return router.createUrlTree(['/login']);
};
```

```typescript
// app.routes.ts
export const routes: Routes = [
  {
    path: 'perfil',
    component: PerfilComponent,
    canActivate: [authGuard]
  }
];
```

### 5.2. canDeactivate

Permite confirmar si el usuario desea abandonar una ruta con cambios sin guardar:

```typescript
// pending-changes.guard.ts
import { CanDeactivateFn } from '@angular/router';

export interface PendingChanges {
  hasPendingChanges(): boolean;
}

export const pendingChangesGuard: CanDeactivateFn<PendingChanges> = (component) => {
  if (component.hasPendingChanges()) {
    return confirm('¿Tienes cambios sin guardar. ¿Deseas salir?');
  }
  return true;
};
```

---

## 6. Navegación Programática

El servicio `Router` permite navegar desde el código TypeScript:

```typescript
import { Router } from '@angular/router';

@Component({ /* ... */ })
export class LoginComponent {
  constructor(private router: Router) {}

  onLogin(): void {
    // lógica de autenticación...
    this.router.navigate(['/dashboard']);
    // o con parámetros:
    this.router.navigate(['/personaje', 42]);
  }
}
```

---

## 7. Formularios en Angular

Angular ofrece dos enfoques para trabajar con formularios: **Template-driven Forms** y **Reactive Forms**.

---

## 8. Template-driven Forms

Los formularios dirigidos por plantilla son simples y se definen principalmente en el HTML. Son ideales para formularios sencillos.

```typescript
// contacto.component.ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-contacto',
  standalone: true,
  imports: [FormsModule],
  template: `
    <form #contactoForm="ngForm" (ngSubmit)="onSubmit(contactoForm)">
      <div>
        <label for="nombre">Nombre:</label>
        <input
          id="nombre"
          name="nombre"
          [(ngModel)]="nombre"
          required
          minlength="3"
          #nombreField="ngModel"
        />
        <span *ngIf="nombreField.invalid && nombreField.touched">
          El nombre es obligatorio (mínimo 3 caracteres).
        </span>
      </div>

      <div>
        <label for="email">Email:</label>
        <input
          id="email"
          name="email"
          type="email"
          [(ngModel)]="email"
          required
          email
          #emailField="ngModel"
        />
        <span *ngIf="emailField.invalid && emailField.touched">
          Introduce un email válido.
        </span>
      </div>

      <button type="submit" [disabled]="contactoForm.invalid">Enviar</button>
    </form>
  `
})
export class ContactoComponent {
  nombre = '';
  email = '';

  onSubmit(form: any): void {
    console.log('Formulario enviado:', form.value);
  }
}
```

### 8.1. Two-way binding con ngModel

`[(ngModel)]` sincroniza el valor del campo con la propiedad del componente en ambas direcciones.

---

## 9. Reactive Forms

Los formularios reactivos ofrecen mayor control, son más testables y escalables. La lógica reside en el componente TypeScript.

```typescript
// registro.component.ts
import { Component, OnInit } from '@angular/core';
import { ReactiveFormsModule, FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-registro',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="registroForm" (ngSubmit)="onSubmit()">
      <div>
        <label>Nombre de usuario:</label>
        <input formControlName="username" />
        <span *ngIf="username?.invalid && username?.touched">
          El nombre es obligatorio.
        </span>
      </div>

      <div>
        <label>Correo electrónico:</label>
        <input type="email" formControlName="email" />
        <span *ngIf="email?.invalid && email?.touched">
          Introduce un correo válido.
        </span>
      </div>

      <div formGroupName="passwords">
        <div>
          <label>Contraseña:</label>
          <input type="password" formControlName="password" />
        </div>
        <div>
          <label>Confirmar contraseña:</label>
          <input type="password" formControlName="confirmPassword" />
        </div>
        <span *ngIf="passwords?.errors?.['mismatch'] && passwords?.touched">
          Las contraseñas no coinciden.
        </span>
      </div>

      <button type="submit" [disabled]="registroForm.invalid">Registrarse</button>
    </form>
  `
})
export class RegistroComponent implements OnInit {
  registroForm!: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.registroForm = this.fb.group({
      username: ['', [Validators.required, Validators.minLength(4)]],
      email: ['', [Validators.required, Validators.email]],
      passwords: this.fb.group(
        {
          password: ['', [Validators.required, Validators.minLength(8)]],
          confirmPassword: ['', Validators.required]
        },
        { validators: this.passwordsMatchValidator }
      )
    });
  }

  get username() { return this.registroForm.get('username'); }
  get email() { return this.registroForm.get('email'); }
  get passwords() { return this.registroForm.get('passwords'); }

  passwordsMatchValidator(group: FormGroup) {
    const pass = group.get('password')?.value;
    const confirm = group.get('confirmPassword')?.value;
    return pass === confirm ? null : { mismatch: true };
  }

  onSubmit(): void {
    if (this.registroForm.valid) {
      console.log('Datos del formulario:', this.registroForm.value);
    }
  }
}
```

---

## 10. Validadores

### 10.1. Validadores integrados

Angular proporciona validadores listos para usar:

| Validador | Descripción |
|-----------|-------------|
| `Validators.required` | El campo no puede estar vacío |
| `Validators.minLength(n)` | Longitud mínima de n caracteres |
| `Validators.maxLength(n)` | Longitud máxima de n caracteres |
| `Validators.email` | Formato de email válido |
| `Validators.min(n)` | Valor numérico mínimo |
| `Validators.max(n)` | Valor numérico máximo |
| `Validators.pattern(regex)` | El valor debe cumplir la expresión regular |

### 10.2. Validadores personalizados

```typescript
// validators/no-espacios.validator.ts
import { AbstractControl, ValidationErrors } from '@angular/forms';

export function noEspaciosValidator(control: AbstractControl): ValidationErrors | null {
  const value: string = control.value || '';
  return value.includes(' ') ? { noEspacios: true } : null;
}
```

Uso en un formulario reactivo:

```typescript
username: ['', [Validators.required, noEspaciosValidator]]
```

### 10.3. Validadores asíncronos

Los validadores asíncronos son útiles para verificar datos contra un servidor (por ejemplo, si un nombre de usuario ya existe):

```typescript
// validators/username-disponible.validator.ts
import { AbstractControl, AsyncValidatorFn } from '@angular/forms';
import { inject } from '@angular/core';
import { UserService } from '../services/user.service';
import { map, debounceTime, switchMap } from 'rxjs/operators';
import { timer } from 'rxjs';

export function usernameDisponibleValidator(): AsyncValidatorFn {
  return (control: AbstractControl) => {
    const userService = inject(UserService);
    return timer(400).pipe(
      switchMap(() => userService.checkUsername(control.value)),
      map(disponible => (disponible ? null : { usernameTomado: true }))
    );
  };
}
```

---

## 11. FormArray

`FormArray` permite gestionar un listado dinámico de controles:

```typescript
import { FormArray, FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({ /* ... */ })
export class HabilidadesComponent implements OnInit {
  form!: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.form = this.fb.group({
      habilidades: this.fb.array([
        this.fb.control('', Validators.required)
      ])
    });
  }

  get habilidades(): FormArray {
    return this.form.get('habilidades') as FormArray;
  }

  agregarHabilidad(): void {
    this.habilidades.push(this.fb.control('', Validators.required));
  }

  eliminarHabilidad(index: number): void {
    this.habilidades.removeAt(index);
  }

  onSubmit(): void {
    console.log(this.form.value);
  }
}
```

---

## 12. Comparativa: Template-driven vs Reactive Forms

| Característica | Template-driven | Reactive |
|----------------|-----------------|---------|
| Definición del formulario | En la plantilla HTML | En el componente TypeScript |
| Modelo de datos | `ngModel` (two-way binding) | `FormControl`, `FormGroup`, `FormArray` |
| Validaciones | Directivas en la plantilla | Funciones en TypeScript |
| Testabilidad | Más difícil de testear | Fácil de testear de forma unitaria |
| Complejidad recomendada | Formularios simples | Formularios complejos y dinámicos |
| Reactividad con RxJS | Limitada | Integración nativa con Observables |

---

## Conclusión

El enrutamiento y los formularios son dos pilares fundamentales en el desarrollo de aplicaciones Angular. El **Router** permite construir SPAs con navegación fluida, rutas protegidas y carga diferida. Los **formularios reactivos** brindan un modelo robusto y testable para gestionar entradas de usuario con validaciones síncronas y asíncronas. Combinar ambos conocimientos es clave para construir aplicaciones web modernas, seguras y de alta calidad.

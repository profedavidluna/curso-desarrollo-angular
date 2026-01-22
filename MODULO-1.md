# 📘 Módulo 1: Fundamentos de Angular y TypeScript

**Duración Total**: 11 horas  
**Nivel**: Intermedio  
**Modalidad**: Teórico-Práctica

---

## 🎯 Descripción General

El Módulo 1 establece las bases fundamentales para el desarrollo con Angular. Aprenderás TypeScript, el lenguaje en el que se escribe Angular, conocerás el framework Angular desde cero, y dominarás la metodología de Diseño Atómico para crear arquitecturas de componentes escalables y mantenibles.

Este módulo es **prerequisito obligatorio** para los módulos siguientes, ya que sienta las bases técnicas y conceptuales sobre las cuales construirás aplicaciones Angular profesionales.

---

## 📚 Estructura del Módulo

El Módulo 1 está dividido en **3 submódulos complementarios**:

### 📖 [Módulo 1A: TypeScript - Fundamentos Completos](./MODULO-1A-TYPESCRIPT.md)

**Duración**: 4 horas  
**Enfoque**: Dominar el lenguaje TypeScript

#### Contenido:
- ✅ ¿Qué es TypeScript y por qué usarlo?
- ✅ Sistema de tipos (primitivos, arrays, tuplas, enums)
- ✅ Interfaces y tipos personalizados
- ✅ Clases y Programación Orientada a Objetos
- ✅ Genéricos (Generics)
- ✅ Decoradores (esenciales para Angular)
- ✅ Módulos y namespaces
- ✅ Configuración de TypeScript (tsconfig.json)
- ✅ Buenas prácticas y anti-patrones
- ✅ Debugging y resolución de errores

#### 🔗 [**IR AL MÓDULO 1A →**](./MODULO-1A-TYPESCRIPT.md)

---

### 🚀 [Módulo 1B: Angular - Introducción Completa](./MODULO-1B-ANGULAR-INTRO.md)

**Duración**: 4 horas  
**Enfoque**: Fundamentos del framework Angular

#### Contenido:
- ✅ ¿Qué es Angular? Historia y evolución
- ✅ Instalación del entorno (Node.js, npm, Angular CLI)
- ✅ Versiones de Angular y ciclo de vida LTS
- ✅ Angular CLI - Comandos esenciales
- ✅ Estructura de carpetas y archivos
- ✅ Arquitectura de Angular (componentes, servicios, módulos)
- ✅ Componentes y templates
- ✅ Data binding (interpolation, property, event, two-way)
- ✅ Directivas estructurales (*ngIf, *ngFor, @if, @for)
- ✅ Servicios y Dependency Injection
- ✅ Routing básico
- ✅ Tu primera aplicación: Lista de Héroes
- ✅ Buenas prácticas desde el inicio
- ✅ Debugging en Angular

#### 🔗 [**IR AL MÓDULO 1B →**](./MODULO-1B-ANGULAR-INTRO.md)

---

### 🎨 [Módulo 1C: Diseño Atómico - Arquitectura de Componentes](./MODULO-1C-DISENO-ATOMICO.md)

**Duración**: 3 horas  
**Enfoque**: Metodología para sistemas de diseño escalables

#### Contenido:
- ✅ ¿Qué es el Diseño Atómico? Origen y filosofía
- ✅ Los 5 niveles: Átomos, Moléculas, Organismos, Templates, Páginas
- ✅ Implementación en Angular
- ✅ Estructura de carpetas para Diseño Atómico
- ✅ Casos de uso reales
- ✅ Anti-patrones y errores comunes
- ✅ Storybook para documentación de componentes
- ✅ Buenas prácticas de composición
- ✅ Design systems y tokens de diseño

#### 🔗 [**IR AL MÓDULO 1C →**](./MODULO-1C-DISENO-ATOMICO.md)

---

## 🗓️ Plan de Estudio Sugerido

### Semana 1: TypeScript (Módulo 1A)
| Día | Contenido | Duración | Actividades |
|-----|-----------|----------|-------------|
| **Lunes** | Tipos básicos, interfaces | 2h | Ejercicios de sintaxis |
| **Miércoles** | Clases, POO, Genéricos | 2h | Crear clases de ejemplo |
| **Viernes** | Práctica y revisión | 2h | Ejercicio integrador |

### Semana 2: Angular Intro (Módulo 1B)
| Día | Contenido | Duración | Actividades |
|-----|-----------|----------|-------------|
| **Lunes** | Instalación, CLI, Estructura | 2h | Crear primer proyecto |
| **Miércoles** | Componentes, Servicios, DI | 2h | App de Héroes |
| **Viernes** | Routing, Práctica | 2h | Ejercicio integrador |

### Semana 3: Diseño Atómico (Módulo 1C)
| Día | Contenido | Duración | Actividades |
|-----|-----------|----------|-------------|
| **Lunes** | 5 niveles, Átomos, Moléculas | 1.5h | Crear átomos básicos |
| **Miércoles** | Organismos, Templates | 1.5h | Estructurar componentes |
| **Viernes** | Proyecto final Módulo 1 | 3h | Sistema de diseño completo |

---

## 📝 Evaluación del Módulo 1

### Componentes de Evaluación

| Componente | Peso | Descripción |
|------------|------|-------------|
| **Ejercicios TypeScript** | 20% | 3-5 ejercicios prácticos |
| **App de Héroes (Angular)** | 30% | Primera aplicación funcional |
| **Sistema de Diseño** | 30% | Implementación de Diseño Atómico |
| **Proyecto Integrador** | 20% | Combina todo el módulo |

---

## 🎓 Proyecto Integrador del Módulo 1

### **Portal de Gestión de Personajes - Rick & Morty**

Crear una aplicación Angular que consuma la API de Rick and Morty, aplicando todo lo aprendido en el Módulo 1.

#### Requisitos Funcionales:
1. **Lista de Personajes** - Grid con imagen, nombre, especie, paginación, búsqueda
2. **Detalle de Personaje** - Vista detallada con información completa
3. **Favoritos** - Marcar favoritos y persistir en localStorage

#### API:
```
Base URL: https://rickandmortyapi.com/api
GET /character              // Lista de personajes
GET /character/:id          // Detalle de personaje
GET /character/?name=rick   // Búsqueda
```

---

## 📚 Recursos Consolidados del Módulo 1

### 📖 Documentación Oficial
- **TypeScript**: https://www.typescriptlang.org/docs/
- **Angular.dev**: https://angular.dev/
- **Atomic Design**: https://atomicdesign.bradfrost.com/

### 🎥 Cursos en Video
- **TypeScript Course (freeCodeCamp)**: https://www.youtube.com/watch?v=30LWjhZzg50
- **Angular University**: https://angular-university.io/
- **Ultimate Angular**: https://ultimatecourses.com/

### 🛠️ Herramientas Esenciales
- **Visual Studio Code**: https://code.visualstudio.com/
- **Angular DevTools**: Chrome Extension
- **Storybook**: https://storybook.js.org/

### 🌐 Comunidades
- **Angular Discord**: https://discord.gg/angular
- **TypeScript Discord**: https://discord.gg/typescript
- **Stack Overflow**: [angular] y [typescript] tags

---

## ✅ Checklist de Completitud del Módulo 1

### Módulo 1A: TypeScript
- [ ] Instalar Node.js, npm, TypeScript
- [ ] Compilar mi primer archivo .ts
- [ ] Crear interfaces y tipos personalizados
- [ ] Implementar clases con modificadores de acceso
- [ ] Usar genéricos en funciones y clases
- [ ] Aplicar decoradores
- [ ] Configurar tsconfig.json
- [ ] Completar ejercicios de TypeScript

### Módulo 1B: Angular
- [ ] Instalar Angular CLI
- [ ] Crear mi primer proyecto Angular
- [ ] Generar componentes con CLI
- [ ] Implementar data binding (4 tipos)
- [ ] Usar directivas estructurales
- [ ] Crear servicios con DI
- [ ] Configurar routing básico
- [ ] Completar App de Héroes

### Módulo 1C: Diseño Atómico
- [ ] Identificar átomos, moléculas, organismos
- [ ] Estructurar carpetas con metodología atómica
- [ ] Crear componentes reutilizables
- [ ] Implementar composición de componentes
- [ ] Documentar componentes en Storybook
- [ ] Evitar anti-patrones comunes
- [ ] Completar sistema de diseño básico

---

## 🚀 Siguientes Pasos

Una vez completado el Módulo 1, estarás preparado para:

### → [Módulo 2: Componentes, Módulos y Servicios](./MODULO-2.md)
### → [Módulo 3: Enrutamiento y Formularios](./MODULO-3.md)
### → [Módulo 4: Consumo de APIs REST y Proyecto Integrador](./MODULO-4.md)

---

## 💬 Preguntas Frecuentes (FAQ)

**¿Es obligatorio hacer los 3 submódulos en orden?**  
Sí, el orden recomendado es 1A → 1B → 1C.

**¿Puedo usar JavaScript en lugar de TypeScript?**  
No, Angular está diseñado para TypeScript.

**¿Cuánto tiempo debo dedicar al Módulo 1?**  
Mínimo 2-3 semanas (4-6 horas por semana).

---

**¡Éxito en tu aprendizaje!**

→ **[COMENZAR CON MÓDULO 1A: TypeScript](./MODULO-1A-TYPESCRIPT.md)** 🚀

---

**Última actualización**: 2026-01-22 08:03:22  
**Autor**: Prof. David Luna  
**Licencia**: MIT
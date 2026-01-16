# 🚀 Curso de Desarrollo con Angular

Bienvenido al **Curso de Desarrollo con Angular** - Un programa completo de 32 horas diseñado para dominar el desarrollo de aplicaciones web modernas con Angular 19.

## 📋 Información del Curso

- **Duración Total**: 32 horas
- **Modalidad**: Práctico-experiencial
- **Nivel**: Intermedio
- **Framework**: Angular 19
- **Lenguaje**: TypeScript

## 🎯 Objetivos Generales

Al finalizar este curso, serás capaz de: 

- ✅ Desarrollar aplicaciones SPA (Single Page Applications) robustas y escalables
- ✅ Implementar arquitecturas modulares siguiendo mejores prácticas
- ✅ Gestionar el estado y la comunicación entre componentes
- ✅ Consumir APIs REST y manejar datos asíncronos
- ✅ Crear formularios reactivos con validaciones complejas
- ✅ Implementar sistemas de enrutamiento y navegación
- ✅ Aplicar pruebas unitarias y desplegar aplicaciones en producción

## 📚 Estructura del Curso

El curso está organizado en 4 módulos progresivos, cada uno con su propio branch:

| Módulo | Tema | Duración | Branch | Competencia SFIA |
|--------|------|----------|--------|------------------|
| **0** | [Fundamentos de CSS - Flexbox y Grid](./MODULO-0.md) | 4 horas | [`modulo-0-css-layouts`](../../tree/modulo-0-css-layouts) | Prerequisito |
| **1** | [Introducción a Angular y TypeScript](./MODULO-1.md) | 8 horas | [`modulo-1-intro-angular-typescript`](../../tree/modulo-1-intro-angular-typescript) | PROG 3-4 |
| **2** | [Componentes, Módulos y Servicios](./MODULO-2.md) | 8 horas | [`modulo-2-componentes-servicios`](../../tree/modulo-2-componentes-servicios) | DESN 4 |
| **3** | [Enrutamiento y Formularios](./MODULO-3.md) | 8 horas | [`modulo-3-enrutamiento-formularios`](../../tree/modulo-3-enrutamiento-formularios) | SINT 4 |
| **4** | [Consumo de APIs REST y Proyecto Integrador](./MODULO-4.md) | 8 horas | [`modulo-4-apis-proyecto`](../../tree/modulo-4-apis-proyecto) | SWDN 4 / TEST 4 |

## 🛠️ Requisitos Previos

Antes de comenzar, asegúrate de tener: 

- Conocimientos básicos de HTML, CSS y JavaScript
- Familiaridad con programación orientada a objetos
- Experiencia básica con Git y GitHub
- Motivación para aprender y practicar

## 💻 Herramientas Necesarias

Instala las siguientes herramientas antes de iniciar:

1. **Node.js** (v18 o superior) - [Descargar](https://nodejs.org/)
2. **npm** (incluido con Node.js)
3. **Angular CLI** v19: 
   ```bash
   npm install -g @angular/cli@19
   ```
4. **Editor de código**: [Visual Studio Code](https://code.visualstudio.com/) (recomendado)
5. **Git**: [Descargar](https://git-scm.com/)

### Extensiones recomendadas para VS Code:
- Angular Language Service
- Angular Snippets
- ESLint
- Prettier
- GitLens

## 🚀 Cómo Usar Este Repositorio

### 1. Clona el repositorio
```bash
git clone https://github.com/profedavidluna/curso-desarrollo-angular.git
cd curso-desarrollo-angular
```

### 2. Navega por los módulos
Cada módulo tiene su propio branch con ejercicios prácticos:

```bash
# Para el Módulo 0
git checkout modulo-0-css-layouts

# Para el Módulo 1
git checkout modulo-1-intro-angular-typescript

# Para el Módulo 2
git checkout modulo-2-componentes-servicios

# Para el Módulo 3
git checkout modulo-3-enrutamiento-formularios

# Para el Módulo 4
git checkout modulo-4-apis-proyecto
```

### 3. Instala las dependencias
En cada branch de módulo: 

```bash
npm install
```

### 4. Ejecuta la aplicación
```bash
ng serve
```

Abre tu navegador en `http://localhost:4200`

## 📖 Metodología de Aprendizaje

Cada módulo sigue esta estructura:

1. **📘 Teoría**: Documentación detallada en archivos `.md`
2. **💡 Ejemplos**: Código funcional comentado
3. **✏️ Ejercicios**: Plantillas con TODOs para completar
4. **🎯 Actividad Experiencial**: Proyecto práctico integrador
5. **🤔 Reflexión Guiada**: Preguntas para profundizar conceptos

## 🎓 Proyecto Integrador Final

El curso culmina con el desarrollo de un **Portal de Gestión de Tareas** que incluye:

- ✅ Autenticación de usuarios
- ✅ CRUD completo de tareas
- ✅ Enrutamiento con guardias de navegación
- ✅ Consumo de API REST
- ✅ Formularios reactivos con validaciones
- ✅ Diseño responsivo
- ✅ Deploy en producción

## 🌐 API Utilizada

A lo largo del curso utilizaremos la **Rick and Morty API** para practicar:

- **Documentación**: https://rickandmortyapi.com/documentation
- **Endpoint Base**: `https://rickandmortyapi.com/api`
- **Recursos**: Characters, Locations, Episodes

Ejemplo: 
```typescript
// Obtener personajes
GET https://rickandmortyapi.com/api/character

// Obtener un personaje específico
GET https://rickandmortyapi.com/api/character/1
```

## 📂 Estructura del Proyecto (en cada módulo)

```
proyecto-modulo/
├── src/
│   ├── app/
│   │   ├── components/      # Componentes reutilizables
│   │   ├── services/        # Servicios e inyección de dependencias
│   │   ├── models/          # Interfaces y tipos TypeScript
│   │   ├── guards/          # Guardias de navegación
│   │   ├── interceptors/    # Interceptores HTTP
│   │   └── modules/         # Módulos de funcionalidad
│   ├── assets/              # Recursos estáticos
│   └── environments/        # Configuración de entornos
├── EJERCICIOS.md            # Guía de ejercicios prácticos
└── README.md                # Documentación del módulo
```

## 🤝 Contribuciones y Preguntas

- **Issues**: Usa la pestaña [Issues](../../issues) para hacer preguntas o reportar problemas
- **Discussions**: Participa en [Discussions](../../discussions) para compartir conocimientos
- **Pull Requests**: Si encuentras errores, ¡las correcciones son bienvenidas!

## 📜 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](./LICENSE) para más detalles.

## 👨‍🏫 Instructor

**Prof. David Luna** - [@profedavidluna](https://github.com/profedavidluna)

## 🌟 Recursos Adicionales

- [Documentación Oficial de Angular](https://angular.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [RxJS Documentation](https://rxjs.dev/)
- [Angular Style Guide](https://angular.dev/style-guide)
- [Rick and Morty API Docs](https://rickandmortyapi.com/documentation)

---

**¡Comienza tu viaje en Angular ahora! 🚀**

Revisa la documentación de cada módulo y prepárate para construir aplicaciones web modernas y escalables.

```bash
git checkout modulo-0-css-layouts
```

**¡Éxito en tu aprendizaje!** 💪
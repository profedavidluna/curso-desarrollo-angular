# Introduction to Angular

## Installation
To install Angular, begin by installing Node.js and npm (Node Package Manager) from [nodejs.org](https://nodejs.org/).
Then, install Angular CLI globally by running:

```bash
npm install -g @angular/cli
```

## Versions
Angular has undergone significant updates. Here are a few major version highlights:
- **AngularJS (1.x)**: The original version focused on two-way data binding.
- **Angular 2**: A complete rewrite, introduced a component-based architecture.
- **Angular 4**: Provided a smaller and faster application, introduced new features like the `ngIf` and `ngFor` structural directives.
- **Current**: Always check [Angular's official changelog](https://github.com/angular/angular/blob/main/CHANGELOG.md) for the latest updates.

## CLI Commands
The Angular CLI provides various commands to work with Angular applications:
- `ng new <app-name>`: Creates a new application.
- `ng serve`: Compiles the application and starts a web server.
- `ng generate component <component-name>`: Generates a new component.
- `ng build`: Compiles the application into an output directory.

## Folder Structure
When you create a new Angular application, the following folder structure is generated:
```
/my-angular-app
  ├── e2e
  ├── node_modules
  ├── src
  │   ├── app
  │   ├── assets
  │   ├── environments
  │   └── index.html
  ├── angular.json
  ├── package.json
  └── tsconfig.json
```

## Architecture
Angular's architecture is based on components, services, and dependency injection. Components control views and serve as the building blocks of an Angular application. Services are used to organize the business logic and data management.

## Best Practices
- Use Angular's CLI for project scaffolding.
- Follow Angular style guide for coding standards.
- Use reactive forms for complex form scenarios.

## Debugging
Utilize browser developer tools for debugging your Angular application. Use Angular DevTools extension for insightful debugging related to Angular components and performance.

## First App Tutorial
To create your first Angular app:
1. Run `ng new my-first-app` in the terminal.
2. Navigate to the app directory with `cd my-first-app`.
3. Start the application with `ng serve`.
4. Open `http://localhost:4200` in a web browser to see your app.

## Learning Resources
- [Official Angular Documentation](https://angular.io/docs)
- [Angular University](https://angular-university.io/)
- [Coursera: Angular](https://www.coursera.org/learn/angular)

Happy coding!
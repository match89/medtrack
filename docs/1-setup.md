## 1. Project Creation and Basic Components

### Prerequisites

Before you begin, make sure you have the following installed:

- **Node.js** (LTS): [Download Node.js](https://nodejs.org/)
- **Visual Studio Code**: [Download VS Code](https://code.visualstudio.com/)

### ⚛️ Concepts

* **Angular CLI:** generating projects and components.

### 🔧 What we need to do

```bash
npm i -g @angular/cli@latest
ng new medtrack # accept all the defaults

cd medtrack
code .
```

Go through and explain the structure of the generated project:
- `package.json` – Project dependencies and scripts.
- `angular.json` – Angular CLI configuration (build, serve, test, etc.).
- `tsconfig[.*].json` – TypeScript configuration files (base, app, spec).
- `index.html` – Main HTML entry point for the app.
- `main.ts` – Application bootstrap file.
- `app.config.ts` – Application-wide configuration (providers, routing, etc.).
- `app.ts` and `app.html` – Root component and its template.

#### Project Structure Explanation

The Angular project is organized to separate concerns and make the codebase maintainable:

- The `src/` folder contains all source code for the application.
- `app/` holds the main application logic, including components, pages, UI elements, and domain services.
- `app/ui/` contains reusable, presentational (dumb) components.
- `app/pages/` contains feature components, each representing a route (page).
- `app/domain/` contains domain logic and services for business rules and data access.
- Configuration files like `angular.json`, `tsconfig.json`, and `package.json` are at the root for easy access and project-wide settings.

This structure helps keep the code modular, easy to navigate, and scalable as the application grows.

### ✅ Final State

* Test by running `ng serve` and verify that components render.

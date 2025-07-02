# Copilot Instructions for Angular Demo Application

## Project Overview

This project is a simple Angular demo application designed for students to learn the basics and new features of Angular. The code should be easy to understand, well-commented, and demonstrate best practices. The application will showcase:

- Standalone Components
- New template syntax with @if, @for, @let, etc
- Angular Signals, resource and httpResource
- Component communication and services
- Basic routing
- Reactive forms
- Dependency Injection
- Angular CLI usage
- Writting tests

## Project description

The application is a medical tracking system that allows users to:
- View a list of patients and their medications.
- Add, Edit and View patient details and their medications.

## Project structure

- src/app/ui/ - Contains reusable dumb components
- src/app/pages/ - Contains feature components (pages), each representing a route
- src/app/domain/ - Contains domain logic and services

## Coding Guidelines

- Use Angular v17+ features where possible.
- Prefer standalone components over NgModules.
- Use Angular Signals for state and reactivity.
- Use httpResource for HTTP operations.
- Keep code simple, readable, and well-commented.
- Demonstrate best practices (naming, structure, separation of concerns).
- Use TypeScript strict mode.
- Avoid unnecessary complexity.

## Required Concepts to Demonstrate

1. **Signals**
    - Use signals for local and shared state.
    - Show computed and effect signals, linked signals.

2. **Standalone Components**
    - All components should be standalone.
    - Demonstrate component composition.

3. **httpResource**
    - Use `httpResource` for fetching and posting data.
    - Show loading and error handling.

4. **Component Communication**
    - Use input() and output() functions for parent-child communication.
    - Use signals or services for sibling/shared state.

5. **Routing**
    - Set up basic routing with standalone components.
    - Demonstrate navigation.

6. **Forms**
    - Include reactive forms, no need for template-driven forms.
    - Show form validation.

7. **Dependency Injection**
    - Use services with DI for shared logic or data.

8. **State Management**
    - Use signals or services for simple state management.

## Documentation

Every step in the learning process should be documented in the docs folder in a Markdown file. This includes:
- Explanation of concepts used.
- Code snippets and examples.
- Instructions for running the application.
- Details about the project structure and organization.
- How to verify that the changes made work as expected (e.g., running tests, checking functionality).
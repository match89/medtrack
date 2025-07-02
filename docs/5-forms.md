# 5. Forms in Angular

## 📝 Concepts

- **Reactive Forms**: Forms are created and managed in the component class using the `FormBuilder` and `FormGroup` APIs. This approach provides more control and scalability for complex forms.
- **FormBuilder**: A service that helps construct form groups and controls with less boilerplate.
- **Validation**: Angular provides built-in validators (like `Validators.required`, `Validators.email`, etc.) and supports custom validators.
- **Form Status**: You can check if a form or control is valid, invalid, touched, dirty, etc.

## 🚀 What we need to do

We will create a standalone page component for adding a new patient. This page will use a reactive form with meaningful validation.

### 1. Create the Add Patient Page

#### `src/app/pages/add-patient/add-patient.ts`
```typescript
import { Component, inject } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-add-patient',
  standalone: true,
  imports: [ReactiveFormsModule],
  templateUrl: './add-patient.html',
})
export class AddPatientPage {
  private fb = inject(FormBuilder);

  // Define the form with validation
  patientForm = this.fb.group({
    name: ['', [Validators.required, Validators.minLength(2)]],
    dob: ['', [Validators.required]],
    email: ['', [Validators.required, Validators.email]],
    phone: ['', [Validators.pattern(/^\+?[0-9\- ]{7,15}$/)]],
  });

  // Helper getters for template
  get name() { return this.patientForm.get('name'); }
  get dob() { return this.patientForm.get('dob'); }
  get email() { return this.patientForm.get('email'); }
  get phone() { return this.patientForm.get('phone'); }

  onSubmit() {
    if (this.patientForm.valid) {
      // Here you would call a service to save the patient
      alert('Patient added: ' + JSON.stringify(this.patientForm.value, null, 2));
      this.patientForm.reset();
    } else {
      this.patientForm.markAllAsTouched();
    }
  }
}
```

#### Validation rules:
- **Name**: Required, at least 2 characters
- **Date of Birth**: Required
- **Email**: Required, must be a valid email
- **Phone**: Optional, must match a phone pattern if provided

### 2. Create the Template

#### `src/app/pages/add-patient/add-patient.html`
```html
<h2>Add Patient</h2>
<form [formGroup]="patientForm" (ngSubmit)="onSubmit()">
  <label>
    Name:
    <input formControlName="name" />
    @if (name?.invalid && (name?.dirty || name?.touched)) {
      @if (name?.errors?.['required']) {<span>Name is required.</span>}
      @if (name?.errors?.['minlength']) {<span>Name must be at least 2 characters.</span>}
    }
  </label>
  <br />
  <label>
    Date of Birth:
    <input type="date" formControlName="dob" />
    @if (dob?.invalid && (dob?.dirty || dob?.touched)) {
      @if (dob?.errors?.['required']) {<span>Date of birth is required.</span>}
    }
  </label>
  <br />
  <label>
    Email:
    <input type="email" formControlName="email" />
    @if (email?.invalid && (email?.dirty || email?.touched)) {
      @if (email?.errors?.['required']) {<span>Email is required.</span>}
      @if (email?.errors?.['email']) {<span>Invalid email address.</span>}
    }
  </label>
  <br />
  <label>
    Phone:
    <input formControlName="phone" />
    @if (phone?.invalid && (phone?.dirty || phone?.touched)) {
      @if (phone?.errors?.['pattern']) {<span>Invalid phone number.</span>}
    }
  </label>
  <br />
  <button type="submit" [disabled]="patientForm.invalid">Add Patient</button>
</form>
```

### 3. Add Route for the Page

Add the route in your main routing configuration (e.g., `app.config.ts`):

```typescript
// ...existing code...
{
  path: 'add-patient',
  loadComponent: () => import('./pages/add-patient/add-patient').then(m => m.AddPatientPage)
},
// ...existing code...
```

### 4. How to Test

- Navigate to `/add-patient` in your app.
- Try submitting the form with missing or invalid fields to see validation in action.
- Fill out the form correctly and submit. You should see an alert with the patient data.

---

This demonstrates how to use **reactive forms** with validation and best practices in Angular 17+ using standalone components, FormBuilder, and the new template control flow syntax.

# PatientService: Providing Patient Data with Angular Signals

This step demonstrates how to create and use a service in Angular to manage and provide patient data using Angular v17+ features, including signals for state management.

## 1. Generating the Service

We used the Angular CLI to generate a new service in the domain layer:

```bash
ng generate service domain/patient
# ng g s domain/patient
```

#### `src/app/domain/patient.ts`
```typescript
import { Injectable, computed, signal } from '@angular/core';

export type Patient = {
  id: number;
  name: string;
  dob: string;
  address: string;
  phone: string;
  email: string;
  medicalConditions: string[];
  medications: Medication[];
};

export type Medication = {
  name: string;
  dose: string;
  schedule: string;
};

@Injectable({ providedIn: 'root' })
export class PatientService {
  
  private patients = signal<Patient[]>(PATIENTS);

  readonly patientList = computed(() => this.patients());

  getPatientDetailById(id: number) {
    return computed(() => this.patients().find(p => p.id === id));
  }
}

const PATIENTS: Patient[] = [
    {
      id: 1,
      name: 'Alice Smith',
      dob: '1991-03-15',
      address: '123 Main St, Springfield',
      phone: '555-1234',
      email: 'alice@example.com',
      medicalConditions: ['Hypertension'],
      medications: [
        { name: 'Amlodipine', dose: '5mg', schedule: 'Morning' }
      ]
    },
    {
      id: 2,
      name: 'Bob Johnson',
      dob: '1982-07-22',
      address: '456 Oak Ave, Metropolis',
      phone: '555-5678',
      email: 'bob@example.com',
      medicalConditions: ['Asthma'],
      medications: [
        { name: 'Metformin', dose: '500mg', schedule: 'Morning' },
        { name: 'Lisinopril', dose: '10mg', schedule: 'Evening' }
      ]
    },
    {
      id: 3,
      name: 'Carol Lee',
      dob: '1996-11-05',
      address: '789 Pine Rd, Gotham',
      phone: '555-9012',
      email: 'carol@example.com',
      medicalConditions: ['Diabetes'],
      medications: [
        { name: 'Insulin', dose: '20U', schedule: 'Morning/Evening' }
      ]
    },
    {
      id: 4,
      name: 'David Kim',
      dob: '1974-09-30',
      address: '321 Maple St, Star City',
      phone: '555-3456',
      email: 'david@example.com',
      medicalConditions: ['High Cholesterol'],
      medications: [
        { name: 'Atorvastatin', dose: '20mg', schedule: 'Night' }
      ]
    },
    {
      id: 5,
      name: 'Eva Müller',
      dob: '1988-01-12',
      address: '654 Elm St, Central City',
      phone: '555-7890',
      email: 'eva@example.com',
      medicalConditions: ['Allergy'],
      medications: [
        { name: 'Cetirizine', dose: '10mg', schedule: 'As needed' }
      ]
    },
    {
      id: 6,
      name: 'Frank O’Connor',
      dob: '1979-05-18',
      address: '987 Cedar Ave, Coast City',
      phone: '555-2345',
      email: 'frank@example.com',
      medicalConditions: ['Arthritis'],
      medications: [
        { name: 'Ibuprofen', dose: '400mg', schedule: 'As needed' }
      ]
    },
    {
      id: 7,
      name: 'Grace Chen',
      dob: '1997-06-25',
      address: '159 Spruce St, Keystone',
      phone: '555-6789',
      email: 'grace@example.com',
      medicalConditions: ['Migraine'],
      medications: [
        { name: 'Sumatriptan', dose: '50mg', schedule: 'As needed' }
      ]
    },
    {
      id: 8,
      name: 'Hiro Tanaka',
      dob: '1970-12-03',
      address: '753 Willow Rd, Blüdhaven',
      phone: '555-4321',
      email: 'hiro@example.com',
      medicalConditions: ['COPD'],
      medications: [
        { name: 'Tiotropium', dose: '18mcg', schedule: 'Morning' }
      ]
    },
    {
      id: 9,
      name: 'Isabella Rossi',
      dob: '1994-04-09',
      address: '852 Birch Ln, Smallville',
      phone: '555-8765',
      email: 'isabella@example.com',
      medicalConditions: ['Thyroid Disorder'],
      medications: [
        { name: 'Levothyroxine', dose: '75mcg', schedule: 'Morning' }
      ]
    }
  ];
```


## 2. Implementing the Service

We renamed the class to `PatientService` and implemented the following:

- **Domain Types:**
  - Defined `Patient` and `Medication` types directly in the domain folder. These types include all patient details and are not imported from any page or component.
- **Signals for State:**
  - `patients`: Holds the list of patients (with all details) as a signal.
- **Computed Signals:**
  - `patientList`: Exposes the patient list as a computed signal.
- **Methods:**
  - `getPatientDetailById(id: number)`: Returns a computed signal for a specific patient's details by ID.

The service demonstrates best practices:
- Uses Angular's `@Injectable` with `providedIn: 'root'` for DI.
- Uses signals for state and computed values for reactivity.
- All patient data is now managed in a single signal, and each patient includes full details (address, phone, email, conditions, medications, etc).
- No dependency on types from feature pages or UI components.

Let's inject and use `PatientService` in the patient list and detail components, replacing hardcoded patient data with data from the service.

#### `src/app/pages/patient-list/patient-list.ts`
```typescript
import { Component, inject } from '@angular/core';
import { MatListModule } from '@angular/material/list';
import { MatCardModule } from '@angular/material/card';
import { MatButtonModule } from '@angular/material/button';
import { MatGridListModule } from '@angular/material/grid-list';
import { PatientHeader } from '../../ui/patient-header/patient-header';
import { RouterLink } from '@angular/router';
import { PatientService } from '../../domain/patient';

@Component({
  selector: 'app-patient-list',
  imports: [RouterLink, MatListModule, MatButtonModule, MatCardModule, MatButtonModule, MatGridListModule, PatientHeader],
  templateUrl: './patient-list.html'
})
export class PatientList {
  private patientService = inject(PatientService);
  
  patients = this.patientService.patientList;

  onHeaderClicked(id: number) {
    console.log('Header clicked for patient ID:', id);
  }
}
```

#### `src/app/pages/patient-list/patient-list.html`
```html
...
@for (patient of patients(); track patient.id) {
...
```

**Explanation:**
- The service is injected using `inject(PatientService)`.
- The `patients` property is now a signal, so the template can use it reactively.
- No local patient data or types are needed; all data comes from the domain service.

---

### Patient Detail Component (`src/app/pages/patient-detail/patient-detail.ts`)

We inject both the `PatientService` and `ActivatedRoute` to fetch the patient ID from the route and retrieve the corresponding patient details as a signal.

```typescript
import { DatePipe } from '@angular/common';
import { Component, inject } from '@angular/core';
import { MatCardModule } from '@angular/material/card';
import { MatTableModule } from '@angular/material/table';
import { PatientService } from '../../domain/patient';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-patient-detail',
  standalone: true,
  imports: [DatePipe, MatCardModule, MatTableModule],
  templateUrl: './patient-detail.html'
})
export class PatientDetail {
  private patientService = inject(PatientService);
  private route = inject(ActivatedRoute);

  // Get patient ID from route params and fetch patient details as a signal
  patient = this.patientService.getPatientDetailById(Number(this.route.snapshot.paramMap.get('id')));
}
```
---
#### `src/app/pages/patient-detail/patient-detail.html`
```html
<mat-card class="patient-detail-card">
  @if (patient(); as patient) {
  <app-patient-header
    [id]="patient.id"
    [name]="patient.name"
    [dob]="patient.dob"
  >
  </app-patient-header>
  <mat-card-content>
    <section class="info-section">
      <h3>Contact Information</h3>
      <p><strong>Address:</strong> {{ patient.address }}</p>
      <p><strong>Phone:</strong> {{ patient.phone }}</p>
      <p><strong>Email:</strong> {{ patient.email }}</p>
    </section>
    <section class="info-section">
      <h3>Medical Conditions</h3>
      <ul>
        @for(condition of patient.medicalConditions; track condition) {
        <li>{{ condition }}</li>
        }
      </ul>
    </section>
    <section class="info-section">
      <h3>Medicine Schedule</h3>
      <table
        mat-table
        [dataSource]="patient.medications || []"
        class="mat-elevation-z1 med-table"
      >
        <ng-container matColumnDef="name">
          <th mat-header-cell *matHeaderCellDef>Medicine</th>
          <td mat-cell *matCellDef="let med">{{ med.name }}</td>
        </ng-container>
        <ng-container matColumnDef="dose">
          <th mat-header-cell *matHeaderCellDef>Dose</th>
          <td mat-cell *matCellDef="let med">{{ med.dose }}</td>
        </ng-container>
        <ng-container matColumnDef="schedule">
          <th mat-header-cell *matHeaderCellDef>Schedule</th>
          <td mat-cell *matCellDef="let med">{{ med.schedule }}</td>
        </ng-container>
        <tr mat-header-row *matHeaderRowDef="['name', 'dose', 'schedule']"></tr>
        <tr
          mat-row
          *matRowDef="let row; columns: ['name', 'dose', 'schedule']"
        ></tr>
      </table>
    </section>
  </mat-card-content>
  }
</mat-card>

<style>
  .patient-detail-card {
    max-width: 700px;
    margin: 2rem auto;
    padding: 1.5rem;
  }

  .info-section {
    margin-bottom: 1.5rem;
  }
  .med-table {
    width: 100%;
    margin-top: 1rem;
  }
</style>
```

**Explanation:**
- The service is injected using `inject(PatientService)`.
- The route is injected using `inject(ActivatedRoute)` to access the patient ID from the URL.
- The `patient` property is a computed signal that provides the details for the selected patient.
- No local patient detail data or types are needed; all data comes from the domain service.

---

By using the service in both components, we ensure a single source of truth for patient data, demonstrate Angular signals for reactivity, and follow best practices for separation of concerns and maintainability.

---
It's time to use input route parameters to fetch patient details dynamically based on the URL. This allows us to display the correct patient information when navigating to the detail page.

#### `src/app/app.config.ts`
```typescript
...
provideRouter(routes, withComponentInputBinding())
...
```
---
#### `src/app/pages/patient-detail/patient-detail.ts`
```typescript
...
export class PatientDetail {
  private patientService = inject(PatientService);
  
  id = input.required<string>();
  patient = computed(() => this.patientService.getPatientDetailById(+this.id())());
}
```

### ✅ Final State
The app now dynamically fetches and displays patient details based on the ID provided in the URL. The `PatientService` manages all patient data, ensuring a clean separation of concerns and reactivity using Angular signals.
Test by navigating to a patient detail page, e.g., `/patient/1`, and verifying that the correct patient information is displayed.

## 3. Routing

### ⚛️ Concepts
* **Router**: switches components based on URL.
* **Route parameters**: dynamic values in the URL
* **ActivatedRoute**: provides access to information about a route associated with a component loaded in an outlet.
* **RouterLink**: directive to link to routes.
* **RouterOutlet**: placeholder for routed components.
* **Lazy loading**: loading modules only when needed.

### 🔧 What we need to do

```bash
ng generate component pages/patient-detail
# ng g c pages/patient-detail
ng generate component pages/medication-list
# ng g c pages/medication-list
```

Enable routing in the app to navigate between the patient list and patient detail pages.

#### `src/app/app.config.ts`
```typescript
import { ApplicationConfig, provideBrowserGlobalErrorListeners, provideZoneChangeDetection } from '@angular/core';
import { provideRouter, Routes } from '@angular/router';
import { PatientDetail } from './pages/patient-detail/patient-detail';
import { PatientList } from './pages/patient-list/patient-list';

const routes: Routes = [
  { path: '', redirectTo: '/patient', pathMatch: 'full' },
  { path: 'patient', component: PatientList },
  { path: 'patient/:id', component: PatientDetail },
  { path: 'medication', loadComponent: () => import('./pages/medication-list/medication-list').then(m => m.MedicationList) }
];
export const appConfig: ApplicationConfig = {
  providers: [
    provideBrowserGlobalErrorListeners(),
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes)
  ]
};
```
---
Use router-outlet to display the current route's component.
#### `src/app/app.ts`
```typescript
import { Component } from '@angular/core';
import { RouterLink, RouterOutlet } from '@angular/router';
import { MatToolbarModule } from '@angular/material/toolbar';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, RouterLink, MatToolbarModule],
  template: `
    <mat-toolbar color="primary"
      ><span [style]="{ cursor: 'pointer' }" routerLink="/"
        >MedTrack</span
      ></mat-toolbar
    >
    <main style="padding: 1rem">
      <router-outlet></router-outlet>
    </main>
  `,
})
export class App {}
```
---
Create a patient detail page that displays patient information based on the ID in the URL.

#### `src/app/pages/patient-detail/patient-detail.ts`
```typescript
import { DatePipe } from '@angular/common';
import { Component, inject, signal } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { MatCardModule } from '@angular/material/card';
import { MatTableModule } from '@angular/material/table';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-patient-detail',
  standalone: true,
  imports: [DatePipe, MatCardModule, MatTableModule],
  templateUrl: './patient-detail.html'
})
export class PatientDetail {
  private readonly activatedRoute = inject(ActivatedRoute);
  
  patientId = this.activatedRoute.params.pipe(map(params => params['id']));
  
  patient: PatientDetailModel = {
    id: this.patientId,
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
  }
}

export type PatientDetailModel = {
  id: number;
  name: string;
  dob: string;
  address: string;
  phone: string;
  email: string;
  medicalConditions: string[];
  medications: Medication[];
}
export type Medication = {
  name: string;
  dose: string;
  schedule: string;
}
```
---
#### `src/app/pages/patient-detail/patient-detail.html`
```html
<mat-card class="patient-detail-card">
  <mat-card-header>
    <div mat-card-avatar>
      <img [src]="'https://api.dicebear.com/9.x/pixel-art/svg?seed=' + patient.name" alt="Avatar of {{patient.name}}" width="40" height="40" />
    </div>
    <mat-card-title>{{ patient.name }}</mat-card-title>
    <mat-card-subtitle>ID: {{ patient.id }} | {{ patient.dob | date }}</mat-card-subtitle>
  </mat-card-header>
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
      <table mat-table [dataSource]="patient.medications || []" class="mat-elevation-z1 med-table">
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
        <tr mat-row *matRowDef="let row; columns: ['name', 'dose', 'schedule'];"></tr>
      </table>
    </section>
  </mat-card-content>
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
---

Add a button link to navigate to the patient detail page from the patient list.
#### `src/app/pages/patient-list/patient-list.ts`
```typescript
...
<mat-card-actions>
  <a mat-button color="primary" [routerLink]="['/patient', patient.id]">Detail</a>
</mat-card-actions>
...
```
---

Now, make the current route visible in the app's navigation bar by using `routerLinkActive`.
#### `src/app/app.ts`
```typescript
import { Component } from '@angular/core';
import { RouterLink, RouterLinkActive, RouterOutlet } from '@angular/router';
import { MatToolbarModule } from '@angular/material/toolbar';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, RouterLink, RouterLinkActive, MatToolbarModule],
  template: `
    <mat-toolbar color="primary">
      <strong routerLink="/">MedTrack</strong>
      <span class="small" routerLink="/patient" routerLinkActive="active" style="margin-left: auto;">Patients</span>
      <span class="small" routerLink="/medication" routerLinkActive="active" style="margin-left: 1rem;">Medications</span>
    </mat-toolbar>
    <main style="padding: 1rem">
      <router-outlet></router-outlet>
    </main>

    <style>
      [routerLink] {
        cursor: pointer;
      }
      [routerLink].small {
        cursor: pointer;
        font-size: 1rem;
      }
      .active {
        font-weight: 500;
        text-decoration: underline;
      }
    </style>
  `,
})
export class App {}

```

### ✅ Final State
* Test by running `ng serve` and verify that navigating to `/patient` shows the patient list, and clicking on a patient navigates to their detail page. The active route should be highlighted in the navigation bar.

## 2. Components and pipes

### ⚛️ Concepts

* **Standalone components:** separate components that can be used independently.
* **Input** and **Output** functions: used for passing data and events between components.
* **Pipes:** used for transforming data in templates.

### 🔧 What we need to do
#### `src/app/pages/patient-list/patient-list.ts`
```typescript
import { Component } from '@angular/core';
import { MatListModule } from '@angular/material/list';
import { MatCardModule } from '@angular/material/card';
import { MatButtonModule } from '@angular/material/button';
import { MatGridListModule } from '@angular/material/grid-list';
import { DatePipe } from '@angular/common';

export type PatientListModel = {
  id: number;
  name: string;
  dob: string;
};

@Component({
  selector: 'app-patient-list',
  imports: [MatListModule, MatCardModule, MatButtonModule, MatGridListModule, DatePipe],
  templateUrl: './patient-list.html'
})
export class PatientList {
  patients: PatientListModel[] = [
    {
      id: 1,
      name: 'Alice Smith',
      dob: '1991-03-15'
    },
    {
      id: 2,
      name: 'Bob Johnson',
      dob: '1983-07-22'
    },
    {
      id: 3,
      name: 'Carol Lee',
      dob: '1996-11-05'
    },
    {
      id: 4,
      name: 'David Kim',
      dob: '1974-09-30'
    },
    {
      id: 5,
      name: 'Eva Müller',
      dob: '1988-01-12'
    },
    {
      id: 6,
      name: 'Frank O’Connor',
      dob: '1979-05-18'
    },
    {
      id: 7,
      name: 'Grace Chen',
      dob: '1997-06-25'
    },
    {
      id: 8,
      name: 'Hiro Tanaka',
      dob: '1970-12-03'
    },
    {
      id: 9,
      name: 'Isabella Rossi',
      dob: '1994-04-09'
    }
  ];
}
```
---
#### `src/app/pages/patient-list/patient-list.html`
```html
<mat-grid-list cols="4" rowHeight="160px" gutterSize="32px">
  @for (patient of patients; track patient.id) {
  <mat-grid-tile>
    <mat-card class="patient-card fill-tile">
      <mat-card-header>
        <div mat-card-avatar class="avatar-container">
          <img
            [src]="
              'https://api.dicebear.com/9.x/pixel-art/svg?seed=' + patient.name
            "
            alt="Avatar of {{ patient.name }}"
            width="40"
            height="40"
          />
        </div>
        <mat-card-title>{{ patient.name }}</mat-card-title>
        <mat-card-subtitle>ID: {{ patient.id }} | {{ patient.dob | date }}</mat-card-subtitle>
      </mat-card-header>
      <mat-card-actions>
      </mat-card-actions>
    </mat-card>
  </mat-grid-tile>
  }
</mat-grid-list>

<style>
  .patient-card.fill-tile {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
  }
</style>
```

Use the component in `app.ts`:
```typescript
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
      <app-patient-list></app-patient-list>
    </main>
  `,
})
export class App {}
```

Let's extract the patient header (with avatar, name, ID and date of birth) into a separate component.

```bash
ng generate component ui/patient-header
# ng g c ui/patient-header
```

#### `src/app/ui/patient-header/patient-header.ts`
```typescript
import { DatePipe } from '@angular/common';
import { Component, input, output } from '@angular/core';
import { MatCardModule } from '@angular/material/card';

@Component({
  selector: 'app-patient-header',
  imports: [DatePipe, MatCardModule],
  templateUrl: './patient-header.html'
})
export class PatientHeader {
  name = input.required<string>();
  id = input.required<number>();
  dob = input.required<string>();

  headerClicked = output<number>();
}
```

#### `src/app/ui/patient-header/patient-header.html`
```html
<mat-card-header (click)="headerClicked.emit(id())">
  <div mat-card-avatar>
    <img
      [src]="'https://api.dicebear.com/9.x/pixel-art/svg?seed=' + name"
      alt="Avatar of {{ name() }}"
      width="40"
      height="40"
    />
  </div>
  <mat-card-title>{{ name() }}</mat-card-title>
  <mat-card-subtitle>ID: {{ id() }} | {{ dob() | date }}</mat-card-subtitle>
</mat-card-header>
```

Now update the patient list to use the new `PatientHeader` component.

#### `src/app/pages/patient-list/patient-list.ts`
```typescript
import { Component } from '@angular/core';
import { RouterLink } from '@angular/router';
import { MatListModule } from '@angular/material/list';
import { MatCardModule } from '@angular/material/card';
import { MatButtonModule } from '@angular/material/button';
import { MatGridListModule } from '@angular/material/grid-list';
import { PatientHeader } from '../../ui/patient-header/patient-header';

export type PatientListModel = {
  id: number;
  name: string;
  dob: string;
};

@Component({
  selector: 'app-patient-list',
  imports: [RouterLink, MatListModule, MatCardModule, MatButtonModule, MatGridListModule, PatientHeader],
  templateUrl: './patient-list.html'
})
export class PatientList {
  patients: PatientListModel[] = [
    {
      id: 1,
      name: 'Alice Smith',
      dob: '1991-03-15'
    },
    {
      id: 2,
      name: 'Bob Johnson',
      dob: '1983-07-22'
    },
    {
      id: 3,
      name: 'Carol Lee',
      dob: '1996-11-05'
    },
    {
      id: 4,
      name: 'David Kim',
      dob: '1974-09-30'
    },
    {
      id: 5,
      name: 'Eva Müller',
      dob: '1988-01-12'
    },
    {
      id: 6,
      name: 'Frank O’Connor',
      dob: '1979-05-18'
    },
    {
      id: 7,
      name: 'Grace Chen',
      dob: '1997-06-25'
    },
    {
      id: 8,
      name: 'Hiro Tanaka',
      dob: '1970-12-03'
    },
    {
      id: 9,
      name: 'Isabella Rossi',
      dob: '1994-04-09'
    }
  ];

  onHeaderClicked(id: number) {
    console.log('Header clicked for patient ID:', id);
  }
}
```

#### `src/app/pages/patient-list/patient-list.html`
```html
<h2>Patient List</h2>
<mat-grid-list cols="4" rowHeight="160px" gutterSize="32px">
  @for (patient of patients; track patient.id) {
  <mat-grid-tile>
    <mat-card class="patient-card fill-tile">
      <app-patient-header
        [name]="patient.name"
        [id]="patient.id"
        [dob]="patient.dob"
        (headerClicked)="onHeaderClicked($event)"
      />
      <mat-card-actions>
        <a mat-button color="primary" [routerLink]="['/patient', patient.id]">Detail</a>
      </mat-card-actions>
    </mat-card>
  </mat-grid-tile>
  }
</mat-grid-list>

<style>
  .patient-card.fill-tile {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
  }
</style>
```
We used the `input` and `output` functions to define the properties and events for the `PatientHeader` component. The `headerClicked` event is emitted when the header is clicked, allowing the parent component to handle it.

We also used the Date pipe to format the date of birth in the `PatientHeader` component.

Let's create our own pipe to display the patient's age based on their date of birth.

```bash
ng generate pipe ui/age
# ng g p ui/age
```

#### `src/app/ui/age.pipe.ts`
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'age'
})
export class AgePipe implements PipeTransform {
  transform(value: string | Date): number {
    if (!value) return 0;
    const birthDate = new Date(value);
    const today = new Date();
    let age = today.getFullYear() - birthDate.getFullYear();
    const m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
      age--;
    }
    return age;
  }
}
```
#### `src/app/ui/patient-header/patient-header.html`
```html
...
<mat-card-subtitle>ID: {{ id() }} | {{ dob() | date }} ({{ dob() | age }} years old)</mat-card-subtitle>
...
```

### ✅ Final State
* Test by running `ng serve` and verify that the patient list displays correctly with the new `PatientHeader` component.
* Ensure that clicking on the patient header logs the patient ID to the console.
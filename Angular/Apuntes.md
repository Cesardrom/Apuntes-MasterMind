# Apuntes Angular

![Grafico sencillo distribucion](./img/overview2.png)

#### NgModules

Los NgModules decalara un contexto de aplicación, proporcionando servicios y componentes a los componentes de la aplicación.

#### Componentes

Un componente es una pieza de código que se utiliza para renderizar una parte de la interfaz de usuario y se crean con el decorador @Component.

#### Plantillas, Directivas y enlaces de datos.

Una plantilla combina html con código angular para renderizar la interfaz de usuario. Las directivas se utilizan para extender el comportamiento de las plantillas. Y los enlaces de datos se utilizan para compartir datos entre la aplicacion y el DOM. Hay dos tipos de enlaces de datos:
- Manejador de eventos : se utilizan para manejar eventos cuando se produzca la entrada del usuario con el objetivo de aplicar tu aplicacion.
- Vincular propiedades: se utilizan para interpolar valores que se calculan a traves de el DOM.

#### Servicios e inyecciones.

Para los doatos que no estan asociados a ninguna vista especifica , se utilizan los servicios. Los servicios son inyectados en los componentes mediante el decorador @Injectable. Y este decorador permite inyectar otros proovedores como dependencias de clase. Tambien existen las inyecciones de dependencia que se utilizan para inyectar servicios en los componentes.

## Modulos

Los modulos son contenedores de codigo dedicado a un dominio de la aplicacion. Pueden contener componentes servicios etc.

Cada aplicacion tiene un modulo raiz que se llama AppModule.
Las propiedades son:
- declarations: Los componentes, directivas, y pipes que pertenecen a este NgModule.

- exports: El subconjunto de declaraciones que deberían ser visibles y utilizables en las plantillas de componentes de otros NgModules.
- imports: Otros módulos cuyas clases exportadas son necesarias para las plantillas de componentes declaradas en este NgModule.

- providers: Creadores de servicios que este NgModule aporta a la colección global de servicios; se vuelven accesibles en todas las partes de la aplicación. (También puedes especificar proveedores a nivel de componente, que a menudo es preferido).

Pueden haber vistas "anidadas" permitiendo una mayor division de responsabilidades. 

![Esquema modulos](./img/view-hierarchy.png)

## Componentes.
Tu defines la logica de la aplicacion de un componente dentro de una clase y esta clase interactua con la vista a traves de una API.

Por ejemplo: _HeroListComponent tiene una propiedad heroes que contiene un array de héroes. Tu método selectHero() establece una propiedad selectedHero cuando el usuario hace clic para elegir un héroe de esa lista._

```angular
    export class HeroListComponent implements OnInit {
    heroes: Hero[];
    selectedHero: Hero;

    constructor(private service: HeroService) { }

    ngOnInit() {
        this.heroes = this.service.getHeroes();
    }

    selectHero(hero: Hero) { this.selectedHero = hero; }
    }
```

Y estos son los ejemplos de enlaces de datos desde el componente y DOM 

![Enlaces de Datos](./img/databinding.png)

Y estos casos los podemos ver reflejados en este codigo 

```angular
<li>{{hero.name}}</li>
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
<li (click)="selectHero(hero)"></li>
<input [(ngModel)]="hero.name">
```

Siendo los tres primeros muy sencillos 
_El {{hero.name}} interpolación muestra el valor de la propiedad hero.name del componente dentro del elemento._

_El \[heroe\] enlace de propiedad pasa el valor de selectedHero del padre HeroListComponent a la propiedad hero del hijo HeroDetailComponent._

_El (click) event binding llama al método selectHero del componente cuando el usuario hace clic en el nombre de un héroe._

Pero el bidireccional es mas complicado , ya que el valor de la propiedad del componente se actualiza cuando el usuario introduce un valor.

    Angular procesa todos los enlaces de datos una vez para cada ciclo de eventos de JavaScript, desde la raíz del árbol de componentes de la aplicación a través de todos los componentes secundarios.

Existen Pipes tambien que son funciones que se pueden aplicar a los datos para transformarlos antes de que se muestren en pantalla.

Tambien hay diferentes directivas; Estructulales y Atributos.
Estructurales:  modifican el diseño agregando, quitando y reemplazando elementos en el DOM. La plantilla de ejemplo utiliza dos directivas estructurales integradas para agregar lógica de aplicación a cómo se representa la vista.
    
    *ngFor es iterativo; le dice a Angular que elimine un li por héroe en la lista de heroes.
    *ngIf es un condicional; incluye el componente HeroDetail solo si existe un héroe seleccionado. 

```angular
<li *ngFor="let hero of heroes"></li>
<app-hero-detail *ngIf="selectedHero"></app-hero-detail>
```

Atributos: alteran la apariencia o el comportamiento de un elemento existente. En las plantillas, se ven como atributos HTML normales, de ahí el nombre.

_Por ejemplo el enlace de datos bidireccional es una directiva de atribitos_

## Servicios.
En Angular, los servicios son clases con un propósito específico que permiten separar la lógica de la aplicación de la presentación en los componentes. Esto promueve la modularidad y reutilización, ya que los componentes se centran en la experiencia del usuario y delegan tareas como la obtención de datos o la validación a los servicios. Angular facilita esta separación mediante la inyección de dependencia, permitiendo que los servicios sean accesibles para cualquier componente y adaptables a diferentes circunstancias.

Los servicios pueden depender de otros servicios. Por ejemplo, hay un HeroService que depende del Logger service, y también usa BackendService para obtener héroes. Ese servicio, a su vez, podría depender del servicio HttpClient para buscar héroes de forma asíncrona desde un servidor.

```angular
export class HeroService {
  private heroes: Hero[] = [];

  constructor(
    private backend: BackendService,
    private logger: Logger) { }

  getHeroes() {
    this.backend.getAll(Hero).then( (heroes: Hero[]) => {
      this.logger.log(`Fetched ${heroes.length} heroes.`);
      this.heroes.push(...heroes); // fill cache
    });
    return this.heroes;
  }
}
```

La inyección de dependencia en Angular es un mecanismo que permite a los componentes acceder a servicios y otras dependencias necesarias. Para definir un servicio, se utiliza el decorador @Injectable(), que permite a Angular inyectarlo en componentes y otras clases. El inyector es el componente central que crea y gestiona las instancias de las dependencias, reutilizándolas cuando es posible. Para que el inyector pueda crear instancias, es necesario registrar un proveedor, que generalmente es la clase del servicio. Además, las dependencias no se limitan a servicios; también pueden ser funciones o valores.

![Inyectores](./img/injector-injects.png)

## Formularios 
Angular proporciona dos enfoques para manejar formularios: formularios reactivos y formularios basados en plantillas.

  - Formularios Reactivos:
    Los formularios reactivos son más escalables y permiten un mayor control sobre la validación y el estado del formulario.

    ```angular
    import { Component, OnInit } from '@angular/core';
    import { FormBuilder, FormGroup } from '@angular/forms';
   
    @Component({
      selector: 'app-formulario',
      template: `
        <form [formGroup]="miFormulario" (ngSubmit)="enviar()">
          <input formControlName="nombre" placeholder="Nombre">
          <button type="submit">Enviar</button>
        </form>
      `
    })
    export class FormularioComponent implements OnInit {
      miFormulario: FormGroup;

      constructor(private fb: FormBuilder) {}

      ngOnInit() {
        this.miFormulario = this.fb.group({
          nombre: ['']
        });
      }

      enviar() {
        console.log(this.miFormulario.value);
      }
    }
    ```
  - Formularios basados en plantillas:
    Estos formularios tienen que seguir distintos pasos para funcionar.
    - Paso 1: Configurar el modulo
      Asegúrate de que tu módulo principal (por ejemplo, app.module.ts) importe el módulo FormsModule, que es necesario para trabajar con formularios basados en plantillas.
      ```angular
        import { NgModule } from '@angular/core';
        import { BrowserModule } from '@angular/platform-browser';
        import { FormsModule } from '@angular/forms'; // Importar FormsModule

        import { AppComponent } from './app.component';
        import { FormularioComponent } from './formulario/formulario.component';

        @NgModule({
          declarations: [
            AppComponent,
            FormularioComponent
          ],
          imports: [
            BrowserModule,
            FormsModule // Agregar FormsModule aquí
          ],
          providers: [],
          bootstrap: [AppComponent]
        })
        export class AppModule { }
      ```
    - Paso 2: Implementar el formulario basaso den plantillas
      ```Angular
        import { Component } from '@angular/core';

        @Component({
          selector: 'app-formulario',
          template: `
            <h1>Formulario Basado en Plantillas</h1>
            <form #miFormulario="ngForm" (ngSubmit)="enviar(miFormulario)">
              <div>
                <label for="nombre">Nombre:</label>
                <input id="nombre" name="nombre" ngModel required>
                <div *ngIf="miFormulario.controls.nombre?.invalid && (miFormulario.controls.nombre?.touched || miFormulario.controls.nombre?.dirty)">
                  <small *ngIf="miFormulario.controls.nombre?.errors?.required">El nombre es obligatorio.</small>
                </div>
              </div>
              <div>
                <label for="email">Email:</label>
                <input id="email" name="email" ngModel required email>
                <div *ngIf="miFormulario.controls.email?.invalid && (miFormulario.controls.email?.touched || miFormulario.controls.email?.dirty)">
                  <small *ngIf="miFormulario.controls.email?.errors?.required">El email es obligatorio.</small>
                  <small *ngIf="miFormulario.controls.email?.errors?.email">El email no es válido.</small>
                </div>
              </div>
              <button type="submit" [disabled]="miFormulario.invalid">Enviar</button>
            </form>
          `
        })
        export class FormularioComponent {
          enviar(form: any) {
            console.log('Datos del formulario:', form.value);
            // Aquí puedes manejar los datos del formulario, como enviarlos a un servicio
          }
        }
      ```
    - En resumen este ejemplo de formulario basado en plantillas en Angular muestra cómo se pueden manejar formularios de manera sencilla utilizando la directiva ngModel para la vinculación de datos y la validación de formularios. La estructura es más declarativa en comparación con los formularios reactivos, lo que puede ser más fácil de entender para formularios simples.

## Observables y promesas.

Angular utiliza Observables para manejar operaciones asíncronas, especialmente con el módulo HttpClient. Los Observables son más poderosos que las Promesas, ya que permiten múltiples valores a lo largo del tiempo y son más fáciles de cancelar.

```angular
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-observable',
  template: `
    <ul>
      <li *ngFor="let item of items">{{ item.name }}</li>
    </ul>
  `
})
export class ObservableComponent implements OnInit {
  items: any[] = [];

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.obtenerDatos().subscribe(data => {
      this.items = data;
    });
  }
}
```

## Lyfecycle Hook

Los hooks de ciclo de vida permiten a los desarrolladores ejecutar código en momentos específicos del ciclo de vida de un componente, como cuando se inicializa o se destruye.

```angular
import { Component, OnInit, OnDestroy } from '@angular/core';

@Component({
  selector: 'app-lifecycle',
  template: `<h1>Componente de Ciclo de Vida</h1>`
})
export class LifecycleComponent implements OnInit, OnDestroy {
  ngOnInit() {
    console.log('El componente se ha inicializado');
  }

  ngOnDestroy() {
    console.log('El componente se va a destruir');
  }
}
```

## Servicios HTTP

El módulo HttpClient se utiliza para realizar solicitudes HTTP. Permite manejar respuestas y errores de manera sencilla, y se basa en Observables para la gestión de datos asíncronos.

```angular
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }
}
```
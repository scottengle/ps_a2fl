# Course Work for Pluralsight Course 'Angular 2: First Look'

https://app.pluralsight.com/library/courses/angular-2-first-look

## Code Samples

All code samples for this course can be found at http://angular2-first-look.azurewebsites.net/

## Beyond the First Look

Quickstart and tutorial at http://angular.io

## Angular Architecture

### Language Concepts

* ES5
* ES6/ES2015
* TypeScript
* Dart

### AngularJS's Impact

Angular 1 is widely used and has a huge ecosystem. There are approximately 1.1 million developers using Angular 1.

### Comparing Concepts from Angular 1 to 2

7 Key Comparisons:

* Modules => Angular Modules (@NgModule)
* Controllers => Components (@Component)
* Structural Built-In Directives (ng-repeat => *ngFor, ng-if => *ngIf)
  * Structural Directives are previxed by asterisk, which indicates we're changing the structure of the DOM
* Data Binding
  * Interpolation
    * No longer need the "vm" context
  * One Way Binding
    * No longer need "ng-bind". Use square brackets
    * `[innerText] (any valid property in HTML)`
  * Event Binding
    * ng-click, ng-blur, etc. no longer necessary
    * enclose valid events in parens
      * `<button (click)="log('click')" (blur)="log('blur')">OK</button>`
  * Two Way Binding
    * ng-model no longer necessary
      * helper directive now with "Football in a Box"
      * `<input [(ngModel)]="story.name">`
* Removes the need for many directives
  * we no longer need ng-style, ng-src, ng-href
  * Since we can bind directly to valid HTML properties, we simply use `[style.visibility]`, `[src]` or `[href]`
  * Ng2 template concepts remove over 40 Ng1 Built-In Directives
* Services
  * No longer have five kinds of services (`Factories`, `Services`, `Providers`, `Constants`, `Values`)
  * Now there is only `Class`
* Dependency Injection
  * DI is different now
* Filters => Pipes
* Routing
* HTTP
* Events
* Promises

### Resources

* https://angular.io/
  * Developer Guides
    * Cheat Sheet: https://angular.io/docs/ts/latest/guide/cheatsheet.html
  * Tutorial
    * Tour of Heroes: https://angular.io/docs/ts/latest/tutorial/

## Angular Essentials: Modules, Component, Templates and Metadata

### ES Modules

Referred to simply as "modules". A module exports some asset (such as a Service). This is essentially 'destructuring'.

### Angular Modules

NgModules are specific to Angular. They help organize our app into cohesive blocks of related functionality.

NgModules are basically containers of components, services and directives that are related to one another.

Angular Modules are classes decorated by @NgModule.

Roles of NgModules:
* Import other modules
* Identify components, pipes and directives
* Export features
* Provide services to injectors
* Can be eagerly or lazily loaded

There's always, at least, one module in our application (the root module).

    @NgModule ({
      imports: [
        BrowserModule,
        FormsModule
      ],
      declarations: [
        VehiclesComponent
      ],
      providers: [
        VehicleService
      ],
      bootstrap: [VehiclesComponent]
    })
    export class AppModule { }

### Components

A component contains app logic that controls a region of the UI that we call a view.

Anatomy:

    // Imports (use other modules)

    import { Component } from '@angular/core';

    import { Vehicle } from './vehicle.service';

    // Metadata (describe the component)

    @Component({
      moduleId: module.id,
      selector: 'story-vehicles',
      templateUrl: 'vehicles.component.html'
    })

    // Class (define the component)

    export class VehicleListComponent {
      vehicles: Vehicle[];
    }

### Bootstrapping

Assemble the app from components. Bootstrap the starting point.

By convention we create main.ts:

    import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

    import { AppModule } from './app/app.module';

    platformBrowserDynamic().bootstrapModule(AppModule);

### Templates

Templates are basically the 'view'. Templates are connected to Components using either the `template` property or `templateUrl` property.

Most often, we use `templateUrl`.

We can create a hierarchical tree of components.

### Metadata

The metadata is always present and goes inside the decorators. One way we can use it is to tell a Component where the template is.

We declare components, directives and pipes in an Angular Module.

Prefer registering providers in Angular Modules instead of Angular Components.

Components can have `@Output()` and `@Input()` parameters that allow a Component to interact with code hosting it.

### @Output and @Input Decorators

`@Output()` decorators allow the Component to emit data to the outside through `EventEmitter<>`s.

These decorators are actually functions.

### ViewChild

The `@ViewChild()` decorator lets parent Components call methods on child Components.

## Data Binding

Use data binding to coordinate communication between a Component and its Template.

`[(ngModel)]` is a special directive that gives us two-way data binding.

In Angular, change detection is done based on unidirectional data flow.

Benefits:

* Easier widget integration
* No more $apply
* No more repeated digest cycles
* No more watchers
* No more performance issues with digest cycle and watcher limits

### Interpolation

Interpolation evaluates an expression between double curly braces.

Angular has a `Safe Navigation Operator` in interpolated references, a.k.a. the `Elvis Operator`, which supresses errors when the property isn't available. `?.`

#### JSON Pipes

You can use this to see what properties are available on an object:

    <pre>
      { myObject | json }
    </pre>

### Property Binding (one-way)

Use `[]` to send values from the Component to the Template. Alternatively use `bind-` assignment. 

We set properties and events of DOM elements, not attributes.

    {{expression}}
    [target] = "expression"
    bind-target = "expression"

For attributes, use `attr.`

    <button [attr.aria-label]="ok">ok</button> // Attribute binding

    <div [class.isStopped]="isStopped">Stopped</div> // Class property binding

    <button [style.color]="isStopped" ? 'red' : 'blue'"> // Style property binding

When you want to specify a value, enclose the value in single quotes.

    <div [style.background]="'#EEE'">    

### Event Bindings (one-way)

Use `()` to send events from the template to the Component. Alternatively use the `on-` assignment.

    (target) = "statement"
    on-target = "statement"

Event Binding Examples:

    <button (click)="save()">Save</button>

    <vehicle-detail (changed)="vehicleChanged()"></vehicle-detail>

Pass messages:

    <input [value]="vehicle.name" (input)="vehicle.name=$event.target.value">

    // then in the Component

    @Input() vehicle: Vehicle;
    @Output() onChanged = new EventEmitter<Vehicle>();
    changed() { this.onChanged.emit(this.vehicle); }

You can bind to any event that an html element exposes, or ones we create.

### Two Way Binding

Use `[()]` (football in a box) to enable two-way binding between the Component and the Template.

The `ngModel` directive comes with the FormsModule in Angular.

    [(ngModel)]="expression"
    bindon-ngModel="expression" // alternative syntax

Using `ngModel`:

    import { NgModule } from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';

    @NgModule({
      imports: [
        BrowserModule,
        FormsModule
      ]
    })
    // ...

### Built-In Directives

When Angular renders templates, it transforms the DOM according to instructions from Directives.

* `[ngClass]`
* `[ngStyle]`

It is easier to set multiple styles with ngStyle:

    <div [ngStyle]="setStyles()">{{vehicle.name}}</div>

The same for classes

    <div [ngClass]="setClasses()">{{vehicle.name}}</div>

Structural Directives

* `*ngFor`
* `*ngIf`
* `*ngSwitch`

All of these built-ins are included in the CommonModule which is included in the BrowserModule.

### Pipes

Pipes allow us to transform data for display in a Template. AngularJS Filters are similar.

String Pipes

  * uppercase
  * lowercase

Date Pipes

  * format
  * expression

Numeric Pipes

  * currency
  * percent
  * number

Async Pipes subscribes to a Promise or Observable, returning the latest value emitted.

#### Custom Pipes

You can create your own pipes using the `@Pipe` decorator.

    import { Pipe, PipeTransform } from '@angular/core';
    
    @Pipe({ name: 'initCaps' })
    export class InitCapsPipe implements PipeTransform {
      transform(value: string, args?: any[]) { // 'args' is optional
        return value.toLowerCase()
          .replace(/(?:^|\s)[a-z]/g, m => m.toUpperCase());
      }
    }

## Services, Dependency Injection, and Component Lifecycle Hooks

### Services

Services provide anything our application needs. It often shares data or functions between other Angular features.

Services are just classes in Angular.

    import { Injectable } from '@angular/core';

    @Injectable()
    export class VehicleService {
      getVehicles() {
        return [
          new Vehicle(10, 'Millenium Falcon'),
          new Vehicle(12, 'X-Wing Fighter'),
          new Vehicle(14, 'TIE Fighter')
        ]
      }
    }

### Dependency Injection

Dependency Injection is how we provide an instance of a class to another Angular feature.

    export class VehicleListComponent {
      vehicles: Vehicle[];

      constructor( private vehicleService: VehicleService ) { }
    }

Injecting a service into another service:

    // @Injectable lets Angular know that the service may have things that can be injected into it
    
    @Injectable() 
    export class VehicleService {
      constructor( private http: Http) { } // <= injecting http service

      getVehicles() {
        return this.http.get(vehiclesUrl)
          .map((res: Response)) => res.json().data);
      }
    }

The general rule of thumb is that all services have the `@Injectable` decorator added to it.

### Injectors

When we inject a service, Angular searches the appropriate injectors for it.

There is one injector for the application root, and a hierarchical DI system with a tree of injectors that parallel an applications component tree.

Register a provider in the Component providers array only when the provider must be hidden from Components outside of the component's injector tree. Generally speaking, it is better to register providers in the AppModule provider array. Provide a service once if you want a singleton.

Angular will look for and provide providers in a prototypal way, by looking at the component and then further up the component tree, all the way to the AppModule.

### Component Lifecycle Hooks

Lifecycle Hooks allow us to tap into specific moments in the application lifecycle to perform logic.

Specifying that a component implements a particular lifecycle hook using the interface provides compile-time safety.

Common Component Lifecycle Hooks

1. ngOnInit
2. ngOnChanges
3. ngAfterViewInit
4. ngOnChanges
5. ngOnDestroy

Example:

    import { Component, EventEmitter, Input, Output, OnChanges, OnInit, AfterViewInit, OnDestroy } from '@angular/core';

    @Component({
      // ...
    })
    export class CharacterComponent implements OnChanges, OnInit, AfterViewInit, OnDestroy {
      @Input() character: Character

      ngAfterViewInit() {
        console.log(`ngAfterViewInit for ${this.character.name}`);
      }

      ngOnChanges() {
        console.log(`ngOnChanges for ${this.character.name}`);
      }

      ngOnDestroy() {
        console.log(`ngOnDestroy for ${this.character.name}`);
      }

      ngOnInit() {
        console.log(`ngOnInit for ${this.character.name}`);
      }
    }

## Data with HTTP

### HTTP

We use HTTP to get and save data with Promises or Observables. We isolate the HTTP calls in a shared Service.

HTTP Requirements

* import the module
    
    import { HttpModule } from '@angular/http';`
    @NgModule({
      imports: [HttpModule]
    })
    export class AppModule { }

* Define a Service

    @Injectable()
    export class VehicleService {
      constructor( private http: Http) { }

      getVehicles() {
        return this.http.get('api/vehicles')
          .map((response: Response) => <Vehicle[]>response.json().data)
          .catch(this.handleError);
      }

      private handleError(error: Response) {
        console.error(error);
        return Observable.throw(error.json().error || 'Server error');
      }
    }

* Subscribe to the Observable

    constructor(private vehicleService: VehicleService) { }
    getHeroes() {
      this.vehicleService.getVehicles()
        .subscribe(
          vehicles => this.vehicles = vehicles,
          error => this.errorMessage = <any>error
        );
    }
    ngOnInit() { this.getHeroes(); }

### RxJs

Reactive JS implements the asynchronous observable pattern and is widely used in Angular 2.

For production, to minimize code bloat, only import parts of RxJs that you are using.

One approach is to create a separate import file:

    // in app.module.ts

    import './rxjs-extensions';

    // in rxjs-extensions.ts

    import 'rxjs/add/operator/catch';
    import 'rxjs/add/operator/do';
    import 'rxjs/add/operator/map';
    import 'rxjs/add/operator/toPromise';

RxJs is an eventing system. You can define an observable that can be subscribed to by other components.

### Async Pipe

The Async Pipe receives a Promise or Observable as input and subscribes to the input, eventually emitting the value(s) as changes arrive.

Async Pipes simplify components. The observables can be sent back to the screen.

    // in vehicle-list.component.ts

    export class VehicleListComponent {
      vehicles: Observable<Vehicle[]>;    // property becomes an Observable
      constructor(private vehicleService: VehicleService) { }
      getVehicles() {
        this.vehicles = this.vehicleService.getVehicles();    // set the Observable from the service
      }
    }

    // in vehicle-list.component.html

    <ul>
      <li *ngFor="let vehicle of vehicles | async">    // subscribes to the vehicles Observable
        {{ vehicle.name }}
      </li>
    </ul>

The Async Pipe is self-cleaning. It will automatically unsubscribe from the Observable when the template goes away.

The Async Pipe can handle both Promises and Observables.

## Routing

Don't forget to set your base URL: https://www.w3schools.com/tags/tag_base.asp

### Routing Essentials

Routing allows our application to navigate between different Components, passing parameters where needed.

Steps to routing:

* Import RouterModule from @angular/router
* Define the routes
* Define a module
* Export the component list
* Declare a <router-outlet>
* Add [routerLink] bindings

Example:

    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router';

    const routes: Routes = [
      { path: '', pathMatch: 'full', redirectTo: 'characters', },
      { path: 'characters', component: CharacterListComponent },
      { path: 'characters/:id', component: CharacterComponent },
      { path: '**', pathMatch: 'full', component: PageNotFoundComponent }
    ];

    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule]
    })
    export class AppRoutingModule { }

    export const routableComponents = [
      CharacterListComponent,
      CharacterComponent,
      PageNotFoundComponent
    ]

We define a Routing Module and import it into the App Root or Feature Module.

    // In app.module.ts

    import { AppComponent } from './app.component';
    import { AppRoutingModule, routableComponents } from './app-routing.module';

    @NgModule({
      imports: [BrowserModule, AppRoutingModule],
      declarations: [AppComponent, routableComponents],
      bootstrap: [AppComponent]
    })
    export class AppModule { }

Add routing to the templates by using the RouterLink directive and define link parameters.

    <nav>
      <ul>
        <li><a [routerLink]="['/characters']" href="">Characters</a></li>
        <li><a [routerLink]="['/vehicles']" href="">Vehicles</a></li>
      </ul>
    </nav>

`ng-view` has been replaced by `<router-outlet></router-outlet>`.

### Routing Parameters

You can define routing parameters using the `:`

    const routes: Routes = [
      { path: '', pathMatch: 'full', redirectTo: 'characters', },
      { path: 'characters', component: CharacterListComponent },
      { path: 'characters/:id', component: CharacterComponent },          // <= route parameter
      { path: '**', pathMatch: 'full', component: PageNotFoundComponent }
    ];

You can pass data through routing with a few different ways:

* Snapshot
  * Easiest, as long as parameter values do not change
* Observable
  * Gets new parameter values when component is re-used
* Resolvers
  * Gets data before a component is loaded

Snapshot Method

    export class SessionComponent implements OnInit {
      private id: any;

      constructor( private route: ActivatedRoute ) { }

      ngOnInit() {
        this.id = Number.parseInt(this.route.snapshot.params['id']);
        this.getSession();
      }
    }

Observable Method

    export class SessionComponent implements OnInit {
      private id: any;

      constructor( private route: ActivatedRoute ) { }

      ngOnInit() {
        this.route.params.map(params => params['id'])
          .do(id => this.id = Number.parseInt(id))
          .subscribe(id => this.getSession());
      }
    }

### Routing Resolvers

Route resolvers are useful when you want to fetch data before navigating to a component.

    @Injectable()
    export class VehicleResolver implements Resolve<Vehicle> {
      constructor(
        private vehicleService: VehicleService,
        private router: Router
      ) { }

      resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
        let id = +route.params['id'];
        return this.vehicleService.getVehicle(id)
          .map(vehicle => vehicle ? vehicle : new Vehicle())
          .catch((error: any) => {
            console.log(`${error}. Headinb back to vehicle list`);
            this.router.navigate(['/vehicles']);
            return Observable.of(null);
          });
      }
    }

In the routing module:

    {
      path: 'vehicles/:id',
      component: VehicleComponent,
      resolve: {
        vehicle: VehicleResolver
      }
    },

You can obtain the resolved data in your component:

    this.route.data.subscribe((data: { vehicle: Vehicle }) => this.vehicle = data.vehicle);

### Routing Guards

Routing guards allow us to make a decision at key points in the routing lifecycle and either continue, abort or take a new direction.

Types of Router Guards:

* Resolve
  * Pre-fetch data before navigating
* CanActivate
  * Are we allowed to navigate to a component?
* CanActivateChild
  * Same as CanActivate, but for child routes
* CanDeactivate
  * Are we allowed to navigate away from a component?
* CanLoad
  * Prevents loading of the templates and JavaScript until the guard has been satisfied

#### CanActivate

Often used for authentication.

    @Injectable()
    export class AuthGuard implements CanActivate {
      constructor( private userProfileService: UserProfileService, private router: Router ) { }

      canActivate(next: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
        if (this.userProfileService.isLoggedIn) {
          return true;
        }
        this.router.navigate(['/login'], { queryParams: { redirectTo: state.url } });
        return false;
      }
    }

    // In app-routing.module.ts

    { 
      path: 'dashboard',
      component: DashboardComponent,
      canActivate: [AuthGuard] // <= this is an array
    },

### Child Routes

    const routes: Routes = [
      { path: '', pathMatch: 'full', redirectTo: 'characters', },
      { path: 'login', component: LoginComponent },
      {
        path: 'characters',
        component: CharactersComponent,
        canActivate: [CanActivateAuthGuard],
        children: [
          { path: '', component: CharacterListComponent },
          { path: ':id', component: CharacterComponent }
        ]
      },
    ];

## Angular Modules In Depth

Angular modules help organize our apps into cohesive blocks of functionality.

### Types of Feature Modules

* Domain
* Routed
* Service
* Widget

### Eagerly Loading Modules

Eager loading is when we load a feature module at start up, with the root module.

Ideal for components that are needed at startup.

    // in characters-router.module.ts

    const routes: Routes = [
      {
        path: 'characters',
        component: CharactersComponent,
        children: [
          { path: '', component: CharacterListComponent },
          { path: ':id', component: CharacterComponent },
        ]
      }
    ];

    @NgModule({
      imports: [RouterModule.forChild(routes)],
      exports: [RouterModule]
    })
    export class CharactersRouterModule { }

    export const routedComponents = [
      CharactersComponent,
      CharacterListComponent,
      CharacterComponent
    ];

    // in app.module.ts

    @NgModule({
      imports: [
        BrowserModule,
        FormsModule,
        HttpModule,
        CharactersModule,   // <= this is where the Characters routing module is imported
        AppRoutingModule    // <= this is listed last because it has a `**` route
      ]
    })
    export class AppModule { }

Order matters! If you have a catch-all route (or `**` route), make sure it is loaded last.

### Lazily Loaded Modules

Loading modules on demand, as needed, just in time. Lowers the initial payload, improving the app startup experience.

    // in app-routing.module.ts

    const routes: Routes = [
      { path: '', pathMatch: 'full', redirectTo: 'characters' },
      { path: 'characters', loadChildren: 'app/characters/characters.module#CharactersModule' },
      { path: 'vehicles', loadChildren: 'app/vehicles/vehicles.module#VehiclesModule' },
      { path: '**', pathMatch: 'full', component: PageNotFoundComponent }
    ];

### Preload Strategies

When a user navigates to a lazily loadable module, the content has to be fetched from the server, which may cause a delay.

The router can preload lazily loadable modules in the background.

    // in app-routing.module.ts

    import { NgModule } from '@angular/core';
    import { PreloadAllModules, NoPreloading, Routes, RouterModule } from '@angular/router';

    @NgModule({
      imports: [RouterModule.forRoot(routes, { preloadStrategy: PreloadAllModules })],
      exports: [RouterModule],
    })
    export class AppRoutingModule { }

### Custom Preload Strategies

You can create custom preload strategies.

    // in preload-strategy.ts

    export class PreloadSelectedModulesList implements PreloadingStrategy {
      preload(route: Route, load: Function): Observable<any> {
        return route.data && route.data['preload'] ? load() : of(null);
      }
    }

    // in app-routing.module.ts

    {
      path: 'speakers', loadChildren: 'app/speakers/speakers.module*SpeakersModule',
      data: { preload: true }
    },

    // in app-routing.module.ts

    @NgModule({
      imports: [RouterModule.forRoot(routes, { preloadingStrategy: PreloadSelectedModulesList })],
      exports: [RouterModule],
      providers: [PreloadSelectedModulesList]
    })
    export class AppRoutingModule { }

### Feature Modules

The four types of feature moduels:

* Domain
  * Modules imported into the root AppModule, typically without routing
* Routed
  * Routed feature modules are Domain feature modules that are the targets of navigation routes
* Service
  * Service feature modules contain singleton services used by anyone across the app
  * Generally, all providers, no components
  * Imported once in app root module
  * don't import core module into other modules
* Widget (i.e. shared modules)
  * shared feature modules generally contain components, directives and pipes
  * Import these in every module that uses them

Viewed another way:

    Type      Declarations  Providers  Exports  Imported By       Examples
    ----------------------------------------------------------------------------------------
    Domain    √             Rare       Some     Feature           MenuModule
    Routed    √             Rare       x        Nobody if Lazy    CharactersModule
    Service   x             √          x        AppModule         CoreModule, HttpModule
    Widget    √             Rare       √        Feature           CommonModule, SharedModule

### Provider Tips

It is important to consider how we want our providers to behave in the module system.

#### Providing Services and Their Behavior

A service provided in a component, is accessible to that component and to its children. Good examples: login component, login service

When we import a module with providers, we'll get a new instance of that service upon injection

Be careful putting providers in a module that can be imported more than once
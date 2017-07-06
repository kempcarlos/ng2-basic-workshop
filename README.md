# ng2-basic-workshop
Angular2 Workshop (2 hours + 1 hour QA)

## 1. Install
- install [node](https://nodejs.org/en/)

- install Angular CLI

    `sudo npm install -g @angular/cli`

## 2. Create the project with Angular CLI
```
ng new quick-app
cd quick-app
```

## 3. Launch the project with ng serve
```
ng serve
```

Open: [http://localhost:4200]()

## 4. Create Page Components with Angular CLI
```
ng g component about
ng g component home
ng g component todo-list
```

## 5. Create the Routes
- Create Routes File `app.routes.ts`

	```
	import { Routes } from '@angular/router';
	
	import { HomeComponent } from './home/home.component';
	import { AboutComponent } from './about/about.component';
	import { TodoListComponent } from './todo-list/todo-list.component';
	
	export const routes: Routes = [
	  { path: '', component: HomeComponent },
	  { path: 'about', component: AboutComponent },
	  { path: 'todo-list', component: TodoListComponent },
	  { path: '**',   redirectTo: '', pathMatch: 'full' }
	];
	```

 - Import Route Files in `app.module.ts`

	```
	(...)
	import { RouterModule } from '@angular/router';
	
	import { routes } from './app.routes';
	(...)
	  imports: [
	    (...)
	    RouterModule.forRoot(routes)
	  ],
	(...)
	```

 - Insert the router outlet in `app.component.html`

	```
	<router-outlet></router-outlet>
	```

- Also add a navigation bar to `app.component.html`

	```
	<nav>
	  <a routerLink="/">Home</a>
	  <a routerLink="/about">About</a>
	  <a routerLink="/todo-list">Todo List</a>
	</nav>
	```

## 6. Create the To-do List Page
- Create a list in `todo-list.component.ts`:
	
	```
	export class TodoListComponent implements OnInit {
	
	  list: Array<any> = [
	    { name: 'clean room', done: false },
	    { name: 'make pancakes', done: false },
	    { name: 'spend 3 hours on reddit', done: true }
	  ];
	
	(...)
	```

- Show it in the HTML in `todo-list.component.html`
 
	```
	<ul>
	  <li *ngFor="let task of list">
	    <input type="checkbox" [(ngModel)]="task.done"/>
	    {{task.name}}
	  </li>
	</ul>
	```

- Allow adding new TODOs
   - Create **newTask** string  in `todo-list.component.ts`:
   
		```
		export class TodoListComponent implements OnInit {
		
			newTask: String = '';

		(...)
		```
   - Create an **addTask** Function in `todo-list.component.ts`:
   
		```
		(...)
		  ngOnInit() {
		  }
		
		  addTask(){
		    this.list.push({
		      name: this.newTask,
		      done: false
		    });

		    this.newTask = '';
		  }
       (...)
		```

   - Create the Add Task HTML in `todo-list.component.html`
   
		```
		<form>
		  <input [(ngModel)]="newTask" name="newTask"/>
		  <button (click)="addTask()" type="submit">Add Task</button>
		</form>
		```
        
## Wait a minute... Let's go back to Architecture overview.

![architecture overview](https://angular.io/generated/images/guide/architecture/overview2.png)
### 1.	Metadata(intro): https://angular.io/guide/architecture#metadata
-	Metadata tells Angular how to process a class(…)

### 2.	Modules: https://angular.io/guide/architecture#modules
> Go to app.module.ts and talk about modules metadata

NgModules help organize an application into cohesive blocks of functionality.

- Angular module is a class adorned with a decorator **@NgModule**;
- **Metadata** describes the module behavior:
    - How to compile and run the module code;
    - Its own components, directives and pipes;
- **Metadata** properties allowed for **NgModules**:
    - **imports**: used to specify other Angular modules. Modules classes decorated with @NgModule
    https://angular.io/guide/bootstrapping#the-imports-array
  
    - **declarations**: used to specify the components that compose the module.
    https://angular.io/guide/bootstrapping#the-declarations-array

    - **bootstrap**: used to specify the main application view, i.e., root component. Only root module should set this property.
    https://angular.io/guide/bootstrapping#the-bootstrap-array

    - **exports**: used to expose any declarable class - component, directives and pipes - making it public classes to other modules.
    https://angular.io/guide/architecture#modules

    - **providers**:  used to expose a global collection of services, accessible across all parts of the app. 
    https://angular.io/guide/architecture#modules
- A module can be defined as public for external usage, i.e., exported; 

[Reference to NgModule official doc](https://angular.io/guide/ngmodule#ngmodules)

### 3.	Angular Libraries: https://angular.io/guide/architecture#angular-libraries
> Talk about imports declared in app.module.ts

- Angular ships as a collection of JavaScript modules. You can think of them as library modules.
- Each Angular library name begins with the **@angular** prefix.
- You install them with the **npm** package manager and import parts of them with JavaScript **import** statements.
- For example, import Angular's **Component** decorator from the **@angular/core** library like this:
```
import { Component } from '@angular/core';
```

### 4.	Components: https://angular.io/guide/architecture#components
> Components are the most basic building block of an UI in an Angular application.

**Component decorator** allows you to mark a class as an Angular component and provide additional metadata that determines how the component should be processed, instantiated and used at runtime.

- For example, **todo-list.component.ts** uses **selector**, **templateUrl** and **styleUrls** metadata properties and the lifecycle hook **OnInit**.
```
@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
  styleUrls: ['./todo-list.component.css']
})
export class TodoListComponent implements OnInit {

(...)

}
```

- **List of Metadata Properties:**  https://angular.io/api/core/Component#description

- **Lifecycle Hooks**: Lifecycle hooks provide us with an easy way of invoking operation based on the lifecycle of our components.
https://angular.io/guide/lifecycle-hooks

### 5. Templates & Data Binding: https://angular.io/guide/architecture#templates
> Go to todo-list.component.html talk about the template syntax

Example 1:
```
<ul>
  <li *ngFor="let task of list">
    <input type="checkbox" [(ngModel)]="task.done"/>
    {{task.name}}
  </li>
</ul>
<form>
  <input [(ngModel)]="newTask" name="newTask"/>
  <button (click)="addTask()" type="submit">Add Task</button>
</form>
```

Example 2:
```
<ul>
  <li *ngFor="let task of list">
    <app-todo-item [task]="task"></app-todo-item>
  </li>
</ul>
<form>
  <input [(ngModel)]="newTask" name="newTask"/>
  <button (click)="addTask()" type="submit">Add Task</button>
</form>
```

#### 5.1. Data binding: https://angular.io/guide/architecture#data-binding

- The `{{task.name}}` interpolation displays the component's `task.name` property value within the `<li>` element.

- The `[task]` property binding passes the value of `task` from the parent `TodoListComponent` to the `task` property of the child `TodoItemComponent`.

- The `(click)` event binding calls the component's `addTask` method when the user clicks the button `Add Task`.

- Two-way data binding is an important fourth form that combines property and event binding in a single notation, using the `ngModel` directive. Here's an example from the TodoListComponent template:

```
<input [(ngModel)]="newTask" name="newTask"/>
```

- In two-way binding, a data property value flows to the input box from the component as with property binding. The user's changes also flow back to the component, resetting the property to the latest value, as with event binding.

- Angular processes all data bindings once per JavaScript event cycle, from the root of the application component tree through all child components.

![component-databinding](https://angular.io/generated/images/guide/architecture/component-databinding.png)

Data binding plays an important role in communication between a template and its component.

![parent-child-binding](https://angular.io/generated/images/guide/architecture/parent-child-binding.png)

Data binding is also important for communication between parent and child components.

#### 5.2. Directives?: https://angular.io/guide/architecture#directives
> *ngFor, etc...

_Angular templates are dynamic_. When Angular renders them, it transforms the DOM according to the instructions given by **directives**.

A `directive` is a class with a `@Directive` decorator.

There are three kinds of directives in Angular:

**Components—directives** - A component is a `directive-with-a-template`. A `@Component` decorator is actually a `@Directive` decorator **extended** with template-oriented features.

**Structural directives** - change the structure of the view(DOM layout) by adding and removing DOM elements. Two examples of _built-in structural directives_ are `NgFor` and `NgIf`.

`*ngFor` tells Angular to stamp out one `<li>` per task in the task list.

`*ngIf` includes the task component only if a task is not done.

Example:

```
<li *ngFor="let task of list">
  <app-todo-item *ngIf="!task.done" [task]="task"></app-todo-item>
</li>
```

**Attribute directives** — change the appearance or behavior of an element, component, or another directive.

_Attribute directives_ listen to and modify the behavior of other HTML elements, attributes, properties, and components. They are usually applied to elements as if they were HTML attributes, hence the name. Three most commonly used  _built-in attribute directives_ are `NgClass`, `NgStyle` and `NgModel`.

`NgClass` - add and remove a set of CSS classes

`NgStyle` - add and remove a set of HTML styles

`NgModel` - two-way data binding to an HTML form element back to top

https://angular.io/guide/attribute-directives#attribute-directives

### 6. Services: https://angular.io/guide/architecture#services

- Almost anything can be a service. A service is typically a class with a narrow, well-defined purpose. It should do something specific and do it well.

- There is nothing specifically Angular about services. Angular has no definition of a service. There is no service base class, and no place to register a service.

- Yet services are fundamental to any Angular application. Components are big consumers of services.

#### 6.1. Create a service class that fetch's the list of task's

```
export class TodoListService {
  getTasks(){
     return Promise.resolve([
	    { name: 'clean room', done: false },
	    { name: 'make pancakes', done: false },
	    { name: 'spend 3 hours on reddit', done: true }
	  ]);
  }
}
```

#### 6.2 Create another service class that logs to the browser console:

```
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```

- Services are everywhere. Component classes should be lean. They don't fetch data from the server, validate user input, or log directly to the console. They delegate such tasks to services.


### 7. Dependency injection: https://angular.io/guide/architecture#dependency-injection

Dependency injection is a way to supply a new instance of a class with the fully-formed dependencies it requires. 

Angular uses dependency injection to provide new components with the services they need.

Angular can tell which services a component needs by looking at the types of its constructor parameters. 

For example, the constructor of your `TodoListComponent` needs a `TodoListService`:

```
constructor(private service: TodoListService) { }
```

When Angular creates a component, it first asks an injector for the services that the component requires.

**How dependency injection works?**
An `injector` maintains a container of service instances that it has previously created. If a requested service instance is not in the container, the injector makes one and adds it to the container before returning the service to Angular. When all requested services have been resolved and returned, Angular can call the component's constructor with those services as arguments. **This is dependency injection.**

If the injector doesn't have a `TodoListService`, how does it know how to make one?

In brief, you must have previously registered a provider of the `TodoListService` with the injector. A provider is something that can create or return a service, typically the service class itself.

You can register providers in modules or in components.


- Add the `TodoListervice` to the array of `providers` of the root module in the file `src/app/app.module.ts`.

```
providers: [
  (...)
  TodoListervice,
  (...)
],
```

This way the same instance of a service is available everywhere.

- Alternatively, register at a component level in the `providers` property of the `@Component` metadata, in file `src/app/todo-list.component.ts`:

```
@Component({
  selector:    'app-todo-list',
  templateUrl: './todo-list.component.html',
  providers:  [ TodoListService ]
})
```

Registering at a component level means you get a new instance of the service with each new instance of that component.

#### 7.1. Why @Injectable()?
> LoggerService


### 9. Angular Internals
- **Change Detection**: https://blog.thoughtram.io/angular/2016/02/22/angular-2-change-detection-explained.html

### 8. Do's and Dont's
_I think an interesting topic would be what we do in Angular 1 that we would like to do in Angular 2 but shouldn't_



# 5. Create the Todo List Page
# 5.1 - Basic Concepts
# Angular Module

NgModules help organize an application into cohesive blocks of functionality.

~~(bullets) An NgModule is a class adorned with the @NgModule decorator function. @NgModule takes a metadata object that tells Angular how to compile and run module code. It identifies the module's own components, directives, and pipes, making some of them public so external components can use them.~~ 
- Angular module is a class adorned with a decorator **@NgModule**;
- **Metadata** describes the module behavior:
    - How to compile and run the module code;
    - Its own components, directives and pipes;
- **Metadata** properties allowed for **NgModules**:
    - **imports**: used to specify other Angular modules. Modules classes decorated with @NgModule
    ~~It tells Angular about specific other Angular modules — all of them classes decorated with @NgModule — that the application needs to function properly~~ https://angular.io/guide/bootstrapping#the-imports-array
  
    - **declarations**: used to specify the components that compose the module.
    ~~tell Angular which components belong to the AppModule.~~ https://angular.io/guide/bootstrapping#the-declarations-array

    - **bootstrap**: used to specify the main application view, i.e., root component. Only root module should set this property.
    ~~launch the application by bootstrapping the root AppModule.~~ https://angular.io/guide/bootstrapping#the-bootstrap-array

    - **exports**: used to expose any declarable class - component, directives and pipes - making it public classes to other modules.
    ~~the subset of declarations that should be visible and usable in the component templates of other modules.~~ https://angular.io/guide/architecture#modules

    - **providers**:  used to expose a global collection of services, accessible across all parts of the app. 
    ~~creators of services that this module contributes to the global collection of services; they become accessible in all parts of the app.~~ https://angular.io/guide/architecture#modules
- A module can be defined as public for external usage, i.e., exported; 

[Reference to NgModule official doc](https://angular.io/guide/ngmodule#ngmodules)
      
## Components
   - Explain each property in the @Component annotation
## Injectables(Services)
## Views
5.2. - One way vs Two way binding
5.3. - New Syntax
5.4. - Lifecycle Hooks

7. Convert the Todo List Page into Component Architecture
- @Input()
- (click)=“addTask_()”
- EventEmitter


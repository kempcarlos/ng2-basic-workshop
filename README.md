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

### 6. Services: https://angular.io/guide/architecture#services
> Create a Service to retrieve the list of To-do’s.

### 7. Dependency injection: https://angular.io/guide/architecture#dependency-injection
> Convert the Service into an injectable.







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


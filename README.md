# ng2-basic-workshop
Angular2 Workshop (2 hour + 1h QA)

1. Install

2. Bootstrap project with Angular CLI

3. Create Component

4. Create Routes

5. Create the Todo List Page
5.1 - Basic Concepts
- Modules

NgModules help organize an application into cohesive blocks of functionality.

(bullets) An NgModule is a class adorned with the @NgModule decorator function. @NgModule takes a metadata object that tells Angular how to compile and run module code. It identifies the module's own components, directives, and pipes, making some of them public so external components can use them.   
Reference: https://angular.io/guide/ngmodule#ngmodules

    - imports: It tells Angular about specific other Angular modules — all of them classes decorated with @NgModule — that the application needs to function properly. https://angular.io/guide/bootstrapping#the-imports-array

    - declarations: tell Angular which components belong to the AppModule. https://angular.io/guide/bootstrapping#the-declarations-array

    - bootstrap: launch the application by bootstrapping the root AppModule. https://angular.io/guide/bootstrapping#the-bootstrap-array

    - exports: the main application view, called the root component, that hosts all other app views. Only the root module should set this bootstrap property https://angular.io/guide/architecture#modules

    - providers:  creators of services that this module contributes to the global collection of services; they become accessible in all parts of the app. https://angular.io/guide/architecture#modules
      
- Components
   - Explain each property in the @Component annotation
- Injectables(Services)
- Views
5.2. - One way vs Two way binding
5.3. - New Syntax
5.4. - Lifecycle Hooks

7. Convert the Todo List Page into Component Architecture
- @Input()
- (click)=“addTask_()”
- EventEmitter


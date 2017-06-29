# ng2-basic-workshop
Angular2 Workshop (2 hour + 1h QA)

1. Install

2. Bootstrap project with Angular CLI

3. Create Component

4. Create Routes

5. Create the Todo List Page
5.1 - Basic Concepts
- Angular Module

NgModules help organize an application into cohesive blocks of functionality.

~~(bullets) An NgModule is a class adorned with the @NgModule decorator function. @NgModule takes a metadata object that tells Angular how to compile and run module code. It identifies the module's own components, directives, and pipes, making some of them public so external components can use them.~~ 
- Angular module is a class adorned with a decorator **@NgModule**;
- **Metadata** describes the module behavior:
    - How to compile and run the module code;
    - Its own components, directives and pipes;
- Can be defined as public for external usage, i.e., exported; 
Reference: https://angular.io/guide/ngmodule#ngmodules

- **Metadata** properties allowed for **NgModules**:
    - imports: used to specify other Angular modules. Modules classes decorated with @NgModule
    ~~It tells Angular about specific other Angular modules — all of them classes decorated with @NgModule — that the application needs to function properly~~ https://angular.io/guide/bootstrapping#the-imports-array
  
    - declarations: used to specify the components that compose the module.
    ~~tell Angular which components belong to the AppModule.~~ https://angular.io/guide/bootstrapping#the-declarations-array

    - bootstrap: used to specify the main application view, i.e., root component. Only root module should set this property.
    ~~launch the application by bootstrapping the root AppModule.~~ https://angular.io/guide/bootstrapping#the-bootstrap-array

    - exports: used to expose any declarable class - component, directives and pipes - making it public classes to other modules.
    ~~the subset of declarations that should be visible and usable in the component templates of other modules.~~ https://angular.io/guide/architecture#modules

    - providers:  used to expose a global collection of services, accessible across all parts of the app. 
    ~~creators of services that this module contributes to the global collection of services; they become accessible in all parts of the app.~~ https://angular.io/guide/architecture#modules
      
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


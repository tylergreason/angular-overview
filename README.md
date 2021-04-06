- [Resources](#resources)
  - [VS Code Extensions:](#vs-code-extensions)
- [Components](#components)
- [Templates](#templates)
    - [Expressions in templates](#expressions-in-templates)
    - [Structural Directives](#structural-directives)
    - [Data Binding](#data-binding)
- [Modules](#modules)
    - [Metadata](#metadata)
    - [Feature Modules](#feature-modules)
- [Services](#services)
- [RXJS](#rxjs)
    - [Operators](#operators)
- [Pipes](#pipes)
- [Routing](#routing)
- [Forms](#forms)

# Resources
- [Angular site](https://angular.io/)
- [Angular Overview](https://angular.io/guide/architecture)
- [Deborah Kurata on Pluralsight](https://app.pluralsight.com/library/courses/angular-2-getting-started-update/table-of-contents) - excellent, albeit possibly android, instructor. Covers all major topics and shows you how to utilize what she teaches. 
## VS Code Extensions: 
- [Angular snippets](https://marketplace.visualstudio.com/items?itemName=johnpapa.Angular2)
- [Scaffold files quickly](https://marketplace.visualstudio.com/items?itemName=alexiv.vscode-angular2-files)
- [Macros for moving between files](https://marketplace.visualstudio.com/items?itemName=infinity1207.angular2-switcher)

___

# Components

- Components are the building blocks of your application.
- Components are classes with a special `@Component` decorator. 
- Components have all the basic features of classes like methods and properties. 
- Components are linked to a template (the HTML or *view* of the component) and a CSS file for styling. 
- [Lifecycle hooks](https://angular.io/guide/lifecycle-hooks) allow us to run code at certain times. `ngOnInit()` is a lifecycle hooks, as well as `ngOnDestroy()`. These will be used frequently. Lifecycle hooks must be imported, and they must be `implemented` at class declaration: 
  ```ts
      // import lifecycle hooks 
      import { Component, OnInit, OnDestroy } from '@angular/core';
      
      // @Component decorator 
      @Component({
      selector: 'app-tickets',
      templateUrl: './tickets.component.html',
      styleUrls: ['./tickets.component.scss']
      })
      
      // declaration of class, which implements lifecycle hooks
      export class TicketsComponent implements OnInit, OnDestroy {
        constructor(){}

        ngOnInit() {
          // Runs when the component is first rendered. Use this instead of the constructor method!
        }

        ngOnDestroy() {
          // Runs when component leaves the page. Use this for unsubscribing from subscriptions, and more! 
        }
      }
  ```


# Templates 
- Templates are the markup of a component.
- Templates use [template syntax](https://angular.io/guide/template-syntax), which is similar to standard HTML but allows the use of Angular expressions and logic. 
### [Expressions in templates](https://angular.io/guide/interpolation)
  - Include TypeScript expressions in your template by wrapping it in `{{double curly braces}}`. This works only for expressions and strings, not for conditional logic (see below).
### [Structural Directives](https://angular.io/guide/structural-directives)
  - Structural directives are how we use conditional logic in our template. 
  - Includes `*ngFor, *ngIf, and *ngSwitch`.
  - [*ngIf](https://angular.io/api/common/NgIf)
    - `<div *ngIf="1 === 1">This element will render while the condition is true</div>`
  - [*ngFor](https://angular.io/api/common/NgForOf)
    - `<div *ngFor="let apple of apples">This will render a div element for each element in the component's 'apples' array property. We can display data from the current element using double curly braces like so: {{apple}}, or {{apple.type}}.</div>`.
  - [*ngSwitch](https://angular.io/api/common/NgSwitch)
### [Data Binding](https://angular.io/guide/binding-syntax)
  - Multiple uses, including using component data as element attribute values, sharing data between two components, and binding events to elements.
  - `<button [disabled]="thisBooleanValueInMyComponent">This will be disabled based on what thisBooleanValueInMyComponent evaluates to</button>`
- #### [Event Binding](https://angular.io/guide/event-binding)
  - Commonly used for click events, or submit events. 
  - `<button (click)="executeThisMethod($event, otherArgument)">Click me</button>`
  - The button above will fire the `executeThisMethod()` method. An event's `event` value can be passed with `$event`. `$event` can be called anything in your component (`$event`, `event`, or `e` are standard), but must be referred to as `$event` in the template. 
- #### [Two-Way Binding](https://angular.io/guide/two-way-binding)
  - Useful for sharing data between components. 
  - One component can modify another component's data. 
  - Think of a component that changes another component's background color. 
  - If more than one component needs to interact with another component's data, consider using a Service instead.
- [Inputs and Outputs](https://angular.io/guide/inputs-outputs)
  
- [Cheat Sheet](https://angular.io/guide/binding-syntax#binding-types-and-targets)
  - Use [] to bind from source to view.
  - Use () to bind from view to source.
  - Use [()] to bind in a two way sequence of view to source to view.

# [Modules](https://angular.io/guide/architecture-modules)
- Collections of components, directives, and pipes that are shared amongst the app. 
- All apps have the `App.module.ts` module, the base module for any Angular application. 
- [Common Module](https://angular.io/api/common/CommonModule#description) - used to import basic Angular features such as `ngOnInit`. 
- You can use a [Shared Module](https://angular.io/guide/sharing-ngmodules) to easily import and export large amounts of commonly used components/etc.
### [Metadata](https://angular.io/guide/architecture-modules)
- A module's metadata describes the imports and declarations of the module. 
- Basically, what the module collects (including components for feature modules) and what data it gives access to.
- Metadata has 5 properties (text from [here](https://angular.io/guide/cheatsheet)): 
  - declarations: List of components, directives, and pipes that belong to this module.
  - imports: List of modules to import into this module. Everything from the imported modules is available to declarations of this module.
  - exports: List of components, directives, and pipes visible to modules that import this module.
  - providers: List of dependency injection providers visible both to the contents of this module and to importers of this module.
  - bootstrap: List of components to bootstrap when this module is bootstrapped. *Only the root NgModule should set the bootstrap property.*
    - Example: a home page module: 
       ```ts 
      import { NgModule } from '@angular/core';
      import { CommonModule, DatePipe } from '@angular/common';
      import { HomeComponent } from './home.component';
      import { HomeRoutingModule } from './home-routing.module';
      import { SharedModule } from '../../shared/shared.module';
      import { FlexModule } from '@angular/flex-layout';

      @NgModule({
        declarations: [ // Declare which components this module will use if it's a feature module.
          HomeComponent
        ],
        imports: [ // Which modules the declarations of this module should have access to.
          CommonModule, // Import common module so that HomeComponent can use NgOnInit() & more.
          HomeRoutingModule, // This app uses lazy loading, so this feature module's RoutingModule must be imported here.
          SharedModule, // The module that imports data most modules will use, SharedModule, is imported here for HomeComponent to have access to that data. 
          FlexModule // HomeComponent needs to use FlexModule, so it's imported here.
        ],
        providers: [
          DatePipe // Import pipes that components declared by this module, and importers of this module, will use. 
        ],
        exports: []
      })
      export class HomeModule { }
      ```

### Feature Modules
- [Feature Modules](https://angular.io/guide/feature-modules) are modules that contain a view, like a component. They are best thought of as major components to your application, components that can directly import what they need, and can be routed to. Use a feature module when you need a major building block of your app that you know will import components to render, but will be able to render data itself as well. 
  - Example: Any page of a website would be considered a feature module, but not the components that make up that page. The home page of Reddit is a feature module, but the posts listed on the home page are each instances of a component that are fed data to display. 
- Usually you do not use modules to import services because it creates a new instance of that service. Just import the service itself. 
  - Use dependency injection for importing services (see below).

# [Services](https://angular.io/guide/architecture-services)
- Services are classes that share data between components. 
- Services can be thought of as a component without the template. 
- Examples include an `httpService`, which gives components access to the same http functions, or a `userService`, which would hold user data to be shown on the navigation bar, profile, and settings components.
- If multiple components need access to the same data, use a service to store that data and share access instead of messy two-way binding.
- This way, if one component updates the data, *all* components see that data updated. 

# [RXJS](https://rxjs-dev.firebaseapp.com/guide/overview)
- RXJS is a JavaScript library for creating observable data. 
- RXJS lets data be observed by any number of entities, and the observed data will tell those entities when changes occur. 
- Observables broadcast three statuses: `next`, `completed`, and `error`. `next` tells each subscribed entity what the next piece of data is, `complete` tells the subscribed entities that there is no more data and they are automatically unsubscribed, and `error` tells the subscribed entities there was an error. 
- When an entity no longer needs to subscribe to an observable, it must be manually unsubscribed (with few exceptions where it will be automatically unsubscribed). A component's `NgOnDestroy()` lifecycle hook is helpful for unsubscribing from subscriptions, because we know if the component is going to be destroyed it no longer needs to be subscribed. Lingering subscriptions can cause data leaks or worse, so always remember to unsubscribe! 
  - Observable example: our `httpService` fires a method that fetches data from a server. That method returns an `observable`, which is then subscribed to by the component that called it. That component will then know when data from that fetch returns, or if there is an error, or when there is no more data to be returned (common with http requests). 
- Observables can also be an open stream of data instead of finishing after the first return. 
  - Example: a `cartService` tracks the number of items in the user's cart. Numerous components have access to this service for ease of modifying the cart. One of `cartService`'s methods is `cartCount()`, which returns an observable of how many items are in the user's cart. Another method of `cartService` is `updateCartCount(quantity: number)`, which takes a numerical value and updates the service's `cartCount` observable. Now, each component that is subscribed to that observable will know what the new cart quantity is, instead of the `cartService` needing to tell those components itself. 
- Many aspects of Angular are able to be subscribed to, such as when a page has loaded, if a page is going to be navigated from, or the route parameters of a URL. 
### [Operators](https://rxjs-dev.firebaseapp.com/guide/operators)
- RXJS has functions called operators.
- Operators either create streams of data (Creation Operators) or modify streams of data (Pipeable Operators).
- Pipeable operators modify data before allowing access to it by subscribers. Pipeable operators can be piped together to run one after the other (think *chained* together). 

# [Pipes](https://angular.io/guide/pipes) 
- Helpful functions for transforming data in template expressions. 
- Example: 
  - Create a property in a component called `today`: 
    - `today = new Date()`
  - Render that date in the template, but use the `date` pipe to render it nicely: 
    - `<div>{{ today | date }}></div>
- Numerous pipes exist. Check out the Angular documentation for more. 
- Angular supports the development of [custom pipes](https://angular.io/guide/pipes#creating-pipes-for-custom-data-transformations).


# Routing 
- [Routing](https://angular.io/guide/router) is built into Angular. 
- There are two types of routing in Angular: 
  - [Lazy loading](https://angular.io/guide/lazy-loading-ngmodules) (suggested): 
    - Loads routes when they're navigated to, not all at once.
    - Feature modules, meaning modules with views, are routed to. 
      - Example: the home page module has its own route. This module has its own component, and it imports other components to display such as lists of items or a home page notification area. 
  - Standard routing: 
    - Loads all components when the app starts (inefficient). 
    - Can route to components, not just modules. 
- Routes are listed in `app-routing.module.ts`.
- Routes can be [lazy-loaded](https://angular.io/guide/lazy-loading-ngmodules) to improve performance. 


# Forms 
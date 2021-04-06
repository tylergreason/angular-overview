# Resources
- [Angular site](https://angular.io/)
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
        constructor(){

        }

        ngOnInit() {

        }

        ngOnDestroy() {

        }
      }
  ```


# Templates 
- Templates are the markup of a component.
- Templates use [template syntax](https://angular.io/guide/template-syntax), which is similar to standard HTML but allows the use of Angular expressions and logic. 
#### [Expressions in templates](https://angular.io/guide/interpolation)
  - Include TypeScript expressions in your template by wrapping it in `{{double curly braces}}`. This works only for expressions and strings, not for conditional logic (see below).
#### [Structural Directives](https://angular.io/guide/structural-directives)
  - Structural directives are how we use conditional logic in our template. 
  - Includes `*ngFor, *ngIf, and *ngSwitch`.
  - `*ngIf`
    - `<div *ngIf="1 === 1">This element will render while the condition is true</div>`
  - `*ngFor`
    - `<div *ngFor="let apple of apples">This will render a div element for each element in the component's 'apples' property. We can access </div>`.
  - [`*ngSwitch`](https://angular.io/api/common/NgSwitch)
#### [Data Binding](https://angular.io/guide/binding-syntax)
  - Multiple uses, including using component data as element attribute values, sharing data between two components, and binding events to elements.
  - `<button [disabled]="thisBooleanValueInMyComponent">This will be disabled based on what thisBooleanValueInMyComponent evaluates to</div>`
- ##### [Event Binding](https://angular.io/guide/event-binding)
  - Commonly used for click events, or submit events. 
  - `<button (click)="executeThisMethod($event, otherArgument)">Click me</button>`
  - The button above will fire the `executeThisMethod()` method. An event's `event` value can be passed with `$event`. `$event` can be called anything in your component (`$event`, `event`, or `e` are standard), but must be referred to as `$event` in the template. 
- ##### [Two-Way Binding](https://angular.io/guide/two-way-binding)
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
- [Metadata](https://angular.io/guide/architecture-modules), including importing and exporting modules. 
- [Common Module](https://angular.io/api/common/CommonModule#description) - used to import basic Angular features such as `ngOnInit`. 
- You can use a [Shared Module](https://angular.io/guide/sharing-ngmodules) to easily import and export large amounts of commonly used components/etc.
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
- RXJS is not very intuitive at first, so don't worry about understanding it immediately. 
- RXJS lets data be observed by any number of entities, and the observed data will tell those entities when changes occur. 
  - Example: our `httpService` fires a method that fetches data from a server. That method returns an `observable`, which is then subscribed to by the component that called it. That component will then know when data from that fetch returns, or if there is an error, or when there is no more data to be returned (common with http requests). 
- Observables can also be an open stream of data instead of finishing after the first return. 
  - Example: a `cartService` tracks the number of items in the user's cart. Numerous components have access to this service for ease of modifying the cart. One of `cartService`'s methods is `cartCount()`, which returns an observable of how many items are in the user's cart. Another method of `cartService` is `updateCartCount(quantity: number)`, which takes a numerical value and updates the service's `cartCount` observable. Now, each component that is subscribed to that observable will know what the new cart quantity is, instead of the `cartService` needing to tell those components itself. 
- Many aspects of Angular are able to be subscribed to, such as when a page has loaded, if a page is going to be navigated from, or the route parameters of a URL. 

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
- Feature modules, meaning modules with views, are routed to. 
  - Example: the home page module has its own route. This module has its own component, and it imports other components to display such as lists of items or a home page notification area. 
- Routes are listed in the `app-routing.module`.
- Routes can be [lazy-loaded](https://angular.io/guide/lazy-loading-ngmodules) to improve performance. 


# Forms 
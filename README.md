# Angular-cheatsheet

## 1. Angular CLI
Angular gives us the ability to do a whole lot using their CLI. You can config the entire application by just using the CLI. Here are some of the commands:

* npm install -g @angular/cli : This command will install the Angular CLI into our local machine using npm.
* ng new <application name> : This will set up a new Angular application using the ng new command.
* ng new <application name> --prefix best : This creates a new project and set the project prefix to new.
* ng new --help: This returns all available Angular command lists.
* ng lint my-app: This command checks our entire application for any linting warnings.
* ng lint my-app --fix: If there are any form of linting errors, this command will fix it.
* ng lint my-app --format stylish : This formats our entire codebase.
* ng lint my-app --help: This command returns all the available linting command lists.
* ng add <package name>: This command will use your package manager to download new dependencies and update the project with configuration changes.
* ng generate component <name>: This will create a new component of our application. We can also use the ng g c <name> shorthand to do this.
##### ng g d <directive name>: This command angular directive.
##### ng g s <service name> : Creates a new Javascript class-based service.
##### ng g p <pipe name>: Generates a new pipe
##### ng g cl <destination> : This will create a new class in the specified directory.
##### ng build: Builds the application for production and stores it in the dist directory.
##### ng serve -o: Serves the application by opening up the application in a browser using any port 4200 or any available port.
##### ng serve -ssl: serves the application using SSL

## 2. Angular Lifecycle Hooks
### A component in Angular has a life cycle, several different phases it goes through from birth to death. We can hook into those different phases to get some pretty fine-grained control of our application. Here you can see some Angular Lifecycle Hooks.

##### ngOnChanges: This is called whenever one of the input properties changes.
##### ngOnInit: This is called immediately after ngOnChanges is completed and it is called once.
##### ngOnDestroy: Called before angular destroys a directive or component
##### ngDoCheck: Whenever a change detection is running, this is called.
##### ngAfterContentInit: Invoked after Angular performs any content projection into the component’s view.
##### ngAfterContentChecked: This is called each time the content of the given component has been checked by the change detection mechanism of Angular.
##### ngAfterViewInit This is called when the component’s view has been fully initialized.
##### ngAfterViewChecked: Invoked each time the view of the given component has been checked by the change detection mechanism of Angular.

## 3. How Angular Hooks are used
### Always remember that hooks work in a component or directory, so use them in our component, we can do this:

```js
class ComponentName {
    @Input('data') data: Data;
    constructor() {
        console.log(`new - data is ${this.data}`);
    }
    ngOnChanges() {
        console.log(`ngOnChanges - data is ${this.data}`);
    }
    ngOnInit() {
        console.log(`ngOnInit  - data is ${this.data}`);
    }
    ngDoCheck() {
        console.log("ngDoCheck")
    }
    ngAfterContentInit() {
        console.log("ngAfterContentInit");
    }
    ngAfterContentChecked() {
        console.log("ngAfterContentChecked");
    }
    ngAfterViewInit() {
        console.log("ngAfterViewInit");
    }
    ngAfterViewChecked() {
        console.log("ngAfterViewChecked");
    }
    ngOnDestroy() {
        console.log("ngOnDestroy");
    }
}
```

## 4. Component DOM
### Angular comes with DOM features where you can do a whole lot from the binding of data and defining dynamic styles. Let’s take a look at some features:
Before we dive into the features, a simple component.ts file is in this manner:

```js
import { Component } from '@angular/core';
@Component({
    // component attributes
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.less']
})
export class AppComponent {
    name: 'Sunil';
}
```

## 5. Let’s look at some template syntax:
##### Interpolation: using {{data to be displayed}} will display dynamic content from the ts file.
##### <button (click)="callMethod()" ... /> : Adding Click events to buttons to call a method defined in the ts file
##### <button *ngIf="loading" ... />: Adding Conditionals to elements. Conditionals have to listen to truthy or falsy values.
##### *ngFor="let item of items": iterate through a defined list of items. Picture this as a for a-loop.
##### <div [ngClass]="{green: isTrue(), bold: itTrue()}"/>: Adding dynamic classes based on conditionals.
##### <div [ngStyle]="{'color': isTrue() ? '#bbb' : '#ccc'}"/>: Adding dynamic styles to the template based on conditions


## 6. Component Communication
### Passing data from one component to another can be a little bit tricky in Angular. You can pass data from child to parent, parent to parent, and between two unrelated components:

* ```input()```: This method helps To pass a value into the child components.

```js
export class SampleComponent {
@Input() value: 'Some Data should go in here';
}
```
Child components are registered in parents’ components like this:

```<child-component [value]="data"></child-component>```

* ```output()```: This method Emits an event to the parent component. A bunch of data can be passed into the emitted event which makes it a medium of passing data from child to parent:

To Emit the event from the child component:

```
@Output() myEvent: EventEmitter < MyModel > = new EventEmitter();
calledEvt(item: MyModel) {
    this.myEvent.emit(item);
}
```

And then the parent component listens to that event:

```
<parent-component 
(myEvent)="callMethod()"></parent-component>
```

7. Angular Routing
Routing is another cool feature of Angular, with the Angular Routing system we can navigate through pages and even add route guards.

* Component Routing: We can define routes in our application by defining the path and the component to be rendered:

```js
const routes: Routes = [ 
  { path: 'home', component:HomeComponent }, 
  { path: 'blog/:id', component: BlogPostCompoent }, 
  { path: '**', component: PageNotFoundComponent } 
];
```


For routing to work, add this the your ```app.module.ts``` file:


```
RouterModule.forRoot(routes)
```

There are situations whereby you want to keep track of what is happening in your routes, you can add this to enable tracing in your angular project:

```js
RouterModule.forRoot(routes,{enableTracing:true})
```


To navigate through pages in Angular, we can use the ```routerLink``` the attribute which takes in the name of the component we are routing to:

```<a routerLink="/home" routerLinkActive="active"> Crisis Center</a>```

The ```routerLinkActive="active"``` will add an active class to the link when active.

8. Writing Route Guards
We can define a guard for route authentication. We can use the CanActivate class to do this:

```js
class AlwaysAuthGuard implements CanActivate {        
        canActivate() {
                return true;
        }
}
```

To use this rote guard in our routes we can define it here:

```js
const routes: Routes = [
  { path: 'home', component:HomeComponent },
  { path: 'blog/:id', component: BlogPostCompoent,canActivate: [AlwaysAuthGuard],  },
    { path: '**', component: PageNotFoundComponent }
];
```

## 9. Angular Services
Angular services come in handy when you can do things like handling HTTP requests and seeding data on your application. They focus on presenting data and delegating data access to a service.

```
@Injectable()
export class MyService {
    public users: Users[];
    constructor() { }
    getAllUsers() {
        // some implementation
    }
}
```
To use this service in your component, import it using the import statement and then register it in the constructor

```
import MyService from '<path>'
constructor(private UserService: MyService)
```
To make things easier, we can use this command to generate service in Angular

```ng g s <service name>```


## 10. HTTP Service
Angular comes with its own HTTP service for making HTTP requests. To use it, you have to first of all import it into your root module:

```js
import { HttpClientModule} from "@angular/common/http";
After importing it, we can now use it inside our service for making HTTP requests:

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
@Injectable({
    providedIn: 'root'
})
export class UserService {
    constructor(private http: HttpClient) { }
    getAllUsers() {
        return this.http.get(`${baseURL}admin/list-users`);
    }
}
```

## 11. HTTP Interceptors
An interceptor is a piece of code that gets activated for every single HTTP request received by your application. Picture an interceptor as a middleware in nodejs where an HTTP request made is passed through this piece of code.

To define an interceptor create a http-interceptor.ts file inside your src directory and add this:
```js
import { Injectable } from '@angular/core';
import {
    HttpEvent,
    HttpInterceptor,
    HttpHandler,
    HttpRequest,
    HttpErrorResponse,
    HttpResponse
} from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
@Injectable({
    providedIn: 'root'
})
export class HttpConfigInterceptor implements HttpInterceptor {
    constructor() { }
    intercept(req: HttpRequest<any>, next: HttpHandler) {
        // Get the auth token from  localstorage.
        const authToken = localStorage.getItem('token');
        // Clone the request and replace the original headers with
        // cloned headers, updated with the authorization.
        const authReq = req.clone({
            headers: req.headers.set('Authorization', authToken)
        });
        // send cloned request with header to the next handler.
        return next.handle(authReq);
    }
}
```
This is a simple interceptor that checks if users have a token in their device’s local storage. If the user does, it will pass the token in all the HTTP headers.

## 12. Pipes
Pipes in Angular give us the ability to transform data into any specific format. For example, you can write a simple pipe that will format an integer to a currency format or format dates to any form.
Angular comes with some built-in pipes like the date and currency pipe.

We can define our own custom pipes too by doing this:
```js
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({ name: 'exponentialStrength' })
export class ExponentialStrengthPipe implements PipeTransform {
    transform(value: number, exponent?: number): number {
        return Math.pow(value, isNaN(exponent) ? 1 : exponent);
    }
}
```
To use a pipe in our component we can do this:

```{{power | exponentialStrength: factor}}```

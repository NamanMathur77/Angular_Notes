# Basics of Angular and javascript    

### What is angular?

Angular is a typescript based open source front-end platform that makes it easy to build web, mobile and desktop applications.

### What are building blocks of angular application

1. Modules - Modules are container for holding the block of code

```Typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
1.1. declaration - contains all the components that are part of this module
1.2. imports - other modules that this module has a dependency on
1.3. providers - contains all the services whose single instance would be available in this module

2. Component - Components are basic building block of the angular application they hold the template and logical part associated with it

```Typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'My Angular App';
}
```

3. Templates - Templates are HTML structures associated with the component

```HTML
<!-- app.component.html -->
<h1>{{ title }}</h1>
<button (click)="doSomething()">Click Me</button>
```

4. Directives - They are used to add behavior to the elements in angular component- three types- component, structural and attribute

```Typescript
// example.directive.ts
import { Directive, ElementRef, Renderer2, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', 'yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.renderer.removeStyle(this.el.nativeElement, 'backgroundColor');
  }
}
```

5. Services - Services are classes that are used to provide a functionality or are used to share the data among the components
    Dependency injection - Angular uses dependency injection to provide instance of service across the application
    Singleton - A single instance of service is shared among the components in the angular application

```Typescript
// example.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ExampleService {
  getData() {
    return 'Data from service';
  }
}
```

6. Pipes - Pipes are used to transform the data in the templates, two types - built-in and custom pipes

```Typescript
// custom.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'customPipe'
})
export class CustomPipe implements PipeTransform {
  transform(value: string): string {
    return value.toUpperCase();
  }
}
```

7. Routing - Angular router modules helps in configuration of routes

8. Forms - Template driven form and Reactive forms


### What are directives and different types of directives?
Directives allows you to add behavior to the HTML elements, manipulate DOM or create reusable components.
There are 3 types of directives - 
1. Component Directive - Contains HTML template and logical Typescript class

```Typescript
// example.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `<h1>{{ title }}</h1>`,
  styles: [`h1 { color: blue; }`]
})
export class ExampleComponent {
  title = 'Hello, World!';
}
```

2. Structural Directive - They are used to add or remove the elements from DOM. eg - *ngIf, *ngFor, *ngSwitch

```Typescript
<!-- example.component.html -->
<div *ngIf="isVisible">This element is visible</div>
```

```Typescript
<!-- example.component.html -->
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

3. Attribute directive - They are used to change the behavior or appearance of the element

```Typescript
// highlight.directive.ts
import { Directive, ElementRef, Renderer2, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', 'yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.renderer.removeStyle(this.el.nativeElement, 'backgroundColor');
  }
}
```
### Angular lifecycle hooks

There are 8 lifecycle hooks in angular 

ngOnInit is called when component initialize. It is called once. Mostly, I used for variable initialize and API call. ngOnDestroy is called before detroying the component. I heavily used for unsubscribe the subscription to prevent the memory leak.

Several times I have used ngOnChanges, ngAfterContentInit, ngAfterViewInit in my career. ngOnChanges method is called once on component’s creation and then every time changes are detected in one of the component’s input properties. It receives a SimpleChanges object as a parameter. ngAfterViewInit is called after the component view and its child views has been initialized. ngAfterContentInit is called after components external content (or from parent ) has been initialized.

There are other hooks like ngDoCheck, ngAfterContentChecked, ngAfterViewChecked, I did not use them too much.

### Data binding in angular

#### From component to DOM

##### Interpolation 

{{value}} used to add value of a property from the component

##### Property binding

[property] = "value" value is passed from component to the specified property

#### From DOM to component

##### Event binding

(event) = "function" when a specific event occurs call the specified function

#### Two way binding

[(ngModel)] = "value" allows data to flow both ways from DOM to component and vice versa


### What is metadata

Metadata is used to decorate a class so that it can configure the expected behavior of the class.

Types- 
1. Class decorators - @Component, @Module
2. Property decorators - @Input, @Output
3. Method decorators - @HostListener


### What is the difference between constructor and ngOnInit

Both constructor and ngOnInit lifecycle hook are used to initialize the component but they serve different purpose and are used in different contexts.

#### Constructor

The constructor is a special method in a class that is called when an instance of the class is created. In angular it is used for initialization tasks and for injecting dependencies via Angular's dependency injection

Constructor runs befor angular has fully initialized the component and before the component's input properties are set

#### NgOnInit

It is a lifecycle hook and is called after angular has fully initialized the component, including its input properties

ngOnInit runs after the constructor and after angular has set all the input properties of the component


### What are services

Services are used to share business logic, data access or any reusable functionality among different components of an application.
Angular uses DI to provide instances of services to components or other services. This promotes loose coupling.
Services are ofter singleton, meaning that angular creates single instance of the service and shares it across the application.

```Typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private data: string[] = ['Item 1', 'Item 2', 'Item 3'];

  getData(): string[] {
    return this.data;
  }

  addData(item: string): void {
    this.data.push(item);
  }
}
```
If you want to create different instance of the service for different components then in providers array of both components add the service to create different instance of the service for both the components

### What is dependency injection in angular

Dependency injection is a design pattern in which a class asks for dependencies from external source rather creating them itself. This helps in efficient management of services and other dependencies across the application.

DI works with @Injectable decorator which marks a class as a service that can be injected into other classes. When class is declared as dependency angular injector looks up the provider for that dependency and supplies an instance of it.

### Pipes in angular

Pipes allows you to transform data within your templates they are used to format, transform or manipulate data for display pupose without altering the underlying data model

Built in pipes - 
DatePipe: Formats a date value according to locale rules.
UpperCasePipe: Transforms text to uppercase.
LowerCasePipe: Transforms text to lowercase.
CurrencyPipe: Formats a number as currency.
DecimalPipe: Formats a number as decimal.
PercentPipe: Formats a number as a percentage.
JsonPipe: Converts a value into a JSON string.
SlicePipe: Creates a new array or string containing a subset (slice) of the elements.

### Async pipe 

Async pipe in angular is a special pipe that automatically subscribes to an observable or promise and returns the latest value it has emitted.

### What happen if you use script tag inside template

To prevent cross site scripting angular sanitizes the content and removes the script tag

### Difference between pure and impure pipes

Pure pipes are default type of pipes in Angular, they are called only when the inputs to the pipe changes

Impure pipes are executed during every change detection cycle, regardless of input value change


### What are observables

Observables are concept for handling asynchronous data and events they are more flexible then the promises and callbacks
An observable is a stream of data that can emit multiple values over time.
```Typescript
import {Observable} from "rxjs";

... 

const myObservable = new Observable( observer =>{     
   let value = 0;
   setInterval ( () =>
   {
      observer.next(value); //this is what sends a new value
      value = value + 10;
      if(value > 50){
         observer.complete(); //finish sending values
       }
    }  ,1000 );
  }
  );

myObservable.subscribe(
   val=> console.log(val), //for a value returned
   error => console.log("problem"), //if something happens
   () => console.log("Done")
   ); //once it is done

myObservable.subscribe( {
      next: val => console.log("Second:" + val),
      error: error => console.log(error),
      complete: () => console.log("Completed")
   } );

```

With the above code both subscriptions are going to get the values independently. Sometimes, instead of starting an independent execution for each subscriber, you want each subscription to get the same values, even if the values have already started emitting. In this case you will need multicast.

### What are subjects



### What are different rxjs operators

1. of(value1, value2, ...): Creates an observable that emits the provided arguments as values
2. map(fn): Applies a function to each emitted value and emits the transformed value
3. filter(fn): Only emits the value passed by the test in the provided function

### What is subscribing

subscribing is listening and reacting to the stream of data emitted by an observable.
When you subscribe to an observable you provide an observer object. This observer decides how you want to handle the data with three callback functions
next(value): this is called and obseervable emits a new value
error(error): this is called when observable encounters an error
complete(): this is called and observable has finished emitting the data, you can use this to unsubscribe the observable


### What are dynamic components

These are components that are created and inserted into the view at runtime rather than deing defined statically in the template

eg.
```Typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-message',
  template: `
    <p>{{ message }}</p>
  `
})
export class MessageComponent {
  @Input() message: string = '';
}
```

```Typescript
import { Component, ViewChild, ViewContainerRef, ComponentFactoryResolver } from '@angular/core';
import { MessageComponent } from './message.component';

@Component({
  selector: 'app-parent',
  template: `
    <button (click)="showMessage()">Show Message</button>
    <div #messageContainer></div>
  `
})
export class ParentComponent {
  @ViewChild('messageContainer', { read: ViewContainerRef }) container: ViewContainerRef;

  constructor(private componentFactoryResolver: ComponentFactoryResolver) {}

  showMessage() {
    const message = 'This is a dynamically created message!';

    // Create component factory
    const factory = this.componentFactoryResolver.resolveComponentFactory(MessageComponent);

    // Create component instance
    const componentRef = this.container.createComponent(factory);

    // Set message input
    componentRef.instance.message = message;
  }
}
```
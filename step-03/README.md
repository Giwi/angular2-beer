# AngularBeer - AngularJS tutorial - Step 03 #

We did a lot of work in laying a foundation for the app in the last step, so now we'll do something simple; we will add full text search (yes, it will be simple!).

We want to add a search box to the app, and we want the results on the beer list change according to what the user types into the search box.

In Angular 1 you got `filter` and `orderBy`, thez do dot exist in angular 2 : [No FilterPipe or OrderByPipe](https://angular.io/docs/ts/latest/guide/pipes.html#!#no-filter-pipe)

## Pipe ##

Let's create a pipe :

`app/pipes/filter-array-pipe.ts`

```typescript
import {Pipe} from 'angular2/core';

// # Filter Array of Objects
@Pipe({ name: 'filter' })
export class FilterArrayPipe {
    transform(value, args) {
        if (!args[0]) {
            return value;
        } else if (value) {
            return value.filter(item => {
                for (let key in item) {
                    if ((typeof item[key] === 'string' || item[key] instanceof String) &&
                        (item[key].indexOf(args[0]) !== -1)) {
                        return true;
                    }
                }
            });
        }
    }
}
```

## Controller ##

```typescript
Import the pipe in `app/beerlist/beerList.component.ts`:

import {Component} from 'angular2/core';
import {FilterArrayPipe} from '../pipes/filter-array-pipe';

@Component({
    selector: 'beer-list',
    templateUrl: './app/beerlist/beerList.html',
    pipes: [FilterArrayPipe]
})

export class BeerList {
[...]
```


## Template ##

We use [Twitter Bootstrap](http://getbootstrap.com) column model to divide the page in two columns, the left one for the search box, the right one for the beer  list.

We need to add a standard HTML `<input>` tag and an Angular's [pipe](https://angular.io/docs/ts/latest/guide/pipes.html) to process the input for the [NgFor directive](https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html) directive.

This lets a user enter search criteria and immediately see the effects of their search on the beer list.

`app/beerlist/beerList.html`:

```html
 <div class="container">
     <div class="row">
         <div class="col-md-2">
             <!--Sidebar content-->
             Search: <input [(ngModel)]="query">
         </div>
         <div class="col-md-10">
             <!--Body content-->
             <ul>
                 <li *ngFor="#beer of beers | filter:query">
                     <span>{{beer.name}}</span>
                     <p>{{beer.description}}</p>
                 </li>
             </ul>
         </div>
     </div>
 </div>
```


This new code demonstrates the following:

* Data-binding: This is one of the core features in Angular. When the page loads, Angular binds the name of the input box to
  a variable of the same name in the data model and keeps the two in sync.

  In this code, the data that a user types into the input box (named `query`) is immediately available as a filter input in the list repeater (`beer in beers | filter:query`). When changes to the data model cause the repeater's input to change, the repeater efficiently updates the DOM to reflect the current state of the model.

* The `FilterArrayPipe` pipe uses the `query` value to create a new array that contains only those records that match the `query`. `*ngFor` automatically updates the view in response to the changing number of phones returned by the `filter` filter. The process is completely transparent to the developer.

## Experiments ##

### Display Current Query ###

Display the current value of the query model by adding a `{{query}}` binding into the `app/beerlist/beerList.html` template, and see how it changes when you type in the input box.

## Summary ##

We have now added full text search! Now let's go on to step 4 to learn how to add sorting capability to the beer app.
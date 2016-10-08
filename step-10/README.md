# AngularBeer - AngularJS tutorial - Step 03 #

In angular, everything is a component and you have to assemble your components together.

We have created the `BeerList` component, now we will create a `BeerExerp` controller.

`app/beerexerp/beerExerp.component.ts` :

```typescript
import {Component, Input} from '@angular/core';
@Component({
    selector: 'beer-exerp',
    templateUrl: './app/beerexerp/beerExerp.html'
})
export class BeerExerp implements OnInit {
    @Input()
    beer: any;
}
```

Note the `@Input()` annotation, you can now pass someting to your component.

`app/beerexerp/beerExerp.html` :

```html
<div class="row">
    <a href="#/beers/{{beer.id}}" class="col-sm-4 thumb">
        <img class="img-thumbnail" [src]="beer.img" alt="Beer image"/>
    </a>
    <div class="col-sm-8 card-block">
        <a href="#/beers/{{beer.id}}"><h4 class="card-title">{{beer.name}} <span
                class="tag tag-default">{{beer.alcohol}} °</span>
        </h4></a>
        <p class="card-text text-justify">{{beer.description}}</p>
        <a href="#/beers/{{beer.id}}" class="btn btn-primary">Detail</a>
    </div>
</div>
```

Now, import it in the AppModule :

```typescript
[...]
import {BeerExerp} from './beerexerp/beerExerp.component'
[...]
    declarations: [
        AppComponent,
        BeerList,
        BeerDetail,
        BeerExerp,
        FilterArrayPipe,
        OrderByPipe
    ],
[...]
```

And now we can use it in the `beerList.html` template :

```html
[...]
<div class="card" *ngFor="let beer of (beers | filter:query | orderby:orderProp)">
    <beer-exerp [beer]="beer"></beer-exerp>
</div>
[...]
```
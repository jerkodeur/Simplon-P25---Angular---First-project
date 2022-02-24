##### Simplon-P25---Angular---First-project
# Points clés du tuto
## Composants
- Chaque composant est crée par une ligne de commande
- Chaque composant comporte à minima 3 fichiers:
  - Un fichier _.html_
  - Un fichier _.css_
  - Un fichier _.ts_

## Directives
    On peut appliquer directement dans les éléments html des traitement javascript à l'aide des directives
Il en existe plusieurs, qui correspondent à leurs homologues en Js, voici celles que l'on a vu dans le tutoriel:
- `*ngFor` : boucle __for _element_ of _elements___ _Affiche pour chaque élément le contenu contenu dans la balise_
- `*ngIf` : condition __if__ _Affiche le contenu dans la balise si la condition est vraie_

## Afficher des données dynamiques
Pour afficher des données dynamiques, il faut placer les variables entre double accolades
> `{{ Donnée }}`

## Rendre les propriétés dynamiques
Afin de pouvoir utiliser des variables comme valeurs de propriétés d'éléments HTML il suffit d'entourer la propriété par des accolades
> `<a [title]="item.value">...</a>`

## Gérer des évènements
Il est possible d'écouter des évènements et de leurs attribuer des actions associées
> `<button (click)="methodToCall">...</button>`

## Transmettre des données à ses composants enfants
Lorsque l'on appelle un composant enfant, il est possible de lui faire passer des données afin qu'il puisse les utiliser
- Ajouter les données dans la propriété de l'élément parent qui pointe vers le composant enfant
- Dans le composant parent, importer la classe _Input_
- A l'initialisation du composant, définir les données entrantes
##### Composant parent
```html

<mon-composant-enfant [item]="item"></mon-composant-enfant>
```
##### Composant enfant
```js
import {Input} from '@angular/core';

export class mon-composant-enfant implements OnInit {

  // Permet d'utiliser item dans le composent actuel
  @Input() item!: Item;
}
```
```html
<p>{{ item.name }}</p>
```

## Faire remonter des données vers le composant parent
Pour que le composant parent puisse répondre à un évènement se produisant dans un de ses composants enfants, Les composants enfant doivent pouvoir émettre cet évènement.

##### Composant enfant
```js
import {Output, EventEmitter} from '@angular/core';

export class mon-composant-enfant {

@Output() action = new EventEmitter();
}
```
```html
<!-- Au clic, indique au composant parent que l'action à été déclenchée -->
<button (click)="action.emit()">...</button>
```
##### Composant parent
```js
export class mon-composant-parent {
  onAction() {
    // Effectuer une ou plusieurs action(s)
  }
}
```
```html
<mon-composant-enfant (action)="onAction()"></mon-composant-enfant>
```

## Naviguer entre les pages
L'on doit pouvoir se déplacer entre les différentes pages lorsque l'on clic sur les liens de l'application
##### src/app/app.module.ts
```js
@NgModule({
  imports: [
    ...,
    RouterModule.forRoot([
      // Définir une route, associée au composant à afficher
      { path: '/products/:id', component: ProductDetails}
    ])
  ]
})
```

```html
<!--
  Composant ou se trouve le lien
  On fournit la route à appeler, ainsi que le paramètre nécessaire
-->
<a [RouterLink]="['/products', product.id]">...</a>
```

## Récupérer les paramêtres d'une route
Il est possible réupérer un tas d'information à partir de la route appelée sur la page courante
```js
  import {ActivedRoute} from '@angular/router';

  export class mon-composant implements OnInit {
    // On injecte le service qui fournit des informations sur la route active dans le constructeur
    constructor(private route: ActivedRoute) {}

    ngOnInit() {
      // Récupérer les paramètres de la route active
      const routeParams = this.route.snapshot.paramMap;
      // Récupérer paramètre spécifié
      const paramId = routeParams.get('id');
    }
  }
```

## Services
### Définition
Un service est une instance de classe qui peut être utilisée partout dans l'application par injection.
### Créer d'un service
```bash
ng generate service cart
```
### Injecter un service
```js
// importer le service
import { MonService } from "./monService.service";

export class myComponent implements OnInit {
  // ...

  // On injecte le service dans le constructeur de la classe
  constructor(
    private monService : MonService
  )

  // Utilisation du service
  const maMethod = () => {
    monService.action();
  }
}
```
## Ajouter un module à l'application
Après avoir installer de nouveaux modules, ou afin de pouvoir en utiliser certains déjà existant dans Angular, il est nécessaire de l'ajouter dans l'application
##### src/app/app.module.ts
```js
// Importe le module à partir du dossier 'node_modules'
import { HttpClientModule } from "@/angular/common/http";

// Permet d'utiliser le module dans toute l'application
@NgModule({
  imports: [
    // ...
    HttpClientModule,
    // ...
  ]
})
```





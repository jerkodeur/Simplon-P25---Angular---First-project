- [Description du projet](#description-du-projet)
- [Installation et exécution](#installation-et-exécution)
- [Points clés du tuto](#points-clés-du-tuto)
  - [Composants](#composants)
  - [Directives](#directives)
  - [Afficher des données dynamiques](#afficher-des-données-dynamiques)
  - [Rendre les propriétés dynamiques](#rendre-les-propriétés-dynamiques)
  - [Gérer des évènements](#gérer-des-évènements)
  - [Transmettre des données à ses composants enfants](#transmettre-des-données-à-ses-composants-enfants)
  - [Faire remonter des données vers le composant parent](#faire-remonter-des-données-vers-le-composant-parent)
  - [Naviguer entre les pages](#naviguer-entre-les-pages)
  - [Récupérer les paramêtres d'une route](#récupérer-les-paramêtres-dune-route)
  - [Services](#services)
    - [Définition](#définition)
    - [Créer d'un service](#créer-dun-service)
    - [Injecter un service](#injecter-un-service)
  - [Modules](#modules)
    - [Ajouter un module à l'application](#ajouter-un-module-à-lapplication)
    - [Utiliser un module](#utiliser-un-module)
  - [Formulaires](#formulaires)
    - [FormBuilder module](#formbuilder-module)

# Description du projet
Afin de prendre en main le framework javascript Angular, il m'a été demandé de suivre le [tutoriel présent sur le site officiel](https://angular.io/start), et de présenter dans ce fichier les éléments qui me semble être important, ainsi que de la manière dont je les interprete.

# Installation et exécution
1. Cloner le projet
2. Se placer dans le répertoire du projet
3. Lancer la commande `npm install` afin d'installer les modules nécessaires
4. Lancer le serveur et afficher la page sur son navigateur avec la commande `ng serve -o`
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


__Composant parent__
```html

<mon-composant-enfant [item]="item"></mon-composant-enfant>
```
__Composant enfant__
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

__Composant enfant__
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
__Composant parent__
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
__src/app/app.module.ts__
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
## Modules
### Ajouter un module à l'application
Après avoir installer de nouveaux modules, ou afin de pouvoir en utiliser certains déjà existant dans Angular, il est nécessaire de l'ajouter dans l'application
__src/app/app.module.ts__
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

### Utiliser un module
```js
import { HttpClientModule } from "@/angular/common/http";

// Injecter le module dans le constructeur
export class myClass {
  constructor() {
    private http: HttpClientModule
}
```

## Formulaires
### FormBuilder module
Grâce au FormBuilder, l'on va pouvoir lier et récupérer les données saisies par les utilisateurs dans des formulaires présents dans nos pages HTML.
__mon-composant.ts__
```js
// Importation du module
import { FormBuilder } from "@angular/forms";

// Injection du service dans le constructeur
export class monComposent {
  constructor(
    private formBuilder = FormBuilder;
  ) {}

  // Création d'un objet qui va contenir les données du formulaire lié (instance de la classe FormGroup)
  monFormulaire = this.formBuilder.group({
    nom: '',
    adresse: ''
  });

  // Méthode qui va être déclenchée lorsque l'utilisateur va soumettre le formulaire
  onSubmit = ():void => {
    // Récupération d'un objet contenant toutes les données soumises
    console.log(this.monFormulaire.value);

    // Récupération d'une des valeurs soumises
    console.log(this.monFormulaire.value.nom);

    // Effacer les données du formulaire
    this.monFormulaire.reset();
  }
}
```
__mon-composant.html__
```html
  <!-- Liaison du formulaire et définition de la méthode à appeler lorsque le formulaire sera validé -->
 <form [formGroup]="monFormulaire" (ngSubmit)="onSubmit()">
   <!-- Liason de chacun des champs avec formControlName -->
  <input type="text" id="nom" formControlName="nom">
  <input type="text" id="adresse" formControlName="adresse">
 <button class="button" type="submit">Soumettre</button>
</form>
``



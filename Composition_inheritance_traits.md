# Un probleme qui n'en finit pas de soulever des questions

## Quelques rappels

| Catégorie | Basé sur les classes (Java) | Basé sur des prototypes (JavaScript) |
| --- | --- | --- |
| Classe et instance | La classe et l'instance sont des entités distinctes. | Tous les objets peuvent hériter d'un autre objet. |
| Définition |  Définir une classe avec une définition de classe ; instancier une classe avec des méthodes de construction.  |  Définir et créer un ensemble d'objets avec des fonctions de construction.  |
| Création d'un nouvel objet | Créer un objet unique avec l'opérateur new. | Pareil. |
| Construction de la hiérarchie des objets |  Construire une hiérarchie d'objets en utilisant des définitions de classes pour définir des classes enfants à partir de classes existantes.  |  Construire une hiérarchie d'objets en assignant un objet comme prototype associé à une fonction de construction.  |
| Modèle d'héritage | Hériter des propriétés en suivant la chaîne de classes. | Hériter des propriétés en suivant la chaîne des prototypes. |
| Extension des propriétés |  La définition de la classe spécifie toutes les propriétés de toutes les instances d'une classe. Impossible d'ajouter des propriétés dynamiquement au moment de l'exécution.  |  La fonction ou le prototype du constructeur spécifie un ensemble initial de propriétés. On peut ajouter ou supprimer dynamiquement des propriétés à des objets individuels ou à l'ensemble des objets.  |

src: https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Details_of_the_Object_Model

## De l'objet à l'instance
https://gist.github.com/travishen/1a03e022f4342481ebf61785e344f24f : pure prototypal inheritance


# Ressources

## Traits
- https://www.barbarianmeetscoding.com/blog/safer-javascript-object-composition-with-traits-and-traits-dot-js
- https://calebporzio.com/equivalent-of-php-class-traits-in-javascript notons que d'apres mon test addMixin ne fonctionne pas
- https://github.com/YR/simple-traits/blob/master/index.js ce qui est intéressant dans ce code c'est l'explication de object.create
```javascript
...
var objectCreate = Object.create
	|| function (proto, propMap) {
			var self;
			function dummy() {};
			dummy.prototype = proto || Object.prototype;
			self = new dummy();
			if (propMap) defineProperties(self, propMap);
			return self;
		};
```

### Librairies
 - https://github.com/SciSpike/mutrait

## Héritage

## Multihéritage

### Librairie
- https://github.com/fasttime/Polytype

## Composition (Mixin)

Dans les commentaires de cette [question](https://stackoverflow.com/questions/66967141/is-composition-less-performant-than-inheritance-in-javascrip) appelle cela : mixin based composition ce qui me semble être très explicite. Je pense que les tests dont le commentaire parle sont ici: https://jsbench.me/0mkn7wwj7n/1
>  Class based inheritance & prototype mixin tests were the fastest 
*NoteStt*
```javascript
Gator2 = {
   name:'',
   biteCount:0
}

console.dir(Gator2); // permet de voir que Gator2 sera bien modifié lors de l'appel a aly

//publicHi.call(Gator2.prototype); ne peut pas fonctionner
//publicBite.call(Gator2.prototype); ne peut pas fonctionner


function invokeBehavior(item) {
  item.sayHi();

  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();

  item.sayHi();
}

aly = Object.create(Gator2)
publicHi.call(aly.__proto__);
publicBite.call(aly.__proto__);
invokeBehavior(aly);
```

juste une information la composition par Object.assign opère une shallow copy (on pointe sur la meme zone), par contre ... opère une deep copy voir dans les tests personnels ([ref](https://stackoverflow.com/questions/184710/what-is-the-difference-between-a-deep-copy-and-a-shallow-copy))

ce que je retiens surtout est illustré dans [cet article](https://medium.com/geekculture/mixin-and-treats-94035022182c) dans la partie javascript, le mixin est une copie dans l'objet de destination des differentes propriétés à recopier.
C'est aussi ce qu'on voit [ici](https://stackoverflow.com/questions/48710304/what-are-the-practical-differences-between-mixins-and-inheritance-in-javascript). De ce dernier article je retiens aussi 
> This flexibility should be not that surprising since mixins/traits contribute to composition just the behavior point of view (an object has a behavior) whereas real inheritance, also in JS, is responsible for the is a thing relationship. 



- https://javascript.info/mixins
- https://stackoverflow.com/questions/49002/prefer-composition-over-inheritance?rq=1
- https://www.digitalocean.com/community/tutorials/js-using-js-mixins rapide
- https://medium.com/geekculture/mixin-and-treats-94035022182c
- https://subscription.packtpub.com/book/application-development/9781783283576/1/ch01lvl1sec11/understanding-javascript-mixins pas compris l'article
- https://javascriptweblog.wordpress.com/2011/05/31/a-fresh-look-at-javascript-mixins/semble semble avoir un lien avec l'article précédent
- https://javascript.info/mixins rien de véritablement neuf

### Librairie
- https://github.com/soundcloud/remixin dommage qu'il y ait une dépendance à underscore semble être entre le hook et le mixin
- https://github.com/daffl/uberproto pas forcément nécessaire mais pourrait être utile
- https://github.com/Gozala/selfish : Class-free, pure prototypal inheritance  pour info

- https://github.com/michaelfranzl/captain-hook  Tiny configurable and isomorphic event emitter library for mixing into JavaScript objects  , NOTESTT complet, intéressant
- https://github.com/glooca/pubsub Javascript publisher-subscriber eventbus / mixin 

## Global

- https://stackoverflow.com/questions/66967141/is-composition-less-performant-than-inheritance-in-javascript NOTESS on retrouve des commentaires de peter sellinger le meme que sur le ce message](https://stackoverflow.com/questions/48710304/what-are-the-practical-differences-between-mixins-and-inheritance-in-javascript) 
- https://stackoverflow.com/questions/48710304/what-are-the-practical-differences-between-mixins-and-inheritance-in-javascript

# Tests personnels

## Général

```javascript
var person = {
  firstname: 'Default',
  lastname: 'Default',
  getName: function() {
    return this.firstname + ' ' + this.lastname;
  }
}

var sayHiMixin = {
  sayHi() {
    //console.log('who is this %o',this);
    return `Hello ${this.firstname}`;
  },
  sayBye() {
    return `Bye ${this.lastname}`;
  }
};
```

``` javascript
var john = Object.create(person);
john.firstname = 'John';
john.lastname = 'Doe';
var jack = Object.create(person);
jack.firstname = 'Jack';
jack.lastname = 'Daniels';
Object.assign(jack.__proto__, sayHiMixin);
console.log('(typeof jack.sayHi) : ', (typeof jack.sayHi)); // (typeof jack.sayHi) :  function
console.log('(typeof person.sayHi) : ', (typeof person.sayHi)); // (typeof person.sayHi) :  function
console.log(jack.sayHi()); // "Hello Jack"
console.log(john.sayHi()); // "Hello John"
// this point bien sur les bons objets mais les objets sont representés comme ceci
/*{
  "firstname": "John",
  "lastname": "Doe"
}*/
//donc sans le mixin
```
mais
``` javascript
Object.assign(jack, sayHiMixin);
console.log('(typeof jack.sayHi) : ', (typeof jack.sayHi)); // (typeof jack.sayHi) :  function
console.log('(typeof person.sayHi) : ', (typeof person.sayHi)); // (typeof person.sayHi) :  undefined
console.log(jack.sayHi()); // "Hello Jack"
console.log(john.sayHi()); // "Uncaught TypeError: john.sayHi is not a function"
// this point bien sur le bon objet pour jack mais l'objet est representé comme ceci
/*
{
  "firstname": "Jack",
  "lastname": "Daniels"
  sayBye: function()
  sayHye: function()
}
*/
```
si
``` javascript
Object.assign(person, sayHiMixin);
console.log('(typeof jack.sayHi) : ', (typeof jack.sayHi)); // (typeof jack.sayHi) :  function
console.log('(typeof person.sayHi) : ', (typeof person.sayHi)); // (typeof person.sayHi) :  function
console.log(jack.sayHi()); // "Hello Jack"
console.log(john.sayHi()); // "Hello John"
// this point bien sur les bons objets mais les objets sont representés comme ceci
/*{
  "firstname": "John",
  "lastname": "Doe"
}*/
sayHiMixin.yop = () => 'yop';
console.log(john.yop()); // "Uncaught TypeError: john.yop is not a function"
``` 
mais
``` javascript
sayHiMixin.yop = () => 'yop';
Object.assign(person, sayHiMixin);
console.log(john.yop()); // "yop"
```
Héritage dans les mixin
```javascript
// ai testé en inserrant juste apres sayHiMixin
var sayHiMixinBis = {
  __proto__: sayHiMixin, // (or we could use Object.setPrototypeOf to set the prototype here)

  sayHi2() {
    // call parent method
    return `hello 2 ${super.sayHi()}`; // (*)
  },
};
... reprise du code précédent
sayHiMixin.yop = () => 'yop';
Object.assign(person, sayHiMixin);
console.log(john.yop()); // "yop"
Object.assign(person, sayHiMixinBis);
console.log(john.sayHi2()); // "hello 2 Hello John"
````
par contre
```javascript
sayHiMixin.yop = () => 'yop';
Object.assign(person, sayHiMixinBis);
console.log(john.yop()); // " Uncaught TypeError: john.yop is not a function"
//cette ligne fonctionnerait
console.log(john.sayHi2()); // "hello 2 Hello John" 
```
pour corriger le probleme on pourrait faire
```javascript
sayHiMixin.__proto__.yop = () => 'yop';
Object.assign(person, sayHiMixinBis);
console.log(john.yop()); "yop"
console.log(john.sayHi2()); // "Hello John"
```
mais ce n'est pas terrible car on est remonté dans le chaine des prototype ce qui n'est pas ce que l'on voulait faire
regarder aussi ici https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object/proto
et ceci
https://stackoverflow.com/questions/332422/get-the-name-of-an-objects-type pour essayer de trouver le nom de mon objet
## trouver les réferences

```javascript
//dans le code précédent si je fais
console.log(Object.getPrototypeOf(jack));
/*
{
  "firstname": "Default",
  "lastname": "Default"
  getName: function(),
  sayBye: function(),
  sayHi: function(),
  sayHi2: function()
  prototype: 
  ...
  yop: function()
}

// note console.log(Object.getPrototypeOf(jack) === person); renvoie bien true
```
## Shallow copy

```javascript
//The swim property here is the mixin
var swim = {
  a:1,
  location() {
    console.log(`Heading ${this.direction} at ${this.speed}`);
  }
};

var Alligator = function(speed, direction) {
  this.speed = speed,
  this.direction = direction
};

//This is our source object
var alligator = new Alligator('20 mph','North');

alligator = Object.assign(alligator, swim);
alligator2 = Object.assign(alligator, swim);
alligator3 = {...alligator, ...swim};
alligator4 = Object.assign({},alligator, swim);
console.log(alligator.location()); // Heading North at 20 mph
console.log(alligator.a); // 1
alligator.a = 2;
console.log(alligator.a); //2
console.log(alligator2.a); //2 
console.log(alligator3.a); //1
console.log(alligator4.a); //1
console.log(alligator === alligator2); // true
```
```javascrip
// pour un joli schéma d'ensemble sur http://www.objectplayground.com/
Gator1 = {
   name:'',
   biteCount:0
}

Gator2 = {
   name:'',
   biteCount:0
}

console.dir(Gator2);
function publicHi() {
  this.sayHi = function () {
    return `Hi! I'm ${ this.name }`;
  }
}
function publicBite() {
  this.bite = function () {
    return `Yum yum!..( ${ ++this.biteCount } )`;
  }
}
//publicHi.call(Gator2.prototype);
//publicBite.call(Gator2.prototype);


function invokeBehavior(item) {
  item.sayHi();

  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();
  item.bite();

  item.sayHi();
}

this.Gator2 = Gator2;
aly = Object.create(Gator2)
publicHi.call(aly.__proto__);
publicBite.call(aly.__proto__);
invokeBehavior(aly);
aly2 = Object.create(Gator2)
invokeBehavior(aly2);
this.aly = aly;
this.aly2 = aly2;
this.Gator1 = Gator1
var publicBite2 = { bite: function(){++this.biteCount}    }
var publicHi2= { sayHi: function(){} }
GatorChild = {...Gator1, ...publicBite2, ...publicHi2};
this.GatorChild = GatorChild;
aly3 = Object.create(GatorChild)
invokeBehavior(aly3);
this.aly3 = aly3;
var swim = {
  a:1,
  location() {
    console.log(`Heading ${this.direction} at ${this.speed}`);
  }
};

var Alligator = function(speed, direction) {
  this.speed = speed,
  this.direction = direction
};

//This is our source object
var alligator = new Alligator('20 mph','North');

alligator = Object.assign(alligator, swim);
alligator2 = Object.assign(alligator, swim);
alligator3 = {...alligator, ...swim};
alligator4 = Object.assign({},alligator, swim);

this.Alligator = Alligator
this.alligator = alligator
this.alligator2  = alligator2;
this.alligator3 = alligator3;
this.alligator4 = alligator4;
```

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

## Héritage

## Composition

juste une information la composition par Object.assign opère une shallow copy (on pointe sur la meme zone), par contre ... opère une deepCopy voir dans les tests personnels

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
console.log(alligator.location()); // Heading North at 20 mph
console.log(alligator.a); // 1
alligator.a = 2;
console.log(alligator.a); //2
console.log(alligator2.a); //2 
console.log(alligator3.a); //1
```
```

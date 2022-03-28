## Général

### Article 
https://nickmeldrum.com/blog/decorators-in-javascript-using-monkey-patching-closures-prototypes-proxies-and-middleware

- Deux patterns intéressants
 middleware : https://nickmeldrum.com/assets/scripts/decorator-middleware.js
 
 ```javascript
 'use strict'

function myComponentFactory() {
    let suffix = ''
    const instance = {
        setSuffix: suff => suffix = suff,
        printValue: value => console.log(`value is ${value + suffix}`),
        addDecorators: decorators => {
            let printValue = instance.printValue
            decorators.slice().reverse().forEach(decorator => printValue = decorator(printValue))
            instance.printValue = printValue
        }
    }
    return instance
}

function toLowerDecorator(inner) {
    return value => inner(value.toLowerCase())
}

function validatorDecorator(inner) {
    return value => {
        const isValid = ~value.indexOf('My')

        setTimeout(() => {
            if (isValid) inner(value)
            else console.log('not valid man...')
        }, 500)
    }
}

const component = myComponentFactory()
component.addDecorators([validatorDecorator, toLowerDecorator])
component.setSuffix('!')
component.printValue('My Value')
component.printValue('Invalid Value')

 ```
 NOTESTT: dans la fonction décorée retournée value prend la valeur de la fonction initiale qui recoit elle même une valeur
 
 
 prototype : https://nickmeldrum.com/assets/scripts/decorator-inheritance.js
 ```javascript
 'use strict'

function myComponentFactory() {
    let suffix = ''

    return {
        setSuffix: suf => suffix = suf,
        printValue: value => console.log(`value is ${value + suffix}`)
    }
}

function toLowerDecorator(inner) {
    const instance = Object.create(inner)
    instance.printValue = value => inner.printValue(value.toLowerCase())
    return instance
}

function validatorDecorator(inner) {
    const instance = Object.create(inner)
    instance.printValue = value => {
        const isValid = ~value.indexOf('My')

        setTimeout(() => {
            if (isValid) inner.printValue(value)
            else console.log('not valid man...')
        }, 500)
    }
    return instance
}

const component = validatorDecorator(toLowerDecorator(myComponentFactory()))
component.setSuffix('!')
component.printValue('My Value')
component.printValue('Invalid Value')
```
 
 A noter dans les commentaires les references à Eliott qui contribue à faire émerger ce pattern
 ``` javascript
 // doing prototypal inheritance by doing 
 Object.assign(Object.create(animal) {...}) 
 ```
 et le contre argument d'un certain [Jeff M] (https://disqus.com/by/disqus_AeSJxF75RE/)
 
 Quand on suit la vidéo de [Mattias](https://www.youtube.com/watch?v=wfMtDGfHWpA) FunFunFunction
 
 on aboutit aussi à ce [commentaire](https://www.youtube.com/channel/UCOzUngjU_lZ746Fo_TfxfzQ) pour un pattern tres propre
 
> Composition gets better with Object Spread (sugar for Object.assign):
> return {...barker(state), ...driver(state), ...killer(state)}
> Spread operators are not just sugar for Object.assign. Object.assign mutates the given object, thus may trigger object setters while spread operator creates a brand new iterable making it more useful with immutable techniques.

 
 

### Exemples concrets
elbywan.github.io/yett/  A small webpage library to control the execution of (third party) scripts TRES INTERRESSANT CAR FAIT DU MONKEY PATCH

### Les fonctions traditionnelles

[https://github.com/getify/TNG-Hooks](https://github.com/getify/TNG-Hooks) : Provides React-inspired 'hooks' like useState(..) for stand-alone functions 

> semble tres compliqué mais tres intéressant très récent, le développeur est bon

[runtime-hooks](https://github.com/gaoding-inc/runtime-hooks) : simple fonctionnel court, 3 ans d'existence

[Patchwork.js](https://github.com/jamesallardice/patchwork.js/) : Monkey-patch native JavaScript constructor functions 

> pas mal à tester mais vieux à plus de 9 ans

[javascript-hooker](https://github.com/cowboy/javascript-hooker) : vieux de 11 ans

[monkeyify](https://github.com/zenboss/monkeyify) : Monkey patch factory 

#### OO

[yo-next](https://github.com/mishoo/yo-next) : Lite “syntactic sugar” for OO/monkey-patching in JavaScript 

>vieux de 2 ans 

### MOCK
https://github.com/simotae14/js-mocking-fundamentals/blob/master/src/no-framework/spy.js cette fonction me semble parfaitement suffisante pour faire le mock

### Le cas particulier de fetch
https://blog.logrocket.com/intercepting-javascript-fetch-api-requests-responses/ : bon article qui réference le projet suivant
https://github.com/werk85/fetch-intercept

## Note pour tampermonkey

https://github.com/chocolateboy/gm-compat : Portable monkey-patching for userscripts , toutefois bien lire ceci https://groups.google.com/g/greasemonkey-users/c/mow6fpWmIvE, sauf erreur grant none permet à window d être le unsafe window

## Articles

https://medium.com/@FloSloot/js-proxy-how-to-monkey-patch-without-breaking-libraries-3fb2b12434d3 : un article qui ne m'aide pas beaucoup

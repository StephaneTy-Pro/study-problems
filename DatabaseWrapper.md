Objectif permettre d'avoir une couche d'abstraction au moteur de base de donnée des applications

## ORM
invoque la notion de activeRecored ou datamapper https://github.com/typeorm/typeorm
exemple https://github.com/typeorm/typeorm TypeORM is an ORM that can run in NodeJS, Browser, Cordova, PhoneGap, Ionic, React Native, NativeScript, Expo, and Electron platforms and can be used with TypeScript and JavaScript (ES5, ES6, ES7, ES8). Its goal is to always support the latest JavaScript features and provide additional features that help you to develop any kind of application that uses databases - from small applications with a few tables to large scale enterprise applications with multiple databases.
https://www.js-data.io/docs/home JSData is a framework-agnostic, datastore-agnostic ORM (Object-Relational Mapper) for Node.js and the Browser.
https://mikro-orm.io/docs/faq
https://www.sitepoint.com/javascript-typescript-orms/


INFO a garde : https://lodash.com/docs/#get permet de trouve la valeur d'un objet profondemment

### ODM
https://github.com/topics/odm?l=javascript
https://github.com/konsultaner/jsonOdm A JSON ODM (object document mapper) for JavaScript to use on the server or in the browser. 
https://github.com/yuval-a/derivejs DeriveJS is a reactive ODM - Object Document Mapper - framework, a "wrapper" around a database, that removes all the hassle of data-persistence by handling it transparently in the background, in a DRY manner. 
https://github.com/limzykenneth/DynamicRecord  A bare minimum Javascript implementation of the Active Record pattern 

### DATAMAPPER
https://github.com/js-data/js-data Give your data the treatment it deserves with a framework-agnostic, datastore-agnostic JavaScript ORM built for ease of use and peace of mind. Works in Node.js and in the Browser. Main Site: http://js-data.io, API Reference Docs: http://api.js-data.io/js-data

## DB
https://github.com/SebastianSpeitel/proxystore intéressant semble permettre de créer ses propres types de store
https://github.com/localForage/localForage/blob/master/src/localforage.js intéressant car définit des drivers

https://jsrepos.com/catalog/javascript-storage_newest_1
https://bestofjs.org/projects?tags=browser-storage

## Proxies

### Articles
https://k33g.gitlab.io/articles/2020-01-05-JS-PROXY.html (premier parce que français)
https://blog.logrocket.com/use-es6-proxies-to-enhance-your-objects/ : bon exemple avec des functionenrichies
https://medium.com/dailyjs/how-to-use-javascript-proxies-for-fun-and-profit-365579d4a9f8 : idem avec notamment des functions de recherche enrichies
https://blog.risingstack.com/writing-a-javascript-framework-data-binding-es6-proxy/ => https://github.com/nx-js/observer-util
https://devinduct.com/blogpost/36/javascript-proxy-and-its-benefits-let-s-create-an-api-wrapper


https://www.javascripttutorial.net/es6/javascript-proxy/
https://indepth.dev/posts/1361/getting-started-with-modern-javascript-proxy

Moins bons
https://amirmustafaofficial.medium.com/javascript-es6-proxy-api-c58a92f475f1

Génériques
https://javascript.info/proxy
https://stackoverflow.com/questions/49116507/using-proxy-with-wrapper-objects-around-primitives

### Librairies
https://github.com/tomgb/wrap-ware A middleware wrapper which works with promises 
https://github.com/Weedshaker/ProxifyJS A pure ES6 api:any - handler library backed by ECMAScript Harmony Proxy. Proxifies any object and goes far beyond... 
https://github.com/Weedshaker/ProxifyJS_Tutorials
https://github.com/ElliotNB/observable-slim Observable Slim is a singleton that utilizes ES6 Proxies to observe changes made to an object and any nested children of that object. It is intended to assist with state management and one-way data binding. 
https://github.com/nx-js/observer-util : Transparent reactivity with 100% language coverage. Made with heart and ES6 Proxies. 
https://github.com/dashersw/regie : An observable state management tool for vanilla JS applications based on Proxies 
https://github.com/elbywan/hyperactiv  A super tiny reactive library


## A noter
https://github.com/elbywan/normaliz : permet de transformer (normaliser) un json vers un autre json selon un schémar de conversion (on fait du mapping ) : A tiny library that normalizes data. factory
https://github.com/SebastianSpeitel/bborg bahs management for borg
https://elbywan.github.io/yett/  A small webpage library to control the execution of (third party) scripts TRES INTERRESSANT CAR FAIT DU MONKEY PATCH

## Bordel
https://github.com/laggingreflex/debug-any-level
https://github.com/debug-js/debug
https://github.com/nutes-uepb/query-strings-parser Middleware to transform query strings in a format that is recognized by the MongoDB, MySQL and other databases... 

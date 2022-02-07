# Qu'est ce donc
https://web.dev/css-module-scripts/
https://web.dev/css-module-scripts/
https://css-tricks.com/the-many-ways-to-include-css-in-javascript-applications/ (moins bon)

# a propos de la compilation
esbuild ne le compilera pas cf https://github.com/evanw/esbuild/issues/20

# insertion dynamique
https://stackoverflow.com/questions/8209086/javascript-can-i-dynamically-create-a-cssstylesheet-object-and-insert-it

# a propos de construct-style-sheets
supporté uniquement par chrome pour le moment
possibilité sour firefox d'utiliser un polyfill
https://github.com/calebdwilliams/construct-style-sheets

# Idée

## Créer des object js pour les exploiter en css pour pouvoir bénéficier des exports 

- https://github.com/robinweser/inline-style-transformer
- https://github.com/jxnblk/object-style : mis à part que j'ai du patcher le source (jsDelivr n'intégrant pas le module de calcul  de hash) ça fonctionne ; cependant le résultat n'est pas très bon, il génère une nom pour chaque règle, et interprete tres mal certaines règles comme le +

```js
var { className1, css1 } = objectSyle({
   
  ".container": { display: "flex" },
  ".item": { flexGrow: 1, height: "100px" },
  ".item + .item": { marginLeft: "2%" }

})

renvoie 
$Object{
  "className": "xp3vdxn x1lcqcjh x10wj2ge xr63qfo",
  "rules": [
    ".xp3vdxn.container{display:flex}",
    ".x1lcqcjh.item{flex-grow:1}.x10wj2ge.item{height:100px}",
    ".xr63qfo.item + .item{margin-left:2%}"
  ],
  "css": ".xp3vdxn.container{display:flex}.x1lcqcjh.item{flex-grow:1}.x10wj2ge.item{height:100px}.xr63qfo.item + .item{margin-left:2%}"
}

```

- https://github.com/soxtoby/StyleMap
- https://github.com/tomhodgins/csson
- https://github.com/rodydavis/object-dom :
 ça aurait pu fonctionner mais en ce qui concerne le style on a besoin d'un fichier texte que l'on va écrire cf https://rodydavis.github.io/object-dom/
 ```js
import myStyle from './json/style_1.json';
 render(new Style({text: `${myStyle}`}), document.body.querySelector("#sttPlaceHolder"));
 ```
 le probleme c'est que le fichier json est integré par esbuild en tant que fichier json, et donc on pas du texte mais des objets, je ne peux pas les inscrire directement il faudrait les convertir en texte
 
 ceci dit esbuild permet d'ingégrer un fichier texte
 
  ```js
 import myStyleTxt from './json/style_1.txt';
 render(new Style({text: `${myStyle}`}), document.body.querySelector("#sttPlaceHolder"));
 ```
 - https://github.com/giuseppeg/style-sheet : Fast styles in JavaScript with support for static CSS extraction.  
 Pourrait convenir mais visiblement quelques soucis sur la génération du nom des classes, or dans mon cas il ne doit surtout pas les générer
 - https://gist.github.com/mrjjwright/982302/e97b8db90e6b81aae23bed2b71e02133b8651162
 Un coffee script qui permet simplement de générer du css à partir d'un objet ; ça donne ça converti
 ```js
 (function() {
  var root, _css;

  _css = {
    toCSS: function(obj) {
      var css, declaration, declarations, propToCSS, property, selector, value;
      propToCSS = function(key, value) {};
      css = '';
      for (selector in obj) {
        declarations = "";
        declaration = obj[selector];
        for (property in declaration) {
          value = declaration[property];
          if (_.isNumber(value)) {
            value = value + "px";
          }
          declarations += property + ": " + value + ";\n";
        }
        css += selector + " {\n" + declarations + "}\n\n";
      }
      return css;
    }
  };

  root = this;

  if ((typeof window === "undefined" || window === null) && (typeof module !== "undefined" && module !== null)) {
    module.exports = _css;
  } else if (root._ != null) {
    root._.mixin(_css);
  } else {
    root._ = _css;
  }

}).call(this);
```

- https://github.com/remarkablemark/style-to-object 
mais utilise une autre librairie pour générer le code donc à voir si c'est vraiment intéressant

(on s'éloigne un peu du sujet car on parle de parsing ccs)
- https://github.com/tomhodgins/parse-css
- https://github.com/tabatkins/parse-css/blob/master/parse-css.js
- https://github.com/tomhodgins/process-css

ici ausssi on s'éloinge mais dans une autre direction
- https://github.com/asmcss/assembler

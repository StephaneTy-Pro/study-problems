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

(on s'éloigne un peu du sujet car on parle de parsing ccs)
https://github.com/tomhodgins/parse-css
https://github.com/tabatkins/parse-css/blob/master/parse-css.js
https://github.com/tomhodgins/process-css

ici ausssi on s'éloinge mais dans une autre direction
https://github.com/asmcss/assembler

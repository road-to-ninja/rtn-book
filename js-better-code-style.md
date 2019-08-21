---
slideOptions:
  transition: slide
---

<!-- .slide: data-background="#1A237E" -->
# Better code style

---

<!-- .slide: data-background="#1A237E" -->
## Eslint

- Airbnb
- Google
- Eslint recommanded

----

### Trends

Airbnb
[Here](http://www.npmtrends.com/eslint-config-google-vs-eslint-config-airbnb-base-vs-eslint-config-recommended)

----

### Commons
- Tabs: 2-Spaces
- Quotes: Single
- Brace Style for Control Blocks: Same Line
- Prefer Const over var: Yes
- No Trailing Spaces: True


----

### Array 
```
- array-callback-return (itérations retour obligatoire)
- new line when array brackets are open
```

----

### Array 
```javascript=
[1, 2, 3].map(x => x + 1); // retour obligatoire, pas de parathèses 
// utiliser map, reduce ...

const numberInArray = [1, 2, 3];
// nouvelle lignes 
```

----

### Blocks 
```
- Utiliser les accolades.
- Else même ligne que if ouvert (brace-style)
- Eviter les return inutiles
```

----

### variables
```javascript=
const good = ''; // ne peut être re assigné
let ok = '';
var bad = '';
```

----

### Controles

```javascript=
if (
  foo === 123
  && bar === 'abc'
) {
  thing1();
}
```
```
- Refactoriser en fonctions boolean
```

----

### Objects 
```javascript=
const sw = {
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
};
```
```
- shorthand en premier
- espaces entre les éléments
- prefer-destructuring
- comma-dangle (git)
```

----

### Spaces
```
- operators
- space-before-blocks
```

----

### imports
```
- saut de ligne après tous les import
- espaces entre accolades
- multiple lignes
```

----

### strings
```
- utiliser les litérals template
- pas d'espaces entre paramètres {}
- single quotes
```

----

### iterators
```
- essayer d'utiliser map, reduce, filter 
- plutot que for (of)
```

----

### autres
```
- no consoles.log()
- Use indentation when making long method chains (2 max)
- max length line 100 ou 80
- no _ for private (doesnt exist in js)
- new line after statements, blocks (if, functions)
```


---

<!-- .slide: data-background="#1A237E" -->
## Tslint

- Angular
- tslint-eslint-rules

----

#### Properties and methods

```
- Lower camel
- Avoid prefixing private properties and methods with an underscore.

```

----

#### Imports 

```
- Consider leaving one empty line between third party imports and application imports.

```

---

## Compléments 

```
- Naming files : route.user.js or route-user.js avoid routeUser.js
```






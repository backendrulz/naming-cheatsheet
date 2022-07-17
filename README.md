<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet" style="text-decoration: none">
    <img src="./naming-cheatsheet.png" alt="Logo cheatsheet de nombres" />
  </a>
</p>


# Cheatsheet de nombres

- [El idioma inglés](#el-idioma-inglés)
- [Convención de nomenclatura](#convención-de-nomenclatura)
- [C-I-D](#c-i-d)
- [Evitar abreviar](#evitar-abreviar)
- [Evitar la duplicación de contexto](#evitar-la-duplicación-de-contexto)
- [Reflejar el resultado esperado](#reflejar-el-resultado-esperado)
- [Nomenclatura de funciones](#nomenclatura-de-funciones)
  - [Patrón A/HC/LC](#patrón-ahclc)
    - [Acciones](#acciones)
    - [Contexto](#contexto)
    - [Prefijos](#prefijos)
- [Singulares y Plurales](#singulares-y-plurales)
- [Traducción al español](#traducción-al-español)

---

Nombrar cosas es difícil. Este documento intenta hacerlo más fácil.

Aunque estas sugerencias se pueden aplicar a cualquier lenguaje de programación, usaré JavaScript para ilustrarlas en la práctica.

## El idioma inglés

Usa el idioma inglés al nombrar tus variables y funciones.

```js
/* Malo */
const primerNombre = 'Gustavo'
const amigos = ['Kate', 'John']

/* Bueno */
const firstName = 'Gustavo'
const friends = ['Kate', 'John']
```

> Nos guste o no, el inglés es el lenguaje dominante en la programación: la sintaxis de todos los lenguajes de programación está escrita en inglés, así como innumerables documentaciones y materiales educativos. Al escribir su código en inglés, aumenta drásticamente la consistencia.

## Convención de nomenclatura

Elije **una** convención de nomenclatura y síguela. Puede ser `camelCase`, `PascalCase`, `snake_case` o cualquier otra, siempre y cuando siga siendo consistente. Muchos lenguajes de programación tienen sus propias tradiciones con respecto a las convenciones de nomenclatura; ¡Consulta la documentación de tu idioma o estudia algunos repositorios populares en Github!

```js
/* Malo */
const page_count = 5
const shouldUpdate = true

/* Bueno */
const pageCount = 5
const shouldUpdate = true

/* Bueno también */
const page_count = 5
const should_update = true
```

## C-I-D

Un nombre debe ser _corto_, _intuitivo_ y _descriptivo_:

- **Corto**. Un nombre no debe tardar mucho en escribirse ni en recordar;
- **Intuitivo**. Un nombre debe leerse de forma natural, lo más cerca posible del habla común;
- **Descriptivo**. Un nombre debe reflejar lo que hace/posee de la manera más eficiente.

```js
/* Malo */
const a = 5 // "a" could mean anything
const isPaginatable = a > 10 // "Paginatable" sounds extremely unnatural
const shouldPaginatize = a > 10 // Made up verbs are so much fun!

/* Bueno */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternatively
```

## Evitar abreviar

**No** uses abreviaciónes. Contribuyen a nada más que a la disminución de la legibilidad del código. Encontrar un nombre breve y descriptivo puede ser difícil, pero la abreviación no es una excusa para no hacerlo.

```js
/* Malo */
const onItmClk = () => {}

/* Bueno */
const onItemClick = () => {}
```

## Evitar la duplicación de contexto

Un nombre no debe duplicar el contexto en el que se define. Eliminar siempre el contexto de un nombre si eso no disminuye su legibilidad.

```js
class MenuItem {
  /* El nombre del método duplica el contexto (que es "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Se lee muy bien como `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Reflejar el resultado esperado

Un nombre debe reflejar el resultado esperado.

```jsx
/* Malo */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Bueno */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# Nomenclatura de funciones

## El patrón A/HC/LC (A/CA/CB)

Hay un patrón útil a seguir al nombrar funciones:

```
prefijo? + acción (A) + contexto alto (CA) + contexto bajo? (CB)
```

Echa un vistazo a cómo se puede aplicar este patrón en la tabla a continuación.

| Nombre                   | Prefijo   | Acción (A) | Contexto alto (CA) | Contexto bajo (CB) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Nota:** El orden del contexto afecta el significado de una variable. Por ejemplo, `shouldUpdateComponent` significa que _tu_ estás a punto de actualizar un componente, mientras que `shouldComponentUpdate` le dice que _component_ se actualizará por sí mismo, y tu sólo controlas cuándo debe actualizarse.
> En otras palabras, **el contexto alto enfatiza el significado de una variable**.

---

## Acciones

La parte del verbo del nombre de la función. La parte más importante responsable de describir lo que la función _hace_.

### `get`

Accede inmediatamente a los datos (es decir, el acceso directo de los datos internos).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> Ver también [compose](#compose).

También puedes usar `get` al realizar operaciones asincrónicas:

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`)
  return user
}
```

### `set`

Establece una variable de forma declarativa, con el valor `A` al valor `B`.

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

Devuelve una variable a su valor o estado inicial.

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `remove`

Elimina algo _de_ alguna parte.

Por ejemplo, si tienes una colección de filtros seleccionados en una página de búsqueda, eliminar uno de ellos de la colección es `removeFilter`, **no** `deleteFilter` (y así es como lo diría naturalmente también en inglés):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> Ver también [delete](#delete).

### `delete`

Borra algo por completo.

Imagina que eres un editor de contenido y hay una publicación de la que deseas deshacerte. Al hacer clic en el botón "Borrar publicación", el CMS realizó una acción `deletePost`, **no** `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> Ver también [remove](#remove).

> **`remove` o `delete`?**
>
> Cuando la diferencia entre `remove` y `delete` no es tan obvia, te sugiero que observes sus acciones opuestas: `add` y `create`.
> La diferencia clave entre `add` y `create` es que `add` necesita un destino, mientras que `create` **no requiere destino**. Tu `add` (añades) un elemento _en algún lugar_, pero no  "`create` (creas)  _a algún lugar_".
> Simplementa empareja `remove` con `add` y `delete` con `create`.
>
> Explicado con detalle [aquí](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962).

### `compose`

Crea nuevos datos a partir de los existentes. Principalmente aplicable a cadenas, objetos o funciones.

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId
}
```

> Ver también [get](#get).

### `handle`

Controla una acción. A menudo se usa al nombrar un método de devolución de llamada (callback).

```js
function handleLinkClick() {
  console.log('Ha cliqueado un link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Contexto

Dominio en el que opera una función.

Una función es a menudo una acción sobre _algo_. Es importante indicar cuál es su dominio operable, o al menos un tipo de datos esperado.

```js
/* Una función pura que opera con primitivos */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Función que opera exactamente en las publicaciones */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> Algunos supuestos específicos del lenguaje pueden permitir omitir el contexto. Por ejemplo, en JavaScript, es común que `filter` opere sobre un Array. Usar `filterArray` sería innecesario.

---

## Prefijos

El prefijo mejora el significado de una variable. Rara vez se usa en nombres de funciones.

### `is`

Describe una característica o estado del contexto actual (generalmente `boolean`).

```js
const color = 'blue'
const isBlue = color === 'blue' // característica
const isPresent = true // estado

if (isBlue && isPresent) {
  console.log('¡El azul está presente!')
}
```

### `has`

Describe si el contexto actual posee cierto valor o estado (generalmente `boolean`).

```js
/* Malo */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Bueno */
const hasProducts = productsCount > 0
```

### `should`

Refleja una declaración condicional positiva (generalmente  `boolean`) junto con una determinada acción.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

Representa un valor mínimo o máximo. Se utiliza para describir límites.

```js
/**
 * Muestra una cantidad aleatoria de publicaciones
 * dentro de los límites mínimos/máximos dados.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

Indica el estado anterior o siguiente de una variable en el contexto actual. Se utiliza cuando se describen transiciones de estado.

```jsx
async function getPosts() {
  const prevPosts = this.state.posts

  const latestPosts = await fetch('...')
  const nextPosts = concat(prevPosts, latestPosts)

  this.setState({ posts: nextPosts })
}
```

## Singulares y plurales

Al igual que un prefijo, los nombres de las variables se pueden hacer singulares o plurales dependiendo de si contienen un solo valor o varios.

```js
/* Malo */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Bueno */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```

## Traducción al español
Traducción realizada por [Cristian Ferreyra](https://github.com/backendrulz/)
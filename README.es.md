# DefinitelyTyped [![Build Status](https://travis-ci.org/DefinitelyTyped/DefinitelyTyped.svg?branch=master)](https://travis-ci.org/DefinitelyTyped/DefinitelyTyped)

[![Join the chat at https://gitter.im/borisyankov/DefinitelyTyped](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/borisyankov/DefinitelyTyped?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> El repositorio de definiciones de TypeScript de alta calidad.

Vea también el sitio web [definitelytyped.org](http://definitelytyped.org), aunque la información en este README está más actualizada.


## ¿Qué son los `declaration files`?

Vea el [Manual de TypeScript](http://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html).


## ¿Cómo los obtengo?

### npm

Este es el método preferido. Solo está disponible para usuarios TypeScript 2.0+. Por ejemplo:

```sh
npm install --save-dev @types/node
```

Los types deberían ser incluidos automaticamente por el compilador.
Vea más en el [manual](http://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html).

Para un paquete NPM "foo", Estos `typings` estarán en "@types/foo".
Si no puedes encontrar tu paquete, búscalo en [TypeSearch](https://microsoft.github.io/TypeSearch/).

Si aún no puedes encontrarlo, comprueba si el paquete ya [incluye](http://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html) los typings.
Esto es provisto usualmente en el campo `"types"` o `"typings"` en el `package.json`,
o solo busca por cualquier archivo ".d.ts" en el paquete e incluyelo manualmente con un `/// <reference path="" />`.


### Otros métodos

Estos pueden ser utilizados por TypeScript 1.0.

* [Typings](https://github.com/typings/typings)
* ~~[NuGet](http://nuget.org/packages?q=DefinitelyTyped)~~ (use las alternativas preferidas, la publicación DT type de nuget ha sido desactivada)
* Descarguelo manualmente desde la `master` branch de este repositorio

Tal vez debas añadir manualmente las [referencias](http://www.typescriptlang.org/docs/handbook/triple-slash-directives.html).


## ¿Cómo puedo contribuir?

¡DefinitelyTyped solo trabaja gracias a contribuidores como tú!

### Prueba

Antes de compartir tu mejora con el mundo, úselo usted mismo.

#### Prueba editando un paquete existente

Para agregar nuevas funciones puedes usar el [module augmentation](http://www.typescriptlang.org/docs/handbook/declaration-merging.html).
También puedes editar directamente los types en `node_modules/@types/foo/index.d.ts`, o copiarlos de ahí y seguir los pasos explicados a continuación.


#### Prueba un nuevo paquete

Añade a tu `tsconfig.json`:

```json
"baseUrl": "types",
"typeRoots": ["types"],
```

(También puedes usar `src/types`.)
Crea un `types/foo/index.d.ts` que contenga declaraciones del módulo "foo".
Ahora deberías poder importar desde `"foo"` a tu código y te enviara a un nuevo tipo de definición.
Entonces compila *y* ejecuta el código para asegurarte que el tipo de definición en realidad corresponde a lo que suceda en el momento de la ejecución.
Una vez que hayas probado tus definiciones con el código real, haz un [PR](#make-a-pull-request)
luego sigue las instrucciones para [editar un paquete existente](#edit-an-existing-package) o
[crear un nuevo paquete](#create-a-new-package).


### Haz un pull request

Una vez que hayas probado tu paquete, podrás compartirlo en DefinitelyTyped.

Primero, haz un [fork](https://guides.github.com/activities/forking/) en este repositorio, instala [node](https://nodejs.org/), y luego ejecuta la `npm install`.


#### Editar un paquete existente

* `cd types/my-package-to-edit`
* Haz cambios. Recuerda editar las pruebas.
* También puede que quieras añadirte la sección "Definitions by" en el encabezado del paquete.
  - Esto hará que seas notificado (a través de tu nombre de usuario en GitHub) cada vez que alguien haga un pull request o issue sobre el paquete.
  - Haz esto añadiendo tu nombre al final de la línea, así como en `// Definitions by: Alice <https://github.com/alice>, Bob <https://github.com/bob>`.
  - O si hay más personas, puede ser multiline
  ```typescript
  // Definitions by: Alice <https://github.com/alice>
  //                 Bob <https://github.com/bob>
  //                 Steve <https://github.com/steve>
  //                 John <https://github.com/john>
  ```
* Si hay un `tslint.json`, ejecuta `npm run lint package-name`. De lo contrario, ejecuta `tsc` en el directorio del paquete.

Cuando hagas un PR para editar un paquete existente, `dt-bot` deberá @-mencionar a los autores previos.
Si no lo hace, puedes hacerlo en el comentario asociado con el PR.


#### Crear un nuevo paquete

Si eres el autor de la librería, o puedes hacer un pull request a la biblioteca, [bundle types](http://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html) en vez de publicarlo en DefinitelyTyped.

Si estás agregando typings para un paquete NPM, crea un directorio con el mismo nombre.
Si el paquete al que le estás agregando typings no es para NPM, asegurate de que el nombre que escojas no genere problemas con el nombre del paquete en NPM.
(Puedes usar `npm info foo` para verificar la existencia del paquete `foo`.)

Tu paquete debería tener esta estructura:

| Archivo | Propósito |
| --- | --- |
| index.d.ts | Este contiene los typings del paquete. |
| foo-tests.ts | Este contiene una muestra del código con el que se realiza la prueba de escritura. Este código *no* es ejecutable, pero sí es type-checked. |
| tsconfig.json | Este permite ejecutar `tsc` dentro del paquete. |
| tslint.json | Permite linting. |

Generalas ejecutando `npm install -g dts-gen` y `dts-gen --dt --name my-package-name --template module`.
Ve todas las opciones en [dts-gen](https://github.com/Microsoft/dts-gen).

También puedes configurar el `tsconfig.json` para añadir nuevos archivos, para agregar un `"target": "es6"` (necesitado por las funciones asíncronas), para agregar a la `"lib"`, o para agregar la opción de compilación `"jsx"`.

Los miembros de DefinitelyTyped frecuentemente monitorean nuevos PRs, pero ten en mente que la cantidad de PRs podrian ralentizar el proceso.

Para un buen paquete de ejemplo, vea [base64-js](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/base64-js).


#### Errores comunes

* Primero, sigue el consejo del [manual](http://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html).
* Formatear: Ya sea utilizar todo en tabs, o siempre utiliza 4 espacios.
* `function sum(nums: number[]): number`: Utiliza `ReadonlyArray` si una funcion no escribe a sus parámetros.
* `interface Foo { new(): Foo; }`:
    Este define el tipo de objeto que esten nuevos. Probablemente quieras `declare class Foo { constructor(); }`.
* `const Class: { new(): IClass; }`:
    Prefiere usar una declaración de clase `class Class { constructor(); }` En vez de una nueva constante.
* `getMeAT<T>(): T`:
    Si un tipo de parámetro no aparece en los tipos de ningún parámetro, no tienes una función genérica, solo tienes un afirmación del tipo disfrazado.
     Prefiera utilizar una afirmación de tipo real, p.ej. `getMeAT() as number`.
    Un ejemplo donde un tipo de parámetro es aceptable: `function id<T>(value: T): T;`.
    Un ejemplo donde no es aceptable: `function parseJson<T>(json: string): T;`.
    Una excepción: `new Map<string, number>()` está bien.
* Utilizando los tipos `Function` y `Object` casi nunca es una buena idea. En 99% de los casos es posible especificar un tipo más especifico. Los ejemplos son `(x: number) => number` para [funciones](http://www.typescriptlang.org/docs/handbook/functions.html#function-types) y `{ x: number, y: number }` para objetos. Si no hay certeza en lo absoluto del tipo, [`any`](http://www.typescriptlang.org/docs/handbook/basic-types.html#any) es la opción correcta, no `Object`. Si el único hecho conocido sobre el tipo es que es un objecto, usa el tipo [`object`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-2.html#object-type), no `Object` o `{ [key: string]: any }`.
* `var foo: string | any`:
    Cuando es usado `any` en un tipo de unión, el tipo resultante todavía es `any`. Así que mientras la porción `string` de este tipo de anotación puede _verse_ útil, de hecho, no ofrece ningún typechecking adicional más que un simple `any`.
    Dependiendo de la intención, una alternativa aceptable puede ser `any`, `string`, o `string | object`.


#### Remover un paquete

Cuando un paquete [bundles](http://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html) sus propios tipos, estos tipos deberán ser removidos de DefinitelyTyped para evitar que generen confusión.

Se puede remover ejecutando `npm run not-needed -- typingsPackageName asOfVersion sourceRepoURL [libraryName]`.
- `typingsPackageName`: Este es el nombre del directorio que tienes que eliminar.
- `asOfVersion`: Un stub será publicado a `@types/foo` con esta versión. Debería ser más grande que cualquier versión publicada actualmente.
- `sourceRepoURL`: Esto debería señalar el repositorio que contiene los typings.
- `libraryName`: Un nombre descriptivo de la librería, p.ej. "Angular 2" en vez de "angular2". (Si es omitido, será idéntico a "typingsPackageName".)

Cualquier otro paquete en DefinitelyTyped que referencie el paquete eliminado deberá ser actualizado para referenciar los tipos bundled. para hacer esto, añade `package.json` con `"dependencies": { "foo": "x.y.z" }`.

Si un paquete nunca estuvo en DefinitelyTyped, no será necesario añadirlo a `notNeededPackages.json`.


#### Lint

Para realizar el lint a un paquete, solo añade `tslint.json` al paquete que contiene `{ "extends": "dtslint/dt.json" }`. Todos los paquetes nuevos deberán pasar por el proceso de linted.
Si el `tslint.json` deshabilita algunas reglas esto se debe a que aún no se ha acomodado. Por ejemplo:

```js
{
    "extends": "dtslint/dt.json",
    "rules": {
        // This package uses the Function type, and it will take effort to fix.
        "ban-types": false
    }
}
```

(Para indicar que la regla lint realmente no es utilizada, usa `// tslint:disable rule-name` o mejor, `//tslint:disable-next-line rule-name`.)

Para afirmar que una expresión es de un tipo dado, utilice `$ExpectType`. Para afirmar que una expresión causa un error de compilación, utilice `$ExpectError`.

```js
// $ExpectType void
f(1);

// $ExpectError
f("one");
```

Para más detalles, vea el [dtslint](https://github.com/Microsoft/dtslint#write-tests) readme.

Realiza una prueba ejecutando `npm run lint package-name` donde `package-name` es el nombre de tu paquete.
Este script utiliza [dtslint](https://github.com/Microsoft/dtslint).


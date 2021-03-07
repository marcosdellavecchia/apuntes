# TypeScript - Conceptos básicos

## Qué es TypeScript?

TypeScript es un _superset_ de JavaScript. Esto quiere decir que es un lenguaje de programación construído sobre JavaScript que **agrega nuevas features**.

TypeScript corre con una única _desventaja_: no puede ser interpretado por navegadores ni tampoco por Node JS. Es por esto que incluye un **compilador** que nos va a permitir transformar nuestro código de TS en JS.

Dentro de las features que incorpora TypeScript, la más importante son los **types** (que le dan el nombre al lenguaje). Estos ayudan a identificar errores de forma anticipada y nos obligan a ser más claros en nuestras intenciones y así escribir mejor código.

## Instalando TypeScript

```
npm install -g typescript
```

## Un pantallazo: diferencias entre JS y TS

#### **JavaScript Vanilla**

El siguiente código recibe el valor de dos inputs y los suma. Si no especificamos que esos valores son un número, JS los va a interpretar como string resultando en una _concatenación_ en lugar de una suma. Este no es un error técnico, pero es algo que TS nos podría ayudar a prevenir.

```js
const button = document.querySelector("button");
const input1 = document.getElementById("num1");
const input2 = document.getElementById("num2");

function add(num1, num2) {
  return num1 + num2;
}

button.addEventListener("click", function () {
  console.log(add(input1.value, input2.value));
});
```

#### **TypeScript**

El código a continuacíon contiene ciertas aclaraciones que utilizan sintaxis propia de TS para prevenir errores y escribir una mejor versión de nuestro código.

#### Qué se agrega?

- El signo de exclamación (!) al final de cada `document.getElementById()` le indica a TS que estamos seguros de que ese elemento existe y no va a arrojar null.
- La sintaxis `as HTMLInputElement` da el indicio de que lo que estamos declarando es una constante que hace referencia a un objeto de nuestro HTML de tipo input.
- Los signos `+` antecediendo a `input1.value` y `input2.value` no son propios de TypeScript, pero es una aclaración que el lenguaje nos incita a hacer para especificar que se trata de números. En caso de no agregarlo, el IDE nos va a estar marcando un error de sintaxis.

```ts
const button = document.querySelector("button");
const input1 = document.getElementById("num1")! as HTMLInputElement;
const input2 = document.getElementById("num2")! as HTMLInputElement;

function add(num1: number, num2: number) {
  return num1 + num2;
}

button.addEventListener("click", function () {
  console.log(add(+input1.value, +input2.value));
});
```

### Compilando TypeScript

El proceso de compilado consiste en transformar nuestro archivo `.ts` a un archivo `.js` que pueda ser correctamente interpretado por el navegador.
Para compilar tenemos que ubicarnos con la terminal en la carpeta en la que estamos trabajando con nuestro archivo de TS (por ejemplo `prueba.ts`) y ejecutar el siguiente comando:

```
tsc prueba.ts
```

## Ventajas de TypeScript

- **Types**: Nos obligan a ser más explícitos en nuestro código y así evitar errores innecesarios.
- Permite implementar features de **JavaScript moderno** que luego son compilados para funcionar en navegadores antiguos.
- **Interfaces / Generics**: No son compilados en JavaScript, pero nos ayudan a lo largo del desarrollo a la hora de prevenir errores.
- **Meta-Programming** features como por ejemplo los decorators.
- **Opciones de configuración** muy variadas que se ajustan a distintos estilos de programación.
- Soporte a traves de herramientas que funcionan incluso en proyectos que no utilizan TypeScript.

## Extensiones recomendadas para Visual Studio Code:

- ESLint
- Prettier
- Path Intellisense

# Módulo 1. Trabajando con Types

_La principal diferencia es que JavaScript utiliza **tipos dinámicos** que se resuelven en tiempo de ejecución, mientras que TypeScript utiliza **tipos estáticos** que son seteados durante el desarrollo._

## **Core Types**: Types de JavaScript también soportados por TypeScript:

1. number
2. string
3. boolean
4. object
5. array

### Type: _number_

Agregando la sintaxis `: number` a continuación de un elemento, le estamos indicando a TS que el valor que espera ese elemento tiene que ser del tipo número.

Esto es especialmente útil a la hora de definir los parámetros que va a tomar una función, como se ve en el ejemplo a continuación:

```ts
const add = (n1: number, n2: number) => {
  return n1 + n2;
};

const number1 = 5;
const number2 = 2.8;

const result = add(number1, number2);
```

En caso de querer asignar un valor de tipo string en `number 1` o `number 2`, el compilador de TS nos va a arrojar una advertencia e indicanros dónde está lo que tenemos que corregir para prevenir un posible error.

**Si el código contiene errores, TypeScript no lo va a compilar** en un archivo de JavaScript, obligándonos a corregir las incoherencias de nuestro programa.

### Type: _boolean_

Agregando la sintaxis `: boolean` a continuación de un elemento, le estamos indicando a TS que el valor que espera ese elemento tiene que ser del tipo `true` o `false` (_aclaración: no truthy o falsy_).

### Type: _string_

Agregando la sintaxis `: string` a continuación de un elemento, le estamos indicando a TS que el valor que espera ese elemento tiene que ser del tipo cadena de texto.

## _Type Inference_: Por qué no es necesario aclarar el type de TODAS las variables definidas

En TypeScript existe algo llamado **type inference**, que permite que el lenguaje infiera automáticamente el tipo de la variable que estmaos definiendo _cuando a esta se le asigna un valor._

Por ejemplo:

```ts
let number1 = 5;
```

En el ejemplo de arriba, TS ya sabe que la variable number1 debe ser del tipo `number`, y nos arrojaría un error si posteriormente, por algún motivo, quisieramos re-asignarle un valor de otro tipo (por ejemplo `Cinco`, que es un `string`).

Es por este motivo que _no es necesario aclarar el tipo al que pertenece un elemento_ cuando estamos asignando un valor al mismo.

Distinto es si estamos creando una variable, pero sin asignarle un valor (por que, por ejemplo, se lo queremos asignar más adelante). En ese caso **sí es necesario aclarar el type que espera ese elemento**, tal como se ve a continuación:

```ts
let number2: number;
```

### Type: _object_

Cuando trabajamos con objetos de JavaScript tenemos la posibilidad de aclarar que los mismos son del tipo `object`. Sin embargo, esto no es estrictamente necesario, ya que mediante **type inference** TS puede inferir que se trata de un objeto literal e incluso analizar el contenido del mismo.

Supongamos que tenemos el siguiente objeto:

```ts
const perro = {
  nombre: "Max",
  edad: 4,
};
```

Como se puede ver, no aclaramos que se trata de un objeto. Sin embargo, si quisieramos ejecutar el comando `console.log(perro.raza)`, TS automáticamente nos arrojaría error a la hora de compilar, ya que estaría analizando el objeto perro e infiriendo que no existe ningúna clave llamada `raza` dentro del mismo.

#### **Object Types**

Si quisieramos ser más rigurosos a la hora de definir nuestro objeto, podríamos crear un **object type** a través del cual no sólo aclaramos que nuestro elemento efectivamente es un objeto, si no también un detalle de los atributos que contiene y el _type_ de cada uno de ellos:

```ts
const perro: {
  nombre: string;
  edad: number;
} = {
  nombre: "Max",
  edad: 4,
};
```

### Type: _Array_

Cuando trabajamos con arreglos de JavaScript tenemos la posibilidad de aclarar que los mismos son del tipo `array`. Esto podría ser algo sencillo de inferir por TS, pero podemos ir más allá y definir qué tipos de elementos van a conformar ese array mediante la sintaxis `string[]` si se tratara de un arreglo de strings o `number[]` si el mismo estuviera conformado por números.

Ejemplo (con inferencia de tipos de TS):
En el siguiente ejemplo no se aclara que el array `hobbies` es de tipo `string`, pero TS lo infiere automáticamente al estar compuesto en su totalidad por elementos de cadena de texto.

```ts
const perro = {
  nombre: "Max",
  edad: 4,
  hobbies: ['Comer', 'Dormir', 'Mover la cola']
  ]
};
```

Ejemplo (especificando tipo de arreglo):
Si quisieramos ser más estrictos y definir un único tipo de dato que va a poder integrar el string `hobbies`, podríamos definir el mismo de la siguiente forma.

```ts
let hobbies: string[];
hobbies = ["Comer", "Dormir", "Mover la cola"];
```

En este último ejemplo, TS ya da por hecho que cualquier elemento dentro de `hobbies` DEBE ser un `string`, y nos arrojará un error en caso de querer compilar sin respetar esta regla.

Un **beneficio** que nos brinda TS al tener la certeza de los tipos de datos que integran una variable, es poder aplicar métodos propios de ese tipo de dato sin que esto suponga un inconveniente.

Por ejemplo, podríamos iterar el array `hobbies` con un ciclo for y dentro del mismo hacer `console.log(hobbies.toUpperCase())` Esto sería válido, y nos arrojaría cada uno de los elementos luego de haber sido convertido a uppercase. Si dentro de `hobbies` existiera un valor del tipo `number`, sin dudas no podría ser transformado a uppercase, pero gracias a TypeScript tenemos la certeza de que esto no va a pasar.

## TypeScript Types: Types específicos de TS

### Type: _Tuple_

El tipo `tuple` es un `Array` donde la longitud y tipos de dato son fijos.

A diferencia de otros types mencionados anteriormente, en este caso es necesario especificar cómo va a estar conformado nuestro tuple para que TS pueda hacer su trabajo correctamente y ahorrarnos posibles errores a la hora de manipular este array.

Ejemplo:

```ts
const perro: {
  nombre: string;
  edad: number;
  hobbies: string[];
  raza: [number, string];
} = {
  nombre: "Max",
  edad: 4,
  hobbies: ["Comer", "Dormir", "Mover la cola"],
  raza: [4, "Ovejero"],
};
```

En el ejemplo anterior estamos especificando que el array `raza` debe tener exactamente 2 posiciones, donde la primera debe ser un `number` y la segunda un `string`.

### Type: _Enums_

̦̌Los `Enums` son uno de los tipos que admite TypeScript pero que no están soportados por JavaScript (aunque sí están presentes en otros lenguajes de progamación).

Los Enums permiten setear una colección de constantes que pueden ser del tipo `number` o `string`, a las cuales se puede acceder posteriormente.

Siguiendo con el ejemplo anterior:

```ts
const Roles = { GUARDIAN = 1, DE_COMPANIA = 2, CAZADOR = 3 };

const perro = {
  nombre: "Max",
  edad: 4,
  hobbies: ["Comer", "Dormir", "Mover la cola"],
  raza: [4, "Ovejero"],
  rol: Roles.GUARDIAN,
};
```

Es importante tener en cuenta que el código anterior no se va a ver exactamente igual transformado en JavaScript luego de ser compilado, porque los Enums no están soportados por dicho lenguaje.

### Type: _any_

Es el type `any` es el más flexible de todos: no le dice nada a TypeScript acerca de la informacion que puede almacenar un elemento. Por este motivo es desaconsejable usarlo, ya que nos priva de todas las ventajas que ofrece TypeScript a la hora de evalular nuestro código en busca de errores.

Ejemplo:

```ts
let arrayCualquiera = any[];
arrayCualquiera = ['Tomate', 47]
```

## Union Types

En el caso de querer especificar que un parmetro podría esperar tanto un `number` como un `string`, aparecen los Union Types.

Estos se expresan separando los types que podría esperar un parámetro separados por barras verticales "`|`" de la siguiente manera:

```ts
const combinar(input1: number | string, input2: number | string) {
  const resultado = input1 + input2;
  return resultado;
}
```

El problema con el código expresado anteriormente es que TypeScript no va a poder resolver la función si asignamos, por ejemplo, un `string` al input1 y un `number` al input2 de forma simultánea. Por ejemplo: `combinar(2, 'Gato')` arrojaría un resultado no esperado.

Por este motivo, debemos aclarar un **runtime check** a nuestra función:

```ts
const combinar(input1: number | string, input2: number | string) {

  let resultado;

  if (typeof input1 === 'number' && typeof input2 === 'number') {
  resultado = input1 + input2;
  } else {
  resultado = input1.toString() + input2.toString();
  }

  return resultado;
}
```

## Literal Types

En estos types no sabemos si una variable o parametro debe contener un `number` o `string`, pero estamos seguros del **valor exacto que debería contener**.

En el siguiente ejemplo se define una constante que va a determinar si el resultado se debe mostrar como `number` o `string` de acuerdo al valor que contenga la misma:

```ts
function combinar(
  input1: number | string,
  input2: number | string,
  conversionResultado: "as-number" | "as-text"
) {
  let resultado;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    conversionResultado === "as-number"
  ) {
    result = +input1 + +input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}
```

## Alias / Custom Types

TypeScript nos permite crear nuestros propios types customizados que almacenen `unions` o `literal types` (incluso también `objects`) y nos ayuden a reutilizar nuestra lógica escribiendo menos líneas de código.

El ejemplo del caso anterior se podría reescribir de esta forma utilizando `aliases / custom types`:

```ts
type Combinable = number | string;
type DescripcionDelResultado = "as-number" | "as-string";

function combinar(
  input1: Combinable,
  input2: Combinable,
  conversionResultado: DescripcionDelResultado
) {
  let resultado;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    conversionResultado === "as-number"
  ) {
    result = +input1 + +input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}
```

## Function Return Types

Hasta el momento las funciones vistas retornaban un tipo de resultado **inferido** por TypeScript. Sin embargo, también tenemos la posibilidad de especificar de qué tipo es el resultado que estamos esperando.

Esto se podría lograr agregando `: number` o `: string` inmediatamente después de definir los parámetros de la función.

De esta forma, si quisieramos que la función nos retorne una cadena de texto, el ejemplo a continuación concatenaría el resultado en lugar de sumarlo:

```ts
function agregar(n1: number, n2: number): string {
  return n1 + n2;
}
```

### El return type _`void`_

Cuando una función no tiene un return, en TypeScript se dice que retorna `void` _(vacío)_.

En el ejemplo a continuación, se agrega el sufijo `: void` luego de definir los parámetros de la función para que sea lo más explícito posible. Sin embargo, TypeScript es capaz de inferir sin problemas que el return de la función es de este tipo, y no sería necesario incluirlo.

```ts
function agregar(n1: number, n2: number) {
  return n1 + n2;
}

function imprimirResultado(num: number): void {
  console.log("Result: " + num);
}

imprimirResultado(agregar(5, 12)); // Imprime en pantalla 17
```

## Function Types

TypeScript nos da la posibilidad de especificar que una variable que estamos definiendo debe contener dentro de si una función. Esto es sencillo de lograr con la sintaxis `: Function` de la siguiente manera:

```ts
function agregar(n1: number, n2: number) {
  return n1 + n2;
}

let combinarValores = agregar: Function;
```

Sin embargo, esto no le dice nada a TypeScript acerca de la cantidad de parámetros que debe recibir esa función, ni tampoco el type de dichos parámetros.

Por este motivo, existe una sintaxis _similar a la de una arrow function_ que nos permite lograr este cometido:

```ts
function agregar(n1: number, n2: number) {
  return n1 + n2;
}

let combinarValores: (a: number, b: number) => number;

combinarValores = agregar;
```

En el ejemplo anterior especificamos la _cantidad de argumentos, el tipo de los mismos y el tipo de elemento que va a retornar la función_. Esto lo hacemos sin utilizar el sufijo `: Function`, si no con una sintaxis similar a la de una función flecha de JavaScript moderno.

## Unknown type

El tipo `unknown` podría parecer similar a `any`, aunque en realidad es un poco más restrictivo (y por ende, más recomendable que el anterior).

`unknown` asume que no sabemos qué valor va a almacenarse en él, pero sí **qué es lo que queremos hacer con ese valor**.

### Entonces... Cuál es la diferencia entre `unknown` y `any`?

Cualquer type puede asignarse a `unknown`, pero `unknown` puede ser asignado únicamente a `any` y a sí mismo. Esto obliga a realizar chequeos antes de efectuar una asignación de variables.

Esto no sucede con `any`, que escapa de cualquier tipo de control por parte de TypeScript (y por eso es menos recomendable su utilización).

## Never type

Al igual que `void`, `never` es otro type que puede ser retornado por funciones.

Este type es relativamente nuevo, y por este motivo, no es inferido por TypeScript si no lo aclaramos. Se utiliza para funciones que **nunca retornan un valor**, como podría ser el siguiente ejemplo:

```ts
function generarError(message: string, code: number): never {
  throw { message: message, errorCode: code };
}
```

Si hicieramos un console.log de esta funcion, veríamos que no devuelve nada (ni siquiera _undefined_). Es por eso que el type `never`, si bien no es obligatorio, forma parte de una buena práctica del código en TypeScript.

# Compilación en TypeScript

## Watch mode

Para hacer menos engorroso el proceso de compilado y no tener que realizarlo cada vez que efectuamos un cambio en nuestro código, existe el **watch mode**.

El watch mode va a estar monitoreando de manera constante los cambios en nuestro documento, y realizar el proceso de compilado **cada vez que guardemos nuestro archivo**.

### Compilar en watch mode para un **único archivo**:

```
tsc app.js -w
```

### Compilar en watch mode para **todo el proyecto**:

```
tsc --init  // Sólo requerido para inicializar el proyecto
tsc         // Se ejecuta para compilar todos los archivos .ts
tsc -w      // Se ejecuta para compilar todos los archivos .ts en watch mode

```

## Incluir y excluir archivos de la compilación

Excluir un archivo del proceso de compilado:

**En el archivo tsconfig.json**

```json
"exclude": [
"node_modules"     // Si no se aclara, viene por default
"prueba.ts"
]
```

Incluir un archivo (o directorio) específico en el proceso de compilado:

**En el archivo tsconfig.json**

```json
"include": [
"prueba.ts"
]
```

## Configurando el _compilation target_

El compilation target hace referencia a la versión de JavaScript que va a utilizar el compilador a la hora de transformar nuestro código. Esto se determina en la propiedad `"compilerOptions"` dentro del archivo **tsconfig.json**

Por ejemplo, `"compilerOptions":{"target": "es5"}` utilizaría `var` para definir variables, mientras que `"compilerOptions":{"target": "es6"}` podría utilizar `let` y `const`, que son admitidas por la versión EcmaScript6 de Javascript.

Esto no es algo menor y representa uno de los **puntos fuertes de TypeScript**, ya que no sólo nos puede ayudar a evitar errores y escribir mejor código, sino también a **hacer que nuestras aplicaciones se puedan adaptar a distintos requisitos de compatibilidad**.

## outDir y rootDir

Dentro del archivo `tsconfig.json` podemos encontrar dos archivos de configuración que nos pueden ser de utilidad a la hora de compilar:

- `"outDir:"` va a especificar la ruta en la cual se almacenarán los archivos .js compilados. Por ejemplo: `./dist/`
- `"rootDir:"` va a especificar la ruta en la cual el compilador buscará los archivos .ts a compilar. Por ejemplo: `./src/`. Además, la estructura de carpetas en `rootDir` se va a ver replicada en el `outDir` en el cual se almacenen los archivos .js finales.

## No emitir compilaciones con errores

Agregando la línea `"noEmitOnError": true` a nuestro archivo `tsconfig.json` nos vamos a asegurar de que el sistema no emita ningun archivo de JavaScript cuya compilación haya arrojado cualquier tipo de error.

## Strict compilation

El archivo `tsconfig.json` contiene una línea con el atributo "`strict": true`.

Al estar en `true` por defecto, la strict compilation activa todas las siguientes _strict options_:

```json
    "noImplicitAny": true,                       /* Raise error on expressions and declarations with an implied 'any' type. */
    "strictNullChecks": true,                    /* Enable strict null checks. */
    "strictFunctionTypes": true,                 /* Enable strict checking of function types. */
    "strictBindCallApply": true,                 /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    "strictPropertyInitialization": true,        /* Enable strict checking of property initialization in classes. */
    "noImplicitThis": true,                      /* Raise error on 'this' expressions with an implied 'any' type. */
    "alwaysStrict": true,                        /* Parse in strict mode and emit "use strict" for each source file. */
```

# Clases e Instancias en TypeScript

El concepto de **clases** tiene su origen en la _Programación Orientada a Objetos_, y hace referencia a los **modelos que definen un conjunto de variables y métodos a ser utilizados por determinados objetos**.

Las clases nos permiten definir como se debería ver un objeto, los métodos que debería tener, los datos que debería almacenar, etc.

Cada objeto que se crea a partir de una clase se denomina **instancia**.

Las clases existen para acelerar y facilitar el proceso de creación de objetos que tienen la misma estructura y métodos, y que difieren únicamente en su información contenida.

## Creando una clase

A modo de ejemplo, creamos una clase `Departamento` y le agregamos el atributo `name`, haciendo referencia a los distintos departamentos que pueden existir en una empresa.

```ts
class Departamento {
  name: string;
  constructor(n: string) {
    this.name = n;
  }
}
```

Creamos una instancia de la clase `Departamento` y la imprimimos por consola:

```ts
const contabilidad = new Departamento("Contabilidad");

console.log(contabilidad);
// imprime el objeto: Departamento {name: "Contabilidad" }
```

### Cómo se ven nuestras clases e instancias compiladas en en JavaScript?

**Así se compila en ES5**

```js
var Departamento = (function () {
  function Departamento(n) {
    this.name = n;
  }
  return Departamento;
})();
var contabilidad = new Departamento("Contabilidad");
console.log(contabilidad);
```

**Así se compila en ES6**

```js
"use strict";
class Departamento {
  constructor(n) {
    this.name = n;
  }
}
const contabilidad = new Departamento("Contabilidad");
console.log(contabilidad);
```

Una vez más se aprecia cómo TypeScript adapta nuestro código según los parámetros establecidos en el archivo `tsconfig.json`.

# TypeScript - Conceptos básicos

## Qué es TypeScript?

TypeScript es un _superset_ de JavaScript. Esto quiere decir que es un lenguaje de programación construído sobre JavaScript que **agrega nuevas features**.

TypeScript corre con una única _desventaja_: no puede ser interpretado por navegadores ni tampoco por Node JS. Es por esto que incluye un **compilador** que nos va a permitir transformar nuestro código de TS en JS.

Dentro de las features que incorpora TypeScript, la más importante son los **types** (que le dan el nombre al lenguaje). Estos ayudan a identificar errores de forma anticipada y nos obligan a ser más claros en nuestras intenciones y así escribir mejor código.

## Instalando TypeScript

```
npm install -g typescript
```

### Ejemplo:

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

## Core Types: Types de JavaScript también soportados por TypeScript:

1. number
2. string
3. boolean

### Type: number

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

### Type: boolean

Agregando la sintaxis `: boolean` a continuación de un elemento, le estamos indicando a TS que el valor que espera ese elemento tiene que ser del tipo `true` o `false` (_aclaración: no truthy o falsy_).

### Type: string

Agregando la sintaxis `: string` a continuación de un elemento, le estamos indicando a TS que el valor que espera ese elemento tiene que ser del tipo cadena de texto.

## Type Inference: No es necesario aclarar el type de TODAS las variables definidas

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

### Type: object

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

### Type: Array

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

### Type: Tuple

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

### Type:

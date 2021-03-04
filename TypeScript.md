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

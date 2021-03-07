# Redux - Conceptos básicos y su utilización en React y React Native

## Qué es Redux?

Redux es una librería utilizada para gestionar el **manejo del estado** en aplicaciones construidas en JavaScript.

Si bien esta guía está enfocada en su uso con React, es una librería _framwork agnostic_, por lo cual su utilización es más amplia (podría utilizarse Redux en apps desarrolladas con Angular, o incluso con JavaScript "vanilla").

## Cuándo utilizar Redux?

React incopora de manera nativa varios mecanismos para almacenar información en el estado, los cuales nos permiten manejar el flujo de dicha información a lo largo de nuestra aplicación.

Sin embargo, cuando trabajamos en aplicaciones grandes que implican una cantidad considerable de componentes y pantallas, hacer que la información viaje a través del estado de un lugar a otro puede volverse algo tedioso y complicado.

En esos escenarios es cuando Redux se vuelve realmente útil, ya que trabaja con una **única fuente de verdad**. Esta fuente se denomina **store**, y es ni más ni menos que un objeto que unifica todo el estado de nuestra aplicación.

---

_**Esta guía no se enfoca en explicar la teoría de Redux, si no en su implementación en una app construida con React Native. Los ejemplos están basados en la aplicación Meals App de Maximilian Schwartzmüller.**_

---

## 0. Estructura de archivos sugerida para trabajar con Redux en React Native

```
.
├── App.js
├── store
│   ├── actions
│   └── reducers
├── components
│   ├── component-category1
│   └── component-category2
├── screens
│   ├── screen-category1
│   └── screen-category2
├── navigation
├── constants
├── assets
├── data
└── app.json
└── package.json
```

## 1. Instalación de Redux y React-Redux

```
npm install redux react-redux
```

## 2. Creando nuestro reducer

`./store/reducers/meals.js`

```js
import { MEALS } from "../../data/meals.js";

const initialState = {
  meals: MEALS,
  filteredMeals: MEALS, // Initially no filters setted up
  favoriteMeals: [],
};

const mealsReducer = (state = initialState, action) => {
  return state;
};

export default mealsReducer;
```

## 3. Configurando el Redux Store

`./App.js`

```js
import { createStore, combineReducers } from "redux";
import { Provider } from "react-redux";

import mealsReducer from "./store/reducers/meals.js";

const rootReducer = combineReducers({
  meals: mealsReducer,
});
const store = createStore(rootReducer);

<Provider store={store}>
  <MealsNavigator />
</Provider>;
```

## 4. Accediendo a nuestro estado desde otro componente

`./screens/FavoriteMealsScreen.js`

```js
import React from "react";
import { useSelector } from "react-redux";

const FavMeals = useSelector((state) => state.meals.favoriteMeals);
```

## 5. Creando y despachando acciones

`./store/actions/meals.js`

```js
export const TOGGLE_FAVORITE = "TOGGLE_FAVORITE";

export const toggleFavorite = (id) => {
  return { type: TOGGLE_FAVORITE, mealId: id };
};
```

## 5.1 Agregando al reducer un `switch` que contemple la acción

`./store/reducers/meals.js`

```js
import { MEALS } from "../../data/meals.js";
import { TOGGLE_FAVORITE } from "./store/actions/meals.js";

const initialState = {
  meals: MEALS,
  filteredMeals: MEALS,
  favoriteMeals: [],
};

const mealsReducer = (state = initialState, action) => {
  switch (action.type) {
    case TOGGLE_FAVORITE:
      const existingIndex = state.favoriteMeals.findIndex(
        (meal) => meal.id === action.mealId
      );
      if (existingIndex >= 0) {
        const updatedFavMeals = [...state.favoriteMeals];
        updatedFavMeals.splice(existingIndex, 1);
        return { ...state, favoriteMeals: updatedFavMeals };
      } else {
        const meal = state.meals.find((meal) => meal.id === action.mealId);
        return { ...state, favoriteMeals: state.favoriteMeals.concat(meal) };
      }

    default:
      return state;
  }
  return state;
};

export default mealsReducer;
```

## 5.2 Despachando la acción `TOGGLE_FAVORITE` en la pantalla de detalle de producto

`./screens/MealDetailScreen.js`

```js
import React from "react";
import { useSelector, useDispatch } from "react-redux";

import { toggleFavorite } from "../store/actions/meals.js";

const mealId = props.navigationOptions.getParam("mealId"); // Param passed through React Navigator.

const dispatch = useDispatch();

const toggleFavoriteHandler = () => {
  dispatch(toggleFavorite(mealId));
};
```

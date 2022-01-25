# Redux 이해하기

## Vanilla Javascript Redux Usage

```js
import { createStore } from "redux";

const addBtn = document.querySelector(".add");
const minusBtn = document.querySelector(".minus");
const count = document.querySelector(".count");

const ADD = "increase";
const MINUS = "decrease";

const reducer = (state = 0, action) => {
  console.log(state, action);

  switch (action.type) {
    case ADD:
      return state + 1;
    case MINUS:
      return state - 1;
    default:
      return state;
  }
};
const countStore = createStore(reducer);

countStore.subscribe(() => {
  console.log("countStore is changed!", countStore.getState());
  count.innerHTML = countStore.getState();
});

addBtn.addEventListener("click", () => {
  countStore.dispatch({ type: ADD });
});
minusBtn.addEventListener("click", () => {
  countStore.dispatch({ type: MINUS });
});
```

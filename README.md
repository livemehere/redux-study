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

## TODO app

### index.html

```html
<h1>Vanilla JS Redux TODO</h1>
<form class="d-flex align-items-center mx-auto justify-content-center">
  <input type="text" class="d-flex mb-0" placeholder="new todo" />
  <button class="py-1">ADD</button>
</form>
<ul></ul>
```

### index.js

```js
import { createStore } from "redux";

const form = document.querySelector("form");
const input = document.querySelector("input");
const ul = document.querySelector("ul");

const reducer = (state = [], action) => {
  // console.log(action);
  switch (action.type) {
    case "ADD":
      return [...state, createNewTodo(action.payload)];
    case "REMOVE":
      return removeTodo(state, action.payload);
    default:
      return state;
  }
};

const store = createStore(reducer);

store.subscribe(() => {
  updateUI(store.getState());
  console.log(store.getState());
});

form.addEventListener("submit", (e) => {
  e.preventDefault();
  store.dispatch({ type: "ADD", payload: input.value });
  input.value = "";
});

function createNewTodo(text) {
  return {
    id: Date.now(),
    text: text,
  };
}

function removeTodo(todos, targetId) {
  const newTodo = todos.filter((todo) => todo.id !== targetId);
  return newTodo;
}

function updateUI(todos) {
  ul.innerHTML = "";
  todos.forEach((todo) => {
    const li = document.createElement("li");
    const button = document.createElement("button");
    button.addEventListener("click", () => {
      store.dispatch({ type: "REMOVE", payload: todo.id });
    });
    button.setAttribute("class", "py-1");
    button.textContent = "삭제";
    li.setAttribute("class", "list-unstyled");
    li.textContent = todo.text;
    li.appendChild(button);
    ul.appendChild(li);
  });
}
```

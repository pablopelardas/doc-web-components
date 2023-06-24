
Vamos a construir una TodoApp como ejemplo de como se construye una web app con Web Components. Esta App podra crear, borrar y actualizar una todo list usando Lit y el local storage. Esto tambien reforzara buenas practicas para la arquitectura de componentes.

![Alt text](image-28.png)

Vamos a usar Lit para los Web Component templates. Tambien vamos a usar Webpack para compilar nuestra app con TypeScript.

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Todo App with LIt</title>
  <link rel="stylesheet" href="./css/index.css">
</head>
<body>
  <main>
    <x-app></x-app>
  </main>
  <script src="../dist/bundle.js"></script>
</body>
</html>
```

Como todas las aplicaciones de componentes vamos a tener un componente root. Nuestro componente va a estar empaquetado en un archivo js unico con Webpack y lo incluiremos al final del archivo con un script.

```ts
//app.ts
import { LitElement, html } from "lit";
import {customElement} from "lit/decorators.js";

@customElement("x-app")
export class Todo extends LitElement {
  constructor(){
    super()
  }
  render(){
    return html`
     <header>
      <h1>Todos</h1>
     </header>
     <main>
      <x-todos></x-todos>
     </main>
     <footer>
      2018
     </footer>
    `
  }
}
```
Nuestro componente root define la estructura basica de nuestra aplicacion. SI nuestra app tuviera un header mas elaborado para la navegacion, tendriamos que incluirlo aqui. 

En el main tenemos un componente `x-todos`, lo importamos al inicio del archivo. Este componente sera el responsable de manejar los todos de nuestra app.

Notemos que en este caso estamos registrando nuestro componente con el decorador `@customElement('x-app')` eliminando boilerplate.

### Todos Data Service

Antes de saltar al componente `x-todos` vamos a hacer un una clase servicio que se encargue de guardar y obtener nuestros todos del local storage. Vamos a crear un archio `todo.service.ts` que sera responsable de los cambios en nuestros todos y de persistirlos en el local storage.

```ts
//todos.service.ts
import {Todo} from './interfaces';

export class TodoService {
  getTodos(){
    const todos: Todo[] = JSON.parse(localStorage.getItem('todos'))
    return todos ? todos : []
  }
  updateTodo(todo: Todo, index: number){
    const todos =  this.getTodos();
    todos[index] = todo;
    return this.saveTodos(todos)
  }
  deleteTodo(todo: Todo){
    const todos = this.getTodos();
    const updateTodos = todos.filter(t => t.value !== todo.value);
    return this.saveTodos(updateTodos);
  }
  createTodo(value: string){
    const todos = this.getTodos();
    const updatedTodos = [...todos, {value, completed: false}]
    return this.saveTodos(updatedTodos)
  }
  private saveTodos(todos: Todo[]){
    localStorage.setItem('todos', JSON.stringify(todos))
    return todos;
  }
}
```
```ts
//interfaces.ts
export interface Todo {
  value: string;
  completed: boolean;
}
```

Nuestro servicio abstrae toda la logica de guardado y actualizacion en una sola clase. De esta manera, podemos mantener nuestros componentes ocupados en solo renderizar la data. La mejor practica en Web Apps es tener una **clara separacion de la logica de datos y la logica de render** entre componentes. Esta separacion permite que la logica y los componentes puedan testearse de manera unitaria mas facilmente.

### Todos List

El componente `x-todos` se va a comunicar directamente con nuestro servicio para guardar y actualizar la data.

```ts
import { LitElement, html } from "lit";
import {customElement, property} from 'lit/decorators.js'
import { TodoService } from "./todo.service";
import { Todo } from "./interfaces";
import './todo-item'

@customElement('x-todos')
export class Todos extends LitElement {
  @property({type: Array})
  todos: Todo[] = [];
  todoService = new TodoService();

  constructor(){
    super()
    this.todos = this.todoService.getTodos();
  }
}
```

Creamos el componente con una propiedad `todos` usando el decorador `@property`. Tambien creamos una instancia de nuestro `TodoService` para usar dentro de nuestro componente. En el constructor asiganmos los todos que esten en local storage a nuestras `todos`.

Ahora agregamos el template y los metodos de la clase

```ts
import { LitElement, html } from "lit";
import { customElement, property } from 'lit/decorators.js'
import { TodoService } from "./todo.service";
import { Todo } from "./interfaces";
import './todo-item'

@customElement('x-todos')
export class Todos extends LitElement {
  @property({ type: Array })
  todos: Todo[] = [];
  todoService = new TodoService();

  constructor() {
    super()
    this.todos = this.todoService.getTodos();
  }
  render() {
    return html`
      <form @submit=${(e: Event) => this.createTodo(e)}>
        <input type="text" placeholder="Add todo" aria-label="Add todo" />
        <button>Add</button>
      </form>
      <ul>
        ${this.todos.map((todo, index) => html`
        <li>
          <x-todo-item 
            .todo=${todo} 
            @update="${(e: CustomEvent) => this.updateTodo(e.detail, index)}" 
            @delete="${(e: CustomEvent) => this.deleteTodo(e.detail)}">
          </x-todo-item>
        </li>
      `)}
      </ul>
    `
  }
  createTodo(e: Event) { 
    e.preventDefault();
    const target = e.target as HTMLFormElement;
    const input = target.querySelector('input');
    if (input instanceof HTMLFormElement) {
      const value = input.value;
      if (!value) return
      this.todos = this.todoService.createTodo(
        value
      )
      input.value = '';
    }
   }
   updateTodo(todo: Todo, index: number){
      this.todos = this.todoService.updateTodo(
        todo,
        index
      )
   }
    deleteTodo (todo: Todo){
      this.todos = this.todoService.deleteTodo(
        todo
      )
    }
}
```

La primer parte de nuestro template define un HTML form para crear los todos. Usando el form podemos disparar el evento `submit` con un boton. En el submit llamamos a `createTodo()` que es un metodo interno de la clase.

La segunda parte del template renderiza una lista de todos. Cada todo con un `x-todo-item` que contiene la logica para cuando un usuario clickea en done, delete or update. Cuando estas acciones ocurren, el `x-todo-item` emite un `@update` o `@delete` para que nuestra lista pueda actualizar los todos con el servicio.

Nuestro `x-todo-item` es un componente que solo renderea. Toma un objeto todo por bindeo en la propiedad `.todo`. Si algo cambia, emite el evento necesario. Este aislamiento permite que el componente `x-todo-item` sea mas generico y reutilizable. 

### Todo Item

El componente `x-todo-item` es responsable de renderizar un solo todo y emitir los eventos de este item.

```ts
import {LitElement, html} from 'lit';
import {customElement, property} from 'lit/decorators.js';
import { Todo } from './interfaces';

@customElement('x-todo-item')
export class XTodoItem extends LitElement {
  @property({type: Object})
  todo: Todo

  @property({type: Boolean})
  editing = false;

  render(){
    return html`
      <style>
        .completed{
          text-decoration: line-through;
        }
        span{
          padding: 0 12px;
        }
        form {
          display: inline-block;
        }
      </style>
      <button @click="${() => this.completeTodo()}" aria-label="mark done">âœ“</button>
      ${this.editing 
        ? html`
          <form @submit="${(e: Event) => this.editTodo(e)}">
            <input type="text" .value="${this.todo.value}" aria-label="Edit todo" placeholder="Todo"/>
          </form>
        ` 
        : html`
          <span class="${this.todo.completed ? 'completed' : ''}" @dblclick="${() => this.toggleForm()}">${this.todo.value}</span>
        `}
        <button @click="${() => this.deleteTodo()}" aria-label="delete todo">x</button>
    `
  }

  completeTodo(){
    this.todo = {...this.todo, completed: !this.todo.completed}
    this.emitUpdate();
  }
  editTodo(event: Event){
    event.preventDefault();
    const target = event.target as HTMLFormElement;
    const input = target.querySelector('input');
    if (input instanceof HTMLInputElement) {
      const value = input.value;
      if (!value) return;
      this.todo.value = value;
      this.toggleForm();
      this.emitUpdate();
    }
  }
  toggleForm(){
    this.editing = !this.editing;
  }
  deleteTodo(){
    this.dispatchEvent(new CustomEvent('delete', {detail: this.todo}))
  }
  private emitUpdate(){
    this.dispatchEvent(new CustomEvent('update', {detail: this.todo}))
  }
}
```

Los metodos en el todo item actualizan nuestro objeto todo y emite la actualizacion.

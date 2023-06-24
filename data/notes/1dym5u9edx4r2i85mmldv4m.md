
El principio fundamental de los Web Components es la API de Custom Elements. Esta nos permite definir nuestras propias etiquetas de elementos HTML y adherirles un comportamiento. Esto podemos definirlo mediante javascript:

```javascript
class XComponent extends HTMLElement {
  constructor(){
    super();
  }

  connectedCallback(){
    this.innerHTML = 'Hello World from a Web Component!'
  }
}
customElements.define('x-component')
```

Debemos extender una clase de la clase base https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement. Llamamos al ```super()``` en el constructor donde también inicializaremos los estados y la lógica inicial del componente.

El método ```connectedCallback()``` es un método del ciclo de vida del componente, se invoca cuando el componente es creado y añadido al DOM. Es donde la lógica más pesada debería ocurrir, como el fetch de datos o la lógica de renderización.

Como extendemos de la clase ```HTMLElement``` obtenemos todos sus métodos y podemos acceder por el ```this```.

Por último registramos el componente en el DOM por medio de la sentencia ```customElements.define('x-component', XComponent)```
Pasamos como primer parámetro la etiqueta que vamos a utilizar y como segundo la referencia a nuestro componente.

Ya podríamos crear un html que importe este js y utilice el componente:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <x-component></x-component>
  <script src="x-component.js"></script>
</body>
</html>
```
![Alt text](image-2.png)
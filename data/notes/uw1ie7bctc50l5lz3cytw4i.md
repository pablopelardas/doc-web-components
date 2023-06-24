
```js
class XComponent extends HTMLElement {
  constructor() { 
    super(); 
    } 

  connectedCallback() { 
    this.innerHTML = 'Hello World from a Web Component!' 
  }
}
customElements.define('x-component', XComponent)
```

El constructor se ejecuta cada vez que una instancia del componente se crea. Sin embargo, si tenemos que instanciar al DOM o ejecutar la lógica inicial, vamos a querer correr este código en el ```connectedCallback()``` hook. Este se ejecuta cuando un componente se agrega al DOM, después de eso podemos empezar a renderizar e interactuar con el DOM.
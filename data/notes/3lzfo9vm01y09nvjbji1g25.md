El selector ```::slotted``` hace que estilar elementos en el slot sea relativamente facil.

```html
<x-component>
  <button>styled with ::slotted</button>
</x-component>
```

El componente puede estilar en su template el slot con el selector ```::slotted```

```js
const template  = document.createElement('template');
template.innerHTML = `
  <style>
    ::slotted(button){
      background-color: #2d2d2d;
      color: white;
      padding: 12px;
      border: 0;
      cursor: pointer;
    }
  </style>
  <slot></slot>
`
class XComponent extends HTMLElement {
  constructor(){
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-component', XComponent);
```

![Alt text](image-21.png)

El selector toma como parametro otro selector para seleccionar que elemento del slot te gustaría estilar. 
El ```::slotted``` nos puede ayudar a crear temas ya que el padre de los elementos puede sobreescribir los estilos para los hijos del slot. 
Hay limitaciones para este selector, solo puede seleccionar hijos directos del componente. Si nuestro boton tuviera un span dentro, el componente no podria llegar a él por medio del ::slotted .
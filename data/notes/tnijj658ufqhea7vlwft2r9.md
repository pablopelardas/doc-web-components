
Cuando utilizamos Web Components solemos usar los atributos HTML para pasar información al componente. 
```html
<x-component message='hello world'></x-component>
```
Estamos usando un atributo custom llamado ```message```. Podemos leer el valor de los atributos desde nuestro componente. Son convenientes para pasar información sin necesidad de usar javascript. Lo malo es que siempre son tratados como strings, no podemos pasar otro tipo de datos sin un parseo adicional al string.

```js
const template = document.createElement('template');
template.innerHTML = `
  <p></p>
`;

class XComponent extends HTMLElement {
  static get observedAttributes() {
    return ['message'];
  }
  set message(value){
    this._message = value;
    this.messageElement.innerText = this._message;
  };
  get message(){
    return this._message;
  }
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
    this.messageElement = this.shadowRoot.querySelector('p');
  }
  attributeChangedCallback(attrName, oldValue, newValue) {
    if (attrName === 'message') {
      this.message = newValue;
    }
  }
}

customElements.define('x-component', XComponent);
```

El metodo estatico ```observedAttributes``` espera una lista con los atributos que el DOM debe esperar que cambie. Esto es requerido para optimizar la performance de la app, y para poder recibir actualizaciones sobre que atributos cambiaron. La segunda parte de la custom Attribute API es  el metodo ```attributeChangeCallback()``` que se llama cada vez que uno de nuestros atributos listados se actualiza. El primer parametrro es el nombre del atributo que cambio, el segundo es el valor anterior y el tercero es el nuevo.
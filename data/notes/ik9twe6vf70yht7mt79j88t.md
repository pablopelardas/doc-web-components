
Lit es una libreria liviana que facilita la escritura y uso de Web Components. Fue creado y mantenido por el equipo Polymer en Google. No es la librería de Polymer Web Component sino que es una nueva individual. Vamos a convertir nuestro [[web-components.examples.dropdown]] en un Lit Element a medida que avanzamos en la sección.

Empecemos viendo las [[web-components.high-level-solutions.lit.template-strings]] de Lit.

Podemos agregar tambien [[Lit Event Listeners|web-components.high-level-solutions.lit.template-event-listeners]] a nuestras templates.

Con estos cambios nuestro dropdown todavía no va a comportarse como deseamos, tenemos que utilizar las [[web-components.high-level-solutions.lit.properties-and-decorators]]

En Lit los [[Custom Events|web-components.communication.events]] se utilizan de la misma manera que en los Web Components.

```js
import {LitElement, html} from 'lit';
class XDropdown extends LitElement{
  static get properties(){
    return{
      visible: {type: Number},
      title: {type: String}
    }
  }
  constructor(){
    super();
    this.visible = false;
    this.title = 'dropdown';
  }
  render(){
    return html`
      <style>
        .dropdown div{
          border: 1px solid #ccc;
          padding; 12px;
        }
      </style> 
      <div class=dropdown>
        <button @click="${() => this.toggle()}">${this.title}</button>
        ${this.visible ? 
          html`
            <div>
              <slot></slot>
            </div>
          ` : ''}
      </div>
    `
  }
  toggle(){
    this.visible = !this.visible
    this.dispatchEvent(new CustomEvent('visibleChange', {detail: this.visible}))
  }
}
```
Ahora que tenemos nuestro componente completo, con los eventos configurados, podemos interactuar con el como si se tratara de cualquier otro web component
```js
//index.js
const dropdown = document.querySelector('x-dropdown');
dropdown.title = 'custom dropdown';
dropdown.addEventListener('visibleChange', (e) => {console.log(e)})
```
#
Ahora veremos como [[Bindear a otros Web Components con Lit|web-components.high-level-solutions.lit.binding-components]]
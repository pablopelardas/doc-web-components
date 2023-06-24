```js
import {LitElement, html} from 'lit';
class XDropdown extends LitElement{
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
  }
}
```

En esta template tenemos un boton y un slot element que se va a mostrar y ocultar dinamicamente. Lit crea automaticamente la template usando la `HTML Template API` y el [[web-components.Apis.shadow-dom]]. Usar estas apis nos permitara lograr la [[web-components.styling.css-encapsulation]].

Lit tiene una API declarativa para escuchar eventos. En el boton agregamos `@click="${() => this.toggle}"`, usando el `@` podemos bindear eventos y reaccionar a ellos.
Para que Lit renderice nuestro componente correctamente cuando las propiedades cambian, debemos decirle que propiedades pueden cambiar explicitamente. Para esto tenemos 2 opciones, la primera es declarar nosotros la lista de propiedades que pueden ser seteadas en el componente

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
  }
}
```

El get estatico `static get properties()` le permite a LIt observar cuando las propiedades se setean y si deberia re-renderizar el componente. La otra API opcional es que podemos usar decoradores experimentales e inicializadores de propiedades en vez de usar una lista estatica de propiedades. Los decoradores y los inicializadores de propiedades son funcionalidades propuestas para Javascript, pero que todavia no estan disponibles. Si usamos Babel o Typescrit, podemos usar el siguiente codigo
```ts
import {LitElement, html} from 'lit';
import {property} from 'lit/decorators.js'
class XDropdown extends LitElement{
  @property()
  visible = false;

  @property()
  title = 'dropdown';

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

Usando decoradores tenemos una sintaxis mas declarativa, la contra es que necesitamos un paso extra en el buildeo del proyecto para transpilar este codigo.
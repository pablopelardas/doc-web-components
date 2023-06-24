Lit puede usarse para interactuar con otros Web Components existentes en nuestras aplicaciones. En el ejemplo anterior creamos de forma imperativa una referencia y un event listener para nuestro dropdown. 

Primero crearemos un componente raiz en el nivel mas alto.

```js
//x-app.js
import {LitElement, html} from 'lit';
import {property} from 'lit/decorators.js'
import 'dropdown'

class XApp extends LitElement{
  render(){
    return html`
      <x-dropdown>
        Hello from Lit!
      </x-dropdown>
    `
  }
}
customElements.define('x-app', XApp)
```

Con este componente podemos usar el binding declarativo de Lit para setear propiedades y escuchar a eventos

```js
//x-app.js
import {LitElement, html} from 'lit';
import {property} from 'lit/decorators.js'
import 'dropdown'

class XApp extends LitElement{
  render(){
    return html`
      <x-dropdown @visibleChange=${(e) => this.log(e)}>
        Hello from Lit!
      </x-dropdown>
    `
  }
  log(e){
    console.log(e)
  }
}
customElements.define('x-app', XApp)
```

Usamos el `@` para escuchar al evento. Esto funciona no solo con eventos nativos del DOM sino tambien con eventos custom como `visibleChange`. Podemos hacer lo mismo con las propiedades:
```js
//x-app.js
import {LitElement, html} from 'lit';
import {property} from 'lit/decorators.js'
import 'dropdown'

class XApp extends LitElement{
  constructor(){
    super();
    this.customTitle = 'Custom Title!'
  }
  render(){
    return html`
      <x-dropdown @visibleChange=${(e) => this.log(e)} .title=${this.customTitle}>
        Hello from Lit!
      </x-dropdown>
    `
  }
  log(e){
    console.log(e)
  }
}
customElements.define('x-app', XApp)
```

En lit usamos el `.` para bindear propiedades.
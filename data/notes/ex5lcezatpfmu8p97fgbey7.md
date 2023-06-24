
Lit incluye una librería de templating construida por encima de las templated literals de Javascript, que son las que construimos entre backsticks: `

Las template literals nos permiten crear templates en el navegador, Lit tambien incluye una clase base que facilita la creacion de Web Components.

En este ejemplo tomaremos el [[web-components.examples.dropdown]] y lo convertiremos usando Lit. 

```js
import {LitElement, html} from 'lit';
class XDropdown extends LitElement{
  render(){
    return html`
      Hello from Lit 
    `
  }
}
```

Importamos la clase `LitElement` del paquete de `lit` para crear nuestro componente. Lit espera que se implemente un metodo `render`. Este metodo retorna una template con nuestro componente. Nuestra template es creada usando la [[Tagged Template Literals | https://www.freecodecamp.org/news/a-quick-introduction-to-tagged-template-literals-2a07fd54bc1d/]] `html` del paquete de lit. `html` nos provee mas funcionalidades de templating que veremos mas adelante.


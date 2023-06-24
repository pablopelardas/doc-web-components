
CSS Parts API nos permite exponer un elemento del DOM específico en nuestro template para que pueda ser estilado publicamente. Por defecto, cualquier elemento de nuestro componente está escudado del estilo global. Podemos customizar [[web-components.styling.css-custom-properties]] pero estás pueden ser dificiles de mantener ya que sólo reprensetan una propiedad.

CSS Parts expone el elemento entero para que el consumidor pueda estilarlo como necesite.

```js
const template  = document.createElement('template');
template.innerHTML = `
  <style>
    button{
      background: green;
      color: #fff;
      border: 0;
      padding: 12px;
    }
  </style>
  <button part="button-one">one</button>
  <button part="button-two">two</button>
  <button >three</button>
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

Aca tenemos un componente básico con 3 botones y algunos estilos internos. En dos de los botones esta el atributo ```part``` con un nombre unico. Este atributo permite que el boton sea accedido por consumidores con css directamente. Usamos el selector ```:part``` y el nombre del part element para accederlo

```css
x-component::part(button-one){
  background: red;
}
x-component::part(button-two){
  background: blue;
}
```
![Alt text](image-20.png)

CSS Parts puede hacer que el codigo sea mans mantenible y flexible a los estilos. Esto igual tiene sus desventajas, si exponemos CSS Parts, nuestros estilos pueden romperse de forma imprevisible, si entregamos componentes con un sistema de diseño esto puede otorgarle demasiada flexibilidad al consumidor.
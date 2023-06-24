
Los eventos proporcionan un mecanismo por el cual el componente puede notificarnjos de que algo cambio. Por ejemplo al usar un botón podemos escuchar el evento ```click``` para notificar que el usuario clickeo el botón. 

```js
const template = document.createElement('template');
template.innerHTML = `
  <button>click me!</button>
`;

class XComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
    this.shadowRoot.querySelector('button').addEventListener('click', () => {
      console.log('clicked!')
    });
  }
}

customElements.define('x-component', XComponent);
```
![Alt text](image-8.png)

Ahora podemos escuchar cuando el usuario clickea pero esto es sólo una parte de la solución. Ahora debemos crear un evento custom para nuestro componente, para que cualquiera usandolo pueda hacer algo cuando nuestro boton es clickeado.

```js
//x-component.js
const template = document.createElement('template');
template.innerHTML = `
  <button>click me!</button>
`;

class XComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
    this.shadowRoot.querySelector('button').addEventListener('click', () => {
      this.handleClick();
    });
  }
  handleClick(){
    this.dispatchEvent(new CustomEvent('customClick', { detail: `Hola desde el evento click, ${Math.random()}` }));
  }
}

customElements.define('x-component', XComponent);
```

Agregamos un nuevo método ```handleClick``` que se llamará cuando alguien haga click sobre el botón y disparará un nuevo custom event ```customClick```.
Cuando despachamos un custom event debemos proveer 2 parametros, el primero es el nombre del evento ```customClick```. Ahora cualquiera que use el componente puede escuchar el evento customClick.
El segundo parametro es un objeto de configuración para el evento, en él ahora tenemos una sola propiedad ```detail``` donde podemos adherir cualquier valor al evento.
Veamos como se escucha este evento
```js
// index.js
import './x-component.js'

const component = document.querySelector('x-component');
component.addEventListener('customClick', e =>{
  console.log(e)
})
```
![Alt text](image-9.png)

Estamos escuchando nuestro custom event, y ahora podemos hacer con el lo que querramos como si se tratara de cualquier evento de javascript.
En el evento recibimos todos los detalles del DOM sobre nuestro componente y en el detalle el valor que pasamos.
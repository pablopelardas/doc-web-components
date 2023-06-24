
Esta api nos permite declarar piezas de de DOM encapsuladas y aisladas en nuestra aplicación. Junto a ellas pueden encapsular sus estilos (CSS) y usar plantillas declarativas de la [[Template API|web-components.Apis.template]]

Este encapsulamiento facilita la reutilización del componente y oculta su complejidad.

### Como instanciar un template component con shadow dom
```js
const template = document.createElement('template');
template.innerHTML = `<p>Hola desde template</p>`;

class XComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-component', XComponent);
```

En vez de adherir la plantilla directo al componente, creamos un ```shadowRoot``` para que utilice.
```this.attachShadow({mode: 'open'})``` le dice al navegador que este custom element debería tener un shadow root adherido. Creando este shadow root ahora tenemos una plantilla aislada donde podemos actualizar nuestro componente. La opción ```mode``` nos permite configurar si queremos que el shadow root sea accesible desde fuera del componente.
Desgraciadamente incluso poniendo el mode en 'closed' hay formas de mediante javascript manipular el shadow root ya que no existe un constructor privado en js aún por lo que no se debería tomar esta medida como seguridad.
Una vez creado el shadow root, podemos adherir y comenzar a actualizar nuestra plantilla.




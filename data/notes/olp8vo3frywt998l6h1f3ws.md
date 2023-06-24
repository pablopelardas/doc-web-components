Tomaremos lo que aprendimos de las APIS [[web-components.Apis.custom-elements]], [[web-components.Apis.template]], [[web-components.Apis.shadow-dom]] y [[web-components.Apis.content-slot]] para crear un simple componente dropdown.

Primero definimos nuestro custom element en un archivo js
```js
const template = document.createElement('template');
template.innerHTML = `
  <button>Dropdown</button>
  <div>
    <slot></slot>
  </div>
`;

class XDropdown extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-dropdown', XDropdown);
```

Nuestro componente tiene un boton para activar o desactivar nuestro contenido. También usamos la Content Slot API para pasar el contenido fácilmente al componente.
Debemos agregar algunas referencias del DOM al componente para acceder a nuestro boton y contenido.
```js
const template = document.createElement('template');
template.innerHTML = `
  <button>Dropdown</button>
  <div>
    <slot></slot>
  </div>
`;

class XDropdown extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }

  connectedCallback() {
    // El shadow root también nos da acceso al inner template del componente
    this.button = this.shadowRoot.querySelector('button');
    this.button.addEventListener('click', () => this.toggle());
    // Seteamos el contenido del dropdown a hidden
    this.content = this.shadowRoot.querySelector('div');
    this.content.style.display = 'none';
  }

  toggle() {
    const show = this.content.style.display === 'block';
    this.content.style.display = show ? 'none' : 'block';
  }
}

customElements.define('x-dropdown', XDropdown);
```

En el constructor podemos hacerle una query a nuestro template por medio del shadow root. Creamos referencias al boton y al contenido para poder manipularlos dinamicamente. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <x-dropdown>
    Hello World!
  </x-dropdown>
  <script type="module" src="x-dropdown.js"></script>
</body>
</html>
```

### Mejoramos nuestro componente
Gracias a las [[web-components.communication.js-properties]] y [[web-components.communication.events]] podemos mejorar nuestro componente y agregarle nuevas funcionalidades. Primero queremos que el texto del botón sea dinámico. Segundo, queremos crear un custom event listener para saber cuando un usuario ha abierto o cerrado el dropdown.

```js
// x-dropdown.js
const template = document.createElement('template');
template.innerHTML = `
  <button>Dropdown</button>
  <div>
    <slot></slot>
  </div>
`;

class XDropdown extends HTMLElement {
  get title(){
    return this._title
  }
  set title(value){
    this._title = value;
    this.button.innerText = this._title;
  }
  constructor() {
    super();
    this._title= 'Dropdown';
    this.show= false;
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }

  connectedCallback() {
    this.button = this.shadowRoot.querySelector('button');
    this.button.innerText = this.title;
    this.button.addEventListener('click', () => this.toggle());
    this.content = this.shadowRoot.querySelector('div');
    this.content.style.display = 'none';
  }

  toggle() {
    this.show = !this.show;
    this.content.style.display = this.show ? 'block' : 'none';
  }
}

customElements.define('x-dropdown', XDropdown);
```

```js
// index.js
import './x-dropdown.js'

const dropdown = document.querySelector('x-dropdown');

dropdown.title = 'Custom Title'
setTimeout(() => {
  dropdown.title = 'New Title'
}, 3000)
```

Establecemos el titulo como *Custom Title* y generamos un timeout para que en 3 segundos cambie a *New Title* dinámicamente.

Con esto logramos pasar la data a nuestro componente via prop y este actualizo su plantilla dinámicamente. Ahora vamos por el evento.

```js
// x-dropdown.js
//...inicio componente
  toggle() {
    this.show = !this.show;
    this.content.style.display = this.show ? 'block' : 'none';
    // despacha el evento show para que otros lo escuchen
    this.dispatchEvent(new CustomEvent('show', { detail: `El componente esta ${this.show ? 'Visible' : 'Oculto'}` }));
  }
//...fin componente

```
```js
//index.js
import './x-dropdown.js'

const dropdown = document.querySelector('x-dropdown');

dropdown.title = 'Custom Title'
setTimeout(() => {
  dropdown.title = 'New Title'
}, 3000)

dropdown.addEventListener('show', (e) => {
  console.log(e)
})

```
![Alt text](image-10.png)

Ya estamos escuchando nuestro evento show y podemos reaccionar a el.

### Mejoremos nuestro componente con custom attributes
Después de aprender sobre los [[web-components.communication.custom-attributes]] podemos seguir mejorando nuestro componente dropdown.


```js
const template = document.createElement('template');
template.innerHTML = `
  <button></button>
  <div>
    <slot></slot>
  </div>
`;

class XDropdown extends HTMLElement {
  static get observedAttributes() {
    return ['title'];
  }
  get title(){
    return this._title
  }
  set title(value){
    this._title = value;
    this.button.innerText = this._title;
  }
  constructor() {
    super();
    this._title= 'Dropdown';
    this.show= false;
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }

  attributeChangedCallback(attrName, oldValue, newValue) {
    if (attrName === 'title') {
      this.title = newValue;
    }
  }

  connectedCallback() {
    this.button = this.shadowRoot.querySelector('button');
    this.button.innerText = this.title;
    this.button.addEventListener('click', () => this.toggle());
    this.content = this.shadowRoot.querySelector('div');
    this.content.style.display = 'none';
  }

  toggle() {
    this.show = !this.show;
    this.content.style.display = this.show ? 'block' : 'none';
    // despacha el evento show para que otros lo escuchen
    this.dispatchEvent(new CustomEvent('show', { detail: `El componente esta ${this.show ? 'Visible' : 'Oculto'}` }));
  }
}

customElements.define('x-dropdown', XDropdown);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <x-dropdown title="Nuevo Titulo desde html">
    Hello World!
  </x-dropdown>
  <script type="module" src="index.js"></script>
</body>
</html>
```

![Alt text](image-13.png)
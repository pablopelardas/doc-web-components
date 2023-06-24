
La Content Slot API nos permite pasar contenido a la plantilla de un componente de forma declarativa. Para utilizarla, debemos estar usando la [[web-components.Apis.shadow-dom]] API con nuestras plantillas.

```html
<x-component>
  Hello World!
</x-component>
```
![Alt text](image-5.png)

Vemos que el componente tiene dentro el Hello World! pero no lo esta renderizando, debemos hacer unos cambios en nuestro javascript.

```js
const template = document.createElement('template');
template.innerHTML = `<p><slot></slot></p>`;

class XComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-component', XComponent);
```

Añadimos la etiqueta ```<slot></slot>``` a nuestra plantilla, lo que hará que lo que este dentro de nuestror componente se renderice.
![Alt text](image-4.png)

La API de Content Slot está sólo disponible si tu plantilla es inicializada con una instancia de shadow dom, como vemos en el constructor.

### Named Slots - Slots Nombrados
La API de Slots también soporta la inserción de más de un slot por componente. Esto es útil cuando precisamos crear componentes estructurados con contenido dinámico. Por ejemplo una tarjeta de un producto.

```js
const template = document.createElement('template');
template.innerHTML = `
  <style>
    .card{
      padding: 12px;
      border: 1px solid #ccc;
    }
    .title{
      border-bottom: 1px solid #ccc;
      margin-bottom: 12px;
    }
  </style>
  <div class="card">
    <div class=title>
      <slot name="title"></slot>
    </div>
    <slot></slot>
  </div>
`;

class XDetailCard extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-detail-card', XDetailCard);
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
  <x-detail-card>
    <span slot="title">Hello!</span>
    from multi slot
  </x-detail-card>
  <script type="module" src="x-detail-card.js"></script>
</body>
</html>
```
![Alt text](image-6.png)

Nuestro ```<span slot="title">``` tiene un slot que define en que slot del componente debería renderizarse.

La encapsulación de css nos permite escribir css especifico por componente que sólo va a aplicar a esa plantilla.

```js
const template  = document.createElement('template');
template.innerHTML = `
  <style>
    p{
      color: red;
    }
  </style>
  <p>Hola desde template, yo soy rojo</p>
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

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    p{
      color: blue;
    }
  </style>
</head>
<body>
  <p>Soy un parrafo azul</p>
  <x-component></x-component>
  <script type="module" src="index.js"></script>
</body>
</html>
```
![Alt text](image-15.png)

Vemos que a pesar de la regla ```p{color: blue}``` que aplica a todos los p, como en nuestro componente el html y el css esta encapsulado, no se ve afectado.

En las devtools podemos ver que el navegador creo un shadow root para el componente. Esto es lo que nos provee nuestro encapsulamiento. Esto funciona para ambos lados, los estilos del componente no impactan en el resto del dom y los estilos globales no impactan en el componente.
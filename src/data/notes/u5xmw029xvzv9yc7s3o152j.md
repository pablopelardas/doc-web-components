El hook ```disconnectedCallback()``` se llama cuando el componente es removido del DOM. Vamos a ver el siguient ejemplo
```js
// x-component.js
class XComponent extends HTMLElement {
  disconnectedCallback() {
    console.log('disconnectedCallback');
  }
}

customElements.define('x-component', XComponent);
```

```js
//index.js
import './x-component.js'

const toggle = document.querySelector('#toggle');
const main = document.querySelector('main');

toggle.addEventListener('change', (e) => {
  if (e.target.checked) {
    main.appendChild(document.createElement('x-component'));
  } else {
    main.removeChild(document.querySelector('x-component'));
  }
});
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
  <input type="checkbox" id="toggle">
  <main>
  </main>
  <script type="module" src="index.js"></script>
</body>
</html>
```

Deberías poder ver por consola cada vez que el componente se desmonta cuando se deschequea el toggle.

![Alt text](image-14.png)
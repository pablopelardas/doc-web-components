
El hook ```attributeChangedCallback()``` nos notifica cada vez que un atributo en nuestro html cambia.

Podemos verlo implementado en el ejemplo del [[web-components.examples.dropdown#mejoremos-nuestro-componente-con-custom-attributes]]

```js
class XComponent extends HTMLElement {
  static get observedAttributes() {
    return ['name'];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    if (name === 'name') {
      console.log('attrChange', name, newValue);
    }
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
</head>
<body>
  <x-component name="test"></x-component>
  <script type="module" src="x-component.js"></script>
</body>
</html>
```

Deberíamos ver por consola el mensaje con el valor del atributo name.

Con las props podemos pasar cualquier tipo de dato de javascript a nuestro componente, incluyendo datos no primitivos como los objetos y arreglos. 

```js
//x-component.js
const template = document.createElement('template');
template.innerHTML = `
  <p></p>
`;

class XComponent extends HTMLElement {
  set name(value){
    this._name = value;
    this.nameElement.innerText = this._name;
  }

  get name(){
    return this._name;
  }

  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
    this.nameElement = this.shadowRoot.querySelector('p');
  }
}

customElements.define('x-component', XComponent);
```

Gracias a ES2015 podemos utilizar getters y setters para definir propiedades en nuestro componente que, cuando se setean, pueden involucrar alguna logica. En este ejemplo tenemos un set y get ```name```. El getter devuelve el valor de ```_name```, nuestra propiedad privada para mantener el valor que se renderizará. El setter setea la variable privada ```_name``` y el texto de nuestro ```nameElement``` el cual creamos en el constructor. Cualquiera que utilice nuestro componente puede setear la variable name para ver los cambios.
```js
// index.js
import './x-component.js'

const component = document.querySelector('x-component');
component.name = 'Hello World from Prop'
``` 
```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <x-component></x-component>
  <script type="module" src="index.js"></script>
</body>
</html>
```
![Alt text](image-7.png)

Usar propiedades es la manera principal y más recomendada para pasar data a los componentes.
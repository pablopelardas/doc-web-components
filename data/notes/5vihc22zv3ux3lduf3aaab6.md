
CSS Custom Properties nos ayuda a solucionar distintos problemas. Nos permitiran definir variables dinamicas para que podamos crear un tema y estilar nuestros componentes individuales. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    :root{
      --primary-color: blue;
      --heading-size: 18px;
    }
    h2{
      color: var(--primary-color);
      font-size: var(--heading-size);
    }
    p{
      color: var(--primary-color);
    }
  </style>
</head>
<body>
  <h2>Titulo estilado con CSS custom property</h2>
  <p>Parrafo estilado con css custom property</p>
  <x-component></x-component>
  <script type="module" src="index.js"></script>
</body>
</html>
```

En la etiqueta style vemos las reglas de css utilizando las custom properties. El selector :root es el elemento más alto del DOM. En este ejemplo este es nuestro archivo global, con el :root podemos definir nuestras primeras CSS Custom Properties. Estas pueden tener cualquier nombre pero deben empezar con dos guiones. Para usarlas usamos ```var(--variable)```.


# Dynamic CSS Custom Properties
Uno de los beneficios principales de estas propiedades es que son dinamicas y podemos cambiar sus valores en tiempo de ejecución con Javascript.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    :root{
      --primary-color: blue;
      --heading-size: 18px;
    }
    h2{
      color: var(--primary-color);
      font-size: var(--heading-size);
    }
    p{
      color: var(--primary-color);
    }
  </style>
</head>
<body>
  <h2>Titulo estilado con CSS custom property</h2>
  <p>Parrafo estilado con css custom property</p>
  <button>Toggle Tema</button>
  <script type="module" src="index.js"></script>
</body>
</html>
```

```js
document.querySelector('button').addEventListener('click', () => {toggleStyles()})

function toggleStyles(){
  const styles= getComputedStyle(document.documentElement);
  const colorValue = styles.getPropertyValue('--primary-color');
  if (colorValue === 'green'){
    document.documentElement.style.setProperty('--primary-color', 'blue');
  } else {
    document.documentElement.style.setProperty('--primary-color', 'green');
  }
}
```

Podemos acceder a las variables dinamicante y cambiar de azul a verde simulando un tema de la aplicación

![Alt text](image-16.png)
![Alt text](image-17.png)

## Usando :host
Podemos apuntar nuestras CSS Custom Properties a componentes especificos utilizando el selector ```:host```
En el siguiente ejemplo tendremos un parrafo estilado globalmente y otro dentro de un componente con estilos por default. Los estilos globales hacen al parrafo azul mientras que el componente lo hace rojo.

![](image-18.png)

Lo que buscamos es customizar el componente y cambiar el parrafo a verde en vez de rojo.

```js
//x-component.js
const template  = document.createElement('template');
template.innerHTML = `
  <style>
    :host{
      --color: red;
      --font-size: 16px;
    }
    p{
      color: var(--color);
      font-size: var(--font-size);
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
En el css usamos el selector ```:host``` que refiere al host element. Para nuestro componente ese sería la etiqueta ```x-component```. Con el selector ```:host``` podemos usar custom css properties en el componente. Creamos la varialbe ```--color``` y ```--size```.
Cuando utilizamos propiedades custom, podemos setearlas desde fuera del componente facilmente refiriendonos al host. Haciendo que hacer temas y customizar componentes sea más fácil.

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
    x-component{
      --color: green;
      --size: 24px;
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

![Alt text](image-19.png)
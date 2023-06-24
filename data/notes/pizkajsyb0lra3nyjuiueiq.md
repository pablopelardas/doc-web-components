Para este ejemplo vamos a dar por sentado algunas cosas sobre cómo nuestro componente debería ser usado.

- Soporta sólo navegadores que soporten el core de Web Components API.
- Los consumidores de nuestro componente usan NPM

En nuestor ejemplo tendremos una estructura de carpetas así
![](image-26.png)

En nuestro ```dropdown.js``` tenemos el componente que construimos en [[web-components.examples.dropdown]]. En ```index.js``` exportamos el componente y cualquier otro componente que haya en nuestra librería.

Para publicar a NPM primero necesitamos crear nuestro ```package.json``` el cual define la metadata de nuestro paquete así como las otras dependencias que podríamos estar utilizando de otros paquetes NPM. 

Para crear nuestro ```package.json``` debemos tener instalado NodeJS. Una vez realizado, la CLI de NPM nos va a permitir crearlo con

```bash
npm init
```

Esto nos abrirá un prompt para completar la configuración básica de nuestro proyecto

```json
{
  "name": "web-componet",
  "version": "0.0.1",
  "description": " Esto es un web component ",
  "main": "src/index.js",
  "module": "src/index.js",
  "directories":{
    "src": "src"
  },
  "author": "El autor",
  "license": "La licencia que corresponda"
}
```

Utilizamos las propiedades ```main``` y ```module``` para ayudar a los bundlers como Webpack a entender como consumir nuestro paquete y donde se encuentra el entry point de la librería. 

Ahora que ya tenemos la configuración mínima, podemos publicarlo con

```bash
npm publish
```

Asumiendo que tenemos un acuenta de NPM y estamos logeados, nuestro componente debería publicarse correctamente en el registro de NPM. 

Para utilizarlo en otro proyecto tenemos un par de opciones. Normalmente tendríamos un bundler como Webpack para realizar el build, pero para esta demo, lo mantendremos simple con una etiqueta script en un HTML. 

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
    Hola mundo desde componente publicado
  </x-dropdown>
  <script type="module" src="https://unpkg.com/web-component-essentials@0.0.1/src/dropdown.js"></script>
</body>
</html>
```
Utilizamos https://unpkg.com/ CDN para publicar nuestro componente sin pasos de buildeo facilmente. 
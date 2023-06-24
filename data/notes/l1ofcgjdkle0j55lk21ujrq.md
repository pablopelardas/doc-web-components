Los Web Components no son soportados en todos los navegadores, en IE11 se necesita compilar a ES5 y utilizar polyfills.

Hay 2 desafios al utilizar Web Components en navegadores que todavia no los soportan. El primero es la falta de soporte de JavaScript ES2015. Los navegadores más antiugos como IE no soportan las ultimas características de JavaScript como las clases de las cuales los custom elements dependen.
Para lograr este soporte debemos agregar un paso a nuestro buildeo para transpilar nuestro codigo. Esto es similar a compilar, tomamos el codigo ES2015 y lo convertimos en codigo de ES5 que todos los navegadores soportan.

Las 2 formas más comunes de transpilar el codigo de ES2015 a ES5 es utilizando Bable y Typescript. Typescript tiene un soporte de transpilación similar a Bable y agrega tipado statico a nuestro js, haciendolo más mantenible y testeable. En este ejemplo utilizaremos vanilla javascript y babel.

## Polyfills

Mientras que casi todos los navegadores soportan ES2015, no todos soportan las APIS de Web Components. Afortunadamente podemos usar polifyll para crear Web Components y mantener un soporte entre los navegadores.

Vamos a usar https://github.com/webcomponents/webcomponentsjs. Estos Polyfills incluyen las funcionalidades esenciales de la Web API de Web Components. 
Si abrimos nuestro dropdown en un navegador que no soporta como IE, vamos a tener errores y el componente no va a tener la funcionalidad que corresponde. Vamos a modificar nuestro html para que los navegadores incompatibles lo soporten

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
  
  <script type="module" src="https://unpkg.com/@webcomponents/webcomponentsjs@2.8.0/webcomponents-bundle.js"></script>
  <script type="module" src="https://unpkg.com/web-component-essentials@0.0.1/src/dropdown.js"></script>
</body>
</html>
```

Ahora con el ```webcomponents-bundle.js``` las features que faltaban se agregan y el componente funcionara. Esto hara que funcione en todos los navegadores modernos pero algunos todavía nos preocupamos por navegadores más antiguos como IE. Para lograr esta compatibilidad necesitaremos un paso extra. Vamos a tener que agregar a nuestra librería conteniendo el dropdown algunas herramientas para compilar el código ES2015 a ES5. Las 2 principales herramientas que usaremos en esta sección son Webpack y Babel. 
Webpack permite enpaquetar dependencias en un bundle para aplicaciones. Babel es un compilador que va a transpilar nuestro codigo a ES5.

# Instalando Webpack y Babel

El primer paso es instalar webpack y babel en nuestra librería

```bash
npm install rimraf webpack webpack-cli babel-core babel-loader babel-preset-env --save-dev
```

Esto añadirá los paquetes a nuestro ```package.json```. Este debe contar aparte con nuestras propiedades ```"main": "dist/index.js``` y ```"module": "src/index.js```
El main indicara donde se encuentra el codigo compilado en ES5. El ```module``` donde se encuentra el codigo ES2015. El JS situado en ```/dist``` sera el compilado de nuestra fuente ```/src```.

También ebemos agregar scripts a nuestro json
```json
"scripts":{ 
  "clean": "rimraf dist",
  "build": "npm run clean && webpack --mode production"
}
```

El script clean limpia la carpeta dist, el build creara el build de nuestro codigo en la carpeta dist.

![[web-components.extra-tools.webpack]]



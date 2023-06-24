Webpack es un empaquetador de modulos y dpeendencias para aplicaciones web. Facilita el tomar todas las dependencias de nuestro proyecto y enpaquetarlas en un solo archivo facil de distribuir en un ambiente productivo.
Webpack puede empaquetar distintos tipos de assets por medio de plugins. Pro ejemplo si queremos usar un preprocesador de CSS como Less o Sass, podríamos usar un plugin de webpack para observar estos archivos y compilarlos. En este caso usaremos Babel para compilar y empaquetar nuestro codigo ES2015 a ES5.

Primero creamos el webpack.config.js

```js
var path = require('path');
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'index.js',
    library: 'webComponent',
    libraryTarget: 'umd'
  }
};
```

Esta es la configuración mínima de Webpack. El ```entry``` es el main de nuestra librería. Desde ese archivo Webpack empezará y resolverá cualquier dependencia (imporst). El ```output``` define donde se va a generar el codigo compilado. También define su nombre y que tipo de archivo JavaScript va a ser.

Ahora podemos correr el comando

```bash
npm run build
```

Si vamos a la carpeta dist vamos a ver un ```index.js``` compilado a ES5 que será compatible con IE11.
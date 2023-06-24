Vue es un framework de javascript más nuevo que Angular o React que recientemente ha ganado popularidad por su simpleza en su API y la facil curva de aprendizaje.

En esta sección crearemos un proyecto con VueJS Cli y agregaremos nuestro [[Dropdown|web-components.deployment.npm]] al proyecto.

Usaremos la herramienta de Vue CLI para generar el proyecto. Nos proveera todas las herramientas necesarias para generar el buildeo y correr nuestra aplicación. 

```bash
npm install -g @vue/cli
vue create my-app
```

Este comando creara nuestro proyecto e instalara las dependencias. Procedemos a instalar nuestro componente

```bash
npm install web-component-essentials --save
```

Ahora podemos importarlo en nuestro ```main.js``` de la aplicación de Vue.

```js
import Vue from 'vue'
import App from './App.vue'
import 'web-component-essentials'

Vue.config.productionTip = false;
new Vue({
  render: h => h(App)
}).$mount('#app')
```

Para correr la aplicación corremos
```bash
npm run serve
```
Esto iniciará nuestra aplicación en ```localhost:8080```. Vamos a ver nuestro componente ```HelloWorld.vue```. Vue organiza todo en un sólo archivo (los estilos, el template y el script).
```html
//HelloWorld.vue
<template>
  <div>
    <h1>VueJS Application using Web Components</h1>
    <p>{{show ? 'open': 'closed'}}</p>
    <x-dropdown :title="myTitle" @show="log">
      Hello from Web Component in Vue
    </x-dropdown>
  </div>
</template>
<script>
  export default {
    name: 'HelloWorld',
    data: function (){
      return {
        myTitle: 'project-vue',
        show: false
      }
    },
    methods:{
      log: function (event){
        console.log(event);
        this.show = event.detail;
      }
    }
  }
</script>
```

En nuestra ```template``` tenemos una sintaxis similar a [[web-components.frameworks-integration.angular]], podemos ver la expresión ```{{show ? 'open' : 'closed'}}```. En el Dropdown estamos usando la sintaxis de bindeo de Vue, esta sintaxis funciona bien con todos los HTML elements asi como tambien custom elements usando Web Components.

Para bindear una propiedad en vue usamos el ```:``` en este caso ```:title="myTitle"```. Nuestro componente Vue tiene una propiedad ```myTitle``` que se le asigna al ```title``` del dropdown.
Para escuchar eventos, usamos el ```@```, en este caso escuchamos nuestro evento show con ```@show="log"```.

En el ```script``` definimos la data y los metodos de nuestro componente que vamos a querer bindear en el template.

Vue es una muy buena opción liviana para crear aplicaciones JS que funcionen bien con Web Components.

La API de Template provee muchas características de bajo nivel que se necesitan para construir componentes de UI. Esta API no soporta muchas de las características a las que estamos acostumbrados en los frameworks, como los bucles o condicionales.

Muchas de las APIs de los Web Componentes son de bajo nivel a propósito para permitir flexibilidad y que cada uno pueda construir lo que necesite sobre ellas.

Esta API nos permite crear plantillas neutras en nuestros componentes. El navegador **ignora todo el código que se escriba dentro de esta etiqueta**.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <template>
    <h1>No voy a renderizarme</h1>
    <script>
      alert('no voy a alertar');
    </script>
  </template>
  <script type="module" src="index.js"></script>
</body>
</html>
```
Podemos ver que el codigo no se renderiza aunque si está en el html dentro de un ```#document-fragment```
![Alt text](image-3.png)

A continuación podemos clonar la plantilla con js, manipularla para luego finalmente renderizarla.

```js
const template = document.createElement('template');
template.innerHTML = `<p>Hola desde template</p>`;

class XComponent extends HTMLElement {
  constructor() {
    super();
    this.appendChild(template.content.cloneNode(true));
  }
}

customElements.define('x-component', XComponent);
```

De esta manera podemos mantener una única referencia a nuestra plantilla. Cada vez que usemos nuestro ```x-component``` reducimos el costo de tener que crear la plantilla nuevamente. Esto nos da una mejor performance a la hora de instanciar nuestros componentes y clonar una copia de la template en cada instancia. 

Antes de seguir con las ventajas de la API de templates, debemos conocer la [[Shadow DOM API|web-components.Apis.shadow-dom]]

![[web-components.Apis.shadow-dom]]

Ahora que tenemos la plantilla aislada, podemos tomar ventaja de otras APIS que la Web nos provee.
- [[web-components.Apis.content-slot]]
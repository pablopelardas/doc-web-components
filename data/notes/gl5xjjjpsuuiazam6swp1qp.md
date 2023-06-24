Stencil es un compilador de Web Components. Al contrario de [[web-components.high-level-solutions.lit]], Stencil añade un proceso de buildeo para crear los Web Components. Es menos flexible que Lit pero provee una API de mas alto nivel para crear componentes. Aprovecha tecnologías como TypeScript y JSX. Fue creado y mantenido por Ionic, la empresa especializada en native mobile apps y progressive web apps con tecnologias web.

El kit de UI de Ionic fue construido inicialmente con Angular components, pero recientemente Ionic creo Stencil para desarrollar sus componentes como Web Components. Esto permitio que cualquiera use los componentes de Ionic y que no esten restringidos a aplicaciones Angular.

Vamos a crear nuestro [[web-components.examples.dropdown]] con Stencil. Vamos a cubrir los beneficios que provee Stencil como una solucion end to end.

## Stencil CLI
Es la herramienta de linea de comandos que facilita el creado y buildeo de librerias y progressive apps con Stencil. Nos permite escribir nuestros componentes con JSX y TypeScript. Tambien provee un mecanismo para escribir CSS y Sass en archivos individuales. Por ultimo, tambien provee test unitario incorporado para escribir test automatizados para los componentes. 

Primero necesitamos instalar la CLI
```bash
npm init Stencil
```

Este comando va a iniciar la Stencil CLI y nos va a generar una prompt para elegir el nombre del proyecto. Nosotros vamos a seleccionar la component library option, es decir una libreria.
Una vez completado, vamosa  tener el proyecto iniciado para poder empezar a crear nuestros componentes.

Vamos a tener un directorio `src/components` donde se va a encontrar un componente único `my-component.tsx`. Stencil se escribe en TypeScript y JSX, si usaste React o Angular esto se va a sentir muy familiar. Se ve como una mezcla entre ambos frameworks.

Creamos el archivo `x-dropdown.tsx` para nuestro componente en el directorio de componentes.

```tsx
 import { Component, Event, EventEmitter, Prop, State, h } from "@stencil/core";

 @Component({
    tag: "x-dropdown",
    styleUrl: "x-dropdown.css",
    shadow: true
 })
 export class XDropdown {
  @Prop() myTitle = 'dropdown';
  @State() show = false;
  @Event() showChange: EventEmitter;

  render(){
    return(
      <div>
        <button onClick={() => this.toggle ()}>{this.myTitle}</button>
        {this.show
          ? <div class="x-dropdown__content"><slot /></div>
          : <div></div>
        }
      </div>
    )
  }
  toggle(){
    this.show = !this.show;
    this.showChange.emit(this.show);
  }
 }
```
![[web-components.high-level-solutions.stencil.decorators]]

Despues de los decoradores encontramos un metodo `render()` que espera que le pasemos una plantilla JSX
![[web-components.high-level-solutions.stencil.jsx-templates]]

Y por ultimo tenemos el metodo `toggle()` que va a invertir nuestra propiedad show y dispara el evento `showChange`

Ahora vamos a ver como podemos utilizar nuestro componente dentro de otro componente de Stencil.

![[web-components.high-level-solutions.stencil.component-bindings]]

Por ultimo vamos a buildear nuestra libreria

![[web-components.high-level-solutions.stencil.building]]

Stencil es una excelente opcion para crear Web Components ya que nos da la conveniencia de un framework y todas las ventajas de los Web Components.
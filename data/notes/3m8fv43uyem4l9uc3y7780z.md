
Angular fue diseñado desde el principio para funcionar con Web Components. Puede consumir Web Components y publicar los componentes de Angular como Web Components por medio de la API Angular Elements. Vamos a instalar nuestro [[Dropdown|web-components.deployment.npm]] en un proyecto de Angular CLI.

Primero creamos el proyecto angular

```bash
npm install -g @angular/cli
ng new my-app
```
Este comando instalar angular, creara un proyecto e instalara todos los paquetes NPM necesarios. Una vez completado utilizaremos
```bash
ng serve
```
Que iniciará nuestra aplicación en ```localhost:4200```. Ahora debemos instalar nuestro componente, en [[Deploy con npm|web-components.deployment.npm]] utilizamos un CDN, aqui lo instalaremos directamente con npm:

```bash
npm install web-component-essentials
```

Este componente instalara el dropdown en nuestro proyecto de Angular. En nuestro ```app.module.ts``` podemos importarlo

```ts
//app.module.ts
import {BrowserModule} from '@angular/platform-browser';
import {NgModule, CUSTOM_ELEMENTS_SCHEMA} from '@angular/core';
import 'web-component-essentials';
import {AppComponent} from './app.component';

@NgModule({
  declarations:[
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent],
  schemas:[
    CUSTOM_ELEMENTS_SCHEMA // Le avisa a Angular que tenemos custom tags
  ]
})

export class AppModule {

}
```
Una vez instalado podemos utilizarlo, vamos a mirar nuestro ```app.component.ts```

```ts
//app.component.ts
import {Component} from '@angular/core'

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent{
  myTitle = 'project-angular';
  open = false;

  toggle(event){
    console.log(event);
    this.open = event.detail;
  }
}
```

Este componente es el root de nuesta aplicación Angular. En nuestro componente, tenemos 2 propiedades: el ```myTitle``` lo pasaremos al dropdown y la propiedad ```open``` observara si el dropdwon esta abierto o cerrado.
También tenemos un metodo ```toggle()``` que se llamará cada vez que el dropdown se abre o cierra.

```html
<!-- app.component.html -->
<h1>Angular Application using Web Components</h1>
<p>
  {{open ? 'open':'closed'}}
</p>
<x-dropdown [title]='myTitle' (show)="toggle($event)">
    Hello from Web Component in Angular!
</x-dropdown>
```

Tenemos un p que muestra el mensaje ```open``` o ```closed``` basado en el valor de la propiedad open. Esta sintaxis no funciona solo para Angular sino también para los Web Components

El primer bindeo es la propiedad ```[title]="myTitle"``` para avisarle a Angular que esa propiedad debería ser seteada. La segunda sintaxis es la que utiliza Angular para escuchar eventos ```(show)="toggle($event)"```. En el parentesis pasamos el nombre del evento que queremos escuchar, y a la derecha el metodo.

Angular es una excelente opción para aplicaciones del lado del cliente por su robusta API que funciona bien con aplicaciones de grandes empresas mientras mantiene el soporte de Web Components.
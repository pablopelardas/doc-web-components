# Decorators
Stencil usa una combinacion de decoradores y JSX para mantener una forma declarativa y sencilla de crear Web Components. 

```tsx
@Component({
  tag: 'x-dropdown',
  styleUrl: 'x-dropdown.css',
  shadow: true
})
```

Si usaste Angular, este decorador te parecerá familiar. Los decoradores se compilan en TypeScript y luego en Javascript. Estos permiten a los desarrolladores agregar metadata adicional a clases y propiedades. Stencil los utiliza para ayudarte a crear y compilar tus componentes en un Web Component.

El decorador `@Component` describe nuestro componente. El `tag` indica cual va a ser el tag de nuestro elemento. El `styleUrl` nos permite definir un archivo separado para los estilos del componente. El `shadow` nos permite decirle a Stencil si queremos que active o desactive el [[web-components.Apis.shadow-dom]] para nuestro componente.

Tambien tenemos decoradores en nuestras propiedades: 

```tsx
@Prop() title = 'dropdown';
@State() show = false;
@Event() showChange: EventEmitter;
```

Cada decorador agrega un comportamiento a nuestro componente. El primero decorador `@Prop` nos permite definir propiedades publicas para que otros pasen data. En este caso tenemos el `title` para setear el texto del boton.

El decorador `@State` nos permite indicarle a Stencil que propiedades, cuando sean actualizadas, deberian disparar un re-render. En nuestro caso, siempre que el boton se clickee, actualizamos al propiedad `show`, y por el `@State` el template se vuelve a renderizar.

El decorador `@Event` nos permite crear [[Custom Events|web-components.communication.events]] para nuestro componente. Aplicando el `@Event` podemos llamar a `this.showChange.emit(this.show)` para disparar nuestro evento `showChange`. El nombre de la propiedad es el nombre del evento emitido.
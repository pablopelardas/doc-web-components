
Con las [[web-components.styling.css-custom-properties]] podemos construir temas flexibles y fáciles de mantener.

- [[web-components.theming.root-host-scope]]

## Implementando un tema oscuro
Cuando implementamos temas en web components, tenemos un par de estrategias disponibles para utilizar. Podríamos utilizar una hoja de estilso separada que contiene una lista actualizada de las CSS Custom Properties para ese tema particular.

```css
:root{
  --modal-background-color: black;
  --modal-color: white;
  --dropdown-background-color: black;
  --dropdown-color: white;
}
...
```
Podríamos aplicar esta hoja cuando querramos cambiar el tema. Otra estrategia es aplicar el stilo utilizando una clase particular, cuando la clase se aplica el tema se aplica:
```css
:root .dark-theme{
  --modal-background-color: black;
  --modal-color: white;
  --dropdown-background-color: black;
  --dropdown-color: white;
}
...
```
Cualquiera de las 2 estrategias funcionaría. Podemos llevar esto un paso más adelante construyendo temas incorporados a nuestros componentes. Para hacer esto usaremos el selector ```:host``` para aplicar distintas propiedades basadas en algún atributo de nuestro elemento host.

```css
/**
  Si nuestro elemento host tiene el atributo dark, hacer esto
*/
:host([dark]) p{
  color: #fff;
  background: #2d2d2d;
}
```

De esta manera podemos usar el ```:host``` para reemplazar las CSS Custom Properties y aplicar unos estilos u otros basados en los atributos de la etiqueta.

Esto nos da una penalidad menor en la performance ya que todos los estilos del componente se van a descargar aunque los estilos de ese tema no se esten usando actualmente. Esto no sucedería si sólo se descarga la hoja de estilos que corresponda al tema actual.




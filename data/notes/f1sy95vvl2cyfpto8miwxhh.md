
Utilizando [[web-components.styling.css-custom-properties]] y [[web-components.Apis.shadow-dom]] podemos crear temas y estilar nuestros componentes.

# Estilos globales
 Uno de los puntos malos del CSS es que por default, el CSS es global. 
 ```css
 h1 {
  color: red
 }

 ```
 Esta regla hace que **todos** los ```h1``` sean rojos. Muchas veces solo queremos aplicar una regla a un componente o vista especifico. Para hacer esto en el pasado se utilizaban convenciones de nombres como [[BEM|https://getbem.com/introduction/]], para poder evitar que accidentalmente se pisaran estilos. Por ejemplo
 ```css
 h1.article-heading{
  color: blue
 }

 h1.article-heading .article-heading--green{
  color: green
 }

 ```
 De esta forma se evita que el estilo impacte en todos los h1 y solo en el que tiene la clase. Esto es mejor pero no es escalable ya que es posible que en proyectos grandes nos encontremos con colisiones en los nombres.

 Con Web Componenst podemos usar el Shadow DOM para simplificar como escribimos CSS. 
[[web-components.styling.css-encapsulation]]

Pero ¿Qué ocurre si queremos pisar los estilos del componente? Tal vez para usar un tema custom. Para esto nos van a servir las
[[web-components.styling.css-custom-properties]]

Mientras que las CSS Custom Properties permiten una mejor customización del componente y los temas, todavía puede haber veces en las que necesitemos más flexibilidad y debamos activar el estilo de elementos del DOM enteros en nuestro componente
[[web-components.styling.css-parts-api]]

Por ultimo nos queda ver como estilar componentes dentro de nuestros slots [[web-components.styling.slotted-elements]]
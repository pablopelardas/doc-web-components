
Cuando utilizamos componentes queremos mantenerlos genéricos y reutilizables. Para lograr esto debemos separar la lógica de fetching de datos del componente y recibir estos últimos por medio de custom properties. El flujo de datos de nuestra aplicación sería algo así:
![Alt text](image-25.png)
Hacemos el fetch desde el main de nuestra aplicación ```index.js```, después pasamos esa data a nuestros componentes por las [[Custom Properties|web-components.communication.js-properties]]. Esta propiedada va a tener el arreglo de personajes, y esto mantiene nuestro componente de lista generico y reutilizable ya que no le importa la data que va a recibir.

Nuestro componente de lista también necesita comunicarse con el main para avisarle que se ha clickeado sobre un item, para eso utilizaremos un [[Custom Event|web-components.communication.events]] para indicarle al root cual fue el item seleccionado y que el pueda pasarle la data al componente de detalle.

Como el de lista, a nuestro componente de detalle no le importa de donde viene la data, su unica responsabilidad es renderizarla. La idea de este patron utilizado por los principales frameworks ( Angular, Vue y React), es que la mayoría de nuestros componentes sean puros, es decir, **que su única responsabilidad sea renderizar algo**. De esta manera separamos la lógica de la renderización. Esto hace que los componentes puros sean reutilizables y más fáciles de testear con unit testing. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h2>Personajes de la Galaxia</h2>
  <x-character-list></x-character-list>
  <x-character-detail></x-character-detail>
  <script type="module" src="index.js"></script>
</body>
</html>
```

```js
//index.js
import './x-character-detail.js';
import './x-character-list.js';

const characterListComponent = document.querySelector('x-character-list');
const characterDetailComponent = document.querySelector('x-character-detail');

characterListComponent.addEventListener('characterSelected', (event) => {
  characterDetailComponent.character = event.detail;
})

fetch('https://swapi.dev/api/people/')
  .then(response => response.json())
  .then(data => {
    characterListComponent.characters = data.results;
    characterDetailComponent.character = data.results[0];
  })
``` 

En nuestro ```index.js``` tomamos las referencias DOM de nuestros componentes. Agregamos el ```eventListener``` para nuestro evento de la lista de componentes y cuando este se dispara, pasamos el ```detail``` del evento (donde vendra nuestro item) a nuestro componente de detalle.

También hacemos la petición a la API para pasarle la lista de persojanes a nuestro componente lista por medio de la propiedad characters. También le damos un personaje inicial a nuestro componente de detalle.

```js
// x-character-list.js
const template  = document.createElement('template');
template.innerHTML = `
  <style>
    button{
      display: block;
      padding: 12px;
      width: 100%;
    }
  </style>
  <section></section>
`
class XCharacterList extends HTMLElement {
  get characters(){
    return this._characters;
  }
  set characters(value){
    this._characters = value;
    this._render();
  }
  constructor(){
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));
    this.characterListElement = this.shadowRoot.querySelector('section');
  }

  _render(){
    this.characterListElement.innerHTML = '';
    this.characters.forEach(character => {
      const button = document.createElement('button');
      button.innerText = character.name;
      button.addEventListener('click', () => {
        this.dispatchEvent(new CustomEvent('characterSelected', { detail: character }));
      });
      this.characterListElement.appendChild(button);
    });
  }
}

customElements.define('x-character-list', XCharacterList);
```
En nuestra template definimos estilos para los botones y una seccion donde agregaremos los botones de nuestros personajes.
En la definición de la clase agregamos la custom property ```characters```. Tenemos sus respectivos setters y getters. El constructor hace el mismo boilerplate que venimos haciendo en nuestros otros ejemplos. Creamos una referencia a la sección que utilizaremos en el método ```_render()``` para agregar los personajes. Este método es llamado cada vez que se actualizan los personajes con el ```set```. 
Dentro del render creamos el custom event para cada botón y despachamos ```characterSelected```. En otra sección hablamos de mejores opciones para manipular las templates ya que vemos que puede volverse algo muy difícil de mantener, con ayuda podremos crear templates más dinamicas y eventos de manera más sencilla.

```js
//x-character-detail.js
class XCharacterDetail extends HTMLElement {
  get character(){
    return this._character;
  }
  set character(value){
    this._character = value;
    this._render();
  }
  constructor(){
    super();
    this.attachShadow({ mode: 'open' });
  }
  _render(){
    this.shadowRoot.innerHTML = `
      <h2>${this.character.name}</h2>
      <ul>
        <li>Height: ${this.character.height}</li>
        <li>Mass: ${this.character.mass}</li>
        <li>Birth Year: ${this.character.birth_year}</li>
      </ul>
    `;
  }
}

customElements.define('x-character-detail', XCharacterDetail);
```

En este ejemplo estamos creando la template en el método render en vez de definirla al principio. Como este componente no debe despachar ningun evento la template es más compacta y legible.

La idea de componentes puros es un patrón común entre los distintos frameworks basados en componentes. Esto es particularmente **importante** en los Web Components ya que es probable que ellos se vayan a mezclar mucha veces con frameworks más cerrados. Mantener los componentes genericos y puros va a ser que la integración de los componentes en otras aplicaciones sea más sencilla.
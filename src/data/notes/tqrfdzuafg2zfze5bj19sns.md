Vamos a ver como podemos integrar un Web Component en una librería de componentes de React.

React es la librería de js hecha por Facebook para que los desarrolladores puedan componer UIs con componentes. Fue la primer librería / framework en popularizar la arquitectura de componentes. Fue creado incluso antes de que se estandarizaran las APIs de Web Components. Por lo tanto no tiene un gran soporte de los Web Components como si los tienen la mayoría de los otros frameworks.

## Compatibilidad

React usa un mecanismo de comunicación similar para la comunicación entre los componentes pasando propiedades y funciones como eventos. Desgraciadamente, el sistema de eventos de React es un sistema procesado que no utiliza los eventos nativos el anvegador. Esto significa que los eventos de los Web Components no pueden comunicarse con los componentes de React. React y la sintaxis de tempaltes JSX tratan todas las propiedades de los custom elements como atributos de manera incorrecta, forzando a los usuarios a usar valores de tipo string.

Para superar estos bloqueos en nuestro ejemplo, vamos a crear componentes wrapper que envuelvan a nuestros Web Components. Esto permitira que React sea compatible con nuestros Web Components.

### Create React App
Vamos a usar la CLI de Create React App para crear la aplicación
```bash
npx create-react-app my-app
cd my-app
npm start
```

Una vez creada, instalamos nuestro componente
```bash
npm install web-component-essentials --save
```
Ahora debemos crear en nuestra aplicación, un React Dropdown que envuelva nuestro x-dropdown
```jsx
import React, {Component} from 'react';
import 'web-component-essentials'

export class Dropdown extends Component{
  render(){
    return(
      <x-dropdown>
        {this.props.children}
      </x-dropdown>
    )
  }
}
```
Importamos nuestro paquete en ```Dropdown.jsx``` y en la función render agregamos ```{this.props.children}``` para pasar los hijos elementos a nuestro slot.

## Propiedades y Eventos

Necesitamos mapear las propiedades y eventos del Web Component a nuestra versión de react. Para eso utilizaremos el hook ```componentDidMount()```

```jsx
import React, {Component} from 'react';
import 'web-component-essentials'

export class Dropdown extends Component{
  constructor(props){
    super(props)
    this.dropdownRef = React.createRef();
  }
  componentDidMount(){
    this.dropdownRef.current.title = this.props.title;
    if (this.props.onShow){
      this.dropdownRef.current.addEventListener('show', (e) => this.props.onShow(e))
    }
  }

  render(){
    return (
      <x-dropdown ref={this.dropdownRef}>
        {this.props.children}
      </x-dropdown>
    )
  }
}
```

Usando la Refs API podemos tomar la referencia a nuestro x-dropdown. con esta referencia podemos crear un ```eventListener```, en él podemos usar la función que pasemos en nuestra ```onShow``` prop de nuestro react component. Esto permitirá que el Web Component se comunique con otros componentes de React. También asignamso el ```title``` a nuestro componente React para pasarlo a nuestra propiedad del web component.

## Actualizar props

Necesitamos agregar codigo adicional para que cuando nuestras propiedades del React component cambien, esto impacte en nuestro dropdown. Para esto agregamos el hook ```componentDidUpdate()```

```jsx
import React, {Component} from 'react';
import 'web-component-essentials'

export class Dropdown extends Component{
  constructor(props){
    super(props)
    this.dropdownRef = React.createRef();
  }
  componentDidMount(){
    this.dropdownRef.current.title = this.props.title;
    if (this.props.onShow){
      this.dropdownRef.current.addEventListener('show', (e) => this.props.onShow(e))
    }
  }

  componentDidUpdate(prevProps){
    if (this.props.title !== prevProps.title){
      this.dropdownRef.current.title = this.props.title
    }
    if (this.props.show !== prevProps.show){
      this.dropdownRef.current.show = this.props.show
    }
  }

  render(){
    return (
      <x-dropdown ref={this.dropdownRef}>
        {this.props.children}
      </x-dropdown>
    )
  }
}
```
Usando el hook ```componentDidUpdate()``` podemos reaccionar cuando se actualizan las propiedades y actualizar nuestro Web Component.

Ahora tenemos todas las funcionalidades de nuestro web component andando y comunicandose con otros componentes de react.
### Qué es un Web Component
Un Web Component es una pieza aislada de interfaz con una responsabilidad única. Existen muchos frameworks que exponen esta idea de componentes pero las diferencias entre ellos impiden que podamos compartir los componentes en las distintas aplicaciones. La API de Web Components permite solucionar esta fragmentación dando reusabilidad a lo largo de las aplicaciones independientemente del framework utilizado.

La API de Web Components es una colección de APIs del navegador que en conjunto forman un combo de herramientas altamente reutilizables. 

### APIS Fundamentales de los Web Components
- [[Custom Elements | web-components.Apis.custom-elements]]
- [[Shadow Dow |web-components.Apis.shadow-dom]]
- [[Content Slot | web-components.Apis.content-slot]]
- [[Template |web-components.Apis.template]]

Podemos ver la puesta en común de estas APIS y como funcionan para crear un Web Component en la primera parte de nuestro ejemplo de [[web-components.examples.dropdown]]

### Comunicacion entre componentes

En esta sección podemos leer sobre los distintos metodos que tienen los Web Components para [[comunicars|web-components.communication]] entre ellos y con el resto de la aplicación.

- [[Propiedades |web-components.communication.js-properties]]
- [[Atributos Personalizados |web-components.communication.custom-attributes]]
- [[Eventos y Eventos Personalizados |web-components.communication.events]]

### Estilos

En esta seccion podremos encontrar las distintas estrategias que podemos tomar para [[estilar|web-components.styling]] nuestros componentes.

- [[Encapsulamiento |web-components.styling.css-encapsulation]]
- [[Propiedades Personalizadas |web-components.styling.css-custom-properties]]
- [[CSS Parts API |web-components.styling.css-parts-api]]
- [[Slotted Elements |web-components.styling.slotted-elements]]

### Temas
 
 Aca podemos ver un ejemplo de como podriamos manejar [[Temas |web-components.theming]] en nuestros componentes.

 - [[Root Host Scope |web-components.theming.root-host-scope]]

### Lifecycle

Describimos los hooks del [[Ciclo de vida |web-components.lifecycle]] de los Web Components.

- [[web-components.lifecycle.constructor-connected-callback]]
- [[web-components.lifecycle.attribute-changed]]
- [[web-components.lifecycle.disconnected-callback]]
- [[web-components.lifecycle.adopted-callback]]

### Jerarquia y Arquitectura

En esta seccion vemos el data flow de los componentes y buenas practicas para su [[Arquitectura |web-components.hierarchy-and-architecture]]

- [[Data Flow|web-components.hierarchy-and-architecture.component-data-flow]]

### Deployment

Vemos como [[Desplegar |web-components.deployment]] nuestros componentes por NPM.

- [[NPM |web-components.deployment.npm]]
- [[Soporte de navegador |web-components.deployment.browser-support]]

### Frameworks Integration
Vemos como podemos [[Integrar |web-components.frameworks-integration]] nuestros componentes en proyectos de los distintos frameworks.

- [[web-components.frameworks-integration.angular]]
- [[web-components.frameworks-integration.react]]
- [[web-components.frameworks-integration.vue]]

### Soluciones de alto nivel

Vemos dos [[Soluciones |web-components.high-level-solutions]] de alto nivel que podemos utilizar para trabajar con componentes.

- [[Libreria Lit |web-components.high-level-solutions.lit]]
- [[Framework StencilJS |web-components.high-level-solutions.stencil]]

### Ejemplos

Tenemos nuestro ejemplo de [[web-components.examples.dropdown]] que utilizamos en varias secciones para que vaya tomando complejidad.

### Herramientas Extras

- [[web-components.extra-tools.webpack]]



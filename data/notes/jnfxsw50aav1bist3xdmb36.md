El unit testing puede mejor considerablemente la calidad de nuestro codigo. Nos aseguran que el codigo que estamos cambiando sigue siendo funcional bajo unos estandares. Como los Web Components corren nativamente en el navegador, podemos correr unit test usando cualquier framework de Javascript.

Para este caso vamos a usar el componente dropdown que desarrollamos en [[Lit|web-components.high-level-solutions.lit]]. 

Los proyectos [[Open Web Components | https://open-wc.org/]] y [[Modern Web | https://modern-web.dev/]] proveen una suit de utilidades para el testeo de Web Components y Web Components construidos con Lit.

## Setting up the Test Runner

Con nuestro dropdown, podemos escribir distintos casos de test pero primero vamos a hacer la configuración incial de nuestra suite de tests. Para correr nuestros test vamos a necesitar un `test runner`. Vamos a usar `@web/test-runner`  para correrlos nativamente en el navegador.
Tambien vamos a necesitar una libreria de testeo para correr aserciones para nuestor codigo. `@open-wc` tiene unas muy buenas opciones para esto.

```json
{
  "name": "14-unit-test",
  "version": "1.0.0",
  "description": "Basic unit testing example",
  "main": "index.js",
  "scripts": {
    "start": "web-dev-server --node-resolve --open",
    "clean": "find . -name 'node_modules' -exec rm -rf '{}' +",
    "test": "web-test-runner ./**/*.spec.ts --node-resolve",
    "test:watch": "web-test-runner ./**/*.spec.ts --node-resolve --watch"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "lit": "^2.0.0"
  },
  "devDependencies": {
    "@open-wc/testing": "^2.5.33",
    "@web/dev-server": "^0.1.24",
    "@web/dev-server-esbuild": "^0.2.14",
    "@web/test-runner": "^0.13.18",
    "@web/test-runner-puppeteer": "^0.13.1",
    "typescript": "^4.4.3"
  }
}
```

El paquete `@open-wc` nos provee de utilidades para test. `@web/dev-server` y `@web/test-server` nos permite correr localhost y test runners. `@web/dev-server-esbuild` y `typescript` nos permitiran escribir nuestros componentes y nuestros unit test en typescript. 

Vamos a ver el `web-test-runner.config.mjs` en el root del proyecto.

```js
import {esbuildPlugin} from '@web/dev-server-esbuild';
import {puppeteerLauncher} from '@web/test-runner-puppeteer';

export default {
  nodeResolve: true,
  rootDir: './',
  plugins: [esbuildPlugin({ts: true, target: 'esnext'})],
  browsers: [puppeteerLauncher({
    launchOptions: {
      executablePath: '/usr/bin/chromium-browser',
    }
  })],
}
```
Esta configuracion nos permite configurar y agregar plugins a nuestros test. Como usamos Lit y Typescript podemos usar el plugin de esbuild para compilar nuestro codigo.

## Unit Test Structure

Vamos a iniciar con nuestro componente para testear lo que creamos en otras secciones

```ts
import { LitElement, html, css } from 'lit';
import { property } from 'lit/decorators/property.js';
import { customElement } from 'lit/decorators/custom-element.js';

@customElement('x-dropdown')
export class XDropdown extends LitElement {
  @property({ type: Boolean }) open = false;
  @property({ type: String }) title = 'dropdown';

  static styles = [css`
    div {
      border: 1px solid #ccc;
      padding: 12px;
    }
  `];

  render() {
    return html`
      <button @click=${() => this.toggle()}>${this.title}</button>
      ${this.open  ? html`<div><slot></slot></div>` : '' }
    `;
  }

  private toggle() {
    this.open = !this.open;
    this.dispatchEvent(new CustomEvent('openChange', { detail: this.open }));
  }
}
```

En nuestro archivo `dropdown.spec.ts` escribimos la configuracion basica de nuestro test
```ts
import {expect, fixture, html} from "@open-wc/testing";
import {Dropdown} from "./dropdown";
import "./dropdown";

describe("x-dropdown", () => {
  it('creates component', async () => {
    const el = await fixture(html`<x-dropdown></x-dropdown>`);
    expect(el.tagName).to.equal("X-DROPDOWN");
  })
});
```
El paquete `@open-wc/testing` nos provee de utilizades para testear componentes de Lit. Los test estan organizados utilizando `describe` y `it` en bloques de funciones. La funcion `describe` define que es lo que estamos testeando, y la funcion `it` corre nuestro `expect` para cada comportamiento que querramos testear.

Para nuestro primer ejemplo, nuestro test va a chequear que nuestro componente se haya creado correctamente. Como nuestro componente esta hecho con `Lit`, la template se va a renderizar de manera asincrona. Podemos usa `fixture` para esperar hasta que el componente haya sido completamente renderizado y asi poder obtener una referencia a el. Esto asegura que el componente fue renderizado y esta disponible para que nosotros accedamos dentro de nuestro test.

## Primer Test Unitario

Para nuestro primer test, podemos chequear que cuando la propiedad `open` sea `falsa`, el dropdown este oculto con `hidden`. Por el contrario que tambien este visible cuando `open=true`.

```ts
import {expect, fixture, html} from "@open-wc/testing";
import {Dropdown} from "./dropdown";
import "./dropdown";

describe("x-dropdown", () => {
  it('creates component', async () => {
    const el = await fixture(html`<x-dropdown></x-dropdown>`);
    expect(el.tagName).to.equal("X-DROPDOWN");
  })
  it('should open and close dropdown when open property is set', async () => {
    const element = await fixture<Dropdown>(html`<x-dropdown></x-dropdown>`);
    await element.updateComplete;
    expect(element.open).to.equal(false);
    expect(element.shadowRoot.querySelector('div')).to.equal(null);
  })
});
```

Usando `await` y `fixture` podemos esperar a que nuestro componente este completamente renderizado. Luego esperamos que `open=false`, podemos hacerle una query a nuestro `shadowRoot` para asegurarnos de que el `div` no se renderizo.

Para correr los test, podemos usar el siguiente comando
```bash
webn-test-runner ./**/*.spec.ts --node-resolve
```
O nuestro script que ya configuramos en el `package.json`
```bash
npm run test
```

Si todo esta bien instalado deberiamos ver el siguiente resultado en nuestra consola
![Alt text](image-29.png)

Ahora mejoremos el test para ver si cuando el `open` es `true` nuestro dropdown se muestra como es esperado.

```ts
import { expect, fixture, html, elementUpdated } from '@open-wc/testing';
import { XDropdown } from './dropdown.element.js';
import './dropdown.element.js';

describe('x-dropdown', () => {
  it('creates component', async () => {
    const element = await fixture(html`<x-dropdown></x-dropdown> `);
  });

  it('should open and close dropdown when open property is set', async () => {
    const element = await fixture<XDropdown>(html`<x-dropdown></x-dropdown> `);
    expect(element.open).to.equal(false);
    expect(element.shadowRoot.querySelector('div')).to.equal(null);

    element.open = true;
    await elementUpdated(element);
    console.log(element.shadowRoot.innerHTML)
    expect(element.shadowRoot.querySelector('div').tagName).to.equal('DIV');
  });

  it('should open and close dropdown when button is clicked', async () => {
    const element = await fixture<XDropdown>(html`<x-dropdown></x-dropdown> `);
    await element.updateComplete;

    element.shadowRoot.querySelector('button').click();
    await element.updateComplete;
    expect(element.open).to.equal(true);
    expect(element.shadowRoot.querySelector('div').tagName).to.equal('DIV');

    element.shadowRoot.querySelector('button').click();
    await element.updateComplete;
    expect(element.open).to.equal(false);
    expect(element.shadowRoot.querySelector('div')).to.equal(null);
  });
});

```

Con esto al correr `npm run test` ya podremos ver que nuestros tests pasan correctamente y hemos terminado con el unit testing.
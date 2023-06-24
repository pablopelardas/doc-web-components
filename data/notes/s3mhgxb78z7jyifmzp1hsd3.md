
En el componente MYComponent que creo la CLI de Stencil vamos a usar nuestro XDropdown.

```tsx
//my-component.tsx
import { Component, h } from '@stencil/core';

@Component({
  tag: 'my-component',
  styleUrl: 'my-component.css',
  shadow: true,
})
export class MyComponent {
  private myTitle = 'StencilJS';
  render() {
    return (
      <div>
        Hello World, I am web component built with stencil
        <div>
          <x-dropdown title={this.myTitle} onShowChange={(e) => this.log(e)}>
            Hello  <b>from Stencil</b> Dropdown!
          </x-dropdown>
        </div>
      </div>
    );
  }
  private log(e: any){
    console.log(e);
  }
}
```
![example](image-27.png)


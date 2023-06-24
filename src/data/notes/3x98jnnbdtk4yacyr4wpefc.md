```tsx
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
```

En la plantilla podemos escuchar eventos del DOM y disparar llamadas a los metodos. En nuestro ejemplo llamamos el metodo `toggle()` en el `onClick`

Para los renderizados condicionales podemos usar ternarios como en
```js
this.show
          ? <div class="x-dropdown__content"><slot /></div>
          : <div></div>
```

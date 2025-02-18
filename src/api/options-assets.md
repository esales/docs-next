# Recursos

## directives

- **Tipo:** `Object`

- **Detalhes:**

  Um conjunto de diretivas que será disponibilizado para a instância do componente.

- **Exemplo:**

  ```js
  const app = createApp({})

  app.component('focused-input', {
    directives: {
      focus: {
        mounted(el) {
          el.focus()
        }
      }
    },
    template: `<input v-focus>`
  })
  ```

- **Ver também:** [Diretivas Personalizadas](../guide/custom-directive.html)

## components

- **Tipo:** `Object`

- **Detalhes:**

  Um conjunto de componentes que será disponibilizado para a instância do componente.

- **Exemplo:**

  ```js
  const Foo = {
    template: `<div>Foo</div>`
  }

  const app = createApp({
    components: {
      Foo
    },
    template: `<Foo />`
  })
  ```

- **Ver também:** [Básico sobre Componentes](../guide/component-basics.html)

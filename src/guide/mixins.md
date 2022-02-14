# Mixins

## Fundamentos

_Mixins_ distribuem funcionalidades reutilizáveis ​​para componentes Vue. Um objeto _mixin_ pode conter quaisquer opções de componente. Quando um componente usa um _mixin_, todas as opções do _mixin_ serão "misturadas" com as opções do próprio componente.

Exemplo:

```js
// define um objeto mixin
const myMixin = {
  created() {
    this.hello()
  },
  methods: {
    hello() {
      console.log('olá do mixin!')
    }
  }
}

// definir um aplicativo que usa este mixin
const app = Vue.createApp({
  mixins: [myMixin]
})

app.mount('#mixins-basic') // => "olá do mixin!"
```

## Mesclagem de Opções

Quando um _mixin_ e o próprio componente contêm opções se sobrepondo, elas serão "mescladas" usando estratégias apropriadas.

Por exemplo, cada _mixin_ pode ter sua própria função `data`. Cada uma delas será chamada, com os objetos retornados sendo mesclados. As propriedades dos próprios dados do componente terão prioridade em caso de conflitos.

```js
const myMixin = {
  data() {
    return {
      message: 'olá',
      foo: 'abc'
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  data() {
    return {
      message: 'Tchau',
      bar: 'def'
    }
  },
  created() {
    console.log(this.$data) // => { message: "Tchau", foo: "abc", bar: "def" }
  }
})
```

Gatilhos de funções com o mesmo nome são mesclados em um Array para que todos sejam chamados. Os gatilhos do _Mixin_ serão chamados **antes** dos próprios gatilhos do componente.

```js
const myMixin = {
  created() {
    console.log('gatilho do mixin chamado')
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  created() {
    console.log('gatilho do componente chamado')
  }
})

// => "gatilho do mixin chamado"
// => "gatilho do componente chamado"
```

Opções que esperam valores em objeto, por exemplo `methods`, `components` e `directives`, serão mescladas no mesmo objeto. As opções do componente terão prioridade quando houver chaves conflitantes nestes objetos:

```js
const myMixin = {
  methods: {
    foo() {
      console.log('foo')
    },
    conflicting() {
      console.log('do mixin')
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  methods: {
    bar() {
      console.log('bar')
    },
    conflicting() {
      console.log('de si mesmo')
    }
  }
})

const vm = app.mount('#mixins-basic')

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "de si mesmo"
```

## Mixin Global

Você também pode aplicar um _mixin_ globalmente para um aplicativo Vue:

```js
const app = Vue.createApp({
  myOption: 'Olá!'
})

// injetar um manipulador para a opção personalizada `myOption`
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

app.mount('#mixins-global') // => "Olá!"
```

Use com cuidado! Depois de aplicar um _mixin_ globalmente, ele afetará **cada** instância de componente criada posteriormente no aplicativo fornecido (por exemplo, componentes filhos):

```js
const app = Vue.createApp({
  myOption: 'Olá!'
})

// injetar um manipulador para a opção personalizada `myOption`
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

// adicione myOption também ao componente filho
app.component('test-component', {
  myOption: 'Olá do componente!'
})

app.mount('#mixins-global')

// => "Olá!"
// => "Olá do componente"
```

Na maioria dos casos, você só deve usá-lo para manipulação de opções personalizadas, conforme demonstrado no exemplo acima. Também é uma boa ideia entregá-los como [Plugins](plugins.html) para evitar aplicação duplicada.

## Estratégias de Mesclagem de Opções Personalizadas

Quando as opções personalizadas são mescladas, elas usam a estratégia padrão que substitui o valor existente. Se você deseja que uma opção personalizada seja mesclada usando uma lógica personalizada, você precisa anexar uma função à `app.config.optionMergeStrategies`:

```js
const app = Vue.createApp({})

app.config.optionMergeStrategies.customOption = (toVal, fromVal) => {
  // retorna valorMesclado (mergedVal)
}
```

A estratégia de mesclagem recebe o valor dessa opção definida nas instâncias pai e filho como o primeiro e segundo argumentos, respectivamente. Vamos tentar verificar o que temos nesses parâmetros quando usamos um _mixin_:

```js
const app = Vue.createApp({
  custom: 'Olá!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => {
  console.log(fromVal, toVal)
  // => "Tchau!", undefined
  // => "Olá", "Tchau!"
  return fromVal || toVal
}

app.mixin({
  custom: 'Tchau!',
  created() {
    console.log(this.$options.custom) // => "Olá!"
  }
})
```

Como você pode ver, no console temos `toVal` e `fromVal` impresso primeiro no _mixin_ e depois no `app`. Nós sempre retornamos `fromVal` se existir, é por isso que `this.$options.custom` está configurado para `hello!` no final. Vamos tentar mudar uma estratégia para _sempre retornar um valor da instância filha_:

```js
const app = Vue.createApp({
  custom: 'Olá!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => toVal || fromVal

app.mixin({
  custom: 'Tchau!',
  created() {
    console.log(this.$options.custom) // => "Tchau!"
  }
})
```

## Desvantagens

No Vue 2, os _mixins_ eram a principal ferramenta para abstrair partes da lógica de componentes em blocos reutilizáveis. No entanto, eles têm alguns problemas:

- _Mixins_ são propensos à conflitos: como as propriedades de cada _mixin_ são mescladas no mesmo componente, você ainda precisa conhecer todos os outros _mixins_ para evitar conflitos de nome de propriedade e para depuração.

- As propriedades parecem surgir do nada: se um componente usa vários _mixins_, não é necessariamente óbvio quais propriedades vieram de cada _mixin_.

- Reutilização é limitada: não podemos passar nenhum parâmetro ao _mixin_ para alterar sua lógica, o que reduz sua flexibilidade em termos de abstração da lógica.

Para resolver esses problemas, adicionamos uma nova maneira de organizar o código por questões lógicas: A [API de Composição](composition-api-introduction.html).

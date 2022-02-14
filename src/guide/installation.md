# Instalação

Vue.js foi projetado propositalmente para ser incrementalmente adotável. Isto significa que pode ser integrado em um projeto de várias formas, dependendo de suas necessidades.

Há quatro formas principais de adicionar Vue.js a um projeto:

1. Importar [a partir de CDN](#cdn) na página desejada
2. Baixe os arquivos JavaScript e [hospede-os você mesmo](#download-and-self-host)
3. Instalar usando [npm](#npm)
4. Usar a ferramenta [CLI](#cli) oficial para criar o projeto, usando configurações previamente ajustadas para o desenvolvimento _frontend_ moderno (com _hot-reload_, _lint-on-save_ e muito mais)

## Notas de Lançamento

Última versão: ![npm](https://img.shields.io/npm/v/vue/next.svg)

Notas de lançamento detalhadas para cada versão estão disponíveis no [GitHub](https://github.com/vuejs/vue-next/blob/master/CHANGELOG.md).

## Vue Devtools

> Atualmente em versão Beta - integração com Vuex e Router ainda em andamento

<VideoLesson href="https://vueschool.io/lessons/using-vue-dev-tools-with-vuejs-3?friend=vuejs" title="Aprenda a instalar o Vue Devtools na Vue School">Aprenda como instalar e usar o Vue Devtools em uma aula gratuita da Vue School</VideoLesson>

Ao utilizar o Vue, recomendamos também instalar a extensão [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) no seu navegador, que lhe permite inspecionar e realizar _debug_ de suas aplicações Vue através de uma interface mais amigável.

[Adquirir a extensão para Chrome](https://chrome.google.com/webstore/detail/vuejs-devtools/ljjemllljcmogpfapbkkighbhhppjdbg)

[Adquirir a extensão para Firefox](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)

[Adquirir a aplicação individual, baseada em Electron](https://github.com/vuejs/vue-devtools/blob/dev/packages/shell-electron/README.md)

## CDN

Para prototipação ou aprendizagem, você pode utilizar a última versão com:

```html
<script src="https://unpkg.com/vue@next"></script>
```

Para produção, recomendamos vinculá-lo a uma versão e uma distribuição (_build_) específicas, a fim de evitar incompatibilidade de funcionalidades devido a uma nova versão.

## Baixar e hospedar em seu próprio servidor

Se você não pode usar uma CDN em produção, ou não deseja fazer uso de ferramentas de construção, você pode baixar o arquivo `.js` e hospedá-lo em seu próprio servidor web. Desta forma, você pode então incluí-lo usando uma tag `<script>`, da mesma forma como feito com a abordagem CDN.

Os arquivos podem ser encontrados e baixados de uma CDN, como [unpkg](https://unpkg.com/browse/vue@next/dist/) ou [jsDelivr](https://cdn.jsdelivr.net/npm/vue@next/dist/). Os vários arquivos diferentes serão [explicados mais tarde](#esclarecimento-sobre-as-diferentes-distribuicoes), mas normalmente você desejará baixar ambas as versões de desenvolvimento e de produção.

## npm

O método de instalação através do npm é o que recomendamos ao construir aplicações de larga escala com o Vue. Ele combina perfeitamente com empacotadores de módulos (_module bundlers_), como o [webpack](https://webpack.js.org/) ou o [Rollup](https://rollupjs.org/).

```bash
# última versão estável
$ npm install vue@next
```

O Vue também fornece ferramentas de acompanhamento para a criação de [Componentes Single-File](../guide/single-file-component.html) (SFCs). Se você quiser usar SFCs, também precisará instalar `@vue/compiler-sfc`:

```bash
$ npm install -D @vue/compiler-sfc
```

Se você está vindo do Vue 2, observe que `@vue/compiler-sfc` substitui `vue-template-compiler`.

Além do `@vue/compiler-sfc`, você também precisará de um _loader_ ou plugin SFC adequado para o empacotador escolhido. Consulte a [documentação de SFC](../guide/single-file-component.html) para obter mais informações.

Na maioria dos casos, a maneira preferida de criar uma construção com webpack e configuração mínima é usar o Vue CLI.

## CLI

Vue oferece uma [CLI oficial](https://github.com/vuejs/vue-cli) para projetar, rapidamente, ambiciosas _Single Page Applications_. Esta oferece conjuntos prontos de configurações de distribuição para um processo de desenvolvimento _frontend_ moderno. Basta apenas alguns minutos para ter tudo funcionando e sendo executado com _hot-reload_, _lint-on-save_ e distribuições (_builds_) prontas para produção. Confira a [documentação da Vue CLI](https://cli.vuejs.org) para mais detalhes.

::: tip
A CLI pressupõe que você já possui conhecimento prévio em Node.js e das ferramentas de distribuição (_build tools_) associadas. Se Vue ou as ferramentas de distribuição _frontend_ associadas são assuntos novos para você, recomendamos fortemente que você passe por todo [o guia](./introduction.html) sem qualquer ferramenta de distribuição, antes de utilizar a CLI.
:::

Para o Vue 3, você deve utilizar, no mínimo, a Vue CLI v4.5, disponível no npm como `@vue/cli`. Para atualizá-lo, você deve reinstalar o pacote `@vue/cli` globalmente, em sua última versão:

```bash
yarn global add @vue/cli
# OU
npm install -g @vue/cli
```

Logo em seguida, em seus projetos Vue, execute:

```bash
vue upgrade --next
```

## Vite

[Vite](https://github.com/vitejs/vite) é uma ferramenta de distribuição (_build tool_) para desenvolvimento _Web_ que permite servir código de maneira ultra rápida devido à sua abordagem de importação _ES Module_ nativa.

Projetos Vue podem ser rapidamente inicializados com Vite ao executar os seguintes comandos no seu _terminal_:

Com npm:

```bash
# npm 6.x
$ npm init vite@latest <nome-do-projeto> --template vue

# npm 7+, é necessário um traço duplo extra:
$ npm init vite@latest <nome-do-projeto> -- --template vue

$ cd <nome-do-projeto>
$ npm install
$ npm run dev
```

Ou com Yarn:

```bash
$ yarn create vite <nome-do-projeto> --template vue
$ yarn create vite-app <nome-do-projeto>
$ cd <nome-do-projeto>
$ yarn
$ yarn dev
```

## Esclarecimento sobre as diferentes Distribuições

No [diretório `dist/` do pacote npm](https://cdn.jsdelivr.net/npm/vue@3.0.2/dist/), você encontrará diversas distribuições (_builds_) do Vue.js. A seguir, temos uma visão geral de qual arquivo de `dist` você deve utilizar, dependendo do seu caso de uso:

### Utilizando a partir de uma CDN ou sem um Empacotador

#### `vue(.runtime).global(.prod).js`:

- Para uso direto no navegador, através de `<script src="...">`, ao expor uma variável Vue global.
- Compilação de _templates_ diretamente no navegador:
  - `vue.global.js` é a distribuição (_build_) "completa", que inclui compilador e _runtime_, a fim de dar suporte à compilação de _templates_ sob demanda.
  - `vue.runtime.global.js` possui apenas o _runtime_ e exige _templates_ pré-compilados.
- Inclui todos os pacotes internos básicos do Vue - em outras palavras, é apenas um arquivo, sem dependências externas. Isto significa que você deve importar tudo deste arquivo - e apenas deste arquivo, a fim de garantir que você sempre estará utilizando a mesma instância de código.
- Possui distribuições (_builds_) _hard-coded_ para produção e desenvolvimento, sendo a distribuição de produção já pré-minificada. Utilize arquivos `*.prod.js` em produção.

:::tip Note
Distribuições globais (_global builds_) não são distribuições [UMD](https://github.com/umdjs/umd). Elas são geradas como [IIFEs](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) e são destinadas apenas para uso direto através de `<script src="...">`.
:::

#### `vue(.runtime).esm-browser(.prod).js`:

- Para uso através de importação _ES modules_ nativa (no navegador, através de `<script type="module">`).
- Compartilha o mesmo _runtime_ de compilação, o mesmo _inlining_ de dependências e o mesmo comportamento de produção/desenvolvimento, _hard-coded_, com a distribuição (_build_) global.

### Utilizando a partir de um Empacotador

#### `vue(.runtime).esm-bundler.js`:

- Para uso de empacotadores, como `webpack`, `rollup` e `parcel`.
- Gera distribuições de produção e desenvolvimento a partir de `process.env.NODE_ENV guards` (deve ser substituído pelo empacotador)
- Não inclui distribuições (_builds_) minificadas (a fim deste processo ser realizado junto do resto do código, pelo empacotador)
- Importa dependências (por exemplo, `@vue/runtime-core`, `@vue/runtime-compiler`)
  - Dependências importadas também são distribuições (_builds_) geradas pelo _esm-bundler_ e, por sua vez, importarão suas dependências (por exemplo, `@vue/runtime-core` importa `@vue/reactivity`)
  - Isto significa que **é possível** instalar ou importar estas dependências individualmente, ficando sem instâncias diferentes. Entretanto, você deve garantir a unicidade, utilizando sempre das mesmas versões.
- Compilação de _template_ diretamente no navegador:
  - `vue.runtime.esm-bundler.js` **(padrão)** compreende apenas o _runtime_, e requer que todos os _templates_ sejam pré-compilados. Por esta razão, empacotadores (_bundlers_) acabam utilizando este arquivo, por padrão (definido em `module` no `package.json`), uma vez que, normalmente, já pré-compilam os _templates_ do projeto (por exemplo, arquivos `*.vue`)
  - `vue.esm-bundler.js`: inclui o compilador e o _runtime_. Utilize este arquivo se você está utilizando um empacotador, mas ainda assim deseja compilar _templates_ sob demanda (por exemplo, _templates_ disponíveis no DOM ou _templates_ em _strings_ do JavaScript)

### Para Renderização no lado do Servidor

#### `vue.cjs(.prod).js`:

- Para renderização no lado do servidor, em Node.js, via `require()`.
- Esta é a distribuição (_build_) que será carregada se você empacota sua aplicação com _webpack_ utilizando `target: 'node'` e externalizando `vue` apropriadamente.
- Os arquivos de desenvolvimento e produção são previamente gerados, mas o arquivo mais apropriado é automaticamente adquirido baseado em `process.env.NODE_ENV`.

## _Runtime_ + Compilador vs. somente _Runtime_

Se você precisar compilar _templates_ no cliente (por exemplo, passando uma _string_ para uma propriedade `template` ou utilizando do conteúdo de um elemento já existente no DOM do HTML), você precisará do compilador e, portanto, da distribuição (_build_) completa:

```js
// isto requer o compilador
Vue.createApp({
  template: '<div>{{ hi }}</div>'
})

// isto não requer o compilador
Vue.createApp({
  render() {
    return Vue.h('div', {}, this.hi)
  }
})
```

Ao utilizar o `vue-loader`, _templates_ dentro de arquivos `*.vue` são pré-compilados para JavaScript durante o processo de construção (_build time_). Você não precisa, necessariamente, do compilador no seu pacote (_bundle_) final e, portanto, pode utilizar apenas a distribuição _runtime_.

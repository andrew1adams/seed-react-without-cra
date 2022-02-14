# **Vallori Solutions**

## **Começando um projeto ReactJS sem o Create-React-App (CRA)**

### **Autor**: _Andrew Adams_.

### **Revisador por**: _Amanda Natallie_.

<br/>

### **Ferramentas necessárias**

Você vai precisar de algumas **ferramentas** para seguir o restante do documento:

- npm **ou** yarn (gerenciador de pacotes);
- git;
- VSCode ou qualquer outra IDE de sua preferência;

<br/>

---

### **Passo #1** ~ Iniciando o repositório e escolhendo o gerenciador de pacotes

<br/>

Inicie na pasta raiz do seu projeto o seu repositório e seu gerenciador de pacotes, seja ele `npm` ou `yarn`.

Com o comando abaixo você iniciará seu repositório:

```bash
git init
```

A partir daqui, escolha seu gerenciador de pacotes, e ele será quem dita as regras.

Com o comando abaixo você inicia o seu gerenciador de pacotes:

```bash
npm init -y
```

##### Se você preferir `npm`

<br/>

**or**

<br/>

```bash
yarn init -y
```

##### Se preferir `yarn`

<br/>

Os dois realizam a mesma tarefa, porém com algumas diferenças durante o percurso, mas não irei entrar em detalhes, a partir daqui seguirei com o `npm`.

<br/>

---

### **Passo #2** ~ Configurando a estrutura inicial de pastas do projeto

<br/>

Criaremos as **principais pastas** do projeto, e é em cima dessa estrutura de pastas que serão **configurados as dependências do nosso projeto**.

Na sua pasta raíz crie a pasta com o seguinte nome: `src`.

##### A pasta `src` ou `source`, é responsável por agrupar os principais códigos de nossa aplicação: `javascript` e/ou `typescript` como exemplo.

<br/>

Agora crie a segunda pasta, com o seguinte nome: `public`.

##### A pasta `public` é responsável por armazenar arquivos ou assets públicos, tais como: `index.html`, `favicon.ico` e `robot.txt`, como exemplo.

<br/>

---

### **Passo #3** ~ Instalando algumas dependências para um projeto ReactJS

<br/>

Como já é sabido, a primeira dependência a ser instalada é o `react`, então:

```bash
npm install react
```

E junto com o `react` instale também o `react-dom`, que é uma biblioteca que permite que o `react` se comunique com a árvore de elementos do `dom`, digite a linha de comando abaixo:

```bash
npm install react-dom
```

A próxima dependência necessária a instalar é o `babel`, como dependência de desenvolvimento, o que determina que essas dependências **não** serão utilizadas em ambiente de produção, esse conjunto de dependências são responsáveis por compilar o código com o intuito de fazer com que os `browsers` o entendam. Em suma eles transformam todo o código para `javascript` básico, então vamos lá:

```bash
npm install @babel/core @babel/cli @babel/preset-env @babel/preset-react babel-loader -D
```

Seguindo com as configurações, a próxima dependência a ser instalada é o `webpack`, que em conjunto com o `babel` complementa o _bundle_ do projeto aumentando a quantidade de extensões a serem compiladas. Abaixo, instale como dependência de desenvolvimento:

```bash
npm install webpack webpack-cli html-webpack-plugin webpack-dev-server  -D
```

Ainda pensando em otimização de ambiente de desenvolvimento e compilação de código vamos adicionar algumas dependências para lidar com arquivos de estilo como `.css` e `.sass`, dito isto, faça:

```bash
npm install node-sass style-loader css-loader sass-loader -D
```

Para ser possível trabalhar com variáveis de desenvolvimento em diferentes sistemas operacionais é necessário instalar como dependência de desenvolvimento a biblioteca `cross-env` que será responsável por universalizar essas variáveis independente do sistema operacional que esteja sendo rodado a aplicação, com isso em mente faça:

```bash
npm install cross-env -D
```

Pensando de uma forma mais performática ao trabalhar com `react`, é necessário pensar na manipulação de estados e afins, para isso iremos instalar algumas dependências que, junto com o `webpack`, serão importantes para evitar o _state refresh_ ao atualizarmos um componente na nossa aplicação:

```bash
npm install @pmmmwh/react-refresh-webpack-plugin react-refresh -D
```

A próxima dependência que vamos instalar em nossa aplicação é o `typescript`, o objetivo dessa dependência é adicionar uma tipagem estática ao `javascript` trazendo um código mais preciso e detalhado durante o desenvolvimento da aplicação. Tendo isso em mente, faça:

```bash
npm install typescript -D
```

Além de instalar a própria dependência do `typescript` será necessário adicionar também a tipagem de algumas da dependências que utilizamos anteriormente, e **atente-se a isso**, pois futuras dependências podem **precisar de tipagem**, já que nem todas as dependências vem com o código `typescript` intrísceco. Abaixo as dependências de tipos necessárias:

```bash
npm install @types/react @types/react-dom -D
```

<br/>

---

### **Passo #4** ~ Configurando nosso ambiente de desenvolvimento

<br/>

A primeira dependência que a configurar é o `babel` então, na pasta raiz, crie o arquivo `babel.config.js` e dentro dele cole a seguinte configuração:

```javascript
module.exports = {
  // `presets` é uma propriedade que determina quais presets ou configurações de ambientes do babel estão sendo utilizados dentro da aplicação.
  presets: [
    '@babel/preset-env',
    '@babel/preset-typescript',
    [
      '@babel/preset-react',
      {
        runtime: 'automatic',
      },
    ],
  ],
};
```

A próxima dependência a ser configurada é o `webpack` que na grande maioria das vezes é utilizada junto com o `babel`, então, crie na pasta raiz o arquivo `webpack.config.js` e cole as seguintes configurações:

```javascript
// Por convenção o uso do `path` é utilizado a fim de globalizar a configuração já que cada sistema operacional interpreta os caminhos das pastas de uma forma.
const path = require('path');

// _Plugin_ responsável por inserir o arquivo `bundle.js` para o nosso `index.html`.
const HtmlWebpackPlugin = require('htmp-webpack-plugin');

// Este _plugin_ é responsável por preservar os dados dos estados de nossa aplicação evitando que haja perda dessas informações durante o desenvolvimento, em fluxos pequenos da aplicação é um impacto quase imperceptível, porém caso haja um fluxo muito grande no decorrer da aplicação ele será de grande valia.
const ReactRefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin');

// Variável responsável por consultar em qual ambiente a aplicação está rodando.
const isDevelopment = process.env.NODE_ENV !== 'production';

module.exports = {
  // `mode` define o modo em que o `webpack` irá compilar o código. Nessa configuração se o ambiente estiver em desenvolvimento, o código será compilado de uma forma menos otimizada, gerando um bundle maior que o necessário, caso esteja em ambiente de produção o `webpack` otimizará ao máximo o arquivo `bundle.js` entregando um arquivo mais enxugado.
  mode: isDevelopment ? 'development' : 'production',

  // `devtool` aplica algumas mudanças na ferramenta de desenvolvimento do navegador, facilitando o entendimento caso necessite consultá-la. Se o ambiente da aplicação estiver em estágio de desenvolvimento será entregue um `eval-source-map`, que é o _source map_ mais recomendado para o tipo do ambiente, caso esteja em ambiente de produção será entregue o `source-map`, que é o _source map_ mais indicado para esse tipo do ambiente.
  devtool: isDevelopment ? 'eval-source-map' : 'source-map',

  // A propriedade `entry` determina o caminho relativo do `index.tsx` do projeto.
  entry: path.resolve(__dirname, 'src', 'index.tsx'),

  // `output` determina onde será e qual será o arquivo gerado pelo `webpack` após a compilação.
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js',
  },

  // `resolve` é a propriedade que auxilia nas importações do nosso código, facilitando o entendimento da _IDE_.
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx'],
  },

  // `devServer` cria um objeto de configurações para iniciar um servidor local que fica responsável por ficar observando as alterações no nosso projeto, atualizando sempre que houver alguma alteração.
  devServer: {
    // `static` propriedades relacionada aos arquivos e assets estáticos do projeto.
    static: {
      // `directory` caminho relativo à pasta pública do projeto.
      directory: path.resolve(__dirname, 'public'),
    },

    // `hot` essa propriedade define se o _Hot Module Replacement_ será utilizado ou não, esse módulo muda, adiciona ou remove módulos do `webpack` enquanto a aplicação está sendo executada sem recarregar a aplicação. Essa funcionalidade trás uma significante produtividade em ambiente de desenvolvimento juntamente com o _react refresh_.
    hot: true,

    // `port` porta em que este servidor local irá rodar.
    port: 3000,
  },

  // `plugins` adiciona plugins que auxiliam na performance do nosso ambiente de desenvolvimento.
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'public', 'index.html'),
    }),
    isDevelopment && new ReactRefreshWebpackPlugin(),

    // `.filter(Boolean)` é uma funcionalidade do `javascript` que permite filtrar resultados baseados nos parâmetros enviados, nesse caso ele filtras _plugins_ que não serão utilizados dependendo do estado da aplicação.
  ].filter(Boolean),

  // `module` é uma propriedade complexa, vale o estudo.
  module: {
    // `rules` é um conjunto de regras que define quais extensões são testadas e quais não serão, e também quais _loaders_ serão utilizados para interpretar cada extensão.
    rules: [
      {
        test: /\.(js|jsx|ts|tsx)$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
    ],
  },
};
```

Com o `babel` e o `webpack` configurados, daremos atenção ao `package.json`, onde encontram-se as configurações e especificações do gerenciador de pacotes do projeto. Cole as configurações abaixo:

##### As configurações abaixo podem ser inseridas em qualquer ordem dentro do `package.json`, desde que não fira a sintaxe do `json`.

```json
...
"scripts": {
  "dev": "cross-env NODE_ENV=development webpack serve",
  "build": "cross-env NODE_ENV=production webpack"
},
...
```

A próxima configuração a ser realizada é a do `typescript`, e será necessário iniciá-lo na pasta raíz do projeto, então:

```bash
npx tsc --init
```

Após isso será criado um arquivo `tsconfig.json` em sua pasta raíz, entre nele e altere para as seguintes configurações:

```json
{
  "compilerOptions": {
    // `lib` adiciona algumas tipagens das determinadas libs que são declaradas na array da propriedade.
    "lib": ["dom", "dom.iterable", "esnext"],

    // `allowJs` permite utilizar arquivos com extensões `.js` e arquivos `.ts`.
    "allowJs": true,

    // `jsx` explicita o uso do `html` dentro de um arquivo `javascript`.
    "jsx": "react-jsx",

    // `noEmit` ao executar o build da aplicação ele não emite o código fonte da aplicação.
    "noEmit": true,

    // `strict` habilita o _strict mode_ do `javascript`.
    "strict": true,

    // `moduleResolution` processo responsável por lidar com as especificações das importações no nosso projeto.
    "moduleResolution": "node",

    // `resolveJsonModule` permite utilizar arquivos com a extensão `.json` no projeto.
    "resolveJsonModule": true,

    // `isolatedModules` alerta que o código escrito não pode ser interpretado corretamente pelo  processo de transpilação.
    "isolatedModules": true,

    // `allowSyntheticDefaultImports` permite que utilize o import como padrão ao realizar um import desconhecido.
    "allowSyntheticDefaultImports": true,

    // `esModuleInterop` auxilia no tratamento de alguns erros de importação e transpilação do código.
    "esModuleInterop": true,

    // `skipLibCheck` acelera o processo de compilação.
    "skipLibCheck": true,

    // `forceConsistentCasingInFileNames` adiciona o tipo de escrita `case sensitivity` como uma regra ao projeto.
    "forceConsistentCasingInFileNames": true
  },

  // `include` propriedade que consulta onde estão os arquivos do tipo `.ts` da aplicação.
  "include": ["src"]
}
```

E por fim, mas não menos importante, configure os arquivos a serem ignorados ao enviar o projeto para o repositório, então crie o arquivo `.gitignore` na sua pasta local e adicione os seguintes arquivos a lista:

```
# Lista de itens a serem ignorados

## Dependências
/node_modules
/.pnp
.pnp.js

## Testes
/coverage

## Produção
/build
/dist

## Configurações de ambientes
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

## Logs do gerenciador de pacotes
npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

<br/>

---

### **Passo #5** ~ Primeiros passos com ReactJS

<br/>

Com o ambiente configurado e as dependências instaladas, crie o arquivo `index.html` dentro da pasta `public`, que foi criada no **Passo #2**, e dentro do arquivo cole a seguinte estrutura:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App ~ by Andrew Adams</title>
  </head>
  <body>
    <!--O elemento abaixo (div) é responsável por receber os componentes React.-->
    <div id="root"></div>
  </body>
</html>
```

Após inserir a `<div id="#root"/>` no corpo do `index.html`, crie um arquivo `index.tsx` e outro com o nome `App.tsx` dentro da pasta `src` também descriminada no **Passo #2**.

##### Insira o código abaixo no `index.tsx`:

```javascript
import ReactDOM from 'react-dom';
import App from './App';

// Com o `ReactDOM` devidamente importado, busque o método `render`, ao invocá-lo, usaremos dois parâmetros, o primeiro é o elemento que deseja renderizar e o segundo elemento é que irá recebê-lo.
ReactDOM.render(<App />, document.getElementById('root'));
```

##### insira o código abaixo no arquivo `App.tsx`:

```javascript
// Exemplo de um código `typescript` com `html` como retorno, e também principal estrutura de um componente React.
const App: React.FC = () => {
  return <h1>Hello World</h1>;
};

export default App;
```

<br/>

---

### **Passo #6** ~ Iniciando a aplicação React e _YAGNI Principle_

<br/>

Com todas as configurações definidas, todas as dependências devidamente instaladas, chegou a hora de se preocupar apenas em **desenvolver sua aplicação**, sempre respeitando um dos principais _patterns_ do mercado _**YAGNI Principle**_ que é a abreviação literal de _**You Ain't Gonna Need It**_, e esse princípio sugere que você não adicione dependências ou códigos que você ainda não precisa, **deixe a própria aplicação requisitar as dependências e funcionalidades** e você terá um código mais otimizado, com menos linhas de código e um bundle mais reduzido possível.

Em resumo, é possível ter um projeto `react` mais enxugado se as **configurações certas** forem realizadas, dando mais **performance ao projeto**. Também foi possível entender como o `react` funciona por baixo dos panos, de forma mais apŕofundada, mas sempre é bom buscar novos conhecimentos, leia as documentações, artigos e coisas que fazem de nós bons profissionais.

Para você ver o resultado de todas as configurações que fizemos, rode a seguinte linha de código:

```bash
npm run dev
```

E seja feliz.

<br />

> Esta documentação, e no momento em que foi desenvolvido, as informações coletadas são funcionais, busque referências mais atuais de acordo com o contexto no qual você está inserido. Agradeço quem tenha lido até aqui.

##### Grato, Andrew Adams.

<br/>

# Configuração inicial de projeto com ExpressJS

## Requisitos

É necessário ter instalado NodeJS e Yarn instalado.

## Configuração inicial

1 - Primeiro abra o terminal e vá na pasta onde você irá armazenar o seu projeto, então crie uma pasta com o seguinte comando:

```sh
mkdir nomeprojeto
```

2 - Entre na pasta criada e execute o seguinte comando:

```sh
yarn init -y
```

3 - Execute o código no vscode ou no editor de texto de preferência.

4 - Usando o terminal do vscode ou de sua preferência, execute o seguinte comando:

```sh
yarn add express
```

5 - Cria uma pasta src na raiz do projeto e crie alguns arquivos, conforme o seguinte:

- app.js
- routes.js
- server.js

6 - No app.js crie algo parecido com o seguinte código:

```js
const express = require("express")
const routes = require("./routes")

class App {
  constructor() {
    this.server = express()
    this.middlewares()
    this.routes()
  }

  middlewares() {
    this.server.use(express.json())
  }

  routes() {
    this.server.use(routes)
  }
}

module.exports = new App().server

```

7 - No server.js crie algo parecido com o seguinte código:

```js
const app = require("./app")

app.listen(3333)

```

8 - E no routes.js crie algo parecido com o seguinte código:

```js
const { Router } = require("express")

const routes = new Router()

routes.get("/", (req, res) => {
  return res.json({ message: "Hello World" })
})

module.exports = routes

```

9 - Para testar o funcionamento, vá no terminal e execute:

```sh
node src/server.js
```

Após isso, teste no seu navegador a seguinte url `localhost:3333` e verifique se retorna algo como:

```json
{"message":"Hello World"}
```

## Nodemon & Sucrase

O nodemon é uma dependência destinada reinicialização automática do servidor quando ocorrer uma atualização do código e o sucrase é uma dependência para mudar a importação de dependências no projeto, usando require, para o uso dos "imports" e "exports.

1 - Instale as duas dependências como dependências de desenvolvimento conforme o seguinte comando:

```sh
yarn add sucrase nodemon -D
```

2 - Após a instalação, você pode trocar todas as importações que usam o require como:

```js
const express = require("express")
const routes = require("./routes")
```

por

```js
import express from "express"
import routes from "./routes"
```

3 - Ah, você pode alterar também onde tem: 

```js
module.exports = new App().server
```

por

```js
export default new App().server
```

4 - Se quiser testar o funcionamento, o correto agora é utilizar o seguinte comando para executar o projeto:

```sh
yarn sucrase-node src/server.js
```

5 - Testado ou não, vamos configurar o nodemon para executar junto com o sucrase, sendo assim, abra o package.json e vamos adicionar um script, conforme abaixo:

```json
{
  "name": "pos-acess-ii",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "dev": "nodemon src/server.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.2",
    "sucrase": "^3.12.0"
  }
}

```

Veja que criei a parte de scripts e consta o script "dev", do jeito que está, ainda não vai ser possível rodar, então vamos a mais um passo.

6 - Crie um arquivo `nodemon.json` e coloque as seguintes linhas:

```json
{
  "execMap": {
    "js": "node -r sucrase/register"
  }
}
```

7 - Pronto, vamos executar o seguinte comando para testar:

```sh
yarn dev
```

Teste novamente a url e você verá que irá funcionar da mesma maneira.

## ESLint, Prettier & EditorConfig

Ferramentas para padronização de código entre vários devs ou até você sozinho, ajustando as identações e diversas outras coisas automaticamente, sendo assim, vamos configurar!

Ah, antes disso, lembre-se de ter os plugins das 3 ferramentas no vscode!

### Eslint

1 - Instale o eslint como dependência de desenvolvimento conforme:

```sh
yarn add eslint -D
``` 

2 - Após instalado utilize o seguinte comando para configurá-lo:

```sh
yarn eslint --init
``` 

3 - Nas perguntas posteriores, marque as opções conforme:

- How would you like to use ESLint? To check syntax, find problems, and enforce code style
- What type of modules does your project use? JavaScript modules (import/export)
- Which framework does your project use? None of these
- Does your project use TypeScript? No
- Where does your code run? Node
- How would you like to define a style for your project? Use a popular style guide
- Which style guide do you want to follow? Airbnb: https://github.com/airbnb/javascript
- What format do you want your config file to be in? JavaScript

Essas respostas podem mudar caso você não esteja usando o sucrase ou typescript, por exemplo, então fica ao seu critério de configuração, porém estou utilizando esta configuração atualmente.

Na pergunta "Would you like to install them now with npm?", pode marcar com "y" e deixar o NPM fazer a instalação das dependências.

4 - Após a instalação, é necessário apagar o arquivo `package-lock.json` e depois executar o seguinte comando para mapeamento das dependências do eslint pelo yarn:

```sh
yarn
```

5 - Sendo assim, agora vá no arquivo .eslintrc.js que foi criado na raiz do projeto e vamos configurar da seguinte forma:

```js
module.exports = {
  env: {
    es6: true,
    node: true,
  },
  extends: ['airbnb-base', 'prettier'],
  plugins: ['prettier'],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
    'prettier/prettier': 'error',
    'class-methods-use-this': 'off',
    'no-param-reassign': 'off',
    camelcase: 'off',
    'no-unused-vars': ['error', { argsIgnorePattern: 'next' }],
  },
};
```

### Prettier

1 - Instale as seguintes dependências:

```sh
yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

2 - A configuração do eslint feita anteriormente já possui algumas configurações que estariam referentes ao prettier, então, vamos só configurar mais algumas coisas devido a interferência da style guide do aribnb junto com o prettier, então crie um arquivo .prettierrc na raiz do projeto e configure-o conforme abaixo:

```json
{
  "singleQuote": true,
  "trailingComma": "es5"
}
```

### EditorConfig

1 - Pelo vscode você pode clicar com o botão direito na raiz do projeto e clicar com o botão direito e escolhe a opção "Generate EditorConfig", sendo assim ele vai gerar um arquivo .editorconfig na raiz do projeto onde você irá configurá-lo da seguinte maneira:

```
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

Pronto, agora bora pro código!
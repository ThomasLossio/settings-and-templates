# Criando e configurando projeto ReactJS

## Criando projeto

Escolha uma pasta onde você irá armazenar o projeto, abra o CMD e execute o seguinte comando:

```sh
yarn create react-app nomedoprojeto
``` 

Aguarde uns minutos para que ele possa realizar a instalação de todas as dependências iniciais.

Ao finalizar, vá para pasta que foi criada e abra no editor de texto de sua preferência.

## Limpando algumas configurações iniciais

Costumo realizar uma configuração diferente que o create react-app (CRA) utiliza, então vamos lá.

1 - Apague o Readme.md, pois já vem um padrão do CRA e algumas instruções que talvez não sirvam ao decorrer do projeto;
2 - Em package.json, apague o seguinte:

```json
  "eslintConfig": {
    "extends": "react-app"
  },
```

3 - Em public/index.html, gosto de apagar os comentários para ficar uma coisa mais limpa, mas até hoje não tive problemas de usar ou não usar haha, então fica ao seu critério.

4 - Se for um projeto que não vai ser utilizado como PWA, no public/index.html, apague a linha relacionada ao manifest.json, deve ter algo parecido com:

```js
<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
```

E também apague o manifest.json que está dentro da pasta public.

5 - Sendo assim, faz um teste executando `yarn start` no terminal e veja se ele está executando normal, se sim, passe pro próximo passo.

6 - Agora vamos tirar algunas arquivos que não vão ser necessários pro decorrer do projeto:

	- public/logo192.png (nesse vai ser necessário retirar a referência que está dentro do index.html, para esta imagem, pois vai dar erro depois)
	- public/logo512.png
	- src/App.css
	- src/App.test.js
	- src/index.css
	- src/logo.svg
	- serviceWorker.js (apenas se você não for utilizar PWA)

7 - Agora vá no arquivo index.js e retire as linhas referentes ao serviceWorker, tais como:

```js
import * as serviceWorker from './serviceWorker';
...
...
serviceWorker.unregister();
```

8 - Ainda em index.js, retire a importação referente ao CSS como:

```js
import './index.css';
```

9 - Vá no App.js e retire as importações sobre CSS e logo como:

```js
import logo from './logo.svg';
import './App.css';
```

10 - Deixe seu App.js parecido com o seguinte código:

```js
import React from "react"

function App() {
  return (
    <div className="App">
      <h1>Hello World</h1>
    </div>
  )
}

export default App

```

## Configurando Eslint, Prettier e EditorConfig

Primeiro é necessário que tenha as extensões do VSCode já instaladas para poder funcionar os seguintes passos:

### EditorConfig

Clica com o botão direito na raiz do projeto e procura por "Generate .editorConfig"

Vai ser gerado um arquivo onde você vai adaptá-lo ao seguinte modelo:

```
root = true

[*]
end_of_line = lf
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

### Eslint

Instale o Eslint como dependência de desenvolvimento executando o seguinte comando:

```sh
yarn add eslint -D
```

Após instalado, execute o seguinte comando:

```sh
yarn eslint --init
```

Siga as seguintes questões:

```
- How would you like to use ESLint? To check syntax, find problems, and enforce code style
- What type of modules does your project use? JavaScript modules (import/export)
- Which framework does your project use? React
- Does your project use TypeScript? No
- Where does your code run? Browser
- How would you like to define a style for your project? Use a popular style guide
- Which style guide do you want to follow? Airbnb (https://github.com/airbnb/javascript)
- What format do you want your config file to be in? Javascript
```

Pode rodar como NPM, após ele baixar tudo, vamos deletar o arquivo package-lock.json que foi criado e depois utilizamos `yarn` para que faça o mapeamento com o yarn.

### Prettier

Antes de continuar com a configuração do ESLint, vamos instalar o prettier e configurá-lo.

Execute o seguinte comando:

```sh
yarn add prettier eslint-config-prettier eslint-plugin-prettier babel-eslint -D
```

### Continuando o ESLint

Para configurar, utilize o seguinte template no seu .eslintrc.js:

Template/ReactJS-Eslint

### Continuando o prettier

Para configurar, crie um arquivo chamado .prettierrc e coloque o seguinte template:

Template/ReactJS-Prettier


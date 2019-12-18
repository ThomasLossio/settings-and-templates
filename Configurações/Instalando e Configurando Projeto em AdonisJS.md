# Instalando e Configurando Projeto em AdonisJS

Este tutorial foi feito para quem já possui NodeJS e NPM ou Yarn instalado em sua máquina.

Primeiramente é necessário a instalação da CLI do Adonis, você pode instalar executando algum dos seguintes comandos:

```sh
npm install -g @adonisjs/cli
```

ou

```sh
yarn global add @adonisjs/cli
```

Espere a instalação terminar e execute `adonis` no terminal, se constar uma lista de comandos, significa que foi instalado com sucesso.

## Inicializando Projeto Api-only

Este tutorial é referente ao uso do adonis com seu funcionamento apenas como API, sendo assim, você pode executar algum dos seguintes comandos, ficando a seu critério usar NPM ou Yarn:

```sh
adonis new newproject --api-only
```

ou, para utilização com Yarn:

```sh
adonis new newproject --api-only --yarn
```

Espere os passos de configuração inicial, após isso, execute o seguinte comando para testar o funcionamento:

```sh
adonis serve --dev
```

Se tudo tiver ok, ele irá inicializar e você conseguirá acessar via navegador a seguinte url: http://localhost:3333/

Se tiver ok, irá aparecer um json conforme o seguinte:

```json
{
	"greeting":"Hello world in JSON"
}
```

## Configuração ESLINT, Prettier e EditorConfig Adonisjs

Antes de tudo, para o funcionamento ocorrer 100%, é necessário que você possua as extensões do vscode instaladas.

### EditorConfig

Por padrão, o AdonisJS já vem com o .editorconfig, sendo assim, só ajuste-o conforme sua preferência. No meu caso eu utilizo a seguinte que já é vem padrão no Adonis:

```
# editorconfig.org
root = true

[*]
indent_size = 2
indent_style = space
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false

```

### Eslint

Para instalação do eslint, vá no terminal dentro da pasta do projeto e execute:

```sh
npm install eslint --dev
```

ou

```sh
yarn add eslint --dev
```

Espere a instalação terminar e execute o seguinte comando:

```sh
npx eslint --init
```

Nas próximas opções, tenta selecionar as seguintes opções:

- How would you like to use ESLint? To check syntax, find problems, and enforce code style
- What type of modules does your project use? None of these
- Which framework does your project use? None of these
- Does your project use TypeScript? No
- Where does your code run? Node
- How would you like to define a style for your project? Use a popular style guide
- Which style guide do you want to follow? Standard (https://github.com/standard/standard)
- What format do you want your config file to be in? JSON

Se aparecer opção perguntando se deseja executar com o NPM, defina como "Yes", espere a instalação concluir e depois vamos apagar o arquivo package-lock.json que foi criada na raiz do projeto e executar o seguinte comando no terminal com o yarn para mapeamento das dependências pelo yarn.lock:

```sh
yarn
```

### Prettier

Para o prettier, de instalação basta a extensão no vscode, sendo assim, apenas precisamos criar o arquivo `.prettierrc` na raiz do projeto e adicionar o seguinte:

```json
{
  "semi": false
}
```

Esta opção é devido ao `'use strict'` que consta nas linhas iniciais dos arquivos do adonis.

Pronto, projeto configurado e agora bora codar!
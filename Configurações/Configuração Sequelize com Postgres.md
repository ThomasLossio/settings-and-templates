# Configuração Sequelize com Postgres

## Requisitos

É necessário já ter um projeto com a estrutura inicial com expressjs.

## Configuração Inicial

1 - Supondo que você seguiu o tutorial de inicialização de um projeto em expressjs, nesse mesmo repositório, agora vamos criar umas pastas a mais. Dentro da pasta src, você vai criar os seguintes diretórios:



Conforme a imagem acima, foi criado uma pasta `app`, contendo duas pastas: `controllers` e `models`. Também foi criado uma pasta `config` onde contém um arquivo `database.js`, que temporariamente ficará vazio e, por último, a pasta `database`, contendo a pasta `migrations`.

2 - Agora abra o terminal na pasta do projeto e execute o seguinte comando para a instalação do sequelize:

```sh
yarn add sequelize
```

3 - Após isso, vamos adicionar a CLI do sequelize como dependência de desenvolvimento com o seguinte comando:

```sh
yarn add sequelize-cli -D
```

A CLI do sequelize refere-se à uma interface de linha de comando, onde poderemos executar os comandos que o sequelize permite, sendo assim, necessitaremos dela.

4 - Crie um arquivo na pasta raiz do projeto, chamado `.sequelizerc`, dependendo do seu editor, configure a linguagem aceita por esse arquivo como "Javascript" e vamos preencher as seguintes configurações:

```js
const { resolve } = require('path');

module.exports = {
  config: resolve(__dirname, 'src', 'config', 'database.js'),
  'models-path': resolve(__dirname, 'src', 'app', 'models'),
  'migrations-path': resolve(__dirname, 'src', 'database', 'migrations'),
  'seeders-path': resolve(__dirname, 'src', 'database', 'seeds'),
}

```

Esse arquivo, não é possível realizar o uso do import e export que definimos anteriormente, então devemos usar o require e module.exports da forma que o express já aceita.

Este arquivo, contém algumas configurações de como o sequelize irá entender onde vamos definir os models, migrations, seeders e o arquivo de configuração de banco, onde vamos definir qual banco.

5 - Pronto, depois de criado, agora vamos baixar as dependências relacionadas ao postgres, sendo assim, execute o seguinte comando:

```sh
yarn add pg pg-hstore
```

6 - Agora vá no `database.js` e configure-o parecido com o seguinte:

```js
module.exports = {
  dialect: 'postgres',
  host: 'localhost',
  username: 'postgres',
  password: 'password',
  database: 'nomedatabase',
  define: {
    timestamps: true,
    underscored: true,
    underscoredAll: true,
  },
};

```

No código acima, você irá fazer alteração nos campos username (caso seja diferente do padrão), password e database, mas lembrando que você deve criar o database com antecedência antes de começar a executar o projeto. Após as alterações, seu sequelize estará configurado.

## Testando funcionamento

Os passos anteriores eram referentes às configurações do sequelize com o postgres, porém precisamos testar, para isso, vamos criar uma migration e tentar executá-la. Dessa forma, saberemos se a configuração feita, está realmente se comunicando com seu banco de dados.

### Criando uma migration

Primeiro, vamos criar uma migration referente à table usuários, vamos utilizar os comandos da CLI, sendo assim, execute o seguinte comando:

```sh
yarn sequelize migration:create --name=create-users
```

Esse comando irá criar uma migration de "create-users" automaticamente pra gente, então não altere o nome que o sequelize der ao arquivo, pois será uma referência de data de criação da migration, fazendo com que o sequelize entenda a sequência que ele deve executar as migrations.

Agora vá no arquivo criado e preencha-o mais ou menos o seguinte:

```js
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('users', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      email: {
        type: Sequelize.STRING,
        allowNull: false,
        unique: true,
      },
      password_hash: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      created_at: {
        type: Sequelize.DATE,
        allowNull: false,
      },
      updated_at: {
        type: Sequelize.DATE,
        allowNull: false,
      },
    });
  },

  down: (queryInterface) => {
    return queryInterface.dropTable('users');
  },
};
```

Essa migration irá criar uma tabela no banco, chamada "users", onde irá conter os campos id, name, email, password_hash, created_at e updated_at, sendo assim, vamos executá-la da seguinte maneira:

```sh
yarn sequelize db:migrate
```

Você pode conferir que no seu banco de dados foi criado a table com extamente esses campos, sendo assim, sabemos que a configuração está ok e que está se comunicando com seu banco de dados.



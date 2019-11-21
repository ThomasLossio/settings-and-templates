Configurando imagens Postgresql, Redis, Mongo e SQLite no docker do windows 10

Apenas comandos das principais imagens.

# Postgresql

Informações dos parâmetros:

- name: Fica a seu critério, é apenas para definir como será o nome da imagem que será criada;
- POSTGRES_PASSWORD: Também fica a seu critério, costumo usar em testes locais uma password fácil;
- p: Porta que o docker irá escutar e para qual deve ser direcionada. O comum é utilizar 5432:5432, porém se você já tiver o postgresql instalado na sua máquina, você pode chamar esta imagem em outra porta, por exemplo 5433:5432, onde você está informando que a porta 5433 do seu windows, deve escutar as requisições para a imagem do postgresql no docker;
- d: Significa que você quer rodar a imagem em segundo plano;
- postgres:11 -> A imagem que você vai usar, opto pela postgres:11, mas fica aos seu critério.

```sh
docker run --name database -e POSTGRES_PASSWORD=adm123 -p 5432:5432 -d postgres:11
```

Se ele não achar na sua máquina, será feito um download da imagem. Após finalizar use `docker ps` e verifique se a imagem está rodando conforme os parâmetros que você passou, se sim, pode conectar no PGAdmin ou PostBird ou outro de preferência no servidor.

Caso você esteja usando o docker tools, no windows home, busque usar o seguinte host para conectar: 192.168.99.100

# Mongo

Informações dos parâmetros:

- name: Fica a seu critério, é apenas para definir como será o nome da imagem que será criada;
- p: Porta que o docker irá escutar e para qual deve ser direcionada. O comum é utilizar a porta padrão 27017:27017;
- d: Significa que você quer rodar a imagem em segundo plano;
- t: A imagem que você vai usar, opto pela "mongo", mas fica aos seu critério caso tenha outra.

```sh
docker run --name mongotest -p 27017:27017 -d -t mongo
```

Se ele não achar na sua máquina, será feito um download da imagem. Após finalizar use `docker ps` e verifique se a imagem está rodando conforme os parâmetros que você passou, se sim, tente entrar na seguinte URL pelo navegador: `localhost:27017`

Se aparecer uma mensagem como:

`It looks like you are trying to access MongoDB over HTTP on the native driver port.`

Significa que já está instalado e mãos à obra!


# Redis

Informações dos parâmetros:

- name: Fica a seu critério, é apenas para definir como será o nome da imagem que será criada;
- p: Porta que o docker irá escutar e para qual deve ser direcionada. O comum é utilizar a porta padrão 6379:6379;
- d: Significa que você quer rodar a imagem em segundo plano;
- t: A imagem que você vai usar, opto pela "redis:alpine", mas fica aos seu critério caso tenha outra.

```sh
docker run --name redistest -p 6379:6379 -d -t redis:alpine
```

Se ele não achar na sua máquina, será feito um download da imagem. Após finalizar use `docker ps` e verifique se a imagem está rodando conforme os parâmetros que você passou, se sim ou não, tente usar o seguinte comando: `docker logs nomedaimagem`, onde nomedaimagem você irá trocar pelo nome que você criou, se for baseado neste exemplo, ficaria algo como redistest.

Se tiver dado certo, ele vai aparecer que já está disponível para aceitar conexões, senão, vai ter informando o erro que não permitiu criar.

# Sqlite
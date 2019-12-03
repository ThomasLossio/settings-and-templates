# Como atualizar variável da coleção após execução da url de sessão no postman 7.12.0

Como optei utilizar o postman como ferramenta de montagem e envio de requisições para APIs no meu desenvolvimento, analisei a automação da atualização da variável "token" que defino para coleção que criei. Sendo assim, primeiro, faça o teste da requisição que você criou para Session, se tudo deu certo, então vamos gerar a automação desse processo de atualizar a variável.

1 - Vá na edição da Collection e procure por "Variables".

2 - Adicione uma variável chamada "token" e defina o "Initial value" e "Current Value" com qualquer coisa, não precisa ser nada válido ^^, pode deixar até em branco.

3 - Salve e agora vá na Request da sessão, você irá encontrar uma aba "Tests" e escreva um código parecido com esse:

```js
var jsonData = pm.response.json();
pm.collectionVariables.set("token", jsonData.token);
```

Caso seja necessário, pode-se alterar os parâmetros do SET, onde o primeiro parâmetro, equivale ao nome da variável de ambiente que vocês criou, que no meu caso foi "token" e, no segundo, você pode mudar a propriedade do jsonData que o postman deve acessar, pois você pode estar usando algum outro parâmetro para retornar o token.

4 - Pronto, agora execute a requisição de novo e verifique se a variável de ambiente foi alterada pelo mesmo token que veio na resposta da sua requisição. Se não tiver dado certo, verifique os parâmetros corretamente, qualquer coisa, pesquisa a maneira certa de se fazer na versão que você estiver utilizando, pois isso aqui foi testado (pelo menos por mim kk), apenas na versão 7.12.0.

Local de pesquisa: https://learning.getpostman.com/docs/postman/environments-and-globals/variables/
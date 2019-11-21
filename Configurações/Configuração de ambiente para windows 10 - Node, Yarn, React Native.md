Configuração de ambiente para windows 10 - Node, Yarn e React Native

# Node

## Instalar Chocolatey

Conferir sempre que possível o site oficial: chocolatey.org

Porém, segue um passo a passo.

1 - Abrir o powershell em modo administrador.

2 - Execute o seguinte comando `Get-ExecutionPolicy`. Caso esteja como "Restricted" utilize o seguinte comando:

```sh
Set-ExecutionPolicy AllSigned
```

Depois use o seguinte comando:

```sh
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Para saber que instalou, observe se existe a seguinte frase: "Choloatey (choco.exe) is now ready.". Ah, pode testar se deu certo também, tentando utilizar "choco" no terminal (talvez seja necessário reiniciar o terminal), daí ele vai apresentar a versão do Chocolatey.

Se tiver dado certo, passe pro próximo passo, senão, ainda não sei, atualizarei quando ocorrer algo :D.

3 - Utilize o seguinte comando:

```sh
cinst nodejs-lts
``` 

Este comando irá instalar a versão LTS atual do Node.

Após conclusão da instalação, reinicie o terminal e teste `node -v` e `npm -v`, deverá aparecer em tela as versões deles, se sim, tudo ok, senão algo a ser analisado ^^.

# Yarn

Existem 2 maneiras, baixe a versão diretamenta pelo site yarnpkg.com ou pelo choloatey utilizando o seguinte comando:

```sh
cinst yarn
```

Após instalação, reinicie o terminal e teste `yarn -v` para ver se consta a versão do mesmo. Se deu certo, passe pro próximo passo, senão análise ^^.

# React Native

Será instalado via chocolatey e também precisa do node. Mas se seguiu o passo ali para instalar o node via chocolatey, então tudo bem, senão faz o passo anterior primeiro e passa para cá ^^.

Ah, se tiver você já tiver o jdk também, favor verifique se está na versão 8, senão roda o comando completo, senão retira o o jdk8 do meio.

## Chocolatey e dependências iniciais

1 - Rode o seguinte comando:

```sh
cinst -y python2 jdk8
```

2 - Após instalação completa, fecha o terminal e abra novamente e instale o CLI do react-native conforme o comando a seguir:

```sh
yarn global add react-native-cli
```

3 - Para verificar se deu certo, execute o seguinte comando:

```sh
react-native -h
```

Se mostrar algo, sucesso, vamos para outro passo.

## Configurando SDK

1 - Crie uma pasta onde você irá armazenar a instalação do SDK. Ex: C:\Android\Sdk

2 - Vá em https://developer.android.com/studio/#downloads, na opção "Command line tools only" e baixe o SDK referente ao seu SO

3 - Após o download, extraia o conteúdo dentro da pasta criada anteriormente.

4 - Agora vá nas variáveis de ambiente do Windows e clique para adicionar uma nova variável de ambiente do SISTEMA.

Em Variable Name, deve colocar como ANDROID_HOME e em Variable Value deve colocar o local da pasta anterior, por exemplo C:\Android\Sdk

5 - Ainda em variáveis de ambiente, procure pela variável PATH e clique em editar. Nesta parte você poderá adicionar os seguintes caminhos:

- %ANDROID_HOME%\platform-tools
- %ANDROID_HOME%\tools

6 - Agora abra um novo terminal como administrador e execute o seguinte comando:

- Command Prompt (Funcionou somente aqui desta vez):

```sh
C:\Android\Sdk\tools\bin\sdkmanager  "platform-tools" "platforms;android-27" "build-tools;27.0.3"
```

- PowerShell:

```sh
PS C:\Android\Sdk\tools\bin> .\sdkmanager.bat  "platform-tools" "platforms;android-27" "build-tools;27.0.3"
```

Lembre-se de alterar o caminho caso você tenha colocado em outro canto.

## Emulador (Genymotion)

Caso sua preferência seja de executar os projetos pelo celular, pule esta etapa, pois este passo irá instalar um emulador android e, esta opção pode comprometer o uso do Docker caso instalado. Dê preferência por aqui somente se seu projeto não estiver utilizando Docker.

1 - Baixe e instale o virtualbox, você pode encontrar o download aqui: https://www.virtualbox.org/wiki/Downloads

2 - Entre no seguinte site https://www.genymotion.com/fun-zone/ e clique no botão "Download Genymotion Personal Edition". Provavelmente será solicitado que você se cadastre para poder realizar o download do software.

3 - Abra o Genymotion e realize seu login. Após o login, vá no menu "Genymotion->Settings", procure por ADB e especifique o caminho do SDK, aquela pasta criada anteriormente, por exemplo C:\Android\Sdk

4 - Feche a tela de configuração e busque por uma plataforma de sua preferência.

Tenho usado o "Samsung Galaxy S* - 7.0.0 API 24", mas fique a vontade na escolha :D.

5 - Dê 2 cliques no emulador de preferência, irá aparecer uma tela para defini Nome e outras configurações da emulação. Caso queira, pode mudar o nome, já nas demais informações deixe como está e clique em "Install"

6 - Após a instalação, caso não tenha o docker funcionando no momento, irá abrir o emulador normalmente. Caso reclame do docker, desative o Hyper-V do windows pela opção de "Ativar ou desativar recursos do Windows".

## Emulador (USB)

Este passo é para caso vá emular diretamente pelo celular via USB.

1 - Primeiramente, habilite a função de desenvolvedor do seu celular (é interessante pesquisar na internet como fazer, pois cada celular pode ser de um jeito diferente).

2 - Com a opção habilitada, no menu de configurações deve ter aparecido algo como "Opções do desenvolvedor". Nessa parte você deverá ativar 2 coisas, a opção propriamente dita, logo no início :D e depois você tem que ativar a opção "Depuração USB".

3 - Após isso, conecte o cabo no computador, abra o terminal do windows e digite `adb devices`. Ele irá iniciar uma listagem de dispositivos permitidos ou não, se aparecer um dispositivo com a opção "unauthorized", provavelmente deve estar aparecendo no celular para você Permitir o acesso e tal, se aparecer o dispostivo, só que com a opção "device", tudo ok, pronto para uso.

4 - Para testar se está funcionando, pegue um projeto e execute `react-native run-android`. 

---------------------------------------------
Caso aconteça erro de invalid regular expression, utilizando versão 12+ do node, há 2 soluçoes:

1 - Realiza o downgrade da versão para 11 ou 10 do nodejs;

2 - Executa os seguintes passo conforme https://github.com/facebook/react-native/issues/26598#issuecomment-555047977:

>I am on Windows 10, what I did is the following:
>
>react-native init appName --version="0.60.0"
>open windows explorer and find this path in your app dir node_modules\metro-config\src\defaults there you will see blacklist.js, you can open it and edit the sharedBlacklist variable (array with regexs) >by removing all / (forward slash symbol) which are not in the begging or in the end of a row (if there are 4-5 rows of regex, you should leave there only the first and last forward slashes)
>open another terminal window and cd into the dir (appName) then react-native start (to start a metro server)
>cd appName && react-native run-android in the first terminal window


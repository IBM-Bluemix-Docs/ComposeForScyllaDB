---

copyright:
  years: 2016,2018
lastupdated: "2018-05-29"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Tutorial de introdução
Este tutorial usa o app de amostra [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) para demonstrar como usar o Node.js para se conectar a um serviço {{site.data.keyword.composeForScyllaDB_full}}. O aplicativo cria, lê e grava em um banco de dados utilizando os dados fornecidos por meio da interface da web do app.
{: shortdesc}

## Antes de iniciar

É necessário ter uma conta do [{{site.data.keyword.cloud_notm}}][ibm_cloud_signup_url]{:new_window}.

Também é necessário instalar o [Node.js](https://nodejs.org/) e [Git](https://git-scm.com/downloads).

## Etapa 1: Criar uma instância de serviço do {{site.data.keyword.composeForScyllaDB}}
{: #create-service}

É possível criar um serviço do {{site.data.keyword.composeForScyllaDB}} por meio da página [{{site.data.keyword.composeForScyllaDB}}](https://console.{DomainName}/catalog/services/compose-for-scylladb/) no catálogo do {{site.data.keyword.cloud_notm}}.

Escolha um nome do serviço e uma região, organização e espaço no qual provisionar o serviço e para o campo **Selecionar uma versão do banco de dados**, escolha _Versão preferencial mais recente_.

Em seguida, escolha um plano de precificação para o seu serviço. É possível escolher os planos *Padrão* ou *Corporativo*. Com o plano *Corporativo*, é possível provisionar sua instância do {{site.data.keyword.composeForScyllaDB}} em um cluster disponível do {{site.data.keyword.composeEnterprise}}. O {{site.data.keyword.composeEnterprise}} fornece a segurança e o isolamento requeridos pela conformidade corporativa e usa rede dedicada para assegurar o desempenho dos bancos de dados implementados. Consulte a documentação do  [ {{site.data.keyword.composeEnterprise}} ](/docs/services/ComposeEnterprise/index.html)  para obter mais detalhes.

Clique em  ** Criar **  para provisionar seu serviço. O fornecimento pode demorar um pouco para ser concluído. É possível verificar o progresso acessando a visualização _Gerenciar_ do serviço.

Não será possível conectar um aplicativo ao serviço até que o fornecimento esteja concluído.
{: .tip}

## Etapa 2: Clonar o aplicativo de amostra Hello World por meio do Github

Clone o app Hello World para o seu ambiente local do seu terminal usando o comando a seguir:

```
git clone https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs.git
```

## Etapa 3: Instalar as dependências do app

Use npm para instalar dependências.

1. De seu terminal, mude o diretório para o local em que o app de amostra está localizado.
  
  ```
  cd compose-scylladb-helloworld-nodejs
  ```

2. Instale as dependências listadas no arquivo `package.json`.
  
  ```
  npm install
  ```

## Etapa 4: Fazer download e instalar a ferramenta da CLI do {{site.data.keyword.cloud_notm}}

A ferramenta da CLI do {{site.data.keyword.cloud_notm}} é o que você vai usar para se comunicar com o {{site.data.keyword.cloud_notm}} de seu terminal ou linha de comandos. Para obter detalhes, veja [Faça download e instale a CLI do {{site.data.keyword.cloud_notm}}](https://console.{DomainName}/docs/cli/reference/bluemix_cli/download_cli.html).

## Etapa 5: Conectar-se ao  {{site.data.keyword.cloud_notm}}

1. Conecte-se ao {{site.data.keyword.cloud_notm}} na ferramenta de linha de comandos e siga os prompts para efetuar login.

  ```
  ibmcloud login
  ```

  Se você tiver um ID de usuário federado, use o comando `ibmcloud login --sso` para efetuar login com seu ID de conexão única. Veja [Efetuando login com um ID federado](https://console.{DomainName}/docs/cli/login_federated_id.html#federated_id) para saber mais.
  {: .tip}

2. Certifique-se de que você esteja direcionando para a organização e o espaço do {{site.data.keyword.cloud_notm}} corretos.

  ```
  ibmcloud target --cf
  ```

  Escolha nas opções fornecidas, usando os mesmos valores usados ao criar o serviço.

## Etapa 6: Atualizar o arquivo manifest do app
{: #update-manifest}

O {{site.data.keyword.cloud_notm}} usa um arquivo manifest - `manifest.yml` para associar um aplicativo com um serviço. Siga estas etapas para criar o seu arquivo manifest.

1. Em um editor, abra um novo arquivo e inclua o seguinte:

  ```
  ---
  applications:
  - name:    compose-scylladb-helloworld-nodejs
    host:    compose-scylladb-helloworld-nodejs
    memory:  128M
    services:
      - my-compose-for-scylladb-service
  ```

2. Mude o valor `host` para algo exclusivo. O host que você escolher determinará o subdomínio da URL do seu aplicativo: `<host>.mybluemix.net`.
3. Mude o valor `name`. O valor que você escolher será o nome do app como aparece em seu painel do {{site.data.keyword.cloud_notm}}.
4. Atualize o valor `services` para corresponder ao nome do serviço criado em [Criar uma instância de serviço do {{site.data.keyword.composeForScyllaDB}}](#create-service). 
  
## Etapa 7: Empurre o app para  {{site.data.keyword.cloud_notm}}.

Quando você enviar por push o app, ele será automaticamente ligado ao serviço especificado no arquivo manifest.

```
bx cf push
```

Essa etapa falhará se o serviço não tiver concluído o fornecimento da Etapa 1. É possível verificar seu progresso acessando a visualização _Gerenciar_ do serviço.
{: .tip}
  
## Etapa 8: Verificar se o app está conectado ao serviço {{site.data.keyword.composeForScyllaDB}}

1. Navegue até o painel de serviço do {{site.data.keyword.composeForScyllaDB}}
2. Selecione _Conexões_ no menu do painel. O seu aplicativo deve ser listado sob _Aplicativos conectados_.

Se o seu aplicativo não estiver listado, repita as Etapas 7 e 8, inserindo os detalhes corretos no [manifest.yml](#update-manifest).

## Etapa 9: Usar o app

Agora, quando você visita `<host>.mybluemix.net/`, você será capaz de visualizar o conteúdo de sua coleção do {{site.data.keyword.composeForScyllaDB}}. Conforme você inclui palavras e as suas definições, elas são incluídas no banco de dados e exibidas. Se você parar e reiniciar o app verá que qualquer palavra e definição já incluída agora será listada.

## Executando o app localmente

Em vez de enviar o app por push para o {{site.data.keyword.cloud_notm}}, é possível executá-lo localmente para testar a conexão com a instância de serviço do {{site.data.keyword.composeForScyllaDB}}. Para se conectar ao serviço será necessário criar um conjunto de credenciais de serviço.

1. No painel do {{site.data.keyword.cloud_notm}}, abra sua instância de serviço do {{site.data.keyword.composeForScyllaDB}}.
2. Selecione _Credenciais de serviço_ no menu principal para abrir a visualização Credenciais de serviço.
3. Clique em **Nova credencial**.
4. Escolha um nome para as suas credenciais e clique em **Incluir**.
5. As suas novas credenciais agora estão listadas. Clique em **Visualizar credenciais** na linha correspondente da tabela para visualizar as credenciais e clique no ícone **Copiar** para copiar as suas credenciais.
6. Em seu editor de escolha, crie um novo arquivo com o seguinte, inserindo as suas credenciais conforme mostrado:

  ```
  {
    "services": {
      "compose-for-scylladb": [
        {
          "credenciais": INSIRA AS SUAS CREDENCIAIS AQUI }
      ]
    }
  }
  ```
7. Salve o arquivo como `vcap-local.json` no diretório no qual o app de amostra está localizado.

Para evitar expor acidentalmente as suas credenciais ao enviar por push um aplicativo para o Github ou o {{site.data.keyword.cloud_notm}}, certifique-se de que o arquivo que contém as suas credenciais seja listado no arquivo ignorar relevante. Se você abrir `.cfignore` e `.gitignore` em seu diretório de aplicativo verá que `vcap-local.json` é listado em ambos, de maneira que isso não será incluído nos arquivos que serão transferidos por upload quando você enviar por push o app para Github ou {{site.data.keyword.cloud_notm}}.
{: .tip}

Agora inicie o servidor local.

```
npm start
```

O app agora está em execução em [http://localhost:8080](http://localhost:8080). É possível incluir palavras e definições no banco de dados do {{site.data.keyword.composeForScyllaDB}}. Quando você parar e reiniciar o app, qualquer palavra já incluída será exibida ao atualizar a página.

Para obter informações sobre as credenciais que você criou para o aplicativo para se conectar ao seu serviço, veja [Credenciais disponíveis](./connecting-bluemix-app.html#available-credentials).

## Etapas seguintes

Para entender mais sobre como o app de amostra [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) funciona, é possível ler o arquivo leia-me do aplicativo ou os comentários do código no `server.js`, que fornecem algumas informações sobre as funções do app.

Para começar a explorar o serviço {{site.data.keyword.composeForScyllaDB}}, veja os tópicos a seguir sobre o painel de serviço:

- [Visão geral do painel](./dashboard-overview.html)
- [Backups](./dashboard-backups.html)
- [Configurações](./dashboard-settings.html)


[ibm_cloud_signup_url]: https://ibm.biz/compose-for-scylladb-signup

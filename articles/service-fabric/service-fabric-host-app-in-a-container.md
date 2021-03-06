---
title: "Implantar um aplicativo .NET em um contêiner do Azure Service Fabric | Microsoft Docs"
description: "Ensina você a empacotar um aplicativo .NET no Visual Studio em um Contêiner do Docker. Esse novo aplicativo de “contêiner” é então implantado em um cluster do Service Fabric."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: a965fcb9dd5caf9656af5ae381b25baaaa01ec6d
ms.contentlocale: pt-br
ms.lasthandoff: 05/10/2017

---

# <a name="deploy-a-net-app-in-a-container-to-azure-service-fabric"></a>Implantar um aplicativo .NET em um contêiner do Azure Service Fabric

Este tutorial mostra como implantar um aplicativo ASP.NET existente em um contêiner do Windows usando a versão prévia do Visual Studio 2017 atualização 3. Em seguida, mostra como implantar o contêiner no Azure usando o Visual Studio Team Services e hospedar o contêiner em um cluster do Service Fabric.

## <a name="prerequisites"></a>Pré-requisitos

1. Instalar o [Docker CE para Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) de forma que você possa executar contêineres no Windows 10.
2. Familiarizar-se com o [início rápido dos Contêineres do Windows 10][link-container-quickstart].
3. Neste artigo, usamos o **Fabrikam Fiber**, um aplicativo de exemplo que pode ser baixado [aqui][link-fabrikam-github].
4. [Azure PowerShell][link-azure-powershell-install]
5. [Extensão Ferramentas de Entrega Contínua para Visual Studio 2017][link-visualstudio-cd-extension]

>[!NOTE]
>Se essa for a primeira vez que você estiver executando imagens de contêiner do Windows no computador, o Docker CE deverá obter as imagens base para os contêineres. As imagens usadas neste tutorial têm 14 GB. Vá em frente e execute o seguinte comando do PowerShell para receber as imagens base:
>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

## <a name="containerize-the-application"></a>Colocar o aplicativo em um contêiner

Para começar a executar o aplicativo em um contêiner, precisamos adicionar o **Suporte do Docker** ao projeto no Visual Studio. Quando você adiciona o **Suporte do Docker** ao aplicativo, duas coisas acontecem. Primeiro, um arquivo _docker_ é adicionado ao projeto. Esse novo arquivo descreve como a imagem de contêiner deve ser criada. Em segundo lugar, um novo projeto _docker-compose_ é adicionado à solução. Esse novo projeto contém alguns arquivos docker-compose, que podem ser usados para descrever como o contêiner é executado.

Mais informações sobre como trabalhar com as [Ferramentas de Contêiner do Visual Studio][link-visualstudio-container-tools].

### <a name="add-docker-support"></a>Adicionar suporte ao Docker

1. Abra o arquivo **FabrikamFiber.CallCenter.sln** no Visual Studio

2. Clique com o botão direito do mouse no projeto **FabrikamFiber.Web** > **Adicionar** > **Suporte do Docker**.

### <a name="add-support-for-sql"></a>Adicionar suporte para SQL

Esse aplicativo usa o SQL como o provedor de dados e, portanto, um SQL Server é necessário para executar o aplicativo. Neste tutorial, usamos o SQL Server em execução em um contêiner.
Para informar ao Docker que desejamos executar um SQL Server em um contêiner, podemos referenciar uma imagem de contêiner do SQL Server em nosso arquivo docker-compose.override.yml no projeto docker-compose. Dessa forma, o SQL Server em execução no contêiner é usado ao depurar o aplicativo no Visual Studio.

1. Abra o **Gerenciador de Soluções**.

2. Abra **docker-compose** > **docker-compose.yml** > **docker-compose.override.yml**.

3. No nó `services:`, adicione um novo nó chamado `db:`. Esse nó declara executar um SQL Server em um contêiner.

   ```yml
     db:
       image: microsoft/mssql-server-windows-developer
       environment:
         sa_password: "Password1"
         ACCEPT_EULA: "Y"
       ports:
         - "1433"
       healthcheck:
         test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
         interval: 1s
         retries: 20
   ```

   >[!NOTE]
   >Você pode usar qualquer SQL Server que preferir para a depuração local, desde que ele seja acessível no host. No entanto, **localdb** não dá suporte à comunicação `container -> host`.

   >[!NOTE]
   >Se você sempre desejar executar o SQL Server em um contêiner, poderá optar por adicionar o anterior ao arquivo docker-compose.yml em vez de ao arquivo docker-compose.override.yml.


4. Modifique o nó `fabrikamfiber.web` e adicione um novo nó filho chamado `depends_on:`. Isso garante que o serviço `db` (o contêiner do SQL Server) é iniciado antes de nosso aplicativo Web (fabrikamfiber.web).

   ```yml
     fabrikamfiber.web:
       ports:
         - "80"
       depends_on:
         - db
   ```

5. No projeto **FabrikamFiber.Web**, atualize a cadeia de conexão no arquivo **web.config** para que ela aponte para o SQL Server no contêiner.

   ```xml
   <add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
   
   <add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
   ```

   >[!NOTE]
   >Se você desejar usar outro SQL Server ao criar um build de versão de seu aplicativo Web, adicione outra cadeia de conexão ao arquivo web.release.config.

6. Pressione **F5** para executar e depurar o aplicativo no contêiner.

   O Edge abre a página de inicialização definida do aplicativo usando o endereço IP do contêiner na rede NAT interna (geralmente, 172.x.x.x). Para saber mais sobre como depurar aplicativos em contêineres usando o Visual Studio 2017, consulte [este artigo][link-debug-container].

   ![exemplo de fabrikam em um contêiner][image-web-preview]

O aplicativo agora está pronto para ser compilado e empacotado em um contêiner. Depois de compilar a imagem de contêiner no computador, você poderá enviá-la por push para qualquer registro de contêiner e efetuar pull dela em qualquer host para execução.

No restante deste tutorial, você usará o Visual Studio Team Services para compilar e implantar o contêiner, enviá-lo por push para um Registro de Contêiner do Azure e implantá-lo no Service Fabric, em execução no Azure.

## <a name="create-a-service-fabric-cluster"></a>Criar um cluster do Service Fabric

Se você já tiver um cluster do Service Fabric para implantar em seu aplicativo, ignore esta etapa. Caso contrário, vamos continuar e criar um Cluster do Service Fabric.

>[!NOTE]
>O procedimento a seguir cria um cluster do Service Fabric, protegido por um certificado autoassinado, que é colocado em um KeyVault, criado como parte da implantação. Para obter mais informações sobre como usar a autenticação do Azure Active Directory, consulte o artigo [Criar um cluster do Service Fabric usando o Azure Resource Manager][link-servicefabric-create-secure-clusters].

1. Baixe uma cópia local do modelo e dos arquivos de parâmetros do Azure referenciados a seguir.
    * [Modelo do Azure Resource Manager para o Service Fabric][link-sf-clustertemplate] – o modelo do Resource Manager que define um Cluster do Service Fabric.
    * [Arquivo de parâmetros de modelo][link-sf-clustertemplate-parameters] – um arquivo de parâmetros para personalizar a implantação do cluster.
2. Personalize os seguintes parâmetros no arquivo de parâmetros:
  
   | Parâmetro       | Descrição |
   | --------------- | ----------- |
   | clusterName     | Nome do cluster. |
   | adminUserName   | A conta do administrador local nas máquinas virtuais do cluster. |
   | adminPassword   | Senha da conta do administrador local nas máquinas virtuais do cluster. |
   | clusterLocation | A região do Azure na qual o cluster será implantado. |
   | vmInstanceCount | O número de nós no cluster (pode ser 1) |

3. Abra o **PowerShell**.
4. **Faça logon** no Azure.

   ```powershell
   Login-AzureRmAccount
   ```

5. Selecione a **assinatura** na qual você deseja implantar o cluster.

   ```powershell
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

6. Crie e **criptografe uma senha** para o certificado usado pelo Service Fabric.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```

7. **Crie o cluster** executando o seguinte comando:

   ```powershell
   New-AzureRmServiceFabricCluster 
       -TemplateFile C:\users\me\downloads\PreviewSecureClusters.json `
       -ParameterFile C:\users\me\downloads\myCluster.parameters.json `
       -CertificateOutputFolder C:\users\me\desktop\ `
       -CertificatePassword $pwd `
       -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
       -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >O parâmetro `-CertificateSubjectName` deve ser alinhado com o parâmetro clusterName, especificado no arquivo de parâmetros e ao domínio vinculado à região do Azure escolhida, como `clustername.eastus.cloudapp.azure.com`.
   
    Quando a configuração for concluída, ela gerará informações sobre o cluster criado no Azure, além de copiar o certificado para o diretório -CertificateOutputFolder.

8. **Clique duas vezes** no certificado para instalá-lo no computador local.

## <a name="deploy-with-visual-studio"></a>Implantação com o Visual Studio

Para configurar a implantação usando o Visual Studio Team Services, você precisa instalar a [extensão Ferramentas de Entrega Contínua para Visual Studio 2017][link-visualstudio-cd-extension]. Essa extensão facilita a implantação no Azure configurando o Visual Studio Team Services e implanta seu aplicativo no cluster do Service Fabric.

Para começar, o código deve ser hospedado no controle do código-fonte. No restante desta seção, pressupomos que o **git** esteja sendo usado.

1. No canto inferior direito do Visual Studio, clique em **Adicionar ao Controle de Código-Fonte** > **Git** (ou a opção que preferir).

   ![pressionar o botão de controle do código-fonte][image-source-control]

2. No painel _Team Explorer_, pressione **Publicar Repositório Git**.

3. Selecione o nome do repositório VSTS e pressione **Repositório**.

   ![publicar o repositório no VSTS][image-publish-repo]

Agora que seu código está sincronizado com um repositório de origem do VSTS, você pode configurar a integração e a entrega contínuas.

1. No _Gerenciador de Soluções_, clique com o botão direito do mouse em **solução** > **Configurar a Entrega Contínua**.

2. Selecione a Assinatura do Azure.

3. Defina **Tipo de Host** como **Cluster do Service Fabric**.

   >[!NOTE]
   >Dependendo dos tipos de contêineres que você está criando, adicionamos mais opções para hospedar seu aplicativo em contêineres do Azure. 

4. Defina **Host de Destino** como o cluster do Service Fabric criado na seção anterior.

5. Escolha um **Registro de Contêiner** no qual o contêiner será publicado.

   >[!TIP]
   >Use o botão **Editar** para criar um registro de contêiner.
    
6. Pressione **OK**.

   ![configurar a integração contínua do service fabric][image-setup-ci]
   
   Depois que a configuração for concluída, o contêiner será implantado no Service Fabric sempre que você enviar atualizações por push ao repositório.

7. **Inicie um build** usando o **Team Explorer** e veja seu aplicativo de contêiner em execução no Service Fabric.

Agora que você colocou a solução Fabrikam Call Center em um contêiner e a implantou, poderá abrir o [portal do Azure][link-azure-portal] e ver o aplicativo em execução no Service Fabric. Para experimentar o aplicativo, abra um navegador da Web e acesse a URL do cluster do Service Fabric.

## <a name="next-steps"></a>Próximas etapas

- [Ferramentas de Contêiner no Visual Studio][link-visualstudio-container-tools]
- [Introdução aos contêineres no Service Fabric][link-servicefabric-containers]
- [Criando aplicativos do Service Fabric][link-servicefabric-createapp]

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-sf-clustertemplate-parameters]: https://aka.ms/securepreviewonelineclusterparameters

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png


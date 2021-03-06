---
title: "Crie seu primeiro aplicativo de microsserviços do Azure no Linux usando Java | Microsoft Docs"
description: Criar e implantar um aplicativo do Service Fabric usando Java
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: e229602b4bfa72977c9b15e854d796ed09fa55d2
ms.contentlocale: pt-br
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-service-fabric-java-application-on-linux" class="xliff"></a>

# Como criar seu primeiro aplicativo em Java do Service Fabric no Linux
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Com este guia de início rápido você criará seu primeiro aplicativo em Java do Azure Service Fabric em um ambiente de desenvolvimento Linux em alguns minutos.  Quando terminar, você terá um aplicativo simples de serviço único Java em execução no cluster de desenvolvimento local.  

<a id="prerequisites" class="xliff"></a>

## Pré-requisitos
Antes de começar, instale o SDK do Service Fabric, a CLI do Azure e configure um cluster de desenvolvimento no seu [ambiente de desenvolvimento Linux](service-fabric-get-started-linux.md). Se estiver usando o Mac OS X, você poderá [configurar um ambiente de desenvolvimento Linux em uma máquina virtual usando Vagrant](service-fabric-get-started-mac.md).

Você também deve configurar a [CLI do Azure 2.0](service-fabric-azure-cli-2-0.md) (recomendado) ou a [CLI XPlat](service-fabric-azure-cli.md) para implantar seu aplicativo.

<a id="create-the-application" class="xliff"></a>

## Criar o aplicativo
Um aplicativo do Service Fabric pode conter um ou mais serviços, cada um com uma função específica no fornecimento de funcionalidade do aplicativo. O SDK do Service Fabric para Linux inclui um gerador [Yeoman](http://yeoman.io/) que facilita a criação de seu primeiro serviço e a adição de mais serviços posteriormente.  Você também pode criar, compilar e implantar aplicativos em Java do Service Fabric usando um plug-in para Eclipse. Confira [Como criar e implantar seu primeiro aplicativo em Java usando o Eclipse](service-fabric-get-started-eclipse.md). Para este início rápido, use Yeoman para criar um aplicativo com um único serviço que armazena e obtém um valor do contador.

1. Em um terminal, digite ``yo azuresfjava``.
2. Nome do seu aplicativo. 
3. Escolha o tipo de seu primeiro serviço e dê um nome para ele. Para este tutorial, escolha um serviço de ator confiável. Para obter mais informações sobre os outros tipos de serviços, confira [Visão geral do modelo de programação do Service Fabric](service-fabric-choose-framework.md).
   ![Gerador de Yeoman do Service Fabric para Java][sf-yeoman]

<a id="build-the-application" class="xliff"></a>

## Compilar o aplicativo
Os modelos Yeoman do Service Fabric incluem um script de compilação para [Gradle](https://gradle.org/), que pode ser usada para compilar o aplicativo no terminal. Para compilar e empacotar o aplicativo, execute o seguinte:

  ```bash
  cd myapp
  gradle
  ```

<a id="deploy-the-application" class="xliff"></a>

## Implantar o aplicativo
Após a compilação do aplicativo, você pode implantá-lo no cluster local.

<a id="using-xplat-cli" class="xliff"></a>

### Usando a CLI XPlat

1. Conectar-se ao cluster local do Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Use o script de instalação fornecido no modelo para copiar o pacote de aplicativo no repositório de imagens do cluster, registre o tipo de aplicativo e crie uma instância do aplicativo.

    ```bash
    ./install.sh
    ```

<a id="using-azure-cli-20" class="xliff"></a>

### Usando a CLI do Azure 2.0

A implantação do aplicativo interno é igual a qualquer outro aplicativo do Service Fabric. Confira a documentação sobre [como gerenciar um aplicativo do Service Fabric com a CLI do Azure](service-fabric-application-lifecycle-azure-cli-2-0.md) para obter instruções detalhadas.

Os parâmetros para esses comandos podem ser encontrados nos manifestos gerados dentro do pacote de aplicativos.

Depois da implantação do aplicativo, abra um navegador e navegue até [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) em [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Em seguida, expanda o nó **Aplicativos** e observe que agora há uma entrada para o seu tipo de aplicativo e outra para a primeira instância desse tipo.

<a id="start-the-test-client-and-perform-a-failover" class="xliff"></a>

## Inicie o cliente de teste e execute um failover
Atores não fazem nada por conta própria, eles precisam de outro serviço ou cliente para enviar mensagens. O modelo de ator inclui um script de teste simples que você pode usar para interagir com o serviço de ator.

1. Execute o script usando o utilitário de inspeção para ver a saída do serviço de ator.  O script de teste chama o `setCountAsync()`método no ator para incrementar um contador, o `getCountAsync()` método no ator para obter o novo valor de contador e exibe o valor para o console.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. No Service Fabric Explorer, localize o nó que hospeda a réplica primária para o serviço de ator. Na captura de tela abaixo, é o nó 3. A réplica do serviço primária é responsável pelas operações de leitura e gravação.  Então, as alterações do estado do serviço são replicadas para as réplicas secundárias, em execução nos nós 0 e 1 na captura de tela abaixo.

    ![Localizar a réplica primária no Service Fabric Explorer][sfx-primary]

3. Em **Nós**, clique no nó encontrado na etapa anterior e selecione **Desativar (Reiniciar)** no menu Ações. Esta ação reiniciará o nó executando a réplica do serviço primária e força um failover para uma das réplicas secundárias em execução em outro nó.  Essa réplica secundária é promovida para principal, outra réplica secundária é criada em um nó diferente e a réplica primária começa a executar operações de leitura/gravação. Conforme o nó reinicia, preste atenção na saída do cliente de teste e observe que o contador continua a aumentar apesar do failover.

<a id="add-another-service-to-the-application" class="xliff"></a>

## Adicione outro serviço para o aplicativo
Para adicionar outro serviço para um aplicativo existente usando `yo`, execute as seguintes etapas:
1. Altere o diretório para a raiz do aplicativo existente.  Por exemplo, `cd ~/YeomanSamples/MyApplication`, se `MyApplication` é o aplicativo criado pelo Yeoman.
2. Execute o `yo azuresfjava:AddService`
3. Compile e implante o aplicativo, conforme as etapas anteriores.

<a id="remove-the-application" class="xliff"></a>

## Remoção do aplicativo
Use o script de desinstalação fornecido no modelo para excluir a instância do aplicativo, cancele o registro do pacote do aplicativo e remova o pacote de aplicativo do repositório de imagens do cluster.

```bash
./uninstall.sh
```

No Service Fabric Explorer, observe que o aplicativo e o tipo de aplicativo não aparecem mais no nó **Aplicativos**.

<a id="next-steps" class="xliff"></a>

## Próximas etapas
* [Como criar seu primeiro aplicativo em Java do Service Fabric no Linux usando o Eclipse](service-fabric-get-started-eclipse.md)
* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interação com clusters do Service Fabric usando a CLI do Azure](service-fabric-azure-cli.md)
* [Solução de problemas de implantação](service-fabric-azure-cli.md#troubleshooting)
* Saiba mais sobre as [opções de suporte do Service Fabric](service-fabric-support.md)

<a id="related-articles" class="xliff"></a>

## Artigos relacionados

* [Introdução ao Service Fabric e a CLI do Azure 2.0](service-fabric-azure-cli-2-0.md)
* [Introdução à CLI XPlat do Service Fabric](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png


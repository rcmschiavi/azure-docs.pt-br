---
title: "Introdução às filas do Barramento de Serviço do Azure | Microsoft Docs"
description: "Escreva um aplicativo de console em C# que usa filas de mensagens do Barramento de Serviço."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 02d0ce093bc42cffa4f3993826c61c8aeca4d033
ms.contentlocale: pt-br
ms.lasthandoff: 07/01/2017


---
# Introdução às filas do Barramento de Serviço
<a id="get-started-with-service-bus-queues" class="xliff"></a>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## O que será realizado
<a id="what-will-be-accomplished" class="xliff"></a>
Este tutorial cobre as seguintes etapas:

1. Crie um namespace do Barramento de Serviço usando o portal do Azure.
2. Crie uma fila do Barramento de Serviço usando o portal do Azure.
3. Escreva um aplicativo de console para enviar uma mensagem.
4. Escreva um aplicativo de console para receber as mensagens enviadas na etapa anterior.

## Pré-requisitos
<a id="prerequisites" class="xliff"></a>
1. [Visual Studio 2015 ou superior](http://www.visualstudio.com). Os exemplos neste tutorial usam o Visual Studio 2017.
2. Uma assinatura do Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 1. Criar um namespace usando o portal do Azure
<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>
Se você já criou um namespace de Mensagens do Barramento de Serviço, vá para a seção [Criar uma fila usando o portal do Azure](#2-create-a-queue-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## 2. Criar uma fila usando o portal do Azure
<a id="2-create-a-queue-using-the-azure-portal" class="xliff"></a>
Se você já criou uma fila do Barramento de Serviço, vá para a seção [Enviar mensagens para a fila](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## 3. Enviar mensagens para a fila
<a id="3-send-messages-to-the-queue" class="xliff"></a>
Para enviar mensagens para a fila, escreveremos um aplicativo de console em C# usando o Visual Studio.

### Criar um aplicativo de console
<a id="create-a-console-application" class="xliff"></a>

Inicie o Visual Studio e crie um novo projeto de **Aplicativo de console (.NET Framework)**.

### Adicionar o pacote NuGet do Barramento de Serviço
<a id="add-the-service-bus-nuget-package" class="xliff"></a>
1. Clique com o botão direito do mouse no projeto recém-criado e selecione **Gerenciar Pacotes NuGet**.
2. Clique na guia **Procurar**, pesquise **Barramento de Serviço do Microsoft Azure** e selecione o item **WindowsAzure.ServiceBus**. Clique em **Instalar** para concluir a instalação e feche essa caixa de diálogo.
   
    ![Selecionar um pacote NuGet][nuget-pkg]

### Escrever um código para enviar uma mensagem para a fila
<a id="write-some-code-to-send-a-message-to-the-queue" class="xliff"></a>
1. Adicione a seguinte instrução `using` à parte superior do arquivo Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Adicione o seguinte código ao `Main` método. Defina a variável `connectionString` para a cadeia de conexão que você obteve ao criar o namespace e defina `queueName` como o nome da fila que você usou ao criar a fila.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Seu arquivo Program.cs deve ficar assim.
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Execute o programa e verifique o portal do Azure: clique no nome da fila na folha **Visão geral** do namespace. A folha **Essentials** da fila é exibida. Observe que agora o valor de **Contagem de Mensagens Ativas** deve ser 1. Cada vez que você executa o aplicativo de remetente sem recuperar mensagens, esse valor aumenta em 1. Observe também que o tamanho atual da fila aumenta cada vez que o aplicativo adiciona uma mensagem à fila.
   
      ![Tamanho da mensagem][queue-message]

## 4. Receber mensagens da fila
<a id="4-receive-messages-from-the-queue" class="xliff"></a>

1. Para receber as mensagens recém-enviadas, crie um novo aplicativo de console e adicione uma referência ao pacote NuGet do Barramento de Serviço, parecido com o aplicativo de envio anterior.
2. Adicione a seguinte instrução `using` à parte superior do arquivo Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Adicione o seguinte código ao `Main` método. Defina a variável `connectionString` para a cadeia de conexão que você obteve ao criar o namespace e defina `queueName` como o nome da fila que você usou ao criar a fila.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    O arquivo Program.cs deve ficar assim:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Execute o programa e verifique o portal novamente. Observe que os valores **Contagem de Mensagens Ativas** e **Atuais** agora são 0.
   
    ![Comprimento da fila][queue-message-receive]

Parabéns! Agora, você criou uma fila, enviou uma mensagem e recebeu uma mensagem.

## Próximas etapas
<a id="next-steps" class="xliff"></a>

Confira nosso [Repositório GitHub com exemplos](https://github.com/Azure/azure-service-bus/tree/master/samples) que demonstram alguns dos recursos mais avançados de mensagens do Barramento de Serviço.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


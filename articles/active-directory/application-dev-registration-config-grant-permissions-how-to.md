---
title: "Como conceder permissões a um aplicativo desenvolvido de forma personalizada | Microsoft Docs"
description: "Como conceder permissões ao seu aplicativo personalizado usando o portal do Azure AD ou um parâmetro de URL"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 922774c2482737537b64787ae473231ec1fbb68e
ms.contentlocale: pt-br
ms.lasthandoff: 04/14/2017

---

<a id="how-to-grant-permissions-to-a-custom-developed-application" class="xliff"></a>

# Como conceder permissões a um aplicativo desenvolvido de forma personalizada

Para conceder consentimento de forma preventiva em seu aplicativo ou se estiver executando um erro não consentido em um aplicativo, tente as etapas a seguir.

<a id="how-to-perform-admin-consent-for-your-application" class="xliff"></a>

## Como executar consentimento de administrador em seu aplicativo

Isso tem o efeito de conceder consentimento ao aplicativo a todos os usuários na organização.

1. Navegue até a folha **Registros do aplicativo** como um **administrador global** e selecione o aplicativo.

2. Selecione **Permissões Necessárias**, e finalmente toque no botão **Conceder Permissões** na parte superior da folha.

Como alternativa, você pode construir uma solicitação para *login.microsoftonline.com* com as configurações do seu aplicativo e anexar um consentimento *&prompt=admin\_*. Após entrar com credenciais de administrador, o aplicativo recebe consentimento para todos os usuários.

<a id="how-to-force-user-consent-for-your-application" class="xliff"></a>

## Como forçar consentimento de usuário em seu aplicativo

* Anexe nas solicitações de autenticação *&prompt=consent* que exige consentimento dos usuários finais cada vez que se autenticam.

<a id="next-steps" class="xliff"></a>

## Próximas etapas

[Consentir e integrar aplicativos ao AzureAD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Consentimento e permissão para aplicativos convergidos do AzureAD v2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[StackOverflow do AzureAD](http://stackoverflow.com/questions/tagged/azure-active-directory)


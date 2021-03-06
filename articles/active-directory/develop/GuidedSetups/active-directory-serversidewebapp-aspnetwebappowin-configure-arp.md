---
title: "Introdução ao servidor Web ASP.NET no Azure AD v2 – Configuração | Microsoft Docs"
description: "Implementando a opção Entrar com uma Conta da Microsoft em uma solução ASP.NET com um aplicativo tradicional baseado em navegador da Web usando o padrão OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: 6c6a44cbbd79cf6422515a6d70d48ac12cba4f9f
ms.contentlocale: pt-br
ms.lasthandoff: 07/06/2017


---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Configurar o aplicativo Web ASP.NET com informações de registro do aplicativo

Nesta etapa, você configurará seu projeto para usar o SSL e, em seguida, usará a URL do SSL para configurar as informações de registro do aplicativo. Depois disso, adicione as informações de registro do aplicativo à sua solução por meio do *web.config*.

1.  No Gerenciador de Soluções, selecione o projeto e examine a janela `Properties` (se uma janela Propriedades não for exibida, pressione F4)
2.  Altere `SSL Enabled` para `True`:<br/>
![Propriedades do projeto](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
3.  Copie o valor de `SSL URL` acima e cole-o no campo `Redirect URL` na parte superior desta página e, depois, clique em *Atualizar*:
4.  Adicione o seguinte ao arquivo `web.config` localizado na pasta da raiz, na seção `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```


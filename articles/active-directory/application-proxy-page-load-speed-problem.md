---
title: Um aplicativo de Proxy de Aplicativo demora muito para carregar | Microsoft Docs
description: "Solucionar problemas de desempenho de carregamento de página com o Proxy de Aplicativo do Azure AD"
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
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: e172f55d45bccfd126928225eb9f78da61a670d3
ms.contentlocale: pt-br
ms.lasthandoff: 04/17/2017

---

<a id="an-application-proxy-application-takes-too-long-to-load" class="xliff"></a>

# Um aplicativo de Proxy de Aplicativo demora muito para carregar

Este artigo ajuda-o a entender porque um aplicativo de Proxy de Aplicativo do Azure AD pode demorar muito tempo para carregar e o que pode ser feito para resolver esse problema.

<a id="overview" class="xliff"></a>

## Visão geral
Se seus aplicativos estão funcionando, mas você percebe uma longa latência, pode haver alguns pequenos ajustes na sua topologia de rede que podem ser considerados para melhorar a velocidade. Para uma avaliação de diferentes topologias, consulte o [documento de considerações de rede](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Se essas considerações não ajudarem, infelizmente nós não temos atualmente outras recomendações para ajuste de desempenho. À medida que o serviço de Proxy de Aplicativo se expande para mais data centers que podem estar mais próximos a você, será possível começar a perceber a latência melhorada diretamente. Para ver a lista completa de data centers do Azure, você pode consultar a [página de teste de latência](http://www.azurespeed.com/Azure/Latency). 

Os data centers com o serviço de Proxy de Aplicativo podem ser localizados com a [Ferramenta de Teste de Portas do Conector](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

<a id="feedback-on-application-proxy-data-center-locations" class="xliff"></a>

## Comentários sobre locais de data centers do Proxy de Aplicativo 
Pode existir data centers do Azure que ainda não incluem Proxy de Aplicativo, mas poderiam conduzi-lo a uma grande melhoria de latência. Locais de data center em <aadapfeedback@microsoft.com>, desse modo, podemos usar seu comentário para planejar conforme expandimos.

Nós estamos trabalhando em alguns recursos adicionais que ajudam a melhorar a latência para locatários que atualmente percebem latências longas e, assegure-se de compartilhar a documentação disponível.

<a id="next-steps" class="xliff"></a>

## Próximas etapas
[Trabalhar com servidores proxy locais existentes](application-proxy-working-with-proxy-servers.md)


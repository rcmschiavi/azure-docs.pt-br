---
title: "Notificações do Azure Active Directory Identity Protection | Microsoft Docs"
description: "Saiba como as notificações dão suporte às suas atividades de investigação."
services: active-directory
keywords: "azure active directory identity protection, cloud app discovery, gerenciamento de aplicativos, segurança, risco, nível de risco, vulnerabilidade, política de segurança"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: ce3379d5b5e883c6601c40aca191e8b84e3ad8d3
ms.openlocfilehash: 0170b5d2435f6e856478ee9e55ae26c626288f75
ms.contentlocale: pt-br
ms.lasthandoff: 12/22/2016


---
<a id="azure-active-directory-identity-protection-notifications" class="xliff"></a>
# Notificações do Azure Active Directory Identity Protection
O Azure AD Identity Protection envia dois tipos de emails de notificação automatizados para ajudar você a gerenciar o risco do usuário e eventos de risco:

* Email de alerta de usuário comprometido
* Email de resumo semanal

<a id="user-compromised-alert-email" class="xliff"></a>
## Email de alerta de usuário comprometido
Um email de alerta de usuário comprometido é gerado quando o Azure AD Identity Protection identifica uma conta como comprometida. O email inclui um link para os Usuários sinalizados para o relatório de risco no painel do Identity Protection. É recomendável investigar imediatamente as notificações das contas comprometidas.

<a id="weekly-digest-email" class="xliff"></a>
## Email de resumo semanal
O email de resumo semanal contém um resumo dos novos eventos de risco.<br>
Ele inclui:

* Usuários em risco
* Atividades suspeitas
* Vulnerabilidades detectadas
* Links para os relatórios relacionados no Identity Protection

<br>
![Correção](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br>

É possível desativar o envio de um email de resumo semanal.
<br><br>
![Riscos do usuário](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>

**Para abrir o diálogo de configurações relacionadas**:

1. Na folha **Azure AD Identity Protection**, clique em **Configurações**.
   <br><br>
   ![Política de risco do usuário](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
   <br>
2. Na seção **Geral**, clique em **Notificações**.
   <br><br>
   ![Política de risco do usuário](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
   <br>

<a id="see-also" class="xliff"></a>
## Consulte também
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)


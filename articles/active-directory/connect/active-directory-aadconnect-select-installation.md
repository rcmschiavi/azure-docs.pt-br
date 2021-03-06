---
title: "Azure AD Connect: Selecionar o tipo de instalação | Microsoft Docs"
description: "Este tópico explica como selecionar o tipo de instalação para o Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 81549c4bc0cabcf46bc7dc66d7e6662d7f787b1e
ms.openlocfilehash: df6353f84f898a1722e3e985244a90dbd6b22522
ms.contentlocale: pt-br
ms.lasthandoff: 01/26/2017

---
# Selecione o tipo de instalação para o Azure AD Connect
<a id="select-which-installation-type-to-use-for-azure-ad-connect" class="xliff"></a>
O Azure AD Connect oferece dois tipos de instalação para uma nova instalação: Expressa e personalizado. Este tópico ajuda você a decidir qual opção usar durante a instalação.

## Express
<a id="express" class="xliff"></a>
Expressa é a opção mais comum e é usada por cerca de 90% de todas as novas instalações. Ela foi projetada para fornecer uma configuração que funcione para os cenários mais comuns dos clientes.

Ela pressupõe que:

- Você tem uma única floresta local do Active Directory.
- Você tem uma conta de administrador corporativo que pode usar para a instalação.
- Há menos de 100.000 objetos em seu Active Directory local.

Você recebe:

- [Sincronização de senha](active-directory-aadconnectsync-implement-password-synchronization.md) do local para o Azure AD para logon único.
- Uma configuração que sincroniza [usuários, grupos, contatos e computadores com Windows 10](active-directory-aadconnectsync-understanding-default-configuration.md).
- Sincronização de todos os objetos qualificados em todos os domínios e em todas as UOs.
- A [Atualização automática](active-directory-aadconnect-feature-automatic-upgrade.md) está habilitada para garantir que você use sempre a versão mais recente disponível.

Opções onde ainda é possível usar a Expressa:

- Se você não quiser sincronizar todas as UOs, ainda será possível usar a Expressa e, na última página, desmarcar **Iniciar o processo de sincronização... ***. Em seguida, execute novamente o assistente de instalação e altere as UOs em [opções de configuração](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) e habilite a sincronização agendada.
- Convém habilitar um dos recursos no Azure AD Premium, como Write-back de Senha. Primeiro, use Expressa para concluir a instalação inicial. Em seguida, execute novamente o assistente de instalação e altere as [opções de configuração](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## Personalizado
<a id="custom" class="xliff"></a>
O caminho personalizado permite muito mais opções do que a Expressa. Ele deve ser usado em todos os casos nos quais a configuração descrita na seção anterior para Expressa não é representativa para sua organização.

Use quando:

- Você não tiver acesso a uma conta de administrador de empresa no Active Directory.
- Você tiver mais de uma floresta, ou planejar sincronizar mais de uma floresta no futuro.
- Houver domínios em sua floresta que não podem ser acessados pelo servidor do Connect.
- Você planejar usar a federação ou autenticação de passagem para entrada do usuário.
- Houver mais de 100.000 objetos e você precisar usar um SQL Server completo.
- Você planeja usar a filtragem baseada em grupo e não apenas a filtragem baseada em domínio ou unidade organizacional.

## Atualizar do DirSync
<a id="upgrade-from-dirsync" class="xliff"></a>
Se você estiver usando o DirSync, execute as etapas em [Atualizar do DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) para atualizar sua configuração existente. Há duas opções de atualização diferentes:

- Atualização in-loco para instalação do Connect no mesmo servidor.
- Implantação paralela para instalação do Connect em um novo servidor, enquanto o servidor DirSync existente ainda está em operação.

## Atualização do Azure AD Sync
<a id="upgrade-from-azure-ad-sync" class="xliff"></a>
Se você estiver usando o Azure AD Sync, poderá executar as [mesmas etapas](active-directory-aadconnect-upgrade-previous-version.md) usadas durante a atualização de uma versão do Connect para uma versão mais recente. Há duas opções de atualização diferentes:

- Atualização in-loco para instalação do Connect no mesmo servidor.
- Migração de balanço (Swing-migration) para instalação do Connect em um novo servidor, enquanto o servidor Azure AD Sync existente ainda está em operação.

## Migrar do FIM2010 ou MIM2016
<a id="migrate-from-fim2010-or-mim2016" class="xliff"></a>
Se você estiver usando o Forefront Identity Manager 2010 ou o Microsoft Identity Manager 2016 com o Azure AD Connector, sua única opção será uma migração. Execute as etapas descritas em [migração de balanço (swing-migration)](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). Nas etapas, substitua qualquer menção ao Azure AD Sync por FIM2010/MIM2016.

## Próximas etapas
<a id="next-steps" class="xliff"></a>
Dependendo da opção selecionada, use o índice à esquerda para localizar o artigo com as etapas detalhadas.


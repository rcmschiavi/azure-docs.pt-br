---
title: "Proxy do aplicativo do Azure AD – introdução à instalação do conector | Microsoft Docs"
description: Ative o Proxy de Aplicativo no Portal do Azure e instale os Conectores para o proxy reverso.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 8bfdc2f4b86e38d707c026365e3ffcce471d5367
ms.lasthandoff: 04/07/2017


---

# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Introdução ao Proxy de Aplicativo e instale o conector
Este artigo orienta você pelas etapas para habilitar o Proxy de Aplicativo do Microsoft Azure AD para seu diretório de nuvem no Azure AD.

Se você estiver familiarizado com o que o Proxy de Aplicativo pode ajudá-lo a fazer, saiba mais sobre [Como fornecer acesso remoto seguro a aplicativos locais](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Pré-requisitos de Proxy de aplicativo
Antes de habilitar e usar os serviços de Proxy de aplicativo, você precisa ter:

* Uma [assinatura premium ou básica](active-directory-editions.md) do Microsoft Azure AD e um diretório do Azure AD do qual você seja administrador global.
* Um servidor executando o Windows Server 2012 R2 ou Windows 8.1 ou posterior em que você possa instalar o Conector do Proxy de Aplicativo. O servidor envia solicitações aos serviços de Proxy de Aplicativo na nuvem e precisa de uma conexão HTTP ou HTTPS para os aplicativos que você está publicando.
  * Para logon único para seus aplicativos publicados, essa máquina deve ser unida ao mesmo domínio do AD que os aplicativos que você está publicando. Para obter mais informações, consulte [Logon único com o Proxy de Aplicativo](active-directory-application-proxy-sso-using-kcd.md)

Se a sua organização usa servidores proxy para se conectar à Internet, leia [Trabalhar com servidores proxy locais existentes](application-proxy-working-with-proxy-servers.md) para obter detalhes sobre como configurá-los.

## <a name="open-your-ports"></a>Abrir as portas

Se houver um firewall no caminho, verifique se o ele está aberto para que o Conector possa fazer solicitações HTTPS (TCP) para o Proxy de Aplicativo. O Conector usa essas portas com subdomínios que fazem parte dos domínios de alto nível: msappproxy.net e servicebus.windows.net. Não esqueça de abrir as seguintes portas para o tráfego de **saída** :

| Número da porta | Descrição |
| --- | --- |
| 80 |Habilite o tráfego HTTP de saída para a validação de segurança. |
| 443 |Habilite a autenticação de usuário no Azure AD (necessário somente para o processo de registro do conector)<br>Habilite as atualizações automáticas e a sequência de inicialização do conector<br>Habilite a comunicação entre o conector e o serviço do Azure para solicitações de entrada  |
| 10100–10120 |Habilitar respostas HTTP LOB enviadas de volta para o proxy |
| 9350 |Opcional, para permitir o melhor desempenho em solicitações de entrada. |
| 8080 |Habilite a sequência de inicialização do conector e a atualização automática do conector |
| 9090 |Habilite o registro do conector (necessário somente para o processo de registro do conector) |
| 9091 |Habilite a renovação automática do certificado de confiança do conector |

> [!IMPORTANT]
> A tabela reflete os requisitos de porta para a versão mais recente do conector. Se você ainda tiver um conector anterior à versão 1.5, você também precisará habilitar 9350, 9352 e 5671. Essas portas habilitam a comunicação entre os conectores mais antigos e o serviço do Azure para solicitações de entrada. 

Se o firewall reforça o tráfego de acordo com os usuários de origem, abra essas portas para o tráfego proveniente de serviços do Windows em execução como um serviço de rede. Além disso, certifique-se de habilitar a porta 8080 para Autoridade NT\Sistema.

Use a [Ferramenta de Teste de Portas do Conector de Proxy de Aplicativo do Azure AD](https://aadap-portcheck.connectorporttest.msappproxy.net/) para verificar se o conector pode alcançar o serviço Proxy de Aplicativo. No mínimo, verifique se a região EUA Central e a região mais próxima de você tem todas as marcas de seleção verdes. Além disso, um número maior de marcas de seleção verdes significa maior resiliência. 


## <a name="install-and-register-a-connector"></a>Instalar e registrar um conector
1. Entre como administrador no [Portal do Azure](https://portal.azure.com/).
2. O diretório atual é exibido sob seu nome de usuário no canto superior direito. Se você precisar alterar os diretórios, selecione esse ícone.
3. Vá para **Azure Active Directory** > **Proxy de Aplicativo**.

   ![Navegue até o Proxy de Aplicativo](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Selecione **Baixar o Conector**.

   ![Baixar o Conector](./media/active-directory-application-proxy-enable/download_connector.png)

4. Execute **AADApplicationProxyConnectorInstaller.exe** no servidor que você preparou de acordo com os pré-requisitos.
5. Siga as instruções no Assistente para instalar.
6. Durante a instalação, é solicitado que você registre o conector com o Proxy de Aplicativo do seu locatário do Azure AD.
   
   * Forneça suas credenciais de administrador global do AD do Azure. Seu locatário de administrador global pode ser diferente das suas credenciais do Microsoft Azure.
   * Verifique se o administrador que registra o Conector está no mesmo diretório onde você habilitou o serviço Proxy de Aplicativo. Por exemplo, se o domínio de locatário for contoso.com, o administrador deve ser admin@contoso.com ou qualquer outro alias nesse domínio.
   * Se a **Configuração de Segurança Aprimorada do Internet Explorer** estiver **Ativada** no servidor em que você estiver instalando o conector, a tela de registro poderá ser bloqueada. Siga as instruções na mensagem de erro para permitir o acesso. Certifique-se de que a Segurança Melhorada do Internet Explorer está desativada.
   * Se o registro do conector não for bem-sucedido, confira [Solucionar problemas de Proxy de Aplicativo](active-directory-application-proxy-troubleshoot.md).  
7. Quando a instalação for concluída, dois novos serviços são adicionados ao seu servidor:
   
   * **Conector de Proxy de Aplicativo do Microsoft AAD** habilita a conectividade
     
   * **Atualizador do Conector de Proxy de Aplicativo do Microsoft AAD** é um serviço de atualização automatizada que verifica periodicamente se há novas versões do conector e o atualiza conforme necessário.
     
   ![Serviços do Conector de Proxy de Aplicativo - captura de tela](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Para saber mais sobre conectores, veja [Noções básicas sobre conectores de proxy de aplicativo do Azure AD](application-proxy-understand-connectors.md). 

Para fins de alta disponibilidade, você deve implantar pelo menos dois conectores. Para implantar mais conectores, repita as etapas 2 e 3. Cada conector deve ser registrado separadamente.

Se você deseja desinstalar o Conector, desinstale o serviço Conector e o serviço Atualizador. Reinicie o computador para remover completamente o serviço.

## <a name="next-steps"></a>Próximas etapas
Agora você está pronto para [Publicar aplicativos com o Proxy de Aplicativo](application-proxy-publish-azure-portal.md).

Se tem aplicativos que estão em redes separadas ou em locais diferentes, você pode usar grupos de conector para organizar os conectores diferentes em unidades lógicas. Saiba mais sobre como [Trabalhar com conectores de Proxy de Aplicativo](active-directory-application-proxy-connectors-azure-portal.md).



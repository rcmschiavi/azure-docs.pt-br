---
title: "Tutorial: integração do Azure Active Directory com o LinkedIn Learning | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o LinkedIn Learning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: a61cf0ffce8bede930744d445df88dde22061a59
ms.contentlocale: pt-br
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: integração do Azure Active Directory com o LinkedIn Learning

Neste tutorial, você aprenderá a integrar o LinkedIn Learning ao Azure AD (Azure Active Directory).

A integração do LinkedIn Learning com o Azure AD oferece os seguintes benefícios:

- É possível controlar, no Azure AD, quem tem acesso ao LinkedIn Learning
- É possível permitir que seus usuários façam logon automaticamente no LinkedIn Learning (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um único local - o portal de Gerenciamento do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o LinkedIn Learning, serão necessários os seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do LinkedIn Learning habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você poderá obter uma avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o LinkedIn Learning da galeria
2. Configurar e testar o logon único do AD do Azure

## <a name="adding-linkedin-learning-from-the-gallery"></a>Adicionando o LinkedIn Learning da galeria
Para configurar a integração do LinkedIn Learning com o Azure AD, é necessário adicionar o LinkedIn Learning da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o LinkedIn Learning da galeria, siga as etapas abaixo:**

1. No **[Portal de Gerenciamento do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![Aplicativos][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicativos][3]

4. Na caixa de pesquisa, digite **LinkedIn Learning**. No painel de resultados, clique em **LinkedIn Learning** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o LinkedIn Learning, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do LinkedIn Learning é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do LinkedIn Learning.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no LinkedIn Learning.

Para configurar e testar o logon único do Azure AD com o LinkedIn Learning, é necessário concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criando um usuário de teste do LinkedIn Learning](#creating-a-linkedin-learning-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** : para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal de Gerenciamento do Azure e configurará o logon único em seu aplicativo LinkedIn Learning.

**Para configurar o logon único do Azure AD com o LinkedIn Learning, siga as etapas abaixo:**

1. No portal de Gerenciamento do Azure, na página de integração de aplicativos do **LinkedIn Learning**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, como **Modo**, selecione **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. Em uma janela diferente do navegador da Web, faça logon em seu locatário do LinkedIn Learning como administrador.

4. Em **Centro de Contas**, clique em **Configurações Globais** em **Configurações**. Além disso, selecione **Learning – Padrão** na lista suspensa.

    ![Configurar o logon único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Clique em **OU Clique aqui para carregar e copiar campos individuais do formulário** e copie a **ID da entidade** e **URL ACS (acesso do consumidor de declaração)**

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. No Portal do Azure, em **URLs e domínio do LinkedIn Learning**, siga as etapas abaixo para configurar o SSO no modo **iniciado pelo IdP**

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. Na caixa de texto **Identificador**, insira a **ID da Entidade** copiada do Portal do LinkedIn 

    b. Na caixa de texto **URL de resposta**, insira a **URL ACS (acesso do consumidor de declaração)** copiada do Portal do LinkedIn

7. Se você desejar configurar o SSO em **Iniciado pelo SP**, clique na opção Mostrar configurações avançadas de URL na seção de configuração e configure o logon na URL com o seguinte padrão:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. Seu aplicativo LinkedIn Learning espera as asserções SAML em um formato específico, o que exige que você adicione mapeamentos de atributos personalizados à sua configuração de atributos de token SAML. A captura de tela a seguir mostra um exemplo disso. O valor padrão do **identificador de usuário** é **user.userprincipalname**, mas o LinkedIn Learning espera que isso seja mapeado com o endereço de email do usuário. Para que você possa usar o atributo **user. mail** na lista ou usar o valor do atributo apropriado com base na configuração da sua organização. 

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. Na seção **Atributos de Usuário**, clique em **Exibir e editar todos os outros atributos de usuário** e defina os atributos. É necessário adicionar outra declaração denominada **departamento** e o valor deve ser mapeado para **user.department**.

    | Nome do atributo | Valor do atributo |
    | --- | --- |    
    | department| user.department |

   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)

   a. Clique em Adicionar atributo para abrir a página de detalhes do atributo. Adicione o atributo department conforme mostrado abaixo.

   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinLearning-tutorial/adduserattribute.png)
   
   b. Clique em **Ok** para salvar o atributo.

10. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo XML em seu computador.

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

11. Clique em **Salvar**.

    ![Configurar o logon único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

12. Vá para a seção **Configurações de administração do LinkedIn**. Carregue o arquivo XML que você acabou de baixar no portal do Azure clicando na opção Carregar arquivo XML.

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

13. Clique em **Ativar** para habilitar o SSO. O status do SSO será alterado de **Não conectado** para **Conectado**

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal de Gerenciamento do Azure chamado Britta Simon.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal de Gerenciamento do Azure**, no painel navegação à esquerda, clique em **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. Vá para **usuários e grupos** e clique em **todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo clique **adicionar** para abrir o **usuário** caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**. 

### <a name="creating-a-linkedin-learning-test-user"></a>Criando um usuário de teste do LinkedIn Learning

O aplicativo Linked Learning dá suporte ao provisionamento de usuário just-in-time e, após a autenticação, os usuários serão automaticamente criados no aplicativo. Na página de configurações de administração no portal do LinkedIn Learning, coloque a opção **Atribuir licenças automaticamente** como ativa para habilitar o provisionamento just-in-time e isso também atribuirá uma licença ao usuário.
   
   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao LinkedIn Learning.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao LinkedIn Learning, siga as etapas abaixo:**

1. No portal de gerenciamento do Azure, abra a exibição de aplicativos e, em seguida, navegue até o modo de exibição de diretório e vá para **aplicativos empresariais** e clique em **todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **LinkedIn Learning**.

    ![Configurar Logon Único](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco LinkedIn Learning no Painel de Acesso, você deve obter a página de logon do Azure e, após o logon bem-sucedido, você deve entrar em seu aplicativo LinkedIn Learning.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png


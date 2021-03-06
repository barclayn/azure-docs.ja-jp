---
title: チュートリアル:Azure Active Directory と Communifire の統合 | Microsoft Docs
description: Azure Active Directory と Communifire の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: de2a164d-2115-43e7-a9ed-e54f483f4aeb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: 52aef32eefe6f4f1f4272986a7de302dafd103aa
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160612"
---
# <a name="tutorial-azure-active-directory-integration-with-communifire"></a>チュートリアル:Azure Active Directory と Communifire の統合

このチュートリアルでは、Communifire と Azure Active Directory (Azure AD) を統合する方法について説明します。

Communifire と Azure AD を統合すると、次の利点があります。

- Communifire にどの Azure AD ユーザーがアクセスできるかを管理できます。
- ユーザーが自分の Azure AD アカウントで自動的に Communifire にサインオン (シングル サインオン) できるようになります。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Communifire と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Communifire でのシングル サインオンが有効に設定されたサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[1 か月の評価版を入手できます](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーから Communifire を追加する
1. Azure AD シングル サインオンの構成とテスト

## <a name="adding-communifire-from-the-gallery"></a>ギャラリーから Communifire を追加する
Azure AD と Communifire の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Communifire を追加する必要があります。

**ギャラリーから Communifire を追加するには、次の手順を実行します。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Azure Active Directory のボタン][1]

1. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[エンタープライズ アプリケーション] ブレード][2]
    
1. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[新しいアプリケーション] ボタン][3]

1. 検索ボックスに「**Communifire**」と入力し、結果パネルで **Communifire** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![結果一覧の Communifire](./media/communifire-tutorial/tutorial_communifire_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト

このセクションでは、"Britta Simon" というテスト ユーザーを使用して、Communifire で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンが機能するには、Azure AD ユーザーに対応する Communifire ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Communifire の関連ユーザーの間で、リンク関係が確立されている必要があります。

Communifire で、Azure AD の **[ユーザー名]** の値を **[ユーザー名]** の値として割り当ててリンク関係を確立します。

Communifire で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configure-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
1. **[Communifire テスト ユーザーの作成](#create-a-communifire-test-user)** - Azure AD の Britta Simon にリンクさせるために、対応するユーザーを Communifire で作成します。
1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - Britta Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[シングル サインオンのテスト](#test-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure ポータルで Azure AD のシングル サインオンを有効にして、Communifire アプリケーションでシングル サインオンを構成します。

**Communifire で Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure ポータルの **Communifire** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![シングル サインオン構成のリンク][4]

1. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[シングル サインオン] ダイアログ ボックス](./media/communifire-tutorial/tutorial_communifire_samlbase.png)

1. アプリケーションを IDP 開始モードで構成する場合は、**[Communifire Domain and URLs]\(Communifire のドメインと URL\)** セクションで次の手順を実行します。

    ![[Communifire Domain and URLs]\(Communifire のドメインと URL\) のシングル サインオン情報](./media/communifire-tutorial/tutorial_communifire_url.png)

    a. **[識別子]** ボックスに、`https://<subdomain>.communifire.com` の形式で URL を入力します。

    b. **[応答 URL]** ボックスに、`https://<subdomain>.communifire.com/SAML/AssertionConsumerService.aspx` のパターンを使用して URL を入力します。

1. アプリケーションを **SP** 開始モードで構成する場合は、**[詳細な URL 設定の表示]** チェックボックスをオンにして次の手順を実行します。

    ![[Communifire Domain and URLs]\(Communifire のドメインと URL\) のシングル サインオン情報](./media/communifire-tutorial/tutorial_communifire_url1.png)

    **[サインオン URL]** ボックスに、`https://<subdomain>.communifire.com/login` のパターンを使用して URL を入力します。
     
    > [!NOTE] 
    > これらは実際の値ではありません。 実際の識別子、応答 URL、サインオン URL でこれらの値を更新します。 これらの値を取得するには、[Communifire クライアント サポート チーム](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome)に問い合わせてください。 

1. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![証明書のダウンロードのリンク](./media/communifire-tutorial/tutorial_communifire_certificate.png) 

1.  **[証明書署名の設定詳細を表示する]** をオンにし、**[署名オプション]** で **[SAML 応答とアサーションへの署名]** を選択します。

    ![証明書のオプション](./media/communifire-tutorial/tutorial_communifire_certificateoption.png) 

1. **[保存]** ボタンをクリックします。

    ![[シングル サインオンの構成] の [保存] ボタン](./media/communifire-tutorial/tutorial_general_400.png)
    
1. **Communifire** 側のシングル サインオンを構成するには、ダウンロードした**メタデータ XML** を [Communifire サポート チーム](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 埋め込みドキュメント機能の詳細については、[Azure AD の埋め込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関するページを参照してください。

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

   ![Azure AD のテスト ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. Azure Portal の左側のウィンドウで、**Azure Active Directory** のボタンをクリックします。

    ![Azure Active Directory のボタン](./media/communifire-tutorial/create_aaduser_01.png)

1. ユーザーの一覧を表示するには、**[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックします。

    ![[ユーザーとグループ] と [すべてのユーザー] リンク](./media/communifire-tutorial/create_aaduser_02.png)

1. **[ユーザー]** ダイアログ ボックスを開くには、**[すべてのユーザー]** ダイアログ ボックスの上部にある **[追加]** をクリックしてきます。

    ![[追加] ボタン](./media/communifire-tutorial/create_aaduser_03.png)

1. **[ユーザー]** ダイアログ ボックスで、次の手順に従います。

    ![[ユーザー] ダイアログ ボックス](./media/communifire-tutorial/create_aaduser_04.png)

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに、ユーザーである Britta Simon の電子メール アドレスを入力します。

    c. **[パスワードを表示]** チェック ボックスをオンにし、**[パスワード]** ボックスに表示された値を書き留めます。

    d. **Create** をクリックしてください。
 
### <a name="create-a-communifire-test-user"></a>Communifire テスト ユーザーの作成

このセクションの目的は、Communifire で Britta Simon というユーザーを作成することです。 Communifire では、Just-In-Time プロビジョニングがサポートされており、この設定は、既定で有効になっています。 Communifire にまだアクセスしていない場合は、アクセスの試行中に、プロファイルの詳細を保存した後、新しいユーザーが作成されます。

>[!Note]
>ユーザーを手動で作成する必要がある場合は、 [Communifire のサポート チーム](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome)にお問い合わせください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Communifire へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザー ロールを割り当てる][200] 

**Communifire に Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

1. アプリケーションの一覧で **[Communifire]** を選択します。

    ![アプリケーションの一覧の Communifire のリンク](./media/communifire-tutorial/tutorial_communifire_app.png)  

1. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![[ユーザーとグループ] リンク][202]

1. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![[割り当ての追加] ウィンドウ][203]

1. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

1. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

1. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="test-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで [Communifire] タイルをクリックすると、自動的に Communifire アプリケーションにサインオンします。
アクセス パネルの詳細については、[アクセス パネルの概要](../user-help/active-directory-saas-access-panel-introduction.md)に関するページを参照してください。 

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/communifire-tutorial/tutorial_general_01.png
[2]: ./media/communifire-tutorial/tutorial_general_02.png
[3]: ./media/communifire-tutorial/tutorial_general_03.png
[4]: ./media/communifire-tutorial/tutorial_general_04.png

[100]: ./media/communifire-tutorial/tutorial_general_100.png

[200]: ./media/communifire-tutorial/tutorial_general_200.png
[201]: ./media/communifire-tutorial/tutorial_general_201.png
[202]: ./media/communifire-tutorial/tutorial_general_202.png
[203]: ./media/communifire-tutorial/tutorial_general_203.png


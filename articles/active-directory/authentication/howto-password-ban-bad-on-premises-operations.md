---
title: Azure AD パスワード保護プレビューの操作とレポート
description: Azure AD パスワード保護プレビューのデプロイ後の操作とレポート
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: a77a6dd8b408fd8151cb12b7d0269b8890ef929b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662416"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>更新:Azure AD パスワード保護の操作手順

|     |
| --- |
| Azure AD パスワード保護は、Azure Active Directory のパブリック プレビュー機能です。 詳細については、「[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)」を参照してください。|
|     |

[Azure AD パスワード保護のインストール](howto-password-ban-bad-on-premises-deploy.md)をオンプレミスで完了した後、Azure portal で構成する必要があるいくつかの項目があります。

## <a name="configure-the-custom-banned-password-list"></a>カスタムの禁止パスワード リストを構成する

組織の禁止パスワード リストをカスタマイズする手順については、「[カスタムの禁止パスワード リストを構成する](howto-password-ban-bad-configure.md)」の記事のガイダンスに従います。

## <a name="enable-password-protection"></a>パスワード保護を有効にする

1. [Azure portal](https://portal.azure.com) にサインインし、**[Azure Active Directory]**、**[認証方法]**、**パスワード保護 (プレビュー)** の順に選択します。
1. **[Windows Server Active Directory のパスワード保護を有効にする]** を **[はい]** に設定します
1. [デプロイ ガイド](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy)のページで説明しているように、最初に **[モード]** を **[監査]** に設定することをお勧めします
   * 機能に問題がなければ、**[モード]** を **[強制]** に切り替えることができます
1. **[保存]**

![Azure portal で Azure AD パスワード保護コンポーネントを有効にする](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>監査モード

監査モードは、"what-if" モードでソフトウェアを実行する方法として用意されています。 各 DC エージェント サービスは、受信したパスワードを現在アクティブなポリシーに従って評価します。 現在のポリシーが監査モードに構成されている場合、"不正な" パスワードは、イベント ログ メッセージが生成される原因となりますが受け入れられます。 これは監査モードと強制モードの唯一の違いであり、他のすべての操作は同じように実行されます。

> [!NOTE]
> 初期のデプロイとテストは常に監査モードで開始することをお勧めします。 イベント ログのイベントを監視して、強制モードが有効になった後で既存の運用プロセスが影響を受けるかどうかを予想してみる必要があります。

## <a name="enforce-mode"></a>強制モード

強制モードは最終構成として用意されています。 前述の監査モードと同じように、各 DC エージェント サービスは、受信したパスワードを現在アクティブなポリシーに従って評価します。 ただし、強制モードが有効になっている場合、ポリシーに従って安全でないと見なされるパスワードは拒否されます。

強制モードで Azure AD パスワード保護 DC エージェントによってパスワードが拒否された場合、エンド ユーザーの目に見える影響は、自分のパスワードが従来のオンプレミス パスワードの複雑さの適用によって拒否された場合と同じです。 たとえば、Windows ログオン\パスワードの変更画面で、次の従来のエラー メッセージがユーザーに表示されることがあります。

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

このメッセージは、いくつかの考えられる結果の一例にすぎません。 具体的なエラー メッセージは、実際のソフトウェアや、安全でないパスワードを設定しようとするシナリオによって異なる可能性があります。

影響を受けるエンド ユーザーは、IT スタッフと協力して、新しい要件を理解し、セキュリティで保護されたパスワードを選択できるようになる必要があります。

## <a name="enable-mode"></a>有効モード

通常、この設定は既定の有効 (はい) 状態のままにしておく必要があります。 この設定を無効 (いいえ) に設定すると、デプロイされているすべての Azure AD パスワード保護 DC エージェントは休止モードになります。このモードでは、すべてのパスワードがそのまま受け入れられ、検証アクティビティはまったく実行されません (たとえば、監査イベントでさえ実行されません)。

## <a name="next-steps"></a>次の手順

[Azure AD のパスワード保護について監視する](howto-password-ban-bad-on-premises-monitor.md)

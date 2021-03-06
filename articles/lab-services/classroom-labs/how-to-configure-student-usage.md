---
title: Azure Lab Services のクラスルーム ラボの使用設定を構成する | Microsoft Docs
description: ラボのユーザー数の構成、それらのユーザーのラボへの登録、ユーザーが VM を使用できる時間数の制御などの方法について説明します。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 946a2a05cee0cf8f3b91eef58442fbb2e26935c4
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490449"
---
# <a name="configure-usage-settings-and-policies"></a>使用設定とポリシーを構成する
この記事では、ラボへのユーザーの追加、それらのユーザーのラボへの登録、ユーザーが VM を使用できる時間数の制御などの方法について説明します。 


## <a name="add-users-to-the-lab"></a>ラボへのユーザーの追加
**[アクセスを制限する]** を有効にしている場合は、ユーザー (メール アドレス) をリストに追加します。

1. 左側のメニューの **[ユーザー]** を選択します。
2. ツール バーの **[ユーザーの追加]** を選択します。 
3. **[ユーザーの追加]** ページで、ユーザーのメール アドレスを入力します。複数のメール アドレスは別々の行に入力するか、1 つの行にセミコロンで区切って入力します。 

    ![ユーザーのメール アドレスを追加](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **[保存]** を選択します。 ユーザーのメール アドレスとそのステータス (登録または未登録) がリストに表示されます。 

    ![ユーザー リスト](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="send-registration-link-to-students"></a>登録リンクを学生に送信する
次の手順では、ユーザーに登録リンクを送信する手順を示します。 ラボの **[アクセスを制限する]** が有効になっている場合、登録リンクを使用してラボに登録できるのは、ユーザーの一覧に含まれるユーザーのみです。 

1. 左側のメニューの **[Users]\(ユーザー\)** を選択して **[Users]\(ユーザー\)** ビューに切り替えます。 
2. **[Get registration link]\(登録リンクの取得\)** タイルを選択します。

    ![学生登録リンク](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **[User registration]\(ユーザー登録\)** ダイアログ ボックスで、**[コピー]** ボタンを選びます。 リンクがクリップボードにコピーされます。 それを電子メール エディターに貼り付け、学生に電子メールを送信します。 

    ![学生登録リンク](../media/tutorial-setup-classroom-lab/registration-link.png)
2. **[User registration]\(ユーザー登録\)** ダイアログ ボックスの **[閉じる]** を選択します。 
4. 学生がクラスに登録できるように、登録リンクを学生と共有します。 **制限オプション**設定が有効になっていて、なおかつ一連のユーザーがリストに存在する場合は、次の操作を行います。
    1. ユーザーの**メール アドレス**をリストで選択します。 
    2. 既定のメール プログラムのウィンドウが、**宛先**アドレスが入力された状態で表示されます。 
    3. 先ほどコピーした**登録 URL** を貼り付けます。 
    4. **メール**を送信します。 

## <a name="view-users-registered-with-the-lab"></a>ラボに登録されているユーザーを確認する

左側のメニューで **[ユーザー]** を選択して、ラボに登録されているユーザーの一覧を表示します。 

![ラボに登録されているユーザーの一覧](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>ユーザーごとのクォータを設定する
次の手順を使用して、ユーザーごとのクォータを設定できます。 

1. 左側のメニューの **[ユーザー]** を選択します。
2. ツールバーの **[Quota per user: unlimited]\(ユーザーあたりのクォータ: 無制限\)** を選択します。 
3. **[Quota per user]\(ユーザーあたりのクォータ\)** ページで **[Limit the number of hours a user can use a VM]\(ユーザーが VM を使用できる時間数を制限する)** を選択します。 
4. **[How many hours do you want to give to each user]\(各ユーザーに許可する時間数)** に時間数を入力し、**[Save]\(保存\)** を選択します。 

    ![ユーザーあたりの時間数](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. 次のように、ツール バーに時間数が表示されるようになりました: **[Quota per user: &lt;number of hours&gt;]\(ユーザーあたりのクォータ: <時間数>\)**。 

    ![ユーザーあたりのクォータ](../media/how-to-configure-student-usage/quota-per-user.png)

> [!IMPORTANT]
> [スケジュールされている VM の実行時間](how-to-create-schedules.md)は、ユーザーに割り当てられるクォータにカウントされません。 クォータは、学生が VM で消費することをスケジュールされている時間以外の時間です。 

### <a name="add-users-by-uploading-a-csv-file"></a>CSV ファイルをアップロードしてユーザーを追加する
また、ユーザーのメール アドレスを指定した CSV ファイルをアップロードしてユーザーを追加することもできます。

1. ツールバーの **[CSV のアップロード]** を選択します。
2. ユーザーのメール アドレスを指定した CSV ファイルを選択します。 すべてのメール アドレスは、Excel で開いたときに 1 列に収まるようにします。 

## <a name="manage-user-vms"></a>ユーザー VM を管理する
提供された登録リンクを使用して学生が Azure Lab Services に登録すると、**[仮想マシン]** タブに学生に割り当てられた VM が表示されます。 

![学生に割り当てられた仮想マシン](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

学生の VM 上では、次のタスクを実行できます。 

- VM が実行中の場合は、VM を停止します。 
- VM が停止している場合は、VM を起動します。 
- VM に接続します 
- VM を削除します。 
- ユーザーが仮想マシンを使用した時間数を表示します。 

## <a name="update-number-of-virtual-machines-in-lab"></a>ラボ内の仮想マシンの数を更新する
ラボ内の仮想マシンの数を更新するには、**[仮想マシン]** ページで次の手順を実行します。

1. 左側のメニューで **[仮想マシン]** を選択します。 
2. ツール バーの **[Lab capacity: &lt;number&gt; machine(s)]\(ラボの容量: <数> 台のマシン\)** を選択します。 
3. 仮想マシンの**数**を入力します。
4. **[保存]** を選択します。

    ![ラボ内の仮想マシン](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>次の手順
次の記事を参照してください。

- [管理者としてラボ アカウントを作成および管理する](how-to-manage-lab-accounts.md)
- [ラボ所有者としてラボを作成および管理する](how-to-manage-classroom-labs.md)
- [ラボ所有者としてテンプレートを設定および発行する](how-to-create-manage-template.md)
- [ラボ ユーザーとしてクラスルーム ラボにアクセスする](how-to-use-classroom-lab.md)

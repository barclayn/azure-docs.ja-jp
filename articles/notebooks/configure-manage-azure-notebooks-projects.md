---
title: Azure Notebook プロジェクトを構成および管理する
description: Azure Notebooks UI とターミナル直接アクセスの両方を使用し、プロジェクト メタデータ、プロジェクト ファイル、プロジェクトの環境を管理して、セットアップする方法。
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 35dd6ff1-a14a-4a2e-b173-6d8467de3e89
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: kraigb
ms.openlocfilehash: 54b211584b170d6e2ee0bcaa6c80bcaed376814f
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904371"
---
# <a name="manage-and-configure-projects"></a>プロジェクトの管理と構成

Azure Notebooks のプロジェクトは、基本的には、Jupyter ノートブックが実行される基になる Linux 仮想マシンと、ファイル フォルダーおよび記述メタデータで構成されます。 Azure Notebooks のプロジェクト ダッシュボードを使用すると、ファイルを管理できます。また、プロジェクトの特性を構成することもできます。

- プロジェクトで実行するコンピューティング レベル。Free レベルまたは Azure 仮想マシンを指定できます。
- プロジェクト メタデータ。これには、名前、説明、プロジェクトを共有するときに使用される識別子のほか、プロジェクトがパブリックかプライベートかも含まれています。
- プロジェクトのノートブック、データ、およびその他のファイル。他のファイル システムと同じように管理します。
- プロジェクトの環境。スタートアップ スクリプトを使用するか、ターミナルから直接管理します。
- ログ。ターミナルからアクセスします。

> [!Note]
> ここで説明されている管理および構成機能は、プロジェクトを最初に作成したプロジェクト所有者のみ使用できます。 ただし、独自のアカウントにプロジェクトを複製することができます。この場合、所有者になり、必要に応じて、プロジェクトを構成することができます。

ノートブックまたはその他のファイルを実行するたびに、Azure Notebooks によって基になる仮想マシンが起動されます。 サーバーによって自動的にファイルが保存されます。サーバーは非アクティブな状態が 60 分間続くとシャットダウンしますが、 **シャットダウン** コマンド (キーボード ショートカット: h) でいつでも停止できます。

## <a name="compute-tier"></a>コンピューティング レベル

プロジェクト ダッシュ ボードの **[実行]** ドロップダウン リストでは、プロジェクトで実行するコンピューティング レベルを選択します。 既定で、プロジェクトは **Free コンピューティング** レベルで実行しますが、このレベルは不正使用を回避するために、4 GB のメモリと 1 GB のデータに制限されます。

![プロジェクト ダッシュ ボードのコンピューティング レベル ドロップダウン リスト](media/project-compute-tier-list.png)

これらの制限をバイパスするには、Azure サブスクリプションでプロビジョニングした別の仮想マシンを使用します。 その仮想マシン上で、JupyterHub をインストールして実行する必要があります。 Data Science Virtual Machine イメージ (オペレーティング システムは任意) は、既定で JupyterHub が含まれるため、適切な選択です。

適切に構成された Azure 仮想マシンを用意したら、ドロップダウン リストで **[Direct Compute]\(直接コンピューティング\)** オプションを選択します。すると、名前 (一覧に表示する名前)、VM の IP アドレスとポート (通常は 8000、JupyterHub がリッスンする既定のポート)、および VM 資格情報の入力を求められます。

![[Direct Compute]\(直接コンピューティング\) オプションのサーバー情報を収集するプロンプト](media/project-compute-tier-direct.png)

次の条件が当てはまる場合、ドロップダウン リストには、[Data Science Virtual Machine (DSVM)](/azure/machine-learning/data-science-virtual-machine) インスタンスも表示されます (これらのいずれかの条件を満たしていない場合でも、[Direct Compute]\(直接コンピューティング\) オプションを使用し、Azure portal から取得した値を入力して、DSVM に接続できます)。

- 会社のアカウントなどの Azure Active Directory (AAD) を使用するアカウントで、Azure Notebooks にサインインしている。
- アカウントが Azure サブスクリプションに接続されている。
- そのサブスクリプションに、少なくとも閲覧者アクセス権を持ち、Data Science Virtual Machine for Linux (Ubuntu) イメージを使用する 1 つ以上の仮想マシンが含まれている。

![プロジェクト ダッシュボードのドロップダウン リストの Data Science Virtual Machine インスタンス](media/project-compute-tier-dsvm.png)

DSVM インスタンスを選択すると、Azure Notebooks によって、VM を作成したときに使用した特定のマシン資格情報が求められることがあります。

新しい DSVM インスタンスを作成するには、[Ubuntu Data Science VM の作成](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)に関するページの指示に従います。 Azure Notebooks 内のドロップダウン リストに DSVM が表示されるようにする場合は、**Data Science Virtual Machine for Linux (Ubuntu)** イメージを使用します。  その他の理由で Windows または CentOS イメージを使用する必要がある場合は、**[Direct Compute]\(直接コンピューティング\)** オプションを使用して手動で DSVM に接続できます。

## <a name="edit-project-metadata"></a>プロジェクト メタデータを編集する

プロジェクト ダッシュボードで、**[プロジェクト設定]**、**[情報]** タブの順に選択します。このタブには、次の表に示すプロジェクトのメタデータが格納されています。 プロジェクト メタデータは、いつでも変更できます。

| Setting | 説明 |
| --- | --- |
| プロジェクト名 | Azure Notebooks が表示目的で使用する、ご自身のプロジェクトのフレンドリ名。 例: "Python の Hello World" |
| プロジェクト ID | プロジェクトの共有に使用するカスタム識別子。URL の一部として使用されます (形式: `https://notebooks.azure.com/<user_id>/projects/<project_id>`)。 この ID には文字、数字、およびハイフンのみを使用できます。また、30 文字に制限されています。 どのような ID を使用すればいいかよくわからない場合、一般的な規則としては、ご自身のプロジェクト名を小文字で表記し、スペース部分をハイフンで表したバージョンを使います。たとえば、プロジェクト名が "My Project Name" の場合、プロジェクト ID は "my-project-name" になります。 |
| パブリック プロジェクト | 設定すると、リンクを知っているすべてのユーザーがプロジェクトにアクセスできます。 プライベート プロジェクトを作成する場合、このオプションはオフにします。 |
| 複製を非表示にする | 設定すると、他のユーザーが、このプロジェクトに対して作成された複製の一覧を表示できません。 複製を非表示にする機能は、講義にノートブックを使っている場合など、同じ組織に所属していない人が共有先として多数存在するプロジェクトに便利です。 |

> [!Important]
>
> プロジェクト ID を変更すると、以前に共有した可能性があるプロジェクトへのリンクすべてが無効になります。

## <a name="manage-project-files"></a>プロジェクト ファイルを管理する

プロジェクト ダッシュボードには、プロジェクトのフォルダー システムの内容が表示されます。 さまざまなコマンドを使用して、これらのファイルを管理できます。

### <a name="create-new-files-and-folders"></a>新しいファイルとフォルダーの作成

**[+ 新規]** コマンド (キーボード ショートカット: n) を実行すると、新しいファイルまたはフォルダーが作成されます。 このコマンドを使用する場合は、作成する項目の種類を最初に選択します。

| 項目の種類 | 説明 | コマンドの動作 |
| --- | --- | --- |
| **ノートブック** | Jupyter Notebook | ノートブックのファイル名と言語を指定するためのポップアップを表示します。 |
| **フォルダー** | サブフォルダー | プロジェクトのファイル リストで、フォルダー名を入力する編集フィールドを作成します。 |
| **空のファイル** | テキスト、データなど、任意のコンテンツを格納できるファイル。 | プロジェクトのファイル リストで、ファイル名を入力する編集フィールドを作成します。 |
| **マークダウン** | マークダウン ファイル。 | プロジェクトのファイル リストで、ファイル名を入力する編集フィールドを作成します。 |

### <a name="upload-files"></a>ファイルのアップロード

**[アップロード]** コマンドにはデータをインポートするオプションが 2 つあり、インポート元として**URL** または**コンピューター**のいずれかを選択できます。 詳細については、「[Work with data files in Azure Notebook projects (Azure Notebook プロジェクトでデータ ファイルを操作する)](work-with-project-data-files.md)」を参照してください。

### <a name="select-file-specific-commands"></a>ファイル固有のコマンドの選択

プロジェクトのファイル リストの各項目を右クリックすると、コンテキスト メニューにコマンドが表示されます。

![ファイルのコンテキスト メニューのコマンド](media/project-file-commands.png)

| コマンド | キーボード ショートカット | Action |
| --- | --- | --- |
| ラン | r (またはクリック) | ノートブック ファイルを実行します。 他の種類のファイルは、閲覧のみの設定で表示されます。  |
| リンクのコピー | y | ファイルへのリンクをクリップボードにコピーします。 |
| Jupyter ラボで実行 | j | JupyterLab でノートブックを実行します。JupyterLab は、通常の Jupyter よりも開発者向けのインターフェイスが用意された開発環境です。 |
| プレビュー | p | ファイルの HTML プレビューを開きます。ノートブックの場合、プレビューは、ノートブックの読み取り専用レンダリングです。 詳細については、「[プレビュー](#preview)」を参照してください。 |
| ファイルの編集 | i | 編集のためにファイルを開きます。 |
| ダウンロード | d | ファイルまたはフォルダーのコンテンツを含む zip ファイルをダウンロードします。 |
| 名前の変更 | a | ファイルまたはフォルダーに付ける新しい名前を求めるメッセージが表示されます。 |
| 削除 | x | 削除の確認メッセージを表示した後、そのファイルをプロジェクトから完全に削除します。 削除を元に戻すことはできません。 |
| Move | m | 同じプロジェクト内の別のフォルダーにファイルを移動します。 |

#### <a name="preview"></a>プレビュー

ファイルまたはノートブックについては、読み取り専用ビューでコンテンツがプレビュー表示されます。ノートブック セルの実行は無効になっています。 プレビューは、ファイルまたはノートブックへのリンクを持っているが、Azure Notebooks にサインインしていないすべてのユーザーに表示されます。 一度サインインすると、そのユーザーは自分のアカウントにノートブックを複製したり、ローカル コンピューターにダウンロードしたりできます。

プレビュー ページでは、複数のツール バー コマンドとキーボード ショートカットを使用できます。

| コマンド | キーボード ショートカット | Action |
| --- | --- | --- |
| 共有 | s | リンクの取得、ソーシャル メディアでの共有、埋め込み用 HTML の取得、電子メールの送信など、共有操作を行うためのポップアップを表示します。 |
| 複製 | c  | ノートブックをご自身のアカウントに複製します。 |
| ラン | r | ノートブックを実行します (実行が許可されている場合)。 |
| ダウンロード | d | ノートブックのコピーをダウンロードします。 |

## <a name="configure-the-project-environment"></a>プロジェクトの環境を構成する

ご自身のノートブックが実行される基になる仮想マシンの環境を構成する方法は 3 つあります。

- 1 回限りの初期化スクリプトを含める
- プロジェクトの環境設定を使用して、セットアップ ステップを指定する
- ターミナル経由で仮想マシンにアクセスする。

仮想マシンが起動すると必ず、プロジェクト構成のすべての形式が適用されます。このため、プロジェクト内のすべてのノートブックが影響を受けます。

### <a name="one-time-initialization-script"></a>1 回限りの初期化スクリプト

Azure Notebooks は、プロジェクトに対してサーバーを初めて作成するときに、そのプロジェクトで *aznbsetup.sh* という名前のファイルを探し、このファイルが存在する場合は、それを実行します。 スクリプトの出力は、*.aznbsetup.log* としてプロジェクト フォルダーに格納されます。

### <a name="environment-setup-steps"></a>環境のセットアップ ステップ

プロジェクトの環境設定を使用すると、環境を構成するためのステップを個別に作成できます。

プロジェクト ダッシュボードで、**[プロジェクト設定]**、**[環境]** タブの順に選択します。ここで、そのプロジェクトのセットアップ ステップを追加、削除、および変更します。

![[環境] タブが選択されている [プロジェクト設定] ポップアップ](media/project-settings-environment-steps.png)

ステップを追加するには、最初に **[+ 追加]** を選択し、**[操作]** ドロップダウン リストでステップの種類を選択します。

![新しい環境セットアップ ステップの操作セレクター](media/project-settings-environment-details.png)

プロジェクトの情報は、選択した操作の種類によって異なります。

- **Requirements.txt**:2 番目のドロップダウン リストで、プロジェクトに既にある *requirements.txt* ファイルを選択します。 次に、表示される 3 番目のドロップダウン リストから Python バージョンを選択します。 *requirements.txt* ファイルを使用すると、Azure Notebooks によって、ノートブック サーバーの起動時に *requirements.txt* ファイルを指定して `pip install -r` が実行されます。 ノートブック自体からパッケージを明示的にインストールする必要はありません。

- **シェル スクリプト**:2 番目のドロップダウン リストで、プロジェクトの bash シェル スクリプト (通常は拡張子が *.sh* のファイル) を選択します。このスクリプトには、環境を初期化するときに実行するコマンドが格納されています。

- **Environment.yml**:2 番目のドロップダウン リストで、conda 環境を使用して、Python プロジェクト用の *environments.yml* ファイルを選択します。

ステップの追加が完了したら、**[保存]** を選択します。

### <a name="use-the-terminal"></a>ターミナルの使用

プロジェクト ダッシュボードで **[ターミナル]** コマンドを実行すると Linux ターミナルが開きます。これによりサーバーに直接アクセスすることができます。 ターミナル内では、データのダウンロード、ファイルの編集または管理、プロセスの検査のほか、vi、nano などのツールを使用することもできます。

> [!Note]
> お使いのプロジェクトの環境にスタートアップ スクリプトがあると、ターミナルを開いたときに、セットアップがまだ進行中であることを示すメッセージが表示される場合があります。

ターミナルでは標準の Linux コマンドを発行できます。 ホーム フォルダーで `ls` を使用して、仮想マシンに存在するさまざまな環境 (*anaconda2_501*、*anaconda3_420*、*anaconda3_501*、*IfSharp*、*R* など) や、そのプロジェクトを含む *project* フォルダーを表示することもできます。

![Azure Notebooks のプロジェクト ターミナル](media/project-terminal.png)

特定の環境に影響を及ぼすには、まず、その環境フォルダーにディレクトリを変更します。

Python 環境については、各環境の *bin* フォルダーに `pip` と `conda` があります。 環境用の組み込みエイリアスを使用することもできます。

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

サーバーに対する変更は、現在のセッションにのみ適用されます。ただし、*project* フォルダー自体に作成したファイルとフォルダーは除きます。 たとえば、プロジェクト フォルダー内のファイルを編集した場合、その編集はセッション間で保持されますが、`pip install` を含むパッケージは保持されません。

> [!Note]
> `python` または `python3` を使用する場合は、システムにインストールされたバージョンの Python を呼び出します。このバージョンはノートブックには使用されません。 `pip install` などの操作のアクセス許可もないため、必ずバージョン固有のエイリアスを使用してください。

## <a name="access-notebook-logs"></a>ノートブック ログにアクセスする

ノートブックを実行しているときに問題が発生すると、Jupyter からの出力が *.nb.log* という名前のフォルダーに格納されます。 これらのログにアクセスするには、**ターミナル** コマンドまたはプロジェクト ダッシュボードを使用します。

通常、Jupyter をローカルで実行している場合は、ターミナル ウィンドウから起動した可能性があります。 ターミナル ウィンドウには、カーネルの状態などの出力が表示されます。

ログを表示するには、ターミナルで次のコマンドを使用します。

```bash
cat .nb.log
```

Python ノートブックのコード セルからコマンドを使用することもできます。

```bash
!cat .nb.log
```

## <a name="next-steps"></a>次の手順

- [方法:プロジェクト データ ファイルを操作する](work-with-project-data-files.md)
- [ノートブックでクラウド データにアクセスする](access-data-resources-jupyter-notebooks.md)

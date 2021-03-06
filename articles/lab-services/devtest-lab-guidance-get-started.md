---
title: Azure DevTest Labs の使用を始める
description: この記事では、Azure DevTest Labs と 2 つの一般的な方針を使用して、組織でサービスの使用を開始するための主要なシナリオを提供します。
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 21837265a963829539f12cf7911191a61be43fbc
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243046"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Azure DevTest Labs の使用を始める
DevTest Labs の詳細を確認することにした場合、概念実証と規模拡大デプロイという 2 つの一般的な以後の方針があります。 

**規模拡大デプロイ**は、数百または数千人の開発者を抱える企業全体に DevTest Labs を展開することを意図した、数週間/数か月間にわたるレビューと計画から構成されます。 

**概念実証デプロイ**は、組織の価値を確立する単一のチームによる集約的な作業に焦点を置きます。 規模拡大デプロイにしようと思いがちですが、このアプローチは、概念実証オプションよりも失敗しがちになる傾向があります。 そのため、小規模で開始し、最初のチームから学習し、別の 2、3 のチームで同じアプローチを繰り返してから、得られた知識に基づいて規模拡大デプロイを計画することをお勧めします。 概念実証が成功したら、1 つまたは 2 つのチームを選択し、そのシナリオ (開発環境とテスト環境) を特定し、その現在のユース ケースを文書化して、DevTest Labs を展開することをお勧めします。 

## <a name="scenarios"></a>シナリオ
DevTest Labs 実装の主要なシナリオは 3 つあります。 

- 開発者デスクトップ
- テスト環境
- ラボ/トレーニング/ハッカソン

## <a name="developer-desktops"></a>開発者デスクトップ
開発者は、さまざまなプロジェクトの開発用コンピューターに対し異なる要件を抱えることがよくあります。 DevTest Labs では、開発者は、最も一般的なシナリオに合わせてあらかじめ構成されている、オンデマンドの VM にアクセスできます。 DevTest Labs には次のような利点があります。

- 組織は、一般的な開発環境およびテスト環境を用意することで、チーム間で一貫性を確保することができます
- 開発者は、必要に応じて開発用コンピューターをすばやくプロビジョニングできます
- 開発者は、サブスクリプション レベルのアクセス許可がなくても、セルフ サービス方式でリソースをプロビジョニングできます
- IT または管理者は、ネットワーク トポロジを事前に定義でき、開発者は、特別なアクセスを必要とすることなく、単純で直感的な方法で直接それを活用できます
- 開発者は、必要に応じて、開発用コンピューターを簡単にカスタマイズできます
- 管理者は、次のことが確実に行われるようにしてコストを管理できます。
    - 開発者は、開発に必要なものより多くの VM を取得できません
    - VM は、使われていないときはシャットダウンされます
    - 特定のワークロードにのみ VM インスタンス サイズのサブセットを許可します

## <a name="test-environments"></a>テスト環境
テスト環境を作成し、これを企業全体にわたって管理するには、多大な労力が必要になります。 DevTest Labs では、テスト環境を簡単に作成、更新、または複製できるので、各チームは必要に応じて、完全に構成された環境にアクセスできます。 このシナリオでは、DevTest Labs には次のような利点があります。

- テスト担当者は、再利用可能なテンプレートとアーティファクトを使って Windows と Linux の環境を迅速にプロビジョニングすることで、アプリケーションの最新バージョンをテストできます。
- テスト担当者は、複数のテスト エージェントをプロビジョニングすることで、ロード テストをスケールアップできます
- 管理者は、Azure DevOps にラボを接続して、DevOps のシナリオを有効にできます
- 管理者は、次のことが確実に行われるようにしてコストを管理できます。
    - テスト担当者は、必要なものより多くの VM を取得できません
    - VM は、使われていないときはシャットダウンされます
    - 特定のワークロードにのみ VM インスタンス サイズのサブセットを許可します

## <a name="labs-workshops-trainings-and-hackathons"></a>ラボ、ワークショップ、トレーニング、およびハッカソン  
Azure DevTest Labs のラボは、ワークショップ、ハンズオン ラボ、トレーニング、ハッカソンなどの一時的なアクティビティの優れたコンテナーとして機能します。 このサービスを使用すると、ラボを作成し、各受講者がトレーニング用に同じ分離環境を作成するために使用できるカスタム テンプレートを提供できます。 各受講者が必要としている場合にのみトレーニング環境を利用できるようにし、トレーニング環境にトレーニングに必要なリソース (仮想マシンなど) が十分に含まれるようにするポリシーを適用できます。 さらに、ワンクリックでアクセスできるラボを受講者と簡単に共有することもできます。 クラスが終了した後は、ラボおよび関連するすべてのリソースを簡単に削除できます。


## <a name="next-steps"></a>次の手順
このシリーズの次の記事をご覧ください。[DevTest Labs デプロイのスケールアップ](devtest-lab-guidance-scale.md)

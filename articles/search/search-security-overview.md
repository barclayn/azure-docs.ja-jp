---
title: セキュリティとデータ プライバシー - Azure Search
description: Azure Search は、SOC 2、HIPAA、およびその他の認定に準拠しています。 接続とデータの暗号化、認証、および ID は、Azure Search フィルターのユーザーとグループのセキュリティ識別子を介してアクセスします。
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 55558f1483a576e7ac3b9ce027588eceabd5db70
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311713"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Azure Search のセキュリティとデータ プライバシー

Azure Search には、包括的なセキュリティ機能とアクセスの制御が組み込まれており、その機能を利用して非公開のコンテンツが保持されます。 この記事では、Azure Search に組み込まれているセキュリティ機能と標準へのコンプライアンスの一覧を紹介します。

Azure Search のセキュリティ アーキテクチャには、物理的なセキュリティ、暗号化された伝送、暗号化されたストレージ、プラットフォーム全体の標準へのコンプライアンスが含まれます。 運用上、Azure Search は認証された要求のみを受け入れます。 必要に応じてセキュリティ フィルターを使用し、コンテンツに対してユーザーごとのアクセス制御を追加できます。 この記事では、各層のセキュリティについて触れますが、主に Azure Search でデータと操作がどのようにセキュリティ保護されるのかについて重点的に説明します。

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>標準へのコンプライアンス: ISO 27001、SOC 2、HIPAA

Azure Search は次の標準について認定され、[2018 年 6 月に発表されました](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/)。

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 Type 2 へのコンプライアンス](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) 完全なレポートについては、[Azure - Azure Government SOC 2 タイプ II レポート](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)を参照してください。 
+ [医療保険の携行性と責任に関する法律 (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Part 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS レベル 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)
+ [オーストラリアの IRAP Unclassified DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm)

標準へのコンプライアンスは、一般的に使用可能な機能に適用されます。 プレビュー機能は一般公開へ切り替える際に認定されます。厳格な標準の要件があるソリューションには使用しないでください。 コンプライアンスの認定については、「[Overview of Microsoft Azure compliance](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)」(Microsoft Azure コンプライアンスの概要) と[セキュリティ センター](https://www.microsoft.com/en-us/trustcenter)を参照してください。 

## <a name="encrypted-transmission-and-storage"></a>伝送とストレージの暗号化

暗号化は、接続から、伝送、Azure Search に格納されているインデックス付きデータまで、インデックス作成のパイプライン全体に拡張されています。

| セキュリティ レイヤー | 説明 |
|----------------|-------------|
| 転送中の暗号化 <br>(HTTPS/SSL/TLS) | Azure Search は HTTPS ポート 443 をリッスンします。 プラットフォーム全体で、Azure サービスへの接続が暗号化されます。 <br/><br/>クライアントからサービスへの Azure Search のすべての通信は、SSL/TLS 1.2 に対応しています。  サービスへの SSL 接続には TLSv1.2 を必ず使用してください。|
| 保存時の暗号化 | 暗号化はインデックス作成処理に完全に含まれ、完了までの時間やインデックス サイズにはほぼ影響しません。 完全に暗号化されていないインデックス (2018 年 1 月より前に作成されたインデックス) の増分更新を含め、すべてのインデックス作成で自動的に行われます。<br><br>内部的には、暗号化は [Azure Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) に基づいており、[256 ビットの AES 暗号化](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)を使用しています。|

暗号化は Azure Search の内部で行われ、Microsoft が内部的に管理する証明書と暗号化キーが使用され、汎用的に適用されます。 暗号化のオン/オフの切り替え、独自のキーの管理または代替、ポータル内またはプログラムによる暗号化設定の表示を行うことはできません。 

保存時の暗号化は、2018 年 1 月 24 日に発表され、すべてのリージョンで、共有 (無料) サービスを含むすべてのサービス層に適用されています。 完全な暗号化を行うには、この日付より前に作成されたインデックスを削除し、再構築する必要があります。 そうしないと、1 月 24 日の後に追加された新しいデータのみが暗号化されます。

## <a name="azure-wide-user-access-controls"></a>Azure 全体のユーザー アクセスの制御

いくつかのセキュリティ メカニズムが Azure 全体で利用できます。これらは当然、作成された Azure Search リソースでも利用できます。

+ [削除を防止するサブスクリプション レベルまたはリソース レベルのロック](../azure-resource-manager/resource-group-lock-resources.md)
+ [情報および管理操作に対するアクセスをコントロールするロールベースのアクセス制御 (RBAC)](../role-based-access-control/overview.md)

ロールベースのアクセス制御 (RBAC) はすべての Azure サービスでサポートされており、すべてのサービスで一貫してアクセスのレベルを設定できます。 たとえば、機微なデータ (管理者キーなど) の表示は所有者ロールと共同作成者ロールに制限されるのに対し、サービスの状態はすべてのロールのメンバーが表示できます。 RBAC には所有者、共同作成者、閲覧者のロールがあります。 既定では、すべてのサービス管理者が、所有者ロールのメンバーです。

## <a name="service-access-and-authentication"></a>サービス アクセスと認証

Azure Search は Azure プラットフォームのセキュリティ保護機能を継承しますが、独自のキーベースの認証も提供しています。 API キーは、ランダムに生成された数字と文字から成る文字列です。 キーの種類 (管理者またはクエリ) によって、アクセスのレベルが決まります。 有効なキーの送信は、要求が信頼されたエンティティのものであることの証明と見なされます。 検索サービスへのアクセスには 2 種類のキーが使用されます。

* 管理 (サービスに対するすべての読み取り/書き込み操作に有効)
* クエリ (インデックスに対するクエリなどの読み取り専用操作に有効)

管理キーは、サービスのプロビジョニング時に作成されます。 2 つの管理キーがあり、区別するために "*プライマリ*" と "*セカンダリ*" と呼ばれますが、これらは実際には交換可能です。 各サービスが 2 つの管理キーを持つため、サービスにアクセスしたままで一方のキーをロールオーバーできます。 どちらの管理キーも再生成できますが、管理キーの合計数は追加できません。 Search サービスごとに最大 2 個の管理キーがあります。

クエリ キーは必要に応じて作成され、Search を直接呼び出すクライアント アプリケーション向けに設計されています。 クエリ キーは最大 50 個まで作成できます。 アプリケーション コードでは、サービスへの読み取り専用アクセスを許可するために検索の URL とクエリ API キーを指定します。 また、アプリケーション コードでは、アプリケーションで使用されるインデックスも指定します。 エンドポイント、読み取り専用アクセスのための API キー、およびターゲット インデックスの組み合わせにより、クライアント アプリケーションからの接続のスコープとアクセス レベルが定義されます。

要求ごとに認証が必要です。この各要求は必須のキー、操作、およびオブジェクトで構成されています。 2 つのアクセス許可レベル (完全または読み取り専用) とコンテキスト (インデックスでのクエリ操作など) を組み合わせれば、サービス操作に対して全範囲のセキュリティを実現できます。 キーの詳細については、[API キーの作成と管理](search-security-api-keys.md)に関するページを参照してください。

## <a name="index-access"></a>インデックへのアクセス

Azure Search では、個別のインデックスはセキュリティ保護可能なオブジェクトでありません。 その代わり、インデックスへのアクセスは操作のコンテキストと共にサービス層 (読み取りまたは書き込みアクセス) で決定されます。

エンドユーザーのアクセスの場合、クエリ キーを使用して接続するようにクエリ要求を構成できます (これにより、すべての要求が読み取り専用になります)。また、アプリで使用される特定のインデックスを含めることができます。 クエリ要求では、インデックスの結合または複数のインデックスへの同時アクセスといった概念がないので、当然すべての要求のターゲットは単一のインデックスです。 そのため、クエリ要求自体 (キーと単一のターゲット インデックス) の構築でセキュリティ境界を定義します。

インデックスに対する管理者と開発者のアクセスは区別されていません。サービスによって管理されているオブジェクトの作成、削除、更新を行うには、どちらも書き込みアクセスが必要です。 サービスに対する管理者キーを持っている人はだれでも、そのサービスのインデックスの読み取り、変更、削除を行えます。 インデックスが誤って削除されたり、悪意によって削除されたりすることを防止するうえで、コード資産の社内ソース管理は、望ましくないインデックスの削除または変更を修復する対応策になります。 Azure Search は可用性を確保するためにクラスター内のフェールオーバーを備えていますが、インデックスの作成または読み込みに使用される専用コードを格納したり実行したりしません。

インデックス レベルでセキュリティ境界が必要なマルチテナントのソリューションでは、通常、そのソリューションに顧客がインデックス分離を処理するための中間層が含まれています。 マルチテナントのユース ケースの詳細については、「[マルチテナント SaaS アプリケーションと Azure Search の設計パターン](search-modeling-multitenant-saas-applications.md)」を参照してください。

## <a name="admin-access-from-client-apps"></a>クライアント アプリからの管理者アクセス

Azure Search 管理 REST API は、Azure リソース マネージャーの拡張機能であり、その依存関係を共有します。 そのため、Azure Search のサービス管理にとって、Active Directory は必須となります。 クライアント コードからのすべての管理要求は、Resource Manager に到達する前に、Azure Active Directory を使用して認証される必要があります。

インデックスの作成 (Azure Search サービス REST API) やドキュメントの検索 (Azure Search サービス REST API) など、Azure Search サービス エンドポイントに対するデータ要求では、要求ヘッダーで API キーが使用されます。

サービス管理操作と検索インデックスまたは検索ドキュメントに対するデータ操作をアプリケーション コードで処理する場合、2 つの認証アプローチ (Azure Search にネイティブなアクセス キーと Resource Manager に必要な Active Directory 認証方法) をコードに実装します。 

Azure Search での要求の構成について詳しくは、「[Azure Search Service REST (Azure Search サービス REST API)](https://docs.microsoft.com/rest/api/searchservice/)」を参照してください。 Resource Manager に関する認証要件の詳細については、「[サブスクリプションにアクセスするための Resource Manager 認証 API の使用](../azure-resource-manager/resource-manager-api-authentication.md)」を参照してください。

## <a name="user-access-to-index-content"></a>インデックス コンテンツへのユーザー アクセス

インデックスのコンテンツへのユーザーごとのアクセスは、クエリに対するセキュリティ フィルターを通じて実装され、特定のセキュリティ ID に関連付けられたドキュメントを返します。 事前定義されたロールとロールの割り当ては使用されず、ID に基づいてドキュメントとコンテンツの検索結果をトリミングするフィルターとして ID ベースのアクセス制御が実装されます。 次の表では、承認されていないコンテンツの検索結果をトリミングする 2 つのアプローチについて説明しています。

| アプローチ | 説明 |
|----------|-------------|
|[ID フィルターに基づいたセキュリティによるトリミング](search-security-trimming-for-azure-search.md)  | ユーザー ID アクセス制御を実装する基本的なワークフローについて記載しています。 また、インデックスへのセキュリティ ID の追加について取り上げているほか、そのフィールドに対してフィルター処理を行い、禁止されているコンテンツの結果をトリミングする方法について説明しています。 |
|[Azure Active Directory ID に基づいたセキュリティによるトリミング](search-security-trimming-for-azure-search-with-aad.md)  | この記事は前の記事を拡張したものであり、Azure クラウド プラットフォームの[無料サービス](https://azure.microsoft.com/free/)の 1 つである Azure Active Directory (AAD) から ID を取得する手順について説明しています。 |

## <a name="table-permissioned-operations"></a>表: 許可される操作

次の表では、Azure Search で許可される操作のほか、特定の操作へのアクセスを有効にするキーについてまとめています。

| Operation | アクセス許可 |
|-----------|-------------------------|
| サービスの作成 | Azure サブスクリプション所有者|
| サービスのスケーリング | 管理者キー、リソースに対する RBAC 所有者または RBAC 共同作成者  |
| サービスの削除 | 管理者キー、リソースに対する RBAC 所有者または RBAC 共同作成者 |
| サービス上のオブジェクトの作成、変更、削除: <br>インデックスとコンポーネント部分 (アナライザーの定義、スコアリング プロファイル、CORS オプションなど)、インデクサー、データ ソース、シノニム、サジェスター。 | 管理者キー、リソースに対する RBAC 所有者または RBAC 共同作成者  |
| インデックスのクエリ | 管理者キーまたはクエリ キー (RBAC は適用不可) |
| クエリ システム情報 (オブジェクトの統計、カウントおよび一覧を返す操作など) | 管理者キー、リソースに対する RBAC (所有者、共同作成者、閲覧者) |
| 管理者キーの管理 | 管理者キー、リソースに対する RBAC 所有者または RBAC 共同作成者。 |
| クエリ キーの管理 |  管理者キー、リソースに対する RBAC 所有者または RBAC 共同作成者。  |

## <a name="physical-security"></a>物理的なセキュリティ

業界最高レベルの物理的なセキュリティを提供する Microsoft データ センターは、標準と規制の広範なポートフォリオに準拠しています。 詳しくは、[グローバル データ センター](https://www.microsoft.com/cloud-platform/global-datacenters)に関するページに移動するか、データ センターのセキュリティに関する短いビデオをご覧ください。

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>関連項目

+ [.NET の作業開始 (管理者キーを使用したインデックスの作成のデモンストレーション)](search-create-index-dotnet.md)
+ [REST の作業開始 (管理者キーを使用したインデックスの作成のデモンストレーション)](search-create-index-rest-api.md)
+ [Azure Search フィルターを使用した ID ベースのアクセス制御](search-security-trimming-for-azure-search.md)
+ [Azure Search フィルターを使用した Active Directory の ID ベースのアクセス制御](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search のフィルター](search-filters.md)
---
title: フェールオーバー グループ - Azure SQL Database | Microsoft Docs
description: 自動フェールオーバー グループは SQL Database 機能の 1 つです。これを使用して、SQL Database サーバー上のデータベースのグループや、マネージド インスタンス内のすべてのデータベースのレプリケーションおよび自動/調整されたフェールオーバーを管理することができます。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: d24f7ce20a9dfb8ede184e8f013c2d988a8a96c2
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468701"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>自動フェールオーバー グループを使用して、複数のデータベースの透過的な調整されたフェールオーバーを有効にする

自動フェールオーバー グループは SQL Database 機能の 1 つです。これを使用して、SQL Database サーバー上のデータベースのグループや、(現在、マネージド インスタンスのパブリック プレビュー段階である) マネージド インスタンスのすべてのデータベースの別のリージョンへのレプリケーションおよびフェールオーバーを管理することができます。 [アクティブ geo レプリケーション](sql-database-active-geo-replication.md)と同じ基礎となるテクノロジが使用されます。 フェールオーバーは、手動で開始することも、ユーザー定義ポリシーに基づいて SQL Database サービスにデリゲートすることもできます。 後者のオプションの場合、プライマリ リージョンで発生した致命的な障害やその他の計画外のイベントにより SQL データベース サービスの可用性がすべてまたは一部失われたときに、セカンダリ リージョンの複数の関連データベースを自動的に復元できます。 さらに、読み取り可能なセカンダリ データベースを使用して、読み取り専用クエリ ワークロードをオフロードできます。 自動フェールオーバー グループには複数のデータベースが関与するため、これらのデータベースをプライマリ サーバーに構成する必要があります。 フェールオーバー グループ内にあるデータベース用のプライマリ サーバーとセカンダリ サーバーは両方とも、同一のサブスクリプションである必要があります。 自動フェールオーバー グループでは、グループ内のすべてのデータベースを別のリージョンの 1 つのセカンダリ サーバーにのみレプリケートできます。

> [!NOTE]
> SQL Database サーバー上のスタンドアロンまたはプールされたデータベースの操作時に、同じまたは異なるリージョンで複数のセカンダリが必要な場合は、[アクティブ geo レプリケーション](sql-database-active-geo-replication.md)を使用します。

自動フェールオーバー グループ ポリシーで自動フェールオーバー グループを使用しているときに、グループ内で 1 つ以上のデータベースに影響する機能停止が発生すると、自動フェールオーバーが行われます。 さらに、自動フェールオーバー グループには、フェールオーバー中にそのまま残る読み取り/書き込みリスナー エンドポイントと読み取り専用リスナー エンドポイントが用意されています。 手動または自動フェールオーバーのどちらをアクティブ化するにしても、フェールオーバーはグループ内のすべてのデータベースをプライマリに切り替えます。 データベースのフェールオーバーが完了した後、DNS レコードが自動的に更新され、エンドポイントが新しいリージョンにリダイレクトされます。 特定の RPO および RTO データについては、[ビジネス継続性の概要](sql-database-business-continuity.md)に関するページを参照してください。

自動フェールオーバー グループ ポリシーで自動フェールオーバー グループを使用しているときに、SQL Database サーバーまたはマネージド インスタンス内のデータベースに影響する機能停止が発生すると、自動フェールオーバーが行われます。 自動フェールオーバー グループは、以下を使用して管理できます。

- [Azure Portal](sql-database-implement-geo-distributed-database.md)
- [PowerShell:フェールオーバー グループ](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [REST API:フェールオーバー グループ](https://docs.microsoft.com/rest/api/sql/failovergroups)。

フェールオーバー後は、サーバーおよびデータベースの認証要件が新しいプライマリで構成されていることを確認してください。 詳細については、 [障害復旧後の SQL Database のセキュリティ](sql-database-geo-replication-security-config.md)に関するページを参照してください。

ビジネスの継続性を実現するにあたって、データセンター間のデータベース冗長性を追加することは、ソリューションのほんの一部です。 致命的な障害の後にアプリケーション (サービス) をエンド ツー エンドで復旧するには、そのサービスと依存しているサービスを構成するすべてのコンポーネントを復旧する必要があります。 このようなコンポーネントの例には、クライアント ソフトウェア (カスタム JavaScript が設定されたブラウザーなど)、Web フロント エンド、ストレージ、DNS などがあります。 すべてのコンポーネントが同じ障害に対する復元性を備えており、アプリケーションの目標復旧時間 (RTO) 以内に利用可能になることが重要です。 そのため、依存するサービスをすべて特定し、これらのサービスが提供する保証と機能について把握しておく必要があります。 そのうえで、依存するサービスのフェールオーバー中もサービスが確実に機能するように対策を講じる必要があります。 ディザスター リカバリーのソリューション設計の詳細については、[アクティブ geo レプリケーションを使用したディザスター リカバリー用のクラウド ソリューションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)に関するページを参照してください。

## <a name="auto-failover-group-terminology-and-capabilities"></a>自動フェールオーバー グループの用語と機能

- **フェールオーバー グループ**

  フェールオーバー グループは、プライマリ リージョンでの機能停止により、すべてまたは一部のプライマリ データベースが使用できなくなった場合に、1 つの単位として別のリージョンにフェールオーバーできる単一のマネージド インスタンス内の、または単一の SQL Database サーバーによって管理されるデータベースのグループです。

  - **SQL Database サーバー**

     SQL Database サーバーでは、単一の SQL Database サーバー上の一部またはすべてのユーザー データベースをフェールオーバー グループに配置することができます。 また、SQL Database サーバーでは、単一の SQL Database サーバー上の複数のフェールオーバー グループがサポートされます。

  - **マネージド インスタンス**
  
     マネージド インスタンスでは、フェールオーバー グループにマネージド インスタンスのすべてのユーザー データベースが含まれます。そのため、マネージド インスタンスでサポートされるのは、単一のフェールオーバー グループのみとなります。

- **プライマリ**

  フェールオーバー グループのプライマリ データベースをホストする SQL Database サーバーまたはマネージド インスタンス。

- **セカンダリ**

  フェールオーバー グループのセカンダリ データベースをホストする SQL Database サーバーまたはマネージド インスタンス。 セカンダリをプライマリと同じリージョンに配置することはできません。

- **SQL Database サーバー上のフェールオーバー グループへのデータベースの追加**

  同じ SQL Database サーバー上のエラスティック プール内の複数のデータベースまたは複数の単一データベースを、同じフェールオーバー グループに配置することができます。 フェールオーバー グループに単一データベースを追加すると、同じエディションとコンピューティング サイズを使用してセカンダリ データベースが自動的に作成されます。 プライマリ データベースがエラスティック プール内にある場合、セカンダリは同じ名前のエラスティック プールに自動的に作成されます。 セカンダリ サーバーにセカンダリ データベースが既に存在するデータベースを追加する場合、その geo レプリケーションがグループに継承されます。 フェールオーバー グループの一部でないサーバーにセカンダリ データベースが既に存在するデータベースを追加すると、セカンダリ サーバーに新しいセカンダリが作成されます。
  
> [!IMPORTANT]
  > マネージド インスタンスでは、すべてのユーザー データベースがレプリケートされます。 フェールオーバー グループで、レプリケーション対象としてユーザー データベースのサブセットを選択することはできません。

- **フェールオーバー グループの読み取り/書き込みリスナー**

  現在のプライマリの URL を指す、形成された DNS CNAME レコード。 これにより、フェールオーバー後にプライマリが変更されたときに、読み取り/書き込み SQL アプリケーションがプライマリ データベースに透過的に再接続できます。

  - **読み取り/書き込みリスナー用の SQL Database サーバーの DNS CNAME レコード**

     SQL Database サーバー上では、現在のプライマリの URL を指すフェールオーバー グループ用の DNS CNAME レコードは、`failover-group-name.database.windows.net` という形式になります。

  - **読み取り/書き込みリスナー用のマネージド インスタンスの DNS CNAME レコード**

     マネージド インスタンスでは、現在のプライマリの URL を指すフェールオーバー グループ用の DNS CNAME レコードは、`failover-group-name.zone_id.database.windows.net` という形式になります。

- **フェールオーバー グループの読み取り専用リスナー**

  セカンダリの URL を指す読み取り専用リスナーを指す、形成された DNS CNAME レコード。 これにより、指定された負荷分散規則を使用して、読み取り専用 SQL アプリケーションをセカンダリに透過的に接続できます。

  - **読み取り専用リスナー用の SQL Database サーバーの DNS CNAME レコード**

     SQL Database サーバー上では、セカンダリの URL を指す読み取り専用リスナー用の DNS CNAME レコードは、`failover-group-name.secondary.database.windows.net` という形式になります。

  - **読み取り専用リスナー用のマネージド インスタンスの DNS CNAME レコード**

     マネージド インスタンスでは、セカンダリの URL を指す読み取り専用リスナー用の DNS CNAME レコードは、`failover-group-name.zone_id.database.windows.net` という形式になります。

- **自動フェールオーバー ポリシー**

  既定では、フェールオーバー グループは自動フェールオーバー ポリシーを使用して構成されます。 SQL データベース サービスでは、エラーが検出され、猶予期間が終了した後、フェールオーバーがトリガーされます。 システムでは、影響の規模が原因で、組み込みの [SQL データベース サービスの高可用性インフラストラクチャ](sql-database-high-availability.md)によって機能停止を軽減できないかどうかを確認する必要があります。 アプリケーションからフェールオーバー ワークフローを制御するには、自動フェールオーバーをオフにします。

- **読み取り専用フェールオーバー ポリシー**

  既定では、読み取り専用リスナーのフェールオーバーは無効になっています。 セカンダリがオフラインのとき、プライマリのパフォーマンスに影響が出ないようにします。 ただし、セカンダリが回復するまで、読み取り専用セッションは接続できません。 読み取り専用セッションのダウンタイムを許容できず、プライマリのパフォーマンスが下がる可能性があっても、読み取り専用と読み取り/書き込みの両方のトラフィックに一時的にプライマリを使用しても良ければ、読み取り専用リスナーのフェールオーバーを有効にできます。 その場合、セカンダリが利用できないと、読み取り専用トラフィックがプライマリに自動的にリダイレクトされます。

- **計画されたフェールオーバー**

   計画されたフェールオーバーでは、セカンダリがプライマリ ロールに切り替わる前に、プライマリ データベースとセカンダリ データベース間の完全同期が行われます。 これにより、データ損失が発生しないことが保証されます。 計画されたフェールオーバーは、次のシナリオで使用されます。

  - データ損失が許容されない場合は、運用環境でディザスター リカバリー (DR) ドリルを行います
  - データベースを別のリージョンに再配置します
  - 機能停止が軽減 (フェールバック) された後、データベースをプライマリ リージョンに返します。

- **計画されていないフェールオーバー**

   計画されていないフェールオーバーや強制フェールオーバーでは、プライマリと同期せずに、セカンダリをすぐにプライマリ ロールに切り替えます。 この操作を行うとデータが失われます。 計画されていないフェールオーバーは、機能停止時においてプライマリにアクセスできない場合に回復手段として使用されます。 元のプライマリは、オンラインに戻ると、同期せずに自動的に再接続され、新しいセカンダリとなります。

- **手動フェールオーバー**

  自動フェールオーバー構成に関係なくいつでも手動でフェールオーバーを開始することができます。 自動フェールオーバー ポリシーが構成されていない場合、セカンダリへのフェールオーバー グループ内のデータベースの復元には手動フェールオーバーが必要です。 強制フェールオーバーまたはフレンドリ フェールオーバーを開始できます (データは完全に同期されます)。 後者は、プライマリをセカンダリ リージョンに再配置する場合に使用できます。 フェールオーバーが完了すると、確実に新しいプライマリに接続されるように、DNS レコードが自動的に更新されます

- **データ消失の猶予期間**

  プライマリ データベースとセカンダリ データベースは非同期レプリケーションを使用して同期されるため、フェールオーバーによりデータが消失する場合があります。 アプリケーションのデータ消失の許容範囲を反映するように、自動フェールオーバー ポリシーをカスタマイズできます。 **GracePeriodWithDataLossHours** を構成することで、データ消失につながる可能性があるフェールオーバーの開始までにシステムが待機する時間を制御できます。

- **複数のフェールオーバー グループ**

  サーバーの同じペアに対して複数のフェールオーバー グループを構成して、フェールオーバーのスケールを制御できます。 各グループは独立してフェールオーバーします。 マルチテナント アプリケーションがエラスティック プールを使用する場合、この機能を使用して各プールのプライマリ データベースとセカンダリ プライマリを混在させることができます。 こうすることで、機能停止の影響をテナントの半分に抑えることができます。

  > [!IMPORTANT]
  > マネージド インスタンスでは、複数のフェールオーバー グループはサポートされません。

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>単一データベースとエラスティック プールでフェールオーバー グループを使用する場合のベスト プラクティス

自動フェールオーバー グループはプライマリ SQL Database サーバーに構成する必要があり、それを別の Azure リージョンのセカンダリ SQL Database サーバーに接続します。  グループには、これらのサーバーのすべてまたは一部のデータベースを含めることができます。 次の図に、複数のデータベースと自動フェールオーバー グループを使用する、geo 冗長クラウド アプリケーションの一般的な構成を示します。

![自動フェールオーバー](./media/sql-database-auto-failover-group/auto-failover-group.png)

ビジネス継続性を考慮してサービスを設計する場合は、次の一般的なガイドラインに従います。

- **1 つまたは複数のフェールオーバー グループを使用して複数のデータベースのフェールオーバーを管理する**

  1 つまたは複数のフェールオーバー グループを異なるリージョンの 2 つのサーバー (プライマリおよびセカンダリ サーバー) 間に作成できます。 各グループには、プライマリ リージョンに発生した機能停止によりすべてまたは一部のプライマリ データベースが使用できなくなった際にユニットとして復元できる、1 つまたは複数のデータベースを含めることができます。 フェールオーバー グループは、プライマリと同じサービスの目的を共有する geo セカンダリ データベースを作成します。 既にある geo レプリケーションのリレーションシップをこのフェールオーバー グループに追加する場合は、その geo セカンダリが、プライマリと同じサービス レベルおよびコンピューティング サイズで構成されていることを確認してください。

- **OLTP ワークロードに読み取り/書き込みのリスナーを使用する**

  OLTP 操作を実行するときに、サーバー URL として `failover-group-name.database.windows.net` を使用すると、自動的にプライマリに接続されます。 この URL はフェールオーバー後にも変更されません。 フェールオーバーでは DNS レコードの更新が行われるため、クライアントの接続は、クライアント DNS キャッシュが最新の情報に更新された後にのみ新しいプライマリにリダイレクトされます。

- **読み取り専用ワークロードには読み取り専用リスナーを使用する**

  データがある程度古くても構わない、論理的に分離された読み取り専用ワークロードがある場合、アプリケーションでセカンダリ データベースを使用できます。 読み取り専用セッションでは、サーバー URL として `failover-group-name.secondary.database.windows.net` を使用すると、自動的にセカンダリに接続されます。 接続文字列に **ApplicationIntent=ReadOnly** を使用して、読み取りの意図を示すこともお勧めします。

- **パフォーマンスの低下に備える**

  SQL のフェールオーバーの決定は、アプリケーションの他の領域や、使用されている他のサービスとは無関係に行われます。 アプリケーションが、同じリージョン内のコンポーネントや別のリージョン内のコンポーネントと "混在" する場合があります。 パフォーマンスの低下を防ぐには、確実に DR リージョンで冗長アプリケーションをデプロイし、これらの[ネットワーク セキュリティ ガイドライン](#failover-groups-and-network-security)に従います。

  > [!NOTE]
  > DR リージョン内のアプリケーションでは、別の接続文字列を使用する必要はありません。  

- **データの損失に備える**

  機能停止が検出された場合は、**GracePeriodWithDataLossHours** で指定した期間、SQL が待機状態となります。 既定値は 1 時間です。 データの損失が許容できない場合は、**GracePeriodWithDataLossHours** に設定する値を十分大きく確保してください (24 時間など)。 セカンダリからプライマリにフェールバックするには、手動のグループ フェールオーバーを使用します。

> [!IMPORTANT]
> 800 以下の DTU と 250 を超えるデータベースを持ち、geo レプリケーションを使用するエラスティック プールでは、計画されたフェールオーバーに時間がかかったりパフォーマンスが低下したりする問題が発生することがあります。  こうした問題は、geo レプリケーション エンドポイントが地理的に広く分散している場合や、各データベースで複数のセカンダリ エンドポイントが使用されている場合に、書き込みの負荷が高いワークロードで発生しやすくなります。  このような症状は、geo レプリケーションのラグが時間の経過と共に増加するときに見られます。  このラグは、[sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) を使用して監視できます。  この問題が発生する場合は、プール DTU の数を増やしたり、同じプール内で geo レプリケーションされるデータベースの数を減らしたりするなどの緩和策があります。

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>マネージド インスタンスでフェールオーバー グループを使用する場合のベスト プラクティス

自動フェールオーバー グループはプライマリ インスタンスに構成する必要があり、それを別の Azure リージョンのセカンダリ インスタンスに接続します。  インスタンス内のすべてのデータベースは、セカンダリ インスタンスにレプリケートされます。 次の図に、マネージド インスタンスと自動フェールオーバー グループを使用する、geo 冗長クラウド アプリケーションの一般的な構成を示します。

![自動フェールオーバー](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!IMPORTANT]
> マネージド インスタンスの自動フェールオーバー グループは、パブリック プレビュー段階です。

アプリケーションでデータ層としてマネージド インスタンスを使用する場合は、ビジネス継続性を考慮して設計する際に以下の一般的なガイドラインに従います。

- **プライマリ インスタンスと同じ DNS ゾーンでセカンダリ インスタンスを作成する**

  新しいインスタンスが作成されると、一意の ID が DNS ゾーンとして自動的に生成され、インスタンスの DNS 名に含まれます。 このインスタンスのマルチドメイン (SAN) 証明書は、`zone_id.database.windows.net` という形式で、SAN フィールドでプロビジョニングされます。 この証明書は、同じ DNS ゾーン内のインスタンスへのクライアント接続を認証する際に使用できます。 フェールオーバー後にプライマリ インスタンスに中断することなく確実に接続するには、プライマリとセカンダリの両方のインスタンスが同じ DNS ゾーンにある必要があります。 アプリケーションを運用環境にデプロイする準備ができたら、別のリージョンでセカンダリ インスタンスを作成し、プライマリ インスタンスと DNS ゾーンを共有していることを確認します。 これを行うには、Azure ポータル、PowerShell、または REST API を使用し、省略可能な `DNS Zone Partner` パラメーターを指定します。

  プライマリ インスタンスと同じ DNS ゾーンでのセカンダリ インスタンスの作成について詳しくは、[マネージド インスタンスでのフェールオーバー グループの管理 (プレビュー)](#powershell-managing-failover-groups-with-managed-instances-preview) に関するページを参照してください。

- **2 つのインスタンス間のレプリケーション トラフィックを有効にする**

  各インスタンスは独自の VNet に分離されるため、これらの VNet 間の 2 方向トラフィックを許可する必要があります。 [Azure VPN ゲートウェイ](../vpn-gateway/vpn-gateway-about-vpngateways.md)に関するページを参照してください

- **インスタンス全体のフェールオーバーを管理するようにフェールオーバー グループを構成する**

  フェールオーバー グループでは、インスタンス内のすべてのデータベースのフェールオーバーを管理します。 グループが作成されると、インスタンス内の各データベースはセカンダリ インスタンスに自動的に geo レプリケートされます。 フェールオーバー グループを使用して、データベース サブセットの部分的なフェールオーバーを開始することはできません。

  > [!IMPORTANT]
  > データベースは、プライマリ インスタンスから削除された場合、geo セカンダリ インスタンスでも自動的に削除されます。

- **OLTP ワークロードに読み取り/書き込みのリスナーを使用する**

  OLTP 操作を実行するときに、サーバー URL として `failover-group-name.zone_id.database.windows.net` を使用すると、自動的にプライマリに接続されます。 この URL はフェールオーバー後にも変更されません。 フェールオーバーでは DNS レコードの更新が行われるため、クライアントの接続は、クライアント DNS キャッシュが最新の情報に更新された後にのみ新しいプライマリにリダイレクトされます。 セカンダリ インスタンスでは DNS ゾーンをプライマリと共有するため、同じ SAN 証明書を使用してクライアント アプリケーションを再接続できます。

- **読み取り専用クエリのために geo レプリケートされたセカンダリに直接接続する**

  データがある程度古くても構わない、論理的に分離された読み取り専用ワークロードがある場合、アプリケーションでセカンダリ データベースを使用できます。 geo レプリケートされたセカンダリに直接接続するには、`server.secondary.zone_id.database.windows.net` をサーバー URL として使用します。これにより、geo レプリケートされたセカンダリに直接接続されます。

  > [!NOTE]
  > 特定のサービス レベルでは、Azure SQL Database で、1 つの読み取り専用レプリカの容量を使用し、また、接続文字列の `ApplicationIntent=ReadOnly` パラメーターを使用して、読み取り専用クエリ ワークロードの負荷を分散するための[読み取り専用レプリカ](sql-database-read-scale-out.md)の使用がサポートされます。 geo レプリケートされたセカンダリを構成した場合は、この機能を使用して、プライマリ ロケーション、または geo レプリケートされたロケーションの読み取り専用レプリカに接続できます。
  > - プライマリ ロケーションの読み取り専用レプリカに接続するには、`failover-group-name.zone_id.database.windows.net` を使用します。
  > - プライマリ ロケーションの読み取り専用レプリカに接続するには、`failover-group-name.secondary.zone_id.database.windows.net` を使用します。

- **パフォーマンスの低下に備える**

  SQL のフェールオーバーの決定は、アプリケーションの他の領域や、使用されている他のサービスとは無関係に行われます。 アプリケーションが、同じリージョン内のコンポーネントや別のリージョン内のコンポーネントと "混在" する場合があります。 パフォーマンスの低下を防ぐには、確実に DR リージョンで冗長アプリケーションをデプロイし、これらの[ネットワーク セキュリティ ガイドライン](#failover-groups-and-network-security)に従います。

- **データの損失に備える**

  機能停止が検出された場合、経験的にデータの損失がゼロであれば、読み取り/書き込みのフェールオーバーが SQL によって自動的にトリガーされます。 それ以外の場合、`GracePeriodWithDataLossHours` で指定した期間、待機状態となります。 `GracePeriodWithDataLossHours` を指定した場合は、データの損失に備えてください。 一般に、機能の停止中、Azure では可用性が重視されます。 データの損失が許容できない場合は、必ず、GracePeriodWithDataLossHours を十分大きい数値 (24 時間など) に設定してください。

  読み取り/書き込みリスナーの DNS の更新は、フェールオーバーが開始された後すぐに行われます。 この操作でデータが失われることはありません。 しかし、データベース ロールの切り替えプロセスには、通常の状況で最大 5 分かかる場合があります。 これが完了するまで、新しいプライマリ インスタンスの一部のデータベースは引き続き読み取り専用となります。 PowerShell を使用してフェールオーバーが開始された場合、操作全体が同期されます。 Azure ポータルを使用して開始された場合、UI で完了状態が示されます。 REST API を使用して開始された場合は、標準的な Azure リソース マネージャーのポーリング メカニズムを使用して、完了を監視します。

  > [!IMPORTANT]
  > プライマリを元の場所に戻すには、手動のグループ フェールオーバーを使用します。 フェールオーバーの原因となった機能停止が軽減されたら、プライマリ データベースを元の場所に移動できます。 そのためには、グループの手動フェールオーバーを開始する必要があります。

## <a name="failover-groups-and-network-security"></a>フェールオーバー グループとネットワーク セキュリティ

一部のアプリケーションについてセキュリティ規則では、データ層へのネットワーク アクセスを、VM や Web サービスなど、特定の 1 つまたは複数のコンポーネントに限定する必要があります。ビジネス継続性の設計と、フェールオーバー グループの使用において、この要件は課題となっています。 このような制限付きアクセスを実装する場合は、次のオプションを検討してください。

### <a name="using-failover-groups-and-virtual-network-rules"></a>フェールオーバー グループと仮想ネットワーク規則を使用する

[Virtual Network サービス エンドポイントおよび規則](sql-database-vnet-service-endpoint-rule-overview.md)を使用して SQL データベースへのアクセスを制限する場合、各 Virtual Network サービス エンドポイントは 1 つの Azure リージョンだけにしか適用されないことに注意してください。 エンドポイントが他のリージョンを有効にして、サブネットからの通信を許可することはありません。 そのため、同じリージョンにデプロイされているクライアント アプリケーションのみが、プライマリ データベースに接続できます。 フェールオーバーの結果として SQL クライアント セッションは別の (セカンダリ) リージョン内のサーバーに再ルーティングされるので、それらのセッションをセカンダリ リージョンの外部にあるクライアントから実行した場合は失敗します。 そのため、参加するサーバーが Virtual Network 規則に含まれている場合、自動フェールオーバー ポリシーを有効にすることはできません。 手動フェールオーバーをサポートするには、次の手順を行います。

1. セカンダリ リージョンで (Web サービスや仮想マシンなどの) アプリケーションのフロントエンド コンポーネントの冗長コピーをプロビジョニングする
2. プライマリ サーバーとセカンダリ サーバーに対して別々に[Virtual Network 規則](sql-database-vnet-service-endpoint-rule-overview.md)を構成する
3. [Traffic Manager の構成を使用したフロント エンド フェールオーバー](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)を有効にする
4. 機能停止が検出されたときに手動フェールオーバーを開始する。 このオプションは、フロント エンドとデータ層の間で一貫した待機時間を必要とするアプリケーションに対して最適化されており、フロント エンド、データ層、またはこの両方が機能停止の影響を受ける場合に回復をサポートします。

> [!NOTE]
> **読み取り専用リスナー**を使用して読み取り専用ワークロードの負荷を分散する場合は、このワークロードを必ずセカンダリ リージョン内の VM などのリソースで実行することで、セカンダリ データベースに接続できるようにします。

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>フェールオーバー グループおよび SQL データベース ファイアウォール規則を使用する

ビジネス継続性計画でグループを使用するフェールオーバーと自動フェールオーバーが必要な場合は、従来のファイアウォール規則を使用して、SQL データベースへのアクセスを制限することができます。  自動フェールオーバーをサポートするには、次の手順を行います。

1. [パブリック IP を作成します](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)。
2. [パブリック ロード バランサーを作成](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer)し、パブリック IP を割り当てます。
3. フロント エンド コンポーネントに対して[仮想ネットワークと仮想マシンを作成](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers)します。
4. [ネットワーク セキュリティ グループを作成](../virtual-network/security-overview.md)し、受信接続を構成します。
5. ‘Sql’ [サービス タグ](../virtual-network/security-overview.md#service-tags)を使用して Azure SQL Database への送信接続が開いていることを確認します。
6. [SQL データベース ファイアウォール規則](sql-database-firewall-configure.md)を作成して、手順 1 で作成したパブリック IP アドレスからの受信トラフィックを許可します。

送信アクセスを構成する方法と、ファイアウォール規則で使用する IP の詳細については、[ロード バランサー送信接続](../load-balancer/load-balancer-outbound-connections.md)に関するページを参照してください。

上記の構成では、自動フェールオーバーによって、フロント エンド コンポーネントからの接続がブロックされることはありません。また、フロント エンドとデータ層の間の長い待機時間がアプリケーションで許容されることを前提としています。

> [!IMPORTANT]
> リージョンの機能停止に対してビジネス継続性を保証するためには、フロント エンド コンポーネントとデータベースの両方に対して地理的な冗長性を確保する必要があります。

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>マネージド インスタンスとその VNet の間の geo レプリケーションを有効にする

2 つの異なるリージョンのプライマリとセカンダリのマネージド インスタンスの間にフェールオーバー グループを設定すると、各インスタンスは独立した VNet を使用して分離されます。 これらの VNet 間のレプリケーション トラフィックを許可するには、以下の前提条件が満たされていることを確認します。

1. 2 つのマネージド インスタンスは、異なる Azure リージョンにある必要があります。
2. セカンダリは空 (ユーザー データベースなし) である必要があります。
3. プライマリとセカンダリのマネージド インスタンスは、同じリソース グループにある必要があります。
4. マネージド インスタンスが含まれている VNet には、[VPN ゲートウェイ](../vpn-gateway/vpn-gateway-about-vpngateways.md)経由で接続する必要があります。 グローバル VNet ピアリングはサポートされません。
5. 2 つのマネージド インスタンス VNet に重複する IP アドレスを含めることはできません。
6. 他のマネージド インスタンス サブネットからの接続では、インバウンドおよびアウトバウンドでポート 5022 およびその範囲の 11000 から 12000 を開くように、ネットワーク セキュリティ グループ (NSG) を設定する必要があります。 これで、インスタンス間のレプリケーション トラフィックを許可します

    > [!IMPORTANT]
    > NSG セキュリティ規則が正しく構成されていないと、データベースのコピー操作が停止します。

7. セカンダリ インスタンス上に DNS ゾーンのパートナーを構成する必要があります。 DNS ゾーンは、マネージド インスタンスのプロパティです。 これは、マネージド インスタンス名に続く、`.database.windows.net` プレフィックスの前にあるホスト名の一部を表します。 各 VNet での最初のマネージド インスタンスの作成時に、ランダムな文字列として生成されます。 DNS ゾーンをマネージド インスタンスの作成後に変更することはできません。同じサブネット内のすべてのマネージド インスタンスで、同じ DNS ゾーンの値が共有されます。 Managed Instance のフェールオーバー グループ設定では、プライマリの Managed Instance とセカンダリの Managed Instance で同じ DNS ゾーンの値を共有する必要があります。 これを実現するには、セカンダリのマネージド インスタンスの作成時に、DnsZonePartner パラメーターを指定します。 DNS ゾーンのパートナー プロパティでは、インスタンスのフェールオーバー グループを共有するマネージド インスタンスを定義します。 DnsZonePartner の入力として、別のマネージド インスタンスのリソース ID に渡すことで、現在作成されているマネージド インスタンスでは、パートナーのマネージド インスタンスと同じ DNS ゾーンの値が継承されます。

## <a name="upgrading-or-downgrading-a-primary-database"></a>プライマリ データベースのアップグレードまたはダウングレード

セカンダリ データベースの接続を解除することなく、プライマリ データベースを (General Purpose と Business Critical 間ではなく、同じサービス レベル内の) 異なるコンピューティング サイズにアップグレードまたはダウングレードできます。 アップグレードの場合は、最初にセカンダリ データベースをアップグレードしてからプライマリをアップグレードすることをお勧めします。 ダウングレードは逆の順序で行います。つまり、最初にプライマリをダウングレードしてからセカンダリをダウングレードします。 データベースを異なるサービス レベルにアップグレードまたはダウングレードすると、この推奨事項が強制されます。

> [!NOTE]
> セカンダリ データベースをフェールオーバー グループ構成の一部として作成した場合、セカンダリ データベースをダウングレードすることは推奨されません。 これは、フェールオーバーがアクティブ化された後にデータ層に通常のワークロードを処理するのに十分な容量を確保するためです。

## <a name="preventing-the-loss-of-critical-data"></a>重要なデータの損失の防止

ワイド エリア ネットワークの遅延は大きいため、連続コピーでは非同期のレプリケーション メカニズムが使用されます。 非同期レプリケーションでは、障害が発生した場合に部分的なデータ損失が避けられません。 しかし、データ損失が許されないアプリケーションもあります。 重要な更新情報を保護するには、アプリケーション開発者は、トランザクションがコミットされた直後に [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) ステム プロシージャを呼び出すことができます。 **sp_wait_for_database_copy_sync** を呼び出すと、最後にコミットされたトランザクションがセカンダリ データベースに転送されるまで、呼び出しスレッドがブロックされます。 ただし、転送されたトランザクションがセカンダリで再生およびコミットされるのを待つことはありません。 **sp_wait_for_database_copy_sync** の対象は、特定の連続コピー リンクです。 プライマリ データベースへの接続権限を持つユーザーが、このプロシージャを呼び出すことができます。

> [!NOTE]
> **sp_wait_for_database_copy_sync** は、フェールオーバー後のデータの損失を防ぎますが、読み取りアクセスに対して完全な同期を保証することはありません。 **sp_wait_for_database_copy_sync** プロシージャ呼び出しによる遅延は、長くなる場合があり、呼び出し時のトランザクション ログのサイズによって異なります。

## <a name="failover-groups-and-point-in-time-restore"></a>フェールオーバー グループとポイントインタイム リストア

フェールオーバー グループでのポイントインタイム リストアの使用については、[特定の時点への復元 (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore) に関するページを参照してください。

## <a name="programmatically-managing-failover-groups"></a>フェールオーバー グループのプログラムによる管理

前に説明したように、自動フェールオーバー グループとアクティブ geo レプリケーションは、Azure PowerShell および REST API を使用してプログラムによって管理することもできます。 次の表では、使用できるコマンド セットについて説明します。 アクティブ geo レプリケーションには、管理のための Azure Resource Manager API 一式 ([Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/)、[Azure PowerShell コマンドレット](https://docs.microsoft.com/powershell/azure/overview)など) が含まれています。 これらの API は、リソース グループの使用を必要とし、ロール ベース セキュリティ (RBAC) をサポートします。 アクセス ロールの実装方法の詳細については、[Azure のロール ベースのアクセス制御](../role-based-access-control/overview.md)に関するページをご覧ください。

### <a name="powershell-manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>PowerShell:単一データベースとエラスティック プールを使用して SQL データベース フェールオーバーを管理する

| コマンドレット | 説明 |
| --- | --- |
| [New-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |このコマンドはフェールオーバー グループを作成し、それをプライマリとセカンダリの両方のサーバーに登録します。|
| [Remove-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | サーバーからフェールオーバー グループを削除し、そのグループが含まれるすべてのセカンダリ データベースを削除します。 |
| [Get-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | フェールオーバー グループ構成を取得します。 |
| [Set-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |フェールオーバー グループの構成を変更します。 |
| [Switch-AzureRMSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | フェールオーバー グループのセカンダリ サーバーに対するフェールオーバーをトリガーします。 |
| [Add-AzureRmSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/add-azurermsqldatabasetofailovergroup)|Azure SQL データベース フェールオーバー グループに 1 つまたは複数のデータベースを追加する|
|  | |

> [!IMPORTANT]
> サンプル スクリプトについては、[単一データベース用のフェールオーバー グループの構成とフェールオーバー](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)に関するページを参照してください。
>

### <a name="powershell-managing-failover-groups-with-managed-instances-preview"></a>PowerShell:マネージド インスタンスでのフェールオーバー グループの管理 (プレビュー)

#### <a name="install-the-newest-pre-release-version-of-powershell"></a>最新のプレリリース バージョンの Powershell をインストールする

1. PowerShellGet モジュールを 1.6.5 (または最新のプレビュー バージョン) に更新します。 [PowerShell プレビューのサイト](https://www.powershellgallery.com/packages/AzureRM.Sql/4.11.6-preview)を参照してください。

   ```Powershell
      install-module PowerShellGet -MinimumVersion 1.6.5 -force
   ```

2. 新しい PowerShell ウィンドウで、次のコマンドを実行します。

   ```Powershell
      import-module PowerShellGet
      get-module PowerShellGet #verify version is 1.6.5 (or newer)
      install-module azurerm.sql -RequiredVersion 4.5.0-preview -AllowPrerelease –Force
      import-module azurerm.sql
   ```

#### <a name="powershell-commandlets-to-create-an-instance-failover-group"></a>インスタンスのフェールオーバー グループを作成する Powershell コマンドレット

| API | 説明 |
| --- | --- |
| New-AzureRmSqlDatabaseInstanceFailoverGroup |このコマンドはフェールオーバー グループを作成し、それをプライマリとセカンダリの両方のサーバーに登録します。|
| Set-AzureRmSqlDatabaseInstanceFailoverGroup |フェールオーバー グループの構成を変更します。|
| Get-AzureRmSqlDatabaseInstanceFailoverGroup |フェールオーバー グループ構成を取得します。|
| Switch-AzureRmSqlDatabaseInstanceFailoverGroup |フェールオーバー グループのセカンダリ サーバーに対するフェールオーバーをトリガーします。|
| Remove-AzureRmSqlDatabaseInstanceFailoverGroup | フェールオーバー グループを削除します|

### <a name="rest-api-manage-sql-database-failover-groups-with-standalone-and-pooled-databases"></a>REST API:スタンドアロン データベースまたはプールされたデータベースで SQL データベースのフェールオーバー グループを管理する

| API | 説明 |
| --- | --- |
| [Create or Update Failover Group](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | フェールオーバー グループを作成または更新します |
| [Delete Failover Group](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | フェールオーバー グループをサーバーから削除します。 |
| [Failover (計画されたフェールオーバー)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | 現在のプライマリ サーバーからこのサーバーにフェールオーバーします。 |
| [Force Failover Allow Data Loss](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |現在のプライマリ サーバーからこのサーバーにフェールオーバーします。 この操作を行うとデータが失われる可能性があります。 |
| [フェールオーバー グループの取得](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | フェールオーバー グループを取得します。 |
| [List Failover Groups By Server](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | サーバーにフェールオーバー グループが表示されます。 |
| [Update Failover Group](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | フェールオーバー グループを更新します。 |
|  | |

### <a name="rest-api-manage-failover-groups-with-managed-instances-preview"></a>REST API:マネージド インスタンスでフェールオーバー グループを管理する (プレビュー)

| API | 説明 |
| --- | --- |
| [Create or Update Failover Group](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | フェールオーバー グループを作成または更新します |
| [Delete Failover Group](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | フェールオーバー グループをサーバーから削除します。 |
| [Failover (計画されたフェールオーバー)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | 現在のプライマリ サーバーからこのサーバーにフェールオーバーします。 |
| [Force Failover Allow Data Loss](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |現在のプライマリ サーバーからこのサーバーにフェールオーバーします。 この操作を行うとデータが失われる可能性があります。 |
| [フェールオーバー グループの取得](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | フェールオーバー グループを取得します。 |
| [List Failover Groups - List By Location](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | 場所ごとにフェールオーバー グループをリストします。 |

## <a name="next-steps"></a>次の手順

- サンプル スクリプトは、以下を参照してください。
  - [アクティブ geo レプリケーションを使用して、単一のデータベースを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [アクティブ geo レプリケーションを使用して、プールされているデータベースを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [単一データベースのフェールオーバー グループを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- ビジネス継続性の概要およびシナリオについては、[ビジネス継続性の概要](sql-database-business-continuity.md)を参照してください。
- Azure SQL Database 自動バックアップの詳細については、「 [SQL Database 自動バックアップ](sql-database-automated-backups.md)」を参照してください。
- 自動バックアップを使用して復旧する方法については、 [サービス主導のバックアップからのデータベース復元](sql-database-recovery-using-backups.md)に関する記事を参照してください。
- 新しいプライマリ サーバーとデータベースの認証要件については、 [障害復旧後の SQL Database のセキュリティ](sql-database-geo-replication-security-config.md)に関する記事を参照してください。

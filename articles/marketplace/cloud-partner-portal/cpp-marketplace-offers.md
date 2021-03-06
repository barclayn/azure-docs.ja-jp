---
title: Azure Marketplace と AppSource Marketplace のオファー | Microsoft Docs
description: Azure Marketplace と AppSource Marketplace のオファーの作成と管理
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: ca4979188830fcb53732750a3eaadfc2009c4f9a
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658710"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure Marketplace と AppSource Marketplace のオファー

このセクションの最初の部分では、Azure Marketplace および AppSource Marketplace 向けのオファーの作成と管理に使用される一般的な操作について説明します。  この部分では、特定のオファーの種類を管理するために理解しておく必要のある背景情報と、すべてのオファーの種類に共通する技術情報を提供します。  このセクションの大部分では、特定のオファーの種類を作成して管理する方法の詳細な手順を示します。  

次のビデオでは、Azure Marketplace または AppSource で利用できるさまざまな機能とオファーの種類が紹介されています。  また、これらのマーケットプレースでのアプリケーションまたはサービスの公開に関する重要な技術的側面およびビジネス的側面についても説明されています。

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Azure Marketplace および AppSource 向けのアプリとサービスの構築 - Build 2018**

これらのマーケットプレースの詳細については、[Azure Marketplace と AppSource の公開ガイド](../marketplace-publishers-guide.md)のページをご覧ください。


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Marketplace および AppSource でのオファーの種類

[Cloud パートナー ポータル](https://cloudpartner.azure.com)で現在サポートされているオファーの種類を次の表に示します。  オファーの種類ごとに、オファーを掲載できるマーケットプレースと、オファーのソリューション テクノロジの一般的な説明が示されています。

|                プランの種類                |  マーケットプレース  |   説明                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure アプリケーション](./azure-applications/cpp-azure-app-offer.md) | Azure | ソリューションは、1 つ以上の仮想マシン (VM) とオプションのカスタム Azure コードで構成され、Azure Resource Manager テンプレートを使用してデプロイされます。  デプロイは、顧客がソリューション テンプレートを使って行うか、または公開元が管理することができます。 この種類は、仮想マシン オファーの種類より高い柔軟性を提供するために使用されます。  |
| [コンサルティング サービス](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | 両方 | Microsoft 認定のコンサルタントは、Azure Marketplace または AppSource のどちらかに、ドメイン固有のサービスを掲載できます。  専門知識によって、顧客による問題の評価と、ビジネス目標を達成する適切なソリューションの作成とデプロイが支援されます。  |
| [コンテナー](./containers/cpp-containers-offer.md)  | Azure | ソリューションは、Kubernetes ベースのサービスまたは Azure コンテナー インスタンスとしてプロビジョニングされた Docker コンテナー イメージです。 |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | このエンタープライズ リソース プランニング (ERP) およびビジネス管理システムを拡張するパッケージ。 |
| [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | 営業、サービス、プロジェクト サービス、およびフィールド サービス モジュールによってこの顧客リソース管理 (CRM) システムを拡張するパッケージ。  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | 高度な財務、経営、製造、およびサプライ チェーン管理をサポートするこのエンタープライズ リソース プランニング (ERP) サービスを拡張するパッケージ。 |
| [IoT Edge モジュール](./iot-edge-module/cpp-offer-process-parts.md) | Azure | IoT Edge デバイス上で実行される Docker 互換コンテナー。  カスタム コード、他の Azure サービス、サード パーティのサービスの組み合わせを使用する小規模のコンピューティング モジュールが含まれています。 |
| [Power BI アプリ](./power-bi/cpp-power-bi-offer.md) | AppSource | データフローを使用して一般的なデータ ストレージ内のデータにレポートとダッシュボードを接続するパッケージ。 |
| [SaaS アプリ](./saas-app/cpp-saas-offer.md) | Azure | ソリューションは公開元によって管理されるサービスとしてのソフトウェア サブスクリプションであり、ユーザーは Azure Active Directory を利用するカスタマイズされたインターフェイスを使用してログオンします。 |
| [仮想マシン](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | ソリューションは、顧客のサブスクリプションにデプロイされる 1 つの仮想マシンに含まれます。  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

詳細については、「[Publishing guide by offer type](../publisher-guide-by-offer-type.md)」(プランの種類別の公開ガイド) をご覧ください。


## <a name="next-steps"></a>次の手順

[オファーの管理](./manage-offers/cpp-manage-offers.md)に関するトピックで、マーケットプレースで実行できる一般的な操作と、一般的な技術属性および資産について学習します。

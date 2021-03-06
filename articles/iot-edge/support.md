---
title: サポートされているオペレーティング システム、コンテナー エンジン - Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge デーモンとランタイムを実行できるオペレーティング システム、運用デバイス用にサポートされるコンテナー エンジンについて説明します。
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6443260de0a8bd8531edb303fa581d281034fef3
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555610"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IoT Edge のサポートされるシステム

Azure IoT Edge 製品のサポートを受ける方法は複数あります。

**バグの報告** – Azure IoT Edge 製品に関する開発の大多数は、IoT Edge のオープン ソース プロジェクトで発生します。 バグはプロジェクトの[問題ページ](https://github.com/azure/iotedge/issues)で報告できます。 修正プログラムはプロジェクトが製品の更新プログラムになるまでの時間を早めます。

**Microsoft カスタマー サポート チーム** – [サポート プラン](https://azure.microsoft.com/support/plans/)に加入しているユーザーは、[Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac) から直接サポート チケットを作成することで、Microsoft カスタマー サポート チームとやり取りをすることができます。

**機能の要望** – Azure IoT Edge 製品はその製品の[ユーザーの声のページ](https://feedback.azure.com/forums/907045-azure-iot-edge)を介して機能の要望を追跡します。

## <a name="operating-systems"></a>オペレーティング システム
Azure IoT Edge はコンテナーを実行できるほとんどのオペレーティング システムで実行できます。ただし、それらがすべて均等にサポートされているわけではありません。 オペレーティング システムは、ユーザーが受けられるサポートのレベルを表す階層別にグループ化されています。

### <a name="tier-1"></a>レベル 1
レベル 1 のシステムは公式にサポートされていると見なされます。 これは、次のことを意味します。
* Microsoft がそれらのオペレーティング システムに対して自動テストを実施している
* Microsoft がそれらのインストール パッケージを提供している

一般公開
| オペレーティング システム | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Raspbian-stretch | いいえ  | はい|
| Ubuntu Server 16.04 | はい | いいえ  |
| Ubuntu Server 18.04 | はい | いいえ  |

パブリック プレビュー
| オペレーティング システム | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core ビルド 17763 | はい | いいえ  |
| Windows コンテナー用 Windows 10 ビルド 17763<br><br>Linux コンテナー用 Windows 10 ビルド 14393 以降\* | はい | いいえ  |
| Windows コンテナー用 Windows Server 2019<br><br>Linux コンテナー用 Windows Server 2016 以降\* | はい | いいえ  |

\* Microsoft は、開発およびテスト目的でのみ、Windows デバイス上の Linux コンテナー用インストール パッケージを提供しています。 これは、運用環境ではサポートされない構成です。 

### <a name="tier-2"></a>レベル 2
レベル 2 のシステムは Azure IoT Edge と互換性があり、比較的簡単に使用できると見なされます。 これは、次のことを意味します。
* Microsoft がそのプラットフォームに対してアドホック テストを実施している、またはそのプラットフォーム上で Azure IoT Edge をパートナーが正常に実行していることを把握している
* 他のプラットフォーム用のインストール パッケージがそれらのプラットフォームで機能することがある

| オペレーティング システム | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | はい | はい |
| Debian 8 | はい | はい |
| Debian 9 | はい | はい |
| RHEL 7.5 | はい | はい |
| Ubuntu 18.04 | はい | はい |
| Ubuntu 16.04 | はい | はい |
| Wind River 8 | はい | いいえ  |
| Yocto | はい | いいえ  |

## <a name="container-engines"></a>コンテナー エンジン
Azure IoT Edge がモジュールを起動するには、それが実行されているオペレーティング システムに関係なく、コンテナー エンジンが必要です。 Microsoft には、この要件を満たすために、moby-engine というコンテナー エンジンが用意されています。 これは Moby オープン ソース プロジェクトをベースとします。 他にも有名なコンテナー エンジンとして、Docker CE や Docker EE が挙げられます。 これらも Moby オープン ソース プロジェクトをベースとし、Azure IoT Edge と互換性があります。 Microsoft はこれらのコンテナー エンジンを使用するシステムに対してベスト エフォート サポートを提供しています。ただし、Microsoft はそれらの問題について修正プログラムを発行することができません。 この理由から、Microsoft では運用システムで moby-engine を使用することを推奨しています。


---
title: Azure RBAC の拒否割り当てについて | Microsoft Docs
description: Azure のリソースに対するロールベースのアクセス制御 (RBAC) の拒否割り当てについて説明します。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: fa1a979c01999bd79c45d24e4c7771edaf346dd8
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632417"
---
# <a name="understand-deny-assignments"></a>拒否割り当てについて

ロールの割り当てと同様に、"*拒否割り当て*" ではアクセスの拒否を目的として、特定のスコープでユーザー、グループ、またはサービス プリンシパルに一連の拒否アクションがアタッチされます。 拒否割り当てでは、ロールの割り当てでアクセスを許可されている場合であっても、指定したアクションがユーザーによって実行されるのをブロックします。 Azure の一部のリソース プロバイダーに拒否割り当てが追加されました。 現在、拒否割り当ては**読み取り専用**であり、Azure によってのみ設定されます。

いくつかの点で、拒否割り当てはロールの割り当てとは異なります。 拒否割り当てはプリンシパルを除外できるほか、子スコープへの継承を避けることができます。 拒否割り当ては、[従来のサブスクリプション管理者](rbac-and-directory-admin-roles.md)の割り当てにも適用されます。

この記事では、拒否割り当てがどのように定義されるかについて説明します。

## <a name="deny-assignment-properties"></a>拒否割り当てのプロパティ

 拒否割り当てには、以下のプロパティがあります。

> [!div class="mx-tableFixed"]
> | プロパティ | 必須 | type | 説明 |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | はい | String | 拒否割り当ての表示名。 名前は、指定のスコープで一意である必要があります。 |
> | `Description` | いいえ  | String | 拒否割り当ての説明。 |
> | `Permissions.Actions` | 少なくとも 1 つの Actions または DataActions | String[] | 拒否割り当てによってアクセスがブロックされる管理操作を指定する文字列の配列。 |
> | `Permissions.NotActions` | いいえ  | String[] | 拒否割り当てから除外される管理操作を指定する文字列の配列。 |
> | `Permissions.DataActions` | 少なくとも 1 つの Actions または DataActions | String[] | 拒否割り当てによってアクセスがブロックされるデータ操作を指定する文字列の配列。 |
> | `Permissions.NotDataActions` | いいえ  | String[] | 拒否割り当てから除外されるデータ操作を指定する文字列の配列。 |
> | `Scope` | いいえ  | String | 拒否割り当てが適用されるスコープを指定する文字列。 |
> | `DoNotApplyToChildScopes` | いいえ  | Boolean | 拒否割り当てが子スコープに適用されるかどうかを指定します。 既定値は false です。 |
> | `Principals[i].Id` | はい | String[] | 拒否割り当てが適用される Azure AD プリンシパル オブジェクト ID (ユーザー、グループ、サービス プリンシパル、またはマネージド ID) の配列。 すべてのプリンシパルを表すために空の GUID `00000000-0000-0000-0000-000000000000` に設定されます。 |
> | `Principals[i].Type` | いいえ  | String[] | Principals[i].Id によって表されるオブジェクトの種類の配列。すべてのプリンシパルを表すために `SystemDefined` に設定されます。 |
> | `ExcludePrincipals[i].Id` | いいえ  | String[] | 拒否割り当てが適用されない Azure AD プリンシパル オブジェクト ID (ユーザー、グループ、サービス プリンシパル、またはマネージド ID) の配列。 |
> | `ExcludePrincipals[i].Type` | いいえ  | String[] | ExcludePrincipals[i].Id によって表されるオブジェクトの種類の配列。 |
> | `IsSystemProtected` | いいえ  | Boolean | この拒否割り当てが Azure によって作成されたものかどうか、およびこの拒否割り当てを編集または削除できるかどうかを指定します。 現在、すべての拒否割り当てはシステムによって保護されています。 |

## <a name="system-defined-principal"></a>システム定義のプリンシパル

拒否割り当てをサポートするために、**システム定義のプリンシパル**が導入されました。 このプリンシパルは、Azure AD ディレクトリ内のすべてのユーザー、グループ、サービス プリンシパル、マネージド ID を表します。 プリンシパル ID がゼロ GUID `00000000-0000-0000-0000-000000000000` で、プリンシパルの種類が `SystemDefined` の場合、プリンシパルはすべてのプリンシパルを表します。 `SystemDefined` を `ExcludePrincipals` と組み合わせて、一部のユーザー以外のすべてのプリンシパルを拒否することができます。 `SystemDefined` には次の制約があります。

- `Principals` でのみ使用することができ、`ExcludePrincipals` では使用できません。
- `Principals[i].Type` は `SystemDefined` に設定する必要があります。

## <a name="next-steps"></a>次の手順

* [RBAC と REST API を使用して拒否割り当てを一覧表示する](deny-assignments-rest.md)
* [ロール定義について](role-definitions.md)

---
title: ordinal 作成済みエンティティ
titleSuffix: Azure
description: この記事には、Language Understanding (LUIS) での ordinal 作成済みエンティティについての情報が含まれます。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: f6762c74dbe6b16791e597464584b91e696220f1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863452"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>LUIS アプリの ordinal 作成済みエンティティ
序数は、セット内のオブジェクトの数値表現 `first`、`second`、`third` です。 このエンティティは既にトレーニングされているので、序数メールを含む発話の例をアプリケーション意図に追加する必要はありません。 ordinal エンティティは、[多くのカルチャ](luis-reference-prebuilt-entities.md)でサポートされます。 

## <a name="types-of-ordinal"></a>ordinal の種類
ordinal は [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) GitHub リポジトリから管理されます。

## <a name="resolution-for-prebuilt-ordinal-entity"></a>作成済み ordinal エンティティの解決
次の例では、**builtin.ordinal** エンティティの解決を示します。

```json
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>次の手順

[percentage](luis-reference-prebuilt-percentage.md)、[phonenumber](luis-reference-prebuilt-phonenumber.md)、[temperature](luis-reference-prebuilt-temperature.md) エンティティについて学習します。 

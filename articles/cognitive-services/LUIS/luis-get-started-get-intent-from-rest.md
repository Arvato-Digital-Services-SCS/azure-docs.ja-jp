---
title: クイック スタート:REST API で意図を取得する - LUIS
titleSuffix: Azure Cognitive Services
description: この REST API のクイックスタートでは、利用可能なパブリック LUIS アプリを使用して、会話形式のテキストからユーザーの意図を判断します。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 10/17/2019
ms.author: diberry
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 3a8badb74bb8919876f3c0670d785f44fbcbb397
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499650"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>クイック スタート:REST API で意図を取得する

このクイック スタートでは、提供されているパブリック LUIS アプリを使って、会話形式のテキストからユーザーの意図を判断します。 パブリック アプリの HTTP 予測エンドポイントにユーザーの意図をテキストとして送信します。 エンドポイントでは、LUIS によってパブリック アプリのモデルが適用されます。これにより自然言語テキストの意味が分析され、全体的な意図が特定されて、アプリのサブジェクト ドメインに関連したデータが抽出されます。 

このクイック スタートでは、エンドポイント REST API を使用します。 詳細については、[エンドポイント API のドキュメント](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)をご覧ください。

この記事には、無料の [LUIS](https://www.luis.ai) アカウントが必要です。 

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"

[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]

::: zone-end

::: zone pivot="programming-language-nodejs"

[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]

::: zone-end

::: zone pivot="programming-language-go"

[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]

::: zone-end

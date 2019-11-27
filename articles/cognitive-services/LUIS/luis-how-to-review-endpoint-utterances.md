---
title: ユーザーの発話を確認する - LUIS
titleSuffix: Azure Cognitive Services
description: アクティブ ラーニングによってキャプチャされた発話を確認し、意図を選択して、文字にされた発話に対応するエンティティをマークします。その後、変更を承認し、トレーニングして公開することになります。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 67953f552b5b2bcdd7d13253548227e57dab8548
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132651"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>エンドポイントの発話を見直し、LUIS アプリを改善する方法

正しく予測するためにエンドポイントの発話を見直すプロセスは[アクティブ ラーニング](luis-concept-review-endpoint-utterances.md)と呼ばれています。 アクティブ ラーニングでは、エンドポイント クエリがキャプチャされ、ユーザーの不確かなエンドポイントの発話が選択されます。 これらの発話をレビューして、意図を選択し、文字にされた発話に対応するエンティティをマークすることになります。 それらの変更を承認して発話の例に追加したら、トレーニングして公開します。 そうすることで、LUIS による発話の識別の精度が向上していきます。

LUIS アプリに多くの人が貢献している場合、 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="enable-active-learning"></a>アクティブ ラーニングを有効にする

アクティブ ラーニングを有効にするには、ユーザー クエリをログに記録する必要があります。 そのためには、`log=true` という querystring パラメーターと値を指定し、[エンドポイント クエリ](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint)を呼び出します。

## <a name="correct-intent-predictions-to-align-utterances"></a>正しい意図予測で発話を調整する

各発話では、 **[Aligned intent]\(配置される意図\)** 列に意図の候補が表示されます。 

> [!div class="mx-imgBorder"]
> [![LUIS で理解されないエンドポイントの発話を見直す](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

その意図に納得できない場合、チェック マークを選択します。 候補に同意しない場合は、配置される意図のドロップダウン リストから適切な意図を選択し、その配置される意図の右側にあるチェック マークをオンにします。 チェック マークを選択すると、発話が意図に移動し、 **[Review Endpoint Utterances]\(エンドポイントの発話を見直す\)** リストから削除されます。 

> [!TIP]
> [Intent detail]\(意図の詳細\) ページに進み、 **[Review Endpoint Utterances]\(エンドポイントの発話を見直す\)** リストの全サンプル発話のエンティティ予測を見直し、修正することが重要です。

## <a name="delete-utterance"></a>発話を削除する

各発話は、レビュー リストから削除できます。 一度削除すると、リストに再び表示されることはなくなります。 ユーザーがエンドポイントから同じ発話を入力したとしても同様です。 

発話を削除すべきかわからない場合、None 意図に移動するか、`miscellaneous` のような新しい意図を作成し、その意図に発話を移動してください。 

## <a name="disable-active-learning"></a>アクティブ ラーニングを無効にする

アクティブ ラーニングを無効にするには、ユーザーのクエリがログされないようにします。 これを行うには、`log=false` querystring パラメーターと値を指定して[エンドポイント クエリ](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint)を設定します。既定値が false のため、querystring 値は使用しません。

## <a name="next-steps"></a>次の手順

推奨される発話にラベル付けした後にパフォーマンスがどのように向上するかをテストするには、上部のパネルの **[Test]\(テスト\)** を選択してテスト コンソールにアクセスします。 テスト コンソールを使用してアプリをテストする手順については、[アプリのトレーニングとテスト](luis-interactive-test.md)に関する記事をご覧ください。

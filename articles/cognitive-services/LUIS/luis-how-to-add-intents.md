---
title: 意図の追加 - LUIS
titleSuffix: Azure Cognitive Services
description: 意図を LUIS アプリに追加して、同じ意図を持つ質問またはコマンドのグループを特定します。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 66a3350dee60772ce706af8995179dcd8c485b64
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904314"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>ユーザーの発話意図を判断する意図を追加する

[意図](luis-concept-intent.md)を LUIS アプリに追加して、同じ意図を持つ質問またはコマンドのグループを特定します。 

インテントを管理するには、上部のナビゲーション バーの **[ビルド]** セクションに移動し、左側のパネルの **[Intents]\(意図\)** を選択します。 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-intent"></a>意図を追加する

1. [LUIS プレビュー ポータル](https://preview.luis.ai)で **[ビルド]** を選択して意図を表示します。 
1. **[意図]** ページで、 **[+ 作成]** を選択します。
1. **[Create new intent]\(意図の新規作成\)** ダイアログ ボックスで、意図の名前 (例: `ModifyOrder`) を入力し、 **[完了]** を選択します。

    > [!div class="mx-imgBorder"]
    > ![意図を追加する](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    意図にはサンプル発話が必要です。

## <a name="add-an-example-utterance"></a>発話の例を追加する

発話の例とは、ユーザーの質問や命令のサンプル テキストです。 この意図を予測するタイミングを Language Understanding (LUIS) に学習させるには、意図に発話の例を追加する必要があります。 LUIS では、15 個から 30 個のサンプル発話がないと意図が理解されません。 発話の例はまとめて追加しないでください。 各発話は、意図に既に含まれているサンプルとの違いに気を付けて選択する必要があります。 

1. 意図詳細ページで、意図の名前の下にあるテキスト ボックスに、該当するユーザーから想定される関連する発話 (`Deliver a large cheese pizza` など) を入力します。
 
    > [!div class="mx-imgBorder"]
    > ![意図の詳細ページのスクリーンショット、発話が強調表示されています](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS では、すべての発話が小文字に変換され、ハイフンなどの[トークン](luis-language-support.md#tokenization)の前後にはスペースが追加されます。

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>意図予測のエラー 

意図に含まれている発話の例では、発話の例が現在含まれている意図とトレーニング中に決定された意図の間に意図予測のエラーが存在することがあります。 

発話予測のエラーを見つけて修正するには、 **[フィルター]** オプションの [Incorrect] (正しくない) および [Unclear] (不明) を **[詳細ビュー]** の **[表示]** オプションと組み合わせて使用します。 

![発話予測のエラーを見つけて修正するには、[フィルター] オプションを使用します。](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

フィルターとビューが適用され、エラーを含む発話の例が存在する場合は、発話の例のリストにそれらの発話と問題が表示されます。

> [!div class="mx-imgBorder"]
> ![フィルターとビューが適用され、エラーを含む発話の例が存在する場合は、発話の例のリストにそれらの発話と問題が表示されます。](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

各行には、発話の例の現在のトレーニングの予測スコア、最も近いライバルのスコア、これらの 2 つのスコアの差が表示されます。 

### <a name="fixing-intents"></a>意図の修正

意図予測のエラーを修正する方法を学習するには、[概要ダッシュボード](luis-how-to-use-dashboard.md)を使用します。 概要ダッシュボードには、アクティブなバージョンの最後のトレーニングに関する分析が表示され、モデルを修正するための最上位の提案が示されます。  

## <a name="using-the-contextual-toolbar"></a>コンテキスト ツールバーの使用

コンテキスト ツール バーに他のアクションがあります。

* 発話の例を編集または削除します
* サンプル発話を別の意図に再割り当てします
* フィルターとビュー: フィルター処理されたエンティティが含まれる発話のみを表示するか、オプションの詳細を表示します
* サンプル発話を検索します

## <a name="train-your-app-after-changing-model-with-intents"></a>意図を含むモデルの変更後にアプリをトレーニングする

意図を追加、編集、または削除したら、その変更がエンドポイントのクエリに反映されるように、アプリを[トレーニング](luis-how-to-train.md)して[発行](luis-how-to-publish-app.md)します。 1 つ変更するたびにトレーニングすることはしないでください。 ひとまとまりの変更後にトレーニングしてください。 

## <a name="next-steps"></a>次の手順

[発話の例](luis-how-to-add-example-utterances.md)にエンティティを追加する方法について学習する。 

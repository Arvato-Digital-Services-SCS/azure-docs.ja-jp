---
title: 変更の追跡に基づいてエンリッチされたコンテンツの増分インデックスを設定する
titleSuffix: Azure Cognitive Search
description: 変更の追跡を有効にし、認知スキルセットの制御された処理のためにエンリッチされたコンテンツの状態を保持します。
author: vkurpad
manager: eladz
ms.author: vikurpad
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ac082d6ecb6624dc0d5bc0ab927ff8b91ebdabce
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510084"
---
# <a name="how-to-set-up-incremental-indexing-of-enriched-documents-in-azure-cognitive-search"></a>Azure Cognitive Search でエンリッチされたドキュメントの増分インデックス作成を設定する方法

この記事では、Azure Cognitive Search エンリッチメント パイプラインを介して移動するエンリッチされたドキュメントに状態とキャッシュを追加して、サポートされている任意のデータ ソースからドキュメントの増分インデックスを作成できるようにする方法を示します。 既定では、スキルセットはステートレスであり、その構成の任意の部分を変更するには、インデクサーを完全に再実行する必要があります。 増分インデックス作成では、インデクサーで、変更されたパイプラインの部分を特定し、変更されていない部分に対して既存のエンリッチメントを再利用し、変更する手順のエンリッチメントを修正できます。 キャッシュされたコンテンツは Azure Storage に配置されます。

インデクサーの設定に慣れていない場合は、[インデクサーの概要](search-indexer-overview.md)から開始し、[スキルセット](cognitive-search-working-with-skillsets.md)に進んで、エンリッチメント パイプラインについて学習してください。 主要な概念の背景の詳細については、[増分インデックス作成](cognitive-search-incremental-indexing-conceptual.md)に関するページを参照してください。

増分インデックスの作成は、[Search REST api-version=2019-05-06-Preview](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations) を使用して構成されます。

> [!NOTE]
> この機能は、ポータルではまだ使用できず、プログラムで使用する必要があります。
>

## <a name="modify-an-existing-indexer"></a>既存のインデクサーを変更する

既存のインデクサーがある場合は、これらの手順に従って、増分インデックスの作成を有効にします。

### <a name="step-1-get-the-indexer"></a>手順 1:インデクサーを取得する

必要なデータ ソースとインデックスが既に定義されている、有効な既存のインデクサーを使用して開始します。 インデクサーは実行可能である必要があります。 API クライアントを使用して、[GET 要求](https://docs.microsoft.com/rest/api/searchservice/get-indexer)を作成し、増分インデックス作成を追加するインデクサーの現在の構成を取得します。

```http
GET https://[service name].search.windows.net/indexers/[your indexer name]?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-2-add-the-cache-property"></a>手順 2:cache プロパティを追加する

GET 要求から応答を編集して、インデクサーに `cache` プロパティを追加します。 キャッシュ オブジェクトには、Azure ストレージ アカウントへの接続文字列である 1 つのプロパティのみが必要です。

```json
    "cache": {
        "storageConnectionString": "[your storage connection string]"
    }
```

### <a name="step-3-reset-the-indexer"></a>手順 3:インデクサーをリセットする

> [!NOTE]
> インデクサーをリセットすると、コンテンツをキャッシュできるように、データ ソース内のすべてのドキュメントが再度処理されます。 すべてのコグニティブ エンリッチメントが、すべてのドキュメントで再実行されます。
>

すべてのドキュメントが一貫した状態になるように、既存のインデクサーに増分インデックス作成を設定するときは、インデクサーをリセットする必要があります。 [REST API](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) を使用して、インデクサーをリセットします。

```http
POST https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-4-save-the-updated-definition"></a>手順 4:更新された定義を保存する

PUT 要求を使用してインデクサーの定義を更新します。要求の本文には、更新されたインデクサーの定義を含める必要があります。

```http
PUT https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
{
    "name" : "your indexer name",
    ...
    "cache": {
        "storageConnectionString": "[your storage connection string]",
        "enableReprocessing": true
    }
}
```

ここでインデクサーに別の GET 要求を発行した場合、サービスからの応答には、キャッシュ オブジェクトの `cacheId` プロパティが含まれます。 `cacheId` は、このインデクサーによって処理される各ドキュメントのキャッシュされたすべての結果と中間状態が格納されているコンテナーの名前です。

## <a name="enable-incremental-indexing-on-new-indexers"></a>新しいインデクサーの増分インデックス作成を有効にする

新しいインデクサーの増分インデックス作成を設定するには、インデクサー定義のペイロードに `cache` プロパティを含めます。 `2019-05-06-Preview` バージョンの API を使用していることを確認してください。

## <a name="overriding-incremental-indexing"></a>増分インデックス作成をオーバーライドする

構成が済むと、増分インデックス作成により、インデックス作成パイプライン全体での変更が追跡されて、インデックスとプロジェクション全体でのドキュメントの最終的な整合性が実現されます。 場合によっては、インデックス作成パイプラインに対する更新の結果としてインデクサーにより追加処理が行われないよう、この動作をオーバーライドする必要があります。 たとえば、データ ソースの接続文字列を更新した場合は、データソースが変化したので、インデクサーをリセットし、すべてのドキュメントのインデックスを再作成する必要があります。 ただし、新しいキーで接続文字列を更新しただけの場合は、変更によって既存のドキュメントが更新されてほしくはありません。 逆に、インデックス作成パイプラインが変更されていない場合でも、インデクサーでキャッシュを無効にしてドキュメントを強化したいことがあります。 たとえば、新しいモデルを使用してカスタム スキルを再デプロイし、すべてのドキュメントでスキルの再実行が必要な場合は、インデクサーを無効にしたいことがあります。

### <a name="override-reset-requirement"></a>リセットの要件をオーバーライドする

インデックス作成パイプラインを変更したときは、キャッシュが無効化されるすべての変更で、インデクサーをリセットする必要があります。 インデクサー パイプラインを変更したときに、変更追跡によってキャッシュが無効にならないようにする場合は、インデクサーまたはデータソースに対する操作のために、`ignoreResetRequirement` クエリ文字列パラメーターを `true` に設定する必要があります。

### <a name="override-change-detection"></a>変更検出をオーバーライドする

スキルセットを更新するとドキュメントに不整合フラグが設定される場合 (たとえば、スキルの再デプロイ時にカスタム スキルの URL を更新する場合)、スキルセットの更新で `disableCacheReprocessingChangeDetection` クエリ文字列パラメーターを `true` に設定します。

### <a name="force-change-detection"></a>変更検出を強制する

インデックス作成パイプラインに外部エンティティの変更を認識させたい場合は (カスタム スキルの新しいバージョンをデプロイするときなど)、スキルセットを更新し、スキルの定義を編集することによって特定のスキルに "タッチ" する必要があります。具体的には、URL にタッチし、変更検出を強制して、そのスキルのキャッシュを無効にします。

## <a name="next-steps"></a>次の手順

この記事では、スキルセットを含むインデクサーの増分インデックスの作成について取り上げました。 さらに知識を深めるために、Azure Cognitive Search のすべてのインデックス作成シナリオに適用される、一般的なインデックスの再作成に関する記事を確認してください。

+ [Azure Cognitive Search インデックスを再構築する方法](search-howto-reindex.md)。 
+ [Azure Cognitive Search で大容量のデータ セットのインデックスを作成する方法](search-howto-large-index.md)。 

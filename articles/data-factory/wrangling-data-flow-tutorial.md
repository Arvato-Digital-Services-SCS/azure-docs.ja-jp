---
title: Azure Data Factory でラングリング データ フローの使用を開始する
description: ラングリング データ フローを使用して Azure Data Factory データを準備する方法に関するチュートリアル
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 8ae109045381dba77610dedc5bb95e233b213eee
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73682272"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>ラングリング データ フローを使用してデータを準備する

## <a name="create-a-wrangling-data-flow"></a>ラングリング データ フローを作成する

Azure Data Factory でラングリング データ フローを作成するには、2 つの方法があります。 1 つ目の方法は、プラス記号のアイコンをクリックし、Factory リソースのウィンドウで **[データ フロー]** を選択することです。

![ラングリング](media/wrangling-data-flow/tutorial7.png)

もう 1 つの方法は、パイプライン キャンバスのアクティビティ ウィンドウで行います。 **[移動と変換]** アコーディオンを開き、 **[データ フロー]** アクティビティをキャンバスにドラッグします。

どちらの方法でも、開いたサイド ウィンドウで **[Create new data flow]\(新しいデータ フローの作成\)** を選択し、 **[Wrangling data flow]\(ラングリング データ フロー\)** を選択します。 [OK] をクリックします。

![ラングリング](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>ラングリング データ フローを作成する

ラングリング データ フロー用の**ソース データセット** を追加します。 既存のデータセットを選択するか、新しいデータセットを作成することができます。 シンク データセットを選択することもできます。 1 つまたは複数のソース データセットを選択できますが、現時点では 1 つのシンクのみが許可されます。 シンク データセットの選択は省略可能ですが、少なくとも 1 つのソース データセットが必要です。

> [!NOTE]
> 限定プレビューでは、ADLS Gen 2 区切りテキストのみがサポートされています。 

![ラングリング](media/wrangling-data-flow/tutorial4.png)

**[作成]** をクリックして、Power Query Online マッシュアップ エディターを開きます。

![ラングリング](media/wrangling-data-flow/tutorial5.png)

コーディング不要のデータ準備を使用して、ラングリング データ フローを作成します。 使用できる関数の一覧については、[変換関数](wrangling-data-flow-functions.md)/に関するページを参照してください

![ラングリング](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>ラングリング データ フローを実行して監視する

ラングリング データ フローのパイプライン デバッグ実行を実行するには、パイプライン キャンバスで **[デバッグ]** をクリックします。 データ フローを発行すると、 **[Trigger Now]\(今すぐトリガー\)** によって、最後に発行されたパイプラインのオンデマンド実行が実行されます。 ラングリング データ フローは、既存のすべての Azure Data Factory トリガーを使用してスケジュールできます。

![ラングリング](media/wrangling-data-flow/tutorial3.png)

**[監視]** タブに移動し、トリガーされたラングリング データ フロー アクティビティの実行の出力を視覚化します。

![ラングリング](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>次の手順

[マッピング データ フローの作成](tutorial-data-flow.md)方法について確認します。
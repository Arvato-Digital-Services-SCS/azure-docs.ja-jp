---
title: Azure Monitor ブックとテキスト パラメーターを使用した対話型レポートの作成 | Microsoft Docs
description: 作成済みのブックやパラメーター化されたカスタム ブックを使用して複雑なレポート作成を簡素化します。 ブックのテキスト パラメーターの詳細について学習します。
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: ee3e24444b87c461841b591176774d4e945e4fcc
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164205"
---
# <a name="workbook-text-parameters"></a>ブックのテキスト パラメーター

テキスト ボックス パラメーターを使用すると、ブック ユーザーからテキスト入力を簡単に収集できます。 これらは、ドロップダウンを使用して入力を収集することが実用的でない場合に使用されます (たとえば、任意のしきい値または汎用フィルター)。 ブックでは、作成者は、クエリからテキスト ボックスの既定値を取得できます。 これにより、メトリックの p95 に基づいて既定のしきい値を設定するなどの興味深いシナリオが可能になります。

テキスト ボックスは、一般的に、他のブック コントロールによって使用される内部変数として使用します。 これは、既定値を求めるクエリを利用し、入力コントロールを読み取りモードで非表示にすることによって実現されます。 たとえば、しきい値を (ユーザーが入力するのではなく) 数式から取得した後、後続のクエリでそのしきい値を使用することができます。

## <a name="creating-a-text-parameter"></a>テキスト パラメーターの作成
1. 編集モードの空白のブックを使用して開始します。
2. ブック内のリンクから _[Add parameters]\(パラメーターの追加\)_ を選択します。
3. 青い _[パラメーターの追加]_ ボタンをクリックします。
4. ポップアップ表示される新しいパラメーター ペインで入力を行います。
    1. Parameter name: `SlowRequestThreshold` (パラメーター名: {2})
    2. [パラメーターの種類]\: [テキスト]
    3. [必須ですか?]\: オン
    4. [クエリから既定値を取得します]\: オフ
5. ツール バーの [保存] を選択して、パラメーターを作成します。

    ![テキスト パラメーターの作成を示す画像](./media/workbooks-text/text-create.png)

ブックが読み取りモードでどのように表示されるかを次に示します。

![読み取りモードのテキスト パラメーターを示す画像](./media/workbooks-text/text-readmode.png)

## <a name="referencing-a-text-parameter"></a>テキスト パラメーターの参照
1. 青い [クエリの追加] リンクを選択してブックにクエリ コントロールを追加し、Application Insights リソースを選択します。
2. KQL ボックスに、このスニペットを追加します。
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= {SlowRequestThreshold}) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
3. 値 500 を指定したテキスト パラメーターをクエリ コントロールと組み合わせて使用することにより、次のクエリを効果的に実行できます。
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= 500) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
4. クエリを実行し、結果を確認します

    ![KQL で参照されるテキスト パラメーターを示す画像](./media/workbooks-text/text-reference.png)


## <a name="setting-default-values"></a>既定値の設定
1. 編集モードの空白のブックを使用して開始します。
2. ブック内のリンクから _[Add parameters]\(パラメーターの追加\)_ を選択します。
3. 青い _[パラメーターの追加]_ ボタンをクリックします。
4. ポップアップ表示される新しいパラメーター ペインで入力を行います。
    1. Parameter name: `SlowRequestThreshold` (パラメーター名: {2})
    2. [パラメーターの種類]\: [テキスト]
    3. [必須ですか?]\: オン
    4. [クエリから既定値を取得します]\: オン
5. KQL ボックスに、このスニペットを追加します。
    ```kusto
    requests
    | summarize round(percentile(duration, 95), 2)
    ```
    このクエリでは、テキスト ボックスの既定値を、アプリ内のすべての要求の 95 パーセンタイル期間に設定します。
6. クエリを実行し、結果を確認します
7. ツール バーの [保存] を選択して、パラメーターを作成します。

    ![KQL の既定値を含むテキスト パラメーターを示す画像](./media/workbooks-text/text-default-value.png)

> [!NOTE]
> この例では Application Insights データを照会していますが、このアプローチは、任意のログ ベースのデータ ソース (Log Analytics、Azure Resource Graph など) に使用できます。

## <a name="next-steps"></a>次の手順

* ブックの豊富な視覚化オプションの学習を[開始](workbooks-visualizations.md)します。
* ブック リソースへのアクセスを[制御](workbooks-access-control.md)し、共有します。

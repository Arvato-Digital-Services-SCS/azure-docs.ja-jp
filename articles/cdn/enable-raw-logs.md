---
title: Azure CDN HTTP 生ログ
description: この記事では、Azure CDN HTTP 生ログについて説明します。
services: cdn
author: sohamnchatterjee
manager: danielgi
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/23/2020
ms.author: sohamnc
ms.openlocfilehash: a2522eba17574246ab99a0d47a42f128d5f61ace
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "84888648"
---
# <a name="azure-cdn-http-raw-logs"></a>Azure CDN HTTP 生ログ
生ログは、監査やトラブルシューティングにとって重要な操作とエラーに関する豊富な情報を提供します。 生ログは、アクティビティ ログとは異なります。 アクティビティ ログは、Azure リソースに対して行われた操作を可視化します。 生ログからは、リソースの操作記録が提供されます。

> [!IMPORTANT]
> HTTP 生ログ機能は、Microsoft の Azure CDN からご利用いただけます。

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。 

## <a name="sign-in-to-azure"></a>Azure へのサインイン

Azure Portal [https://portal.azure.com](https://portal.azure.com) にサインインします。

## <a name="configuration"></a>構成

Microsoft プロファイルから Azure CDN の生ログを構成するには: 

1. Azure portal メニューから、 **[すべてのリソース]**  >>  **\<your-CDN-profile>** を選択します。

2. **[監視]** で **[診断設定]** を選択します。

3. **[+ 診断設定の追加]** を選択します。

    ![CDN 診断設定](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > 生ログは、集計された HTTP ステータス コード ログがエンドポイント レベルで利用できるときにのみ、プロファイル レベルで利用できます。

4. **[診断設定]** の **[診断設定の名前]** で診断設定の名前を入力します。

5. **ログ**を選択し、保有期間を日数で設定します。

6. **[宛先の詳細]** を選択します。 出力先の選択肢:
    * **Log Analytics への送信**
        * **サブスクリプション**と **Log Analytics ワークスペース**を選択します。
    * **ストレージ アカウントへのアーカイブ**
        * **サブスクリプション**と**ストレージ アカウント**を選択します。
    * **イベント ハブへのストリーム**
        * **[サブスクリプション]** 、 **[イベント ハブの名前空間]** 、 **[イベント ハブ名 (オプション)]** 、 **[イベント ハブ ポリシー名]** を選択します。

    ![CDN 診断設定](./media/cdn-raw-logs/raw-logs-02.png)

7. **[保存]** を選択します。

## <a name="raw-logs-properties"></a>生ログのプロパティ

Microsoft サービスの Azure CDN は現在、生ログを提供しています。 生ログでは、次のスキーマを使用した各エントリが個々の API 要求に提供されます。 

| プロパティ              | 説明                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Front Door によって提供された要求を識別する一意の参照文字列。X-Azure-Ref ヘッダーとしてクライアントにも送信されます。 アクセス ログで特定の要求の詳細を検索するために必要です。 |
| HttpMethod            | 要求で使用される HTTP メソッド。                                                                                                                                                                     |
| HttpVersion           | 要求または接続の種類。                                                                                                                                                                   |
| RequestUri            | 受信した要求の URI。                                                                                                                                                                         |
| RequestBytes          | 要求ヘッダーと要求本文を含む HTTP 要求メッセージのサイズ (バイト単位)。                                                                                                   |
| ResponseBytes         | 応答としてバックエンド サーバーによって送信されたバイト数。                                                                                                                                                    |
| UserAgent             | クライアントで使用されたブラウザーの種類。                                                                                                                                                               |
| ClientIp              | 要求を行ったクライアントの IP アドレス。                                                                                                                                                  |
| TimeTaken             | アクションにかかった時間の長さ (ミリ秒単位)。                                                                                                                                            |
| SecurityProtocol      | 要求によって使用された TLS/SSL プロトコルのバージョン。暗号化がない場合は、null 値。                                                                                                                           |
| エンドポイント              | CDN エンドポイント ホストは、親 CDN プロファイルで構成されています。                                                                                                                                   |
| Backend Host name     | 要求が送信されているバックエンド ホストまたはオリジンの名前。                                                                                                                                |
| Sent to origin shield | true の場合、要求は、エッジ ポップではなく、オリジン シールド キャッシュから応答されました。 オリジン シールドは、キャッシュ ヒット率を向上させる目的で使用される親キャッシュです。                                       |
| HttpStatusCode        | プロキシから返された HTTP 状態コード。                                                                                                                                                        |
| HttpStatusDetails     | 要求の結果の状態。 この文字列値の意味は、状態参照テーブルで確認できます。                                                                                              |
| Pop                   | ユーザー要求に応答したエッジ ポップ。 POP の略語はそれぞれの都市の空港コードです。                                                                                   |
| Cache Status          | オブジェクトがキャッシュから返されたか、配信元から届いたかを意味します。                                                                                                             |
> [!IMPORTANT]
> HTTP 生ログ機能は、**2020 年 2 月 25 日**以降に作成または更新されたプロファイルで自動的に使用可能になります。 それよりも前に作成した CDN プロファイルの場合、ログ記録のセットアップ後に CDN エンドポイントを更新する必要があります。 たとえば、CDN エンドポイントで geo フィルタリングに移動し、そのワークロードに関係のない国および地域をブロックし、保存することができます。 

> [!NOTE]
> クエリを実行することによって、Log Analytics プロファイルでログを表示できます。 クエリの例は次のとおりです。              AzureDiagnostics | where Category == "AzureCdnAccessLog"

## <a name="next-steps"></a>次の手順
この記事では、Microsoft CDN サービスの HTTP 生ログを有効にしました。

Azure CDN とこの記事で言及しているその他の Azure サービスの詳細については、次をご覧ください。

* Azure CDN の使用パターンを[分析する](cdn-log-analysis.md)。

* 詳細については、「[Azure Monitor の概要](https://docs.microsoft.com/azure/azure-monitor/overview)」を参照してください。

* [Azure Monitor で Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) を構成する。

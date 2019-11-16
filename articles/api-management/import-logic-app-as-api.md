---
title: Azure Portal を使用してロジック アプリを API としてインポートする | Microsoft Docs
description: このチュートリアルでは、API Management (APIM) を使用してロジック アプリを API としてインポートする方法を示します。
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: apimpm
ms.openlocfilehash: 4077187fe04e3be914a6f7fba84c03df1b79d06a
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "74108395"
---
# <a name="import-a-logic-app-as-an-api"></a>ロジック アプリを API としてインポート

この記事では、ロジック アプリを API としてインポートし、そのインポート済みの API をテストする方法について説明します。

この記事では、次のことについて説明します。

> [!div class="checklist"]
>
> -   ロジック アプリを API としてインポート
> -   Azure Portal での API のテスト
> -   開発者ポータルでの API のテスト

## <a name="prerequisites"></a>前提条件

-   次のクイック スタートを完了すること:[Azure API Management インスタンスを作成する](get-started-create-service-instance.md)
-   HTTP エンドポイントが公開されるサブスクリプションにロジック アプリがあることを確認します。 詳しくは、[HTTP エンドポイントでのワークフローのトリガー](../logic-apps/logic-apps-http-endpoint.md)に関する記事をご覧ください

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>バックエンド API のインポートと公開

1. **[API Management]** で **[API]** を選択します。
2. **[Add a new API]\(新しい API の追加\)** の一覧から **[ロジック アプリ]** を選択します。

    ![ロジック アプリ](./media/import-logic-app-as-api/logic-app-api.png)

3. **[参照]** を押して、サブスクリプション内の、HTTP トリガーを持つロジック アプリの一覧を表示します (HTTP トリガーを持たないロジック アプリは一覧に表示されないことに注意してください)。
4. アプリを選びます。 API Management では、選択したアプリに関連付けられている Swagger が検索されてフェッチされ、インポートされます。
5. API URL サフィックスを追加します。 サフィックスは、この API Management インスタンスでこの特定の API を識別する名前です。 この API Management インスタンス内で一意である必要があります。
6. API を成果物に関連付けることで API を公開します。 この場合、"_無制限_" の成果物が使用されます。 API を公開して開発者が利用できるようにするには、その API を成果物に追加します。 API の作成時に行うことも、後で設定することもできます。

    製品には、1 つまたは複数の API が関連付けられています。 複数の API を組み込み、開発者ポータルを通じてそれらを開発者に提供できます。 開発者は、まず成果物をサブスクライブして API へのアクセス権を取得する必要があります。 サブスクライブすると、その製品の API に適したサブスクリプション キーを受け取ります。 API Management インスタンスを作成した場合は、既に管理者になっているため、既定ですべての製品をサブスクライブしています。

    すべての API Management インスタンスは、2 つのサンプル成果物を既定で備えています。

    - **スターター**
    - **無制限**

7. **作成** を選択します。

## <a name="test-the-api-in-the-azure-portal"></a>Azure Portal での API のテスト

Azure Portal には、API の操作を表示およびテストするための便利な環境が用意されており、操作を直接呼び出すことができます。

1. 前の手順で作成した API を選びます。
2. **[テスト]** タブをクリックします。
3. いくつかの操作を選びます。

    ページに、クエリ パラメーターのフィールドとヘッダーのフィールドが表示されます。 この API に関連付けられている成果物のサブスクリプション キーの場合、ヘッダーの 1 つは "Ocp-Apim-Subscription-Key" です。 API Management インスタンスを作成した場合は、既に管理者になっているので、キーが自動的に入力されます。

4. **[送信]** をクリックします。

    バックエンドは **200 OK** といくつかのデータで応答します。

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>すべてのロジック アプリには、**手動呼び出し**操作があります。 複数ロジック アプリの API を含める場合は、競合を回避するために、関数の名前を変更する必要があります。

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
>
> [公開された API の変換と保護](transform-api.md)

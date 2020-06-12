---
title: SQL Server または Azure SQL Database に接続する
description: Azure Logic Apps を使用して、オン プレミスまたはクラウド内の SQL データベースに関するタスクを自動化します
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/12/2020
tags: connectors
ms.openlocfilehash: f63553ced8484b3ce328fb9537d5831ae1e27fe8
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2020
ms.locfileid: "84191489"
---
# <a name="automate-workflows-for-sql-server-or-azure-sql-database-by-using-azure-logic-apps"></a>Azure Logic Apps を使用して SQL Server または Azure SQL Database のワークフローを自動化する

この記事では、SQL Server コネクタを使用してロジック アプリの中から SQL データベースのデータにアクセスする方法を示します。 ロジック アプリを作成することによって、SQL データとリソースを管理するタスク、プロセス、ワークフローを自動化できます。 SQL Server コネクタは、[SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) だけでなく、[Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md) と [Azure SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md) でも機能します。

SQL データベースや Dynamics CRM Online などの他のシステム内のイベントによってトリガーされたときに実行されるロジック アプリを作成できます。 ロジック アプリは、データの取得、挿入、削除のほか、SQL クエリやストアド プロシージャを実行することもできます。 たとえば、Dynamics CRM Online の新しいレコードを自動的に確認し、新しいレコード用の項目を SQL データベースに追加した後、追加した項目に関する電子メール アラートを送信するロジック アプリをビルドできます。

ロジック アプリを初めて使用する場合は、「[Azure Logic Apps とは](../logic-apps/logic-apps-overview.md)」と[クイック スタートの初めてのロジック アプリの作成](../logic-apps/quickstart-create-first-logic-app-workflow.md)に関するページを参照してください。 コネクタ固有の技術情報、制限事項、既知の問題については、[SQL Server コネクタ リファレンスのページ](https://docs.microsoft.com/connectors/sql/)をご覧ください。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。 サブスクリプションをお持ちでない場合には、[無料の Azure アカウントにサインアップ](https://azure.microsoft.com/free/)してください。

* [SQL Server データベース](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database)または [Azure SQL データベース](../azure-sql/database/single-database-create-quickstart.md)

  テーブルには、ロジック アプリが操作を呼び出したときに結果を返すことができるように、データが含まれている必要があります。 Azure SQL Database を作成する場合は、用意されているサンプル データベースを使用できます。

* SQL サーバー名、データベース名、ユーザー名およびパスワード。 ロジックを承認して SQL Server にアクセスできるようにするには、これらの資格情報が必要です。

  * SQL Server の場合、これらの詳細は、接続文字列で確認できます。

    `Server={your-server-address};Database={your-database-name};User Id={your-user-name};Password={your-password};`

  * Azure SQL Database の場合、これらの詳細は、接続文字列か、Azure Portal の SQL Database のプロパティで確認できます。

    `Server=tcp:{your-server-name}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

* ローカル コンピューターにインストールされた[オンプレミス データ ゲートウェイ](../logic-apps/logic-apps-gateway-install.md)と以下のシナリオのために [Azure portal で作成された Azure データ ゲートウェイ リソース](../logic-apps/logic-apps-gateway-connection.md):

  * ロジック アプリは、[統合サービス環境 (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) では実行されません。

  * ロジック アプリは統合サービス環境で*実行されます*が、SQL Server 接続には Windows 認証を使用する必要があります。 ISE バージョンは Windows 認証をサポートしていないため、このシナリオでは、データ ゲートウェイと共に SQL Server コネクタの非 ISE バージョンを使用します。

* SQL データベースにアクセスする必要があるロジック アプリ。 SQL トリガーを使用してロジック アプリを起動するには、[空のロジック アプリ](../logic-apps/quickstart-create-first-logic-app-workflow.md)が必要です。

<a name="add-sql-trigger"></a>

## <a name="add-a-sql-trigger"></a>SQL トリガーを追加する

Azure Logic Apps では、すべてのロジック アプリは、必ず[トリガー](../logic-apps/logic-apps-overview.md#logic-app-concepts)から起動されます。トリガーは、特定のイベントが起こるか特定の条件が満たされたときに発生します。 トリガーが発生するたびに、Logic Apps エンジンによってロジック アプリ インスタンスが作成され、ロジック アプリのワークフローが開始されます。

1. Azure Portal または Visual Studio で、Logic Apps デザイナーを開いて、空のロジック アプリを作成します。 この例では、Azure Portal を使用します。

1. デザイナーの検索ボックスに、フィルターとして「sql server」と入力します。 トリガーの一覧から、使用する SQL トリガーを選択します。

   この例では、 **[項目が作成されたとき]** トリガーを使用します。

   ![[項目が作成されたとき] トリガーの選択](./media/connectors-create-api-sqlazure/select-sql-server-trigger.png)

1. 接続の作成を求められたら、[SQL 接続を今すぐ作成](#create-connection)します。 接続が存在する場合は、 **[テーブル名]** を選択します。

   ![使用するテーブルの選択](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

1. **［間隔］** プロパティと **［頻度］** を設定します。これらは、ロジック アプリがテーブルをチェックする頻度を指定します。

   このトリガーでは、選択したテーブルから 1 行のみを返し、それ以外は何もしません。 他のタスクを実行するには、必要なタスクを実行する他のアクションを追加します。 たとえば、この行のデータを表示するには、返された行のフィールドを含むファイルを作成する他のアクションを追加し、電子メール通知を送信します。 このコネクタで使用できるその他のアクションの詳細については、[コネクタのリファレンス ページ](https://docs.microsoft.com/connectors/sql/)を参照してください。

1. 操作が完了したら、デザイナーのツールバーで、 **[保存]** を選択します。

   この手順によって、ロジック アプリが自動的に有効化され、Azure に発行されます。

<a name="add-sql-action"></a>

## <a name="add-a-sql-action"></a>SQL アクションを追加する

Azure Logic Apps では、[アクション](../logic-apps/logic-apps-overview.md#logic-app-concepts)とは、トリガーまたは別のアクションに続くワークフロー内のステップです。 この例では、ロジック アプリは、[繰り返しトリガー](../connectors/connectors-native-recurrence.md)で起動され、SQL データベースから行を取得するアクションを呼び出します。

1. Azure Portal または Visual Studio で、Logic Apps デザイナーでロジック アプリを開きます。 この例では、Azure Portal を使用します。

1. SQL アクションを追加するトリガーまたはアクションで、 **[新しいステップ]** を選択します。

   ![ロジック アプリに新しいステップを追加する](./media/connectors-create-api-sqlazure/select-new-step-logic-app.png)

   既存のステップの間にアクションを追加するには、接続矢印の上にマウスを移動します。 表示されるプラス記号 ( **+** ) を選択してから、 **[アクションの追加]** を選択します。

1. **[アクションを選択してください]** の下の検索ボックス内に、フィルターとして「sql server」と入力します。 アクションの一覧から、使用する SQL アクションを選択します。

   この例では、1 つのレコードを取得する **[行の取得]** アクションを使用します。

   ![SQL の [行の取得] アクションを検索して選択](./media/connectors-create-api-sqlazure/find-select-sql-get-row-action.png)

   このアクションでは、選択したテーブルから 1 行のみが返され、それ以外は何も行われません。 この行のデータを表示するには、返される行のフィールドが含まれるファイルを作成し、そのファイルをクラウド ストレージ アカウントに格納するその他のアクションを追加します。 このコネクタで使用できるその他のアクションの詳細については、[コネクタのリファレンス ページ](https://docs.microsoft.com/connectors/sql/)を参照してください。

1. 接続の作成を求められたら、[SQL 接続を今すぐ作成](#create-connection)します。 接続が存在する場合は、 **[テーブル名]** を選択し、使用するレコードの **[行 ID]** を入力します。

   ![テーブル名と行 ID の入力](./media/connectors-create-api-sqlazure/specify-table-row-id-property-value.png)

1. 操作が完了したら、デザイナーのツールバーで、 **[保存]** を選択します。

   この手順によって、ロジック アプリが自動的に有効化され、Azure に発行されます。

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>データベースに接続する

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>一括データを処理します。

サイズが大きすぎてコネクタが同時にすべての結果を返さない結果セットを操作する必要がある場合、または、結果セットのサイズと構造を詳細に制御したい場合があります。 このような大きな結果セットを処理する方法は、いくつかあります。

* 結果を、より小さなセットとして管理しやすくするには、*改ページ位置の自動修正*をオンします。 詳細は、「[Get bulk data, records, and items by using pagination](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md)」(改ページ位置の自動修正を使用した一括データ、レコードおよび項目) を参照してください。

* 希望どおりの結果を編成するストアド プロシージャを作成します。

  複数の行を取得または挿入する場合、ロジック アプリは、こちらの[制限](../logic-apps/logic-apps-limits-and-config.md)の中で "[*until ループ*](../logic-apps/logic-apps-control-flow-loops.md#until-loop)" を使用することで、行を反復処理できます。 ただし、ロジック アプリは、数千から数百万の行がある非常に大きなレコード セットを処理する場合があります。このような場合は、データベースへの呼び出しコストを最小限にする必要があります。

  代わりに、SQL インスタンスで実行され、**SELECT - ORDER BY**ステートメントを使用して、望みどおりの方法で結果を整理する "[*ストアド プロシージャ*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine)" を作成できます。 このソリューションでは、結果のサイズと構造を詳細に制御できます。 ロジック アプリは、SQL Server コネクタの **［ストアド プロシージャの実行］** アクションを使用して、ストアド プロシージャを呼び出します。

  ソリューションの詳細については、次の記事を参照してください。

  * [Logic Apps での一括データ転送に対する SQL の改ページ処理](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SELECT - ORDER BY Clause](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

### <a name="handle-dynamic-bulk-data"></a>動的な一括データを処理する

SQL Server コネクタでストアド プロシージャを呼び出すと、返される出力は動的になることがあります。 このシナリオでは、次の手順を実行します。

1. **Logic Apps デザイナー**を開きます。
1. ロジック アプリのテストの実行を実行して、出力形式を確認します。 サンプル出力をコピーします。
1. デザイナーで、ストアド プロシージャを呼び出す操作の下にある **[新しいステップ]** を選択します。
1. **[アクションの選択]** で、[ **[JSON の解析]** ](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action) アクションを検索して選択します。
1. **[JSON の解析]** アクションで、 **[サンプルのペイロードを使用してスキーマを生成する]** を選択します。
1. **[サンプルの JSON ペイロードを入力するか、貼り付けます]** ウィンドウで、サンプル出力を貼り付け、 **[完了]** を選択します。
1. Logic Apps がスキーマを生成できないというエラーが発生した場合は、サンプル出力の構文が正しい形式であることを確認してください。 それでもスキーマを生成できない場合は、 **[スキーマ]** ボックスに手動で入力します。
1. デザイナーのツール バーで、 **[保存]** を選択します。
1. JSON コンテンツ プロパティにアクセスするには、[ **[JSON の解析]** アクション](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action)の下の動的コンテンツ一覧に表示されるデータ トークンを使用します。

## <a name="connector-specific-details"></a>コネクタ固有の詳細

このコネクタのトリガー、アクション、および制限に関する技術情報については、[コネクタのリファレンス ページ](https://docs.microsoft.com/connectors/sql/)を参照してください。

## <a name="next-steps"></a>次のステップ

* [Azure Logic Apps の他のコネクタ](../connectors/apis-list.md)の詳細情報

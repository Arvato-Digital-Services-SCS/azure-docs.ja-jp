---
title: Azure portal での Azure Database for MariaDB のサーバー ログの構成とアクセス
description: この記事では、Azure portal から Azure Database for MariaDB のサーバー ログの構成方法とアクセス方法について説明します。
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c8be9519d3393330b3022fadd2de6a49e58ecdcf
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703512"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Azure Portal でのサーバー ログの構成とアクセス

Azure portal から [Azure Database for MariaDB の低速クエリ ログ](concepts-server-logs.md)の構成、一覧表示、ダウンロードを行うことができます。

## <a name="prerequisites"></a>前提条件
このハウツー ガイドの手順を実行するには、以下が必要です。
- [Azure Database for MariaDB サーバー](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>ログの構成
低速クエリ ログへのアクセスを構成します。 

1. [Azure Portal](https://portal.azure.com/) にサインインします。

2. Azure Database for MariaDB サーバーを選択します。

3. サイドバーの **[監視]** セクションの下で、 **[サーバー ログ]** を選択します。 
   ![サーバー ログを選択し、クリックして構成](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. サーバー パラメーターを表示するには、見出しの **[ログを有効にし、ログ パラメーターを構成するには、ここをクリックしてください]** を選択します。

5. 調整する必要のあるパラメーターを変更します ("slow_query_log" を "ON" にする、など)。 このセッションで行ったすべての変更が紫色で強調表示されます。 

   パラメーターを変更すると、 **[保存]** をクリックできます。 または変更を **[破棄]** することができます。

   ![[保存] または [破棄] をクリック](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. ログのリストに戻るには、 **[サーバー パラメーター]** ページの**閉じるボタン** (X のアイコン) をクリックします。

## <a name="view-list-and-download-logs"></a>リストの表示とログのダウンロード
ログ記録が開始されると、使用可能な低速クエリ ログの一覧を表示したり、[サーバー ログ] ウィンドウで個々のログ ファイルをダウンロードしたりすることができます。 

1. Azure Portal を開きます。

2. Azure Database for MariaDB サーバーを選択します。

3. サイドバーの **[監視]** セクションの下で、 **[サーバー ログ]** を選択します。 ページには、次のようにログ ファイルのリストが表示されます。

   ![ログのリスト](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > ログの名前付け規則は、 **-< your server name>-yyyymmddhh.log** です。 ファイル名に使用される日時は、ログが発行された時間です。 ログ ファイルのローテーションは、24 時間ごとか 7.5 GB ごとのどちらか早い方のタイミングで行われます。

4. 必要に応じて、**検索ボックス**を使用して、日付/時刻に基づいて特定のログをすばやく絞り込みます。 検索はログの名前に対して行われます。

5. 図に示されているように、表の行内の各ログ ファイルの横にある**ダウンロード** ボタン (下向き矢印のアイコン) を使用して、個々のログ ファイルをダウンロードします。

   ![ダウンロード アイコンをクリック](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>診断ログの設定

1. サイドバーの **[監視]** セクションの下で、 **[診断設定]** を選択します。

1. [+ Add diagnostic setting] (診断設定の追加) をクリックします。![診断設定の追加](./media/howto-configure-server-logs-portal/add-diagnostic-setting.png)

1. 診断設定の名前を指定します。

1. どのデータ シンク (ストレージ アカウント、イベント ハブ、Log Analytics ワークスペース) に低速クエリ ログを送信するか指定します。

1. ログの種類として "MySqlSlowLogs" を選択します。
![診断設定の構成](./media/howto-configure-server-logs-portal/configure-diagnostic-setting.png)

1. 低速クエリ ログをパイプするようにデータ シンクを設定したら、 **[保存]** をクリックすることができます。
![診断設定の保存](./media/howto-configure-server-logs-portal/save-diagnostic-setting.png)

1. 構成したデータ シンクを調べて低速クエリ ログにアクセスします。 ログが表示されるまでに最大で 10 分かかる可能性があります。

## <a name="next-steps"></a>次の手順
- [CLI で低速クエリ ログにアクセスする](howto-configure-server-logs-cli.md)に関する記事を参照して、プログラムで低速クエリ ログをダウンロードする方法について学習します。
- Azure Database for MariaDB の[低速クエリ ログ](concepts-server-logs.md)の詳細について学習します。
- パラメーターの定義とログ記録の詳細については、[ログ](https://mariadb.com/kb/en/library/slow-query-log-overview/) に関するページの MariaDB のドキュメントを参照してください。
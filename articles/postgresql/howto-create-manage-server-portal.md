---
title: Azure Database for PostgreSQL を管理する - Azure portal
description: Azure portal から Azure Database for PostgreSQL サーバーを管理する方法について説明します。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/20/2019
ms.openlocfilehash: bcddd64afca29ac9fdd5d284fc8f809ff9e2477d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "74534887"
---
# <a name="manage-an-azure-database-for-postgresql-server-using-the-azure-portal"></a>Azure portal を使用して Azure Database for PostgreSQL サーバーを管理する
この記事では、Azure Database for PostgreSQL サーバーを管理する方法を示します。 管理タスクには、コンピューティングとストレージのスケーリング、管理者パスワードのリセット、サーバーの詳細の表示が含まれます。

## <a name="sign-in"></a>サインイン
[Azure portal](https://portal.azure.com) にサインインします。

## <a name="create-a-server"></a>サーバーの作成
Azure Database for PostgreSQL サーバーを作成して実行する方法については、[クイックスタート](quickstart-create-server-database-portal.md)を参照してください。

## <a name="scale-compute-and-storage"></a>コンピューティングとストレージのスケーリング

サーバー作成後は、ニーズの変化に応じて、汎用レベルとメモリ最適化レベルの間でスケーリングできます。 また、仮想コアを増減することによって、コンピューティングとメモリのスケーリングもできます。 ストレージをスケールアップすることはできますが、ストレージをスケールダウンすることはできません。

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>汎用レベルとメモリ最適化レベルの間のスケーリング

汎用からメモリ最適化 (またはその逆) にスケーリングできます。 サーバー作成後の Basic レベルへの変更や Basic レベルからの変更はサポートされていません。 

1. お使いのサーバーを Azure portal で選択します。 **[設定]** セクションにある **[価格レベル]** を選択します。

2. スケーリングの対象に応じて、 **[汎用]** または **[メモリ最適化]** を選択します。 

    ![change-pricing-tier](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > レベルを変更すると、サーバーが再起動されます。

4. **[OK]** を選択して変更を保存します。


### <a name="scale-vcores-up-or-down"></a>仮想コアのスケールアップまたはスケールダウン

1. お使いのサーバーを Azure portal で選択します。 **[設定]** セクションにある **[価格レベル]** を選択します。

2. スライダーを目的の値に動かして、 **[vCore]** の設定を変更します。

    ![scale-compute](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > 仮想コアをスケーリングすると、サーバーが再起動されます。

3. **[OK]** を選択して変更を保存します。


### <a name="scale-storage-up"></a>ストレージのスケールアップ

1. お使いのサーバーを Azure portal で選択します。 **[設定]** セクションにある **[価格レベル]** を選択します。

2. スライダーを目的の値まで上に動かして、 **[ストレージ]** の設定を変更します。

    ![scale-storage](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > ストレージはスケールダウンできません。

3. **[OK]** を選択して変更を保存します。


## <a name="update-admin-password"></a>管理パスワードの更新
Azure portal を使用して、管理者ロールのパスワードを変更できます。

1. お使いのサーバーを Azure portal で選択します。 **[概要]** ウィンドウで **[パスワードのリセット]** を選択します。

   ![概要](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. 新しいパスワードを入力し、そのパスワードを確認します。 テキストボックスに、パスワードの複雑さの要件についてのメッセージが表示されます。

   ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)

3. **[OK]** を選択して新しいパスワードを保存します。


## <a name="delete-a-server"></a>サーバーの削除

不要になったサーバーは削除できます。 

1. お使いのサーバーを Azure portal で選択します。 **[概要]** ウィンドウで **[削除]** を選択します。

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. 入力ボックスにサーバーの名前を入力して、削除するサーバーであることを確認します。

    ![confirm-delete](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > サーバーを削除すると、元に戻せません。

3. **[削除]** を選択します。


## <a name="next-steps"></a>次のステップ
- [バックアップとサーバー復元](howto-restore-server-portal.md)について学習する
- [Azure Database for PostgreSQL のチューニングと監視のオプション](concepts-monitoring.md)について学ぶ

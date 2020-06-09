---
title: FAQ - Azure VM 上の SAP HANA データベースのバックアップ
description: この記事では、Azure Backup サービスを使用した SAP HANA データベースのバックアップに関する一般的な質問への回答を示します。
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 56f98dddb00eb3ffc87eb27da73066de807a1ee1
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2020
ms.locfileid: "83701011"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>よく寄せられる質問 - Azure VM 上の SAP HANA データベースをバックアップする

この記事では、Azure Backup サービスを使用した SAP HANA データベースのバックアップに関する一般的な質問への回答を示します。

## <a name="backup"></a>バックアップ

### <a name="how-many-full-backups-are-supported-per-day"></a>1 日あたり何回の完全バックアップがサポートされていますか?

サポートされている完全バックアップは 1 日 1 回のみです。 同じ日に差分バックアップと完全バックアップをトリガーすることはできません。

### <a name="do-successful-backup-jobs-create-alerts"></a>成功したバックアップ ジョブでアラートが作成されますか?

いいえ。 成功したバックアップ ジョブではアラートは生成されません。 アラートは、失敗したバックアップ ジョブに対してのみ送信されます。 ポータル アラートの詳細な動作は[ここ](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor)に記載されています。 ただし、成功したジョブに対してもアラートを生成することに関心がある場合は、[Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor) を使用できます。

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>スケジュールされたバックアップ ジョブを [バックアップ ジョブ] メニューで確認できますか?

[バックアップ ジョブ] メニューには、アドホック バックアップ ジョブしか表示されません。 スケジュールされたジョブの場合は、[Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor) を使用します。

### <a name="are-future-databases-automatically-added-for-backup"></a>今後作成されるデータベースはバックアップに自動的に追加されますか?

いいえ、これは現在サポートされていません。

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>インスタンスからデータベースを削除した場合、バックアップはどうなりますか?

SAP HANA インスタンスからデータベースが削除された場合でも、データベース バックアップは引き続き試行されます。 これは、削除されたデータベースが **[バックアップ項目]** に異常として表示され始め、引き続き保護されていることを示しています。
このデータベースの保護を停止するための正しい方法は、このデータベースに対して "**バックアップを停止してデータを削除する**" を実行することです。

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>データベースが保護された後にその名前を変更した場合、その動作はどうなりますか?

名前が変更されたデータベースは、新しいデータベースとして処理されます。 このため、サービスはこの状況を、データベースが見つからず、バックアップが失敗したかのように処理します。 名前が変更されたデータベースは新しいデータベースとして表示されるため、保護するように構成する必要があります。

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Azure VM 上の SAP HANA データベースをバックアップするための前提条件は何ですか?

「[前提条件](tutorial-backup-sap-hana-db.md#prerequisites)」セクションと[事前登録スクリプトの内容](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)に関するセクションを参照してください。

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>Azure で SAP HANA データベースをバックアップできるようにするには、どのようなアクセス許可を設定する必要がありますか?

事前登録スクリプトを実行すると、Azure で SAP HANA データベースをバックアップできるようにするために必要なアクセス許可が設定されます。 事前登録スクリプトで実行される処理の詳細については、[こちら](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)をご覧ください。

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>SAP HANA を SDC から MDC に移行した後、バックアップは機能しますか?

トラブルシューティング ガイドの[こちらのセクション](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#sdc-to-mdc-upgrade-with-a-change-in-sid)をご覧ください。

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>仮想マシンではなく、仮想 IP (ロード バランサー) に対して Azure HANA バックアップを設定できますか?

現時点では、仮想 IP のみに対してソリューションを設定することはできません。 ソリューションを実行するには、仮想マシンが必要です。

### <a name="i-have-a-sap-hana-system-replication-hsr-how-should-i-configure-backup-for-this-setup"></a>SAP HANA システム レプリケーション (HSR) を使用しています。この設定のバックアップはどのように構成すればよいですか?

HSR のプライマリおよびセカンダリのノードは、関連のない 2 つの個別の VM として扱われます。 バックアップはプライマリ ノードで構成する必要があります。また、フェールオーバーが発生したときに、セカンダリ ノード (これがプライマリ ノードになります) でバックアップを構成する必要があります。 他のノードに対するバックアップの自動 "フェールオーバー" はありません。

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>オンデマンド バックアップを Azure コンテナーではなくローカル ファイル システムに移動する方法はありますか?

1. 目的のデータベースで現在実行中のバックアップが完了するまで待ちます (完了は Studio で確認します)
1. 次の手順を使用して、目的の DB のログ バックアップを無効にし、カタログ バックアップを **[Filesystem]\(ファイルシステム\)** に設定します。
1. **[SYSTEMDB]**  ->  **[構成]**  ->  **[データベースの選択]**  ->  **[Filter (log)]\(フィルター (ログ)\)** の順にダブルクリックします
    1. [enable_auto_log_backup] を **[no]** に設定します
    1. [log_backup_using_backint] を **[false]** に設定します
1. 目的のデータベースでオンデマンド バックアップを実行し、バックアップとカタログ バックアップが完了するまで待ちます。
1. 以前の設定に戻して、バックアップが Azure コンテナーにフローできるようにします。
    1. [enable_auto_log_backup] を **[yes]** に設定します
    1. [log_backup_using_backint] を **[true]** に設定します

## <a name="restore"></a>復元

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>データベースの復元先の HANA システムが表示されないのはなぜですか?

ターゲットの SAP HANA インスタンスに復元するためのすべての前提条件が満たされているかどうかを確認してください。 詳しくは、[Azure VM の SAP HANA データベースの復元の前提条件](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites)に関する記事をご覧ください。

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>データベースで DB の上書き復元が失敗するのはなぜですか?

復元の間に **[強制的に上書きします]** オプションが選択されていることを確認します。

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>"復元のソース システムとターゲット システムには互換性がありません" というエラーが表示されるのはなぜですか?

SAP HANA ノート [1642148](https://launchpad.support.sap.com/#/notes/1642148) を参照し、現在サポートされている復元の種類を確認してください。

## <a name="next-steps"></a>次のステップ

Azure VM で実行されている [SAP HANA データベースをバックアップする](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)方法を学習します。

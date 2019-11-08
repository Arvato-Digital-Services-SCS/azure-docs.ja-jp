---
title: Azure Database for PostgreSQL の監視とチューニング - Hyperscale (Citus)
description: この記事では、Azure Database for PostgreSQL (Hyperscale (Citus)) の監視およびチューニング機能について説明します。
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 6c9bf7a427c0d3d26c3accd1d0d7098413916993
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510240"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---hyperscale-citus"></a>Azure Database for PostgreSQL の監視とチューニング - Hyperscale (Citus)

サーバーに関する監視データは、ワークロードをトラブルシューティングしたり最適化したりするのに役立ちます。 Hyperscale (Citus) には、サーバー グループ内のノードの動作を分析するためのさまざまな監視オプションが用意されています。

## <a name="metrics"></a>メトリック

Hyperscale (Citus) には、サーバー グループ内の各ノード用のメトリックが用意されています。 メトリックを使うと、サポート リソースの動作を分析できます。 各メトリックは 1 分間隔で出力されます。履歴は最大 30 日分です。

メトリックのグラフを表示するだけでなく、アラートを構成することもできます。 詳細な手順については、[アラートの設定方法](howto-hyperscale-alert-on-metric.md)に関する記事をご覧ください。  その他のタスクとして、自動化されたアクションの設定、高度な分析の実行、履歴のアーカイブなどがあります。 詳細については、[Azure のメトリックの概要](../monitoring-and-diagnostics/monitoring-overview-metrics.md)に関する記事をご覧ください。

### <a name="list-of-metrics"></a>メトリックの一覧

これらのメトリックは、Hyperscale (Citus) ノードで使用できます。

|メトリック|メトリックの表示名|単位|説明|
|---|---|---|---|
|active_connections|アクティブな接続|Count|サーバーへのアクティブな接続の数|
|cpu_percent|CPU 使用率|Percent|使用されている CPU の割合|
|iops|IOPS|Count|[IOPS 定義](../virtual-machines/linux/premium-storage-performance.md#iops)と [Hyperscale スループット](concepts-hyperscale-configuration-options.md)に関する記事を参照してください|
|memory_percent|メモリの割合|Percent|使用されているメモリの割合|
|network_bytes_ingress|Network In|Bytes|アクティブな接続全体のネットワーク受信。|
|network_bytes_egress|Network Out|Bytes|アクティブな接続全体のネットワーク送信。|
|storage_percent|ストレージの割合|Percent|サーバーの最大数のうち使用されているストレージの割合|
|storage_used|使用済みストレージ|Bytes|使用されているストレージの量。 サービスで使用されるストレージには、データベース ファイル、トランザクション ログ、サーバー ログが含まれることがあります。|

Azure にはクラスター全体の集計メトリックは用意されていませんが、複数のノードのメトリックを同じグラフに配置することができます。

## <a name="next-steps"></a>次の手順

- メトリックに対するアラートの作成のガイダンスについては、[アラートを設定する方法](howto-hyperscale-alert-on-metric.md)に関するページをご覧ください。

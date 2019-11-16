---
title: Azure Cache for Redis のサーバー側の問題に関するトラブルシューティング
description: Azure Cache for Redis の一般的なサーバー側の問題を解決する方法について説明します
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: yegu
ms.openlocfilehash: 22cb4beb3411c617882972e1b91c5f538019fae4
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "74122562"
---
# <a name="troubleshoot-azure-cache-for-redis-server-side-issues"></a>Azure Cache for Redis のサーバー側の問題に関するトラブルシューティング

このセクションでは、Azure Cache for Redis またはそれがホストされている仮想マシンの状態が原因で発生する問題のトラブルシューティングについて説明します。

- [Redis サーバーでのメモリ不足](#memory-pressure-on-redis-server)
- [高い CPU 使用率またはサーバーの負荷](#high-cpu-usage-or-server-load)
- [実行時間の長いコマンド](#long-running-commands)
- [サーバー側の帯域幅の制限](#server-side-bandwidth-limitation)

> [!NOTE]
> このガイドのトラブルシューティング手順のいくつかには、Redis コマンドを実行し、さまざまなパフォーマンス メトリックを監視する手順が含まれています。 詳細および手順については、「 [追加情報](#additional-information) 」セクションの記事を参照してください。
>

## <a name="memory-pressure-on-redis-server"></a>Redis サーバーでのメモリ不足

サーバー側のメモリ不足により、要求処理を遅らせる可能性があるあらゆる種類のパフォーマンス問題が発生します。 メモリ不足になると、システムはデータをディスクにページングする可能性があります。 この _ページ フォールト_ により、システムの処理速度が大幅に低下します。 このメモリ不足の原因として考えられるものをいくつか以下に示します。

- キャッシュに、その最大容量に近いデータが格納されている。
- Redis でメモリの断片化が大量に発生している。 Redis は小さいオブジェクト用に最適化されているため、この断片化は、大きいオブジェクトを格納することによって最も頻繁に発生します。

Redis は、[INFO](https://redis.io/commands/info) コマンドを通して、この問題を識別するために役立つ "used_memory" と "used_memory_rss" という 2 つの統計を公開しています。 ポータルを使用して[これらのメトリックを表示](cache-how-to-monitor.md#view-metrics-with-azure-monitor)できます。

メモリ使用量を正常な状態に保つために実行できる、可能性のあるいくつかの変更を次に示します。

- [メモリ ポリシーを構成](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) し、キーに有効期限を設定します。 断片化が発生している場合、このポリシーでは十分でない可能性があります。
- [maxmemory-reserved 値を構成](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) します。
- 大きいキャッシュ オブジェクトをより小さい関連オブジェクトに分割します。
- 潜在的な影響について早期に通知される、使用済みメモリなどのメトリックに関する[アラートを作成](cache-how-to-monitor.md#alerts)します。
- メモリ容量の多いより大きいキャッシュ サイズに[スケーリング](cache-how-to-scale.md)します。

## <a name="high-cpu-usage-or-server-load"></a>高い CPU 使用率またはサーバーの負荷

高いサーバーの負荷または CPU 使用率は、サーバーが適切なタイミングで要求を処理できないことを示します。 サーバーは応答が遅くなり、要求レートについていけない可能性があります。

CPU またはサーバーの負荷などの[メトリックを監視](cache-how-to-monitor.md#view-metrics-with-azure-monitor)します。 タイムアウトに対応する CPU 使用率が急激に増えていないか監視します。

高いサーバーの負荷を軽減するために実行できるいくつかの変更を次に示します。

- 以下に示す[実行時間の長いコマンド](#long-running-commands)や深刻なメモリ不足によるページ フォールトなどの、CPU スパイクの原因を調査します。
- 潜在的な影響について早期に通知される、CPU または サーバーの負荷などのメトリックに関する[アラートを作成](cache-how-to-monitor.md#alerts)します。
- CPU 容量の多いより大きいキャッシュ サイズに[スケーリング](cache-how-to-scale.md)します。

## <a name="long-running-commands"></a>実行時間の長いコマンド

Redis コマンドの中には、他のコマンドより実行コストが高いものがあります。 [Redis コマンドのドキュメント](https://redis.io/commands)は、各コマンドの時間複雑度を示しています。 Redis コマンドの処理はシングルスレッドで行われるため、コマンドの実行時間が長いと、それより後に実行される他のコマンドはすべてブロックされます。 Redis サーバーに対して発行しているコマンドを確認し、パフォーマンスへの影響を理解する必要があります。 たとえば、[KEYS](https://redis.io/commands/keys) コマンドは多くの場合、それが O(N) 操作であることを認識せずに使用されます。 [SCAN](https://redis.io/commands/scan) を使用して KEYS を回避することにより CPU スパイクを削減できます。

[SLOWLOG](https://redis.io/commands/slowlog) コマンドを使用すると、サーバーに対して実行されているコストの高いコマンドを測定できます。

## <a name="server-side-bandwidth-limitation"></a>サーバー側の帯域幅の制限

キャッシュ サイズが違えば、ネットワーク帯域幅容量も異なります。 サーバーが使用可能な帯域幅を超えている場合、データはすばやくクライアントに送信されません。 サーバーが十分な速さでデータをクライアントにプッシュできないため、クライアント要求はタイムアウトする可能性があります。

"キャッシュの読み取り" および "キャッシュの書き込み" メトリックを使用すると、サーバー側の帯域幅がどれだけ使用されているかを確認できます。 ポータルで[これらのメトリックを表示](cache-how-to-monitor.md#view-metrics-with-azure-monitor)できます。

ネットワーク帯域幅の使用量が最大容量に近い状況を緩和するには:

- ネットワーク要求を削減するようにクライアント呼び出し動作を変更します。
- 潜在的な影響について早期に通知される、キャッシュの読み取りやキャッシュの書き込みなどのメトリックに関する[アラートを作成](cache-how-to-monitor.md#alerts)します。
- ネットワーク帯域幅容量の多いより大きいキャッシュ サイズに[スケーリング](cache-how-to-scale.md)します。

## <a name="additional-information"></a>追加情報

- [Azure Cache for Redis のクライアント側の問題に関するトラブルシューティング](cache-troubleshoot-client.md)
- [Azure Cache for Redis のサービス内容と適切なサイズの選択](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [キャッシュのベンチマークを実行およびテストする方法](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Azure Cache for Redis を監視する方法](cache-how-to-monitor.md)
- [Redis コマンドの実行方法](cache-faq.md#how-can-i-run-redis-commands)

---
title: Azure PowerShell のサンプル - ネットワーク
description: Azure PowerShell のサンプル
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: gwallace
ms.openlocfilehash: fb4b63d8329f0a175252b819653e192c746489b6
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74092034"
---
# <a name="azure-powershell-samples-for-networking"></a>ネットワークに関する Azure PowerShell のサンプル

次の表には、Azure PowerShell を使って構築されたスクリプトへのリンクが含まれています。

| | |
|-|-|
|**Azure リソースの間の接続**||
| [多層アプリケーション用の仮想ネットワークの作成](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークを作成します。 フロントエンド サブネットへのトラフィックは HTTP に制限され、バックエンド サブネットへのトラフィックは SQL、ポート 1433 に制限されます。 |
| [2 つの仮想ネットワークのピアリング](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | 同じリージョンに 2 つの仮想ネットワークを作成し、接続します。 |
| [ネットワーク仮想アプライアンス経由のトラフィックのルーティング](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークと、2 つのサブネット間でトラフィックをルーティングできる VM を作成します。 |
| [VM ネットワークの着信トラフィックおよび送信トラフィックのフィルター処理](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークを作成します。 フロントエンド サブネットへの受信ネットワーク トラフィックは、HTTP および HTTPS に制限されます。 バックエンド サブネットからインターネットへの送信トラフィックは許可されません。 |
|**負荷分散とトラフィックの方向**||
| [高可用性を確保するための、VM へのトラフィックの負荷分散](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | 可用性が高く、負荷分散がされた構成で仮想マシンを複数作成します。 |
| [VM 上の複数の Web サイトの負荷分散](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Azure 可用性セットに接続され、Azure Load Balancer 経由でアクセス可能な、複数の IP 構成を持つ仮想マシンを 2 つ作成します。 |
| [アプリケーションの高可用性を確保するための、複数リージョン間でのトラフィック転送](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  2 つの App Service プラン、2 つの Web アプリ、1 つの Traffic Manager プロファイル、および 2 つの Traffic Manager エンドポイントを作成します。 |
| | |

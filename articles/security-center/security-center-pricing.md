---
title: Azure Security Center の価格レベル
description: この記事では、Azure Security Center の価格について説明します。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 0c3fdc48d9b3bc77b629d4d6f1da081d70658c88
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73664258"
---
# <a name="upgrade-to-security-centers-standard-tier-for-enhanced-security"></a>Azure Security Center を Standard レベルへアップグレードすることによるセキュリティ強化
Azure Security Center は、Azure、オンプレミス、他のクラウドで実行されているワークロードの統合セキュリティ管理と高度な脅威保護を実現します。 ハイブリッド クラウド ワークロードの可視化と制御、脅威にさらされる機会を減らす積極的防御、急速に進化するサイバー攻撃への対応に役立つインテリジェント検出などの機能が提供されます。

## <a name="pricing-tiers"></a>価格レベル
Azure Security Center は 2 つのレベルで提供されます。

- Azure portal 内の Azure Security Center ダッシュボードに初めてアクセスしたとき、または API を介してプログラムで有効にした場合は、すべての Azure サブスクリプションで **Free** レベルが有効になります。 Free レベルは、セキュリティ ポリシー、継続的なセキュリティ評価、および Azure リソースを保護するための実践的なセキュリティに関する推奨事項を提供します。
- **Standard** レベルは、Free レベルの機能をプライベートおよびその他のパブリック クラウドで実行されているワークロードまで拡張したもので、統合されたセキュリティの管理と脅威の保護をハイブリッド クラウド ワークロード全体で提供します。 Standard レベルでは、高度な脅威検出機能も追加されます。これは、組み込みの行動分析と機械学習を利用して、各種攻撃やゼロデイ攻撃、アクセスやアプリケーション制御を特定し、ネットワーク攻撃やマルウェアなどによる侵害を減らします。 Standard レベルは無料でお試しいただけます。 Security Center Standard では、VM、仮想マシン スケール セット、App Service、SQL サーバー、ストレージ アカウントなどの Azure リソースがサポートされます。 Azure Security Center Standard をお使いの場合、リソースの種類に基づいてサポートをオプト アウトすることができます。 

VM に対する Free レベルのセキュリティ評価の大半は、Standard レベルのセキュリティ アラートの多くと同様、Microsoft Monitoring Agent (MMA) 機能のインストールが必要となります。 Security Center の [Auto Provision]\(自動プロビジョニング\) を有効にすることで、Azure VM のエージェントを自動的にデプロイすることができます。

## <a name="try-standard-free-for-30-days"></a>Standard レベルのサービスを 30 日間無料で試用する
Standard レベルは、最初の 30 日間は無料です。 30 日経過した時点で、サービスの利用を継続することを選択した場合は、使用量に応じて自動的に課金が開始されます。

Azure サブスクリプション全体を Standard レベルにアップグレードできます。この場合、サブスクリプション内のすべてのリソースが Standard レベルを継承します。

Standard レベルを取得するには

1. **Security Center** メイン メニューの **[Pricing & settings]\(価格と設定\)** を選択します。
2. Standard レベルにアップグレードするサブスクリプションを選択します。
3. **[価格レベル]** を選択します。
4. **[Standard]** を選択してアップグレードします。
5. **[Save]** をクリックします。

(画像内の価格はあくまでも例です) [![セキュリティ センターの価格](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> Security Center のすべての機能を有効にするには、Standard 価格レベルを適用可能な仮想マシンを含むサブスクリプションに適用する必要があります。 ワークスペースの価格を構成しても、Just-In-Time VM アクセス、適応型アプリケーション制御、および Azure リソースのネットワーク検出は有効になりません。
>

## <a name="why-upgrade-to-standard"></a>Standard レベルにアップグレードする理由
Security Center は、次のようなハイブリッド クラウド ワークロード用の強化されたセキュリティと脅威保護を提供します。

- **ハイブリッド セキュリティ** - オンプレミスとクラウドのすべてのワークロードのセキュリティを、統合された 1 つのビューで確認できます。 セキュリティ ポリシーを適用し、ハイブリッド クラウドのワークロードのセキュリティを継続的に評価することで、セキュリティ標準に確実に準拠できます。 ファイアウォールやその他のパートナー ソリューションなどのさまざまなソースから、セキュリティ データを収集、検索、分析します。
- **高度な脅威検出** - 高度な分析と Microsoft インテリジェント セキュリティ グラフを使用して、巧妙化していくサイバー攻撃に対応します。 組み込みの行動分析と機械学習を活用して、各種攻撃やゼロデイ攻撃を特定します。 ネットワーク、マシン、クラウド サービスに対する攻撃や侵害後のアクティビティを監視します。 対話型のツールと状況に応じた脅威インテリジェンスにより、調査を効率化します。
- **アクセスとアプリケーションの制御** - 特定のワークロードに適応し、機械学習を活用したホワイトリスト登録の推奨事項を適用することで、マルウェアや他の望ましくないアプリケーションをブロックします。 Azure VM の管理ポートに対するジャスト イン タイムの制御されたアクセスで、ネットワーク攻撃対象領域を減らします。 ブルート フォースなどのネットワーク攻撃に対する露出が、これによって劇的に減少します。
- **コンテナーのセキュリティ機能** - コンテナー化された環境で、脆弱性管理とリアルタイム脅威検出が利用できます。 コンテナー レジストリ リソースを有効にする際、すべての機能が有効になるまでには最大 12 時間かかることがあります。


## <a name="next-steps"></a>次の手順
この記事では、Security Center の価格について紹介しました。 Standard レベルの強化されたセキュリティと高度な脅威保護に関する詳細については、次の記事を参照してください。

- [高度な脅威検出](security-center-threat-report.md)
- [Just-In-Time VM アクセスの制御](security-center-just-in-time.md)
- [コンテナーのセキュリティの概要](container-security.md)
- [ご利用の通貨とリージョンにおける価格の詳細](https://azure.microsoft.com/pricing/details/security-center/)
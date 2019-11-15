---
title: Azure の仮想ネットワークにアクセスする - Azure Logic Apps
description: 統合サービス環境 (ISE) を利用して、ロジック アプリから Azure の仮想ネットワーク (VNET) にアクセスする方法の概要
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 11/08/2019
ms.openlocfilehash: da68cfe504332ed6641c52322f0df0d2efd95997
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820662"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>統合サービス環境 (ISE) を使用して、Azure Logic Apps から Azure Virtual Network リソースにアクセスする

場合によっては、ご利用のロジック アプリと統合アカウントで、[Azure 仮想ネットワーク](../virtual-network/virtual-networks-overview.md)内の仮想マシン (VM) や他のシステムまたはサービスなど、セキュリティで保護されたリソースにアクセスする必要が生じます。 このアクセスを設定するために、["*統合サービス環境*" (ISE) を作成して](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)、そこで自分のロジック アプリを実行したり統合アカウントを作成したりすることができます。

ISE を作成すると、その ISE は、Azure により、使用している Azure 仮想ネットワークに "*挿入*" された後、Logic Apps サービスの分離されたプライベート インスタンスが Azure 仮想ネットワークにデプロイされます。 このプライベート インスタンスは、ストレージなどの専用のリソースを使用し、パブリックで "グローバル" な Logic Apps サービスとは別に実行されます。 分離したプライベート インスタンスとパブリック グローバル インスタンスを分けることで、他の Azure テナントが自分のアプリのパフォーマンスに与える可能性がある影響 (["うるさい隣人" エフェクト](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors)とも呼ばれる) を減らすことができます。

ISE の作成後は、ロジック アプリや統合アカウントを作成するときに、以下のように ISE をロジック アプリまたは統合アカウントの場所として選択することができます。

![統合サービス環境を選択する](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

これで、ロジック アプリは、以下のいずれかの項目を使用して、仮想ネットワーク内のシステム、または仮想ネットワークに接続されているシステムに直接アクセスできるようになりました。

* そのシステムの **ISE** とラベル付けされたコネクタ
* **Core** とラベル付けされた組み込みトリガーまたは組み込みアクション (例: HTTP トリガーまたは HTTP アクション)
* カスタム コネクタ

この概要では、ISE によってロジック アプリと統合アカウントに Azure 仮想ネットワークへの直接アクセスが提供される方法を詳しく説明し、ISE とグローバルな Logic Apps サービスとの違いを比較します。

> [!IMPORTANT]
> ISE 内で実行されるロジック アプリ、組み込みトリガー、組み込みアクション、およびコネクターでは、使用量ベースの価格プランとは異なる価格プランが使用されます。 ISE の価格と課金のしくみについては、「[固定価格モデル](../logic-apps/logic-apps-pricing.md#fixed-pricing)」を参照してください。 価格については、[Logic Apps の価格](../logic-apps/logic-apps-pricing.md)に関する記事を参照してください。
>
> また、ISE では、実行継続時間、ストレージのリテンション期間、スループット、HTTP の要求と応答のタイムアウト、メッセージのサイズ、およびカスタム コネクタの要求の上限も引き上げられました。 
> 詳細については、[Azure Logic Apps の制限と構成](logic-apps-limits-and-config.md)に関するページを参照してください。

<a name="difference"></a>

## <a name="isolated-versus-global"></a>分離とグローバル

Azure で統合サービス環境 (ISE) を作成するときは、ISE を "*挿入*" する Azure 仮想ネットワークを選択することができます。 それにより、Azure は、Logic Apps サービスのプライベート インスタンスを仮想ネットワークに挿入、またはデプロイします。 このアクションにより、専用リソースでロジック アプリを作成して実行できる分離環境が作成されます。 ロジック アプリを作成するときにアプリの場所として ISE を選択すると、ロジック アプリは仮想ネットワークとそのネットワーク内のリソースに直接アクセスできるようになります。

ISE のロジック アプリはグローバルな Logic Apps サービスとユーザー操作が同じで、同様の機能を提供しています。 グローバルな Logic Apps サービスで同じ組み込みトリガー、組み込みアクション、コネクタを使用できるだけでなく、ISE 固有のコネクタも使用できます。 たとえば、次のような標準コネクタでは ISE で動作するバージョンが提供されています。

* Azure Blob Storage、Azure File Storage、Azure Table Storage
* Azure キュー、Azure Service Bus、Azure Event Hubs、IBM MQ
* FTP、SFTP-SSH
* SQL Server、Azure SQL Data Warehouse、Azure Cosmos DB
* AS2、X12、EDIFACT

ISE と ISE 以外のコネクタには、トリガーとアクションが実行される場所に違いがあります。

* ISE では、HTTP などの組み込みトリガーおよび組み込みアクションは、常にロジック アプリと同じ ISE 内で実行され、**Core** のラベルが表示されます。

  !["Core" 組み込みトリガーと組み込みアクションを選択する](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* ISE 内で実行されるコネクタには、グローバルな Logic Apps サービスで使用可能な、パブリックにホストされるバージョンがあります。 2 つのバージョンが提供されているコネクタの場合、**ISE** のラベルがあるコネクタは、常に、ご利用のロジック アプリと同じ ISE で動作します。 **ISE** ラベルがないコネクタはグローバルな Logic Apps サービスで実行されます。

  ![ISE のコネクタを選択する](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

また、ISE では、実行継続時間、ストレージのリテンション期間、スループット、HTTP の要求と応答のタイムアウト、メッセージのサイズ、およびカスタム コネクタの要求の上限も引き上げられます。 詳細については、[Azure Logic Apps の制限と構成](logic-apps-limits-and-config.md)に関するページを参照してください。

<a name="ise-level"></a>

## <a name="ise-skus"></a>ISE SKU

ISE を作成するときは、Developer SKU または Premium SKU を選択できます。 これらの SKU の違いを以下に示します。

* **開発者**

  実験、開発、テストには使用できますが、運用環境とパフォーマンス テストには使用できない、低コストの ISE が提供されます。 Developer SKU には、組み込みのトリガーとアクション、標準コネクタ、エンタープライズ コネクタ、および固定月額料金用の 1 つの [Free レベル](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)統合アカウントが含まれています。 ただし、この SKU には、サービス レベル アグリーメント (SLA)、容量のスケールアップ オプション、またはリサイクル中の冗長性は含まれず、これは遅延やダウンタイムが発生する可能性があることを意味します。

* **Premium**

  運用環境で使用できる ISE が提供され、SLA のサポート、組み込みのトリガーとアクション、標準コネクタ、エンタープライズ コネクタ、単一の [Standard レベル](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)の統合アカウント、容量スケールアップ オプション、固定月額料金のリサイクル中の冗長性などが含まれます。

> [!IMPORTANT]
> この SKU オプションは、ISE 作成時にのみ使用できます。後で変更することはできません。

価格については、[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps/)に関する記事を参照してください。 ISE の価格と課金のしくみについては、「[固定価格モデル](../logic-apps/logic-apps-pricing.md#fixed-pricing)」を参照してください。

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>ISE エンドポイントへのアクセス

ISE を作成するときに、内部アクセス エンドポイントと外部アクセス エンドポイントのどちらを使用するかを選択できます。 これらのエンドポイントにより、ISE 内のロジック アプリ上で要求または Webhook トリガーが仮想ネットワークの外からの呼び出しを受信できるかどうかが決まります。 これらのエンドポイントは、ロジック アプリの実行履歴の入力と出力へのアクセスにも影響します。

* **内部**:ISE 内のロジック アプリへの呼び出しに加え、"*仮想ネットワーク内から*" のみ実行履歴の入力と出力へのアクセスを許可するプライベート エンドポイント

* **外部**:ISE 内のロジック アプリへの呼び出しに加え、"*仮想ネットワーク外からの*" 実行履歴の入力と出力へのアクセスを許可するパブリック エンドポイント

> [!IMPORTANT]
> このアクセス エンドポイント オプションは、ISE 作成時にのみ使用できます。後で変更することはできません。

<a name="on-premises"></a>

## <a name="access-to-on-premises-data-sources"></a>オンプレミス データ ソースにアクセスする

Azure 仮想ネットワークに接続されているオンプレミス システムの場合は、ロジック アプリからそのシステムに直接アクセスできるように、次の項目のいずれかを使用してそのネットワークに ISE を挿入します。

* HTTP アクション

* そのシステムの ISE とラベル付けされたコネクタ

  > [!NOTE]
  > [統合サービス環境 (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) で SQL Server コネクタで Windows 認証を使用するには、[オンプレミス データ ゲートウェイ](../logic-apps/logic-apps-gateway-install.md)でコネクタの ISE 以外のバージョンを使用します。 ISE とラベル付けされたバージョンでは、Windows 認証はサポートされていません。

* カスタム コネクタ

  * オンプレミス データ ゲートウェイを必要とするカスタム コネクタを ISE の外部で作成した場合は、ISE 内のロジック アプリでもそれらのコネクタを使用できます。
  
  * ISE 内で作成されたカスタム コネクタは、オンプレミス データ ゲートウェイでは動作しません。 ただし、これらのコネクタでは、ISE をホストしている仮想ネットワークに接続されているオンプレミス データ ソースに直接アクセスできます。 そのため、ISE 内のロジック アプリでは、ほとんどの場合、それらのリソースと通信するときにデータ ゲートウェイは不要です。

仮想ネットワークに接続されていない、または ISE とラベル付けされたコネクタがないオンプレミス システムでは、お使いのロジック アプリからこれらのシステムに接続する前に、まず[オンプレミスのデータ ゲートウェイを設定する](../logic-apps/logic-apps-gateway-install.md)必要があります。

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE での統合アカウント

統合サービス環境 (ISE) の内部のロジック アプリで統合アカウントを使用できます。 ただし、これらの統合アカウントでは、リンクされているロジック アプリと "*同じ ISE*" を使用する必要があります。 ISE のロジック アプリは、同じ ISE にある統合アカウントのみを参照できます。 統合アカウントを作成すると、ご利用の統合アカウントの場所として、自分の ISE を選択できます。 統合アカウントと ISE に対する価格と課金のしくみについては、「[固定価格モデル](../logic-apps/logic-apps-pricing.md#fixed-pricing)」を参照してください。 価格については、[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps/)に関する記事を参照してください。

## <a name="next-steps"></a>次の手順

* [分離されたロジック アプリから Azure 仮想ネットワークに接続する](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* [成果物を統合サービス環境に追加する](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [統合サービス環境を管理する](../logic-apps/ise-manage-integration-service-environment.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) の詳細を理解する
* [Azure サービスの仮想ネットワーク統合](../virtual-network/virtual-network-for-azure-services.md)について理解する

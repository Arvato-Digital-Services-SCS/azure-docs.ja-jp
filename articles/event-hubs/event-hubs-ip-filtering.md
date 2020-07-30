---
title: Azure Event Hubs のファイアウォール ルール | Microsoft Docs
description: ファイアウォール ルールを使用して、特定の IP アドレスから Azure Event Hubs への接続を許可します。
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: 2b886aaaf40e5c82d9c7ac3ce5abeda8f54cad3b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288040"
---
# <a name="configure-ip-firewall-rules-for-an-azure-event-hubs-namespace"></a>Azure Event Hubs 名前空間に対する IP ファイアウォール規則を構成する
既定では、要求が有効な認証と承認を受けている限り、Event Hubs 名前空間にはインターネットからアクセスできます。 これは IP ファイアウォールを使用して、さらに [CIDR (クラスレス ドメイン間ルーティング)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) 表記の一連の IPv4 アドレスまたは IPv4 アドレス範囲のみに制限できます。

この機能は、Azure Event Hubs へのアクセスを特定の既知のサイトからのみに制限したいシナリオで役立ちます。 ファイアウォール規則を使用すると、特定の IPv4 アドレスから送信されたトラフィックを受け入れる規則を構成できます。 たとえば、[Azure ExpressRoute][express-route] で Event Hubs を使用する場合、オンプレミス インフラストラクチャ IP アドレスからのトラフィックのみ許可する**ファイアウォール規則**を作成できます。 

>[!WARNING]
> IP フィルタリングを有効にすると、他の Azure サービスが Event Hubs と対話するのを禁止できます。
>
> 仮想ネットワークが実装されているときは、信頼できる Microsoft サービスはサポートされません。
>
> 仮想ネットワークでは動作しない Azure の一般的なシナリオは次のとおりです (網羅的なリストでは**ない**ことに注意してください)
> - Azure Stream Analytics
> - Azure IoT Hub ルート
> - Azure IoT Device Explorer
>
> 仮想ネットワーク上には、次の Microsoft サービスが必要です
> - Azure Web Apps
> - Azure Functions


## <a name="ip-firewall-rules"></a>IP ファイアウォール規則
IP ファイアウォール規則は、Event Hubs 名前空間レベルで適用されます。 したがって、規則は、サポートされているプロトコルを使用するクライアントからのすべての接続に適用されます。 Event Hubs 名前空間上の許可 IP 規則に一致しない IP アドレスからの接続試行は、未承認として拒否されます。 IP 規則に関する記述は応答に含まれません。 IP フィルター規則は順に適用され、IP アドレスと一致する最初の規則に基づいて許可アクションまたは拒否アクションが決定されます。

## <a name="use-azure-portal"></a>Azure Portal の使用
このセクションでは、Azure portal を使用して、Event Hubs 名前空間に対する IP ファイアウォール規則を作成する方法について説明します。 

1. [Azure portal](https://portal.azure.com) で **[Event Hubs 名前空間]** に移動します。
2. 左側のメニューで、 **[ネットワーク]** オプションを選択します。 **[すべてのネットワーク]** オプションを選択した場合、イベント ハブはあらゆる IP アドレスからの接続を受け入れます。 この設定は、IP アドレス範囲 0.0.0.0/0 を受け入れる規則と同じです。 

    ![ファイアウォールで [すべてのネットワーク] のオプションが選択されている](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. アクセスを特定のネットワークと IP アドレスに制限するには、 **[選択されたネットワーク]** オプションを選択します。 **[ファイアウォール]** セクションで、次の手順のようにします。
    1. 現在のクライアント IP にその名前空間へのアクセスを許可するには、 **[クライアント IP アドレスを追加する]** オプションを選択します。 
    2. **[アドレス範囲]** に、特定の IPv4 アドレスまたは IPv4 アドレスの範囲を CIDR 表記で入力します。 
    3. **信頼された Microsoft サービスがこのファイアウォールをバイパスすることを許可する**かどうかを指定します。 

        > [!WARNING]
        > **[選択されたネットワーク]** オプションを選び、IP アドレスまたはアドレス範囲を指定しなかった場合、サービスではすべてのネットワークからのトラフィックが許可されます。 

        ![[ファイアウォール] - [すべてのネットワーク] オプションが選択されている](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. ツール バーの **[保存]** を選択して設定を保存します。 ポータルの通知に確認が表示されるまで、数分間お待ちください。


## <a name="use-resource-manager-template"></a>Resource Manager テンプレートの使用

> [!IMPORTANT]
> ファイアウォール ルールは、**Standard** レベルと **Dedicated** レベルの Event Hubs でサポートされます。 Basic レベルではサポートされません。

次の Resource Manager テンプレートでは、既存の Event Hubs 名前空間に IP フィルター規則を追加できます。

テンプレート パラメーター:

- **ipMask** は、1 つの IPv4 アドレスか、または CIDR 表記法で記述した IP アドレス ブロックです。 たとえば、CIDR 表記では、70.37.104.0/24 は 70.37.104.0 から 70.37.104.255 までの 256 個の IPv4 アドレスを表し、24 は範囲に対する有効プレフィックス ビット数を示します。

> [!NOTE]
> 可能な拒否ルールはありませんが、Azure Resource Manager テンプレートには、接続を制限しない **"Allow"** に設定された既定のアクション セットがあります。
> 仮想ネットワークまたはファイアウォールのルールを作成するときは、***"defaultAction"*** を変更する必要があります。
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> to
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

テンプレートをデプロイするには、[Azure Resource Manager][lnk-deploy] の手順に従います。

## <a name="next-steps"></a>次のステップ

Event Hubs へのアクセスを Azure 仮想ネットワークに制約するには、次のリンクをご覧ください。

- [Event Hubs の仮想ネットワーク サービス エンドポイント][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md

---
title: Azure Cosmos アカウントの Azure Private Link を構成する
description: 仮想ネットワークのプライベート IP アドレスを使用して Azure Cosmos アカウントにアクセスするように Azure Private Link を設定する方法について説明します。
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: thweiss
ms.openlocfilehash: 34b54459629560ba80e6a38d10edbab32ea44778
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820155"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account-preview"></a>Azure Cosmos アカウントの Azure Private Link を構成する (プレビュー)

Azure Private Link を使用すると、プライベート エンドポイント経由で Azure Cosmos アカウントに接続できます。 プライベート エンドポイントは、仮想ネットワークのサブネットにある一組のプライベート IP アドレスです。 Private Link を使用すると、特定の Azure Cosmos アカウントへのアクセスをプライベート IP アドレスで制限できます。 Private Link と制限付き NSG ポリシーを結合されることで、データ流出のリスクを軽減することができます。 プライベート エンドポイントの詳細については、[Azure Private Link](../private-link/private-link-overview.md) に関する記事を参照してください。

また、Private Link を使用すると、仮想ネットワーク内またはピアリングされた仮想ネットワーク内から Azure Cosmos アカウントにアクセスできます。 Private Link にマップされたリソースは、プライベートピアリングを使用して、VPN または ExpressRoute 経由でオンプレミスからアクセスすることもできます。 

"自動" または "手動" の承認方法により、Private Link を使用して構成された Azure Cosmos アカウントに接続できます。 詳細については、Private Link のドキュメントの[承認ワークフロー](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow)に関するセクションを参照してください。 この記事では、自動承認方法を使用している前提で、Private Link を作成するステップについて説明します。

## <a name="create-a-private-link-using-the-azure-portal"></a>Azure portal を使用して Private Link を作成する

Azure portal を使用して既存の Azure Cosmos アカウントの Private Link を作成するには、次のステップに従います。

1. **[すべてのリソース]** ウィンドウで、セキュリティで保護する Azure Cosmos DB アカウントを見つけます。

1. [設定] メニューから **[プライベート エンドポイント接続]** を選択し、 **[プライベート エンドポイント]** オプションを選択します。

   ![Azure portal を使用してプライベート エンドポイントを作成する](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. **[Create a private endpoint (Preview) - Basics]\(プライベート エンドポイント (プレビュー) の作成 - 基本\)** ウィンドウで次の詳細を入力または選択します。

    | Setting | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | Subscription | サブスクリプションを選択します。 |
    | Resource group | リソース グループを選択します。|
    | **インスタンスの詳細** |  |
    | 名前 | プライベート エンドポイントの名前を入力します。名前がすでに使用されている場合は、一意の名前を作成します。 |
    |リージョン| Private Link をデプロイするリージョンを選択します。 プライベート エンドポイントは、仮想ネットワークと同じ場所に作成する必要があります。|
    |||
1. **[次へ:リソース]** を選択します。
1. **[プライベート エンドポイントの作成 - リソース]** で、次の情報を入力または選択します。

    | Setting | 値 |
    | ------- | ----- |
    |接続方法  | 自分のディレクトリ内の Azure リソースに接続するように選択します。 <br/><br/> このオプションを使用すると、Private Link を設定したり、共有しているリソース ID または別名を使用して他のユーザーのリソースに接続したりするリソースを 1 つ選択することができます。|
    | Subscription| サブスクリプションを選択します。 |
    | リソースの種類 | **AzureCosmosDB/databaseAccounts** を選択します。 |
    | リソース |Azure Cosmos アカウントを選択する |
    |ターゲット サブリソース |マップする Cosmos DB API の種類を選択します。 既定では SQL、MongoDB、Cassandra API では、1 つしか選択できません。 Gremlin と Table API のような API は SQL API と相互運用可能であるため、*SQL* を選択することもできます。 |
    |||

1. **[次へ:構成]** を選択します。
1. **[Create a private endpoint (Preview) - Configuration]\(プライベート エンドポイント (プレビュー) の作成 - 構成\)** で次の情報を入力または選択します。

    | Setting | 値 |
    | ------- | ----- |
    |**ネットワーク**| |
    | 仮想ネットワーク| 仮想ネットワークを選択します。 |
    | Subnet | サブネットを選択します。 |
    |**プライベート DNS の統合**||
    |プライベート DNS ゾーンとの統合 |**[はい]** を選択します。 <br><br/> プライベート エンドポイントに非公開で接続するには、DNS レコードが必要です。 プライベート エンドポイントとプライベート DNS ゾーンを統合することをお勧めします。 また、独自の DNS サーバーを利用したり、仮想マシン上のホスト ファイルを使用して DNS レコードを作成したりすることもできます。 |
    |プライベート DNS ゾーン |*privatelink.documents.azure.com* を選択する <br><br/> プライベート DNS ゾーンは自動的に決定され、現時点では Azure portal を使用して変更することはできません。|
    |||

1. **[Review + create]\(レビュー + 作成\)** を選択します。 **[確認および作成]** ページが表示され、Azure によって構成が検証されます。
1. "**証に成功しました**" というメッセージが表示されたら、 **[作成]** を選択します。

Azure Cosmos アカウントの Private Link を承認すると、Azure portal の **[ファイアウォールと仮想ネットワーク]** ウィンドウにある **[すべてのネットワーク]** オプションがグレー表示されます。

次の表は、さまざまな Azure Cosmos アカウント API の種類、サポートされているサブ リソース、および対応するプライベート ゾーン名間のマッピングを示しています。 Gremlin および Table API アカウントには SQL API を通じてアクセスすることもできるため、これらの API には 2 つのエントリがあります。

|Azure Cosmos アカウント API の種類  |サポートされているサブリソース (または groupIds) |プライベート ゾーン名  |
|---------|---------|---------|
|SQL    |   SQL      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  SQL       |  privatelink.documents.azure.com    |
|テーブル    |    テーブル     |   privatelink.table.cosmos.azure.com    |
|テーブル     |   SQL      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>プライベート IP アドレスを取り込む

プライベート エンドポイントがプロビジョニングされたら、IP アドレスにクエリを実行できます。 Azure portal から IP アドレスを表示するには、以下の手順に従います。 **[すべてのリソース]** を選択し、先ほど作成したプライベート エンドポイント (この例では "dbPrivateEndpoint3") を検索して [概要] タブを選択すると、DNS の設定と IP アドレスが表示されます。

![Azure portal のプライベート IP アドレス](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

プライベート エンドポイントごとに複数の IP アドレスが作成されます。

* 1 つは、Azure Cosmos アカウントのグローバルな (リージョンに依存しない) エンドポイント用です。
* もう 1 つは、Azure Cosmos アカウントがデプロイされているリージョン用です。

## <a name="create-a-private-link-using-azure-powershell"></a>Azure PowerShell を使用して Private Link を作成する

次の PowerSehll スクリプトを実行して、既存の Azure Cosmos アカウントに "MyPrivateEndpoint" という名前の Private Link を作成します。 必ず、変数値を環境に固有の詳細で置き換えます。

```azurepowershell-interactive
Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Cosmos DB account: Sql or MongoDB or Cassandra or Gremlin or Table
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-private-dns-zone"></a>プライベート エンドポイントとプライベート DNS ゾーンを統合する

プライベート エンドポイントを作成した後は、次の PowerSehll スクリプトを使用して、プライベート DNS ゾーンと統合できます。

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>プライベート IP アドレスを取り込む

プライベート エンドポイントがプロビジョニングされたら、次の PowerShell スクリプトを使用して、IP アドレスと FQDN のマッピングにクエリを実行できます。

```azurepowershell-interactive

$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-link-using-a-resource-manager-template"></a>Resource Manager テンプレートを使用して Private Link を作成する

Private Link を設定するには、仮想ネットワーク サブネットにプライベート エンドポイントを作成します。 これは、Azure Resource Manager テンプレートを使用することで可能です。 次のコードを使用して、"PrivateEndpoint_parameters.json" という名前の Resource Manager テンプレートを作成します。 このテンプレートは、既存の仮想ネットワークにある既存の Azure Cosmos SQL API アカウントのプライベート エンドポイントを作成します。

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

### <a name="define-the-template-parameters-file"></a>テンプレート パラメーター ファイルを定義する

テンプレートのパラメーター ファイルを作成し、"PrivateEndpoint_parameters.json" という名前を付けます。 次のコードをパラメーター ファイルに追加します。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

### <a name="deploy-the-template-by-using-a-powershell-script"></a>PowerShell スクリプトを使用してテンプレートをデプロイする

次に、次のコードを使用して PowerShell スクリプトを作成します。 スクリプトを実行する前に、サブスクリプション ID、リソース グループ名、その他の変数値を環境の詳細に合わせて置換してください。

```azurepowershell-interactive
### This script creates a private endpoint for an existing Cosmos DB account in an existing VNet

## Step 1: Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Cosmos DB account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

PowerShell スクリプトの "GroupId" 変数には、アカウントの API 型である値を 1 つしか含めることができません。 使用できる値は、以下のとおりです。SQL、MongoDB、Cassandra、Gremlin、Table。 一部の Azure Cosmos アカウントの種類は、複数の API を使用してアクセスできます。 例:

* Gremlin API アカウントは、Gremlin アカウントおよび SQL API アカウントの両方からアクセスできます。
* Table API アカウントは、Table アカウントおよび SQL API アカウントの両方からアクセスできます。

このようなアカウントでは、対応する API の種類を "GroupId" 配列に指定し、API の種類ごとにプライベート エンドポイントを 1 つ作成する必要があります。

テンプレートが正常にデプロイされると、次の図に示すような出力が表示されます。 プライベート エンドポイントが正しく設定されると、provisioningState の値は "Succeeded" になります。

![Resource Manager テンプレートのデプロイの出力](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

テンプレートがデプロイされると、プライベート IP アドレスがサブネット内で予約されます。 Azure Cosmos アカウントのファイアウォール規則は、プライベート エンドポイントからの接続のみを受け入れるように構成されています。

## <a name="configure-custom-dns"></a>カスタム DNS の構成

プライベート エンドポイントが作成されたサブネット内で、プライベート DNS を使用する必要があります。 また、各プライベート IP アドレスが DNS エントリにマップされるようにエンドポイントを構成します (上記の応答の "fqdn" プロパティを参照してください)。

プライベート エンドポイントを作成するときに、Azure のプライベート DNS ゾーンと統合できます。 プライベート エンドポイントを Azure のプライベート DNS ゾーンと統合せず、代わりにカスタム DNS を使用する場合は、プライベート エンドポイント用に予約されているすべてのプライベート IP アドレスの DNS レコードを追加するように DNS を構成する必要があります。

## <a name="firewall-configuration-with-private-link"></a>Private Link を使用したファイアウォールの構成

Private Link をファイアウォール規則と組み合わせて使用した場合のさまざまな状況と結果を次に示します。

* ファイアウォール規則が構成されていない場合、既定では、すべてのトラフィックは Azure Cosmos アカウントにアクセスできます。

* パブリック トラフィックまたはサービス エンドポイントが構成済みで、プライベート エンドポイントが作成されている場合、異なる種類の受信トラフィックは、対応するファイアウォール規則の種類に応じて承認されます。

* パブリック トラフィックまたはサービス エンドポイントが構成されておらず、プライベート エンドポイントが作成されている場合、Azure Cosmos アカウントにはプライベート エンドポイント経由でのみアクセスできます。 パブリック トラフィックまたはサービス エンドポイントが構成されていない場合は、承認されたすべてのプライベート エンドポイントが拒否または削除されてから、アカウントがすべてのネットワークに対して開かれます。

## <a name="update-private-endpoint-when-you-add-or-remove-a-region"></a>リージョンの追加または削除時にプライベート エンドポイントを更新する

Azure Cosmos アカウントにリージョンを追加または削除する場合、そのアカウントの DNS エントリを追加または削除する必要があります。 これらの変更は、プライベート エンドポイントで適宜更新する必要があります。 現時点では、次の手順に従って手動でこの変更を行う必要があります。

1. Azure Cosmos DB 管理者が、リージョンを追加または削除します。 その後、ネットワーク管理者に保留中の変更に関する通知が送信されます。 Azure Cosmos アカウントにマップされたプライベート エンドポイントの "ActionsRequired" プロパティが "None" から "Recreate" に変更されます。 ネットワーク管理者は次に、作成に使用した Resource Manager ペイロードを含む PUT 要求を発行して、プライベート エンドポイントを更新します。

1. この操作が完了したら、さらに追加または削除した DNS エントリと対応するプライベート IP アドレスを反映するため、サブネットのプライベート DNS を更新する必要があります。

たとえば、Azure Cosmos アカウントを 3 つのリージョンにデプロイする場合は、次のようになります。"米国西部"、"米国中部"、および "西ヨーロッパ"。 アカウントのプライベート エンドポイントを作成すると、4 つのプライベート IP がサブネットで予約されます。 この内、各リージョンごとに 1 つ (合計 3 つ)、およびグローバル/リージョンに依存しないエンドポイントの 1 つです。

Azure Cosmos アカウントに "米国東部" などの新しいリージョンを追加する場合は、後で行います。 既定では、既存のプライベート エンドポイントから新しいリージョンにアクセスすることはできません。 Azure Cosmos アカウント管理者は、新しいリージョンからアクセスする前に、プライベート エンドポイント接続を更新する必要があります。 

` Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>` コマンドを実行すると、コマンドの出力には、"Recreate" に設定された `actionsRequired` パラメーターが含まれます。 この値は、プライベート エンドポイントを更新する必要があることを示します。 次に、Azure Cosmos アカウント管理者が `Set-AzPrivateEndpoint` コマンドを実行して、プライベート エンドポイントの更新をトリガーします。

```powershell
$pe = Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>

Set-AzPrivateEndpoint -PrivateEndpoint $pe
```

新しいプライベート IP がこのプライベート エンドポイントの下のサブネットに自動的に予約され、値 `actionsRequired` が `None` になります。 プライベート DNZ ゾーンを統合していない場合 (つまり、カスタム プライベート DNS を使用している場合)、新しいリージョンに対応するプライベート IP に新しい DNS レコードを追加するには、プライベート DNS を構成する必要があります。

リージョンを削除する際も同じステップを使用できます。 削除されたリージョンのプライベート IP が自動的に回収され、`actionsRequired` フラグが `None` になります。 プライベート DNZ ゾーンを統合していない場合は、削除されたリージョンの DNS レコードを削除するには、プライベート DNS を構成する必要があります。

プライベート DNS ゾーンの DNS レコードは、プライベート エンドポイントが削除されるか、Azure Cosmos アカウントのリージョンが削除されても、自動的には削除されません。 DNS レコードを手動で削除する必要があります。

## <a name="current-limitations"></a>現時点での制限事項

Azure Cosmos アカウントで Private Link を使用する場合は、次の制限事項が適用されます。

* Azure Cosmos アカウントおよび VNET での Private Link のサポートは、特定のリージョンでのみご利用いただけます。 サポートされているリージョンの一覧については、Private Link に関する記事の[利用可能なリージョン](../private-link/private-link-overview.md#availability)に関するセクションを参照してください。 **プライベート エンドポイントを作成できるようにするには、VNET と Azure Cosmos アカウントの両方が、サポートされているリージョンに存在する必要があります。**

* ダイレクト モード接続を使用して Azure Cosmos アカウントで Private Link を使用する場合は、TCP プロトコルのみを使用できます。 HTTP プロトコルはまだサポートされていません

* MongoDB アカウント用の Azure Cosmos DB の API を使用する場合、プライベート エンドポイントは、サーバーのバージョンが 3.6 のアカウントでのみサポートされます (つまり、`*.mongo.cosmos.azure.com` 形式でエンドポイントを使用するアカウント)。 サーバーのバージョンが 3.2 のアカウント (`*.documents.azure.com` の形式でエンドポイントを使用するアカウント) では、Private Link はサポートされていません。 Private Link を使用するには、古いアカウントを新しいバージョンに移行する必要があります。

* Private Link を採用している MongoDB アカウントで Azure Cosmos DB の API を使用する場合、Robo 3T、Studio 3T、Mongoose などのツールは使用できません。このエンドポイントは、appName =<account name> パラメーターが指定されている場合にのみ、Private Link をサポートできます。 例: replicaSet=globaldb&appName=mydbaccountname。 これらのツールは接続文字列のアプリ名をサービスに渡さないため、Private Link は使用できません。 ただし、3.6 バージョンの SDK ドライバーでは、これらのアカウントにアクセスすることができます。

* Private Link が含まれている仮想ネットワークを移動または削除することはできません。

* プライベート エンドポイントに接続されている Azure Cosmos アカウントは削除できません。

* Azure Cosmos アカウントは、アカウントに接続されているすべてのプライベート エンドポイントにマップされていないリージョンにフェールオーバーすることはできません。 詳細については、前のセクションのリージョンの追加または削除について参照してください。

* 自動承認されるプライベート エンドポイントを作成するには、管理者によって、Azure Cosmos アカウント スコープで少なくとも"*/PrivateEndpointConnectionsApproval" アクセス許可がネットワーク管理者に付与されている必要があります。

### <a name="private-dns-zone-integration-limitations"></a>プライベート DNS ゾーン統合の制限事項

プライベート DNS ゾーンの DNS レコードは、プライベート エンドポイントが削除されるか、Azure Cosmos アカウントのリージョンが削除されても、自動的には削除されません。 以下の操作の前に、DNS レコードを手動で削除する必要があります。

* このプライベート DNS ゾーンにリンクされる新しいプライベート エンドポイントを追加する。
* このプライベート DNS ゾーンにリンクされるプライベート エンドポイントがあるすべてのデータベース アカウントに新しいリージョンを追加する。

DNS レコードをクリーンアップしないと、プライベート エンドポイントの削除またはリージョンの削除後に追加されたリージョンへのデータの停止など、予期しないデータ プレーンの問題が発生する可能性があります

## <a name="next-steps"></a>次の手順

Azure Cosmos DB の他のセキュリティ機能の詳細については、次の記事を参照してください。

* Azure Cosmos DB 用のファイアウォールを構成するには、[ファイアウォールのサポート](firewall-support.md)を参照してください。

* [Azure Cosmos アカウントに仮想ネットワーク サービス エンドポイントを構成する方法。](how-to-configure-vnet-service-endpoint.md)

* Private Link の詳細については、[Azure Private Link](../private-link/private-link-overview.md) に関するドキュメントを参照してください。

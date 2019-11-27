---
title: Azure Resource Manager テンプレートでの条件付きデプロイ
description: Azure Resource Manager テンプレート内のリソースを条件付きでデプロイする方法について説明します。
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: tomfitz
ms.openlocfilehash: b6d707fc4bbc5fa57ffb0c809d7f70efebef99e9
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881657"
---
# <a name="conditional-deployment-in-resource-manager-templates"></a>Resource Manager テンプレートでの条件付きデプロイ

オプションでテンプレート内のリソースをデプロイする必要があることがあります。 リソースをデプロイするかどうかを指定するには、`condition` 要素を使用します。 この要素の値は、true または false に解決されます。 値が true の場合、リソースが作成されます。 値が false の場合、リソースは作成されません。 この要素の値は、リソース全体にのみ適用できます。

## <a name="new-or-existing-resource"></a>新規または既存のリソース

条件付きデプロイを使用して、新しいリソースを作成したり、既存のリソースを使用したりすることができます。 次の例は、条件を使用して新しいストレージ アカウントをデプロイする方法、または既存のストレージ アカウントを使用する方法を示します。

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

パラメーター **newOrExisting** が **new** に設定されると、その条件は true に評価されます。 ストレージ アカウントはデプロイされます。 ただし、**newOrExisting** が **existing** に設定されると、その条件は false に評価され、ストレージ アカウントはデプロイされません。

`condition` 要素を使用する完全なテンプレート例については、[新規または既存の仮想ネットワーク、ストレージ、およびパブリック IP を使用する VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) に関するページを参照してください。

## <a name="allow-condition"></a>条件を許可する

条件が許可されるかどうかを示すパラメーター値を渡すことができます。 次の例では、SQL サーバーがデプロイされ、必要に応じて Azure IP が許可されます。

```json
{
    "type": "Microsoft.Sql/servers",
    "name": "[parameters('serverName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "properties": {
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "version": "12.0"
    },
    "resources": [
        {
            "condition": "[parameters('allowAzureIPs')]",
            "type": "firewallRules",
            "name": "AllowAllWindowsAzureIps",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
            ],
            "properties": {
                "endIpAddress": "0.0.0.0",
                "startIpAddress": "0.0.0.0"
            }
        }
    ]
}
```

完全なテンプレートについては、[Azure SQL 論理サーバー](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server)に関するページをご覧ください。

## <a name="runtime-functions"></a>ランタイム関数

条件付きでデプロイされるリソースで [reference](resource-group-template-functions-resource.md#reference) または [list](resource-group-template-functions-resource.md#list) 関数を使用した場合、この関数はリソースがデプロイされていなくても評価されます。 この関数が存在しないリソースを参照する場合、エラーが返されます。

リソースがデプロイされるときにのみ条件に対してこの関数が評価されるようにするには、[if](resource-group-template-functions-logical.md#if) 関数を使用します。 条件付きでデプロイされるリソースで if と reference を使用するサンプル テンプレートについては、[if 関数](resource-group-template-functions-logical.md#if)に関する説明を参照してください。

## <a name="condition-with-complete-mode"></a>完全モードの状態

[完全モード](deployment-modes.md)を使用したテンプレートをデプロイする場合で、なおかつ condition が false と評価されるためにリソースがデプロイされない場合、テンプレートをデプロイするために使用する REST API のバージョンによって結果が異なります。 2019-05-10 より前のバージョンを使用する場合、リソースは**削除されません**。 2019-05-10 以降では、リソースは**削除されます**。 最新バージョンの Azure PowerShell および Azure CLI では、condition が false の場合、リソースが削除されます。

## <a name="next-steps"></a>次の手順

* テンプレート作成に関する推奨事項については、「[Azure Resource Manager テンプレートのベスト プラクティス](template-best-practices.md)」を参照してください。
* リソースの複数のインスタンスを作成するには、「[Azure Resource Manager テンプレートでのリソース、プロパティ、または変数の反復](resource-group-create-multiple.md)」を参照してください。
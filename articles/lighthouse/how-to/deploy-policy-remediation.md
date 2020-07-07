---
title: 修復できるポリシーをデプロイする
description: Azure の委任されたリソース管理に顧客をオンボードする方法について説明します。これにより、自分のテナントからそれらのリソースにアクセスして管理できるようになります。
ms.date: 10/11/2019
ms.topic: how-to
ms.openlocfilehash: a953db44d8b4fc035d947d3534185062d0ec884b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "84634134"
---
# <a name="deploy-a-policy-that-can-be-remediated-within-a-delegated-subscription"></a>委任されたサブスクリプション内で修復が可能なポリシーをデプロイする

[Azure Lighthouse](../overview.md) では、サービス プロバイダーが委任されたサブスクリプション内にポリシー定義を作成したり、その編集をしたりすることができます。 ただし、[修復タスク](../../governance/policy/how-to/remediate-resources.md)を使用しているポリシー (つまり、効果が [deployIfNotExists](../../governance/policy/concepts/effects.md#deployifnotexists) または [modify](../../governance/policy/concepts/effects.md#modify) のポリシー) をデプロイする場合には、顧客のテナントに[マネージド ID](../../active-directory/managed-identities-azure-resources/overview.md) を作成する必要があります。 このマネージド ID は、ポリシー内にテンプレートをデプロイする際に Azure Policy が使用するものです。 このシナリオを実現するにあたっては、顧客を Azure の委任されたリソース管理にオンボードする場合とポリシーそのものをデプロイする場合のどちらにも必要な手順があります。

## <a name="create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant"></a>顧客のテナント内でマネージド ID にロールを割り当てることができるユーザーを作成する

Azure の委任されたリソースの管理に顧客をオンボードする際には、[Azure Resource Manager テンプレート](onboard-customer.md#create-an-azure-resource-manager-template)のほかに、パラメーター ファイルを使用します。このパラメーター ファイルには、管理主体となるテナント内のユーザー、ユーザー グループ、サービス プリンシパルのうち、顧客テナント内の委任されたリソースにアクセスできるようにするものを定義します。 パラメーター ファイルでは、ファイルに定義した各ユーザー (**principalId**) に対して、アクセス権のレベルを定めた[組み込みロール](../../role-based-access-control/built-in-roles.md) (**roleDefinitionId**) が割り当てられます。

ある **principalId** に顧客テナント内でのマネージド ID の作成を許可するには、**roleDefinitionId** を**ユーザー アクセス管理者**に設定する必要があります。 このロールは一般にサポートされているものではありませんが、今回のように、このアクセス許可が設定されているユーザーがマネージド ID に対していくつかの組み込みロールを割り当てられるようにするシナリオでは利用が可能です。 これらのロールは、**delegatedRoleDefinitionIds** プロパティで定義します。 そこには、ユーザー アクセス管理者または所有者を除き、あらゆる組み込みロールを指定できます。

顧客のオンボードが終わると、この承認で作成された **principalId** が、顧客テナント内のマネージド ID にこれらの組み込みロールを割り当てることができるようになります。 ただし、ユーザー アクセス管理者ロールに通常関連付けられている他のアクセス許可が付与されることはありません。

以下の例は、ある **principalId** にユーザー アクセス管理者ロールを設定するものです。 このユーザーは、顧客テナント内でマネージド ID に次の 2 種類の組み込みロールを割り当てることができます:共同作成者および Log Analytics 共同作成者。

```json
{
    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
    "principalIdDisplayName": "Policy Automation Account",
    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "delegatedRoleDefinitionIds": [
         "b24988ac-6180-42a0-ab88-20f7382dd24c",
         "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
    ]
}
```

## <a name="deploy-policies-that-can-be-remediated"></a>修復可能なポリシーをデプロイする

上で説明したとおりに必要なアクセス許可を設定してユーザーを作成したら、そのユーザーが顧客テナント内に修復タスクを使ったポリシーをデプロイできるようになります。

たとえば、こちらの[サンプル](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring)にあるように、顧客テナント内にある Azure Key Vault リソースを対象とした診断を有効にしたい場合を考えてみましょう。 (上で説明したとおり) 管理主体となるテナント内で適切なアクセス許可の設定を済ませたユーザーが、このシナリオを実現するために [Azure Resource Manager テンプレート](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/policy-enforce-keyvault-monitoring/enforceAzureMonitoredKeyVault.json)をデプロイします。

現時点では、委任されたサブスクリプションと一緒に使用するポリシーの割り当てを作成する操作に Azure portal ではなく、API を使用する必要があります。 その際、**apiVersion** は新しい **delegatedManagedIdentityResourceId** プロパティが含まれる **2019-04-01-preview** に設定する必要があります。 このプロパティを使用すると、(Azure の委任されたリソース管理にオンボードしたサブスクリプションまたはリソース グループに) 顧客テナント内のマネージド ID を含めることができるようになります。

次の例は、**delegatedManagedIdentityResourceId** を使ったロール割り当てを示したものです。

```json
"type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2019-04-01-preview",
            "name": "[parameters('rbacGuid')]",
            "dependsOn": [
                "[variables('policyAssignment')]"
            ],
            "properties": {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', variables('rbacContributor'))]",
                "principalType": "ServicePrincipal",
                "delegatedManagedIdentityResourceId": "[concat(subscription().id, '/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment'))]",
                "principalId": "[toLower(reference(concat('/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment')), '2018-05-01', 'Full' ).identity.principalId)]"
            }
```

> [!TIP]
> 委任されたサブスクリプションに (modify の効果を使って) タグを追加または削除するポリシーのデプロイ方法は、[別のよく似たサンプル](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag)でも確認できます。

## <a name="next-steps"></a>次のステップ

- [Azure Policy](../../governance/policy/index.yml) の詳細を確認する。
- [Azure リソースのマネージド ID](../../active-directory/managed-identities-azure-resources/overview.md) の詳細を確認する。

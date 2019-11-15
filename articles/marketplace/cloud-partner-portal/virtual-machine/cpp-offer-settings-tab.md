---
title: Azure Marketplace 向け Cloud パートナー ポータルでの仮想マシンの [オファーの設定] タブ
description: Azure Marketplace の VM オファーの作成で使用する [プランの設定] タブについて説明します。
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 04/25/2019
ms.author: pabutler
ms.openlocfilehash: 6f7b90f6b02999869026db24836091233692143c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824441"
---
# <a name="virtual-machine-offer-settings-tab"></a>仮想マシンの [プランの設定] タブ

仮想マシンに対する **[新しいプラン]** ページの最初のタブは **[プランの設定]** という名前です。  

![仮想マシンの [新しいプラン] ページ](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>[Offer Settings]\(オファーの設定\) のフィールド

**[オファーの設定]** タブでは、次のフィールドを指定する必要があります。  フィールド名に付いているアスタリスク (*) は、そのフィールドが必須であることを示します。 

|  **フィールド**       |     **説明**                                                          |
|  ---------       |     ---------------                                                          |
| **オファー ID\***   | オファーの (発行元プロファイル内での) 一意識別子です。 この ID は、製品 URL、Azure Resource Manager テンプレート、および課金レポートに表示されます。 最大長は 50 文字で、小文字の英数字とハイフン (-) だけを使用でき、ダッシュで終わることはできません。 このフィールドは、オファーの運用開始後に変更することはできません。 <br> たとえば、Contoso がオファー ID **sample-vm** でオファーを発行した場合、そのオファーには Azure Marketplace URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` が割り当てられます。 |
| **発行元\***  | Azure Marketplace での組織の一意識別子です。 すべてのオファリングには、発行元 ID が関連付けられる必要があります。 この値は、オファーを保存した後は変更できません。 |
| **[名前]\***       | オファーの表示名です。 この名前は、Azure Marketplace と Cloud パートナー ポータルに表示されます。 最大で 50 文字の長さにできます。 製品の覚えやすいブランド名を含めることをお勧めします。 販売方法である場合を除き、組織名はここに含めないでください。 他の Web サイトやパブリケーションでこのオファーを販売している場合は、すべてのパブリケーションで名前が正確に同じであることを確認します。 |
|   |   |
 
すべてのフィールドを指定した後、 **[保存]** をクリックします。 


## <a name="next-steps"></a>次の手順

次のタブでは、[SKU](./cpp-skus-tab.md) をオファーに追加します。

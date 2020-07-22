---
title: Azure DevTest Labs で Azure Marketplace イメージの設定を構成する
description: Azure DevTest Labs で VM を作成する場合にどの Azure Marketplace イメージを使用できるようにするかを構成する
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9fdb4e3a888e876f91b8af2e4854a9c101eea45c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482719"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs で Azure Marketplace イメージの設定を構成する
DevTest Labs では、実際のラボで使用する Azure Marketplace イメージの構成方法に応じて、Azure Marketplace イメージに基づく VM を作成することができます。 この記事では、ラボで VM を作成する際に使用できるようにする Azure Marketplace イメージ (該当するものがある場合) を指定する方法を説明します。 これにより、チームだけが必要な Marketplace イメージにアクセスできるようになります。 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>VM を作成する際に使用できるようにする Azure Marketplace イメージを選択する
1. [Azure portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) にサインインする
2. **[すべてのサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。
3. ラボの一覧で目的のラボを選択します。 
4. ラボのブレードで、 **[構成とポリシー]** を選択します。
5. ラボの **[Configuration and policies]\(構成とポリシー\)** ブレードの **[Virtual Machine Bases]\(仮想マシンのベース\)** の下で **[Marketplace images]\(Marketplace イメージ\)** を選択します。
6. 対象となるすべての Azure Marketplace イメージを新しい VM のベースとして利用できるようにするかどうかを指定します。 **[はい]** を選択すると、次の条件をすべて満たす Azure Marketplace イメージのすべてがラボで使用できるようになります。
   
   * イメージは 1 つの VM を作成する、 **かつ**
   * イメージは Azure Resource Manager を使用して VM をプロビジョニングする、 **かつ**
   * イメージは追加のライセンス プランの購入を必要としない。
     
     イメージの使用を許可しない場合、または使用できるようにするイメージを指定する場合は、 **[いいえ]** を選択します。
     
     ![すべての Marketplace イメージを VM のベース イメージとして使用できるようにするオプション](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. 前の手順で **[いいえ]** を選択した場合は、 **[Allowed images (許可するイメージ)] の [すべて選択]** チェック ボックスが有効になります。 
   このオプションを検索ボックスと組み合わせて使用すると、一覧に表示されたすべてのアイテムをすばやく選択または選択解除することができます。
   * Azure Marketplace イメージの対応するチェック ボックスをオンにすると、VM の作成に使用するイメージが個別に選択されます。
   * どの Azure Marketplace イメージもラボでの使用を許可しない場合は、一覧で何も選択しないでください。
   
     ![どの Azure Marketplace イメージを VM のベース イメージとして使用できるようにするかを指定できる](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>次のステップ
VM を作成するときに Azure Marketplace イメージを使用できるようにする方法を構成したら、次は [VM をラボに追加](devtest-lab-add-vm.md)します。


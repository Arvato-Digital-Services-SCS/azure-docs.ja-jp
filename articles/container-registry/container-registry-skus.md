---
title: Azure Container Registry SKU
description: Azure Container Registry の Basic、Standard、および Premium サービス階層 (SKU) の機能と制限について説明します。
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 11/05/2019
ms.author: danlep
ms.openlocfilehash: 19b1fb78413f82d422779b12227b4a5e2361d813
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681812"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry SKU

Azure Container Registry (ACR) は、SKU と呼ばれる複数のサービス階層で使用できます。 これらの SKU は、Azure におけるプライベート Docker レジストリの容量と使用パターンに合ったさまざまなオプションと予測可能な価格を提供します。

| SKU | 説明 |
| --- | ----------- |
| **Basic** | Azure Container Registry について学習する開発者向けのコスト最適化エントリ ポイント。 Basic レジストリには Standard および Premium と同じプログラム機能があります (Azure Active Directory [認証の統合](container-registry-authentication.md#individual-login-with-azure-ad)、[イメージ削除][container-registry-delete]、[Webhook][container-registry-webhook] など)。 ただし、含まれているストレージとイメージのスループットは、使用率が低いシナリオに最も適しています。 |
| **Standard** | Standard レジストリは、Basic と同じ機能を提供しますが、含まれているストレージとイメージ スループットが拡大されています。 Standard レジストリは、ほとんどの運用シナリオのニーズを満たすはずです。 |
| **Premium** | Premium レジストリは、含まれているストレージが最も大きく、同時実行操作数も最大であり、大容量シナリオに対応できます。 イメージのスループットの増加に加え、Premium では、複数のリージョン間で 1 つのレジストリを管理するための [geo レプリケーション][container-registry-geo-replication]、イメージ タグに署名するための[コンテンツの信頼](container-registry-content-trust.md)、レジストリへのアクセスを制限する[ファイアウォールおよび仮想ネットワーク (プレビュー)](container-registry-vnet.md) などの機能が追加されています。 |

Basic、Standard、および Premium SKU は、すべて同じプログラム機能を提供しています。 また、これらすべては、Azure によって完全に管理されている[イメージ ストレージ][container-registry-storage]から恩恵を受けられます。 上位の SKU を選択するほど、パフォーマンスとスケールが向上します。 複数のサービス レベルがあることで、最初は Basic を導入し、その後、レジストリの使用量の増加に伴って Standard や Premium に切り替えることができます。

## <a name="sku-features-and-limits"></a>SKU の機能と制限

次の表に、Basic、Standard、および Premium サービス階層の機能と制限について説明します。

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKU の変更

レジストリの SKU 変更は、Azure CLI または Azure Portal で行うことができます。 切り替え先の SKU が最大記憶域容量の要件を満たしていれば、SKU 間で自由に移動することができます。 

### <a name="azure-cli"></a>Azure CLI

Azure CLI で SKU を切り替えるには、[az acr update][az-acr-update] コマンドを使用します。 たとえば Premium への切り替えは、次のようにして行います。

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure ポータル

Azure Portal のコンテナー レジストリの **[Overview]\(概要\)** で、 **[Update]\(更新\)** を選択し、[SKU] ドロップダウン リストから新しい **SKU** を選択します。

![Azure Portal でのコンテナー レジストリ SKU の更新][update-registry-sku]

## <a name="pricing"></a>価格

各 Azure Container Registry SKU の価格については、「[Container Registry の価格][container-registry-pricing]」を参照してください。

データ転送の価格に関する詳細については、「[帯域幅の料金詳細](https://azure.microsoft.com/pricing/details/bandwidth/)」を参照してください。 

## <a name="next-steps"></a>次の手順

**Azure Container Registry ロードマップ**

GitHub の [ACR ロードマップ][acr-roadmap]を参照して、サービスの今後の機能に関する情報を確認してください。

**Azure Container Registry UserVoice**

[ACR UserVoice][container-registry-uservoice] で、新機能に関する提案を送信し、投票してください。

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md

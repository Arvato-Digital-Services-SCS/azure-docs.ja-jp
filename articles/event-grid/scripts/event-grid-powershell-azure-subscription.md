---
title: Azure PowerShell - Azure サブスクリプションのサブスクライブ
description: Azure PowerShell と Azure Event Grid のサンプル スクリプト - Azure サブスクリプションのサブスクライブ
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/22/2019
ms.author: spelluru
ms.openlocfilehash: 8ab0d61e2c7ec23b7c3ed82d460cbb53711d5c7f
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790497"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-powershell"></a>PowerShell を使用した Azure サブスクリプションのイベントのサブスクライブ

このスクリプトは、Azure サブスクリプションのイベントに対する Event Grid サブスクリプションを作成します。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

プレビュー版サンプル スクリプトには、Event Grid モジュールが必要です。 インストールするには、`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery` を実行します

## <a name="sample-script---stable"></a>サンプル スクリプト - 安定版

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.ps1 "Subscribe to Azure subscription")]

## <a name="sample-script---preview-module"></a>サンプル スクリプト - プレビュー モジュール

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription-preview/subscribe-to-azure-subscription-preview.ps1 "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトは、次のコマンドを使用してイベント サブスクリプションを作成します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| command | メモ |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Event Grid のサブスクリプションを作成する。 |

## <a name="next-steps"></a>次の手順

* マネージド アプリケーションの概要については、「[Azure マネージド アプリケーションの概要](../overview.md)」を参照してください。
* PowerShell について詳しくは、[Azure PowerShell のドキュメント](https://docs.microsoft.com/powershell/azure/get-started-azureps)をご覧ください。

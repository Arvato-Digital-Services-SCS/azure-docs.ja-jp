---
title: Azure Firewall の FQDN タグの概要
description: Azure Firewall の FQDN タグについて説明します
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 4/23/2019
ms.author: victorh
ms.openlocfilehash: d78f67b96776fa3fb1d13fbf7b385e41ca3aa59a
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73665555"
---
# <a name="fqdn-tags-overview"></a>FQDN タグの概要

FQDN タグは、よく知られている Microsoft サービスに関連付けられた完全修飾ドメイン名 (FQDN) のグループを表します。 アプリケーション ルールで FQDN タグを使用すると、必要な送信ネットワーク トラフィックがファイアウォールを通過するのを許可できます。

たとえば、手動で Windows Update ネットワーク トラフィックがファイアウォールを通過できるようにするには、Microsoft のドキュメントに従って複数のアプリケーション ルールを作成する必要があります。 FQDN タグを使用すると、**Windows Updates** タグを含むアプリケーション ルールを作成できるため、Microsoft Windows Update エンドポイントへのネットワーク トラフィックがファイアウォールを通過できるようになります。

独自の FQDN タグを作成することも、タグに含める FQDN を指定することもできません。 Microsoft では、FQDN タグに含まれる FQDN を管理し、FQDN の変更に合わせてタグを更新しています。 

<!--- screenshot of application rule with a FQDN tag.-->

次の表に、現在使用できる FQDN タグを示します。 これらのタグは Microsoft によって管理されており、定期的に追加される予定です。

## <a name="current-fqdn-tags"></a>現在の FQDN タグ

|FQDN タグ  |説明  |
|---------|---------|
|Windows Update     |「[ソフトウェアの更新用にファイアウォールを構成する方法](https://technet.microsoft.com/library/bb693717.aspx)」で説明されているように、Microsoft Update への発信アクセスを許可します。|
|Windows 診断|すべての [Windows 診断のエンドポイント](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints)への発信アクセスを許可します。|
|Microsoft Active Protection Service (MAPS)|[MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/) への発信アクセスを許可します。|
|App Service Environment (ASE)|ASE プラットフォーム トラフィックへの発信アクセスを許可します。 このタグの対象には、ASE によって作成されたお客様固有のストレージ エンドポイントおよび SQL エンドポイントは含まれません。 これらは、[サービス エンドポイント](../virtual-network/tutorial-restrict-network-access-to-resources.md)を使用して有効にするか、手動で追加する必要があります。<br><br>Azure Firewall と ASE の統合の詳細については、「[App Service 環境をロックする](../app-service/environment/firewall-integration.md#configuring-azure-firewall-with-your-ase)」を参照してください。|
|Azure Backup|Azure Backup サービスへの発信アクセスを許可します。|
|Azure HDInsight|HDInsight プラットフォーム トラフィックへの発信アクセスを許可します。 このタグでは、HDInsight からのお客様固有のストレージ トラフィックまたは SQL トラフィックは対象になりません。 [サービス エンドポイント](../virtual-network/tutorial-restrict-network-access-to-resources.md)を使用してこれらを有効にするか、手動で追加します。|

> [!NOTE]
> アプリケーション ルールで FQDN タグを選択する場合は、[プロトコル:ポート] フィールドを **[https]** に設定する必要があります。

## <a name="next-steps"></a>次の手順

Azure ファイアーウォールのデプロイ方法については、「[Tutorial: Deploy and configure Azure Firewall using the Azure portal (チュートリアル: Azure portal を使用して Azure Firewall のデプロイと構成を行う)](tutorial-firewall-deploy-portal.md)」を参照してください。

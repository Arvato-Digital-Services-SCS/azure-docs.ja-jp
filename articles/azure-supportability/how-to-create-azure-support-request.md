---
title: Azure サポート要求を作成する方法 | Microsoft Docs
description: サポートが必要なお客様は、Azure portal を使用して、セルフサービス ソリューションを見つけたり、サポート リクエストを作成および管理したりできます。
services: Azure Supportability
author: ganganarayanan
manager: scotthit
ms.assetid: fd6841ea-c1d5-4bb7-86bd-0c708d193b89
ms.service: azure-supportability
ms.topic: article
ms.date: 11/13/2019
ms.author: kfollis
ms.openlocfilehash: fbcf534b549a12887732ce5ae4db1e4d45692791
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131064"
---
# <a name="how-to-create-an-azure-support-request"></a>Azure サポート要求を作成する方法

## <a name="overview"></a>概要

Azure のお客様は、[Azure portal](https://portal.azure.com) でサポート要求を作成し、管理できます。

> [!NOTE]
> Azure portal の URL は、組織がデプロイされている Azure クラウドに固有のものです。 
>* 商用利用の Azure portal は次のとおりです。[https://portal.azure.com](https://portal.azure.com)
>* ドイツの Azure portal は次のとおりです: [https://portal.microsoftazure.de](https://portal.microsoftazure.de)
>* 米国政府機関向けの Azure portal は次のとおりです: [https://portal.azure.us](https://portal.azure.us)
>
>

お客様からいただいたご意見とご要望に基づき、サポート要求のシステムを改善しました。大きな目標として次の 3 つに取り組んでいます。

* **合理化**:サポートとトラブルシューティングを見つけやすくして、サポート リクエストの送信方法を簡略化します。
* **統合**:Azure リソースに関する問題のトラブルシューティングを行う際に、コンテキストを切り替えずに、サポート リクエストを容易に開くことができます。
* **効率性**: 問題を効率的に解決するために、お客様のサポート エージェントが必要とする重要な情報を集めます。

## <a name="getting-started"></a>使用の開始

Azure portal で **[ヘルプとサポート]** にアクセスできます。 それは、グローバル ヘッダーから、またはサービスのリソース メニューから使用できます。

### <a name="go-to-help--support-from-the-global-header"></a>グローバル ヘッダーから [ヘルプとサポート] にアクセスする

Azure portal 内の任意の場所からサポート リクエストを開始するには、次の操作を行います。

1. **[?]** を グローバル ヘッダーで選択します。 次に、 **[ヘルプとサポート]** を選択します。

   ![ヘルプとサポート](./media/how-to-create-azure-support-request/helpandsupportnewlower.png)

2. **[+ 新しいサポート リクエスト]** を選択します。 画面の指示に従って、発生している問題に関する情報を Microsoft に提供します。 Microsoft は考えられる解決策をいくつか提案し、問題の詳細を収集し、サポート リクエストの送信と追跡を支援します。

   ![新しいサポート要求](./media/how-to-create-azure-support-request/newsupportrequest2lower.png)

### <a name="go-to-help--support-from-a-resource-menu"></a>リソース メニューから [ヘルプとサポート] にアクセスする

リソースのコンテキストでサポート リクエストを開始するには、現在は次の操作を行います。

1. リソース メニューの **[サポート + トラブルシューティング]** セクションで、 **[新しいサポート リクエスト]** を選択します。

   ![コンテキストで](./media/how-to-create-azure-support-request/incontext2lower.png)

2. 画面の指示に従って、発生している問題に関する情報を Microsoft に提供します。 リソースからサポート リクエスト プロセスを開始すると、いくつかのオプションがあらかじめ自動的に選択されています。

## <a name="create-a-support-request"></a>サポート要求の作成

Microsoft は、発生している問題について情報を収集してその解決を支援するためのいくつかの手順について説明します。 各手順については、次のセクションで説明します。

### <a name="basics"></a>基本

サポート要求プロセスの最初の手順は、ユーザーが抱える問題とそのサポート プランに必要な基本情報を集めることです。

**[新しいサポート リクエスト]** の **[基本]** タブで、セレクターを使用して、Microsoft への問題の説明を開始します。 まず、問題の種類の一般的なカテゴリを特定し、関連するサブスクリプションを選択します。 サービス ("Windows を実行する仮想マシン" など) とリソース (ご利用の仮想マシンの名前) を選択することが、ヘルプを得るための次の手順となります。 ご自分の言葉で問題を簡潔に説明してから、 **[問題の種類を選択してください]** を選択して、より具体的になるようにします。

![[基本] ブレード](./media/how-to-create-azure-support-request/basics2lower.png)

> [!NOTE]
> Azure は、サブスクリプション管理 (請求、割り当て調整、アカウント振り替えなど) に無制限のサポートを提供します。 技術的なサポートについては、サポート プランが必要になります。 [サポート プランについて詳しく見る](https://azure.microsoft.com/support/plans)。
>
>

### <a name="solutions"></a>解決方法

Microsoft は基本的な情報を収集できたら、次にユーザーにソリューションを提示して、お客様に試してもらいます。 場合によっては、クイック診断を実行することもあります。 ソリューションは Azure エンジニアによって作成されたものであり、大抵の一般的な問題はこれで解決します。

### <a name="details"></a>詳細

次に、Microsoft は問題に関する追加の詳細を収集します。 この手順で徹底的かつ詳細な情報が提供されれば、Microsoft はお客様のサポート リクエストを適切なエージェントに転送し、問題の診断を開始させることができます。

問題が発生し始めた時期およびそれを再現するための手順をお知らくださることをお勧めします。 また、ログ ファイルや診断の出力などのファイルをアップロードすることもできます。

発生している問題に関する情報をすべて Microsoft に提供したら、サポートを受ける方法を選択できるようになります。 **[詳細]** タブの **[サポート方法]** セクションで、影響の重大度を選択します。 ご自分の希望する連絡方法、連絡のための都合のよいタイミング、およびサポート言語を指定します。

次に、 **[連絡先情報]** セクションに入力して、ご自分への連絡方法をお知らせください。

### <a name="review--create"></a>確認と作成

各タブですべての必須情報を入力して、 **[確認および作成]** を選択します。 サポートに送信する詳細情報を確認します。 必要に応じて、任意のタブに戻り、変更を行います。 完了したサポート リクエストが適切であれば、 **[作成]** を選択します。

サポート エージェントが、指定された方法を使用してお客様に連絡します。 初回の応答時間については、「[サポート内容と応答性](https://azure.microsoft.com/support/plans/response/)」を参照してください。

## <a name="all-support-requests"></a>すべてのサポート リクエスト

サポート リクエストの詳細と状態を表示するには、 **[ヘルプとサポート]**  >   **[すべてのサポート リクエスト]** の順に移動します。

![すべてのサポート リクエスト](./media/how-to-create-azure-support-request/allrequestslower.png)

このページでは、サブスクリプション、作成日 (UTC)、および状態によってサポート リクエストをフィルター処理することができます。 また、このページでサポート要求を並べ替えたり検索したりすることもできます。

サポート リクエストを選択すると、重大度やサポート エージェントが応答するまでの予想時間など、詳細が表示されます。

リクエストの重大度を変更する場合は、 **[事業影響度]** を選択します。 割り当てることができる重大度の一覧が表示されます。

> [!NOTE]
> 最高重大度はサポート プランごとに異なります。 [サポート プランについて詳しく見る](https://azure.microsoft.com/support/plans)。
>
>
Azure のセルフヘルプ サポート オプションの詳細については、次のビデオをご覧ください。

> [!VIDEO https://www.youtube.com/embed/gNhzR5FE9DY]

## <a name="next-steps"></a>次の手順
* [お客様からのフィードバックとご提案の送信](https://feedback.azure.com/forums/266794-support-feedback)
* [Twitter](https://twitter.com/azuresupport) でのご協力
* [MSDN フォーラムで同僚からのヘルプを得る](https://social.msdn.microsoft.com/Forums/azure)
* [Azure サポートに関する FAQ で詳細を確認する](https://azure.microsoft.com/support/faq)

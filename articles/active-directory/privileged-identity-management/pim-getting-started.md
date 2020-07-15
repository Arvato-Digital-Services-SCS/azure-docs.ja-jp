---
title: PIM の使用を開始する - Azure Active Directory | Microsoft Docs
description: Azure portal で Azure AD Privileged Identity Management (PIM) を有効にして使用を開始する方法を説明します。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: how-to
ms.workload: identity
ms.date: 04/23/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: cad6c41b465e14b11d4fa7b8e7fa6037d49e8eae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742914"
---
# <a name="start-using-privileged-identity-management"></a>Privileged Identity Management の使用開始

この記事では、Privileged Identity Management (PIM) を有効にしてその使用を開始する方法について説明します。

Privileged Identity Management (PIM) を使用すると、Azure Active Directory (Azure AD) 組織内のアクセス権を管理、制御、および監視できます。 PIM を使用すると、Azure リソース、Azure AD リソースのほか、Office 365 や Microsoft Intune などのその他の Microsoft オンライン サービスへのアクセスを、必要に応じて適切なタイミングで提供できます。

## <a name="prerequisites"></a>前提条件

Privileged Identity Management を使用するには、次のライセンスのいずれかが必要です。

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

詳細については、「[Privileged Identity Management を使用するためのライセンスの要件](subscription-requirements.md)」を参照してください。

## <a name="prepare-pim-for-azure-ad-roles"></a>Azure AD ロール用に PIM を準備する

ディレクトリに対して Privileged Identity Management を有効にしたら、Azure AD ロールを管理するために Privileged Identity Management を準備できます。

Azure AD ロールを準備するために推奨されるタスクは次のとおりです。

1. [Azure AD ロールの設定を構成する](pim-how-to-change-default-settings.md)。
1. [資格に応じて割り当てる](pim-how-to-add-role-to-user.md)。
1. [資格のあるユーザーが Azure AD ロールを適切なタイミングでアクティブ化できるようにする](pim-how-to-activate-role.md)。

## <a name="prepare-pim-for-azure-roles"></a>Azure ロール用に PIM を準備する

ディレクトリに対して Privileged Identity Management を有効にしたら、サブスクリプションの Azure リソース アクセス用の Azure ロールを管理するために Privileged Identity Management を準備できます。

Azure ロールを準備するために推奨されるタスクは次のとおりです。

1. [Azure リソースを見つける](pim-resource-roles-discover-resources.md)
1. [Azure ロールの設定を構成する](pim-resource-roles-configure-role-settings.md)。
1. [資格に応じて割り当てる](pim-resource-roles-assign-roles.md)。
1. [資格のあるユーザーが Azure ロールを適切なタイミングでアクティブ化できるようにする](pim-resource-roles-activate-your-roles.md)。

## <a name="navigate-to-your-tasks"></a>タスクへの移動

Privileged Identity Management の設定が済んだら、使用してみることができます。

![[タスク] と [管理] のオプションを示す Privileged Identity Management のナビゲーション ウィンドウ](./media/pim-getting-started/pim-quickstart-tasks.png)

| タスク + 管理 | 説明 |
| --- | --- |
| **自分のロール**  | 自分に割り当てられている適格でアクティブなロールの一覧が表示されます。 ここでは、割り当てられている適格なロールをアクティブにできます。 |
| **個人の要求** | 適格なロール割り当てのアクティブ化の保留中要求が表示されます。 |
| **申請の承認** | ディレクトリ内のユーザーによる適格なロールのアクティブ化要求のうち、自分が承認するものの一覧が表示されます。 |
| **アクセスのレビュー** | 自分に完了が割り当てられているアクティブなアクセス レビューが一覧表示されます (自分自身のアクセスをレビューするものと、他のユーザーのアクセスをレビューするものの両方)。 |
| **Azure AD ロール** | Azure AD ロールの割り当てを管理するための、特権ロール管理者向けのダッシュボードと設定が表示されます。 このダッシュボードは、特権ロール管理者以外に対しては無効になっています。 これらのユーザーは、[自分のビュー] という特殊なダッシュボードにアクセスできます。 [自分のビュー] ダッシュボードには、組織全体ではなく、ダッシュボードにアクセスしているユーザーに関する情報のみが表示されます。 |
| **Azure リソース** | Azure リソース ロールの割り当てを管理するための、特権ロール管理者向けのダッシュボードと設定が表示されます。 このダッシュボードは、特権ロール管理者以外に対しては無効になっています。 これらのユーザーは、[自分のビュー] という特殊なダッシュボードにアクセスできます。 [自分のビュー] ダッシュボードには、組織全体ではなく、ダッシュボードにアクセスしているユーザーに関する情報のみが表示されます。 |

## <a name="add-a-pim-tile-to-the-dashboard"></a>ダッシュボードに PIM タイルを追加する

Privileged Identity Management を簡単に開けるようにするには、Azure portal ダッシュボードに PIM タイルを追加します。

1. [Azure portal](https://portal.azure.com/) にサインインします。

1. **[すべてのサービス]** を選択し、**Azure AD Privileged Identity Management** サービスを探します。

    ![[すべてのサービス] でのAzure AD Privileged Identity Management](./media/pim-getting-started/pim-all-services-find.png)

1. [Privileged Identity Management Quickstart]\(Privileged Identity Management のクイックスタート\) を選択します。

1. [Privileged Identity Management Quickstart] (Privileged Identity Management のクイックスタート) ブレードをダッシュボードにピン留めするには、 **[ダッシュボードにブレードをピン留め]** をオンにします。

    ![[Privileged Identity Management] ブレードをダッシュボードにピン留めする画びょうアイコン](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure ダッシュボードでは、タイルが次のように表示されます。

    ![ダッシュボード上の [Privileged Identity Management Quickstart] (Privileged Identity Management のクイックスタート) タイル](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>次のステップ

- [Privileged Identity Management で Azure AD ロールを割り当てる](pim-how-to-add-role-to-user.md)
- [Privileged Identity Management で Azure リソースのアクセス権を管理する](pim-resource-roles-discover-resources.md)

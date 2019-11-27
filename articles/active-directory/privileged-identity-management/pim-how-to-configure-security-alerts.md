---
title: PIM の Azure AD ロールに対するセキュリティ アラート - Azure AD | Microsoft Docs
description: Azure Active Directory で Azure AD ロール Privileged Identity Management に対するセキュリティ アラートを構成します。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e05502b40f7f917526b2514d83dcc58636ac06f3
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2019
ms.locfileid: "74023039"
---
# <a name="configure-security-alerts-for-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management で Azure AD ロールに対するセキュリティ アラートを構成する

Azure Active Directory (Azure AD) 組織内で疑わしいアクティビティや危険なアクティビティが行われると、Privileged Identity Management (PIM) によりアラートが生成されます。 アラートは、トリガーされると Privileged Identity Management ダッシュボードに表示されます。 アラートを選択して、アラートをトリガーしたユーザーまたはロールが一覧表示されたレポートを表示します。

![Azure AD ロール - アラートと重大度を一覧表示するアラート ウィンドウ](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>セキュリティのアラート

このセクションでは、Azure AD ロールのすべてのセキュリティ アラートの一覧を、修正方法および回避方法と共に示します。 重大度には、次のような意味があります。

- **高**: ポリシー違反のため直ちに対処が必要です。
- **中**: 早急の対処は必要ありませんが、ポリシー違反の可能性が通知されています。
- **低**: 早急な対処は必要ありませんが、望ましいポリシー変更が提案されています。

### <a name="administrators-arent-using-their-privileged-roles"></a>管理者が特権ロールを使用してません

| | |
| --- | --- |
| **重大度** | 低 |
| **このアラートが表示される理由** | 特権ロールが割り当てられているユーザーは、攻撃の可能性を高める必要がありません。 アクティブに使用されていないアカウントでは、攻撃者が気付かれずに居続けることもより簡単です。 |
| **修正方法** | リスト内のユーザーを確認し、必要のない特権ロールから、これらのユーザーを削除します。 |
| **防止** | 業務上の正当な理由があるユーザーにのみ、特権ロールを割り当てます。 </br>定期的な[アクセス レビュー](pim-how-to-start-security-review.md)をスケジュールして、ユーザーにまだそれらのアクセスが必要なことを確認します。 |
| **ポータル内での軽減策のアクション** | 対象の特権ロールからアカウントを削除します。 |
| **トリガー** | ユーザーがロールをアクティブ化しないで一定の日数が経過するとトリガーされます。 |
| **日数** | この設定では、ユーザーがロールをアクティブ化しないままでいられる最大日数を 0 から 100 で指定します。|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>ロールをアクティブ化するのに多要素認証は必要ありません

| | |
| --- | --- |
| **重大度** | 低 |
| **このアラートが表示される理由** | Multi-Factor Authentication を使用しないと、侵害されたユーザーが特権ロールをアクティブ化できます。 |
| **修正方法** | ロールのリストを確認し、すべてのロールに対して [Multi-Factor Authentication を要求](pim-how-to-change-default-settings.md)します。 |
| **防止** | すべてのロールに対して [MFA を要求](pim-how-to-change-default-settings.md)します。  |
| **ポータル内での軽減策のアクション** | 特権ロールのアクティブ化には Multi-Factor Authentication が必要となるようにします。 |

### <a name="the-organization-doesnt-have-azure-ad-premium-p2"></a>組織に Azure AD Premium P2 がない

| | |
| --- | --- |
| **重大度** | 低 |
| **このアラートが表示される理由** | 現在の Azure AD 組織に、Azure AD Premium P2 がありません。 |
| **修正方法** | [Azure AD のエディション](../fundamentals/active-directory-whatis.md)に関する情報を確認します。 Azure AD Premium P2 にアップグレードします。 |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Potential stale accounts in a privileged role (特権ロール内のアカウントが古い可能性があります)

| | |
| --- | --- |
| **重大度** | Medium |
| **このアラートが表示される理由** | 過去 90 日間にパスワードを変更していない特権ロール内のアカウントです。 このようなアカウントは、保守されておらず、攻撃者に対して脆弱なサービス アカウントまたは共有アカウントの可能性があります。 |
| **修正方法** | リスト内のアカウントを確認します。 もうアクセスが不要になっているアカウントは、特権ロールから削除します。 |
| **防止** | パスワードを知っているユーザーの変更がある場合に、共有されているアカウントが、強固なパスワードのローテーションを行っていることを確認します。 </br>[アクセス レビュー](pim-how-to-start-security-review.md)を使用して特権ロールを持つアカウントを定期的に確認し、不要になっているロールの割り当てを削除します。 |
| **ポータル内での軽減策のアクション** | 対象の特権ロールからアカウントを削除します。 |
| **ベスト プラクティス** | パスワードを使って認証を行い、グローバル管理者やセキュリティ管理者などの高い特権を持つ管理者ロールに割り当てられる共有、サービス、および緊急アクセス用のアカウントでは、次の場合にパスワードをローテーションする必要があります。<ul><li>管理アクセス権の悪用や侵害が関係するセキュリティ インシデントの後</li><li>管理者ではなくなったユーザーの特権を変更した後 (たとえば、IT 管理者であった従業員が IT 部門から異動したり退職したりした後)</li><li>IT スタッフの認識された侵害や変更がない場合でも、一定の間隔で (たとえば、毎四半期または毎年)</li></ul>これらのアカウントの資格情報には複数のユーザーがアクセスできるので、資格情報をローテーションして、それらのロールから抜けたユーザーがアカウントにアクセスできないようにする必要があります。 [詳細情報](https://aka.ms/breakglass) |

### <a name="roles-are-being-assigned-outside-of-privileged-identity-management"></a>ロールが Privileged Identity Management の外部に割り当てられている

| | |
| --- | --- |
| **重大度** | 高 |
| **このアラートが表示される理由** | Privileged Identity Management の外部で行われた特権ロールの割り当てが正しく監視されておらず、アクティブな攻撃を示している可能性があります。 |
| **修正方法** | リスト内のユーザーを確認し、Privileged Identity Management の外部で割り当てられた特権ロールから、これらのユーザーを削除します。 |
| **防止** | ユーザーが特権ロールを割り当てられた Privileged Identity Management の外部の場所を調査し、そこからの今後の割り当てを禁止します。 |
| **ポータル内での軽減策のアクション** | 対象の特権ロールからユーザーを削除します。 |

### <a name="there-are-too-many-global-administrators"></a>グローバル管理者が多すぎます

| | |
| --- | --- |
| **重大度** | 低 |
| **このアラートが表示される理由** | グローバル管理者は、最上位の特権ロールです。 グローバル管理者が侵害された場合、攻撃者はこの管理者のすべてのアクセス許可を使用できるようになり、システム全体が危険にさらされます。 |
| **修正方法** | リスト内のユーザーを確認し、必ずしもグローバル管理者ロールが必要ではないユーザーをすべて削除します。 </br>代わりに、下位の特権ロールをこれらのユーザーに割り当てます。 |
| **防止** | ユーザーには最低限必要な特権ロールを割り当てます。 |
| **ポータル内での軽減策のアクション** | 対象の特権ロールからアカウントを削除します。 |
| **トリガー** | 2 つの異なる条件が満たされるとトリガーされます。これらの両方の条件を構成できます。 1 つ目に、グローバル管理者の数が特定のしきい値に達する必要があります。 2 つ目には、総ロール割り当て数のうち一定の割合がグローバル管理者である必要があります。 これらの測定値の一方のみが満たされている場合は、アラートは表示されません。 |
| **Minimum number of Global Administrators (グローバル管理者の最低人数)** | この設定では、Azure AD 組織には少なすぎると考えられるグローバル管理者の人数を 2 から 100 で指定します。 |
| **グローバル管理者の割合** | この設定では、グローバル管理者である管理者の最低限の割合を 0% から 100% で、Azure AD 組織で減少すべきではない割合より低く指定します。 |

### <a name="roles-are-being-activated-too-frequently"></a>ロールをアクティブ化する頻度が高すぎます

| | |
| --- | --- |
| **重大度** | 低 |
| **このアラートが表示される理由** | 同じユーザーが同じ特権ロールに対してアクティブ化を複数行っているのは、攻撃の兆候です。 |
| **修正方法** | リスト内のユーザーを確認し、それらのユーザーの特権ロールの[アクティブ化期間](pim-how-to-change-default-settings.md)が、タスクを実行するのに十分長い設定になっていることを確認します。 |
| **防止** | 特権ロールの[アクティブ化期間](pim-how-to-change-default-settings.md)が、タスクを実行するのに十分長い設定になっていることを確認します。</br>複数の管理者によって共有されるアカウントを持つ特権ロールの [Multi-Factor Authentication を要求](pim-how-to-change-default-settings.md)します。 |
| **ポータル内での軽減策のアクション** | 該当なし |
| **トリガー** | ユーザーが指定期間内に同じ特権ロールを複数回アクティブ化するとトリガーされます。 期間とアクティブ化の回数の両方を構成できます。 |
| **アクティブ化の更新の時間枠** | この設定では、不審な更新の追跡に使用する期間を日、時間、分、および秒で指定します。 |
| **Number of activation renewals (アクティブ化の更新回数)** | この設定では、選択した期間内に通知を受け取るアクティブ化の回数を 2 から 100 で指定します。 スライダーを動かすか、テキスト ボックスに数字を入力して設定を変更できます。 |

## <a name="configure-security-alert-settings"></a>セキュリティ アラート設定の構成

Privileged Identity Management のセキュリティ アラートの一部をカスタマイズして、組織のニーズやセキュリティ上の目標に対応することができます。 以下の手順に従ってセキュリティ アラートの設定を開きます。

1. Azure AD で **[Privileged Identity Management]** を開きます。

1. **[Azure AD ロール]** を選択します。

1. **[設定]** 、 **[アラート]** の順に選択します。

    ![Azure AD ロール - アラートが選択されている設定](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. そのアラートの設定を構成するアラート名を選択します。

    ![選択されたアラートの、セキュリティ アラートの設定ウィンドウ](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>次の手順

- [Privileged Identity Management で Azure AD ロールの設定を構成する](pim-how-to-change-default-settings.md)

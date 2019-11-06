---
title: Azure AD エンタイトルメント管理 (プレビュー) をトラブルシューティングする - Azure Active Directory
description: Azure Active Directory エンタイトルメント管理 (Preview) をトラブルシューティングする際に確認しておくべき事項について説明します。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea979731c27a8d332102c3215e80510994f2ab3f
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430242"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Azure AD エンタイトルメント管理 (プレビュー) をトラブルシューティングする

> [!IMPORTANT]
> Azure Active Directory (Azure AD) エンタイトルメント管理は現在、パブリック プレビュー段階です。
> このプレビュー バージョンはサービス レベル アグリーメントなしで提供されています。運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。
> 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

この記事では、Azure Active Directory (Azure AD) エンタイトルメント管理 (Preview) をトラブルシューティングする際に確認しておくべき事項について説明します。

## <a name="checklist-for-entitlement-management-administration"></a>エンタイトルメント管理のチェックリスト

* エンタイトルメント管理の構成時に、アクセスが拒否されたというメッセージが表示された場合、自分がグローバル管理者であるときは、ディレクトリに [Azure AD Premium P2 (または EMS E5) ライセンス](entitlement-management-overview.md#license-requirements)があることを確認してください。  
* アクセス パッケージの作成中または表示中に、アクセスが拒否されたというメッセージが表示された場合、自分がカタログ作成者グループのメンバーであるときは、最初のアクセス パッケージを作成する前にカタログを作成する必要があります。

## <a name="checklist-for-adding-a-resource"></a>リソースを追加する場合のチェックリスト

* アプリケーションがアクセス パッケージ内のリソースになるためには、割り当てることができるリソース ロールを少なくとも 1 つは持っている必要があります。 ロールはアプリケーション自体によって定義され、Azure AD で管理されます。 Azure portal には、アプリケーションとして選択できないサービスのサービス プリンシパルも表示される場合があることに注意してください。  特に、**Exchange Online** と **SharePoint Online** はサービスであり、ディレクトリにリソース ロールを持つアプリケーションではないため、アクセス パッケージに含めることができません。  代わりに、グループ ベースのライセンスを使用して、それらのサービスへのアクセスを必要とするユーザーに対して適切なライセンスを確立します。

* グループがアクセス パッケージ内のリソースになるためには、そのグループが Azure AD で変更可能である必要があります。  オンプレミスの Active Directory に由来するグループは、その所有者またはメンバー属性を Azure AD で変更できないため、リソースとして割り当てることができません。   Exchange Online で配布グループとして生成されるグループも、Azure AD で変更できません。 

* SharePoint Online のドキュメント ライブラリと個別のドキュメントはリソースとして追加できません。  代わりに、Azure AD セキュリティ グループを作成し、そのグループとサイト ロールをアクセス パッケージに含め、SharePoint Online でそのグループを使用してドキュメント ライブラリまたはドキュメントへのアクセスを制御します。

* アクセス パッケージで管理するリソースにユーザーが既に割り当てられている場合は、適切なポリシーを使用してユーザーがアクセス パッケージに割り当てられていることを確認します。 たとえば、既にユーザーがいるグループをアクセス パッケージに含めるとよいでしょう。 グループ内のこれらのユーザーが引き続きアクセスを必要とする場合、これらのユーザーは、アクセス パッケージに対する適切なポリシーを持ち、グループへのアクセスが不可能にならないようにする必要があります。 アクセス パッケージを割り当てるには、そのリソースを含むアクセス パッケージを要求するようにユーザーに依頼するか、またはアクセス パッケージにユーザーを直接割り当てます。 詳細については、[アクセス パッケージの要求と承認の設定の変更](entitlement-management-access-package-request-policy.md)に関する記事を参照してください。

## <a name="checklist-for-providing-external-users-access"></a>外部ユーザーにアクセスを提供するためのチェックリスト

* B2B [許可リスト](../b2b/allow-deny-list.md)がある場合、そのディレクトリが許可されていないユーザーはアクセスを要求できません。

* 外部ユーザーがアクセスを要求したり、アクセス パッケージ内のアプリケーションを使用したりできなくする[条件付きアクセス ポリシー](../conditional-access/require-managed-devices.md)が存在していないことを確認してください。

## <a name="checklist-for-request-issues"></a>要求を発行する場合のチェックリスト

* ユーザーがアクセス パッケージへのアクセスを要求する必要がある場合は、アクセス パッケージの**マイ アクセス ポータル リンク**をユーザーが使用していることを確認します。 詳細については、[アクセス パッケージを要求するためのリンクの共有](entitlement-management-access-package-settings.md)に関する記事を参照してください。  外部ユーザーが **myaccess.microsoft.com** にアクセスする場合、自分の組織内で使用可能なアクセス パッケージが表示されます。

* ディレクトリにまだ存在しないユーザーが、アクセス パッケージを要求するためにマイ アクセス ポータルにサインインする場合は、ユーザーが組織の自分のアカウントを使用して認証するようにします。 組織のアカウントとして使用できるのは、リソース ディレクトリ内のアカウント、またはアクセス パッケージのいずれかのポリシーに含まれるディレクトリ内のアカウントです。 ユーザーのアカウントが組織のアカウントでない場合、または認証するディレクトリがポリシーに含まれていない場合は、ユーザーにアクセス パッケージが表示されません。 詳細については、[アクセス パッケージへのアクセスの要求に関するページ](entitlement-management-request-access.md)を参照してください。

* リソース ディレクトリへのサインインがブロックされた場合、ユーザーは、マイ アクセス ポータルでアクセスを要求できなくなります。 ユーザーがアクセスを要求できるようにするには、ユーザーのプロファイルからサインインのブロックを削除する必要があります。 サインインのブロックを削除するには、Azure portal 内で **[Azure Active Directory]** 、 **[ユーザー]** をクリックし、ユーザーをクリックしてから **[プロファイル]** をクリックします。 **[設定]** セクションを編集し、 **[サインインのブロック]** を **[いいえ]** に変更します。 詳細については、「[Azure Active Directory を使用してユーザーのプロファイル情報を追加または更新する](../fundamentals/active-directory-users-profile-azure-portal.md)」を参照してください。  [ID 保護ポリシー](../identity-protection/howto-unblock-user.md)が原因でユーザーがブロックされたかどうかも確認できます。

* マイ アクセス ポータルの場合、ユーザーが要求者と承認者を兼ねているときは、 **[承認]** ページにアクセス パッケージの要求が表示されません。 これは、ユーザーが自分の要求を承認できないようにするための動作です。 ユーザーが要求しているアクセス パッケージで、別の承認者がポリシーに基づいて構成されていることを確認します。 詳細については、[アクセス パッケージの要求と承認の設定の変更](entitlement-management-access-package-request-policy.md)に関する記事を参照してください。

* ディレクトリにまだサインインしていない新規の外部ユーザーが、SharePoint Online サイトを含むアクセス パッケージを受信した場合、このアクセス パッケージは、ユーザーのアカウントが SharePoint Online でプロビジョニングされるまで、配信未完了として表示されます。

## <a name="next-steps"></a>次の手順

- [エンタイトルメント管理でユーザーがアクセス権をどのように取得したかについてのレポートを確認する](entitlement-management-reports.md)
- [外部ユーザーのアクセス権の管理](entitlement-management-external-users.md)

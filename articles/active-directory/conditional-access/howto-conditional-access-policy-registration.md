---
title: 条件付きアクセス - 統合されたセキュリティ情報 - Azure Active Directory
description: カスタムの条件付きアクセス ポリシーを作成して、信頼できる場所をセキュリティ情報の登録に必須とします
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 771e4e0ecbda4baf1f38aacd1f39397875bbd0dc
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73150762"
---
# <a name="conditional-access-require-trusted-location-for-mfa-registration"></a>条件付きアクセス:信頼できる場所を MFA 登録で必須にする

Azure Multi-Factor Authentication とパスワード リセットのセルフサービスの登録をユーザーがいつどのように行うかについてのセキュリティ保護が、条件付きアクセス ポリシーのユーザー アクションを使用して可能になりました。 このプレビュー機能は、[統合登録プレビュー](../authentication/concept-registration-mfa-sspr-combined.md)を有効にしている組織で使用することができます。 この機能は、HR オンボード中に信頼できるネットワークの場所などの一元化された場所から Azure Multi-Factor Authentication および SSPR の登録をユーザーに行わせたい組織で有効にすることができます。 条件付きアクセスでの信頼できる場所の作成について詳しくは、[Azure Active Directory 条件付きアクセスの場所の条件の概要](../conditional-access/location-condition.md#named-locations)に関する記事をご覧ください。

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>信頼できる場所からの登録を要求するポリシーを作成する

次のポリシーは、統合された登録エクスペリエンスを使用して登録を試みるすべての選択ユーザーに適用され、信頼されたネットワークとマークされている場所から接続している場合を除き、アクセスをブロックします。

1. **Azure portal** で、 **[Azure Active Directory]**  >  **[条件付きアクセス]** に移動します。
1. **[新しいポリシー]** を選択します。
1. [名前] に、このポリシーの名前を入力します。 たとえば、「**信頼されたネットワーク上の統合されたセキュリティ情報の登録**」など。
1. **[割り当て]** から **[ユーザーとグループ]** をクリックし、このポリシーを適用するユーザーとグループを選択します。

   > [!WARNING]
   > [[統合された登録プレビュー]](../authentication/howto-registration-mfa-sspr-combined.md) に対してユーザーを有効にする必要があります。

1. **[クラウド アプリまたはアクション]** から **[ユーザー アクション]** を選択し、 **[セキュリティ情報の登録 (プレビュー)]** をオンにします。
1. **[条件]**  >  **[場所]** で
   1. **[はい]** を構成します。
   1. **[任意の場所]** を含めます。
   1. **[すべての信頼できる場所]** を除外します。
   1. [場所] ブレードで **[完了]** をクリックします。
   1. [条件] ブレードで **[完了]** をクリックします。
1. **[アクセス制御]**  >  **[付与]** で
   1. **[アクセスのブロック]** をクリックします。
   1. **[選択]** をクリックします。
1. **[ポリシーを有効にする]** を **[オン]** に設定します。
1. **[Create]** をクリックします。

## <a name="next-steps"></a>次の手順

[Conditional Access common policies](concept-conditional-access-policy-common.md) (条件付きアクセスの一般的なポリシー)

[Simulate sign in behavior using the Conditional Access What If tool](troubleshoot-conditional-access-what-if.md) (条件付きアクセスの What If ツールを使用したサインイン動作のシミュレート)

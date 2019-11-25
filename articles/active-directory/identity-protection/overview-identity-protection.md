---
title: Azure Active Directory Identity Protection とは
description: Azure AD Identity Protection を使用したリスクの検出、修復、調査、および分析
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e928d67ba7102df3d342e77705ea895f9230ff3
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2019
ms.locfileid: "72887702"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection とは

Identity Protection は、組織が次の 3 つの主要なタスクを実行できるツールです。

- ID ベースのリスクの検出と修復を自動化します。
- ポータルのデータを使用してリスクを調査します。
- 詳細な分析のために、サードパーティ製ユーティリティにリスク検出データをエクスポートします。

Identity Protection は、Azure AD により組織について、Microsoft アカウントによりコンシューマー領域について、そして Xbox によりゲームについて得られた学習内容を使用して、ユーザーを保護します。 Microsoft は、脅威を特定し顧客を保護するために、1 日あたり 6 兆 5000 億の信号を分析します。

Identity Protection によって生成された信号、受信した信号は、条件付きアクセスなどのツールにさらに送信してアクセスに関する決定を行うことができます。また、セキュリティ情報とイベント管理 (SIEM) ツールに送り戻して、組織の適用するポリシーに基づく詳細な調査を行うこともできます。

## <a name="why-is-automation-important"></a>自動化が重要な理由

Microsoft の ID セキュリティと保護チームを主導する Alex Weinert による[ 2018 年 10 月のブログ記事](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Eight-essentials-for-hybrid-identity-3-Securing-your-identity/ba-p/275843)では、イベントのボリュームを処理するときに自動化が非常に重要な理由について説明しています。

> 機械学習およびヒューリスティック システムは、毎日 8 億の異なるアカウントに対して 180 億回のログイン試行のリスクスコアを提供します。そのうち 3 億は、明らかに敵対者 (例: 犯罪アクター、ハッカー) によるものです。
>
> 昨年の Ignite では、ID システムが受ける上位 3 つの攻撃について説明しました。 これらの攻撃の最近のボリュームは次のとおりです
>   
>   - **侵害のリプレイ**:2018 年 5 月に 46 億件の攻撃を検出
>   - **パスワード スプレー**:2018 年 4 月に 35 万件
>   - **フィッシング**:これは正確に定量化するのは困難ですが、2018 年 3 月には 2,300 万件のリスクイベントがあり、その多くはフィッシング関連です

## <a name="risk-detection-and-remediation"></a>リスクの検出と修復

Identity Protection は、次の分類のリスクを識別します。

| リスク検出の種類 | 説明 |
| --- | --- |
| 特殊な移動 | ユーザーの最近のサインインに基づき特殊と判断された場所からのサインイン。 |
| 匿名 IP アドレス | 匿名の IP アドレスからのサインイン (例:Tor Browser、Anonymizer VPN)。 |
| 通常とは異なるサインイン プロパティ | 指定されたユーザーで最近観察されていないプロパティを使用したサインイン。 |
| マルウェアにリンクした IP アドレス | マルウェアにリンクした IP アドレスからのサインイン |
| 資格情報の漏洩 | このリスク検出は、ユーザーの有効な資格情報が漏洩したことを示します |
| Azure AD 脅威インテリジェンス | Microsoft の内部および外部の脅威インテリジェンス ソースが既知の攻撃パターンを特定しました |

これらのリスクとその計算方法の詳細については、「[リスクとは](concept-identity-protection-risks.md)」を説明する記事を参照してください。

リスクシグナルは、Azure Multi-Factor Authentication の実行、セルフサービスのパスワード リセットを使用したパスワードのリセット、管理者がアクションを実行するまでのブロックなど、修復作業をトリガーすることができます。

## <a name="risk-investigation"></a>リスクの調査

管理者は検出内容を確認し、必要に応じて手動で操作を行うことができます。 Identity Protection では、管理者が調査に使用する 3 つの主要なレポートがあります。

- 危険なユーザー
- リスクの高いサインイン
- リスク検出

詳細については以下に関する記事を参照してください。[リスクを調査する方法](howto-identity-protection-investigate-risk.md)。

## <a name="exporting-risk-data"></a>リスク データのエクスポート

Identity Protection からのデータを他のツールにエクスポートしてアーカイブしたり、さらに調査したり、関連付けしたりすることができます。 Microsoft Graph ベースの API を使用すると、組織はこのデータを収集して、SIEM などのツールでさらに処理することができます。 Identity Protection API へのアクセス方法の詳細については、「[Azure Active Directory Identity Protection と Microsoft Graph の基本](howto-identity-protection-graph-api.md)」を参照してください

Identity Protection の情報と Azure Sentinel の統合に関する情報については、「[Azure AD Identity Protection からデータを接続する](../../sentinel/connect-azure-ad-identity-protection.md)」を参照してください。

## <a name="permissions"></a>アクセス許可

Identity Protection にユーザーがにアクセスするためには、セキュリティ閲覧者、セキュリティ オペレーター、セキュリティ管理者、グローバル閲覧者、またはグローバル管理者である必要があります。

## <a name="license-requirements"></a>ライセンスの要件

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

| 機能 | 詳細 | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/Free |
| --- | --- | --- | --- | --- |
| リスク ポリシー | ユーザー リスク ポリシー (Identity Protection 経由) | はい | いいえ | いいえ |
| リスク ポリシー | サインイン リスク ポリシー (Identity Protection または条件付きアクセス経由) | はい | いいえ | いいえ |
| セキュリティ レポート | 概要 | はい | いいえ | いいえ |
| セキュリティ レポート | 危険なユーザー | フル アクセス | 限定的な情報 | 限定的な情報 |
| セキュリティ レポート | リスクの高いサインイン | フル アクセス | 限定的な情報 | 限定的な情報 |
| セキュリティ レポート | リスク検出 | フル アクセス | 限定的な情報 | いいえ |
| 通知 | 危険な状態のユーザーが検出されたアラート | はい | いいえ | いいえ |
| 通知 | 週間ダイジェスト | はい | いいえ | いいえ |
| | MFA 登録ポリシー | はい | いいえ | いいえ |

## <a name="next-steps"></a>次の手順

- [セキュリティの概要](concept-identity-protection-security-overview.md)

- [リスクとは](concept-identity-protection-risks.md)

- [リスクを軽減するために使用できるポリシー](concept-identity-protection-policies.md)

---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: baf39c1fe72703d8ebc18b03bae1c963536d7ced
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475200"
---
アプリケーションを Azure AD B2C テナントに登録するには、現在の**アプリケーション** エクスペリエンス、または新しく統合された**アプリの登録 (プレビュー)** エクスペリエンスを使用できます。 [プレビュー エクスペリエンスの詳細を参照してください](https://aka.ms/b2cappregintro)。

#### <a name="applicationstabapplications"></a>[アプリケーション](#tab/applications/)

1. [Azure Portal](https://portal.azure.com) にサインインします。
1. 上部のメニューにある **[ディレクトリ + サブスクリプション]** フィルターを選択し、Azure AD B2C テナントを含むディレクトリを選択します。
1. 左側のメニューで **[Azure Active Directory]** (Azure AD B2C *ではない*) を選択します。 または、 **[すべてのサービス]** を選択してから、 **[Azure Active Directory]** を検索して選択します。
1. **[管理]** の **[アプリの登録 (レガシ)]** を選択します。
1. **[新しいアプリケーションの登録]** を選択します。
1. アプリケーションの名前を入力します。 たとえば、*managementapp1* と入力します。
1. **[アプリケーションの種類]** で、 **[Web アプリ / API]** を選択します。
1. **[サインオン URL]** に、有効な URL を入力します。 たとえば、「 `https://localhost` 」のように入力します。 エンドポイントを到達可能にする必要はありませんが、有効な URL にする必要があります。
1. **作成** を選択します。
1. **[登録済みのアプリケーション]** の概要ページに表示される **[アプリケーション ID]** を記録します。 この値は、後の手順で使用します。

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[アプリの登録 (プレビュー)](#tab/app-reg-preview/)

1. [Azure Portal](https://portal.azure.com) にサインインします。
1. 上部のメニューにある **[ディレクトリ + サブスクリプション]** フィルターを選択し、Azure AD B2C テナントを含むディレクトリを選択します。
1. 左側のメニューで、 **[Azure AD B2C]** を選択します。 または、 **[すべてのサービス]** を選択し、 **[Azure AD B2C]** を検索して選択します。
1. **[アプリの登録 (プレビュー)]** 、 **[新規登録]** の順に選択します。
1. アプリケーションの**名前**を入力します。 たとえば、*managementapp1* と入力します。
1. **[この組織のディレクトリ内のアカウントのみ]** を選択します。
1. **[アクセス許可]** で、 *[openid と offline_access アクセス許可に対して管理者の同意を付与します]* チェック ボックスをオフにします。
1. **[登録]** を選択します。
1. アプリケーションの概要ページに表示されている **[アプリケーション (クライアント) ID]** を記録します。 この値は、後の手順で使用します。
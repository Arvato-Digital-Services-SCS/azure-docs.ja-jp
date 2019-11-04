---
title: Azure Active Directory と Azure リソースのマネージド ID を使用して BLOB およびキューへのアクセスを承認する - Azure Storage
description: Azure の BLOB とキュー ストレージでは、Azure Active Directory と Azure リソースのマネージド ID を使用したアクセスの承認がサポートされています。 Azure リソースのマネージド ID を使用して、Azure の仮想マシン、関数アプリ、仮想マシン スケール セット、およびその他で実行されているアプリケーションからの BLOB とキューへのアクセスを承認することができます。
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 833aa7dcce5c429b3005a378e93e2177df1eb0d4
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595176"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Azure Active Directory と Azure リソースのマネージド ID を使用して BLOB とキューへのアクセスを承認する

Azure の Blob およびキュー ストレージは、Azure Active Directory (Azure AD) 認証を[ Azure リソースのマネージド ID ](../../active-directory/managed-identities-azure-resources/overview.md)を使用してサポートします。 Azure リソースのマネージド ID により、Azure 仮想マシン (VM) で実行されているアプリケーション、関数アプリ、仮想マシン スケール セット、およびその他のサービスから Azure AD 資格情報を使用して、BLOB およびキューのデータへのアクセスを認証することができます。 Azure リソースのマネージド ID を Azure AD 認証と一緒に使用することで、クラウドで動作するアプリケーションに資格情報を保存することを避けることができます。  

この記事では、Azure リソースにマネージド ID を使用して Azure VM から Blob またはキュー データへのアクセスを認証する方法について示します。 また、開発環境でコードをテストする方法についても説明します。

## <a name="enable-managed-identities-on-a-vm"></a>VM 上のマネージド ID を有効にする

Azure リソースのマネージド ID を使用して VM から BLOB およびキューへのアクセスを認証するには、最初に VM で Azure リソースのマネージド ID を有効にする必要があります。 Azure リソースのマネージド ID を有効にする方法については、次の記事のいずれかを参照してください。

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager テンプレート](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager クライアント ライブラリ](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

マネージド ID の詳細については、[Azure リソースのマネージド ID](../../active-directory/managed-identities-azure-resources/overview.md) に関するページを参照してください。

## <a name="authenticate-with-the-azure-identity-library-preview"></a>Azure ID ライブラリを使用した認証 (プレビュー)

.NET 対応の Azure ID クライアント ライブラリ (プレビュー) では、セキュリティ プリンシパルが認証されます。 コードが Azure 上で実行されている場合、セキュリティ プリンシパルは Azure リソースに対するマネージド ID です。

開発環境でコードを実行している場合は、認証が自動的に処理されるか、使用しているツールに応じてブラウザー ログインが必要になることがあります。 Microsoft Visual Studio では、アクティブな Azure AD ユーザー アカウントが自動的に認証に使用されるように、シングル サインオン (SSO) がサポートされます。 SSO の詳細については、[アプリケーションへのシングル サインオン](../../active-directory/manage-apps/what-is-single-sign-on.md)に関するページを参照してください。

他の開発ツールでは、Web ブラウザー経由でログインするように求められる場合があります。 また、サービス プリンシパルを使用して、開発環境から認証することもできます。 詳細については、[ポータルでの Azure アプリ用の ID 作成](../../active-directory/develop/howto-create-service-principal-portal.md)に関するページを参照してください。

認証後、Azure ID クライアント ライブラリでは、トークン資格情報を取得します。 このトークン資格情報は、Azure Storage に対して操作を実行するために作成するサービス クライアント オブジェクトにカプセル化されます。 ライブラリでは、適切なトークン資格情報を取得することで、これをシームレスに処理します。

Azure ID クライアント ライブラリの詳細については、「[.NET 用 Azure ID クライアント ライブラリ](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity)」を参照してください。

## <a name="assign-rbac-roles-for-access-to-data"></a>RBAC ロールを割り当ててデータにアクセスする

Azure AD セキュリティ プリンシパルが Blob またはキュー データにアクセスしようとする場合、そのセキュリティ プリンシパルはリソースへのアクセス許可を保持している必要があります。 セキュリティ プリンシパルが Azure 内のマネージド ID であるか、開発環境でコードを実行している Azure AD ユーザー アカウントであるかにかかわらず、Azure Storage での Blob またはキュー データへのアクセスを許可する RBAC ロールをセキュリティ プリンシパルに割り当てる必要があります。 RBAC 経由でのアクセス許可の割り当てについては、「[Azure Active Directory を使用して Azure Blob およびキューへのアクセスを承認します](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights)」にある「**アクセス権に RBAC ロールを割り当てる**」というタイトルのセクションを参照してください。

## <a name="install-the-preview-packages"></a>プレビュー パッケージをインストールする

この記事の例では、BLOB ストレージ用の Azure Storage クライアント ライブラリの最新のプレビュー バージョンが使用されます。 プレビュー パッケージをインストールするには、NuGet パッケージ マネージャー コンソールから次のコマンドを実行します。

```powershell
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

この記事の例では、Azure AD の資格情報で認証するために、[.NET 用 Azure ID クライアント ライブラリ](https://www.nuget.org/packages/Azure.Identity/)の最新のプレビュー バージョンも使用されます。 プレビュー パッケージをインストールするには、NuGet パッケージ マネージャー コンソールから次のコマンドを実行します。

```powershell
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="net-code-example-create-a-block-blob"></a>.NET コード例: ブロック BLOB を作成する

Azure ID と Azure Storage クライアント ライブラリのプレビューバージョンを使用するために、次の `using` ディレクティブをコードに追加します。

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

Azure Storage への要求を承認するためにコード上で使用できるトークン資格情報を取得するには、[DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) クラスのインスタンスを作成します。 次のコード例では、認証済みのトークン資格情報を取得し、それを使用してサービス クライアント オブジェクトを作成した後、サービス クライアントを使用して新しい Blob をアップロードする方法を示します。

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (StorageRequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Azure AD を使用して BLOB またはキュー データに対する要求を承認するには、それらの要求に HTTPS を使用する必要があります。

## <a name="next-steps"></a>次の手順

- Azure Storage の RBAC ロールについては、[RBAC を使用したストレージ データへのアクセス権の管理](storage-auth-aad-rbac.md)に関するページをご覧ください。
- ストレージ アプリケーション内からコンテナーやキューへのアクセスを承認する方法については、[ストレージ アプリケーションで Azure AD を使用する](storage-auth-aad-app.md)方法に関するページを参照してください。
- Azure AD 資格情報を使用して Azure CLI と PowerShell のコマンドを実行する方法については、「[Azure AD ID を使用し、CLI または PowerShell で BLOB とキューのデータにアクセスする](storage-auth-aad-script.md)」を参照してください。

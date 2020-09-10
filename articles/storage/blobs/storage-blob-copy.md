---
title: .NET を使用して BLOB をコピーする - Azure Storage
description: .NET クライアント ライブラリを使用して、Azure Storage アカウント内の BLOB をコピーする方法について説明します。
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/01/2020
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.custom: devx-track-csharp
ms.openlocfilehash: a7ca195bdfb05baff6100b3481903f9ca0841dc6
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320666"
---
# <a name="copy-a-blob-with-net"></a>.NET を使用して BLOB をコピーする

この記事では、Azure Storage アカウントを使用して BLOB をコピーする方法について説明します。 また、非同期のコピー操作を中止する方法も示します。 コード例では [.NET 用の Azure Storage クライアント ライブラリ](/dotnet/api/overview/azure/storage)を使用します。

## <a name="about-copying-blobs"></a>BLOB のコピーについて

同じストレージ アカウント内で BLOB をコピーすると、同期操作になります。 アカウントをまたいでコピーする場合は、非同期操作です。

コピー操作のコピー元 BLOB にできるのは、ブロック BLOB、アペンド BLOB、ページ BLOB、またはスナップショットです。 コピー先 BLOB が既に存在する場合、コピー元 BLOB と同じ BLOB の種類である必要があります。 既存のコピー先 BLOB は上書きされます。

コピー操作の進行中は、コピー先 BLOB を変更できません。 コピー先 BLOB には、未完了の BLOB コピー操作は 1 つしか存在できません。 言い換えると、1 つの BLOB を、複数の保留中のコピー操作のコピー先にすることはできません。

常に、コピー元の BLOB またはファイル全体がコピーされます。 バイト範囲またはブロックのセットのコピーはサポートされていません。

BLOB がコピーされると、そのシステム プロパティが同じ値でコピー先 BLOB にコピーされます。

コピー操作は、次のいずれかの形態で実行できます。

- コピー元 BLOB は別の名前でコピー先 BLOB にコピーできます。 コピー先 BLOB は、同じ BLOB の種類 (ブロック、アペンド、またはページ) の既存の BLOB でも、コピー操作によって作成される新しい BLOB でもかまいません。
- コピー元 BLOB を同じ名前でコピー先 BLOB にコピーして、コピー先 BLOB を実質的に置き換えることができます。 このようなコピー操作では、コミットされていないブロックはすべて削除され、コピー先 BLOB のメタデータが上書きされます。
- Azure File Service 内のコピー元ファイルをコピー先 BLOB にコピーできます。 コピー先 BLOB は、既存のブロック BLOB でも、コピー操作によって作成される新しいブロック BLOB でもかまいません。 ファイルからページ BLOB またはアペンド BLOB へのコピーはサポートされていません。
- スナップショットをベース BLOB にコピーします。 スナップショットをベース BLOB に昇格することにより、BLOB を以前のバージョンに復元できます。
- スナップショットを別の名前でコピー先 BLOB にコピーします。 結果として得られるコピー先 BLOB は書き込み可能な BLOB であり、スナップショットではありません。

## <a name="copy-a-blob"></a>BLOB をコピーする

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

BLOB をコピーするには、次のいずれかのメソッドを呼び出します。

- [StartCopyFromUri](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.startcopyfromuri)
- [StartCopyFromUriAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.startcopyfromuriasync)

**StartCopyFromUri** メソッドと **StartCopyFromUriAsync** メソッドは、コピー操作に関する情報が含まれる [CopyFromUriOperation](/dotnet/api/azure.storage.blobs.models.copyfromurioperation) オブジェクトを返します。

次のコード例では、以前に作成した BLOB を表す [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) を取得し、同じコンテナー内の新しい BLOB にコピーします。

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CopyBlob.cs" id="Snippet_CopyBlob":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

BLOB をコピーするには、次のいずれかのメソッドを呼び出します。

- [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy)
- [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync)

**StartCopy** および **StartCopyAsync** メソッドは、状態を確認またはコピー操作を中止するために使用されるコピー ID の値を返します。

次のコード例では、以前に作成した BLOB への参照を取得し、同じコンテナー内の新しい BLOB にコピーします。

```csharp
private static async Task CopyBlockBlobAsync(CloudBlobContainer container)
{
    CloudBlockBlob sourceBlob = null;
    CloudBlockBlob destBlob = null;
    string leaseId = null;

    try
    {
        // Get a block blob from the container to use as the source.
        sourceBlob = container.ListBlobs().OfType<CloudBlockBlob>().FirstOrDefault();

        // Lease the source blob for the copy operation 
        // to prevent another client from modifying it.
        // Specifying null for the lease interval creates an infinite lease.
        leaseId = await sourceBlob.AcquireLeaseAsync(null);

        // Get a reference to a destination blob (in this case, a new blob).
        destBlob = container.GetBlockBlobReference("copy of " + sourceBlob.Name);

        // Ensure that the source blob exists.
        if (await sourceBlob.ExistsAsync())
        {
            // Get the ID of the copy operation.
            string copyId = await destBlob.StartCopyAsync(sourceBlob);

            // Fetch the destination blob's properties before checking the copy state.
            await destBlob.FetchAttributesAsync();

            Console.WriteLine("Status of copy operation: {0}", destBlob.CopyState.Status);
            Console.WriteLine("Completion time: {0}", destBlob.CopyState.CompletionTime);
            Console.WriteLine("Bytes copied: {0}", destBlob.CopyState.BytesCopied.ToString());
            Console.WriteLine("Total bytes: {0}", destBlob.CopyState.TotalBytes.ToString());
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        // Break the lease on the source blob.
        if (sourceBlob != null)
        {
            await sourceBlob.FetchAttributesAsync();

            if (sourceBlob.Properties.LeaseState != LeaseState.Available)
            {
                await sourceBlob.BreakLeaseAsync(new TimeSpan(0));
            }
        }
    }
}
```

---

## <a name="abort-a-blob-copy-operation"></a>BLOB コピー操作の中止

コピー操作を中止した結果は、ブロック BLOB、アペンド BLOB、ページ BLOB の場合、長さゼロのコピー先 BLOB です。 ただし、コピー先 BLOB のメタデータは、コピー元 BLOB からコピーされた値、またはコピー操作中に明示的に設定された値に変わります。 コピー前のメタデータを元のまま維持するには、いずれかのコピー方法を呼び出す前に、コピー先 BLOB のスナップショットを作成します。

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

コピー先 BLOB の [BlobProperties.CopyStatus](/dotnet/api/azure.storage.blobs.models.blobproperties.copystatus) プロパティをチェックして、コピー操作の状態を取得できます。 コピーが完了すると、最終 BLOB がコミットされます。

進行中の BLOB コピー操作を中止すると、コピー先 BLOB のコピーの状態は [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus) に設定されます。

[AbortCopyFromUri](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.abortcopyfromuri) メソッドと [AbortCopyFromUriAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.abortcopyfromuriasync) メソッドによって、進行中の BLOB コピー操作がキャンセルされます。

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CopyBlob.cs" id="Snippet_StopBlobCopy":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

コピー先 BLOB の [CopyState.Status](/dotnet/api/microsoft.azure.storage.blob.copystate.status) プロパティをチェックして、コピー操作の状態を取得できます。 コピーが完了すると、最終 BLOB がコミットされます。

進行中の BLOB コピー操作を中止すると、コピー先 BLOB のコピーの状態は [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus) に設定されます。

[AbortCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopy) メソッドと [AbortCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopyasync) メソッドによって、進行中の BLOB コピー操作がキャンセルされます。

```csharp
// Fetch the destination blob's properties before checking the copy state.
await destBlob.FetchAttributesAsync();

// Check the copy status. If it is still pending, abort the copy operation.
if (destBlob.CopyState.Status == CopyStatus.Pending)
{
    await destBlob.AbortCopyAsync(copyId);
    Console.WriteLine("Copy operation {0} has been aborted.", copyId);
}
```

---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>次のステップ

次のトピックでは、Azure REST API を使用した BLOB のコピーと、進行中のコピー操作の中止に関する情報を示しています。

- [Copy Blob](/rest/api/storageservices/copy-blob)
- [BLOB のコピーを中止する](/rest/api/storageservices/abort-copy-blob)

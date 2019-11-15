---
title: クイック スタート:Azure Blob Storage ライブラリ v12 - Java
description: このクイックスタートでは、Java 用 Azure Blob Storage クライアント ライブラリ バージョン 12 を使用して、BLOB (オブジェクト) ストレージ内にコンテナーと BLOB を作成する方法について説明します。 次に、ローカル コンピューターに BLOB をダウンロードする方法と、コンテナー内のすべての BLOB を一覧表示する方法について説明します。
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/05/2019
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: a7cd61854176dc702f213211b14c2361b3e433ad
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825359"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-java"></a>クイック スタート:Java 用 Azure Blob Storage クライアント ライブラリ v12

Java 用 Azure Blob Storage クライアント ライブラリ v12 を使用してみましょう。 Azure Blob Storage は、Microsoft のクラウド用オブジェクト ストレージ ソリューションです。 手順に従ってパッケージをインストールし、基本タスクのコード例を試してみましょう。 Blob Storage は、テキスト データやバイナリ データなどの大量の非構造化データを格納するために最適化されています。

> [!NOTE]
> 以前の SDK バージョンを使ってみるには、「[クイックスタート: Java 用 Azure Blob Storage クライアント ライブラリ](storage-quickstart-blobs-java-legacy.md)」を参照してください。

Java 用 Azure Blob Storage クライアント ライブラリ v12 を使用すると、以下のことができます。

* コンテナーを作成する
* Azure Storage への BLOB のアップロード
* コンテナー内のすべての BLOB を一覧表示する
* ローカル コンピューターに BLOB をダウンロードする
* コンテナーを削除する

[API リファレンスのドキュメント](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-blob/12.0.0/index.html) | [ライブラリのソース コード](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob) | [パッケージ (Maven)](https://mvnrepository.com/artifact/com.azure/azure-storage-blob?repo=jcenter) | [サンプル](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob/src/samples/java/com/azure/storage/blob)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>前提条件

* [Java Development Kit (JDK)](/java/azure/jdk/?view=azure-java-stable) バージョン 8 以降
* [Apache Maven](https://maven.apache.org/download.cgi)
* Azure サブスクリプション - [無料アカウントを作成する](https://azure.microsoft.com/free/)
* Azure Storage アカウント - [ストレージ アカウントの作成](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

## <a name="setting-up"></a>設定

このセクションでは、Java 用 Azure Blob Storage クライアント ライブラリ v12 を操作するためのプロジェクトの準備について説明します。

### <a name="create-the-project"></a>プロジェクトを作成する

*blob-quickstart-v12* という名前の Java Core アプリケーションを作成します。

1. コンソール ウィンドウ (cmd、PowerShell、Bash など) で、Maven コマンドを使用し、*blob-quickstart-v12* という名前で新しいコンソール アプリを作成します。 次の **mvn** コマンドをすべて 1 行に入力して、単純な "Hello world!" を作成します。 Java プロジェクト。 ここでは、読みやすくするために、コマンドが複数の行に表示されています。

   ```console
   mvn archetype:generate -DgroupId=com.blobs.quickstart
                          -DartifactId=blob-quickstart-v12
                          -DarchetypeArtifactId=maven-archetype-quickstart
                          -DarchetypeVersion=1.4
                          -DinteractiveMode=false
   ```

1. プロジェクトの生成からの出力は、次のようになります。

    ```console
    [INFO] Scanning for projects...
    [INFO]
    [INFO] ------------------< org.apache.maven:standalone-pom >-------------------
    [INFO] Building Maven Stub Project (No POM) 1
    [INFO] --------------------------------[ pom ]---------------------------------
    [INFO]
    [INFO] >>> maven-archetype-plugin:3.1.2:generate (default-cli) > generate-sources @ standalone-pom >>>
    [INFO]
    [INFO] <<< maven-archetype-plugin:3.1.2:generate (default-cli) < generate-sources @ standalone-pom <<<
    [INFO]
    [INFO]
    [INFO] --- maven-archetype-plugin:3.1.2:generate (default-cli) @ standalone-pom ---
    [INFO] Generating project in Batch mode
    [INFO] ----------------------------------------------------------------------------
    [INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
    [INFO] ----------------------------------------------------------------------------
    [INFO] Parameter: groupId, Value: com.blobs.quickstart
    [INFO] Parameter: artifactId, Value: blob-quickstart-v12
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.blobs.quickstart
    [INFO] Parameter: packageInPathFormat, Value: com/blobs/quickstart
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.blobs.quickstart
    [INFO] Parameter: groupId, Value: com.blobs.quickstart
    [INFO] Parameter: artifactId, Value: blob-quickstart-v12
    [INFO] Project created from Archetype in dir: C:\QuickStarts\blob-quickstart-v12
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  7.056 s
    [INFO] Finished at: 2019-10-23T11:09:21-07:00
    [INFO] ------------------------------------------------------------------------
        ```

1. Switch to the newly created *blob-quickstart-v12* folder.

   ```console
   cd blob-quickstart-v12
   ```

1. *blob-quickstart-v12* ディレクトリ内に、*data* という別のディレクトリを作成します。 BLOB データ ファイルは、ここに作成され格納されます。

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>パッケージをインストールする

テキスト エディターで *pom.xml* ファイルを開きます。 依存関係のグループに、次の dependency 要素を追加します。

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>12.0.0</version>
</dependency>
```

### <a name="set-up-the-app-framework"></a>アプリのフレームワークを設定する

プロジェクト ディレクトリで次の操作を行います。

1. */src/main/java/com/blobs/quickstart* ディレクトリに移動します
1. 使用しているエディターで *App.java* ファイルを開きます
1. `System.out.println("Hello world!");` ステートメントを削除します
1. `import` ディレクティブを追加します

コードは次のとおりです。

```java
package com.blobs.quickstart;

/**
 * Azure blob storage v12 SDK quickstart
 */
import com.azure.storage.blob.*;
import com.azure.storage.blob.models.*;
import java.io.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
    }
}
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>Azure Portal で資格情報をコピーする

サンプル アプリケーションから Azure Storage に対して要求を実行するときは、承認されている必要があります。 要求を承認するには、ストレージ アカウントの資格情報を接続文字列としてアプリケーションに追加します。 次の手順に従って、ストレージ アカウントの資格情報を表示します。

1. [Azure Portal](https://portal.azure.com) にサインインします。
2. 自分のストレージ アカウントを探します。
3. ストレージ アカウントの概要の **[設定]** セクションで、 **[アクセス キー]** を選択します。 ここでは、アカウント アクセス キーと各キーの完全な接続文字列を表示できます。
4. **[Key1]** の **[接続文字列]** の値を見つけ、 **[コピー]** ボタンを選択して接続文字列をコピーします。 すぐ後の手順で、接続文字列の値を環境変数に追加します。

    ![Azure portal から接続文字列をコピーする方法を示すスクリーンショット](../../../includes/media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>ストレージ接続文字列の構成

接続文字列をコピーした後、アプリケーションを実行しているローカル マシンの新しい環境変数にそれを書き込みます。 環境変数を設定するには、コンソール ウィンドウを開いて、お使いのオペレーティング システムの手順に従います。 `<yourconnectionstring>` は、実際の接続文字列に置き換えてください。

#### <a name="windows"></a>Windows

```cmd
setx CONNECT_STR "<yourconnectionstring>"
```

Windows で環境変数を追加した後、コマンド ウィンドウの新しいインスタンスを開始する必要があります。

#### <a name="linux"></a>Linux

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="macos"></a>macOS

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="restart-programs"></a>プログラムの再起動

環境変数を追加した後、環境変数の読み取りを必要とする実行中のプログラムをすべて再起動します。 たとえば、続行する前に、ご使用の開発環境またはエディターを再起動します。

## <a name="object-model"></a>オブジェクト モデル

Azure Blob Storage は、大量の非構造化データを格納するために最適化されています。 非構造化データとは、特定のデータ モデルや定義に従っていないデータであり、テキスト データやバイナリ データなどがあります。 Blob Storage には、3 種類のリソースがあります。

* ストレージ アカウント
* ストレージ アカウント内のコンテナー
* コンテナー内の BLOB

次の図に、これらのリソースの関係を示します。

![Blob Storage のアーキテクチャ図](./media/storage-blob-introduction/blob1.png)

これらのリソースとやり取りするには、以下の Java クラスを使用します。

* [BlobServiceClient](/java/api/com.azure.storage.blob.blobserviceclient):`BlobServiceClient` クラスを使用して、Azure Storage リソースと BLOB コンテナーを操作できます。 ストレージ アカウントでは、Blob service に対して最上位の名前空間が提供されます。
* [BlobServiceClientBuilder](/java/api/com.azure.storage.blob.blobserviceclientbuilder):`BlobServiceClientBuilder` クラスでは、`BlobServiceClient` オブジェクトの構成とインスタンス化を支援する fluent ビルダー API が提供されています。
* [BlobContainerClient](/java/api/com.azure.storage.blob.blobcontainerclient):`BlobContainerClient` クラスを使用して、Azure Storage コンテナーとその BLOB を操作できます。
* [BlobClient](/java/api/com.azure.storage.blob.blobclient):`BlobClient` クラスを使用して、Azure Storage BLOB を操作できます。
* [BlobItem](/java/api/com.azure.storage.blob.blobitem):`BlobItem` クラスでは、`listBlobsFlat` の呼び出しから返された個々の BLOB が示されます。

## <a name="code-examples"></a>コード例

これらのコード例のスニペットは、Java 用 Azure Blob Storage クライアント ライブラリを使用して以下を実行する方法を示します。

* [接続文字列を取得する](#get-the-connection-string)
* [コンテナーの作成](#create-a-container)
* [コンテナーに BLOB をアップロードする](#upload-blobs-to-a-container)
* [コンテナー内の BLOB を一覧表示する](#list-the-blobs-in-a-container)
* [BLOB をダウンロードする](#download-blobs)
* [コンテナーの削除](#delete-a-container)

### <a name="get-the-connection-string"></a>接続文字列を取得する

以下のコードでは、「[ストレージ接続文字列の構成](#configure-your-storage-connection-string)」セクションで作成した環境変数から、ストレージ アカウントに対する接続文字列を取得します。

このコードを `Main` メソッド内に追加します。

```java
System.out.println("Azure Blob storage v12 - Java quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called CONNECT_STR. If the environment variable
// is created after the application is launched in a console or with
// Visual Studio, the shell or application needs to be closed and reloaded
// to take the environment variable into account.
String connectStr = System.getenv("CONNECT_STR");
```

### <a name="create-a-container"></a>コンテナーを作成する

新しいコンテナーの名前を決定します。 次のコードでは、確実に一意になるように、コンテナー名に UUID 値を追加します。

> [!IMPORTANT]
> コンテナーの名前は小文字にする必要があります。 コンテナーと BLOB の名前付けの詳細については、「[Naming and Referencing Containers, Blobs, and Metadata (コンテナー、BLOB、メタデータの名前付けと参照)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)」を参照してください。

次に、[BlobContainerClient](/java/api/com.azure.storage.blob.blobcontainerclient) クラスのインスタンスを作成し、[create](/java/api/com.azure.storage.blob.blobcontainerclient.create) メソッドを呼び出して、ストレージ アカウントに実際にコンテナーを作成します。

`Main` メソッドの末尾に、次のコードを追加します。

```java
// Create a BlobServiceClient object which will be used to create a container client
BlobServiceClient blobServiceClient = new BlobServiceClientBuilder().connectionString(connectStr).buildClient();

//Create a unique name for the container
String containerName = "quickstartblobs" + java.util.UUID.randomUUID();

// Create the container and return a container client object
BlobContainerClient containerClient = blobServiceClient.createBlobContainer(containerName);
```

### <a name="upload-blobs-to-a-container"></a>コンテナーに BLOB をアップロードする

次のコード スニペット:

1. ローカルの *data* ディレクトリにテキスト ファイルを作成します。
1. 「[コンテナーを作成する](#create-a-container)」セクションでのコンテナー上で [getBlobClient](/java/api/com.azure.storage.blob.blobcontainerclient.getblobclient) メソッドを呼び出すことで、[BlobClient](/java/api/com.azure.storage.blob.blobclient) オブジェクトへの参照を取得します。
1. [uploadFromFile](/java/api/com.azure.storage.blob.blobclient.uploadfromfile) メソッドを呼び出して、ローカル テキスト ファイルを BLOB にアップロードします。 このメソッドは、BLOB がまだ存在しない場合は作成し、既に存在する場合は上書きします。

`Main` メソッドの末尾に、次のコードを追加します。

```java
// Create a local file in the ./data/ directory for uploading and downloading
String localPath = "./data/";
String fileName = "quickstart" + java.util.UUID.randomUUID() + ".txt";
File localFile = new File(localPath + fileName);

// Write text to the file
FileWriter writer = new FileWriter(localPath + fileName, true);
writer.write("Hello, World!");
writer.close();

// Get a reference to a blob
BlobClient blobClient = containerClient.getBlobClient(fileName);

System.out.println("\nUploading to Blob storage as blob:\n\t" + blobClient.getBlobUrl());

// Upload the blob
blobClient.uploadFromFile(localPath + fileName);
```

### <a name="list-the-blobs-in-a-container"></a>コンテナー内の BLOB を一覧表示する

[listBlobsFlat](/java/api/com.azure.storage.blob.blobcontainerclient.listblobsflat) メソッドを呼び出して、コンテナー内の BLOB を一覧表示します。 この場合、コンテナーに BLOB が 1 つだけ追加されているので、一覧表示操作ではその 1 つの BLOB だけが返されます。

`Main` メソッドの末尾に、次のコードを追加します。

```java
System.out.println("\nListing blobs...");

// List the blob(s) in the container.
for (BlobItem blobItem : containerClient.listBlobs()) {
    System.out.println("\t" + blobItem.getName());
}
```

### <a name="download-blobs"></a>BLOB をダウンロードする

[downloadToFile](/java/api/com.azure.storage.blob.blobclient.downloadtofile) メソッドを呼び出して、以前に作成した BLOB をダウンロードします。 サンプル コードでは、ローカル ファイル システム内で両方のファイルを見ることができるように、ファイル名に "DOWNLOAD" というサフィックスを追加します。

`Main` メソッドの末尾に、次のコードを追加します。

```java
// Download the blob to a local file
// Append the string "DOWNLOAD" before the .txt extension so that you can see both files.
String downloadFileName = fileName.replace(".txt", "DOWNLOAD.txt");
File downloadedFile = new File(localPath + downloadFileName);

System.out.println("\nDownloading blob to\n\t " + localPath + downloadFileName);

blobClient.downloadToFile(localPath + downloadFileName);
```

### <a name="delete-a-container"></a>コンテナーを削除する

次のコードでは、[delete](/java/api/com.azure.storage.blob.blobcontainerclient.delete) メソッドを使用して、コンテナー全体を削除することで、アプリによって作成されたリソースをクリーンアップします。 また、アプリによって作成されたローカル ファイルも削除します。

アプリでは、BLOB、コンテナー、およびローカル ファイルを削除する前に、`System.console().readLine()` を呼び出すことで、ユーザーの入力を一時停止します。 このとき、リソースが削除される前に、実際に正しく作成されたことを確認できます。

`Main` メソッドの末尾に、次のコードを追加します。

```java
// Clean up
System.out.println("\nPress the Enter key to begin clean up");
System.console().readLine();

System.out.println("Deleting blob container...");
containerClient.delete();

System.out.println("Deleting the local source and downloaded files...");
localFile.delete();
downloadedFile.delete();

System.out.println("Done");
```

## <a name="run-the-code"></a>コードの実行

このアプリでは、ローカル フォルダーにテスト ファイルが作成され、BLOB ストレージにアップロードされます。 次に、コンテナー内の BLOB を一覧表示し、ファイルを新しい名前でダウンロードして、古いファイルと新しいファイルを比較できるようにします。

*pom.xml* ファイルが格納されているディレクトリに移動し、次の `mvn` コマンドを使用してプロジェクトをコンパイルします。

```console
mvn compile
```

次に、パッケージをビルドします。

```console
mvn package
```

次の `mvn` コマンドを実行して、アプリを実行します。

```console
mvn exec:java -Dexec.mainClass="com.blobs.quickstart.App" -Dexec.cleanupDaemonThreads=false
```

アプリの出力は、次の例のようになります。

```output
Azure Blob storage v12 - Java quickstart sample

Uploading to Blob storage as blob:
        https://mystorageacct.blob.core.windows.net/quickstartblobsf9aa68a5-260e-47e6-bea2-2dcfcfa1fd9a/quickstarta9c3a53e-ae9d-4863-8b34-f3d807992d65.txt

Listing blobs...
        quickstarta9c3a53e-ae9d-4863-8b34-f3d807992d65.txt

Downloading blob to
        ./data/quickstarta9c3a53e-ae9d-4863-8b34-f3d807992d65DOWNLOAD.txt

Press the Enter key to begin clean up

Deleting blob container...
Deleting the local source and downloaded files...
Done
```

クリーンアップ プロセスを開始する前に、 *[MyDocuments]* フォルダー内の 2 つのファイルをチェックします。 それらを開いて、同じであるかどうかを確認します。

ファイルを確認した後、**Enter** キーを押してテスト ファイルを削除し、デモを終了します。

## <a name="next-steps"></a>次の手順

このクイックスタートでは、Java を使用して BLOB をアップロード、ダウンロード、および一覧表示する方法について説明しました。

BLOB ストレージのサンプル アプリを確認するには、以下に進んでください。

> [!div class="nextstepaction"]
> [Azure Blob Storage SDK v12 の Java サンプル](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob/src/samples/java/com/azure/storage/blob)

* 詳細については、「[Azure SDK for Java (Java 用の Azure SDK)](https://github.com/Azure/azure-sdk-for-java/blob/master/README.md)」を参照してください。
* チュートリアル、サンプル、クイックスタートなどのドキュメントについては、「[Java クラウド開発者向けの Azure](/azure/java/)」を参照してください。

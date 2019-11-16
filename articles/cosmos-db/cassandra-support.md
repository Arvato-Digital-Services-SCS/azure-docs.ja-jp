---
title: Azure Cosmos DB の Cassandra API でサポートされる Apache Cassandra の機能とコマンド
description: Azure Cosmos DB の Cassandra API でサポートされる Apache Cassandra の機能について説明します
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: c0a47e922ae8cdca3c70cb53f9fa2f7dafe191c6
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2019
ms.locfileid: "73889221"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB の Cassandra API でサポートされる Apache Cassandra の機能 

Azure Cosmos DB は、Microsoft のグローバルに分散されたマルチモデル データベース サービスです。 Azure Cosmos DB の Cassandra API とは、Cassandra Query Language (CQL) v4 [ワイヤ プロトコル](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec)準拠のオープン ソースの Cassandra クライアント [ドライバー](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver)経由で通信できます。 

Azure Cosmos DB の Cassandra API を使用することで、Apache Cassandra API のメリットと、Azure Cosmos DB が提供するエンタープライズ機能を享受できます。 エンタープライズ機能には、[グローバル配信](distribute-data-globally.md)、[自動スケールアウトのパーティション分割](partition-data.md)、可用性と待機時間の保証、保存時の暗号化などがあります。

## <a name="cassandra-protocol"></a>Cassandra プロトコル 

Azure Cosmos DB の Cassandra API は、CQL バージョン **v4** と互換性があります。 サポートされている CQL コマンド、ツール、制限事項、および例外を、以下に示します。 これらのプロトコルを認識するクライアント ドライバーはすべて、Azure Cosmos DB の Cassandra API に接続できるはずです。

## <a name="cassandra-driver"></a>Cassandra ドライバー

Azure Cosmos DB の Cassandra API では、次のバージョンの Cassandra ドライバーがサポートされています。

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>CQL データ型 

Azure Cosmos DB の Cassandra API では、次の CQL データ型がサポートされています。

* ascii  
* bigint  
* BLOB  
* ブール値  
* counter  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* text  
* time  
* timestamp  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udt  
* map  

## <a name="cql-functions"></a>CQL 関数

Azure Cosmos DB の Cassandra API では、次の CQL 関数がサポートされています。

* トークン  
* 集計関数
  * min、max、avg、count
* Blob 変換関数 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID および timeuuid 関数 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date) 
  


## <a name="cassandra-api-limits"></a>Cassandra API の制限

Azure Cosmos DB の Cassandra API には、テーブルに格納されるデータのサイズに制限がありません。 パーティション キーの制限を確実に適用しながら、数百テラバイトまたはペタバイトのデータを格納することができます。 同様に、すべてのエンティティまたは同等の行に列数の制限はありません。 ただし、エンティティの合計サイズが 2 MB を超えることはできません。 他のすべての API の場合と同様に、パーティション キーあたりのデータが 10 GB を超えることはできません。

## <a name="tools"></a>ツール 

Azure Cosmos DB の Cassandra API は、管理されたサービス プラットフォームです。 クラスターを管理するために、管理オーバーヘッドや、ガベージ コレクター、Java 仮想マシン (JVM)、nodetool などのユーティリティは必要ありません。 バイナリ CQLv4 互換性を利用する cqlsh などのツールをサポートしています。 

* Azure portal のデータ エクスプローラー、メトリック、ログの診断、PowerShell、および CLI は、アカウントを管理するためにサポートされているその他のメカニズムです。

## <a name="cql-shell"></a>CQL シェル  

CQLSH コマンドライン ユーティリティは、Apache Cassandra 3.1.1 に付属しており、いくつかの環境変数を設定することですぐに機能します。

**Windows:**

Windows を使用している場合は、[Linux 用の Windows ファイルシステム](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux)を有効にすることをお勧めします。 その後、以下の linux コマンドを実行できます。

**Unix/Linux/Mac:**

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

## <a name="cql-commands"></a>CQL コマンド

Azure Cosmos DB は、Cassandra API アカウントで以下のデータベース コマンドをサポートしています。

* CREATE KEYSPACE (このコマンドのレプリケーション設定は無視されます)
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH - unlogged コマンドのみサポートされています 
* DELETE

CQL v4 互換の SDK から実行された場合のすべての CRUD 操作で、エラーと消費された要求の単位数に関する追加情報が返されます。 DELETE および UPDATE コマンドは、プロビジョニングされたスループットを最も効率的に使用できるように、リソース ガバナンスを考慮して処理する必要があります。

* gc_grace_seconds を指定する場合は値を 0 にする必要があることに注意してください。

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## <a name="consistency-mapping"></a>一貫性のマッピング 

Azure Cosmos DB の Cassandra API では、読み取り操作の一貫性を選択することができます。  一貫性のマッピングについては、[こちら](consistency-levels-across-apis.md#cassandra-mapping)に詳しく説明されています。

## <a name="permission-and-role-management"></a>アクセス許可とロールの管理

Azure Cosmos DB は、プロビジョニングのためのロールベースのアクセス制御 (RBAC)、キーのローテーション、メトリックの表示、[Azure portal](https://portal.azure.com) から取得できる読み書きおよび読み取り専用のパスワードおよびキーをサポートしています。 Azure Cosmos DB では、CRUD アクティビティのためのロールはサポートされていません。

## <a name="keyspace-and-table-options"></a>キースペースとテーブルのオプション

現在、"Create Keyspace" コマンドでのリージョン名、クラス、replication_factor、およびデータセンターのオプションは無視されます。 システムは、基になる Azure Cosmos DB の[グローバル分散](global-dist-under-the-hood.md)レプリケーション方法を使用して、リージョンを追加します。 複数リージョンにまたがってデータが存在する必要がある場合は、PowerShell、CLI、またはポータルを使用して、アカウント レベルでこれを有効にすることができます。詳細については、[リージョンを追加する方法](how-to-manage-database-account.md#addremove-regions-from-your-database-account)に関する記事を参照してください。 Azure Cosmos DB ではすべての書き込みが持続的であることが保証されるため、Durable_writes を無効にすることはできません。 すべてのリージョンで、Azure Cosmos DB は最大 4 つのレプリカで構成されるレプリカ セット全体にデータをレプリケートします。このレプリカ セットの[構成](global-dist-under-the-hood.md)は変更できません。
 
ゼロに設定する必要がある gc_grace_seconds を除き、テーブルの作成時にはすべてのオプションが無視されます。
キースペースとテーブルには、最小値が 400 RU/秒の、"cosmosdb_provisioned_throughput" という名前の追加オプションがあります。 キースペースのスループットによって、複数のテーブル間でスループットを共有することが可能になります。これは、プロビジョニングされたスループットがすべてのテーブルで利用されていないシナリオに役立ちます。 Alter Table コマンドを使用すると、複数のリージョン間でプロビジョニングされたスループットを変更できます。 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```


## <a name="usage-of-cassandra-retry-connection-policy"></a>Cassandra 再試行接続ポリシーの使用

Azure Cosmos DB は、リソースによって管理されるシステムです。 これは、操作によって使用される要求単位に基づいて、1 秒間に特定の数の操作を実行できることを意味します。 アプリケーションが特定の秒においてその制限を超えると、要求はレート制限され、例外がスローされます。 Azure Cosmos DB の Cassandra API は、このような例外を Cassandra ネイティブ プロトコルの過負荷エラーに変換します。 レート制限時にアプリケーションが要求をインターセプトして再試行できるように、[spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) および [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) 拡張機能が提供されています。 他の SDK を使用して Azure Cosmos DB の Cassandra API にアクセスする場合は、これらの例外に対して再試行する接続ポリシーを作成します。

## <a name="next-steps"></a>次の手順

- Java アプリケーションを使用した[ Cassandra API アカウント、データベースおよびテーブルの作成](create-cassandra-api-account-java.md)の開始


---
title: Azure Data Factory でサポートされるファイル形式
description: このトピックでは、Azure Data Factory 内のファイルベースのコネクタでサポートされている圧縮コードやファイル形式について説明します。
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 24edeb686136a1264d75e81fd9527feff3d17c48
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2020
ms.locfileid: "84191087"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Azure Data Factory でサポートされるファイル形式と圧縮コーデック
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

*この記事は、次のコネクターに適用されます。[Amazon S3](connector-amazon-simple-storage-service.md)、[Azure Blob](connector-azure-blob-storage.md)、[Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)、[Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)、[Azure File Storage](connector-azure-file-storage.md)、[ファイル システム](connector-file-system.md)、[FTP](connector-ftp.md)、[Google Cloud Storage](connector-google-cloud-storage.md)、[HDFS](connector-hdfs.md)、[HTTP](connector-http.md)、および [SFTP](connector-sftp.md)。*

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

[コピー アクティビティ](copy-activity-overview.md)を使用すると、ファイル ベースの 2 つのデータ ストア間でファイルをそのままコピーできます。その場合、データはシリアル化または逆シリアル化なしで効率的にコピーされます。 

さらに、特定の形式のファイルを解析または生成することもできます。 たとえば、次の操作を実行できます。

* SQL Server データベースからデータをコピーし、Parquet 形式で Azure Data Lake Storage Gen2 に書き込む。
* オンプレミスのファイル システムからテキスト (CSV) 形式でファイルをコピーし、Azure BLOB ストレージに Avro 形式で書き込む。
* オンプレミスのファイル システムから zip 形式のファイルをコピーし、その場で圧縮解除して、抽出されたファイルを Azure Data Lake Storage Gen2 に書き込む。
* Azure BLOB ストレージから Gzip 圧縮テキスト (CSV) 形式でデータをコピーし、Azure SQL Database に書き込む。
* シリアル化/逆シリアル化または圧縮/展開を必要とする他の多くのアクティビティ。

## <a name="next-steps"></a>次のステップ

コピー アクティビティの他の記事を参照してください。

- [コピー アクティビティの概要](copy-activity-overview.md)
- [コピー アクティビティのパフォーマンス](copy-activity-performance.md)

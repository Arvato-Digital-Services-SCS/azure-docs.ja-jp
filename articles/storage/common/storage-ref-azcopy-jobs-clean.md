---
title: azcopy jobs clean | Microsoft Docs
description: この記事では、azcopy jobs clean コマンドに関する参照情報を提供します。
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: a06e428908777c526602166f127a28304b595ba0
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220083"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

すべてのジョブのログ ファイルとプラン ファイルをすべて削除します

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>関連する概念に関する記事

- [AzCopy を使ってみる](storage-use-azcopy-v10.md)
- [AzCopy と Blob Storage でデータを転送する](storage-use-azcopy-blobs.md)
- [AzCopy とファイル ストレージでデータを転送する](storage-use-azcopy-files.md)
- [AzCopy の構成、最適化、トラブルシューティング](storage-use-azcopy-configure.md)

## <a name="examples"></a>例

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Options

**-h, --help**                clean のヘルプを表示します。

**--with-status** string   この状態のジョブのみが削除されます。使用できる値は次のとおりです。Canceled、Completed、Failed、InProgress、All (既定値は "All")

## <a name="options-inherited-from-parent-commands"></a>親コマンドから継承されるオプション

**--cap-mbps uint32**      転送速度の上限を設定します (メガビット/秒)。 瞬間的なスループットは、上限と若干異なる場合があります。 このオプションを 0 に設定した場合や省略した場合、スループットは制限されません。

**--output-type** string   コマンドの出力形式。 選択肢には、text、json などがあります。 既定値は "text" です。 (既定値は "text")

**--trusted-microsoft-suffixes** 文字列   Azure Active Directory ログイン トークンを送信できる追加のドメイン サフィックスを指定します。  既定値は " *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net" です。 ここに記載されているすべてが既定値に追加されます。 セキュリティのために、Microsoft Azure のドメインのみをここに入力してください。 複数のエンティティは、セミコロンで区切ります。

## <a name="see-also"></a>関連項目

- [azcopy jobs](storage-ref-azcopy-jobs.md)

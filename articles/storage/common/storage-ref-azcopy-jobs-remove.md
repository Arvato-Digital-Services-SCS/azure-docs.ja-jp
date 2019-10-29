---
title: azcopy jobs resume | Microsoft Docs
description: この記事では、azcopy jobs remove コマンドに関する参照情報を提供します。
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 82c399580322334e67c0c9c2b88d1edf6f175e0c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518123"
---
# <a name="azcopy-jobs-remove"></a>azcopy jobs remove

指定されたジョブ ID に関連付けられているすべてのファイルを削除します。

> [!NOTE] 
> ログ ファイルとプラン ファイルが保存される場所をカスタマイズできます。 詳細については、[azcopy env](storage-ref-azcopy-env.md) コマンドを参照してください。

```
azcopy jobs remove [jobID] [flags]
```

## <a name="examples"></a>例

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>オプション

**-h, --help**                remove のヘルプを表示します。

## <a name="options-inherited-from-parent-commands"></a>親コマンドから継承されるオプション

**--cap-mbps uint32**      転送速度の上限を設定します (メガビット/秒)。 瞬間的なスループットは、上限と若干異なる場合があります。 このオプションを 0 に設定した場合や省略した場合、スループットは制限されません。

**--output-type** string   コマンドの出力形式。 選択肢には、text、json などがあります。 既定値は 'text' です (既定値 "text")。

## <a name="see-also"></a>関連項目

- [azcopy jobs](storage-ref-azcopy-jobs.md)

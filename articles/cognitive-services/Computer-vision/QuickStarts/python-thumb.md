---
title: クイック スタート:サムネイルの生成 - REST、Python
titleSuffix: Azure Cognitive Services
description: このクイック スタートでは、Computer Vision API と Python を使って、画像からサムネイルを生成します。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 05/20/2020
ms.author: pafarley
ms.custom: seodec18, tracking-python
ms.openlocfilehash: 762688aaf21f88ef1404bc09be32cf4129bfd0db
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84608844"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-python"></a>クイック スタート:Computer Vision の REST API と Python を使用したサムネイルを生成する

このクイックスタートでは、Computer Vision の REST API を使用して、画像からサムネイルを生成します。 [サムネイル取得](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/56f91f2e778daf14a499f20c)メソッドでは、必要な高さと幅を指定できます。また、Computer Vision では、スマート トリミングを使ってインテリジェントに関心領域を識別し、その領域に基づいてトリミングの座標を生成します。

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/try/cognitive-services/) を作成してください。

## <a name="prerequisites"></a>前提条件

- Computer Vision のサブスクリプション キーが必要です。 無料試用版のキーは「[Cognitive Services を試す](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)」から取得できます。 または、[Cognitive Services アカウントの作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)に関するページの手順に従って、Computer Vision をサブスクライブし、キーを取得します。 次に、キーとサービス エンドポイント文字列用に、それぞれ `COMPUTER_VISION_SUBSCRIPTION_KEY` と `COMPUTER_VISION_ENDPOINT` という名前の[環境変数を作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)します。
- コード エディター ([Visual Studio Code](https://code.visualstudio.com/download) など)。

## <a name="create-and-run-the-sample"></a>サンプルの作成と実行

サンプルを作成して実行するために、コード エディターに次のコードをコピーします。 

```python
import os
import sys
import requests
# If you are using a Jupyter notebook, uncomment the following lines.
# %matplotlib inline
# import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

thumbnail_url = endpoint + "vision/v3.0/generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

# Construct URL
headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data = {'url': image_url}
# Call API
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

# Open the image from bytes
thumbnail = Image.open(BytesIO(response.content))

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))

# Save thumbnail to file
thumbnail.save('thumbnail.png')

# Display image
thumbnail.show()

# Optional. Display the thumbnail from Jupyter.
# plt.imshow(thumbnail)
# plt.axis("off")
```

続けて次の作業を行います。

1. (省略可能) `image_url` の値を独自の画像の URL に置き換えます。
1. `.py` 拡張子のファイルとして、コードを保存します。 たとえば、「 `get-thumbnail.py` 」のように入力します。
1. コマンド プロンプト ウィンドウを開きます。
1. プロンプトで、`python` コマンドを使用してサンプルを実行します。 たとえば、「 `python get-thumbnail.py` 」のように入力します。

## <a name="examine-the-response"></a>結果の確認

成功応答は、サムネイル画像データを表すバイナリ データとして返されます。 この画像が表示されることが必要です。 要求が失敗した場合、応答がコマンド プロンプト ウィンドウに表示され、エラー コードが記録されている必要があります。

## <a name="run-in-jupyter-optional"></a>Jupyter での実行 (任意)

このクイック スタートは、必要に応じて [MyBinder](https://mybinder.org) 上で Jupyter Notebook を使い、ステップ バイ ステップで実行することができます。 Binder を起動するには、次のボタンを選択します。

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="next-steps"></a>次のステップ

次は、Computer Vision を使用して光学文字認識 (OCR) を実行する Python アプリケーションについて説明します。これは、スマートにクロップされたサムネイルを作成し、イメージ内の視覚的な特徴を検出、カテゴライズ、タグ付け、および記述します。

> [!div class="nextstepaction"]
> [Computer Vision API Python チュートリアル](../Tutorials/PythonTutorial.md)

* Computer Vision API を簡単に試す場合は、[Open API テスト コンソール](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/56f91f2e778daf14a499f20c)をお試しください。

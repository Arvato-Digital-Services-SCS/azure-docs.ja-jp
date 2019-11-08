---
title: クイック スタート:BLOB ストレージに格納された音声を認識する (C#) - Speech Service
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 77ab519ae966ab6b3dfc9fd309ce9a5740a5ce0f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505893"
---
## <a name="prerequisites"></a>前提条件

開始する前に、必ず次のことを行ってください。

> [!div class="checklist"]
> * [Azure Speech リソースを作成する](../../../../get-started.md)
> * [ソース ファイルを Azure BLOB にアップロードする](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
> * [使用する開発環境を設定する](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [空のサンプル プロジェクトを作成する](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Visual Studio でプロジェクトを開きます。

最初の手順として、ご利用のプロジェクトを Visual Studio で開いていることを確認します。

1. Visual Studio 2019 を起動します。
2. プロジェクトを読み込んで `Program.cs` を開きます。

## <a name="add-a-reference-to-newtonsoftjson"></a>NewtonSoftJSon への参照を追加する

1. ソリューション エクスプローラーで **helloworld** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択して NuGet パッケージ マネージャーを表示します。

1. 右上隅で **[パッケージ ソース]** ドロップダウン ボックスを探し、 **[`nuget.org`]** が選択されていることを確認します。

1. 左上隅で **[参照]** を選択します。

1. 検索ボックスで、「*newtonsoft.json*」と入力し、 **[入力]** を選択します。

1. 検索結果から **[Newtonsoft.Json]** パッケージを選択し、次に、 **[インストール]** を選択して最新の安定バージョンをインストールします。

1. すべての契約とライセンスに同意して、インストールを開始します。

   パッケージがインストールされると、 **[パッケージマネージャー コンソール]** ウィンドウに確認が表示されます。

## <a name="start-with-some-boilerplate-code"></a>スケルトン コードを使用して開始する

このプロジェクトのスケルトンとして機能するコードを追加してみましょう。

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=6-43,138,277)]
(`YourSubscriptionKey`、`YourServiceRegion`、および `YourFileUrl` の値を独自の値に置き換える必要があります。)
## <a name="json-wrappers"></a>JSON ラッパー

REST API は JSON 形式で要求を受け取り、結果も JSON で返すため、それらとは文字列のみを使用してやりとりすることが可能ですが、これについてはお勧めしていません。
要求と応答をより管理しやすくするために、JSON のシリアル化/逆シリアル化に使用するクラスをいくつか宣言します。

先に進み、`TranscribeAsync` の後に宣言を置きます。
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=140-276)]

## <a name="create-and-configure-an-http-client"></a>Http クライアントを作成して構成する
最初に必要なものは、正しいベース URL と認証セットを持つ Http クライアントです。
このコードを `TranscribeAsync` に挿入します。[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=46-50)]

## <a name="generate-a-transcription-request"></a>文字起こし要求を生成する
次に、文字起こし要求を生成します。 このコードを `TranscribeAsync` に追加します。[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=52-57)]

## <a name="send-the-request-and-check-its-status"></a>要求を送信し、その状態を確認する
次に、Speech Service に要求を投稿し、初期の応答コードを確認します。 この応答コードは、サービスで要求が受信されたかどうかを単に示すものです。 文字起こしの状態が格納される場所である URL が応答ヘッダーに入れられて、サービスから返されます。
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=59-70)]

## <a name="wait-for-the-transcription-to-complete"></a>文字起こしが完了するのを待つ
サービスでは文字起こしが非同期的に処理されるため、その状態を頻繁にポーリングする必要があります。 5 秒ごとにチェックします。

状態を確認するには、要求が投稿されたときに取得した URL でコンテンツを取得します。 コンテンツを取得したら、それをヘルパー クラスの 1 つに逆シリアル化することで、やりとりがさらに容易になります。

ここではポーリング コードと、すべての場合の状態の表示を示します。ただし、次に取り上げる正常な完了は除きます。
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=72-106,121-137)]

## <a name="display-the-transcription-results"></a>文字起こしの結果を表示する
サービスによる文字起こしが正常に完了すると、その結果は、状態の応答から取得できる別の URL に格納されます。

ここでは、それらの結果を読み取って逆シリアル化する前に、一時ファイルにダウンロードする要求を実行します。
結果が読み込まれたら、コンソールに出力できます。
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=107-120)]

## <a name="check-your-code"></a>コードを確認する
この時点で、コードは次のようになります。(このバージョンにはいくつかのコメントを追加してあります) [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=6-277)]

## <a name="build-and-run-your-app"></a>アプリをビルドして実行する

これで、アプリをビルドし、Speech サービスを使用して音声認識をテストする準備ができました。

1. **コードをコンパイルする** - Visual Studio のメニュー バーで、 **[ビルド]**  >  **[ソリューションのビルド]** の順に選択します。
2. **アプリを開始する** - メニュー バーで **[デバッグ]**  >  **[デバッグの開始]** の順に選択するか、**F5** キーを押します。
3. **認識を開始する** - 英語で語句を読み上げるように求められます。 ご自分の音声が Speech サービスに送信され、テキストとして文字起こしされて、コンソールに表示されます。

## <a name="next-steps"></a>次の手順

[!INCLUDE [footer](./footer.md)]

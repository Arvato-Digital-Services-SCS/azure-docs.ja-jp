---
title: Speech Devices SDK について - Speech Service
titleSuffix: Azure Cognitive Services
description: Speech Devices SDK を使ってみましょう。 Speech Service は、さまざまなデバイスやオーディオ ソースと連携します。 Speech Devices SDK は、専用マイク配列開発キットとの組み合わせで事前に調整されたライブラリです。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 31d1a60cc6c1316dcfccb4f9ba2571708fddd991
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072378"
---
# <a name="about-the-speech-devices-sdk"></a>Speech Devices SDK について

[Speech Services](overview.md) は、さまざまなデバイスやオーディオ ソースと連携します。 適合するハードウェアやソフトウェアで音声アプリケーションを次のレベルに引き上げることができます。 Speech Devices SDK は、専用マイク配列開発キットとの組み合わせで事前に調整されたライブラリです。

Speech Devices SDK は、次の場合に役立ちます。

- 新しい音声シナリオをすばやくテストする。
- クラウドベースの Speech Services をより簡単にお使いのデバイスに統合する。
- 顧客向けに優れたユーザー エクスペリエンスを作成する。

Speech Devices SDK は、[Speech SDK](speech-sdk.md) を利用しています。 Speech SDK を使って、Microsoft の先進の音声処理アルゴリズムによって処理された音声をデバイスのマイク配列から [Speech Services](overview.md) に送信します。 マルチチャンネル オーディオを使用して、ノイズの抑制、エコー キャンセレーション、ビームフォーミング、残響除去によってより正確な遠距離[音声認識](speech-to-text.md)が実現されます。

また、Speech Devices SDK を使用して、独自の[カスタマイズされたキーワード](speech-devices-sdk-create-kws.md)を持つアンビエント デバイスを構築することで、ユーザー操作を開始するキューをブランド固有のものにすることができます。

Speech Devices SDK を使用すると、[音声アシスタント](https://aka.ms/bots/speech/va)、ドライブスルー注文システム、[会話の文字起こし](conversation-transcription-service.md)、スマート スピーカーなど、さまざまな音声対応シナリオが容易になります。 ユーザーへのテキストでの応答、既定の音声または[カスタム音声](how-to-customize-voice-font.md)での応答、検索結果の提供、他の言語への[翻訳](speech-translation.md)などを行うことができます。 皆様の開発の成果を楽しみにしております。

## <a name="get-the-speech-devices-sdk"></a>Speech Devices SDK を取得する

### <a name="android"></a>Android

Android デバイスの場合は、[Android Speech Devices SDK](https://aka.ms/sdsdk-download-android) の最新バージョンをダウンロードします。

### <a name="windows"></a>Windows

Windows の場合、サンプル アプリケーションは、クロスプラット フォーム Java アプリケーションとして提供されます。 [JRE Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) の最新バージョンをダウンロードします。
アプリケーションは、Speech SDK パッケージと、64 ビット Windows 上の Eclipse Java IDE (v4) で構築されます。 これは、64 ビットの Java 8 のランタイム環境 (JRE) で実行されます。

### <a name="linux"></a>Linux

Linux の場合、サンプル アプリケーションは、クロスプラット フォーム Java アプリケーションとして提供されます。 [JRE Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) の最新バージョンをダウンロードします。
アプリケーションは、Speech SDK パッケージと、64 ビット Linux (Ubuntu 16.04、Ubuntu 18.04、Debian 9) 上の Eclipse Java IDE (v4) で構築されます。 これは、64 ビットの Java 8 のランタイム環境 (JRE) で実行されます。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Speech Device を選択する](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Speech Service のサブスクリプション キーを無料で取得する](get-started.md)

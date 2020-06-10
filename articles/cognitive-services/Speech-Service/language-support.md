---
title: 言語サポート - 音声サービス
titleSuffix: Azure Cognitive Services
description: 音声サービスでは、音声翻訳に加え、音声からテキストへの変換とテキストから音声への変換のためのさまざまな言語がサポートされます。 この記事では、サービス機能によってサポートされている言語の包括的な一覧を示します。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: trbye
ms.custom: seodec18
ms.openlocfilehash: 43476f3e3f125bc338ba25e8cce0e96baa76262b
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636106"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>音声サービスの言語と音声のサポート

言語サポートは、音声サービスの機能によって異なります。 次の表は、[音声テキスト変換](#speech-to-text)、[テキスト読み上げ](#text-to-speech)、および [音声翻訳](#speech-translation)サービスの言語サポートをまとめたものです。

## <a name="speech-to-text"></a>音声テキスト変換

Microsoft Speech SDK と REST API は、どちらも以下の言語 (ロケール) をサポートしています。 

精度を高めるために、 **"オーディオ + 人間" というラベルが付いたトランスクリプト**または**関連テキスト (文)** をアップロードすることにより、言語のサブセットに対してカスタマイズが提供されます。 カスタマイズの詳細については、[Custom Speech の利用の開始](how-to-custom-speech.md)に関するページを参照してください。

発音の質を向上させる方法の詳細については、[Custom Speech のモデルの改良](how-to-custom-speech-improve-accuracy.md#add-new-words-with-pronunciation)に関するセクションを参照してください。

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Locale  | Language                          | サポートされています | カスタマイズ                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | アラビア語 (UAE)                      | はい       | いいえ                                                |
| `ar-BH` | アラビア語 (バーレーン)、現代標準 | はい       | 言語モデル                                    |
| `ar-EG` | アラビア語 (エジプト)                    | はい       | 言語モデル                                    |
| `ar-IL` | アラビア語 (イスラエル)                   | はい       | いいえ                                                |
| `ar-JO` | アラビア語 (ヨルダン)                   | はい       | いいえ                                                |
| `ar-KW` | アラビア語 (クウェート)                   | はい       | いいえ                                                |
| `ar-LB` | アラビア語 (レバノン)                  | はい       | いいえ                                                |
| `ar-PS` | アラビア語 (パレスチナ)                | はい       | いいえ                                                |
| `ar-QA` | アラビア語 (カタール)                    | はい       | いいえ                                                |
| `ar-SA` | アラビア語 (サウジアラビア)             | はい       | いいえ                                                |
| `ar-SY` | アラビア語 (シリア)                    | はい       | 言語モデル                                    |
| `ca-ES` | カタロニア語                           | はい       | 言語モデル                                    |
| `da-DK` | デンマーク語 (デンマーク)                  | はい       | 言語モデル                                    |
| `de-DE` | ドイツ語 (ドイツ)                  | はい       | 音響モデル<br>言語モデル<br>発音 |
| `en-AU` | 英語 (オーストラリア)               | はい       | 音響モデル<br>言語モデル                  |
| `en-CA` | 英語 (カナダ)                  | はい       | 音響モデル<br>言語モデル                  |
| `en-GB` | 英語 (イギリス)          | はい       | 音響モデル<br>言語モデル<br>発音 |
| `en-IN` | 英語 (インド)                   | はい       | 音響モデル<br>言語モデル                  |
| `en-NZ` | 英語 (ニュージーランド)             | はい       | 音響モデル<br>言語モデル                  |
| `en-US` | 英語 (米国)           | はい       | 音響モデル<br>言語モデル<br>発音 |
| `es-ES` | スペイン語 (スペイン)                   | はい       | 音響モデル<br>言語モデル                  |
| `es-MX` | スペイン語 (メキシコ)                  | はい       | 音響モデル<br>言語モデル                  |
| `fi-FI` | フィンランド語 (フィンランド)                 | はい       | 言語モデル                                    |
| `fr-CA` | フランス語 (カナダ)                   | はい       | 音響モデル<br>言語モデル                  |
| `fr-FR` | フランス語 (フランス)                   | はい       | 音響モデル<br>言語モデル<br>発音 |
| `gu-IN` | グジャラート語 (インド)                 | はい       | 言語モデル                                    |
| `hi-IN` | ヒンディー語 (インド)                     | はい       | 音響モデル<br>言語モデル                  |
| `it-IT` | イタリア語 (イタリア)                   | はい       | 音響モデル<br>言語モデル<br>発音 |
| `ja-JP` | 日本語 (日本)                  | はい       | 言語モデル                                    |
| `ko-KR` | 韓国語 (韓国)                    | はい       | 言語モデル                                    |
| `mr-IN` | マラーティー語 (インド)                   | はい       | 言語モデル                                    |
| `nb-NO` | ノルウェー語 (ブークモール) (ノルウェー)       | はい       | 言語モデル                                    |
| `nl-NL` | オランダ語 (オランダ)               | はい       | 言語モデル                                    |
| `pl-PL` | ポーランド語 (ポーランド)                   | はい       | 言語モデル                                    |
| `pt-BR` | ポルトガル語 (ブラジル)               | はい       | 音響モデル<br>言語モデル<br>発音 |
| `pt-PT` | ポルトガル語 (ポルトガル)             | はい       | 言語モデル                                    |
| `ru-RU` | ロシア語 (ロシア)                  | はい       | 音響モデル<br>言語モデル                  |
| `sv-SE` | スウェーデン語 (スウェーデン)                  | はい       | 言語モデル                                    |
| `ta-IN` | タミール語 (インド)                     | はい       | 言語モデル                                    |
| `te-IN` | テルグ語 (インド)                    | はい       | 言語モデル                                    |
| `th-TH` | タイ語 (タイ)                   | はい       | いいえ                                                |
| `tr-TR` | トルコ語 (トルコ)                  | はい       | 言語モデル                                    |
| `zh-CN` | 中国語 (標準、簡体字)    | はい       | 音響モデル<br>言語モデル                  |
| `zh-HK` | 中国語 (繁体字)  | はい       | 言語モデル                                    |
| `zh-TW` | 中国語 (台湾標準中国語)      | はい       | 言語モデル                                    |

## <a name="text-to-speech"></a>テキスト読み上げ

Microsoft Speech SDK と REST API のどちらでもこれらの音声がサポートされ、そのいずれでもロケールで識別される特定の言語と方言がサポートされています。 [音声/リスト API](rest-text-to-speech.md#get-a-list-of-voices) を使用して、特定のリージョン/エンドポイントごとにサポートされている言語と音声の完全な一覧を取得することもできます。 

> [!IMPORTANT]
> 価格は、標準音声、カスタム音声、ニューラル音声ごとに異なります。 詳細については、[価格](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)のページを参照してください。

### <a name="neural-voices"></a>ニューラル音声

ニューラル テキスト読み上げは、ディープ ニューラル ネットワークを利用した新しい種類の音声合成です。 ニューラル音声を使用した場合、合成音声は人間の録音とほとんど区別がつきません。

ニューラル音声を使用すると、チャットボットや音声アシスタントとの対話をより自然で魅力的なものにできます。また、電子書籍などのデジタル テキストをオーディオブックに変換したり、カーナビゲーション システムを強化したりすることもできます。 ニューラル音声では、人間のような自然な韻律と明瞭な発音により、ユーザーが AI システムと対話する際のリスニング疲労が大幅に軽減されます。

リージョン別の提供状況の詳細については、[リージョン](regions.md#standard-and-neural-voices)に関するページを参照してください。

|Locale  | Language            | 性別 | 音声名 | スタイルのサポート |
|--|--|--|--|--|
| `de-DE` | ドイツ語 (ドイツ)                | Female | "de-DE-KatjaNeural"      | 全般 |
| `en-AU` | 英語 (オーストラリア)             | Female | "en-AU-NatashaNeural"    | 全般 |
| `en-CA` | 英語 (カナダ)                | Female | "en-CA-ClaraNeural"      | 全般 |
| `en-GB` | 英語 (英国)                    | Female | "en-GB-LibbyNeural"      | 全般 |
|         |                                 | Female | "en-GB-MiaNeural"        | 全般 |
| `en-US` | 英語 (米国)                    | Female | "en-US-AriaNeural"       | 全般、複数の音声スタイルを使用可能 |
|         |                                 | Male   | "en-US-GuyNeural"        | 全般 |
| `es-ES` | スペイン語 (スペイン)                 | Female | "es-ES-ElviraNeural"     | 全般 |
| `es-MX` | スペイン語 (メキシコ)                | Female | "es-MX-DaliaNeural"      | 全般 |
| `fr-CA` | フランス語 (カナダ)                 | Female | "fr-CA-SylvieNeural"     | 全般 |
| `fr-FR` | フランス語 (フランス)                 | Female | "fr-FR-DeniseNeural"     | 全般 |
| `it-IT` | イタリア語 (イタリア)                 | Female | "it-IT-ElsaNeural"       | 全般 |
| `ja-JP` | 日本語                        | Female | "ja-JP-NanamiNeural"     | 全般 |
| `ko-KR` | 韓国語                          | Female | "ko-KR-SunHiNeural"      | 全般 |
| `nb-NO` | ノルウェー語                       | Female | "nb-NO-IselinNeural"     | 全般 |
| `pt-BR` | ポルトガル語 (ブラジル)             | Female | "pt-BR-FranciscaNeural"  | 全般 |
| `tr-TR` | トルコ語                         | Female | "tr-TR-EmelNeural"       | 全般 |
| `zh-CN` | 中国語 (標準、簡体字)  | Female | "zh-CN-XiaoxiaoNeural"   | 全般、複数の音声スタイルを使用可能 |
|         |                                 | Female | "zh-CN-XiaoyouNeural"    | 子供の音声、ストーリーのナレーション向けに最適化 |
|         |                                 | Male   | "zh-CN-YunyangNeural"    | ニュースの読み上げ用に最適化、複数の音声スタイルを使用可能 |
|         |                                 | Male   | "zh-CN-YunyeNeural"      | ストーリーのナレーション向けに最適化 |

> [!IMPORTANT]
> `en-US-JessaNeural` 音声が `en-US-AriaNeural` に変更されました。 前に "Jessa" を使用していた場合は、"Aria" に変換します。

ニューラル音声を構成および調整する方法については、「[音声合成マークアップ言語](speech-synthesis-markup.md#adjust-speaking-styles)」を参照してください。

> [!TIP]
> 音声合成要求では、"Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)" のような完全なサービス名マッピングを引き続き使用できます。

### <a name="standard-voices"></a>標準音声

テキストから合成音声への変換用に、45 を超える言語とロケールで 75 を超える標準的な音声が用意されています。 リージョン別の提供状況の詳細については、[リージョン](regions.md#standard-and-neural-voices)に関するページを参照してください。

| Locale | Language | 性別 | 音声名 |
|--|--|--|--|
| <sup>1</sup>`ar-EG` | アラビア語 (エジプト) | Female | "ar-EG-Hoda" |
| `ar-SA` | アラビア語 (サウジアラビア) | Male | "ar-SA-Naayf" |
| `bg-BG` | ブルガリア語 | Male |  "bg-BG-Ivan" |
| `ca-ES` | カタロニア語 | Female |  "ca-ES-HerenaRUS" |
| `cs-CZ` | チェコ語 | Male | "cs-CZ-Jakub" |
| `da-DK` | デンマーク語 | Female |  "da-DK-HelleRUS" |
| `de-AT` | ドイツ語 (オーストリア) | Male | "de-AT-Michael" |
| `de-CH` | ドイツ語 (スイス) | Male |  "de-CH-Karsten" |
| `de-DE` | ドイツ語 (ドイツ) | Female |  "de-DE-Hedda" |
|  |  | Female | "de-DE-HeddaRUS" |
|  |  | Male |  "de-DE-Stefan-Apollo" |
| `el-GR` | ギリシャ語 | Male | "el-GR-Stefanos" |
| `en-AU` | 英語 (オーストラリア) | Female |  "en-AU-Catherine" |
|  |  | Female |  "en-AU-HayleyRUS" |
| `en-CA` | 英語 (カナダ) | Female |  "en-CA-Linda" |
|  |  | Female |  "en-CA-HeatherRUS" |
| `en-GB` | 英語 (英国) | Female |  "en-GB-Susan-Apollo" |
|  |  | Female |  "en-GB-HazelRUS" |
|  |  | Male |  "en-GB-George-Apollo" |
| `en-IE` | 英語 (アイルランド) | Male | "en-IE-Sean" |
| `en-IN` | 英語 (インド) | Female | "en-IN-Heera-Apollo" |
|  |  | Female |  "en-IN-PriyaRUS" |
|  |  | Male |  "en-IN-Ravi-Apollo" |
| `en-US` | 英語 (米国) | Female |  "en-US-ZiraRUS" |
|  |  | Female | "en-US-AriaRUS" |
|  |  | Male | "en-US-BenjaminRUS" |
|  |  | Male |  "en-US-Guy24kRUS" |
| `es-ES` | スペイン語 (スペイン) | Female |  "es-ES-Laura-Apollo" |
|  |  | Female | "es-ES-HelenaRUS" |
|  |  | Male | "es-ES-Pablo-Apollo" |
| `es-MX` | スペイン語 (メキシコ) | Female |  "es-MX-HildaRUS" |
|  |  | Male | "es-MX-Raul-Apollo" |
| `fi-FI` | フィンランド語 | Female | "fi-FI-HeidiRUS" |
| `fr-CA` | フランス語 (カナダ) | Female | "fr-CA-Caroline" |
|  |  | Female | "fr-CA-HarmonieRUS" |
| `fr-CH` | フランス語 (スイス) | Male | "fr-CH-Guillaume" |
| `fr-FR` | フランス語 (フランス) | Female |  "fr-FR-Julie-Apollo" |
|  |  | Female |"fr-FR-HortenseRUS" |
|  |  | Male |  "fr-FR-Paul-Apollo" |
| `he-IL` | ヘブライ語 (イスラエル) | Male |  "he-IL-Asaf" |
| `hi-IN` | ヒンディー語 (インド) | Female | "hi-IN-Kalpana-Apollo" |
|  |  | Female |  "hi-IN-Kalpana" |
|  |  | Male |  "hi-IN-Hemant" |
| `hr-HR` | クロアチア語 | Male | "hr-HR-Matej" |
| `hu-HU` | ハンガリー語 | Male |  "hu-HU-Szabolcs" |
| `id-ID` | インドネシア語 | Male | "id-ID-Andika" |
| `it-IT` | イタリア語 | Male |  "it-IT-Cosimo-Apollo" |
|  |  | Female |  "it-IT-LuciaRUS" |
| `ja-JP` | 日本語 | Female |  "ja-JP-Ayumi-Apollo" |
|  |  | Male | "ja-JP-Ichiro-Apollo" |
|  |  | Female |  "ja-JP-HarukaRUS" |
| `ko-KR` | 韓国語 | Female | "ko-KR-HeamiRUS" |
| `ms-MY` | マレー語 | Male |  "ms-MY-Rizwan" |
| `nb-NO` | ノルウェー語 | Female |  "nb-NO-HuldaRUS" |
| `nl-NL` | オランダ語 | Female |  "nl-NL-HannaRUS" |
| `pl-PL` | ポーランド語 | Female |  "pl-PL-PaulinaRUS" |
| `pt-BR` | ポルトガル語 (ブラジル) | Female | "pt-BR-HeloisaRUS" |
|  |  | Male |  "pt-BR-Daniel-Apollo" |
| `pt-PT` | ポルトガル語 (ポルトガル) | Female | "pt-PT-HeliaRUS" |
| `ro-RO` | ルーマニア語 | Male | "ro-RO-Andrei" |
| `ru-RU` | ロシア語 | Female |  "ru-RU-Irina-Apollo" |
|  |  | Male | "ru-RU-Pavel-Apollo" |
|  |  | Female |  ru-RU-EkaterinaRUS |
| `sk-SK` | スロバキア語 | Male | "sk-SK-Filip" |
| `sl-SI` | スロベニア語 | Male |  "sl-SI-Lado" |
| `sv-SE` | スウェーデン語 | Female | "sv-SE-HedvigRUS" |
| `ta-IN` | タミール語 (インド) | Male |  "ta-IN-Valluvar" |
| `te-IN` | テルグ語 (インド) | Female |  "te-IN-Chitra" |
| `th-TH` | タイ語 | Male |  "th-TH-Pattara" |
| `tr-TR` | トルコ語 (トルコ) | Female | "tr-TR-SedaRUS" |
| `vi-VN` | ベトナム語 | Male |  "vi-VN-An" |
| `zh-CN` | 中国語 (標準、簡体字) | Female |  "zh-CN-HuihuiRUS" |
|  |  | Female | "zh-CN-Yaoyao-Apollo" |
|  |  | Male | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | 中国語 (繁体字) | Female |  "zh-HK-Tracy-Apollo" |
|  |  | Female | "zh-HK-TracyRUS" |
|  |  | Male |  "zh-HK-Danny-Apollo" |
| `zh-TW` | 中国語 (台湾標準中国語) | Female |  "zh-TW-Yating-Apollo" |
|  |  | Female | "zh-TW-HanHanRUS" |
|  |  | Male |  "zh-TW-Zhiwei-Apollo" |

**1** *ar-EG では現代標準アラビア語 (MSA) をサポートしています。*

> [!IMPORTANT]
> `en-US-Jessa` 音声が `en-US-Aria` に変更されました。 前に "Jessa" を使用していた場合は、"Aria" に変換します。

> [!TIP]
> 音声合成要求では、"Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)" のような完全なサービス名マッピングを引き続き使用できます。

### <a name="customization"></a>カスタマイズ

音声のカスタマイズは、`de-DE`、`en-GB`、`en-IN`、`en-US`、`es-MX`、`fr-FR`、`it-IT`、`pt-BR`、および `zh-CN` に使用できます。 カスタム音声モデルをトレーニングする必要があるトレーニング データと一致する適切なロケールを選択します。 たとえば、持っている録音データが英国アクセントの英語で話されている場合は、`en-GB` を選択します。

> [!NOTE]
> Custom Voice では、中国語と英語のバイリンガルを除き、バイリンガル モデル トレーニングはサポートされていません。 英語も話すことができる中国人の音声をトレーニングする場合は、[Chinese-English bilingual]\(中国語 - 英語のバイリンガル\) を選択します。 すべてのロケールの音声トレーニングは、任意のサイズのトレーニング データから始めることができる `en-US` と `zh-CN` を除き、2,000 以上の発話のデータ セットから始まります。

## <a name="speech-translation"></a>音声翻訳

**Speech Translation** API では、音声を音声に翻訳し、音声をテキストに翻訳するために、さまざまな言語をサポートしています。 ソース言語は、音声テキスト変換言語の表に常に含まれている必要があります。 使用可能なターゲット言語は、何に翻訳するか (音声かテキストか) によって決まります。 受信した音声は [60 言語](https://www.microsoft.com/translator/business/languages/)以上に翻訳できます。 一部の言語は[音声合成](language-support.md#text-languages)に利用できます。

### <a name="text-languages"></a>テキスト言語

| テキスト言語           | 言語コード |
|:------------------------|:-------------:|
| アフリカーンス語               | `af`          |
| アラビア語                  | `ar`          |
| ベンガル語                  | `bn`          |
| ボスニア語 (ラテン)         | `bs`          |
| ブルガリア語               | `bg`          |
| 広東語 (繁体字) | `yue`         |
| カタロニア語                 | `ca`          |
| 簡体中国語      | `zh-Hans`     |
| 中国語 (繁体字)     | `zh-Hant`     |
| クロアチア語                | `hr`          |
| チェコ語                   | `cs`          |
| デンマーク語                  | `da`          |
| オランダ語                   | `nl`          |
| 英語                 | `en`          |
| エストニア語                | `et`          |
| フィジー語                  | `fj`          |
| フィリピン語                | `fil`         |
| フィンランド語                 | `fi`          |
| フランス語                  | `fr`          |
| ドイツ語                  | `de`          |
| ギリシャ語                   | `el`          |
| グジャラート語                | `gu`          |
| ハイチ・クレオール語          | `ht`          |
| ヘブライ語                  | `he`          |
| ヒンディー語                   | `hi`          |
| ミャオ語               | `mww`         |
| ハンガリー語               | `hu`          |
| インドネシア語              | `id`          |
| アイルランド語                   | `ga`          |
| イタリア語                 | `it`          |
| 日本語                | `ja`          |
| カンナダ語                 | `kn`          |
| スワヒリ語               | `sw`          |
| クリンゴン語                 | `tlh`         |
| クリンゴン語 (plqaD)         | `tlh-Qaak`    |
| 韓国語                  | `ko`          |
| ラトビア語                 | `lv`          |
| リトアニア語              | `lt`          |
| マダガスカル語                | `mg`          |
| マレー語                   | `ms`          |
| マラヤーラム語               | `ml`          |
| マルタ語                 | `mt`          |
| マオリ語                   | `mi`          |
| マラーティー語                 | `mr`          |
| ノルウェー語               | `nb`          |
| ペルシャ語                 | `fa`          |
| ポーランド語                  | `pl`          |
| ポルトガル語 (ブラジル)     | `pt-br`       |
| ポルトガル語 (ポルトガル)   | `pt-pt`       |
| パンジャーブ語                 | `pa`          |
| オトミ語         | `otq`         |
| ルーマニア語                | `ro`          |
| ロシア語                 | `ru`          |
| サモア語                  | `sm`          |
| セルビア語 (キリル)      | `sr-Cyrl`     |
| セルビア語 (ラテン)         | `sr-Latn`     |
| スロバキア語                  | `sk`          |
| スロベニア語               | `sl`          |
| スペイン語                 | `es`          |
| スウェーデン語                 | `sv`          |
| タヒチ語                | `ty`          |
| タミル語                   | `ta`          |
| テルグ語                  | `te`          |
| タイ語                    | `th`          |
| トンガ語                  | `to`          |
| トルコ語                 | `tr`          |
| ウクライナ語               | `uk`          |
| ウルドゥ語                    | `ur`          |
| ベトナム語              | `vi`          |
| ウェールズ語                   | `cy`          |
| ユカテコ語            | `yua`         |

## <a name="next-steps"></a>次のステップ

* [音声サービス試用版サブスクリプションを取得する](https://azure.microsoft.com/try/cognitive-services/)
* [C# で音声を認識する方法を確認する](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)

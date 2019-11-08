---
title: Azure Blockchain Tokens の構成可能性
description: Azure Blockchain Tokens の構成可能性により、高度なシナリオに合わせて柔軟にトークンを作成できます。
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
ms.openlocfilehash: fe932d3ae1874e7a35abb9729a0b80e4fa3512eb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510120"
---
# <a name="azure-blockchain-tokens-composability"></a>Azure Blockchain Tokens の構成可能性

[!INCLUDE [Preview note](./includes/preview.md)]

トークンの構成可能性により、高度なシナリオに合わせて柔軟にトークンを作成できます。 複雑なシナリオで、[4 つの構築済みのトークン テンプレート](templates.md#base-token-types)を使用しても実装できない場合があります。 トークンの構成可能性により、定義済みの動作を追加または削除して独自のトークン テンプレートを構築することで、独自のトークン テンプレートを設計できます。 新しいトークン テンプレートを作成すると、Azure Blockchain Tokens によってすべてのトークンの文法規則が検証されます。 作成されたテンプレートは、接続されているブロック チェーン ネットワークに発行するために Azure Blockchain Tokens サービスで保存されます。

次のセクションの[トークン ビヘイビアー](templates.md#token-behaviors)を使用して、トークン テンプレートを設計できます。

## <a name="burnable-b"></a>Burnable (消費可能) (b)

トークンをサプライから削除する機能。

たとえば、ギフト カードのオンライン クレジット カード ポイントを利用すると、クレジット カード ポイントが消費されます。

## <a name="delegable-g"></a>Delegable (委任可能) (g)

所有しているトークンに対して実行されるアクションを委任する機能。

代理人は、トークンの所有者としてアクションを実行できます。 たとえば、投票の実行に委任可能なトークンを使用できます。 委任可能なトークンを使用すると、投票トークンの所有者は、自分に代わって他のユーザーに投票してもらうことができます。

## <a name="logable-l"></a>Logable (ログ記録可能) (l)

ログに記録する機能。

たとえば、特定の映画を上映することができる映画配信用のログ記録可能なトークンを各シアターに発行することができます。 映画を再生する場合、ロイヤルティの支払いは映画のリリース期間中の上映ごとに行われるため、上映では各上映のトランザクションを記録する必要があります。 俳優ビルドでは、映画トークンを使用して、配信で映画館ごとに上映される映画ごとの支払いを検証できます。

## <a name="mint-able-m"></a>Mint-able (作成可能) (m)

トークン クラスの追加のトークンを作り出す機能。 minter ロールには、この作成可能ビヘイビアーが含まれます。

たとえば、ロイヤルティ プログラムを実装する小売企業の場合、ロイヤルティ プログラムに作成可能トークンを使用できます。 顧客ベースの成長に合わせて、顧客のために追加のロイヤルティ ポイントを作り出すことができます。  

## <a name="non-subdividable-or-whole-d"></a>再分割不可または全体 (~d)

トークンが細分化されないように防ぐための制限。

たとえば、1 つの絵画を複数の小さなパーツに分割することはできません。 

## <a name="non-transferable-t"></a>譲渡不可 (~t)

初期トークン所有者からの所有権の変更を防ぐための制限。

たとえば、大学の卒業証書は譲渡不可トークンです。 卒業証書が卒業生に渡された後に、卒業生から他の人に譲渡することはできません。

## <a name="roles-r"></a>ロール (r)

特定の動作のトークン テンプレート クラス内でロールを定義する機能。

トークンの作成時にトークンがサポートするロール名の一覧を提供できます。 ロールを指定すると、ユーザーはこれらの動作にロールを割り当てることができます。 現時点では、minter ロールのみがサポートされています。

## <a name="singleton-s"></a>Singleton (シングルトン) (s)

1 つのトークンの提供を許可する制限。

たとえば、博物館の展示物はシングルトン トークンです。 博物館の展示物は唯一のものです。 展示物を表すトークンは、サプライに単一の項目のみを持ちます。

## <a name="subdividable-d"></a>Subdividable (再分割可能) (d)

1 つのトークンをより小さなパーツに分割する機能。

たとえば、ドルはセントに分割することができます。

## <a name="transferable-t"></a>譲渡可能 (t)

トークンの所有権を譲渡する機能。

たとえば、不動産の権原は譲渡可能なトークンであり、不動産が売却されるときに、ある人から別の人に譲渡される可能性があります。

## <a name="next-steps"></a>次の手順

[Azure Blockchain Tokens アカウントの管理](account-management.md)について学習します。

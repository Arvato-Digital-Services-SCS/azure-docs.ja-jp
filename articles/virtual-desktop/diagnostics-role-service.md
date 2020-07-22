---
title: Windows Virtual Desktop の診断の問題 - Azure
description: Windows Virtual Desktop の診断機能を使用して問題を診断する方法。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2ead16c655d4790e81931371e67da8106dabf83e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85200547"
---
# <a name="identify-and-diagnose-issues"></a>問題の特定と診断

>[!IMPORTANT]
>このコンテンツは、Azure Resource Manager Windows Virtual Desktop オブジェクトと Spring 2020 更新プログラムの組み合わせに適用されます。 Azure Resource Manager オブジェクトなしで Windows Virtual Desktop Fall 2019 リリースを使用している場合は、[この記事](./virtual-desktop-fall-2019/diagnostics-role-service-2019.md)を参照してください。
>
> Windows Virtual Desktop Spring 2020 更新プログラムは現在、パブリック プレビュー段階です。 このプレビュー バージョンはサービス レベル アグリーメントなしで提供されており、運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。
> 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

Windows Virtual Desktop では、管理者が単一のインターフェイスを使用して問題を特定できる診断機能が提供されます。 Windows Virtual Desktop の診断機能の詳細については、「[診断機能に Log Analytics を使用する](diagnostics-log-analytics.md)」を参照してください。

診断ロール サービス自体が Windows Virtual Desktop の一部であるため、Windows Virtual Desktop に到達しない接続は診断結果に表示されません。 Windows Virtual Desktop 接続の問題は、エンドユーザーにネットワーク接続の問題が発生しているときに発生する可能性があります。

## <a name="common-error-scenarios"></a>一般的なエラー シナリオ

エラー シナリオは、サービスの内部と Windows Virtual Desktop の外部に分類されます。

* 内部の問題: 顧客が緩和できず、サポートの問題として解決する必要があるシナリオを指定します。 [Windows Virtual Desktop Tech コミュニティ](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)を通してフィードバックを提供する場合は、関連付け ID と問題が発生したときのおおよその概算時間を含めてください。
* 外部の問題: 顧客が緩和できるシナリオに関連します。 これらは、Windows Virtual Desktop の外部で発生します。

次の表では、管理者が経験する可能性のある一般的なエラーの一覧を示します。

>[!NOTE]
>この一覧には、最も一般的なエラーが含まれており、定期的に更新されます。 最新の情報を得るために、必ず 1 か月に 1 回以上この記事をご確認ください。

## <a name="management-errors"></a>管理エラー

|エラー メッセージ|推奨されている解決方法|
|---|---|
|登録キーを作成できませんでした |登録トークンを作成できませんでした。 有効期限を短くして (1 時間から 1 か月)、もう一度作成してみてください。 |
|登録キーを削除できませんでした|登録トークンを削除できませんでした。 もう一度削除してみてください。 それでもうまくいかない場合は、PowerShell を使用して、トークンがまだ存在するかどうかを確認します。 存在する場合は、PowerShell を使用して削除します。|
|セッション ホストのドレイン モードを変更できませんでした |VM でドレイン モードを変更できませんでした。 VM の状態を確認します。 VM を使用できない場合、ドレイン モードを変更することはできません。|
|ユーザー セッションを切断できませんでした |VM からユーザーを切断できませんでした。 VM の状態を確認します。 VM を使用できない場合、ユーザー セッションを切断することはできません。 VM を使用できる場合は、ユーザー セッションの状態を確認して、接続が切断されていないかどうかを確認します。 |
|セッション ホスト内のすべてのユーザーをログオフできませんでした |VM からユーザーをサインアウトすることができませんでした。 VM の状態を確認します。 使用できない場合、ユーザーはサインアウトできません。ユーザー セッションの状態を確認して、既にサインアウトされていないかどうかを確認します。PowerShell を使用して強制的にサインアウトすることができます。 |
|アプリケーション グループからユーザーの割り当てを解除できませんでした|ユーザーのアプリ グループの公開を取り消すことができませんでした。 ユーザーが Azure AD で利用できるかどうかを確認します。 ユーザーが、アプリ グループが公開されているユーザー グループに属しているかどうかを確認します。 |
|利用できる場所の取得でエラーが発生しました |ホスト プールの作成ウィザードで使用されている VM の場所を確認します。 その場所でイメージを使用できない場合は、その場所にイメージを追加するか、別の VM の場所を選択します。 |

### <a name="external-connection-error-codes"></a>外部接続のエラー コード

|数値コード|エラー コード|推奨されている解決方法|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|セッション ホストは、Active Directory に正しく参加していません。|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|セッション ホストが使用できないため、接続が失敗しました。 セッション ホストの正常性を確認してください。|
|-2146233088|ConnectionFailedClientDisconnect|このエラーが頻繁に発生する場合は、ユーザーのコンピューターがネットワークに接続されていることを確認してください。|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|ホスト ユーザーが接続しようとしたセッションが正常ではありません。 仮想マシンをデバッグしてください。|
|-2146233088|ConnectionFailedUserNotAuthorized|ユーザーは、公開されたアプリまたはデスクトップにアクセスするためのアクセス許可を持っていません。 このエラーは、公開されたリソースを管理者が削除した後に発生する可能性があります。 リモート デスクトップ アプリケーション内のフィードを更新するようユーザーに依頼してください。|
|2|FileNotFound|ユーザーがアクセスしようとしたアプリケーションが正しくインストールされていないか、正しくないパスに設定されています。<br><br>ユーザーがアクティブなセッションを使用しているときに新しいアプリを公開すると、ユーザーはこのアプリにアクセスできなくなります。 ユーザーがアプリにアクセスできるようにするには、セッションをシャットダウンして再起動する必要があります。 |
|3|InvalidCredentials|ユーザーが入力したユーザー名またはパスワードが、既存のどのユーザー名またはパスワードとも一致しません。 資格情報に入力ミスがないことを確認し、もう一度やり直してください。|
|8|ConnectionBroken|クライアントとゲートウェイまたはサーバーとの間の接続が削除されました。 予期せず発生した場合を除き、アクションは不要です。|
|14|UnexpectedNetworkDisconnect|ネットワークへの接続が削除されました。 ユーザーにもう一度接続するよう依頼してください。|
|24|ReverseConnectFailed|ホスト仮想マシンには、RD ゲートウェイへの直接の見通し線がありません。 ゲートウェイ IP アドレスを解決できることを確認してください。|

## <a name="next-steps"></a>次のステップ

Windows Virtual Desktop 内のロールについて詳しくは、「[Windows Virtual Desktop 環境](environment-setup.md)」をご覧ください。

Windows Virtual Desktop に使用可能な PowerShell コマンドレットの一覧を表示するには、[PowerShell リファレンス](/powershell/windows-virtual-desktop/overview)に関する記事をご覧ください。

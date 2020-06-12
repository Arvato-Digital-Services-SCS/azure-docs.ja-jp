---
title: Azure Migrate アプライアンスのデプロイと検出のトラブルシューティング
description: Azure Migrate アプライアンスのデプロイとマシンの検出に関するヘルプを表示します。
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 3d9e4e54d2b1186278afc72c72cdd6bcf33dd41b
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235454"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Azure Migrate アプライアンスと検出のトラブルシューティング

この記事は、[Azure Migrate](migrate-services-overview.md) アプライアンスをデプロイし、そのアプライアンスを使用してオンプレミスのマシンを検出する際の問題のトラブルシューティングに役立ちます。


## <a name="whats-supported"></a>サポート対象

アプライアンスのサポート要件を[確認](migrate-appliance.md)します。


## <a name="invalid-ovf-manifest-entry"></a>"Invalid OVF manifest entry (無効な OVF マニフェスト エントリ)"

"The provided manifest file is invalid: Invalid OVF manifest entry" (指定されたマニフェスト ファイルが無効です: 無効な OVF マニフェスト エントリ) というエラーを受け取る場合は、次のようにします。

1. Azure Migrate アプライアンスの OVA ファイルが正常にダウンロードされていることを、そのハッシュ値をチェックして確認します。 [詳細については、こちらを参照してください](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware)。 ハッシュ値が一致しない場合は、OVA ファイルをもう一度ダウンロードしてデプロイを再試行してください。
2. それでもデプロイが失敗する場合で、VMware vSphere クライアントを使って OVF ファイルをデプロイしている場合は、vSphere Web クライアントでデプロイしてみてください。 展開がそれでも失敗する場合は、別の Web ブラウザーをお試しください。
3. vSphere Web クライアントを使用しており、vCenter Server 6.5 または 6.7 にそれをデプロイしようとしている場合は、ESXi ホストに直接 OVA をデプロイしてみてください。
   - Web クライアント (https://<*ホスト IP アドレス*>/ui) を使用して (vCenter Server ではなく) ESXi ホストに直接接続します。
   - **[ホーム]**  >  **[インベントリ]** で、 **[ファイル]**  >  **[Deploy OVF template]\(OVF テンプレートのデプロイ\)** を選択します。 OVA を参照して、デプロイを完了する。
4. それでもデプロイが失敗する場合は、Azure Migrate のサポートにお問い合わせください。

## <a name="cant-connect-to-the-internet"></a>インターネットに接続できない

これは、アプライアンス マシンがプロキシの内側にある場合に発生する可能性があります。

- プロキシに承認資格情報が必要な場合は、それを提供します。
- URL ベースのファイアウォール プロキシを使用して送信接続を制御している場合は、[以下の URL](migrate-appliance.md#url-access) を許可リストに追加します。
- インターネットへの接続にインターセプト プロキシを使用している場合は、[こちらの手順](https://docs.microsoft.com/azure/migrate/concepts-collector)を使用して、プロキシの証明書をアプライアンス VM にインポートします。

## <a name="cant-sign-into-azure-from-the-appliance-web-app"></a>アプライアンス Web アプリから Azure にサインインできない

Azure へのサインインに正しくない Azure アカウントを使用している場合は、"申し訳ありませんが、サインイン中に問題が発生しました" というエラーが表示されます。 このエラーは、次のいくつかの理由で発生します。

- パブリック クラウド用のアプライアンス Web アプリケーションにサインインする場合に、Government クラウド ポータルのユーザー アカウント資格情報を使用している。
- Government クラウド用のアプライアンス Web アプリケーションにサインインする場合に、プライベート クラウド ポータルのユーザー アカウントの資格情報を使用している。

正しい資格情報を使用していることを確認します。

##  <a name="datetime-synchronization-error"></a>日付と時刻の同期エラー

日付と時刻の同期に関するエラー (802) は、サーバー クロックが現在の時刻と同期しておらず、5 分以上ずれている可能性があることを示します。 コレクター VM のクロックの時刻を変更して、現在の時刻と一致させます。

1. VM で管理者用のコマンド プロンプトを開きます。
2. タイム ゾーンを確認するには、**w32tm /tz** を実行します。
3. 時刻を同期するには、**w32tm /resync** を実行します。


## <a name="unabletoconnecttoserver"></a>"UnableToConnectToServer"

この接続エラーを受け取る場合、vCenter Server <*サーバー名*>.com:9443 に接続できない可能性があります。 エラーの詳細では、`https://\*servername*.com:9443/sdk` でリッスンしているエンドポイントで、メッセージを受け入れることができるものがないことが示されています。

- 最新バージョンのアプライアンスを実行しているかどうかを確認します。 そうでない場合は、アプライアンスを[最新バージョン](https://docs.microsoft.com/azure/migrate/concepts-collector)にアップグレードします。
- 最新バージョンでも引き続き問題が発生する場合は、指定された vCenter Server 名をアプライアンスで解決できないか、または指定されたポートが間違っている可能性があります。 ポートが指定されていない場合、コレクターでは既定でポート番号 443 への接続が試みられます。

    1. アプライアンスから <*サーバー名*>.com に対して ping を実行します。
    2. ステップ 1 が失敗する場合は、IP アドレスを使用して vCenter サーバーへの接続を試みます。
    3. vCenter Server に接続するための正しいポート番号を確認します。
    4. vCenter Server が起動されて実行中であることを確認します。


## <a name="error-6005260039-appliance-might-not-be-registered"></a>エラー 60052/60039: アプライアンスが登録されていない可能性がある

- エラー 60052 "The appliance might not be registered successfully to the Azure Migrate project" (アプライアンスが Azure Migrate プロジェクトに正常に登録されていない可能性があります) は、アプライアンスの登録に使用された Azure アカウントに十分な権限がない場合に発生します。
    - アプライアンスの登録に使用する Azure ユーザー アカウントに、少なくともサブスクリプションの共同作成者アクセス許可があることを確認します。
    - 必要な Azure ロールとアクセス許可の[詳細を参照](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)してください。
- エラー 60039 "The appliance might not be registered successfully to the Azure Migrate project" (アプライアンスが Azure Migrate プロジェクトに正常に登録されていない可能性があります) は、アプライアンスの登録に使用された Azure Migrate プロジェクトが見つからない場合に発生する可能性があります。
    - Azure portal で、リソース グループにプロジェクトが存在するかどうかを確認してください。
    - プロジェクトが存在しない場合は、ご利用のリソース グループに新しい Azure Migrate プロジェクトを作成して、アプライアンスをもう一度登録してください。 新しいプロジェクトを作成する[方法を参照](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool)してください。

## <a name="error-6003060031-key-vault-management-operation-failed"></a>エラー 60030/60031: Key Vault の管理操作が失敗した

"Azure Key Vault 管理操作に失敗しました" というエラー 60030 または 60031 を受け取る場合は、次のようにします。
- アプライアンスの登録に使用する Azure ユーザー アカウントに、少なくともサブスクリプションの共同作成者アクセス許可があることを確認します。
- エラー メッセージで指定されているキー コンテナーへのアクセス権がアカウントにあることを確認した後、操作を再試行します。
- 引き続き問題が発生する場合は、Microsoft サポートにお問い合わせください。
- 必要な Azure ロールとアクセス許可の[詳細を参照](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)してください。

## <a name="error-60028-discovery-couldnt-be-initiated"></a>エラー 60028: 検出を開始できなかった

エラー 60028: "Discovery couldn't be initiated because of an error. The operation failed for the specified list of hosts or clusters" (エラーが発生したため、検出を開始できませんでした。指定されたホストまたはクラスターのリストに対する操作に失敗しました) は、VM 情報のアクセスまたは取得で問題が発生したため、エラーで示されているホストで検出を開始できなかったことを示します。 残りのホストは正常に追加されました。

- **[ホストの追加]** オプションを使用して、エラーで示されているホストをもう一度追加します。
- 検証エラーが発生している場合は、修復のガイダンスを確認してエラーを修正し、 **[保存して検出を開始]** オプションをもう一度試します。

## <a name="error-60025-azure-ad-operation-failed"></a>エラー 60025: Azure AD operation failed (Azure AD の操作が失敗しました) 
エラー 60025: "An Azure AD operation failed. The error occurred while creating or updating the Azure AD application" (Azure AD の操作が失敗しました。Azure AD アプリケーションを作成または更新しているときにエラーが発生しました) は、検出を開始するために使用された Azure ユーザー アカウントが、アプライアンスの登録に使用されたアカウントと異なる場合に発生します。 次のいずれかの操作を行います。

- 検出を開始するユーザー アカウントが、アプライアンスの登録に使用するものと同じであることを確認します。
- 検出操作が失敗しているユーザー アカウントに、Azure Active Directory アプリケーションのアクセス許可を提供します。
- Azure Migrate プロジェクト用に以前に作成したリソース グループを削除します。 別のリソース グループを作成して、もう一度開始します。
- Azure Active Directory アプリケーションのアクセス許可に関する[詳細を参照](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)してください。


## <a name="error-50004-cant-connect-to-host-or-cluster"></a>エラー 50004: ホストまたはクラスターに接続できない

エラー 50004: "Can't connect to a host or cluster because the server name can't be resolved. (サーバー名を解決できないため、ホストまたはクラスターに接続できません。) WinRM エラー コード: 0x803381B9" は、アプライアンスの要求を処理する Azure DNS で、指定されたクラスターまたはホストの名前を解決できない場合に発生する可能性があります。

- クラスターでこのエラーが発生する場合は、クラスターの FQDN。
- クラスター内のホストについても、このエラーが表示されることがあります。 これは、アプライアンスはクラスターに接続できますが、クラスターから FQDN ではないホスト名が返されることを示します。 このエラーを解決するには、IP アドレスとホスト名のマッピングを追加することでアプライアンス上の hosts ファイルを更新します。
    1. 管理者としてメモ帳を開きます。
    2. C:\Windows\System32\Drivers\etc\hosts ファイルを開きます。
    3. IP アドレスとホスト名を 1 行で追加します。 このエラーが見られるホストまたはクラスターごとに繰り返します。
    4. hosts ファイルを保存して閉じます。
    5. アプライアンス管理アプリを使用して、アプライアンスからホストに接続できるかどうかを確認します。 30 分後、これらのホストの最新情報が Azure portal に表示されるようになるはずです。

## <a name="discovered-vms-not-in-portal"></a>ポータルで VM が検出されない

検出状態は "Discovery in progress (検出中)" だが、ポータルに VM がまだ表示されない場合は、数分お待ちください。
- VMware VM では約 15 分かかります。
- Hyper-V VM 検出でホストが追加されるごとに、約 2 分かかります。

待っても状態が変わらない場合は、 **[サーバー]** タブで **[最新の情報に更新]** を選択してください。これにより、検出されたサーバーの数が Azure Migrate に表示されます。Server Assessment、Azure Migrate: Server Migration に関するエラーのトラブルシューティングに役立つ情報を提供しています。

これがうまくいかず、VMware サーバーを検出している場合は、次を実行してください。

- 指定した vCenter アカウントに、少なくとも 1 つの VM にアクセスできるアクセス許可が正しく設定されていることを確認してください。
- vCenter アカウントに VM フォルダー レベルでアクセス権が付与されている場合、Azure Migrate は VMware VM を検出できません。 検出のスコープ設定についての[詳細をご覧ください](set-discovery-scope.md)。

## <a name="vm-data-not-in-portal"></a>VM データがポータルにない

検出された VM がポータルに表示されないか、VM データが期限切れになっている場合は、数分待ってください。 検出された VM 構成データの変更内容がポータルに表示されるまでには最大で 30 分かかります。 アプリケーション データの変更が表示されるまでに数時間かかる場合もあります。 この時間が経過してもデータがない場合は、次のように最新の情報に更新してみてください

1. **[サーバー]**  >  **[Azure Migrate Server Assessment]\(Azure Migrate Server Assessment\)** で、 **[概要]** を選択します。
2. **[管理]** の下で、 **[Agent Health]\(エージェントの正常性\)** を選択します。
3. **[エージェントを更新する]** を選択します。
4. 更新操作が完了するまで待ちます。 これで、最新の情報が表示されるはずです。

## <a name="deleted-vms-appear-in-portal"></a>削除した VM がポータルに表示される

VM を削除してもまだポータルに表示されている場合は、30 分待ってください。 まだ表示されている場合は、前述の説明に従って最新の情報に更新してください。

## <a name="error-the-file-uploaded-is-not-in-the-expected-format"></a>エラー:アップロードされたファイルの形式が正しくありません
一部のツールの地域設定によって、セミコロンが区切り記号として使用される CSV ファイルが作成されます。 設定を変更して、区切り記号をコンマにしてください。

## <a name="i-imported-a-csv-but-i-see-discovery-is-in-progress"></a>CSV をインポートしましたが、"検出が進行中です" と表示されます
この状態は、検証の失敗が原因で CSV のアップロードに失敗した場合に表示される可能性があります。 CSV をもう一度インポートしてみてください。 以前のアップロードのエラー レポートをダウンロードし、ファイルの修復ガイダンスに従って、エラーを修正できます。 エラー レポートは、[マシンの検出] ページの [インポートの詳細] セクションからダウンロードできます。

## <a name="do-not-see-application-details-even-after-updating-guest-credentials"></a>ゲストの資格情報を更新した後でもアプリケーションの詳細が表示されない
アプリケーションの検出は 24 時間ごとに実行されます。 詳細をすぐに確認するには、次のように更新します。 検出された VM 数によっては、 この実行に数分かかることがあります。

1. **[サーバー]**  >  **[Azure Migrate Server Assessment]\(Azure Migrate Server Assessment\)** で、 **[概要]** を選択します。
2. **[管理]** の下で、 **[Agent Health]\(エージェントの正常性\)** を選択します。
3. **[エージェントを更新する]** を選択します。
4. 更新操作が完了するまで待ちます。 これで、最新の情報が表示されるはずです。

## <a name="unable-to-export-application-inventory"></a>アプリケーション インベントリのエクスポートができない
ポータルからインベントリをダウンロードするユーザーに、サブスクリプションに対する共同作成者権限があることを確認します。

## <a name="common-app-discovery-errors"></a>一般的なアプリ検出エラー

Azure Migrate は、Azure Migrate を使用してアプリケーション、ロール、および機能の検出をサポートします。Server Assessment を使用して作成する方法について説明します。 現在、アプリ検出は VMware でのみサポートされています。 アプリ検出を設定するための要件と手順についての[詳細をご覧ください](how-to-discover-applications.md)。

一般的なアプリ検出エラーを表にまとめています。 

**Error** | **原因** | **操作**
--- | --- | --- | ---
10000: "Unable to discover the applications installed on the server" (サーバーにインストールされているアプリケーションを検出できません) | これは、マシンのオペレーティング システムが Windows でも Linux でもない場合に発生する可能性があります。 | アプリ検出は、Windows/Linux にのみ使用してください。
10001: "Unable to retrieve the applications installed the server" (サーバーにインストールされているアプリケーションを取得できません) | 内部エラー - アプライアンスでいくつかのファイルが不足しています。 | Microsoft サポートにお問い合わせください。
10002: "Unable to retrieve the applications installed the server" (サーバーにインストールされているアプリケーションを取得できません) | アプライアンス上の検出エージェントが正常に動作していない可能性があります。 | 24 時間以内に問題が自動的に解決しない場合は、サポートにお問い合わせください。
10003: "Unable to retrieve the applications installed the server" (サーバーにインストールされているアプリケーションを取得できません) | アプライアンス上の検出エージェントが正常に動作していない可能性があります。 | 24 時間以内に問題が自動的に解決しない場合は、サポートにお問い合わせください。
10004: "Unable to discover installed applications for <Windows/Linux> machines" (<Windows/Linux> マシンにインストールされているアプリケーションを検出できません) |  <Windows/Linux> マシンにアクセスするための資格情報がアプライアンスで提供されていません。| <Windows/Linux> マシンにアクセスできる資格情報をアプライアンスに追加してください。
10005: "Unable to access the on-premises server" (オンプレミスのサーバーにアクセスできません) | アクセス資格情報が間違っている可能性があります。 | アプライアンスの資格情報を更新し、それを使用して関連するマシンにアクセスできることを確認してください。 
10006: "Unable to access the on-premises server" (オンプレミスのサーバーにアクセスできません) | これは、マシンのオペレーティング システムが Windows でも Linux でもない場合に発生する可能性があります。|  アプリ検出は、Windows/Linux にのみ使用してください。
10007:"Unable to process the metadata retrieved" (取得されたメタデータを処理できません) | この内部エラーは、JSON を逆シリアル化しようとしている間に発生しました | Microsoft サポートに解決方法をお問い合わせください
9000/9001/9002: "Unable to discover the applications installed on the server" (サーバーにインストールされているアプリケーションを検出できません) | VMware ツールがインストールされていないか、破損している可能性があります。 | 関連するマシンに VMware ツールをインストール/再インストールし、実行されていることを確認します。
9003: "Unable to discover the applications installed on the server" (サーバーにインストールされているアプリケーションを検出できません) | これは、マシンのオペレーティング システムが Windows でも Linux でもない場合に発生する可能性があります。 | アプリ検出は、Windows/Linux にのみ使用してください。
9004: "Unable to discover the applications installed on the server" (サーバーにインストールされているアプリケーションを検出できません) | これは、VM の電源がオフになっている場合に発生する可能性があります。 | 検出を行うには、VM がオンになっていることを確認します。
9005: "Unable to discover the applications installed on the VM" (VM にインストールされているアプリケーションを検出できません) | これは、マシンのオペレーティング システムが Windows でも Linux でもない場合に発生する可能性があります。 | アプリ検出は、Windows/Linux にのみ使用してください。
9006/9007: "Unable to retrieve the applications installed the server" (サーバーにインストールされているアプリケーションを取得できません) | アプライアンス上の検出エージェントが正常に動作していない可能性があります。 | 24 時間以内に問題が自動的に解決しない場合は、サポートにお問い合わせください
9008: "Unable to retrieve the applications installed the server". (サーバーにインストールされているアプリケーションを取得できません。) | 内部エラーの可能性があります。  | 24 時間以内に問題が自動的に解決しない場合は、サポートにお問い合わせください。
9009: "Unable to retrieve the applications installed the server" (サーバーにインストールされているアプリケーションを取得できません) | サーバーの Windows ユーザー アカウント制御 (UAC) の設定の制限により、インストールされているアプリケーションの検出が妨げられている場合に発生します。 | サーバーで [ユーザーアカウント制御] の設定を探し、サーバーの UAC のレベル設定が下位の 2 つのいずれかになるように構成します。
9010: "VM の電源がオフになっています" | VM の電源がオフになっています。  | VM の電源がオンか確認します。
9011:"File to download from guest is not found on the guest VM" (ゲストからダウンロードするファイルがゲスト VM に見つかりません) | この問題は、内部エラーのために発生します。 | この問題は、24 時間以内に自動的に解決されます。 問題が解決しない場合は、Microsoft サポートにお問い合わせください。
9012:"Result file contents are empty" (結果ファイルの内容が空です) | この問題は、内部エラーのために発生します。 | この問題は、24 時間以内に自動的に解決されます。 問題が解決しない場合は、Microsoft サポートにお問い合わせください。
9013:"A new temporary profile is created for every login to the VMware VM" (VMware VM へのログインごとに新しい一時プロファイルが作成されます) | VM へのログインごとに新しい一時プロファイルが作成されます | ゲスト VM の資格情報で指定されたユーザー名が UPN 形式であることを確認してください。
9014:"Unable to retrieve metadata from guest VM file system" (ゲスト VM ファイル システムからメタデータを取得できません) | ESXi ホストへの接続で問題が発生しました | VM を実行している ESXi ホスト上のポート 443 にアプライアンスが接続できることを確認します
9015:"Unable to connect to VMware VMs due to insufficient privileges on vCenter" (vCenter の特権が不足しているため VMware VM に接続できません) | vCenter ユーザー アカウントでゲスト操作ロールが有効になっていません | vCenter ユーザー アカウントでゲスト操作ロールが有効になっていることを確認してください。
9016:"Unable to connect to VMware VMs as the guest operations agent is out of data" (ゲスト操作エージェントのデータが不足しているため VMware VM に接続できません) | VMware ツールが正しくインストールされていないか、最新の状態ではありません。 | VMware ツールが正しくインストールされ、最新の状態になっていることを確認してください。
9017:"File with discovered metadata is not found on the VM" (検出されたメタデータを含むファイルが VM 上に見つかりません) | この問題は、内部エラーのために発生します。 | Microsoft サポートに解決方法をお問い合わせください。
9018:"PowerShell is not installed in the Guest VMs" (PowerShell がゲスト VM にインストールされていません) | PowerShell がゲスト VM で利用できません。 | ゲスト VM に PowerShell をインストールしてください。
9019:"Unable to discover due to guest VM operation failures" (ゲスト VM 操作エラーのため、検出できませんでした) | VM で VMware ゲスト操作が失敗しました。 | VM の資格情報が有効であること、およびゲスト VM の資格情報で指定されたユーザー名が UPN 形式であることを確認してください。
9020:"File creation permission is denied" (ファイル作成のアクセス許可が拒否されました) | ユーザーまたはグループ ポリシーに関連付けられているロールにより、ユーザーがフォルダー内にファイルを作成することが制限されています | 指定されたゲスト ユーザーが、フォルダー内のファイルの作成アクセス許可を持っているかどうかを確認してください。 フォルダーの名前については、Server Assessment の **[Notifications]\(通知\)** を参照してください。
9021:"File create permission is denied in folder System Temp Path" (System Temp Path フォルダーのファイル作成アクセス許可が拒否されました) | VM 上の VMware ツール バージョンがサポートされていません | VMware ツール バージョンを 10.2.0 より上にアップグレードしてください。
9022:"Get WMI object access is denied" (WMI オブジェクトの取得アクセスが拒否されました) | ユーザーまたはグループ ポリシーに関連付けられているロールにより、ユーザーが WMI オブジェクトにアクセスすることが制限されています。 | Microsoft サポートにお問い合わせください。
9023:"SystemRoot environment variable value is empty" (SystemRoot 環境変数の値が空です) | 不明 | Microsoft サポートにお問い合わせください。
9024:"TEMP environment variable value is empty" (TEMP 環境変数の値が空です) | 不明 | Microsoft サポートにお問い合わせください。
9025:"PowerShell is corrupt in the Guest VMs" (PowerShell がゲスト VM で破損しています) | 不明 | PowerShell をゲスト VM に再インストールし、ゲスト VM で PowerShell を実行できるかどうかを確認してください。
8084: "Unable to discover applications due to VMware error: <Exception from VMware>" (次の VMware エラーのため、アプリケーションを検出できません:) | Azure Migrate アプライアンスは、アプリケーションの検出に VMware API を使用しています。 この問題は、アプリケーションを検出しようとしている時に vCenter Server から例外がスローされた場合に発生することがあります。 VMware からのエラー メッセージは、ポータルに表示されるエラー メッセージに表示されます。 | [VMware ドキュメント](https://pubs.vmware.com/vsphere-51/topic/com.vmware.wssdk.apiref.doc/index-faults.html)でメッセージを検索し、修正手順に従ってください。 修正できない場合は、Microsoft サポートにお問い合わせください。



## <a name="next-steps"></a>次のステップ
[VMware](how-to-set-up-appliance-vmware.md)、[Hyper-V](how-to-set-up-appliance-hyper-v.md)、または[物理サーバー](how-to-set-up-appliance-physical.md)用にアプライアンスを設定します。

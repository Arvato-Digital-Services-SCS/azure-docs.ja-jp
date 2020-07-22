---
title: Windows Server ノード プールの制限
titleSuffix: Azure Kubernetes Service
description: Windows Server ノード プールとアプリケーション ワークロードを Azure Kubernetes Service (AKS) 内で実行するときの既知の制限事項について説明します
services: container-service
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: c420eb850313900d3726b93dd97f911a428d3560
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85339879"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) での Windows Server ノード プールとアプリケーション ワークロードについての現在の制限事項

Azure Kubernetes Service (AKS) では、Windows Server をゲスト OS としてノード上で実行するノード プールを作成することができます。 これらのノードは、.NET Framework 上に構築されたものなど、ネイティブの Windows コンテナー アプリケーションを実行できます。 Linux と Windows OS がコンテナー サポートを提供する方法に大きな相違点があるため、いくつかの一般的な Kubernetes と Pod 関連の機能は Windows ノード プールで現在使用できません。

この記事では、AKS 内の Windows Server ノードの制限事項および OS の概念について説明します。

## <a name="which-windows-operating-systems-are-supported"></a>どの Windows オペレーティング システムがサポートされていますか?

AKS では、ホスト OS のバージョンとして Windows Server 2019 が使用され、プロセス分離のみがサポートされます。 他のバージョンの Windows Server を使用してビルドされたコンテナー イメージはサポートされていません。 「[Windows コンテナー バージョンの互換性][windows-container-compat]」を参照してください。

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Windows と Linux では Kubernetes に違いはありますか?

Window Server ノード プールのサポートには、Kubernetes プロジェクトの上流 Windows Server の一部であるいくつかの制限が含まれます。 これらの制限は、AKS 固有ではありません。 Kubernetes での Windows Server のこのアップストリーム サポートの詳細については、Kubernetes プロジェクトの「[Kubernetes での Windows のサポートの概要][intro-windows]」ドキュメントの「[サポートされている機能と制限事項][upstream-limitations]」を参照してください。

Kubernetes は従来より Linux が中心となっています。 アップストリームの [Kubernetes.io][kubernetes] Web サイトで使用されている多くの例は、Linux ノードでの使用を意図したものです。 Windows Server コンテナーを使用したデプロイを作成する場合、OS レベルの次の考慮事項が該当します。

- **ID** - Linux では、整数のユーザー識別子 (UID) によってユーザーが識別されます。 ユーザーは、ログオンに使用する英数字のユーザー名も持っています。これは、Linux によって、ユーザーの UID に変換されます。 同様に、Linux では、整数のグループ識別子 (GID) によってユーザー グループが識別され、グループ名が対応する GID に変換されます。
    - Windows Server では、Windows Security Access Manager (SAM) データベースに格納されている大きいバイナリ セキュリティ識別子 (SID) を使用します。 このデータベースは、ホストとコンテナー、またはコンテナー間で共有されません。
- **ファイルのアクセス許可** - Windows Server では、アクセス許可および UID と GID のビットマスクではなく、SID に基づくアクセス制御リストを使用します
- **ファイル パス** - Windows Server の規則では、/ の代わりに \ が使用されます。
    - ボリュームをマウントするポッド仕様では、Windows Server コンテナー用にパスを正しく指定してください。 たとえば、Linux コンテナーの */mnt/volume* というマウント ポイントではなく、*K:* ドライブとしてマウントするために、 */K/Volume* のようにドライブ文字と場所を指定してください。

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Windows ではどのような種類のディスクがサポートされていますか?

Azure Disk と Azure Files はサポートされているボリュームの種類であり、Windows Server コンテナー内で NTFS ボリュームとしてアクセスされます。

## <a name="can-i-run-windows-only-clusters-in-aks"></a>Windows のみのクラスターを AKS で実行できますか?

AKS クラスターのマスター ノード (コントロール プレーン) は、AKS サービスによってホストされています。マスター コンポーネントがホストされているノードのオペレーティング システムはユーザーには公開されていません。 すべての AKS クラスターは既定の最初のノード プールで作成され、これは Linux ベースです。 このノード プールには、クラスターが機能するために必要なシステム サービスが含まれています。 クラスターの信頼性を確保し、クラスター操作を実行できるようにするため、最初のノード プールで少なくとも 2 つのノードを実行することをお勧めします。 最初の Linux ベースのノード プールは、AKS クラスター自体を削除しない限り削除できません。

## <a name="what-network-plug-ins-are-supported"></a>どのネットワーク プラグインがサポートされていますか?

Windows ノード プールの AKS クラスターでは、Azure CNI (高度) ネットワーク モデルを使用する必要があります。 Kubenet (基本) ネットワークはサポートされていません。 ネットワーク モデルの違いの詳細については、[AKS のアプリケーションにおけるネットワークの概念][azure-network-models]に関する記事を参照してください。 - Azure CNI ネットワーク モデルでは、IP アドレス管理に関する追加の計画と考慮事項が必要です。 Azure CNI を計画して実装する方法の詳細については、[AKS での Azure CNI ネットワークの構成][configure-azure-cni]に関するページを参照してください。

## <a name="can-i-change-the-max--of-pods-per-node"></a>ノードあたりのポッドの最大数を変更できますか?

はい。 影響と使用可能なオプションについては、[ポッドの最大数][maximum-number-of-pods]に関するページを参照してください。

## <a name="how-do-patch-my-windows-nodes"></a>Windows ノードに修正プログラムを適用するにはどうすればいいですか?

最新の修正プログラムと更新プログラムを取得するには、AKS の Windows Server ノードを "*アップグレード*" する必要があります。 AKS のノードでは Windows Update は有効になっていません。 AKS では、修正プログラムが利用可能になるとすぐに新しいノード プール イメージがリリースされます。更新プログラムや修正プログラムを最新の状態に維持するために、ノード プールをアップグレードするのは、お客様の責任です。 これは、使用されている Kubernetes のバージョンにも当てはまります。 新しいバージョンが利用可能になると、AKS のリリース ノートで示されます。 Windows Server ノード プールのアップグレードの詳細については、[AKS でのノード プールのアップグレード][nodepool-upgrade]に関するページを参照してください。

> [!NOTE]
> 更新された Windows Server イメージは、ノード プールをアップグレードする前にクラスターのアップグレード (コントロール プレーンのアップグレード) が実行された場合にのみ使用されます。
>

## <a name="why-am-i-seeing-an-error-when-i-try-to-create-a-new-windows-agent-pool"></a>新しい Windows エージェント プールを作成しようとしたときにエラーが表示されるのはなぜですか。

2020 年 2 月より前にクラスターを作成し、クラスターのアップグレード操作が行われていない場合、クラスターは引き続き古い Windows イメージを使用します。 次のようなエラーが表示されている可能性があります。

"デプロイ テンプレートから参照されている次のイメージの一覧が見つかりません:発行元: MicrosoftWindowsServer、オファー:WindowsServer、Sku:2019-datacenter-core-smalldisk-2004、バージョン: 最新。 利用可能なイメージの検索方法については、 https://docs.microsoft.com/azure/virtual-machines/windows/cli-ps-findimage をご覧ください。"

この問題を解決するには:

1. [クラスター コントロール プレーン][upgrade-cluster-cp]をアップグレードします。 これにより、イメージのオファーと発行元が更新されます。
1. 新しい Windows エージェント プールを作成します。
1. Windows ポッドを既存の Windows エージェント プールから新しい Windows エージェント プールに移動します。
1. 古い Windows エージェント プールを削除します。

## <a name="how-do-i-rotate-the-service-principal-for-my-windows-node-pool"></a>Windows ノード プールのサービス プリンシパルはどのようにローテーションするのですか?

Windows ノード プールは、サービス プリンシパルのローテーションをサポートしていません。 サービス プリンシパルを更新するには、新しい Windows ノード プールを作成し、ポッドを古いプールから新しいプールに移行します。 この処理が完了したら、古いノード プールを削除します。

## <a name="how-many-node-pools-can-i-create"></a>ノード プールはいくつ作成できますか?

AKS クラスターでは、最大で 10 のノード プールを作成できます。 それらのノード プール全体で最大 1,000 個のノードを使用できます。 [ノード プールの制限][nodepool-limitations]に関するページを参照してください。

## <a name="what-can-i-name-my-windows-node-pools"></a>Windows ノード プールにはどのような名前を指定できますか?

名前は 6 文字以下にする必要があります。 これは AKS の現在の制限です。

## <a name="are-all-features-supported-with-windows-nodes"></a>Windows ノードではすべての機能がサポートされていますか?

現在、Windows ノードでは、ネットワーク ポリシーと kubernet はサポートされていません。

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Windows ノードでイングレス コントローラーを実行できますか?

はい。Windows Server コンテナーがサポートされているイングレス コントローラーは、AKS の Windows ノードで実行できます。

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Windows ノードで Azure Dev Spaces を使用できますか?

Azure Dev Spaces は現在、Linux ベースのノード プールに対してのみ使用できます。

## <a name="can-my-windows-server-containers-use-gmsa"></a>Windows Server コンテナーで gMSA を使用できますか?

グループの管理されたサービス アカウント (gMSA) のサポートは、現在 AKS では使用できません。

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Windows ノードとコンテナーを含むコンテナーには Azure Monitor を使用できますか?

はい。ただし、Windows コンテナーからログ (stdout、stderr) とメトリックを収集するための Azure Monitor はパブリック プレビュー段階です。 また、Windows コンテナーから stdout ログのライブ ストリームにアタッチできます。

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>サポートされていない機能が必要な場合はどうすればよいですか?

AKS での Windows に必要なすべての機能を組み込む作業が行われていますが、ギャップに気付かれた場合は、オープンソースのアップストリーム [aks-engine][aks-engine] プロジェクトで、Azure で Kubernetes を実行する簡単で完全にカスタマイズ可能な方法が提供されており、Windows のサポートが含まれます。 [AKS のロードマップ][aks-roadmap]に関するページで今後の機能のロードマップを確認してください。

## <a name="next-steps"></a>次のステップ

AKS で Windows Server コンテナーの使用を開始するには、[AKS で Windows Server を実行するノード プールを作成][windows-node-cli]します。

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[upgrade-cluster]: upgrade-cluster.md
[upgrade-cluster-cp]: use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[windows-container-compat]: /virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-2019%2Cwindows-10-1909
[maximum-number-of-pods]: configure-azure-cni.md#maximum-pods-per-node
[azure-monitor]: ../azure-monitor/insights/container-insights-overview.md#what-does-azure-monitor-for-containers-provide

---
title: コンテナーに対する Azure Monitor エージェントを管理する方法 | Microsoft Docs
description: この記事では、コンテナーに対する Azure Monitor によって使用されるコンテナー化された Log Analytics エージェントで、最も一般的なメンテナンス タスクを管理する方法について説明します。
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: fc5bc0d60cb4ef1e375a997cbb3fe4bd2aed3235
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2020
ms.locfileid: "86107412"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>コンテナーに対する Azure Monitor エージェントを管理する方法

コンテナーに対する Azure Monitor では、コンテナー化されたバージョンの Linux 用 Log Analytics エージェント が使用されます。 初期のデプロイ後は、ライフ サイクル中に実行する必要のあるルーチンまたは省略可能なタスクが存在します。 この記事では、エージェントを手動でアップグレードし、特定のコンテナーから環境変数のコレクションを無効にする方法について詳しく説明します。 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>コンテナーに対する Azure Monitor エージェントをアップグレードする方法

コンテナーに対する Azure Monitor では、コンテナー化されたバージョンの Linux 用 Log Analytics エージェント が使用されます。 エージェントの新しいバージョンがリリースされると、Azure Kubernetes Service (AKS) および Azure Red Hat OpenShift バージョン 3.x でホストされているマネージド Kubernetes クラスター上のエージェントが自動的にアップグレードされます。 [hybrid Kubernetes クラスター](container-insights-hybrid-setup.md)と Azure Red Hat OpenShift バージョン 4.x のエージェントは管理されていないため、エージェントを手動でアップグレードする必要があります。

AKS または Azure Red Hat OpenShift バージョン 3.x でホストされているクラスターのエージェントのアップグレードに失敗している場合、この記事では、エージェントを手動でアップグレードするプロセスについても説明します。 リリースされたバージョンを確認するには、[エージェントのリリースのお知らせ](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)を参照してください。

### <a name="upgrade-agent-on-aks-cluster"></a>AKS クラスター上のエージェントのアップグレード

AKS クラスター上のエージェントをアップグレードするプロセスは、2 つの単純な手順で構成されます。 最初の手順は、コンテナーに対する Azure Monitor の監視を Azure CLI を使用して無効にすることです。 [監視の無効化](container-insights-optout.md?#azure-cli)に関する記事の手順に従ってください。 Azure CLI を使用して、ソリューションとワークスペースに格納されている対応するデータに影響を与えることなく、クラスター内のノードからエージェントを削除できます。 

>[!NOTE]
>このメンテナンス アクティビティを実行している間、クラスター内のノードによる収集されたデータの転送は行われず、エージェントを削除して新しいバージョンをインストールするまでの間、パフォーマンス ビューにデータは表示されません。 
>

新しいバージョンのエージェントをインストールするには、[Azure CLI を使用して監視を有効にする](container-insights-enable-new-cluster.md#enable-using-azure-cli)に関する記事の手順に従って、このプロセスを完了します。  

監視を再び有効にした後、クラスターの更新された正常性メトリックが表示されるまで、約 15 分かかる場合があります。 エージェントが正常にアップグレードされたことを確認するには、`kubectl logs omsagent-484hw --namespace=kube-system` コマンドを実行します

ステータスは次の例のようになります。ここで、*omi* と *omsagent* の値は、[エージェントのリリース履歴](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)で指定された最新バージョンと一致する必要があります。  

```console
User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
:
:
instance of Container_HostInventory
{
    [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
    Computer=aks-nodepool1-39773055-0
    DockerVersion=1.13.1
    OperatingSystem=Ubuntu 16.04.3 LTS
    Volume=local
    Network=bridge host macvlan null overlay
    NodeRole=Not Orchestrated
    OrchestratorType=Kubernetes
}
Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc
Status: Onboarded(OMSAgent Running)
omi 1.4.2.5
omsagent 1.6.0-163
docker-cimprov 1.0.0.31
```

### <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>ハイブリッド Kubernetes クラスター上でエージェントをアップグレードする

次の手順を実行して、以下の場所で実行されている Kubernetes クラスター上のエージェントをアップグレードします。

* AKS エンジンを使用して、Azure でホストされた自己管理の Kubernetes クラスター。
* AKS エンジンを使用して、Azure Stack またはオンプレミスでホストされた自己管理の Kubernetes クラスター。
* Red Hat OpenShift バージョン 4.x。

Log Analytics ワークスペースが商用 Azure にある場合には、次のコマンドを実行します。

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Log Analytics ワークスペースが Azure China 21Vianet にある場合には、次のコマンドを実行します。

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Log Analytics ワークスペースが Azure US Government にある場合には、次のコマンドを実行します。

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-red-hat-openshift-v4"></a>Azure Red Hat OpenShift v4 上のエージェントのアップグレード

次の手順を実行して、Azure Red Hat OpenShift バージョン 4.x で実行されている Kubernetes クラスター上でエージェントをアップグレードします。 

>[!NOTE]
>Azure Red Hat OpenShift バージョン 4.x は、Azure 商用クラウドでの実行のみをサポートしています。
>

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<azureAroV4ResourceId> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-arc-enabled-kubernetes"></a>Azure Arc 対応 Kubernetes 上のエージェントのアップグレード

次のコマンドを実行して、プロキシ エンドポイントを使用せずに Azure Arc 対応 Kubernetes クラスター上のエージェントをアップグレードします。

```console
$ helm upgrade --install azmon-containers-release-1  –set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<resourceIdOfAzureArcK8sCluster>
```

プロキシ エンドポイントが指定されている場合は、次のコマンドを実行してエージェントをアップグレードします。 プロキシ エンドポイントの詳細については、[プロキシ エンドポイントの構成](container-insights-enable-arc-enabled-clusters.md#configure-proxy-endpoint)に関するページを参照してください。

```console
$ helm upgrade –name azmon-containers-release-1 –set omsagent.proxy=<proxyEndpoint>,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<resourceIdOfAzureArcK8sCluster>
```

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>コンテナーの環境変数コレクションを無効にする方法

コンテナーに対する Azure Monitor は、ポッドで実行されているコンテナーから環境変数を収集し、 **[コンテナー]** ビューで選択したコンテナーのプロパティ ウィンドウに表示します。 この動作を制御するには、Kubernetes クラスターのデプロイ中、またはデプロイ後に環境変数 *AZMON_COLLECT_ENV* を設定して、特定のコンテナーのコレクションを無効にします。 この機能は、エージェント バージョン ciprod11292018 以降で使用できます。  

新規または既存のコンテナーの環境変数のコレクションを無効にするには、Kubernetes デプロイ yaml 構成ファイルで変数 **AZMON_COLLECT_ENV** を値 **False** に設定します。 

```yaml
- name: AZMON_COLLECT_ENV  
  value: "False"  
```

`kubectl apply -f  <path to yaml file>` コマンドを実行して、Azure Red Hat OpenShift 以外の Kubernetes クラスターに変更を適用します。 ConfigMap を編集し、この変更を Azure Red Hat OpenShift クラスターに適用するには、次のコマンドを実行します。

```bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

これにより、既定のテキスト エディターが開きます。 変数を設定したら、ファイルをエディターに保存します。

構成の変更が有効になったことを確認するには、コンテナーに対する Azure Monitor の **[コンテナー]** ビューでコンテナーを選択し、プロパティ ウィンドウで **[環境変数]** を展開します。  このセクションには、先ほど作成した変数 - **AZMON_COLLECT_ENV = FALSE** のみが表示されます。 その他のすべてのコンテナーでは、[環境変数] セクションには、検出されたすべての環境変数がリストされます。

環境変数の検出を再度有効にするには、先ほどと同じプロセスを適用し、値を **False** から **True** に変更して、`kubectl` コマンドを再実行してコンテナーを更新します。  

```yaml
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>次のステップ

エージェントのアップグレード中に問題が発生した場合は、[トラブルシューティング ガイド](container-insights-troubleshoot.md)を参照してください。

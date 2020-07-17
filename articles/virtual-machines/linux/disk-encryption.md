---
title: Azure Managed Disks のサーバー側暗号化 - Azure CLI
description: Azure Storage では、保存時に暗号化してデータを保護してから、ストレージ クラスターに保存します。 マネージド ディスクの暗号化には Microsoft のマネージド キーを使用できます。また、カスタマー マネージド キーを使用し、独自のキーを使って暗号化を管理できます。
author: roygara
ms.date: 04/21/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.openlocfilehash: a77f40f554e459ad1f28b11969421689cd29b4bb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85609387"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Azure Managed Disks のサーバー側暗号化

Azure マネージド ディスクは、データをクラウドに永続化するときに、既定で自動的にデータを暗号化します。 サーバー側暗号化 (SSE) によってデータが保護され、組織のセキュリティおよびコンプライアンス コミットメントを満たすのに役立ちます。 

Azure マネージド ディスク内のデータは、利用できる最も強力なブロック暗号の 1 つである 256 ビット [AES 暗号化](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)を使って透過的に暗号化され、FIPS 140-2 に準拠しています。 Azure マネージド ディスクの基になっている暗号化モジュールについて詳しくは、「[暗号化 API:次世代](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)」を参照してください。

暗号化は、マネージド ディスクのパフォーマンスには影響しません。また、暗号化に追加のコストはかかりません。 

> [!NOTE]
> 一時ディスクはマネージド ディスクではなく、SSE によって暗号化されません。一時ディスクの詳細については、[マネージド ディスクの概要に関するページの「ディスク ロール」](managed-disks-overview.md#disk-roles)セクションを参照してください。

## <a name="about-encryption-key-management"></a>暗号化キーの管理について

プラットフォーム マネージド キーを利用してお使いのマネージド ディスクを暗号化することも、お使いの独自のキーを使用して暗号化を管理することもできます。 独自のキーを使用して暗号化を管理する場合は、マネージド ディスク内のすべてのデータの暗号化と暗号化解除に使用する*カスタマー マネージド キー*を指定できます。 

以降のセクションでは、キー管理の各オプションについてさらに詳しく説明します。

## <a name="platform-managed-keys"></a>プラットフォーム マネージド キー

既定では、マネージド ディスクはプラットフォーム マネージド暗号化キーを使用します。 2017 年 6 月 10 日をもって、新しいすべてのマネージド ディスク、スナップショット、イメージ、および既存のマネージド ディスクに書き込まれる新しいデータは、プラットフォーム マネージド キーで保存時に自動的に暗号化されます。

## <a name="customer-managed-keys"></a>カスタマー マネージド キー

ユーザー独自のキーを使用して、各マネージド ディスクのレベルで暗号化を管理できます。 カスタマー マネージド キーを使用したサーバー側でのマネージド ディスクの暗号化により、Azure Key Vault との統合されたエクスペリエンスが提供されます。 [ご使用の RSA キー](../../key-vault/keys/hsm-protected-keys.md)を Key Vault にインポートするか、Azure Key Vault で新しい RSA キーを生成することができます。 

Azure マネージド ディスクは[エンベロープ暗号化](../../storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique)を使用して、暗号化と暗号化解除を完全に透過的な方法で処理します。 これは、[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256 ベースのデータ暗号化キー (DEK) を使用してデータを暗号化します。このキーは、ご使用のキーを使用して保護されます。 ストレージ サービスは、データ暗号化キーを生成し、RSA 暗号化を使用してカスタマー マネージド キーで暗号化します。 エンベロープ暗号化を使用すると、VM に影響を与えることなく、コンプライアンス ポリシーに従って定期的にキーをローテーション (変更) することができます。 キーを交換すると、ストレージ サービスによって、新しいカスタマー マネージド キーを使用してデータ暗号化キーが再暗号化されます。 

DEK の暗号化と暗号化解除にキーを使用するための、Key Vault 内のマネージド ディスクへのアクセス権の付与を行う必要があります。 これにより、データとキーを完全に制御できます。 任意の時点で、キーを無効にしたり、マネージド ディスクへのアクセスを取り消したりすることができます。 また、Azure Key Vault 監視で暗号化キーの使用状況を監査して、マネージド ディスクまたは他の信頼された Azure サービスのみがキーにアクセスしていることを確認することもできます。

Premium SSD、Standard SSD、Standard HDD の場合:キーを無効にするか削除すると、そのキーを使用しているディスクを持つ VM はすべて自動的にシャットダウンされます。 この後、キーが再度有効になるか、新しいキーを割り当てない限り、VM は使用できません。

Ultra ディスクの場合、キーを無効にしたり削除したりしても、そのキーを使用している Ultra ディスクを持つ VM は自動的にシャットダウンされません。 VM の割り当てを解除して再起動すると、ディスクはキーの使用を停止し、VM はオンラインに戻らなくなります。 VM をオンラインに戻すには、新しいキーを割り当てるか、既存のキーを有効にする必要があります。

次の図は、マネージド ディスクで Azure Active Directory と Azure Key Vault を使用して、カスタマー マネージド キーを使って要求を行う方法を示しています。

![マネージド ディスクとカスタマー マネージド キーのワークフロー。 管理者は Azure Key Vault を作成し、ディスク暗号化セットを作成してから、ディスク暗号化セットを設定します。 このセットは VM に関連付けられています。これにより、ディスクは Azure AD を使用して認証を行うことができます。](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


次の一覧では、図についてさらに詳しく説明します。

1. Azure Key Vault の管理者が Key Vault リソースを作成します。
1. Key Vault 管理者は、RSA キーを Key Vault にインポートするか、Key Vault 内で新しい RSA キーを生成します。
1. この管理者は、Azure Key Vault ID とキー URL を指定して、ディスク暗号化セット リソースのインスタンスを作成します。 ディスク暗号化セットは、マネージド ディスクのキー管理を簡略化するために導入された新しいリソースです。 
1. ディスク暗号化セットを作成すると、[システムによって割り当てられたマネージド ID](../../active-directory/managed-identities-azure-resources/overview.md) が Azure Active Directory (AD) に作成され、ディスク暗号化セットに関連付けられます。 
1. Azure Key Vault の管理者はその後、Key Vault 内での操作を実行するためのアクセス許可をマネージド ID に付与します。
1. VM ユーザーは、ディスク暗号化セットにディスクを関連付けることによってディスクを作成します。 また、VM ユーザーは、既存のリソースに対するカスタマー マネージド キーを使用したサーバー側の暗号化を、ディスク暗号化セットへのリソースの関連付けによって有効にすることもできます。 
1. マネージド ディスクは、マネージド ID を使用して Azure Key Vault に要求を送信します。
1. データの読み取りまたは書き込みを行うために、マネージド ディスクは Azure Key Vault に要求を送信して、データの暗号化と暗号化解除を実行するためにデータ暗号化キーを暗号化 (ラップ) および暗号化解除 (ラップ解除) します。 

カスタマー マネージド キーへのアクセスを取り消すには、[Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) および [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault) に関する記事をご覧ください。 アクセスを取り消すと、Azure Storage が暗号化キーにアクセスできなくなるため、ストレージ アカウント内の全データへのアクセスが事実上ブロックされます。

### <a name="supported-regions"></a>サポートされているリージョン

[!INCLUDE [virtual-machines-disks-encryption-regions](../../../includes/virtual-machines-disks-encryption-regions.md)]

### <a name="restrictions"></a>制限

現在、カスタマー マネージド キーには次の制限があります。

- この機能がディスクで有効になっている場合、無効にすることはできません。
    これを回避する必要がある場合は、カスタマー マネージド キーを使用していないまったく別のマネージド ディスクに[すべてのデータをコピー](disks-upload-vhd-to-managed-disk-cli.md#copy-a-managed-disk)する必要があります。
- サイズ 2048 の [ソフトウェアと HSM の RSA キー](../../key-vault/keys/about-keys.md)のみがサポートされており、その他のキーまたはサイズはサポートされていません。
- サーバー側の暗号化とカスタマー マネージド キーを使用して暗号化されたカスタム イメージから作成されたディスクは、同じカスタマー マネージド キーを使用して暗号化する必要があり、同じサブスクリプション内に存在する必要があります。
- サーバー側の暗号化とカスタマー マネージド キーで暗号化されたディスクから作成されたスナップショットは、同じカスタマー マネージド キーを使用して暗号化する必要があります。
- お使いのカスタマー マネージド キー (Azure Key Vault、ディスク暗号化セット、VM、ディスク、およびスナップショット) に関連するすべてのリソースは、同じサブスクリプションとリージョンに存在する必要があります。
- カスタマー マネージド キーで暗号化されたディスク、スナップショット、およびイメージは、別のサブスクリプションに移動できません。
- お客様が管理するキーを使用して暗号化されたマネージド ディスクを Azure Disk Encryption で暗号化することもできません。
- カスタマー マネージド キーを共有イメージ ギャラリーで使用する方法の詳細については、「[プレビュー:イメージの暗号化にカスタマー マネージド キーを使用する](../image-version-encryption.md)」を参照してください。

### <a name="cli"></a>CLI
#### <a name="setting-up-your-azure-key-vault-and-diskencryptionset"></a>Azure Key Vault と DiskEncryptionSet の設定

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault](../../../includes/virtual-machines-disks-encryption-create-key-vault-cli.md)]

#### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Marketplace イメージを使用して VM を作成し、カスタマー マネージド キーで OS とデータ ディスクを暗号化する

```azurecli
rgName=yourResourceGroupName
vmName=yourVMName
location=westcentralus
vmSize=Standard_DS3_V2
image=UbuntuLTS 
diskEncryptionSetName=yourDiskencryptionSetName

diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)

az vm create -g $rgName -n $vmName -l $location --image $image --size $vmSize --generate-ssh-keys --os-disk-encryption-set $diskEncryptionSetId --data-disk-sizes-gb 128 128 --data-disk-encryption-sets $diskEncryptionSetId $diskEncryptionSetId
```


#### <a name="encrypt-existing-managed-disks"></a>既存のマネージド ディスクを暗号化する 

既存のディスクは、次のスクリプトを使用して暗号化するために、実行中の VM に接続することはできません。

```azurecli
rgName=yourResourceGroupName
diskName=yourDiskName
diskEncryptionSetName=yourDiskEncryptionSetName
 
az disk update -n $diskName -g $rgName --encryption-type EncryptionAtRestWithCustomerKey --disk-encryption-set $diskEncryptionSetName
```

#### <a name="create-a-virtual-machine-scale-set-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Marketplace イメージを使用して仮想マシン スケール セットを作成し、カスタマー マネージド キーで OS とデータ ディスクを暗号化する

```azurecli
rgName=yourResourceGroupName
vmssName=yourVMSSName
location=westcentralus
vmSize=Standard_DS3_V2
image=UbuntuLTS 
diskEncryptionSetName=yourDiskencryptionSetName

diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)
az vmss create -g $rgName -n $vmssName --image UbuntuLTS --upgrade-policy automatic --admin-username azureuser --generate-ssh-keys --os-disk-encryption-set $diskEncryptionSetId --data-disk-sizes-gb 64 128 --data-disk-encryption-sets $diskEncryptionSetId $diskEncryptionSetId
```

#### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>カスタマー マネージド キーを使用したサーバー側の暗号化で暗号化された空のディスクを作成し、VM に接続する

```azurecli
vmName=yourVMName
rgName=yourResourceGroupName
diskName=yourDiskName
diskSkuName=Premium_LRS
diskSizeinGiB=30
location=westcentralus
diskLUN=2
diskEncryptionSetName=yourDiskEncryptionSetName


diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)

az disk create -n $diskName -g $rgName -l $location --encryption-type EncryptionAtRestWithCustomerKey --disk-encryption-set $diskEncryptionSetId --size-gb $diskSizeinGiB --sku $diskSkuName

diskId=$(az disk show -n $diskName -g $rgName --query [id] -o tsv)

az vm disk attach --vm-name $vmName --lun $diskLUN --ids $diskId 

```

#### <a name="change-the-key-of-a-diskencryptionset-to-rotate-the-key-for-all-the-resources-referencing-the-diskencryptionset"></a>DiskEncryptionSet のキーを変更して、DiskEncryptionSet を参照しているすべてのリソースのキーをローテーションする

```azurecli

rgName=yourResourceGroupName
keyVaultName=yourKeyVaultName
keyName=yourKeyName
diskEncryptionSetName=yourDiskEncryptionSetName


keyVaultId=$(az keyvault show --name $keyVaultName--query [id] -o tsv)

keyVaultKeyUrl=$(az keyvault key show --vault-name $keyVaultName --name $keyName --query [key.kid] -o tsv)

az disk-encryption-set update -n keyrotationdes -g keyrotationtesting --key-url $keyVaultKeyUrl --source-vault $keyVaultId

```

#### <a name="find-the-status-of-server-side-encryption-of-a-disk"></a>ディスクのサーバー側暗号化の状態を確認する

[!INCLUDE [virtual-machines-disks-encryption-status-cli](../../../includes/virtual-machines-disks-encryption-status-cli.md)]

> [!IMPORTANT]
> カスタマー マネージド キーは、Azure Active Directory (Azure AD) の 1 つの機能である Azure リソース用マネージド ID に依存します。 カスタマー マネージド キーを構成すると、内部でマネージド ID がリソースに自動的に割り当てられます。 その後、サブスクリプション、リソース グループ、またはマネージド ディスクを 1 つの Azure AD ディレクトリから別のディレクトリに移動した場合、そのマネージド ディスクに関連付けられているマネージド ID は新しいテナントに転送されないため、カスタマー マネージド キーが機能しなくなることがあります。 詳細については、「[Azure AD ディレクトリ間のサブスクリプションの転送](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)」を参照してください。

[!INCLUDE [virtual-machines-disks-encryption-portal](../../../includes/virtual-machines-disks-encryption-portal.md)]

> [!IMPORTANT]
> カスタマー マネージド キーは、Azure Active Directory (Azure AD) の 1 つの機能である Azure リソース用マネージド ID に依存します。 カスタマー マネージド キーを構成すると、内部でマネージド ID がリソースに自動的に割り当てられます。 その後、サブスクリプション、リソース グループ、またはマネージド ディスクを 1 つの Azure AD ディレクトリから別のディレクトリに移動した場合、そのマネージド ディスクに関連付けられているマネージド ID は新しいテナントに転送されないため、カスタマー マネージド キーが機能しなくなることがあります。 詳細については、「[Azure AD ディレクトリ間のサブスクリプションの転送](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)」を参照してください。

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>サーバー側の暗号化と Azure ディスク暗号化の比較

[仮想マシンと仮想マシン スケール セットの Azure Disk Encryption](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) では、Linux の [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 機能を利用して、ゲスト VM 内のカスタマー マネージド キーを使用してマネージド ディスクを暗号化します。  カスタマー マネージド キーを使用したサーバー側の暗号化では、ストレージ サービス内のデータを暗号化することで、VM に対して任意の OS の種類とイメージを使用できるため、ADE がさらに向上します。

## <a name="next-steps"></a>次のステップ

- [カスタマー マネージド キーで暗号化されたディスクを作成するための Azure Resource Manager テンプレートを探索する](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Azure Key Vault とは](../../key-vault/general/overview.md)
- [カスタマー マネージド キー対応ディスクを含むコンピューターをレプリケートする](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [PowerShell を使用して Azure への VMware VM のディザスター リカバリーを設定する](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [PowerShell と Azure Resource Manager を使用して Azure への Hyper-V VM のディザスター リカバリーを設定する](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)

---
title: Windows 上で Azure Files で使用するポイント対サイト (P2S) VPN を構成する | Microsoft Docs
description: Windows 上で Azure Files で使用するポイント対サイト (P2S) VPN を構成する方法
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 527ab905997d18433d1dba5c16ee67c8146f5afa
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126458"
---
# <a name="configure-a-point-to-site-p2s-vpn-on-windows-for-use-with-azure-files"></a>Windows 上で Azure Files で使用するポイント対サイト (P2S) VPN を構成する
ポイント対サイト (P2S) VPN 接続を使用すると、ポート 445 を開くことなく、Azure の外部から SMB 経由で Azure ファイル共有をマウントできます。 ポイント対サイト VPN 接続は、Azure と個々のクライアントの間の VPN 接続です。 Azure Files で P2S VPN 接続を使用するには、接続したいクライアントごとに P2S VPN 接続を構成する必要があります。 オンプレミス ネットワークから Azure ファイル共有に接続する必要のある多数のクライアントが存在する場合は、クライアントごとのポイント対サイト接続の代わりにサイト間 (S2S) VPN 接続を使用できます。 詳細については、「[Azure Files で使用するサイト間 VPN を構成する](storage-files-configure-s2s-vpn.md)」を参照してください。

このハウツー記事を読み進める前に、Azure Files で使用可能なネットワーク オプションの完全な説明について[直接 Azure ファイル共有アクセスのネットワークに関する考慮事項](storage-files-networking-overview.md)に関するページを参照することを強くお勧めします。

この記事では、Azure ファイル共有をオンプレミスに直接マウントするために、Windows (Windows クライアントおよび Windows Server) 上でポイント対サイト VPN を構成する手順について詳細に説明します。 Azure File Sync のトラフィックを VPN 経由でルーティングすることを検討している場合は、[Azure File Sync のプロキシとファイアウォールの設定の構成](storage-sync-files-firewall-and-proxy.md)に関するページを参照してください。

## <a name="prerequisites"></a>前提条件
- 最新バージョンの Azure PowerShell モジュール。 Azure PowerShell をインストールする方法の詳細については、[Azure PowerShell モジュールのインストール](https://docs.microsoft.com/powershell/azure/install-az-ps)に関するページを参照し、オペレーティング システムを選択してください。 Windows 上で Azure CLI を使用することは可能ですが、下の手順は Azure PowerShell 用に提供されています。

- Azure Private DNS PowerShell モジュール。 現在、これは Azure PowerShell モジュールの一部として配布されていないため、次の方法でインストールできます。
    ```PowerShell
    if ($PSVersionTable.PSVersion -ge [System.Version]::new(6, 0)) {
        Install-Module -Name Az.PrivateDns -AllowClobber -AllowPrerelease
    } else {
        Install-Module -Name Az.PrivateDns -RequiredVersion "0.1.3"
    }

    Import-Module -Name Az.PrivateDns
    ```  

- オンプレミスにマウントする Azure ファイル共有。 ポイント対サイト VPN には [Standard](storage-how-to-create-file-share.md) または [Premium Azure ファイル共有](storage-how-to-create-premium-fileshare.md)のどちらかを使用できます。

## <a name="deploy-a-virtual-network"></a>仮想ネットワークをデプロイする
ポイント対サイト VPN 経由でオンプレミスから Azure ファイル共有やその他の Azure リソースにアクセスするには、仮想ネットワーク (VNet) を作成する必要があります。 自動的に作成される P2S VPN 接続は、オンプレミスの Windows コンピューターとこの Azure 仮想ネットワークの間のブリッジです。

次の PowerShell は、ストレージ アカウントのサービス エンドポイント用に 1 つ、ストレージ アカウントのプライベート エンドポイント用に 1 つ (これは、変化する可能性があるストレージ アカウントのパブリック IP のカスタム ルーティングを作成することなく、オンプレミスのストレージ アカウントにアクセスするために必要です)、および VPN サービスを提供する仮想ネットワーク ゲートウェイ用に 1 つの 3 つのサブネットを含む Azure 仮想ネットワークを作成します。 

忘れずに、`<region>`、`<resource-group>`、および `<desired-vnet-name>` を実際の環境の適切な値に置き換えてください。

```PowerShell
$region = "<region>"
$resourceGroupName = "<resource-group>" 
$virtualNetworkName = "<desired-vnet-name>"

$virtualNetwork = New-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName `
    -Location $region `
    -AddressPrefix "192.168.0.0/16"

Add-AzVirtualNetworkSubnetConfig `
    -Name "ServiceEndpointSubnet" `
    -AddressPrefix "192.168.0.0/24" `
    -VirtualNetwork $virtualNetwork `
    -ServiceEndpoint "Microsoft.Storage" `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "PrivateEndpointSubnet" `
    -AddressPrefix "192.168.1.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "GatewaySubnet" `
    -AddressPrefix "192.168.2.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

$virtualNetwork | Set-AzVirtualNetwork | Out-Null
$virtualNetwork = Get-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName

$serviceEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "ServiceEndpointSubnet" }
$privateEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "PrivateEndpointSubnet" }
$gatewaySubnet = $virtualNetwork.Subnets | ` 
    Where-Object { $_.Name -eq "GatewaySubnet" }
```

## <a name="restrict-the-storage-account-to-the-virtual-network"></a>ストレージ アカウントを仮想ネットワークに制限する
既定では、ストレージ アカウントを作成すると、要求を認証する手段 (Active Directory ID やストレージ アカウント キーの使用など) を持っている限り、世界中のどこからでもそのアカウントにアクセスできます。 このストレージ アカウントへのアクセスを今作成した仮想ネットワークに制限するには、仮想ネットワーク内のアクセスを許可し、その他のすべてのアクセスを拒否するネットワーク ルール セットを作成する必要があります。

ストレージ アカウントを仮想ネットワークに制限するには、サービス エンドポイントを使用する必要があります。 サービス エンドポイントは、パブリック DNS/パブリック IP に仮想ネットワーク内からしかアクセスできないようにするためのネットワーク構造です。 パブリック IP アドレスは同じままであることが保証されないため、最終的にはストレージ アカウントのサービス エンドポイントではなく、プライベート エンドポイントを使用する必要がありますが、サービス エンドポイントも公開されない限り、ストレージ アカウントを制限することはできません。

`<storage-account-name>` は、アクセスするストレージ アカウントに必ず置き換えてください。

```PowerShell
$storageAccountName = "<storage-account-name>"

$storageAccount = Get-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName

$networkRule = Add-AzStorageAccountNetworkRule `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -VirtualNetworkResourceId $serviceEndpointSubnet.Id

Update-AzStorageAccountNetworkRuleSet `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Bypass AzureServices `
    -DefaultAction Deny `
    -VirtualNetworkRule $networkRule | Out-Null
``` 

## <a name="create-a-private-endpoint-preview"></a>プライベート エンドポイントを作成する (プレビュー)
ストレージ アカウントのプライベート エンドポイントを作成すると、そのストレージ アカウントに仮想ネットワークの IP アドレス空間内の IP アドレスが割り当てられます。 このプライベート IP アドレスを使用してオンプレミスから Azure ファイル共有をマウントすると、VPN のインストールにより自動的に定義されたルーティング規則によって、マウント要求が VPN 経由でストレージ アカウントにルーティングされます。 

```PowerShell
$internalVnet = Get-AzResource `
    -ResourceId $virtualNetwork.Id `
    -ApiVersion "2019-04-01"

$internalVnet.Properties.subnets[1].properties.privateEndpointNetworkPolicies = "Disabled"
$internalVnet | Set-AzResource -Force | Out-Null

$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
    -Name "myConnection" `
    -PrivateLinkServiceId $storageAccount.Id `
    -GroupId "file"

$privateEndpoint = New-AzPrivateEndpoint `
    -ResourceGroupName $resourceGroupName `
    -Name "$storageAccountName-privateEndpoint" `
    -Location $region `
    -Subnet $privateEndpointSubnet `
    -PrivateLinkServiceConnection $privateEndpointConnection

$zone = Get-AzPrivateDnsZone -ResourceGroupName $resourceGroupName
if ($null -eq $zone) {
    $zone = New-AzPrivateDnsZone `
        -ResourceGroupName $resourceGroupName `
        -Name "privatelink.file.core.windows.net"
} else {
    $zone = $zone[0]
}

$link = New-AzPrivateDnsVirtualNetworkLink `
    -ResourceGroupName $resourceGroupName `
    -ZoneName $zone.Name `
    -Name ($virtualNetwork.Name + "-link") `
    -VirtualNetworkId $virtualNetwork.Id

$internalNic = Get-AzResource `
    -ResourceId $privateEndpoint.NetworkInterfaces[0].Id `
    -ApiVersion "2019-04-01"

foreach($ipconfig in $internalNic.Properties.ipConfigurations) {
    foreach($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) {
        $recordName = $fqdn.split('.', 2)[0]
        $dnsZone = $fqdn.split('.', 2)[1]
        New-AzPrivateDnsRecordSet `
            -ResourceGroupName $resourceGroupName ` 
            -Name $recordName `
            -RecordType A `
            -ZoneName $zone.Name `
            -Ttl 600 `
            -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
                -IPv4Address $ipconfig.properties.privateIPAddress) | Out-Null
    }
}
```

## <a name="create-root-certificate-for-vpn-authentication"></a>VPN 認証用のルート証明書の作成
オンプレミスの Windows マシンからの VPN 接続を認証して仮想ネットワークにアクセスできるようにするには、2 つの証明書を作成する必要があります。1 つは仮想マシン ゲートウェイに提供されるルート証明書、もう 1 つはルート証明書で署名されているクライアント証明書です。 次の PowerShell は、ルート証明書を作成します。クライアント証明書は、ゲートウェイからの情報を使用して Azure 仮想ネットワーク ゲートウェイを作成した後に作成されます。 

```PowerShell
$rootcertname = "CN=P2SRootCert"
$certLocation = "Cert:\CurrentUser\My"
$vpnTemp = "C:\vpn-temp\"
$exportedencodedrootcertpath = $vpnTemp + "P2SRootCertencoded.cer"
$exportedrootcertpath = $vpnTemp + "P2SRootCert.cer"

if (-Not (Test-Path $vpnTemp)) {
    New-Item -ItemType Directory -Force -Path $vpnTemp | Out-Null
}

if ($PSVersionTable.PSVersion -ge [System.Version]::new(6, 0)) {
    Install-Module WindowsCompatibility
    Import-WinModule PKI
}

$rootcert = New-SelfSignedCertificate `
    -Type Custom `
    -KeySpec Signature `
    -Subject $rootcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -KeyUsageProperty Sign `
    -KeyUsage CertSign

Export-Certificate `
    -Cert $rootcert `
    -FilePath $exportedencodedrootcertpath `
    -NoClobber | Out-Null

certutil -encode $exportedencodedrootcertpath $exportedrootcertpath | Out-Null

$rawRootCertificate = Get-Content -Path $exportedrootcertpath

[System.String]$rootCertificate = ""
foreach($line in $rawRootCertificate) { 
    if ($line -notlike "*Certificate*") { 
        $rootCertificate += $line 
    } 
}
```

## <a name="deploy-virtual-network-gateway"></a>仮想ネットワーク ゲートウェイのデプロイ
Azure 仮想ネットワーク ゲートウェイは、オンプレミスの Windows マシンの接続先となるサービスです。 このサービスをデプロイするには、2 つの基本的なコンポーネントが必要です。クライアントが世界中のどこにあってもそのクライアントへのゲートウェイを識別するパブリック IP と、クライアントの認証に使用される先ほど作成したルート証明書です。

`<desired-vpn-name-here>` は、これらのリソースに必要な名前に置き換えてください。

> [!Note]  
> Azure 仮想ネットワーク ゲートウェイのデプロイには、最大で 45 分かかる場合があります。 このリソースがデプロイされている間、この PowerShell スクリプトはデプロイの完了をブロックします。 これは予期されることです。

```PowerShell
$vpnName = "<desired-vpn-name-here>" 
$publicIpAddressName = "$vpnName-PublicIP"

$publicIPAddress = New-AzPublicIpAddress `
    -ResourceGroupName $resourceGroupName ` 
    -Name $publicIpAddressName `
    -Location $region `
    -Sku Basic `
    -AllocationMethod Dynamic

$gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig `
    -Name "vnetGatewayConfig" `
    -SubnetId $gatewaySubnet.Id `
    -PublicIpAddressId $publicIPAddress.Id

$azRootCertificate = New-AzVpnClientRootCertificate `
    -Name "P2SRootCert" `
    -PublicCertData $rootCertificate

$vpn = New-AzVirtualNetworkGateway `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -Location $region `
    -GatewaySku VpnGw1 `
    -GatewayType Vpn `
    -VpnType RouteBased `
    -IpConfigurations $gatewayIpConfig `
    -VpnClientAddressPool "172.16.201.0/24" `
    -VpnClientProtocol IkeV2 `
    -VpnClientRootCertificates $azRootCertificate
```

## <a name="create-client-certificate"></a>クライアント証明書の作成
クライアント証明書は、仮想ネットワーク ゲートウェイの URI を使用して作成されます。 この証明書は、先ほど作成したルート証明書で署名されています。

```PowerShell
$clientcertpassword = "1234"

$vpnClientConfiguration = New-AzVpnClientConfiguration `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -AuthenticationMethod EAPTLS

Invoke-WebRequest `
    -Uri $vpnClientConfiguration.VpnProfileSASUrl `
    -OutFile "$vpnTemp\vpnclientconfiguration.zip"

Expand-Archive `
    -Path "$vpnTemp\vpnclientconfiguration.zip" `
    -DestinationPath "$vpnTemp\vpnclientconfiguration"

$vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"
$vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

$exportedclientcertpath = $vpnTemp + "P2SClientCert.pfx"
$clientcertname = "CN=" + $vpnProfile.VpnServer

$clientcert = New-SelfSignedCertificate `
    -Type Custom `
    -DnsName $vpnProfile.VpnServer `
    -KeySpec Signature `
    -Subject $clientcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -Signer $rootcert `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")

$mypwd = ConvertTo-SecureString -String $clientcertpassword -Force -AsPlainText

Export-PfxCertificate `
    -FilePath $exportedclientcertpath `
    -Password $mypwd `
    -Cert $clientcert | Out-Null
```

## <a name="configure-the-vpn-client"></a>VPN クライアントの構成
Azure 仮想ネットワーク ゲートウェイは、オンプレミスの Windows マシンで VPN 接続を初期化するために必要な構成ファイルを含むダウンロード可能なパッケージを作成します。 Windows 10/Windows Server 2016 以降の [Always On VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/) 機能を使用して VPN 接続を構成します。 このパッケージには、レガシ Windows VPN クライアントを構成する実行可能パッケージも含まれます (必要な場合)。 このガイドでは、レガシ Windows VPN クライアントではなく、Always On VPN を使用します。Always On VPN クライアントは、自分のマシンに対する管理者権限を持っていないエンドユーザーでも、Azure VPN に接続したり切断したりできるからです。 

次のスクリプトでは、仮想ネットワーク ゲートウェイの認証に必要なクライアント証明書をインストールし、VPN パッケージをダウンロードしてインストールします。 `<computer1>`、`<computer2>` は、忘れずに希望するコンピューターに置き換えてください。 このスクリプトの `$sessions` 配列に PowerShell セッションを追加すると、必要な数のマシンで実行できます。 使用アカウントは、これらの各マシンの管理者である必要があります。 マシンのいずれかがスクリプトを実行しているローカル マシンである場合、昇格された PowerShell セッションからスクリプトを実行する必要があります。 

```PowerShell
$sessions = [System.Management.Automation.Runspaces.PSSession[]]@()
$sessions += New-PSSession -ComputerName "<computer1>"
$sessions += New-PSSession -ComputerName "<computer2>"

foreach ($session in $sessions) {
    Invoke-Command -Session $session -ArgumentList $vpnTemp -ScriptBlock { 
        $vpnTemp = $args[0]
        if (-Not (Test-Path $vpnTemp)) {
            New-Item `
                -ItemType Directory `
                -Force `
                -Path "C:\vpn-temp" | Out-Null
        }
    }

    Copy-Item `
        -Path $exportedclientcertpath, $exportedrootcertpath, "$vpnTemp\vpnclientconfiguration.zip" `
        -Destination $vpnTemp `
        -ToSession $session

    Invoke-Command `
        -Session $session `
        -ArgumentList `
            $mypwd, `
            $vpnTemp, `
            $virtualNetworkName
        -ScriptBlock { 
            $mypwd = $args[0] 
            $vpnTemp = $args[1]
            $virtualNetworkName = $args[2]

            Import-PfxCertificate `
                -Exportable `
                -Password $mypwd `
                -CertStoreLocation "Cert:\LocalMachine\My" `
                -FilePath "$vpnTemp\P2SClientCert.pfx" | Out-Null

            Import-Certificate `
                -FilePath "$vpnTemp\P2SRootCert.cer" `
                -CertStoreLocation "Cert:\LocalMachine\Root" | Out-Null

            Expand-Archive `
                -Path "$vpnTemp\vpnclientconfiguration.zip" `
                -DestinationPath "$vpnTemp\vpnclientconfiguration"
            $vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"

            $vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

            Add-VpnConnection `
                -Name $virtualNetworkName `
                -ServerAddress $vpnProfile.VpnServer ` 
                -TunnelType Ikev2 `
                -EncryptionLevel Required `
                -AuthenticationMethod MachineCertificate `
                -SplitTunneling `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.Routes `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.VpnClientAddressPool `
                -AllUserConnection

            rasdial $virtualNetworkName
        }
}

Remove-Item -Path $vpnTemp -Recurse
```

## <a name="mount-azure-file-share"></a>Azure ファイル共有をマウントする
ポイント対サイト VPN のセットアップが完了したので、これを使用して、PowerShell でセットアップしたコンピューターに Azure ファイル共有をマウントします。 次の例では、共有をマウントし、共有が実際にマウントされていることを証明するために共有のルート ディレクトリを一覧表示し、共有のマウントを解除します。 残念ながら、PowerShell リモート処理を介して共有を永続的にマウントすることはできません。 永続的にマウントする方法については、[Windows で Azure ファイル共有を使用する](storage-how-to-use-files-windows.md)方法に関する記事を参照してください。 

```PowerShell
$myShareToMount = "<file-share>"

$storageAccountKeys = Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName
$storageAccountKey = ConvertTo-SecureString `
    -String $storageAccountKeys[0].Value `
    -AsPlainText `
    -Force

$nic = Get-AzNetworkInterface -ResourceId $privateEndpoint.NetworkInterfaces[0].Id
$storageAccountPrivateIP = $nic.IpConfigurations[0].PrivateIpAddress

Invoke-Command `
    -Session $sessions `
    -ArgumentList  `
        $storageAccountName, `
        $storageAccountKey, `
        $storageAccountPrivateIP, `
        $myShareToMount `
    -ScriptBlock {
        $storageAccountName = $args[0]
        $storageAccountKey = $args[1]
        $storageAccountPrivateIP = $args[2]
        $myShareToMount = $args[3]

        $credential = [System.Management.Automation.PSCredential]::new(
            "AZURE\$storageAccountName", 
            $storageAccountKey)

        New-PSDrive `
            -Name Z `
            -PSProvider FileSystem `
            -Root "\\$storageAccountPrivateIP\$myShareToMount" `
            -Credential $credential `
            -Persist | Out-Null
        Get-ChildItem -Path Z:\
        Remove-PSDrive -Name Z
    }
```

## <a name="see-also"></a>関連項目
- [直接 Azure ファイル共有アクセスに関するネットワークの考慮事項](storage-files-networking-overview.md)
- [Linux 上で Azure Files で使用するポイント対サイト (P2S) VPN を構成する](storage-files-configure-p2s-vpn-linux.md)
- [Azure Files で使用するサイト間 (S2S) VPN を構成する](storage-files-configure-s2s-vpn.md)
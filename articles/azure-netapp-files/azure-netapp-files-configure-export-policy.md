---
title: NFS ボリュームのエクスポート ポリシーを構成する - Azure NetApp Files
description: Azure NetApp Files を使用して NFS ボリュームへのアクセスを制御するエクスポート ポリシーの構成方法について説明します
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 10/18/2019
ms.openlocfilehash: e59648ee76b6715029c690329cbf8f4f1eee7243
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483654"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>NFS ボリュームのエクスポート ポリシーを構成する

Azure NetApp Files ボリュームへのアクセスを制御するエクスポート ポリシーは、必要に応じて構成することができます。 Azure NetApp Files エクスポート ポリシーでサポートされるのは NFS のみです。  NFSv3 と NFSv4 の両方がサポートされています。 

## <a name="steps"></a>手順 

1.  Azure NetApp Files ナビゲーション ウィンドウで **[ポリシーのエクスポート]** をクリックします。 

2.  次のフィールドの情報を指定して、エクスポート ポリシー ルールを作成します。   
    *  **[インデックス]**    
        ルールのインデックス番号を指定します。  
        エクスポート ポリシーは、最大 5 つのルールで構成できます。 ルールは、リスト内の対応するインデックス番号の順に評価されます インデックス番号が小さいルールが先に評価されます。 たとえば、インデックス番号が 1 のルールは、インデックス番号が 2 のルールよりも前に評価されます。 

    * **[許可されたクライアント]**    
        次のいずれかの形式で値を指定します。  
        * IPv4 アドレス (例: `10.1.12.24`) 
        * ビット数として表されたサブネット マスクを指定した IPv4 アドレス (例: `10.1.12.10/4`)

    * **Access (アクセス)**  
        次のアクセス タイプのいずれかを選択します。  
        * アクセス権なし 
        * 読み取りと書き込み
        * [読み取り専用]

    ![エクスポート ポリシー](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>次のステップ 
* [ボリュームを管理する](azure-netapp-files-manage-volumes.md)
* [仮想マシンのボリュームをマウント/マウント解除する](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [スナップショットを管理する](azure-netapp-files-manage-snapshots.md)

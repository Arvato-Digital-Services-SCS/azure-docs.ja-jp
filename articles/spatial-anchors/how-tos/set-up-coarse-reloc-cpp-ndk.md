---
title: C++/NDK でデバイス上のセンサーを使用してアンカーを作成して配置する方法 | Microsoft Docs
description: C++/NDK でデバイス上のセンサーを使用してアンカーを作成して配置する方法の詳細な説明。
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: f5de4ae050ff01bc86f8c1e11a2afb2887fd8bd7
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74093028"
---
# <a name="how-to-create-and-locate-anchors-using-on-device-sensors-in-cndk"></a>C++/NDK でデバイス上のセンサーを使用してアンカーを作成して配置する方法

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Azure Spatial Anchors では、デバイス上の配置センサー データを、作成したアンカーに関連付けることができます。 このデータを使用して、デバイスの近くにアンカーがあるかどうかをすばやく判断することもできます。 詳細については、「[粗い再局在化](../concepts/coarse-reloc.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

このガイドを完了するには、以下が必要です。

- C++ と <a href="https://developer.android.com/ndk/" target="_blank">Android ネイティブ開発キット</a>についての基本的な知識。
- 「[Azure Spatial Anchors の概要](../overview.md)」を読んでいる。
- [5 分間のクイック スタート](../index.yml)のいずれかを完了している。
- 「[アンカーを作成および探知する方法](../create-locate-anchors-overview.md)」読んでいる。

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```cpp
// Create the sensor fingerprint provider
std::shared_ptr<PlatformLocationProvider> sensorProvider;
sensorProvider = std::make_shared<PlatformLocationProvider>();

// Allow GPS
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->GeoLocationEnabled(true);

// Allow WiFi scanning
sensors->WifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
std::vector<std::string> uuids;
uuids.push_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.push_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

// Allow the set of known BLE beacons
sensors->BluetoothEnabled(true);
sensors->KnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```cpp
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession->LocationProvider(sensorProvider);

// Configure the near-device criteria
auto nearDeviceCriteria = std::make_shared<NearDeviceCriteria>();
nearDeviceCriteria->DistanceInMeters(5.0f);
nearDeviceCriteria->MaxResultCount(25);

// Set the session's locate criteria
auto anchorLocateCriteria = std::make_shared<AnchorLocateCriteria>();
anchorLocateCriteria->NearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession->CreateWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]
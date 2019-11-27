---
title: Azure Event Grid の Machine Learning イベント スキーマ
description: Azure Event Grid の Machine Learning ワークスペース イベント用に用意されているプロパティについて説明します
services: event-grid
author: jenns
ms.service: event-grid
ms.topic: reference
ms.date: 10/18/2019
ms.author: jenns
ms.openlocfilehash: 5f2d23b3fe33691d37dc00b2d4e79036293252d9
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132875"
---
# <a name="azure-event-grid-event-schema-for-azure-machine-learning"></a>Azure Machine Learning 用の Azure Event Grid イベント スキーマ

この記事では、Machine Learning ワークスペース イベントのプロパティとスキーマについて説明します。 イベント スキーマの概要については、「[Azure Event Grid イベント スキーマ](event-schema.md)」を参照してください。

サンプル スクリプトとチュートリアルの一覧については、[AzureML のイベント ソース](event-sources.md#azure-machine-learning)に関する記事をご覧ください。

## <a name="available-event-types"></a>使用可能なイベントの種類

Azure Machine Learning から出力されるイベントの種類は次のとおりです。

| イベントの種類 | 説明 |
| ---------- | ----------- |
| Microsoft.MachineLearningServices.ModelRegistered | 新しいモデルまたはモデルのバージョンが正常に登録された場合に発生します。 |
| Microsoft.MachineLearningServices.ModelDeployed | モデルがエンドポイントに正常にデプロイされたときに発生します。 |
| Microsoft.MachineLearningServices.RunCompleted | 実行が正常に完了したときに発生します。 |
| Microsoft.MachineLearningServices.DatasetDriftDetected | データセット ドリフト モニターがドリフトを検出したときに発生します。 |

## <a name="the-contents-of-an-event-response"></a>イベント応答の内容

イベントがトリガーされると、Event Grid サービスにより、そのイベントに関するデータがサブスクライブしているエンドポイントに送信されます。

このセクションには、各イベントでそのデータがどのように見えるかの例が含まれています。

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Microsoft.MachineLearningServices.ModelRegistered イベント

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Microsoft.MachineLearningServices.ModelDeployed イベント

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Microsoft.MachineLearningServices.RunCompleted イベント

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Microsoft.MachineLearningServices.DatasetDriftDetected イベント

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>イベントのプロパティ

イベントのトップレベルのデータを次に示します。

| プロパティ | 種類 | 説明 |
| -------- | ---- | ----------- |
| topic | string | イベント ソースの完全なリソース パス。 このフィールドは書き込み可能ではありません。 この値は Event Grid によって指定されます。 |
| subject | string | 発行元が定義したイベントの対象のパス。 |
| eventType | string | このイベント ソース用に登録されたイベントの種類のいずれか。 |
| eventTime | string | プロバイダーの UTC 時刻に基づくイベントの生成時刻。 |
| id | string | イベントの一意識別子。 |
| data | object | Blob Storage イベントのデータ。 |
| dataVersion | string | データ オブジェクトのスキーマ バージョン。 スキーマ バージョンは発行元によって定義されます。 |
| metadataVersion | string | イベント メタデータのスキーマ バージョン。 最上位プロパティのスキーマは Event Grid によって定義されます。 この値は Event Grid によって指定されます。 |

データ オブジェクトには、イベントの種類ごとに次のプロパティがあります。

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft.MachineLearningServices.ModelRegistered

| プロパティ | 種類 | 説明 |
| -------- | ---- | ----------- |
| ModelName | string | 登録されたモデルの名前。 |
| ModelVersion | int | 登録されたモデルのバージョン。 |
| ModelTags | object | 登録されたモデルのタグ。 |
| ModelProperties | object | 登録されたモデルのプロパティ。 |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft.MachineLearningServices.ModelDeployed

| プロパティ | 種類 | 説明 |
| -------- | ---- | ----------- |
| ServiceName | string | デプロイされたサービスの名前。 |
| ServiceComputeType | string | デプロイされたサービスのコンピューティングの種類 (ACI、AKS など)。 |
  | ModelIds | string | モデル ID のコンマ区切りリスト。 サービスにデプロイされているモデルの ID。 |
| ServiceTags | object | デプロイされたサービスのタグ。 |
| ServiceProperties | object | デプロイされたサービスのプロパティ。 |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft.MachineLearningServices.RunCompleted

| プロパティ | 種類 | 説明 |
| -------- | ---- | ----------- |
| ExperimentId | string | 実行が属する実験の ID。 |
| ExperimentName | string | 実行が属する実験の名前。 |
| RunId | string | 完了した実行の ID。 |
| RunType | string | 完了した実行の種類。 |
| RunTags | object | 完了した実行のタグ。 |
| RunProperties | object | 完了した実行のプロパティ。 |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft.MachineLearningServices.DatasetDriftDetected

| プロパティ | 種類 | 説明 |
| -------- | ---- | ----------- |
| DataDriftId | string | イベントをトリガーしたデータ ドリフト モニターの ID。 |
| DataDriftName | string | イベントをトリガーしたデータ ドリフト モニターの名前。 |
| RunId | string | データ ドリフトを検出した実行の ID。 |
| BaseDatasetId | string | ドリフトを検出するために使用される基本データセットの ID。 |
| TargetDatasetId | string | ドリフトを検出するために使用されるターゲット データセットの ID。 |
| DriftCoefficient | double | イベントをトリガーした係数結果。 |
| StartTime | datetime | ドリフト検出の原因となったターゲット データセット時系列の開始時刻。  |
| EndTime | datetime | ドリフト検出の原因となったターゲット データセット時系列の終了時刻。 |


## <a name="next-steps"></a>次の手順

* Azure Event Grid の概要については、[Event Grid の紹介](overview.md)に関する記事を参照してください。
* Azure Event Grid サブスクリプションの作成の詳細については、「[Event Grid サブスクリプション スキーマ](subscription-creation-schema.md)」を参照してください。
* Azure Machine Learning での Azure Event Grid の使用方法の概要については、「[Azure Machine Learning イベントを使用する](/azure/machine-learning/service/concept-event-grid-integration)」を参照してください。
* Azure Machine Learning で Azure Event Grid を使用する例については、[イベント ドリブン Machine Learning ワークフローの作成](/azure/machine-learning/service/how-to-use-event-grid)に関する記事を参照してください。

---
title: このリリースの新機能
titleSuffix: Azure Machine Learning
description: Azure Machine Learning および Machine Learning SDK と Data Prep Python SDK の最新の更新プログラムについて説明します。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 03/10/2020
ms.openlocfilehash: 6bf26a739169c561e95c7376a75166daf9aa9fb0
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309987"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning のリリース ノート

この記事では、Azure Machine Learning の各リリースについて説明します。  SDK リファレンス コンテンツの詳細については、Azure Machine Learning の[**メインの SDK for Python**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) のリファレンス ページを参照してください。

バグおよび対処法については、[既知の問題のリスト](resource-known-issues.md)を参照してください。

## <a name="2020-05-26"></a>2020-05-26

### <a name="azure-machine-learning-sdk-for-python-v160"></a>Azure Machine Learning SDK for Python v1.6.0

+ **新機能**
  + **azureml-automl-runtime**
    + AutoML の予測で、モデルを再トレーニングしなくても、事前に指定した期間の最大値を超える顧客の予測がサポートされるようになりました。 予測の対象が指定した期間の最大値よりも未来になる場合でも、forecast() 関数によって、再帰的操作モードを使用してそれ以降の日付に対してポイント予測が作成されます。 この新機能の説明については、[フォルダー](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)内の "forecasting-forecast-function" ノートブックの「期間の最大値を超える予測」セクションを参照してください。"
  
  + **azureml-pipeline-steps**
    + ParallelRunStep がリリースされ、**azureml-pipeline-steps** パッケージに含まれるようになりました。 **azureml-contrib-pipeline-steps** パッケージに含まれる既存の ParallelRunStep は非推奨になります。 パブリック プレビュー バージョンからの変更点は次のとおりです。
      + 指定した任意のバッチに対してメソッドを実行する最大呼び出しを制御するための、省略可能な構成可能パラメーター `run_max_try` が追加されました (既定値は 3)。
      + PipelineParameter が自動生成されなくなりました。 次の構成可能な値は、PipelineParameter として明示的に設定できます。
        + mini_batch_size
        + node_count
        + process_count_per_node
        + logging_level
        + run_invocation_timeout
        + run_max_try
      + process_count_per_node の既定値が 1 に変更されます。 パフォーマンスを向上させるには、ユーザーがこの値を調整する必要があります。 ベスト プラクティスは、GPU または CPU ノードが持つ数として設定することです。
      + ParallelRunStep ではパッケージが挿入されません。ユーザーが **azureml-core** および **azureml-dataprep[pandas, fuse]** パッケージを環境定義に含める必要があります。 user_managed_dependencies と共にカスタム Docker イメージを使用する場合は、ユーザーがイメージに Conda をインストールする必要があります。
      
+ **重大な変更**
  + **azureml-pipeline-steps**
    + AutoMLConfig に対する有効な種類の入力としての azureml.dprep.Dataflow の使用が非推奨になりました
  + **azureml-train-automl-client**
    + AutoMLConfig に対する有効な種類の入力としての azureml.dprep.Dataflow の使用が非推奨になりました

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + ユーザーにクライアントのダウングレードを要求した `get_output` 中に、警告が出力されることがあるバグを修正しました。
    + cudatoolkit=9.0 に依存するように Mac を更新しました (バージョン 10 ではまだ使用できないため)。
    + リモート コンピューティングでトレーニングを行ったときの phrophet および xgboost モデルに対する制限が削除されます。
    + AutoML でのログ記録が改善されました
    + 予測タスクにおけるカスタム特徴量化のエラー処理が改善されました。
    + ユーザーがタイム ラグ機能を含めて予測を生成できるようにする機能が追加されました。
    + ユーザー エラーを正しく表示するためのエラー メッセージに対する更新。
    + training_data と共に使用される cv_split_column_names のサポート
    + 例外メッセージとトレースバックのログ記録が更新されます。
  + **azureml-automl-runtime**
    + 欠落値の補完を予測するためのガードレールが有効になります。
    + AutoML でのログ記録が改善されました
    + データ準備例外に対する詳細なエラー処理を追加しました
    + リモート コンピューティングでトレーニングを行ったときの phrophet および xgboost モデルに対する制限が削除されます。
    + `azureml-train-automl-runtime` および `azureml-automl-runtime` の `pytorch`、`scipy`、および `cudatoolkit` に対する依存関係が更新されました。 `pytorch==1.4.0`、`scipy>=1.0.0,<=1.3.1`、および `cudatoolkit==10.1.243` がサポートされるようになりました。
    + 予測タスクにおけるカスタム特徴量化のエラー処理が改善されました。
    + データセットの予測頻度の検出メカニズムが改善されました。
    + 一部のデータセットでトレーニングする Prophet モデルに関する問題を修正しました。
    + 予測中の期間の最大値の自動検出が改善されました。
    + ユーザーがタイム ラグ機能を含めて予測を生成できるようにする機能が追加されました。
    +  予測関数に機能が追加され、予測モデルを再トレーニングすることなくトレーニングされた期間を超えて予測を提供できるようになります。
    + training_data と共に使用される cv_split_column_names のサポート
  + **azureml-contrib-automl-dnn-forecasting**
    + AutoML でのログ記録が改善されました
  + **azureml-contrib-mir**
    + ManagedInferencing に Windows サービスのサポートが追加されます
    + MIR コンピューティングのアタッチ、SingleModelMirWebservice クラスなどの古い MIR ワークフローが削除されます。contrib-mir パッケージに配置されているモデルのプロファイルが削除されます
  + **azureml-contrib-pipeline-steps**
    + YAML サポートに対するマイナー修正
    + ParallelRunStep が一般提供としてリリースされます。azureml.contrib.pipeline.steps に非推奨の通知があり、azureml.pipeline.steps に移行されます
  + **azureml-contrib-reinforcementlearning**
    + RL ロード テスト ツール
    + RL 推定器でのスマートな既定値の使用
  + **azureml-core**
    + MIR コンピューティングのアタッチ、SingleModelMirWebservice クラスなどの古い MIR ワークフローが削除されます。contrib-mir パッケージに配置されているモデルのプロファイルが削除されます
    + プロファイル エラーが発生した場合にユーザーに表示される情報を修正しました。要求 ID が記載され、メッセージがよりわかりやすく変更されました。 プロファイル ランナーに新しいプロファイル ワークフローが追加されました
    + データセットの実行エラーが発生した場合のエラー テキストが大幅に改善されました。
    + ワークスペースのプライベート リンク CLI のサポートが追加されました。
    + 省略可能なパラメーター `invalid_lines` が `Dataset.Tabular.from_json_lines_files` に追加されました。これにより、無効な JSON を含む行の処理方法を指定できます。
    + 次のリリースでは、コンピューティングの実行ベースの作成が非推奨になります。 実際の Amlcompute クラスターを永続的なコンピューティング先として作成し、そのクラスター名を実行構成でのコンピューティング先として使用することをお勧めします。 こちらのノートブックの例を参照してください: aka.ms/amlcomputenb
    + データセットの実行エラーが発生した場合のエラー メッセージが大幅に改善されました。
  + **azureml-dataprep**
    + pyarrow バージョンのアップグレードに対する警告がより明示的になりました。
    + データフローの実行に失敗した場合のエラー処理と返されるメッセージが改善されました。
  + **azureml-interpret**
    + azureml-interpret パッケージのドキュメントが更新されます。
    + 解釈可能パッケージとノートブックが最新の sklearn 更新プログラムと互換性を持つように修正されました
  + **azureml-opendatasets**
    + 返されるデータがない場合に None が返されます。
    + to_pandas_dataframe のパフォーマンスが改善されます。
  + **azureml-pipeline-core**
    + YAML からの読み込みが破損していた ParallelRunStep のクイック修正
    + ParallelRunStep が一般提供としてリリースされます。azureml.contrib.pipeline.steps に非推奨の通知があり、azureml.pipeline.steps に移行されます。次のような新機能があります。1. PipelineParameter としての データセット 2. 新しいパラメーター run_max_retry 3. 構成可能な append_row 出力ファイル名
  + **azureml-pipeline-steps**
    + 入力データの有効な種類として azureml.dprep.Dataflow を非推奨にしました。
    + YAML からの読み込みが破損していた ParallelRunStep のクイック修正
    + ParallelRunStep が一般提供としてリリースされます。azureml.contrib.pipeline.steps に非推奨の通知があり、azureml.pipeline.steps に移行されます。次のような新機能があります。
      + PipelineParameter としての データセット
      + 新しいパラメーター run_max_retry
      + 構成可能な append_row 出力ファイル名
  + **azureml-telemetry**
    + 例外メッセージとトレースバックのログ記録が更新されます。
  + **azureml-train-automl-client**
    + AutoML でのログ記録が改善されました
    + ユーザー エラーを正しく表示するためのエラー メッセージに対する更新。
    + training_data と共に使用される cv_split_column_names のサポート
    + 入力データの有効な種類として azureml.dprep.Dataflow を非推奨にしました。
    + cudatoolkit=9.0 に依存するように Mac を更新しました (バージョン 10 ではまだ使用できないため)。
    + リモート コンピューティングでトレーニングを行ったときの phrophet および xgboost モデルに対する制限が削除されます。
    + `azureml-train-automl-runtime` および `azureml-automl-runtime` の `pytorch`、`scipy`、および `cudatoolkit` に対する依存関係が更新されました。 `pytorch==1.4.0`、`scipy>=1.0.0,<=1.3.1`、および `cudatoolkit==10.1.243` がサポートされるようになりました。
    + ユーザーがタイム ラグ機能を含めて予測を生成できるようにする機能が追加されました。
  + **azureml-train-automl-runtime**
    + AutoML でのログ記録が改善されました
    + データ準備例外に対する詳細なエラー処理を追加しました
    + リモート コンピューティングでトレーニングを行ったときの phrophet および xgboost モデルに対する制限が削除されます。
    + `azureml-train-automl-runtime` および `azureml-automl-runtime` の `pytorch`、`scipy`、および `cudatoolkit` に対する依存関係が更新されました。 `pytorch==1.4.0`、`scipy>=1.0.0,<=1.3.1`、および `cudatoolkit==10.1.243` がサポートされるようになりました。
    + ユーザー エラーを正しく表示するためのエラー メッセージに対する更新。
    + training_data と共に使用される cv_split_column_names のサポート
  + **azureml-train-core**
    + 新しい HyperDrive 固有の例外セットが追加されました。 azureml.train.hyperdrive で詳細な例外がスローされるようになります。
  + **azureml-widgets**
    + AzureML ウィジェットが JupyterLab で表示されません
  

## <a name="2020-05-11"></a>2020-05-11

### <a name="azure-machine-learning-sdk-for-python-v150"></a>Azure Machine Learning SDK for Python v1.5.0

+ **新機能**
  + **プレビュー機能**
    + **azureml-contrib-reinforcementlearning**
        + Azure Machine Learning は、[Ray](https://ray.io) フレームワークを使用した強化学習のプレビュー サポートをリリースします。 `ReinforcementLearningEstimator` を使用すると、Azure Machine Learning の GPU および CPU コンピューティング先全体にわたり、強化学習エージェントをトレーニングできます。

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + 以前の PR で誤って残されていた警告ログを修正しました。 ログはデバッグに使用されたもので、誤って残されていました。
    + バグの修正: プロファイリング中の部分的な障害についてクライアントに通知します
  + **azureml-automl-core**
    + データセットに複数の時系列が含まれている場合に時系列の並列調整を有効にすることで、AutoML の予測で Prophet/AutoArima モデルが高速化されます。 この新機能を活用するために、AutoMLConfig で "max_cores_per_iteration =-1" (使用可能なすべての CPU コアを使用) を設定することをお勧めします。
    + コンソール インターフェイスの印刷ガードレールのキー エラーを修正しました
    + Experimentation_timeout_hours のエラーメッセージを修正しました
    + AutoML の Tensorflow モデルを非推奨にしました。
  + **azureml-automl-runtime**
    + Experimentation_timeout_hours のエラーメッセージを修正しました
    + キャッシュ ストアから逆シリアル化しようとしたときの未分類の例外を修正しました
    + データセットに複数の時系列が含まれている場合に時系列の並列調整を有効にすることで、AutoML の予測で Prophet/AutoArima モデルが高速化されます。
    + テスト/予測セットにトレーニング セットのグレインのいずれかが含まれていないデータ セットで、有効なローリング ウィンドウを使用した予測を修正しました。
    + 欠損データの処理を改善しました
    + 時系列を含むデータ セットの予測時の予測間隔に関する問題を修正しました。予測間隔は時間に沿っていませんでした。
    + 予測タスクのデータ シェイプの検証が向上しました。
    + 頻度の検出が向上しました。
    + 予測タスクのクロス検証の分割が生成されない場合に、より適切なエラー メッセージが作成されるようになりました。
    + 欠損値ガードレールを正しく印刷するために、コンソール インターフェイスを修正しました。
    + AutoMLConfig の cv_split_indices 入力にデータ型チェックを適用しました。
  + **azureml-cli-common**
    + バグの修正: プロファイリング中の部分的な障害についてクライアントに通知します
  + **azureml-contrib-mir**
    + 現在展開されている MIR リビジョンと、ユーザーが指定した最新のバージョンに関する情報をリレーする azureml.contrib.mir.RevisionStatus クラスを追加しました。 このクラスは、'deployment_status' 属性の下にある MirWebservice オブジェクトに含まれています。
    + MirWebservice 型の Web サービスとその子クラス SingleModelMirWebservice の更新を有効にしました。
  + **azureml-contrib-reinforcementlearning**
    + Ray 0.8.3 のサポートを追加しました
    + AmlWindowsCompute は、マウントされたストレージとして Azure Files のみをサポートします
    + Health_check_timeout を health_check_timeout_seconds に名称変更しました
    + いくつかのクラス/メソッドの説明を修正しました。
  + **azureml-core**
    + USGovernment および China のクラウドで WASB から Blob への変換が有効になりました。
    + 閲覧者ロールで az ml run CLI コマンドを使用して実行情報を取得できるようにバグを修正しました
    + 入力データ セットを使用した Azure ML リモート実行中に行っていた不要なログ記録を廃止しました。
    + RCranPackage で、CRAN パッケージ バージョンの "version" パラメーターがサポートされるようになりました。
    + バグの修正: プロファイリング中の部分的な障害についてクライアントに通知します
    + azureml-core のヨーロッパ スタイルの float 処理を追加しました。
    + Azure ml sdk でワークスペースのプライベート リンク機能が有効になりました。
    + `from_delimited_files` を使用して TabularDataset を作成する場合、ブール型の引数 `empty_as_string` を設定して、空の値を None として読み込むか、空の文字列として読み込むかを指定できます。
    + データ セットのヨーロッパ スタイルの float 処理を追加しました。
    + データ セットのマウントのエラーに関するエラーメッセージが改善されました。
  + **azureml-datadrift**
    + SDK からのデータ ドリフトの結果クエリに、特徴量の最小値、最大値、平均値メトリックを区別しないバグがあり、その結果、重複する値が生成されていました。 メトリック名にターゲットかベースラインをプレフィックスとして付けることで、このバグを修正しました。 修正前: 最小値、最大値、平均値を複製。 修正後: target_min、target_max、target_mean、baseline_min、baseline_max、baseline_mean。
  + **azureml-dataprep**
    + データ配信に必要な .NET 依存関係を確認する際の、書き込み制限付き Python 環境の処理を改善しました。
    + 先頭に空のレコードを含むファイルでのデータフローの作成を修正しました。
    + `to_pandas_dataframe` に類似した `to_partition_iterator` のエラー処理オプションが追加されました。
  + **azureml-interpret**
    + Windows の制限を超える可能性を減らすために、説明パスの長さ制限を引き下げました
    + 線形サロゲートモデルを使用して Mimic Explainer で作成されたスパースの説明のバグを修正しました。
  + **azureml-opendatasets**
    + MNIST の列が文字列として解析される問題を修正します (int である必要があります)。
  + **azureml-pipeline-core**
    + ModuleStep に埋め込まれているモジュールを使用する場合に、出力を再生成できるオプションが許可されました。
  + **azureml-train-automl-client**
    + AutoML の Tensorflow モデルを非推奨にしました。
    + ローカル モードでユーザーがサポートされていないアルゴリズムをホワイトリストに登録できるバグを修正しました
    + AutoMLConfig のドキュメントを修正しました。
    + AutoMLConfig の cv_split_indices 入力にデータ型チェックを適用しました。
    + show_output での AutoML 実行に関する問題を修正しました
  + **azureml-train-automl-runtime**
    + モデル ダウンロードのタイムアウトの正常な動作を妨げていたアンサンブル イテレーションのバグを修正しました。
  + **azureml-train-core**
    + azureml.train.dnn.Nccl クラスのタイポを修正しました。
    + PyTorch Estimator での PyTorch バージョン 1.5 のサポート
    + トレーニング フレームワーク推定器を使用すると、Fairfax リージョンでフレームワーク イメージをフェッチできないという問題を修正しました

  
## <a name="2020-05-04"></a>2020-05-04
**新しいノートブック エクスペリエンス**

Azure Machine Learning の Studio Web エクスペリエンス内で、機械学習のノートブックとファイルを直接作成、編集、共有できるようになりました。 これらのノートブック内から、[Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) で利用可能なすべてのクラスとメソッドを使用できます。[こちら](https://docs.microsoft.com/azure/machine-learning/how-to-run-jupyter-notebooks)から始めてください

**導入された新機能:**

+ VS Code によって使用される改良されたエディター (Monaco Editor) 
+ UI/UX の機能強化
+ セル ツールバー
+ 新しいノートブック ツールバーとコンピューティング コントロール
+ ノートブックのステータス バー 
+ インライン カーネル切り替え
+ R のサポート
+ アクセシビリティとローカライズの機能強化
+ コマンド パレット
+ 追加のキーボード ショートカット
+ 自動保存
+ パフォーマンスと信頼性の向上

Studio から、次の Web ベースの作成ツールにアクセスします。
    
| Web ベースのツール  |     説明  | Edition | 
|---|---|---|
| Azure ML Studio ノートブック   |     ノートブック ファイル用の初のクラス内作成ツールであり、Azure ML Python SDK で利用可能なすべての操作をサポートします。 | Basic および Enterprise  |   

## <a name="2020-04-27"></a>2020-04-27

### <a name="azure-machine-learning-sdk-for-python-v140"></a>Azure Machine Learning SDK for Python v1.4.0

+ **新機能**
  + AmlCompute クラスターで、プロビジョニング時にクラスターのマネージド ID の設定がサポートされるようになりました。 システムによって割り当てられた ID とユーザーによって割り当てられた ID のどちらを使用するかを指定するだけです。後者の場合は ID を渡します。 次に、AmlCompute で現在採用されているトークンベースのアプローチではなく、データに安全にアクセスするためにコンピューティングの ID を使用する方法で、アクセス許可を設定して、ストレージや ACR などのさまざまなリソースにアクセスできます。 パラメーターの詳細については、SDK リファレンスを参照してください。
  

+ **重大な変更**
  + AmlCompute クラスターで、実行ベースの作成に関するプレビュー機能がサポートされましたが、2 週間後に非推奨になる予定です。 Amlcompute クラスを使用すると、常に永続的なコンピューティング ターゲットを作成し続けることができますが、実行構成でコンピューティング ターゲットとして識別子 "amlcompute" を指定するこの特殊なアプローチは、近い将来サポートされなくなります。 

+ **バグの修正と機能強化**
  + **azureml-automl-runtime**
    + 列内の一意の値の数を計算するときに、ハッシュ化できない型のサポートを有効にします。
  + **azureml-core**
    + TabularDataset を使用して Azure Blob Storage から読み取るときの安定性が向上しました。
    + `Datastore.register_azure_blob_store` の `grant_workspace_msi` パラメーターについてのドキュメントが改善されました。
    + `/` または `\` で終わる `src_dir` 引数をサポートする `datastore.upload` のバグを修正しました。
    + アクセス キーまたは SAS トークンがない Azure Blob Storage データトアにアップロードしようとしたときの実用的なエラーメッセージが追加されました。
  + **azureml-interpret**
    + アップロードされた説明について、視覚化データのファイル サイズに上限を追加しました。
  + **azureml-train-automl-client**
    + AutoMLConfig の label_column_name および weight_column_name パラメーターが文字列型であることを明示的にチェックします。
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep は、パイプライン パラメーターとしてデータセットをサポートするようになりました。 ユーザーは、サンプル データセットを使用してパイプラインを作成できます。また、新しいパイプラインの実行用に同じ種類 (ファイルまたはテーブル) の入力データセットを変更できます。

  
## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>Azure Machine Learning SDK for Python v1.3.0

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + トレーニング後の操作に関するテレメトリが追加されました。
    + 100 より長いとき、CSS (Conditional Sum of Squares、条件付き平方和) を利用し、自動 ARIMA トレーニングを高速化します。 使用される長さは、/src/azureml-automl-core/azureml/automl/core/shared/constants.py で、TimeSeriesInternal クラス内に定数 ARIMA_TRIGGER_CSS_TRAINING_LENGTH として保存されることにご留意ください
    + 予測実行のユーザー ログが改善されました。現在実行されている段階に関してログに表示される情報が増えました
    + target_rolling_window_size を 2 未満の値に設定することが禁止されました
  + **azureml-automl-runtime**
    + 重複するタイムスタンプが見つかったときに表示されるエラー メッセージが改善されました。
    + target_rolling_window_size を 2 未満の値に設定することが禁止されました。
    + 遅延を補完できない問題を解消しました。 この問題は、連続を期間別に分解するために必要となる観察数が十分ではないことが原因でした。 この "期間調整された" データは、遅延の長さを判断する目的で偏自己相関関数 (PACF) を計算するために使用されます。
    + 特徴付け構成による予測タスクのために、列の目的の特徴付けカスタマイズを有効にしました。予測タスクの列の目的として数値別とカテゴリ別を使用できるようになりました。
    + 特徴付け構成による予測タスクのために、列の削除の特徴付けカスタマイズを有効にしました。
    + 特徴付け構成による予測タスクのために、補完カスタマイズを有効にしました。ターゲット列の定数値補完と、データをトレーニングするための平均値、中央値、最頻値、定数値補完がサポートされるようになりました。
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig に渡す文字列計算名を受け取ります
  + **azureml-core**
    +  Environment オブジェクトのコピーを作成する目的で Environment.clone(new_name) API を追加しました
    +  Environment.docker.base_dockerfile でファイルパスを受け取ります。 ファイルを解決できる場合、コンテンツは base_dockerfile 環境プロパティに読み込まれます
    + ユーザーが Environment.docker で値を手動設定するとき、base_image と base_dockerfile の相互に排他的な値が自動的にリセットされます
    + 環境がユーザーまたは AzureML のどちらによって管理されているかを示す user_managed フラグが RSection に追加されました。
    + データセット: データ パスに Unicode 文字が含まれる場合、データセットをダウンロードできない問題を修正しました。
    + データセット: Azure Machine Learning コンピューティングの最小ディスク容量要件を満たすよう、データセット マウント キャッシュ メカニズムが改善されました。ノードを使用できなくなったり、ジョブがキャンセルされたりする事態を回避します。
    + データセット: pandas データフレームとしての時系列データセットにアクセスすると、時系列の列にインデックスが追加されます。このインデックスは、時系列ベースのデータへのアクセスを高速化する目的で使用されます。  以前は、このインデックスにタイムスタンプ列と同じ名前が与えられていました。それだと、どちらが現在のタイムスタンプ列でどちらがインデックスなのか、ユーザーを混乱させます。 インデックスは列として使用するべきではないため、今後は特定の名前を付けることはありません。 
    + データセット: ソブリン クラウドのデータセット認証問題を修正しました。
    + データセット: Azure PostgreSQL データストアから作成されたデータセットの `Dataset.to_spark_dataframe` エラーを修正しました。
  + **azureml-interpret**
    + ローカルの重要度値が少ない場合、視覚化にグローバル スコアを追加しました
    + azureml-interpret が interpret-community 0.9 に更新されました。*
    + 評価データが少ない説明をダウンロードできない問題を修正しました
    + AutoML で形式データが少ない説明オブジェクトのサポートを追加しました
  + **azureml-pipeline-core**
    + パイプラインの計算ターゲットとして ComputeInstance をサポート
  + **azureml-train-automl-client**
    + トレーニング後の操作に関するテレメトリが追加されました。
    + 早期停止時の回帰を修正しました
    + 入力データの有効な種類として azureml.dprep.Dataflow を非推奨にしました。
    +  AutoML の既定の実験タイムアウトを 6 日間に変更。
  + **azureml-train-automl-runtime**
    + トレーニング後の操作に関するテレメトリが追加されました。
    + データの少ない AutoML e2e のサポートを追加しました
  + **azureml-opendatasets**
    + サービス モニターのテレメトリを追加しました。
    + 安定性を上げるために BLOB のフロント ドアが有効になります 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>Azure Machine Learning SDK for Python v1.2.0

+ **重大な変更**
  + Python 2.7 のサポートは終了します

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + CLI の `az ml model/computetarget/service` コマンドに "--subscription-id" が追加されます
    + ACI デプロイ用にカスタマーマネージド キー (CMK) vault_url、key_name、および key_version を渡すためのサポートが追加されます
  + **azureml-automl-core** 
    + X と Y の両方のデータ予測タスクに対して、定数値を使用したカスタマイズされた補完を使用できるようになりました。
    + ユーザーに対するエラー メッセージの表示に関する問題が修正されました。    
  + **azureml-automl-runtime**
    + 1 行のみのグレインが含まれるデータ セットの予測に関する問題が修正されました
    + 予測タスクに必要なメモリ量が少なくなりました。
    + 時刻列の形式が正しくない場合の、より適切なエラー メッセージが追加されました。
    + X と Y の両方のデータ予測タスクに対して、定数値を使用したカスタマイズされた補完を使用できるようになりました。
  + **azureml-core**
    + 次の環境変数から ServicePrincipal を読み込むためのサポートが追加されました: AZUREML_SERVICE_PRINCIPAL_ID、AZUREML_SERVICE_PRINCIPAL_TENANT_ID、および AZUREML_SERVICE_PRINCIPAL_PASSWORD
    + 新しいパラメーター `support_multi_line` が `Dataset.Tabular.from_delimited_files` に導入されました。既定 (`support_multi_line=False`) では、すべての改行 (引用符で囲まれたフィールド値内のものも含む) がレコード区切りとして解釈されます。 この方法でデータを読み込むと、複数の CPU コアでの並列実行で速度と最適化が向上します。 ただし、フィールド値が不整合なレコードが警告なしで多数生成される可能性があります。 引用符で囲まれた改行が区切りファイルに含まれていることがわかっている場合は、これを `True` に設定する必要があります。
    + Azure Machine Learning CLI に ADLS Gen2 を登録する機能が追加されました
    + パラメーターの使用方法をより正確に反映するため、TabularDataset の with_timestamp_columns() メソッドのパラメーター 'fine_grain_timestamp' の名前が 'timestamp' に変更され、パラメーター 'coarse_grain_timestamp' の名前が 'partition_timestamp' に変更されました。
    + 実験名の最大長が 255 に増加されました。
  + **azureml-interpret**
    + azureml-interpret が interpret-community 0.7 に更新されました。*
  + **azureml-sdk**
    + プレリリース版と安定版リリースでの修正プログラムの適用をサポートするために、互換バージョンの Tilde での依存関係に変更されます。


## <a name="2020-03-11"></a>2020-03-11

### <a name="azure-machine-learning-sdk-for-python-v115"></a>Azure Machine Learning SDK for Python v1.1.5

+ **機能の非推奨**
  + **Python 2.7**
    + Python 2.7 をサポートする最後のバージョンです

+ **重大な変更**
  + **セマンティック バージョニング 2.0.0**
    + バージョン 1.1 以降、Azure ML Python SDK では、セマンティック バージョニング 2.0.0 が採用されています。 詳細については、[こちら](https://semver.org/)を参照してください。 以降のすべてのバージョンは、新しい番号付けスキームとセマンティック バージョニング コントラクトに従います。 

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + 一貫性を保つために、エンドポイントの CLI コマンド名が 'az ml endpoint aks' から 'az ml endpoint real time' に変更されます。
    + 安定した実験用ブランチ CLI の CLI インストール手順を更新します
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
  + **azureml-automl-core**
    + AutoML ONNX モデルのバッチ モード推論 (複数行を 1 回取得) が有効になりました
    + データ セットの周波数の検出、データの不足、または不規則なデータ ポイントの検出の改善
    + 主要な周波数に準拠していないデータ ポイントを削除する機能が追加されました。
    + 対応する列の補完オプションを適用するためのオプション一覧を取得するように、コンストラクターの入力を変更しました。
    + エラーのログ記録が改善されました。
  + **azureml-automl-runtime**
    + トレーニング セットに存在しないグレインがテスト セットに含まれていた場合にスローされるエラーの問題を修正しました
    + 予測サービスでのスコアリング中に y_query 要件が削除されました
    + データセットに長い時間差の短いグレインが含まれている場合の予測に関する問題を修正しました。
    + 自動最大期間が有効になっていて、日付列に文字列形式の日付が含まれている場合の問題を修正しました。 日付への変換ができない場合の、適切な変換およびエラーのメッセージを追加しました
    + FileCacheStore の中間データのシリアル化と逆シリアル化にネイティブの NumPy と SciPy を使用します (ローカル AutoML の実行に使用)
    + 失敗した子の実行が実行状態のままになるバグを修正しました。
    + 特性付けの速度が向上しました。
    + スコアリング中の頻度チェックを修正しました。予測タスクでは、トレーニングとテスト セット間で頻度が厳密に同一である必要はなくなりました。
    + 対応する列の補完オプションを適用するためのオプション一覧を取得するように、コンストラクターの入力を変更しました。
    + ラグの種類の選択に関連するエラーを修正しました。
    + データ セットで発生した未分類エラーを修正し、粒度を単一行にしました
    + 周波数検出のパフォーマンスが低下する問題を修正しました。
    + トレーニング エラーの実際の原因が AttributeError に置き換えられる、AutoML 例外処理のバグを修正します。
  + **azureml-cli-common**
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
  + **azureml-contrib-mir**
    + アクセス トークンを取得する機能を MirWebservice クラスに追加
    + MirWebservice.run() の呼び出し中、MirWebservice に対して既定ではトークン認証を使用し、呼び出しが失敗した場合のみ更新します
    + Mir webservice のデプロイでは、[Ds2v2、A2v2、および F16] ではなく、それぞれ適切な SKU である [Standard_DS2_v2、Standard_F16、Standard_A2_v2] が必要になりました。
  + **azureml-contrib-pipeline-steps**
    + 省略可能なパラメーター side_inputs が ParallelRunStep に追加されました。 このパラメーターを使用して、コンテナーにフォルダーをマウントできます。 現在サポートされている型は、DataReference と PipelineData です。
    + ParallelRunConfig で渡されたパラメーターが、パイプライン パラメーターを渡すことによって上書きできるようになりました。 サポートされる新しいパイプライン パラメーターは、aml_mini_batch_size、aml_error_threshold、aml_logging_level、aml_run_invocation_timeout (aml_node_count と aml_process_count_per_node は以前のリリースに既に含まれています) です。
  + **azureml-core**
    + デプロイされた AzureML Webservices は、既定値で `INFO`ログになります。 これは、デプロイされたサービスで `AZUREML_LOG_LEVEL` 環境変数を設定することによってコントロールできます。
    + Python sdk は、検出サービスを使用して「パイプライン」の代わりに 「api」エンドポイントを使用します。
    + すべての SDK 呼び出しの新しいルートにスワップします。
    + ModelManagementService への呼び出しのルート指定を新しい統合構造体に変更しました。
      + ワークスペースの更新メソッドを公開しました。
      + ユーザーがイメージ ビルドの計算を更新できるように、ワークスペース更新メソッドに image_build_compute パラメーターを追加しました。
    + 古いプロファイル ワークフローに非推奨のメッセージを追加しました。 プロファイルの CPU とメモリの制限を修正しました。
    + R ジョブを実行する環境の一部として RSection を追加しました。
    + データセットのソースにアクセスできない場合、またはデータが含まれていない場合にエラーを発生させるために、`Dataset.mount` に検証を追加しました。
    + Azure BLOB コンテナーを登録するためのデータストア CLI の追加パラメーターとして `--grant-workspace-msi-access` を追加しました。これにより、VNet の背後にある BLOB コンテナーを登録できるようになります。
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
    + aks.py _deploy の問題を修正しました。
    + サイレント ストレージ障害を回避するために、アップロードされるモデルの整合性が検証されます。
    + ユーザーは、Web サービス用のキーを再生成するときに、認証キーの値を指定できるようになりました。
    + データセットの入力名として大文字を使用できないバグを修正しました。
  + **azureml-defaults**
    + `azureml-dataprep` は `azureml-defaults` の一部としてインストールされるようになりました。 データセットをマウントするためにコンピューティング先に data prep[fuse] を手動でインストールする必要がなくなりました。
  + **azureml-interpret**
    + azureml-interpret を interpret-community 0.6 へと更新しました。*
    + azureml-interpret が interpret-community 0.5.0 に依存するように更新されました
    + azureml-interpret に azureml-style 例外を追加しました
    + keras モデルの DeepScoringExplainer シリアル化を修正しました
  + **azureml-mlflow**
    + ソブリン クラウドのサポートを azureml.mlflow に追加
  + **azureml-pipeline-core**
    + パイプライン バッチ スコアリング ノートブックで ParallelRunStep が使用されるようになりました
    + 引数リストを変更しても PythonScriptStep の結果が誤って再利用される可能性のあるバグを修正しました
    + `PipelineOutputFileDataset` で parse_* メソッドを呼び出すときに、列の型を設定する機能を追加しました
  + **azureml-pipeline-steps**
    + `AutoMLStep` を `azureml-pipeline-steps` パッケージに移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
    + PythonScriptStep 入力としてのデータセットのドキュメント例を追加しました
  + **azureml-tensorboard**
    + azureml-tensorboard を更新して、tensorflow 2.0 をサポートしました
    + コンピューティング インスタンスでカスタムの Tensorboard ポートを使用するときに、正しいポート番号を表示します
  + **azureml-train-automl-client**
    + リモート実行で特定のパッケージが正しくないバージョンでインストールされる可能性がある問題を修正しました。
    + カスタムの特性付け構成をフィルター処理する FeaturizationConfig のオーバーライドの問題を修正しました。
  + **azureml-train-automl-runtime**
    + リモート実行での周波数の検出に関する問題を修正
    + `azureml-pipeline-steps` パッケージ内の `AutoMLStep` を移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
  + **azureml-train-core**
    + PyTorch Estimator での PyTorch バージョン 1.4 のサポート
  
## <a name="2020-03-02"></a>2020 年 03 月 02 日

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>Azure Machine Learning SDK for Python v1.1.2rc0 (プレリリース)

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + AutoML ONNX モデルのバッチ モード推論 (複数行を 1 回取得) が有効になりました
    + データ セットの周波数の検出、データの不足、または不規則なデータ ポイントの検出の改善
    + 主要な周波数に準拠していないデータ ポイントを削除する機能が追加されました。
  + **azureml-automl-runtime**
    + トレーニング セットに存在しないグレインがテスト セットに含まれていた場合にスローされるエラーの問題を修正しました
    + 予測サービスでのスコアリング中に y_query 要件が削除されました
  + **azureml-contrib-mir**
    + アクセス トークンを取得する機能を MirWebservice クラスに追加
  + **azureml-core**
    + デプロイされた AzureML Webservices は、既定値で `INFO`ログになります。 これは、デプロイされたサービスで `AZUREML_LOG_LEVEL` 環境変数を設定することによってコントロールできます。
    + ワークスペースに登録されているすべてのデータ セットを返すために `Dataset.get_all` の繰り返しを修正しました。
    + データセット作成 API の `path` 引数に無効な型が渡されると、エラー メッセージが改善されます。
    + Python sdk は、検出サービスを使用して「パイプライン」の代わりに 「api」エンドポイントを使用します。
    + すべての SDK 呼び出しの新しいルートにスワップする
    + ModelManagementService への呼び出しのルート指定を新しい統合構造体に変更します。
      + ワークスペースの更新メソッドを公開しました。
      + ユーザーがイメージ ビルドの計算を更新できるように、ワークスペース更新メソッドに image_build_compute パラメーターを追加
    +  古いプロファイル ワークフローに非推奨のメッセージを追加しました。 プロファイルの cpu とメモリの制限を修正
  + **azureml-interpret**
    + azureml-interpret から interpret-community 0.6 に更新されました。*
  + **azureml-mlflow**
    + ソブリン クラウドのサポートを azureml.mlflow に追加
  + **azureml-pipeline-steps**
    + `AutoMLStep` を `azureml-pipeline-steps package`に移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
  + **azureml-train-automl-client**
    + リモート実行で特定のパッケージが正しくないバージョンでインストールされる可能性がある問題を修正しました。
  + **azureml-train-automl-runtime**
    + リモート実行での周波数の検出に関する問題を修正
    + `AutoMLStep` を `azureml-pipeline-steps package`に移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
  + **azureml-train-core**
    + `AutoMLStep` を `azureml-pipeline-steps package`に移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。

## <a name="2020-02-18"></a>2020-02-18

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>Azure Machine Learning SDK for Python v1.1.1rc0 (プレリリース)

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
  + **azureml-automl-core**
    + エラーのログ記録が改善されました。
  + **azureml-automl-runtime**
    + データセットに長い時間差の短いグレインが含まれている場合の予測に関する問題を修正しました。
    + 自動最大期間が有効になっていて、日付列に文字列形式の日付が含まれている場合の問題を修正しました。 日付への変換ができない場合の適切な変換と適切なエラーを追加しました
    + FileCacheStore の中間データのシリアル化と逆シリアル化にネイティブの NumPy と SciPy を使用します (ローカル AutoML の実行に使用)
    + 失敗した子の実行が実行状態のままになるバグを修正しました。
  + **azureml-cli-common**
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
  + **azureml-core**
    + Azure BLOB コンテナーを登録するためのデータストア CLI の追加パラメーターとして `--grant-workspace-msi-access` を追加しました。これにより、VNet の背後にある BLOB コンテナーを登録できるようになります
    + 単一インスタンスのプロファイリングが、レコメンデーションを生成するために修正され、コアの SDK で使用できるようになりました。
    + aks.py _deploy の問題を修正しました
    + サイレント ストレージ障害を回避するために、アップロードされるモデルの整合性が検証されます。
  + **azureml-interpret**
    + azureml-interpret に azureml-style 例外を追加しました
    + keras モデルの DeepScoringExplainer シリアル化を修正しました
  + **azureml-pipeline-core**
    + パイプライン バッチ スコアリング ノートブックで ParallelRunStep が使用されるようになりました
  + **azureml-pipeline-steps**
    + `azureml-pipeline-steps` パッケージ内の `AutoMLStep` を移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
  + **azureml-contrib-pipeline-steps**
    + 省略可能なパラメーター side_inputs が ParallelRunStep に追加されました。 このパラメーターを使用して、コンテナーにフォルダーをマウントできます。 現在サポートされている型は、DataReference と PipelineData です。
  + **azureml-tensorboard**
    + azureml-tensorboard を更新して、tensorflow 2.0 をサポートしました
  + **azureml-train-automl-client**
    + カスタムの特性付け構成をフィルター処理する FeaturizationConfig のオーバーライドの問題を修正しました。
  + **azureml-train-automl-runtime**
    + `azureml-pipeline-steps` パッケージ内の `AutoMLStep` を移動しました。 `azureml-train-automl-runtime` 内の `AutoMLStep` は非推奨になりました。
  + **azureml-train-core**
    + PyTorch Estimator での PyTorch バージョン 1.4 のサポート
  
## <a name="2020-02-04"></a>2020-02-04

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>Azure Machine Learning SDK for Python v1.1.0rc0 (プレリリース)

+ **重大な変更**
  + **セマンティック バージョニング 2.0.0**
    + バージョン 1.1 以降、Azure ML Python SDK では、セマンティック バージョニング 2.0.0 が採用されています。 詳細については、[こちら](https://semver.org/)を参照してください。 以降のすべてのバージョンは、新しい番号付けスキームとセマンティック バージョニング コントラクトに従います。 
  
+ **バグの修正と機能強化**
  + **azureml-automl-runtime**
    + 特性付けの速度が向上しました。
    + スコアリング中の頻度チェックを修正しました。現在、予測タスクでは、トレーニングとテストのセット間で頻度が厳密に同一である必要はありません。
  + **azureml-core**
    + ユーザーは、Web サービス用のキーを再生成するときに、認証キーの値を指定できるようになりました。
  + **azureml-interpret**
    + azureml-interpret が interpret-community 0.5.0 に依存するように更新されました
  + **azureml-pipeline-core**
    + 引数リストを変更しても PythonScriptStep の結果が誤って再利用される可能性のあるバグを修正しました
  + **azureml-pipeline-steps**
    + PythonScriptStep 入力としてのデータセットのドキュメント例を追加しました
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig で渡されたパラメーターが、パイプライン パラメーターを渡すことによって上書きできるようになりました。 サポートされる新しいパイプライン パラメーターは、aml_mini_batch_size、aml_error_threshold、aml_logging_level、aml_run_invocation_timeout (aml_node_count と aml_process_count_per_node は以前のリリースに既に含まれています) です。
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>Azure Machine Learning SDK for Python v1.0.85

+ **新機能**
  + **azureml-core**
    + 特定のワークスペースとサブスクリプションで AmlCompute リソースの現在のコア使用率とクォータ制限を取得する
  
  + **azureml-contrib-pipeline-steps**
    + 前の手順から parallelrunstep への中間結果として表形式データセットを渡すことをユーザーに許可する

+ **バグの修正と機能強化**
  + **azureml-automl-runtime**
    + デプロイされた予測サービスへの要求で y_query 列の要件を削除しました。 
    + 'y_query' が、Dominick のオレンジジュース ノートブックのサービス リクエスト セクションから削除されました。
    + デプロイされたモデルでの予測を妨げ、日付と時刻の列を含むデータセットを操作するバグを修正しました。
    + 二項分類と多クラス分類の両方において、マシューズ相関係数を分類メトリックとして追加しました。
  + **azureml-contrib-interpret**
    + テキストの説明が、間もなくリリースされる interpret-text リポジトリに移動されたため、azureml-contrib-interpret から Text Explainer が削除されました。
  + **azureml-core**
    + データセット: ファイル データセットの使用は、python env にインストールされる numpy と pandas に依存しなくなりました。
    + 正常性エンドポイントに ping を実行する前にローカル Docker コンテナーの状態を確認するよう、LocalWebservice.wait_for_deployment() を変更しました。これにより、失敗したデプロイの報告にかかる時間を大幅に短縮しました。
    + LocalWebservice.reload() コンストラクターを使用して、既存のデプロイからサービス オブジェクトが作成されるときに、LocalWebservice() で使用される内部プロパティの初期化を修正しました。
    + より明確にするため、エラー メッセージを編集しました。
    + get_access_token という新しいメソッドを AksWebservice に追加しました。このメソッドには、アクセス トークン、タイムスタンプの後の更新、タイムスタンプの有効期限、およびトークンの種類が含まれている AksServiceAccessToken オブジェクトが返されます。 
    + 新しいメソッドは、AksWebservice の既存の get_token () メソッドが返すすべての情報を返すため、非推奨としました。
    + az ml サービス get-access-token コマンドの出力が変更されました。 トークンの名前を accessToken に変更し、refreshBy を refreshAfter に変更しました。 expiryOn および tokenType プロパティが追加されました。
    + Fixed get_active_runs
  + **azureml-explain-model**
    + shap を 0.33.0 に更新し、interpret-community を 0.4 に変更しました。*
  + **azureml-interpret**
    + shap を 0.33.0 に更新し、interpret-community を 0.4 に変更しました。*
  + **azureml-train-automl-runtime**
    + 二項分類と多クラス分類の両方において、マシューズ相関係数を分類メトリックとして追加しました。
    + コードから前処理フラグを廃止し、特性付けに置き換えました。特性付けは既定でオンになっています

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>Azure Machine Learning SDK for Python v1.0.83

+ **新機能**
  + データセット: `to_pandas_dataframe` に対して `on_error` および `out_of_range_datetime` の2 つのオプションを追加します。データにエラーがある場合は、`None` を使用して入力するのではなく、エラーが発生します。
  + ワークスペース:機密データを含むワークスペースの `hbi_workspace` フラグが追加されました。これにより、さらに暗号化したり、ワークスペースの高度な診断を無効にしたりできます。 また、ワークスペースの作成時に `cmk_keyvault` パラメーターと `resource_cmk_uri` パラメーターを指定することによって、関連付けられている Cosmos DB インスタンスに対して独自のキーを取り込むためのサポートを追加しました。これにより、ワークスペースのプロビジョニングの再にサブスクリプションに Cosmos DB インスタンスが作成されます。 [詳細については、こちらを参照してください。](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **バグの修正と機能強化**
  + **azureml-automl-runtime**
    + バージョン 3.5.4 より下の Python でAutoML を実行するときに TypeError が発生する原因となった回帰を修正しました。
  + **azureml-core**
    + `./` で開始されていない相対パスを使用できない `datastore.upload_files` のバグを修正しました。
    + すべての Image クラスのコードパスについて非推奨のメッセージを追加しました。
    + Azure China 21Vianet リージョンのモデル管理 URL の構成を修正しました。
    + source_dir を使用するモデルを Azure Functions 用にパッケージ化できない問題を修正しました。    
    + イメージを AzureML ワークスペース コンテナー レジストリにプッシュするためのオプションを [Environment.build_local()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)に追加しました。
    + Azure Synapse の新しいトークン ライブラリを後方互換性のある方法で使用するように SDK を更新しました。
  + **azureml-interpret**
    + ダウンロードに対する説明が使用できなかったときに None が返されるバグを修正しました。 例外を発生させ、それ以外では動作が一致します。
  + **azureml-pipeline-steps**
    + `Estimator` が `EstimatorStep` で使用される場合、`DatasetConsumptionConfig` を `Estimator` の `inputs` パラメーターに渡すことはできません。
  + **azureml-sdk**
    + azureml-sdk パッケージに AutoML クライアントを追加しました。完全な AutoML パッケージをインストールしなくても、リモート AutoML の実行をサブミットすることができます。
  + **azureml-train-automl-client**
    + AutoML 実行用のコンソール出力の配置を修正しました
    + リモートの amlcompute に正しくないバージョンの pandas がインストールされる可能性があるバグを修正しました。

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>Azure Machine Learning SDK for Python v1.0.81

+ **バグの修正と機能強化**
  + **azureml-contrib-interpret**
    + interpret-community に対する shap の依存関係を azureml-interpret から保留します
  + **azureml-core**
    + コンピューティング先を、対応するデプロイ構成オブジェクトのパラメーターとして指定できるようになりました。 これは、具体的には、SDK オブジェクトではなく、デプロイ先となるコンピューティング先の名前です。
    + Model および Service オブジェクトに CreatedBy 情報を追加しました。 <var>.created_by でアクセスできます。
    + Docker コンテナーの HTTP ポートを正しく設定していなかった ContainerImage.run() を修正しました。
    + `az ml dataset register` CLI コマンドの `azureml-dataprep` を省略可能にします
    + `TabularDataset.to_pandas_dataframe` が代替リーダーに誤ってフォールバックし、警告を出力するバグを修正しました。
  + **azureml-explain-model**
    + interpret-community に対する shap の依存関係を azureml-interpret から保留します
  + **azureml-pipeline-core**
    + パイプラインのステップとしてローカル ノートブックを実行するための新しいパイプライン ステップ `NotebookRunnerStep` を追加しました。
    + PublishedPipelines、Schedules、および PipelineEndpoints の非推奨の get_all 関数を削除しました。
  + **azureml-train-automl-client**
    + AutoML への入力として data_script の非推奨を開始しました。


## <a name="2019-12-09"></a>2019-12-09

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>Azure Machine Learning SDK for Python v1.0.79

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + featurizationConfig をログの記録から除外しました
      + "auto"/"off"/"customized" のみを記録するようにログ方式を更新しました。
  + **azureml-automl-runtime**
    + 列のデータ型を検出するため、pandas. Series と pandas. Categorical のサポートを追加しました。 以前は numpy.ndarray のみがサポートされていました
      + カテゴリ型 dtype を正しく処理するための関連するコード変更を追加しました。
    + 予測関数インターフェイスが改善されました。y_pred パラメーターが省略可能になりました。 -docstring が改善されました。
  + **azureml-contrib-dataset**
    + ラベルが付けられたデータセットをマウントできないバグを修正しました。
  + **azureml-core**
    + `Environment.from_existing_conda_environment(name, conda_environment_name)` のバグ修正。 ユーザーは、ローカル環境の完全なレプリカである環境のインスタンスを作成できます
    + 時系列に関連するデータセット メソッドが既定で `include_boundary=True` に変更されました。
  + **azureml-train-automl-client**
    + show output が false に設定されている場合に検証結果が出力されない問題を修正しました。


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>Azure Machine Learning SDK for Python v1.0.76

+ **重大な変更**
  + Azureml-Train-AutoML のアップグレードに関する問題
    + 1\.0.76 よりも前の azureml-train-automl 1.0.76 から1.0.76 以降の azureml-train-automl にアップグレードすると、部分的なインストールが発生し、一部の AutoML のインポートが失敗する可能性があります。 これを解決するには、 https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmd で見つかったセットアップ スクリプトを実行します。 または、pip を直接使用している場合は、以下を実行することができます。
      + "pip install --upgrade azureml-train-automl"
      + "pip install --ignore-installed azureml-train-automl-client"
    + または、アップグレードする前に古いバージョンをアンインストールすることもできます。
      + "pip uninstall azureml-train-automl"
      + "pip install azureml-train-automl"

+ **バグの修正と機能強化**
  + **azureml-automl-runtime**
    + AutoML では、二項分類タスクの平均スカラーメトリックを計算するときに、true と false の両方のクラスが考慮されるようになりました。
    + AzureML-AutoML-Core の機械学習とトレーニング コードを新しいパッケージ AzureML-AutoML-Runtime に移動しました。
  + **azureml-contrib-dataset**
    + ダウンロード オプションを使用してラベル付きのデータセットで `to_pandas_dataframe` を呼び出すと、既存のファイルを上書きするかどうかを指定できるようになりました。
    + time series、label、または image 列が削除される `keep_columns` または `drop_columns` を呼び出すと、データセットの対応する機能も削除されます。
    + 物体検出タスク用の PyTorch ローダーに関する問題を修正しました。
  + **azureml-contrib-interpret**
    + azureml-contrib-interpret から説明ダッシュボード ウィジェットを削除しました、interpret_community の新しいものを参照するようにパッケージを変更しました
    + interpret-community のバージョンを 0.2.0 に更新しました
  + **azureml-core**
    + `workspace.datasets` のパフォーマンスが向上します。
    + ユーザー名とパスワード認証を使用して Azure SQL Database データストアを登録する機能が追加されました。
    + 相対パスから RunConfigurations を読み込むように修正します。
    + time series 列が削除される `keep_columns` または `drop_columns` を呼び出すと、データセットの対応する機能も削除されます。
  + **azureml-interpret**
    + interpret-community のバージョンを 0.2.0 に更新しました
  + **azureml-pipeline-steps**
    + Azure Machine Learning パイプラインの手順の `runconfig_pipeline_params` でサポートされている値を記載しました。
  + **azureml-pipeline-core**
    + パイプライン コマンドの json 形式で出力をダウンロードするための CLI オプションが追加されました。
  + **azureml-train-automl**
    + AzureML-Train-AutoML を 2 つのパッケージに分割します (クライアント パッケージ AzureML-Train-AutoML-Client と ML トレーニング パッケージ AzureML-Train-AutoML-Runtime)
  + **azureml-train-automl-client**
    + 機械学習の依存関係をローカルにインストールすることなく、AutoML 実験を送信できるシン クライアントが追加されました。
    + リモート実行で自動的に検出されたラグ、ローリング ウィンドウ サイズ、最大期間のログ記録を修正しました。
  + **azureml-train-automl-runtime**
    + 新しい AutoML パッケージを追加して、機械学習とランタイム コンポーネントをクライアントから分離しました。
  + **azureml-contrib-train-rl**
    + SDK に強化学習サポートが追加されました。
    + RL SDK に AmlWindowsCompute サポートが追加されました。


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>Azure Machine Learning SDK for Python v1.0.74

  + **プレビュー機能**
    + **azureml-contrib-dataset**
      + azureml-contrib-dataset をインポートすると、`._Labeled` の代わりに `Dataset.Labeled.from_json_lines` を呼び出して、ラベル付きのデータセットを作成できます。
      + ダウンロード オプションを使用してラベル付きのデータセットで `to_pandas_dataframe` を呼び出すと、既存のファイルを上書きするかどうかを指定できるようになりました。
      + time series、label、または image 列が削除される `keep_columns` または `drop_columns` を呼び出すと、データセットの対応する機能も削除されます。
      + `dataset.to_torchvision()` を呼び出すときの PyTorch ローダーの問題が修正されました。

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + プレビュー CLI にモデルのプロファイルが追加されました。
    + AzureML CLI が失敗する原因となる Azure Storage の破壊的変更を修正します。
    + AKS タイプの MLC に Load Balancer タイプが追加されました
  + **azureml-automl-core**
    + 欠損値と複数のグレインを含む、時系列の最大期間の検出に関する問題を修正しました。
    + 交差検証分割の生成時のエラーに関する問題を修正しました。
    + このセクションをマークダウン形式のメッセージと置き換え、リリース ノートに表示されようにします。- 予測データセットの短いグレインが処理しやすくなりました。
    + ログ記録時に一部のユーザー情報がマスキングされる問題を修正しました。 -予測実行中のエラーのログ記録が改善されました。
    + conda 依存関係としての psutil を自動生成された yml デプロイ ファイルに追加します。
  + **azureml-contrib-mir**
    + AzureML CLI が失敗する原因となる Azure Storage の破壊的変更を修正します。
  + **azureml-core**
    + Azure Functions にデプロイされたモデルによって 500 番台が生成される原因となったバグを修正します。
    + スナップショットに amlignore ファイルが適用されない問題を修正しました。
    + 新しい API amlcompute.get_active_runs が追加されました。この API は、指定された amlcompute で実行するまたは実行キューに入れるジェネレーターを返します。
    + AKS タイプの MLC に Load Balancer タイプが追加されました。
    + run.py の download_files と artifacts_client の download_artifacts_from_prefix に append_prefix ブール パラメーターを追加しました。 このフラグは、元のファイル パスを選択的にフラット化するために使用されます。ファイル名またはフォルダー名のみが output_directory に追加されます
    + データセットの使用状況に関する `run_config.yml` の逆シリアル化の問題を修正しました。
    + time series 列が削除される `keep_columns` または `drop_columns` を呼び出すと、データセットの対応する機能も削除されます。
  + **azureml-interpret**
    + interpret-community バージョンを 0.1.0.3 に更新しました
  + **azureml-train-automl**
    + automl_step が検証の問題を出力しない可能性がある問題を修正しました。
    + モデルの環境にローカルで依存関係がない場合でも成功するように register_model を修正しました。
    + 一部のリモート実行で docker が有効になっていない問題を修正しました。
    + ローカル実行が早期に失敗する原因となっている例外のログ記録を追加します。
  + **azureml-train-core**
    + 自動化されたハイパーパラメーターのチューニングの最適な子実行の計算で resume_from の実行を検討します。
  + **azureml-pipeline-core**
    + パイプラインの引数の構築時のパラメーターの処理を修正しました。
    + パイプラインの説明とステップの種類の yaml パラメーターを追加しました。
    + パイプラインステップの新しい yaml 形式と、古い形式の非推奨警告を追加しました。



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Web エクスペリエンス

[https://ml.azure.com](https://ml.azure.com) での共同ワークスペースのランディング ページは、Azure Machine Learning Studio (プレビュー) として強化およびブランド化されています。

この Studio からは、データセット、パイプライン、モデル、エンドポイントなどの Azure Machine Learning の資産をトレーニング、テスト、デプロイ、管理することができます。

Studio から、次の Web ベースの作成ツールにアクセスします。

| Web ベースのツール | 説明 | Edition |
|-|-|-|
| ノートブック VM (プレビュー) | 完全に管理されたクラウドベースのワークステーション | Basic および Enterprise |
| [自動化された機械学習](tutorial-first-experiment-automated-ml.md) (プレビュー) | 機械学習モデルの開発に向けた、コードを使用しないエクスペリエンス | Enterprise |
| [デザイナー](concept-designer.md) (プレビュー) | 以前はデザイナーと呼ばれていた機械学習モデリング ツールをドラッグ アンド ドロップします | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Azure Machine Learning デザイナーの強化

+ 以前はビジュアル インターフェイスと呼ばれていました 
+    11 個の新しい[モジュール](algorithm-module-reference/module-reference.md)には、特徴エンジニアリング、クロス検証、データ変換など、レコメンダー、分類子、トレーニング ユーティリティが含まれています。

### <a name="r-sdk"></a>R SDK 
 
データ サイエンティストと AI 開発者は、[Azure Machine Learning SDK for R](tutorial-1st-r-experiment.md) を使用して、Azure Machine Learning で機械学習のワークフローをビルドして実行します。

Azure Machine Learning SDK for R では、Python SDK にバインドする `reticulate` パッケージが使用されます。 Python に直接バインドすることにより、R 用の SDK では、選択した R 環境から、Python SDK で実装されているコア オブジェクトおよびメソッドにアクセスできます。

SDK の主な機能は次のとおりです。

+    機械学習の実験を監視、ログ記録、整理するためのクラウド リソースを管理します。
+    GPU アクセラレーション モデルのトレーニングなどのクラウド リソースを使用して、モデルをトレーニングします。
+    Azure Container Instances (ACI) および Azure Kubernetes Service (AKS) で、モデルを Webservice としてデプロイします。

すべてのドキュメントについては、[パッケージ Web サイト](https://azure.github.io/azureml-sdk-for-r)を参照してください。

### <a name="azure-machine-learning-integration-with-event-grid"></a>Azure Machine Learning と Event Grid の統合 

Azure Machine Learning が Event Grid 用のリソース プロバイダーになりました。Azure portal または Azure CLI を使用して機械学習イベントを構成できます。 ユーザーは、実行の完了、モデルの登録、モデル デプロイ、データ ドリフト検出のイベントを作成できます。 これらのイベントは、Event Grid でサポートされているイベント ハンドラーにルーティングして使用できます。 詳細については、機械学習イベントの[スキーマ](https://docs.microsoft.com/azure/event-grid/event-schema-machine-learning)、および[チュートリアル](how-to-use-event-grid.md)に関する記事を参照してください。

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>Azure Machine Learning SDK for Python v1.0.72

+ **新機能**
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift) パッケージを通じてデータセット モニターが追加されました。これにより、データ ドリフトや、その他の時間の経過による統計上の変化に関する時系列データセットの監視が可能になります。 アラートとイベントは、誤差が検知された場合、またはデータ上でその他の条件が満たされた場合にトリガーされます。 詳細については、[ドキュメント](https://aka.ms/datadrift)を参照してください。
  + Azure Machine Learning で、2 つの新しいエディション (SKU とも呼ばれます) について発表します。 このリリースでは、Basic または Enterprise の Azure Machine Learning ワークスペースを作成できるようになりました。 すべての既存のワークスペースは、Basic エディションに既定で設定されます。また、Azure portal または Studio にアクセスして、いつでもワークスペースをアップグレードできます。 Azure portal から、Basic または Enterprise のワークスペースを作成できます。 詳細については、[こちらのドキュメント](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace)を参照してください。 SDK では、ワークスペース オブジェクトの "sku" プロパティを使用して、ワークスペースのエディションを特定できます。
  + また、Azure Machine Learning コンピューティングの強化を行いました。デバッグ用の診断ログを表示するだけでなく、Azure Monitor でクラスターのメトリック (合計ノード、実行中のノード、合計コア クォータなど) を表示できるようになりました。 さらに、クラスター上の現在実行中またはキューに登録された実行や、クラスター上のさまざまなノードの IP などの詳細を表示することもできます。 これらは、ポータルで、または SDK や CLI で対応する関数を使用して表示できます。

  + **プレビュー機能**
    + Azure Machine Learning コンピューティングでのローカル SSD のディスク暗号化に対するプレビュー サポートをリリースしています。 テクニカル サポート チケットを作成し、サブスクリプションをホワイトリストに登録してもらって、この機能を使用してください。
    + Azure Machine Learning バッチ推論のパブリック プレビュー。 Azure Machine Learning バッチ推論は、時間の制約を受けない大規模な推論ジョブを対象とします。 バッチ推論では、非同期アプリケーション向けの比類なきスループットで、コスト効率のよい推論コンピューティングのスケーリングが提供されます。 大規模なデータ コレクションにわたって、高スループットのファイア アンド フォーゲット推論に対して最適化されています。
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + ラベル付きデータセットに対して有効な機能
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask

        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)

        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()

        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD)
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)

        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **バグの修正と機能強化**
  + **azure-cli-ml**
    + CLI でモデル パッケージがサポートされるようになりました。
    + データセット CLI が追加されました。 詳細については、`az ml dataset --help` を参照してください
    + サポートされているモデル (ONNX、scikit-learn、TensorFlow) の InferenceConfig インスタンスを使用しないデプロイとパッケージ化のサポートが追加されました。
    + SDK および CLI でのサービスのデプロイ (ACI および AKS) の上書きフラグが追加されました。 指定した場合、既存の名前が付けられたサービスについては、既存のサービスを上書きします。 サービスが存在しない場合、新しいサービスが作成されます。
    + モデルは、Onnx および Tensorflow という 2 つの新しいフレームワークに登録できます。 - モデルの登録では、モデルのサンプルの入力データ、サンプルの出力データ、リソース構成が受け入れられます。
  + **azureml-automl-core**
    + イテレーションのトレーニングは、実行時の制約が設定されている場合にのみ、子プロセスで実行されます。
    + 予測タスクのガードレールが追加されました。指定された max_horizon が特定のコンピューターでメモリの問題を引き起こしているかどうかを確認します。 その場合、ガードレール メッセージが表示されます。
    + 2 年 1 か月というような複雑な頻度に対するサポートが追加されました。 \- 頻度を特定できない場合に対して、わかりやすいエラーメッセージが追加されました。
    + モデル デプロイの失敗を解決するために、自動生成された conda env に azureml-default を追加します
    + Azure Machine Learning パイプラインの中間データを表形式のデータセットに変換し、`AutoMLStep` で使用できるようにします。
    + ストリーミング用の列の目的の更新が実装されました。
    + ストリーミング用の Imputer および HashOneHotEncoder のトランスフォーマー パラメーターの更新プログラムが実装されました。
    + 検証エラー メッセージに、現在のデータ サイズと最低限必要なデータ サイズが追加されました。
    + 各検証フォールドに最低 2 つのサンプルが確実に提供されるよう、クロス検証のために最低限必要なデータ サイズが更新されました。
  + **azureml-cli-common**
    + CLI でモデル パッケージがサポートされるようになりました。
    + モデルは、Onnx および Tensorflow という 2 つの新しいフレームワークに登録できます。
    + モデルの登録では、モデルのサンプルの入力データ、サンプルの出力データ、リソース構成が受け入れられます。
  + **azureml-contrib-gbdt**
    + ノートブックのリリース チャネルが修正されました
    + サポートされていない AmlCompute 以外のコンピューティング先に対する警告が追加されました
    + azureml-contrib-gbdt パッケージに LightGMB Estimator が追加されました
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + CLI でモデル パッケージがサポートされるようになりました。
    + 非推奨のデータセット API に対する非推奨警告が追加されます。 https://aka.ms/tabular-dataset でデータセット API の変更通知を参照してください。
    + データセットが登録されている場合は、登録名とバージョンを返すように [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) を変更します。
    + 引数としてのデータセットを伴う ScriptRunConfig を実験の実行を送信するために繰り返し使用することができないというバグが修正されます。
    + 実行中に取得されたデータセットは、追跡され、実行の詳細ページに、または実行の完了後に [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) を呼び出すことで表示することができます。
    + Azure Machine Learning パイプラインの中間データを表形式のデータセットに変換し、[`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep) で使用できるようにします。
    + サポートされているモデル (ONNX、scikit-learn、TensorFlow) の InferenceConfig インスタンスを使用しないデプロイとパッケージ化のサポートが追加されました。
    + SDK および CLI でのサービスのデプロイ (ACI および AKS) の上書きフラグが追加されました。 指定した場合、既存の名前が付けられたサービスについては、既存のサービスを上書きします。 サービスが存在しない場合、新しいサービスが作成されます。
    +  モデルは、Onnx および Tensorflow という 2 つの新しいフレームワークに登録できます。 モデルの登録では、モデルのサンプルの入力データ、サンプルの出力データ、リソース構成が受け入れられます。
    + Azure Database for MySQL の新しいデータストアが追加されました。 Azure Machine Learning パイプラインに DataTransferStep の Azure Database for MySQL を使用するための例が追加されました。
    + 機能が追加され、実験にタグが追加されて削除されました。機能が追加され、実行からタグが削除されました
    + SDK および CLI でのサービスのデプロイ (ACI および AKS) の上書きフラグが追加されました。 指定した場合、既存の名前が付けられたサービスについては、既存のサービスを上書きします。 サービスが存在しない場合、新しいサービスが作成されます。
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + `azureml-contrib-datadrift` から `azureml-datadrift` への移動
    + ドリフトやその他の統計メジャーの時系列データセットの監視に対するサポートの追加
    + [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) クラスの新しいメソッド (`create_from_model()` および `create_from_dataset()`)。 `create()` メソッドが非推奨となる予定。
    + Azure Machine Learning Studio の Python および UI での視覚エフェクトの調整。
    + データセットの毎日の監視に加え、毎週および毎月の監視のスケジュール設定をサポート。
    + データセット モニターの履歴データを分析するためのデータ モニター メトリックのバックフィルをサポート。
    + さまざまなバグの修正
  + [**azureml-pipeline-core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + パイプラインの `yaml` ファイルから Azure Machine Learning パイプラインの実行を送信するのに、azureml-dataprep は不要になりました。
  + [**azureml-train-automl**](/python/api/azureml-train-automl-runtime/)
    + モデル デプロイの失敗を解決するために、自動生成された conda env に azureml-default を追加します
    + AutoML リモートトレーニングに azureml-default が含まれるようになり、推論にトレーニング env を再利用できるようになりました。
  + **azureml-train-core**
    + [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch) Estimator への PyTorch 1.3 サポートの追加

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>ビジュアル インターフェイス (プレビュー)

+ Azure Machine Learning ビジュアル インターフェイス (プレビュー) は、[Azure Machine Learning パイプライン](concept-ml-pipelines.md)で実行するために全面的に見直されました。 ビジュアル インターフェイスで作成されたパイプライン (以前は実験と呼ばれていました) は、コア Azure Machine Learning エクスペリエンスと完全に統合されました。
  + SDK 資産と統合された管理エクスペリエンス
  + ビジュアル インターフェイスのモデル、パイプライン、およびエンドポイントのバージョン管理と追跡
  + 新しい UI デザイン
  + バッチ推論のデプロイの追加
  + 推論コンピューティング先に対する Azure Kubernetes Service (AKS) のサポートの追加
  + 新しい Python ステップのパイプライン作成ワークフロー
  + ビジュアルの作成ツールの新しい[ランディング ページ](https://ml.azure.com)

+ **新しいモジュール**
  + 算術演算の適用
  + SQL 変換の適用
  + クリップの値
  + データの集計
  + SQL データベースからのインポート

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>Azure Machine Learning SDK for Python v1.0.69

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + 最適に実行されるよう、実行ごとに説明を計算せずにモデル説明をが制限されるようになりました。 ローカル、リモート、および ADB で動作変更が実施されます。
    + UI のオンデマンドのモデル説明のサポートを追加
    + `automl` の依存関係として psutil を追加し、amlcompute に conda 依存関係として psutil を含めました。
    + いくつかの系列で線形代数エラーを引き起こしていた予測データにおけるヒューリスティックの遅延とウィンドウのサイズが元に戻る問題を修正しました
      + 予測実行にヒューリスティックによって決定されたパラメーターの印刷出力を追加しました。
  + **azureml-contrib-datadrift**
    + データセット レベルの誤差が最初のセクションに含まれていない場合、出力メトリックの作成に保護を追加しました。
  + **azureml-contrib-interpret**
    + azureml-contrib-explain-model パッケージの名前が azureml-contrib-interpret に変更されました
  + **azureml-core**
    + 未登録のデータ セットに API を追加しました。 `dataset.unregister_all_versions()`
    + azureml-contrib-explain-model パッケージの名前が azureml-contrib-interpret に変更されました。
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core)**
    + 未登録のデータ セットに API を追加しました。 dataset.[unregister_all_versions()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--)。
    + データの変更時間を確認するデータ セット API が追加されました。 `dataset.data_changed_time`
    + Azure Machine Learning パイプラインで `PythonScriptStep`、`EstimatorStep`、`HyperDriveStep` への入力として `FileDataset` および `TabularDataset` を使用できるようになりました
    + 多数のファイルを含むフォルダーの `FileDataset.mount` のパフォーマンスが向上しました
    + [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) および [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) を、[PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep)、[EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep)、[HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) への入力として Azure Machine Learning パイプラインで使用できるようになりました。
    + 多数のファイルを含むフォルダーの FileDataset.[mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) のパフォーマンスが向上しました
    + 実行の詳細で既知のエラーに対する推奨事項に関する URL を追加しました。
    + run.get_metrics において実行の子が多すぎる場合に要求が失敗するバグを修正しました
    + [run.get_metrics](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) において実行の子が多すぎる場合に要求が失敗するバグを修正しました
    + Arcadia クラスターでの認証のサポートが追加されました。
    + 実験オブジェクトを作成すると、実行履歴の追跡を行うため [Azure Machine Learning] ワークスペースで実験が取得または作成されます。 実験 ID およびアーカイブされた時間は、作成時に実験オブジェクトに設定されます。 例: experiment = Experiment (ワークスペース、"新しい実験")。experiment_id = experiment.id archive() とおよび reactivate() は、実験に対して呼び出すことができる関数で、UX に表示されるか、または実験の一覧を表示するよう規定で返されます。 アーカイブされた実験と同じ名前の新しい実験を作成した場合は、新しい名前を渡すことで、再アクティブ化するときにアーカイブされた実験の名前を変更できます。 存在できる同じ名前のアクティブな実験は 1 つだけです。 例: experiment1 = Experiment (ワークスペース、"アクティブな実験") experiment1.archive() # アーカイブと同じ名前で新しいアクティブな実験を作成します。 experiment2. = Experiment (ワークスペース、"アクティブな実験") experiment1.reactivate (新しい名前 = "以前アクティブだった実験")。Experiment の静的メソッドの list() は、名前フィルターと ViewType フィルターを受け取ることができます。 ViewType 値は、"ACTIVE_ONLY"、"ARCHIVED_ONLY"、および "ALL" です。例: archived_experiments = Experiment.list (ワークスペース、view_type = "ARCHIVED_ONLY") all_first_experiments = Experiment.list (ワークスペース、name = "最初の実験"、view_type = "ALL")
    + モデル デプロイとサービスの更新で環境を使用するサポート
  + **azureml-datadrift**
    + DataDriftDector クラスの show 属性では、省略可能な引数 'with_details' がサポートされなくなります。 show 属性では、特徴列のデータ誤差の係数とデータ誤差の影響のみが表示されます。
    + DataDriftDetector 属性 'get_output' の動作変更:
      + 入力パラメーター start_time、end_time は必須ではなく省略可能です。
      + 同じ呼び出しで特定の run_id を使用している入力固有の start_time および/または end_time は相互に排他的であるため、値エラーの例外が発生します
      + 入力固有の start_time または end_time では、スケジュールされた実行の結果のみが返されます。
      + パラメーター 'daily_latest_only' は推薦されません。
    + データセット ベースのデータ誤差の出力の取得がサポートされるようになりました。
  + **azureml-explain-model**
    + AzureML-explain-model パッケージの名前を AzureML-interpret に変更し、現時点では旧バージョンとの互換性を維持するために古いパッケージを保持します
    + ExplanationClient からのダウンロード時に、未加工の説明が既定の回帰ではなく分類タスクに設定される `automl` バグを修正しました
    + `MimicWrapper` を使用して直接作成する `ScoringExplainer` のサポートを追加しました
  + **azureml-pipeline-core**
    + 大規模なパイプライン作成のパフォーマンスが向上しました
  + **azureml-train-core**
    + TensorFlow Estimator で TensorFlow 2.0 がサポートされるようになりました
  + **azureml-train-automl**
    + [実験](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment)オブジェクトを作成すると、実行履歴の追跡を行うための Azure Machine Learning ワークスペースで実験が取得または作成されます。 実験 ID およびアーカイブされた時間は、作成時に実験オブジェクトに設定されます。 例:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [archive()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) および [reactivate()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) は、実験を呼び出し、その実験を非表示にして UX での表示から復元するか、既定で返されて実験の一覧を表示できる関数です。 アーカイブされた実験と同じ名前の新しい実験を作成した場合は、新しい名前を渡すことで、再アクティブ化するときにアーカイブされた実験の名前を変更できます。 存在できる同じ名前のアクティブな実験は 1 つだけです。 例:

        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        実験の静的メソッド [list()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) は、名前フィルターと ViewType フィルターを受け取ることができます。 ViewType 値は、"ACTIVE_ONLY"、"ARCHIVED_ONLY"、"ALL" です。 例:

        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + モデル デプロイおよびサービス更新向けの環境の使用をサポートします。
  + **[azureml-datadrift](https://docs.microsoft.com/python/api/azureml-datadrift)**
    + [DataDriftDetector](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) クラスの show 属性では、省略可能な引数 'with_details' がサポートされなくなります。 show 属性では、特徴列のデータ誤差の係数とデータ誤差の影響のみが表示されます。
    + DataDriftDetector 関数 [get_output]https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) の動作が変更されます。
      + 入力パラメーター start_time、end_time は必須ではなく省略可能です。
      + 同じ呼び出しで特定の run_id を使用している入力固有の start_time および/または end_time は相互に排他的であるため、値エラーの例外が発生します。
      + 入力固有の start_time または end_time では、スケジュールされた実行の結果のみが返されます。
      + パラメーター 'daily_latest_only' は推薦されません。
    + データセット ベースのデータ誤差の出力の取得がサポートされるようになりました。
  + **azureml-explain-model**
    + AzureML-explain-model パッケージの名前を AzureML-interpret に変更し、現時点では旧バージョンとの互換性を維持するために古いパッケージを保持します。
    + ExplanationClient からのダウンロード時に、未加工の説明が既定の回帰ではなく分類タスクに設定されるバグを修正しました。
    + MimicWrapper を使用して直接作成する [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer?view=azure-ml-py) のサポートを追加しました
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + 大規模なパイプライン作成のパフォーマンスが向上しました。
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow) Estimator で TensorFlow 2.0 がサポートされるようになりました。
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + オーケストレーションで既に処理が行われているため、セットアップの反復処理に失敗した場合でも親の実行が失敗しなくなりました。
    + AutoML 実験で local-docker および local-conda がサポートされるようになりました
    + AutoML 実験で local-docker および local-conda がサポートされるようになりました。


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning ワークスペースの新しい Web エクスペリエンス (プレビュー)

[新しいワークスペース ポータル](https://ml.azure.com)の [実験] タブが更新され、データ サイエンティストがより高性能な方法で実験を監視できるようになりました。 次の機能を調査できます。
+ 実験の一覧を簡単にフィルター処理して並べ替えることができる試験的なメタデータ
+ 実行を視覚化して比較できる、簡略化された高性能な実験の詳細ページ
+ トレーニング実行を理解して監視するために詳細ページを実行するための新しいデザイン

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Azure Machine Learning SDK for Python v1.0.65

  + **新機能**
    + キュレートされた環境が追加されました。 これらの環境は、一般的な機械学習タスク用のライブラリを使用してあらかじめ構成されており、実行時間を短縮するため、Docker イメージとして事前にビルドおよびキャッシュされています。 既定で、"AzureML" というプレフィックスが付けられて、ワークスペースの環境の一覧に表示されます。
    + キュレートされた環境が追加されました。 これらの環境は、一般的な機械学習タスク用のライブラリを使用してあらかじめ構成されており、実行時間を短縮するため、Docker イメージとして事前にビルドおよびキャッシュされています。 既定で、"AzureML" というプレフィックスが付けられて、[ワークスペース](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29)の環境の一覧に表示されます。

  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + ADB と HDI に対する ONNX の変換サポートが追加されました

+ **プレビュー機能**
  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + テキスト フィーチャライザーとして BERT と BiLSTM がサポートされました (プレビューのみ)
    + 列の目的およびトランスフォーマーのパラメーターに対して特性付けのカスタマイズがサポートされました (プレビューのみ)
    + トレーニングの間にユーザーがモデルの説明を有効にしたときに、生の説明がサポートされるようになりました (プレビューのみ)
    + トレーニング可能なパイプラインとして、`timeseries` 予測用の Prophet が追加されました (プレビューのみ)

  + **azureml-contrib-datadrift**
    + azureml-contrib-datadrift から azureml-datadrift にパッケージが再配置されました。`contrib` パッケージは、今後のリリースで削除される予定です

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + FeaturizationConfig が AutoMLConfig と AutoMLBaseSettings に導入されました
    + FeaturizationConfig が [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) と AutoMLBaseSettings に導入されました
      + 特性付けの列の目的が特定の列と特徴の種類でオーバーライドされます
      + トランスフォーマー パラメーターがオーバーライドされます
    + explain_model() および retrieve_model_explanations() の非推奨メッセージが追加されました
    + トレーニング可能なパイプラインとして Prophet が追加されました (プレビューのみ)
    + explain_model() および retrieve_model_explanations() の非推奨メッセージが追加されました。
    + トレーニング可能なパイプラインとして Prophet が追加されました (プレビューのみ)。
    + ターゲットのラグ、ローリング ウィンドウのサイズ、最大の水平の自動検出のサポートが追加されました。 target_lags、target_rolling_window_size、または max_horizon のいずれかが "auto" に設定されている場合、トレーニング データに基づいて対応するパラメーターの値を推定するために、ヒューリスティックが適用されます。
    + データ セットに 1 つのグレイン列が含まれている場合の予測を修正しました。このグレインは数値型であり、トレーニング セットとテスト セットの間にギャップがあります
    + 予測タスクでのリモート実行での重複インデックスに関するエラー メッセージを修正しました
    + データ セットに 1 つのグレイン列が含まれている場合の予測を修正しました。このグレインは数値型であり、トレーニング セットとテスト セットの間にギャップがあります。
    + 予測タスクでのリモート実行での重複インデックスに関するエラー メッセージを修正しました。
    + データセットが不均衡かどうかを確認するためのガードレールが追加されました。 そうである場合は、ガードレール メッセージがコンソールに書き込まれます。
  + **azureml-core**
    + モデル オブジェクトを使用してストレージ内のモデルへの SAS URL を取得する機能が追加されました。 例: model.get_sas_url()
    + 送信された実行に関連付けられたデータセットを取得するための `run.get_details()['datasets']` が導入されます
    + JSON Lines ファイルから TabularDataset を作成する API `Dataset.Tabular.from_json_lines_files` が追加されます。 TabularDataset での JSON Lines ファイルのこの表形式データの詳細については、 https://aka.ms/azureml-data のドキュメントを参照してください。
    + 追加の VM サイズ フィールド (OS ディスク、GPU の数) が supported_vmsizes() 関数に追加されました
    + 実行、プライベートおよびパブリック IP、ポートなどを表示する追加のフィールドが、list_nodes() 関数に追加されました。
    + クラスターのプロビジョニング時に新しいフィールドを指定できるようになりました。--remotelogin_port_public_access は、クラスターの作成時に SSH ポートを開いたままにするか、閉じるかに応じて、有効または無効に設定できます。 それを指定しない場合は、クラスターが VNet の内部にデプロイされるかどうかに応じて、ポートはサービスによってスマートに開くか、閉じるかされます。
  + **azureml-explain-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + モデル オブジェクトを使用してストレージ内のモデルへの SAS URL を取得する機能が追加されました。 例: model.[get_sas_url()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + 送信された実行に関連付けられたデータセットを取得するための run.[get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)['datasets'] が導入されます
    + JSON Lines ファイルから TabularDataset を作成する API `Dataset.Tabular`.[from_json_lines_files()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) が追加されます。 TabularDataset での JSON Lines ファイルのこの表形式データの詳細については、 https://aka.ms/azureml-data のドキュメントを参照してください。
    + 追加の VM サイズ フィールド (OS ディスク、GPU の数) が [supported_vmsizes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-) 関数に追加されました
    + 実行、プライベートおよびパブリック IP、ポートなどを表示する追加のフィールドが、[list_nodes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) 関数に追加されました。
    + クラスターの[プロビジョニング](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--)時に新しいフィールドを指定できるようになりました。`--remotelogin_port_public_access` は、クラスターの作成時に SSH ポートを開いたままにするか、閉じるかに応じて、enabled または disabled に設定できます。 それを指定しない場合は、クラスターが VNet の内部にデプロイされるかどうかに応じて、ポートはサービスによってスマートに開くか、閉じるかされます。
  + **azureml-explain-model**
    + 分類シナリオでの説明の出力に関するドキュメントが改善されました。
    + 評価の例の説明で予測された y 値をアップロードする機能が追加されました。 視覚化がいっそう有用になります。
    + 説明プロパティが MimicWrapper に追加され、基になる MimicExplainer を取得できるようになりました。
  + **azureml-pipeline-core**
    + Module、ModuleVersion、ModuleStep について説明するノートブックが追加されました
  + **azureml-pipeline-steps**
    + AML パイプラインによる R スクリプトの実行をサポートする RScriptStep が追加されました。
    + "パラメーター SubscriptionId の割り当てが指定されていない" というエラー メッセージの原因になっていた AzureBatchStep でのメタデータ パラメーターの解析を修正しました。
  + **azureml-train-automl**
    + データ入力形式として、training_data、validation_data、label_column_name、weight_column_name がサポートされるようになりました
    + explain_model() および retrieve_model_explanations() の非推奨メッセージが追加されました
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + [Module](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class))、[ModuleVersion、および [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep) について説明する[ノートブック](https://aka.ms/pl-modulestep)が追加されました。
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + AML パイプラインによる R スクリプトの実行をサポートする [RScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) が追加されました。
    + "パラメーター SubscriptionId の割り当てが指定されていない" というエラー メッセージの原因になっていた [AzureBatchStep でのメタデータ パラメーターの解析を修正しました。
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + データ入力形式として、training_data、validation_data、label_column_name、weight_column_name がサポートされるようになりました。
    + [explain_model()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) および [retrieve_model_explanations()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-) の非推奨メッセージが追加されました。


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Azure Machine Learning SDK for Python v1.0.62

+ **新機能**
  + TabularDataset に `timeseries` 特性が導入されました。 この特性により、ある期間のすべてのデータまたは最新のデータを取得するなど、TabularDataset のデータに対するタイムスタンプのフィルター処理が容易になります。 TabularDataset のこの `timeseries` 特性の詳細については、 https://aka.ms/azureml-data のドキュメントまたは https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb のノートブックの例を参照してください。
  + TabularDataset と FileDataset でのトレーニングが有効になりました。 https://aka.ms/dataset-tutorial のノートブックの例を参照してください。

  + **azureml-train-core**
      + PyTorch Estimator に `Nccl` および `Gloo` のサポートが追加されました

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + AutoML の設定 "lag_length" および LaggingTransformer が非推奨になりました。
    + データフロー形式で指定されている場合の入力データの正しい検証を修正しました
    + グラフ json を生成して成果物にアップロードするように fit_pipeline.py を変更しました。
    + `Cytoscape` を使用して `userrun` の下にグラフを表示しました。
  + **azureml-core**
    + ADB コードでの例外処理を再検討し、新しいエラー処理に従って変更しました。
    + Notebook VM の自動 MSI 認証を追加しました。
    + 再試行失敗のために破損したモデルまたは空のモデルがアップロードされるバグを修正しました。
    + `DataReference` モードが変更されたときに (`as_upload`、`as_download`、`as_mount` を呼び出したときなど) `DataReference` の名前が変更されるバグを修正しました。
    + `FileDataset.mount` および `FileDataset.download` に対する `mount_point` と `target_path` を省略可能にしました
    + 細かいタイムスタンプ列が割り当てられずに時刻のシリアル関連 API が呼び出された場合や、割り当てられたタイムスタンプ列が削除された場合、タイムスタンプ列が見つからないという例外がスローされます。
    + Date 型列には時刻のシリアル列が割り当てられている必要があり、そうでないと例外が予想されます
    + 時刻のシリアル列割り当て API "with_timestamp_columns" は、細かい/粗いタイムスタンプ列名として None 値を受け取ることができ、これにより以前に割り当てられたタイムスタンプ列がクリアされます。
    + 粗い粒度または細かい粒度のタイムスタンプ列が削除されると例外がスローされ、ユーザーに対して、削除リストのタイムスタンプ列を除外するか、None 値を指定して with_time_stamp を呼び出すことによりタイムスタンプを解放した後で、削除できることが示されます
    + 粗い粒度または細かい粒度のタイムスタンプ列が保持列リストに含まれていないと例外がスローされ、ユーザーに対して、保持列リストにタイムスタンプ列を含めるか、None 値を指定して with_time_stamp を呼び出すことによりタイムスタンプを解放した後で、保持できることが示されます。
    + 登録済みモデルのサイズのログ記録を追加しました。
  + **azureml-explain-model**
    + "packaging" Python パッケージがインストールされていない場合にコンソールに出力される警告を修正しました: "Using older than supported version of lightgbm, please upgrade to version greater than 2.2.1" (サポートされているバージョンより古い lightgbm を使用しています。2.2.1 より後のバージョンにアップグレードしてください)
    + 多くの機能でグローバルな説明に対するシャーディングに関するダウンロード モデルの説明を修正しました
    + 出力の説明で初期化の例がない mimic explainer を修正しました
    + 2 つの異なる種類のモデルを使用する説明クライアントでアップロードするときの設定プロパティの不変エラーを修正しました
    + 1 つのスコアリング Explainer でエンジニアリングされた値と生の値の両方を返すことができるように、スコアリング explainer.explain() に get_raw パラメーターを追加しました。
  + **azureml-train-automl**
    + `automl` 説明 SDK からの説明をサポートするために、AutoML からのパブリック API が導入されました - AutoML の特徴付け SDK と説明 SDK を分離することによる、AutoML の説明をサポートする新しい方法 - AutoML モデルに対して AzureML 説明 SDK からの生の説明のサポートを統合しました。
    + リモート トレーニング環境からの azureml-defaults の削除。
    + 既定のキャッ シュストアの場所を、FileCacheStore ベースの場所から、Azure Databricks コード パス上の AutoML に対する AzureFileCacheStore の場所に変更しました。
    + データフロー形式で指定されている場合の入力データの正しい検証を修正しました
  + **azureml-train-core**
    + source_directory_data_store の非推奨が元に戻されました。
    + AzureML でインストールされるパッケージのバージョンをオーバーライドする機能が追加されました。
    + Estimator の `environment_definition` パラメーターに dockerfile のサポートが追加されました。
    + Estimator での分散トレーニング パラメーターが簡略化されました。

         ```py
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning ワークスペースの新しい Web エクスペリエンス (プレビュー)
データ サイエンティストやデータ エンジニアは、新しい Web エクスペリエンスで、データの準備および視覚化から、モデルのトレーニングおよびデプロイまでの機械学習のエンドツーエンドのライフサイクルを 1 つの場所で完了できます。

![Azure Machine Learning ワークスペースの UI (プレビュー)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**主な機能:**

新しい Azure Machine Learning インターフェイスでは、次のことができるようになりました。
+ ご自分のノートブックまたはリンクを Jupyter 外で管理する
+ [自動 ML の実験を実行する](tutorial-first-experiment-automated-ml.md)
+ [ローカル ファイル、データストアおよび Web ファイルからデータセットを作成する](how-to-create-register-datasets.md)
+ モデル作成のためにデータセットを調査および準備する
+ ご自身のモデルのデータ誤差を監視する
+ ダッシュボードに最近使用したリソースを表示する

このリリースの時点では、次のブラウザーがサポートされています:Chrome、Firefox、Safari、Microsoft Edge プレビュー。

**既知の問題:**

1. デプロイ中に "Something went wrong! Error loading chunk files" (問題が発生しました。チャンク ファイルの読み込み中にエラーが発生しました) と表示される場合、ご使用のブラウザーを更新します。

1. Notebooks と Files でファイルを削除または名前変更できない。 パブリック プレビュー中に、Notebook VM の Jupyter UI またはターミナルを使用して、ファイルの更新操作を実行できます。 これはマウントされたネットワーク ファイル システムであるため、Notebook VM に対して行ったすべての変更は、直ちに Notebook ワークスペースに反映されます。

1. Notebook VM に SSH 接続するには:
   1. VM のセットアップ時に作成された SSH キーを検索します。 または、Azure Machine Learning ワークスペースでそのキーを探します。[コンピューティング] タブを開き、一覧から Notebook VM を見つけ、そのプロパティを開いて、そのダイアログからキーをコピーします。
   1. それらの公開および秘密 SSH キーをご自分のローカル コンピューターにインポートします。
   1. それらを Notebook VM への SSH 接続に使用します。

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Azure Machine Learning SDK for Python v1.0.60

+ **新機能**
  + データストアまたはパブリック URL 内の 1 つまたは複数のファイルを参照する FileDataset が導入されました。 ファイルの形式は任意です。 FileDataset では、ファイルをダウンロードしたり、コンピューターにマウントしたりできます。 FileDataset の詳細については、 https://aka.ms/file-dataset を参照してください。
  + PythonScript Step、Adla Step、Databricks Step、DataTransferStep、AzureBatch Step にパイプライン YAML サポートを追加しました

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + AutoArima は、プレビュー専用のサジェスト可能パイプラインになりました。
    + 予測のエラー報告を改善しました。
    + 予測タスクの汎用ではなくカスタムの例外を使用してログ記録を改善しました。
    + イテレーションの合計数よりも少なくなるように、max_concurrent_iterations のチェックを削除しました。
    + AutoML モデルで AutoMLExceptions が返されるようになりました
    + このリリースでは、自動化された機械学習のローカル実行の実行パフォーマンスが向上しています。
  + **azureml-core**
    + 登録名でキーが指定された `TabularDataset` と `FileDataset` のオブジェクトのディクショナリを返す Dataset.get_all(workspace) が導入されました。

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + `Dataset.Tabular.from_delimited_files` と `Dataset.Tabular.from_parquet.files` の引数として `parition_format` が導入されました。 各データ パスのパーティション情報は、指定された形式に基づいて列に抽出されます。 '{column_name}' では文字列の列、'{column_name:yyyy/MM/dd/HH/mm/ss}' では datetime の列が作成されます。ここで、'yyyy'、'MM'、'dd'、'HH'、'mm'、'ss' は datetime 型の年、月、日、時間、分、秒の抽出に使用されます。 partition_format は、最初のパーティション キーの位置から始まり、ファイル パスの末尾までになります。 たとえば、パーティションが国と時間のパス '../USA/2019/01/01/data.csv' の場合、partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' では値が 'USA' の文字列の列 'Country' と、値が '2019-01-01' の datetime 列 'PartitionDate' が作成されます。
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + `Dataset.Tabular.from_delimited_files` と `Dataset.Tabular.from_parquet.files` の引数として `partition_format` が導入されました。 各データ パスのパーティション情報は、指定された形式に基づいて列に抽出されます。 '{column_name}' では文字列の列、'{column_name:yyyy/MM/dd/HH/mm/ss}' では datetime の列が作成されます。ここで、'yyyy'、'MM'、'dd'、'HH'、'mm'、'ss' は datetime 型の年、月、日、時間、分、秒の抽出に使用されます。 partition_format は、最初のパーティション キーの位置から始まり、ファイル パスの末尾までになります。 たとえば、パーティションが国と時間のパス '../USA/2019/01/01/data.csv' の場合、partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' では値が 'USA' の文字列の列 'Country' と、値が '2019-01-01' の datetime 列 'PartitionDate' が作成されます。
    + `to_csv_files` と `to_parquet_files` のメソッドが `TabularDataset` に追加されました。 これらのメソッドにより、指定された形式のファイルにデータが変換され、`TabularDataset` と `FileDataset` の間の変換が可能になります。
    + Model.package() によって生成された Dockerfile を保存するときに、基本イメージ レジストリに自動的にログインします。
    + 'gpu_support' は、不要になりました。AML は nvidia docker 拡張機能が利用可能になったときに自動的にそれを検出し、使用するようになりました。 将来のリリースでは削除される予定です。
    + PipelineDrafts を作成、更新、および使用するためのサポートが追加されました。
    + このリリースでは、自動化された機械学習のローカル実行の実行パフォーマンスが向上しています。
    + ユーザーは、実行履歴のメトリックを名前でクエリできます。
    + 予測タスクの汎用ではなくカスタムの例外を使用してログ記録を改善しました。
  + **azureml-explain-model**
    + feature_maps パラメーターが新しい MimicWrapper に追加され、ユーザーが未加工の機能の説明を取得できるようになりました。
    + 説明のアップロードでは、データセットのアップロードが既定でオフになり、upload_datasets=True を指定して再度有効にすることができます
    + "Is_law" フィルター処理パラメーターが説明リストとダウンロード機能に追加されました。
    + グローバルとローカルの両方の説明オブジェクトに `get_raw_explanation(feature_maps)` メソッドが追加されます。
    + サポートされているバージョンを下回る場合に警告を出力するバージョン チェックが lightgbm に追加されました
    + 説明をバッチ処理するときのメモリ使用量が最適化されました
    + AutoML モデルで AutoMLExceptions が返されるようになりました
  + **azureml-pipeline-core**
    + PipelineDrafts を作成、更新、および使用するためのサポートが追加されました。変更可能なパイプライン定義を維持し、対話形式で実行するために使用できます
  + **azureml-train-automl**
    + GPU 対応 PyTorch v1.1.0 や :::no-loc text="cuda":::CUDA Toolkit 9.0、およびリモートの Python ランタイム環境で BERT/XLNet を有効にするために必要な pytorch-transformer の特定のバージョンをインストールするための機能が作成されました。
  + **azureml-train-core**
    + 一部のハイパーパラメーター空間定義エラーの初期エラーが、サーバー側ではなく SDK で直接発生します。

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Prep SDK v1.1.14
+ **バグの修正と機能強化**
  + 未加工のパスと資格情報を使用した ADLS/ADLSGen2 への書き込みが有効になりました。
  + `include_path=True` が `read_parquet` で使用できなかった原因となったバグを修正しました。
  + "無効なプロパティ値: hostsecret" の例外が原因で発生した `to_pandas_dataframe()` エラーを修正しました。
  + Spark モードで DBFS のファイルを読み取ることができなかったバグを修正しました。

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Azure Machine Learning SDK for Python v1.0.57
+ **新機能**
  + `TabularDataset` を AutomatedML で使用できるようにしました。 `TabularDataset` の詳細については、 https://aka.ms/azureml/howto/createdatasets にアクセスしてください。

+ **バグの修正と機能強化**
  + **automl-client-core-nativeclient**
    + トレーニングおよび検証ラベル (y と y_valid) が NumPy 配列ではなく、Pandas データフレームの形式で提供されるときに発生するエラーを修正しました。
    + `RawDataContext` の作成にデータと `AutoMLBaseSettings` オブジェクトのみを必要とするようにインターフェイスを更新しました。
    +  AutoML ユーザーが予測時に十分な長さではないトレーニング シリーズをドロップできるようにしました。 - AutoML ユーザーが予測時にトレーニング セットに存在しないテスト セットからグレインをドロップできるようにしました。
  + **azure-cli-ml**
    + Microsoft が生成した証明書と顧客証明書の両方について、AKS クラスターにデプロイされたスコアリング エンドポイントの TLS/SSL 証明書を更新できるようになりました。
  + **azureml-automl-core**
    + ラベルのない行が正常に削除されなかった AutoML の問題を修正しました。
    + AutoML のエラー ログ記録を改良し、常にエラー メッセージ全文がログ ファイルに書き込まれるようになりました。
    + AutoML のパッケージのピン留めを更新し、`azureml-defaults`、`azureml-explain-model`、および `azureml-dataprep` が追加されました。 AutoML で、パッケージの不一致に関する警告は表示されなくなりました (`azureml-train-automl` パッケージを除く)。
    + cv の分割によるサイズの不一致が原因でビンの計算が失敗する、`timeseries` の問題を修正しました。
    + 種類がクロス検証のトレーニングに対してアンサンブル イテレーションを実行するときに、データセット全体でトレーニングされたモデルのダウンロードに問題が発生した場合、モデルの重みと投票アンサンブルに取り込まれていたモデルとの間に矛盾がありました。
    + トレーニングおよび検証ラベル (y と y_valid) が NumPy 配列ではなく、Pandas データフレームの形式で提供されるときに発生するエラーを修正しました。
    + 入力テーブルのブール値の列が None になったときの予測タスクの問題を修正しました。
    + AutoML ユーザーが予測時に十分な長さではないトレーニング シリーズをドロップできるようにしました。 - AutoML ユーザーが予測時にトレーニング セットに存在しないテスト セットからグレインをドロップできるようにしました。
  + **azureml-core**
    + Blob_cache_timeout パラメーターの順序付けに関する問題を修正しました。
    + 外部例外 fit 型 および transform 型をシステム エラーに追加しました。
    + リモート実行のための Key Vault シークレットのサポートが追加されました。 ご自身のワークスペースに関連付けられている keyvault のシークレットを追加、取得、一覧表示するための azureml.core.keyvault.Keyvault クラスを追加しました。 サポートされている操作は次のとおりです。
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.keyvault.Keyvault.set_secret(name, value)
      + azureml.core.keyvault.Keyvault.set_secrets(secrets_dict)
      + azureml.core.keyvault.Keyvault.get_secret(name)
      + azureml.core.keyvault.Keyvault.get_secrets(secrets_list)
      + azureml.core.keyvault.Keyvault.list_secrets()
    + リモート実行中に既定のキー コンテナーやシークレットを取得する、次のメソッドを追加しました。
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.run.Run.get_secret(name)
      + azureml.core.run.Run.get_secrets(secrets_list)
    + submit-hyperdrive CLI コマンドを送信するオーバーライド パラメーターをさらに追加しました。
    + API 呼び出しの信頼性を向上させ、再試行を一般的な要求のライブラリ例外に拡大します。
    + 送信済みの実行からの実行を送信するためのサポートを追加します。
    + 最初のトークンの有効期限が切れた後にファイルがアップロードされない原因となった FileWatcher の期限が切れる SAS トークンの問題を修正しました。
    + データセット Python SDK への HTTP csv/tsv ファイルのインポートがサポートされるようになりました。
    + Workspace.setup() メソッドを非推奨にしました。 ユーザーに表示される警告メッセージとして、代わりに create() または get()/from_config() の使用が推奨されるようになりました。
    + Environment.add_private_pip_wheel() を追加しました。これにより、カスタマイズしたプライベート Python パッケージ `whl` をワークスペースにアップロードし、それらを安全に利用して環境をビルド/具体化できるようにしました。
    + Microsoft が生成した証明書と顧客証明書の両方について、AKS クラスターにデプロイされたスコアリング エンドポイントの TLS/SSL 証明書を更新できるようになりました。
  + **azureml-explain-model**
    + アップロードの説明にモデル ID を追加するためのパラメーターを追加しました。
    + メモリとアップロードの説明に `is_raw` のタグを追加しました。
    + PyTorch のサポートと azureml-explain-model パッケージに対するテストを追加しました。
  + **azureml-opendatasets**
    + 自動テスト環境の検出とログ記録をサポートします。
    + 米国の人口を郡および ZIP コード別に取得するクラスを追加しました。
  + **azureml-pipeline-core**
    + 入力ポートと出力ポートの定義にラベル プロパティが追加されました。
  + **azureml-telemetry**
    + 不適切なテレメトリ構成を修正しました。
  + **azureml-train-automl**
    + セットアップ失敗時に、セットアップ実行に対して [エラー] フィールドにログが記録されず、そのため親の実行の [エラー] に格納されなかったバグを修正しました。
    + ラベルのない行が正常に削除されなかった AutoML の問題を修正しました。
    + AutoML ユーザーが予測時に十分な長さではないトレーニング シリーズをドロップできるようにしました。
    + AutoML ユーザーが予測時にトレーニング セットに存在しないテスト セットからグレインをドロップできるようにしました。
    + AutoMLStep で、新しい構成パラメーターの変更や追加に関する問題を回避するために、`automl` の構成がバックエンドにパススルーされるようになりました。
    + AutoML データ ガードレールがパブリック プレビューになりました。 トレーニング後にデータ ガードレール レポート (分類/回帰タスク用) が表示され、ユーザーがそれに SDK API を使用してアクセスすることもできます。
  + **azureml-train-core**
    + PyTorch エスティメーターに torch 1.2 のサポートが追加されました。
  + **azureml-widgets**
    + 分類トレーニングのために混同行列グラフを改良しました。

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Prep SDK v1.1.12
+ **新機能**
  + 文字列のリストを入力として `read_*` メソッドに渡すことができるようになりました。

+ **バグの修正と機能強化**
  + `read_parquet` を Spark で 実行するときのパフォーマンスが大幅に向上しています。
  + あいまいな日付形式の列が 1 つ存在することで `column_type_builder` が失敗する問題を修正しました。

### <a name="azure-portal"></a>Azure portal
+ **プレビュー機能**
  + ログと出力ファイルのストリーミングが、[実行の詳細] ページで使用できるようになりました。 プレビューの切り替えが有効になっている場合、ファイルによってリアルタイムで更新がストリーミングされます。
  + ワークスペース レベルでクォータを設定する機能がプレビューでリリースされます。 AmlCompute クォータはサブスクリプション レベルで割り当てられますが、ワークスペース間でそのクォータを配分し、公平な共有とガバナンスのために割り当てることができるようになりました。 ワークスペースの左側のナビゲーション バーにある **[Usages+Quotas]\(使用量 + クォータ\)** ブレードをクリックして、 **[Configure Quotas]\(クォータの構成\)** タブを選択します。これはワークスペースをまたぐ操作であるため、ワークスペース レベルでクォータを設定できるようにするには、サブスクリプション管理者である必要があることに注意してください。

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Azure Machine Learning SDK for Python v1.0.55

+ **新機能**
  + AKS にデプロイされたスコアリング エンドポイントへの呼び出しに対して、トークンベースの認証がサポートされるようになりました。 現在のキー ベースの認証は引き続きサポートされ、ユーザーはこれらの認証メカニズムのいずれか一方を使用できます。
  + 仮想ネットワーク (VNet) の内側にある BLOB ストレージをデータストアとして登録する機能。

+ **バグの修正と機能強化**
  + **azureml-automl-core**
    + CV 分割の検証サイズが小さくて、回帰や予測の真のグラフに対して予測が不適切になるバグを修正しました。
    + リモート実行での予測タスクのログ記録が改善され、実行が失敗した場合に、ユーザーに包括的なエラー メッセージが提供されるようになりました。
    + 前処理フラグが True の場合の `Timeseries` のエラーを修正しました。
    + いくつかの予測データ検証エラー メッセージをより実用的なものにしました。
    + データセットの削除や遅延読み込みによって (特にプロセスの生成間)、AutoML のメモリ消費量を削減しました
  + **azureml-contrib-explain-model**
    + Explainer に model_task フラグを追加し、ユーザーがモデルの種類の既定の自動推論ロジックをオーバーライドできるようにしました
    + ウィジェットの変更:`contrib` で自動的にインストールされます。`nbextension` はインストール/有効化されなくなります。グローバルな機能の重要性 (Permutative など) に関する説明のみがサポートされます
    + ダッシュボードの変更: - [概要] ページでの `beeswarm` プロットに加えて、box プロットと violin プロット - "Top -k'" スライダー変更での `beeswarm` プロットのレンダリングの高速化 - top-k 計算方法を説明する便利なメッセージ - データが提供されないときのグラフに代わる便利なカスタマイズ可能なメッセージ
  + **azureml-core**
    + モデルとその依存関係をカプセル化する Docker イメージと Dockerfile を作成するための Model.package() メソッドを追加しました。
    + Environment オブジェクトを含む InferenceConfig を受け入れるようにローカルな Web サービスを更新しました。
    + model_path パラメーターとして "." (現在のディレクトリ) が渡されたときに無効なモデルを生成する Model.register() を修正しました。
    + Run.submit_child を追加しました。機能は Experiment.submit と同じですが、送信された子実行の親として実行を指定します。
    + Run.register_model で Model.register からの構成オプションがサポートされるようになりました。
    + 既存のクラスターで JAR ジョブを実行する機能。
    + instance_pool_id パラメーターと cluster_log_dbfs_path パラメーターをサポートするようになりました。
    + Web サービスにモデルをデプロイするときに、Environment オブジェクトの使用のサポートを追加しました。 InferenceConfig オブジェクトの一部として Environment オブジェクトを提供できるようになりました。
    + 次の新しいリージョンに対する appinsifht のマッピングを追加しました - centralus - westus - northcentralus
    + すべての Datastore クラスのすべての属性に関するドキュメントを追加しました。
    + blob_cache_timeout パラメーターを `Datastore.register_azure_blob_container` に追加しました。
    + save_to_directory メソッドと load_from_directory メソッドを azureml.core.environment.Environment に追加しました。
    + "az ml environment download" コマンドと "az ml environment register" コマンドを CLI に追加しました。
    + Environment.add_private_pip_wheel メソッドを追加しました。
  + **azureml-explain-model**
    + Dataset サービス を使用する説明に対してデータセットの追跡を追加しました (プレビュー)。
    + グローバルな説明をストリーミングするときの既定のバッチ サイズを、10k から 100 に減らしました。
    + Explainer に model_task フラグを追加し、ユーザーがモデルの種類の既定の自動推論ロジックをオーバーライドできるようにしました。
  + **azureml-mlflow**
    + 入れ子になったディレクトリが無視される mlflow.azureml.build_image のバグを修正しました。
  + **azureml-pipeline-steps**
    + 既存の Azure Databricks クラスターで JAR ジョブを実行する機能を追加しました。
    + DatabricksStep ステップに対する instance_pool_id パラメーターと cluster_log_dbfs_path パラメーターのサポートを追加しました。
    + DatabricksStep ステップでのパイプライン パラメーターのサポートを追加しました。
  + **azureml-train-automl**
    + アンサンブル関連のファイルに対する `docstrings` を追加しました。
    + `max_cores_per_iteration` および `max_concurrent_iterations` に対する言語がより適切になるようにドキュメントを更新しました
    + リモート実行での予測タスクのログ記録が改善され、実行が失敗した場合に、ユーザーに包括的なエラー メッセージが提供されるようになりました。
    + パイプラインの `automlstep` ノートブックから get_data を削除しました。
    + `automlstep`での `dataprep` のサポートを開始しました。

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v1.1.10

+ **新機能**
  + 特定の列に対して特定のインスペクター (ヒストグラム、散布図など) の実行を要求できるようになりました。
  + `append_columns` に並列化引数を追加しました。 True の場合、データはメモリに読み込まれますが、実行は並列で実行されます。False の場合、実行はストリーミングされますが、シングル スレッドです。

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Azure Machine Learning SDK for Python v1.0.53

+ **新機能**
  + 自動化された機械学習で、リモート コンピューティング ターゲットでの ONNX モデルのトレーニングがサポートされるようになりました
  + Azure Machine Learning で、以前の実行、チェックポイント、またはモデル ファイルからトレーニングを再開できるようになりました。
    + [推定器を使用して前回の実行からトレーニングを再開する](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/training/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb)方法を確認してください

+ **バグの修正と機能強化**
  + **automl-client-core-nativeclient**
    + 変換後の列の型の損失に関するバグを修正します (リンクされているバグ)。
    + y_query を、先頭に None を含んだオブジェクト型にできるようにします (#459519)。
  + **azure-cli-ml**
    + CLI コマンド "model deploy" と "service update" は、パラメーター、構成ファイル、またはこれらの 2 つの組み合わせを受け入れるようになりました。 パラメーターは、ファイル内の属性よりも優先されます。
    + 登録後にモデルの説明を更新できるようになりました
  + **azureml-automl-core**
    + NimbusML 依存関係を 1.2.0 バージョン (現在の最新バージョン) に更新します。
    + AutoML 推定器内で使用される NimbusML 推定器とパイプラインのサポートの追加。
    + アンサンブルの選択手順で、スコアが一定のままであっても、結果のアンサンブルが過剰に増加するバグの修正。
    + 予測タスクのための CV 分割全体での一部の特徴量化を再利用できるようにします。 これにより、遅延やローリング時間帯のような負荷の高い特徴付けで、セットアップ実行の実行時間がほぼ n_cross_validations 倍高速になります。
    + 時間が Pandas でサポートされている時間の範囲外である場合の問題への対処。 時間が pd.Timestamp.min より小さいか pd.Timestamp.max より大きい場合、DataException を発生させるようになりました
    + 予測で、調整可能な場合は、トレーニングとテストのセットで異なる周波数を使用できるようになりました。 たとえば、"1 月に開始される四半期" と "10 月に開始される四半期" を調整できます。
    + プロパティ "parameters" が TimeSeriesTransformer に追加されました。
    + 古い例外クラスを削除します。
    + 予測タスクで、パラメーター `target_lags` が 1 つの整数値または整数のリストを受け入れるようになりました。 整数が指定されている場合は、1 つのラグだけが作成されます。 リストが指定されている場合は、ラグの一意の値が取得されます。 target_lags=[1, 2, 2, 4] では、1、2、および 4 つの期間のラグが作成されます。
    + 変換後の列の型の損失に関するバグを修正します (リンクされているバグ)。
    + `model.forecast(X, y_query)` では、y_query を、先頭に None を含んだオブジェクト型にできるようにします (#459519)。
    + `automl` 出力に予期される値を追加します
  + **azureml-contrib-datadrift**
    +  azureml-contrib-opendatasets の代わりに azureml-opendatasets への切り替えを含むノートブックの例の向上と、データを強化する際のパフォーマンスの向上
  + **azureml-contrib-explain-model**
    + azureml-contrib-explain-model パッケージの生の特徴の重要度のための LIME Explainer の変換引数を修正しました
    + AzureML-contrib-explain-model パッケージの Image Explainer のイメージの説明にセグメント化を追加しました
    + LimeExplainer の scipy sparse のサポートを追加します
    + グローバル説明を一括でストリーミングして DecisionTreeExplainableModel の実行時間を向上させるために、`include_local=False` の場合の mimic explainer に `batch_size` を追加します
  + **azureml-contrib-featureengineering**
    + set_featurizer_timeseries_params () の呼び出しを修正しました: dict 値の型変更と null チェック - `timeseries` フィーチャライザーのノートブックを追加しました
    + NimbusML 依存関係を 1.2.0 バージョン (現在の最新バージョン) に更新します。
  + **azureml-core**
    + AzureML CLI で DBFS データストアをアタッチする機能を追加しました
    + データストアのアップロードに関する問題 (`target_path` が `/` で始まる場合に空のフォルダーが作成されるバグ) を修正しました
    + ServicePrincipalAuthentication の `deepcopy` の問題を修正しました。
    + "az ml environment show" コマンドと "az ml environment list" コマンドを CLI に追加しました。
    + 環境で、already-built base_image の代わりに base_dockerfile をサポートするようになりました。
    + 使用されていない RunConfiguration 設定、auto_prepare_environment は非推奨としてマークされるようになっています。
    + 登録後にモデルの説明を更新できるようになりました
    + バグの修正:モデルとイメージの削除の際、アップストリームの依存関係が原因で削除が失敗した場合に、それらに依存しているアップストリーム オブジェクトの取得に関する詳細情報が提供されるようになりました。
    + 一部の環境でワークスペースの作成時に発生する、デプロイの空白の期間が出力されたバグを修正しました。
    + ワークスペースの作成エラーの例外が改善されました。 ユーザーに対して "Unable to create workspace. Unable to find..." (ワークスペースを作成できません... を検索できません) というメッセージは表示されず、実際の作成エラーが代わりに表示されます。
    + AKS Web サービスのトークン認証のサポートが追加されました。
    + `Webservice` オブジェクトに `get_token()` メソッドを追加します。
    + 機械学習データセットを管理するための CLI サポートを追加しました。
    + `Datastore.register_azure_blob_container` は、必要に応じて、このデータストアのキャッシュの有効期限を有効にする blobfuse のマウント パラメーターを構成する `blob_cache_timeout` 値 (秒単位) を取得するようになりました。 既定では、タイムアウトはありません。つまり、BLOB が読み込まれると、ジョブが完了するまでローカル キャッシュにとどまります。 ほとんどのジョブでこの設定が優先されますが、一部のジョブは、そのノードに収まりきらない大きなデータセットからデータを読み取る必要があります。 このようなジョブでは、このパラメーターを調整すると成功します。 このパラメーターを調整する場合は注意が必要です。値を小さく設定すると、エポックで使用されるデータが再度使用される前に期限切れになる場合があるため、パフォーマンスが低下する可能性があります。 つまり、すべての読み取りがローカル キャッシュではなく BLOB ストレージ (ネットワークなど) から実行され、これがトレーニング時間に悪影響を及ぼします。
    + 登録後にモデルの説明を適切に更新できるようになりました
    + モデルとイメージを削除する場合に、それらに依存するアップストリーム オブジェクト (削除が失敗する原因となります) に関する詳細情報が提供されるようになりました
    + azureml.mlflow を使用するリモート実行のリソース使用率を向上させます。
  + **azureml-explain-model**
    + azureml-contrib-explain-model パッケージの生の特徴の重要度のための LIME Explainer の変換引数を修正しました
    + LimeExplainer の scipy sparse のサポートを追加します
    + 線形モデルを説明する Shape Linear Explainer ラッパーと、別のレベルの Tabular Explainer を追加しました
    + 説明モデル ライブラリの Mimic Explainer で、スパース データ入力で include_local = False の場合のエラーを修正しました
    + `automl` 出力に予期される値を追加します
    + 生の特徴の重要度を取得するために変換引数が指定された場合の、permutation feature importance を修正しました
    + グローバル説明を一括でストリーミングして DecisionTreeExplainableModel の実行時間を向上させるために、`include_local=False` の場合の mimic explainer に `batch_size` を追加します
    + モデル説明ライブラリで、予測のために Pandas データフレーム入力が必要な Blackbox Explainer を修正しました
    + `explanation.expected_values` で、float を含むリストではなく、float が返されることがあったバグを修正しました。
  + **azureml-mlflow**
    + mlflow.set_experiment(experiment_name) のパフォーマンスを向上させます
    + mlflow tracking_uri の InteractiveLoginAuthentication の使用に関するバグを修正します
    + azureml.mlflow を使用するリモート実行のリソース使用率を向上させます。
    + azureml-mlflow パッケージのドキュメントが改善されます
    + mlflow.log_artifacts("my_dir") で、"<artifact-paths>" ではなく "my_dir/<artifact-paths>" の下にアーティファクトが保存されるバグにパッチを適用します
  + **azureml-opendatasets**
    + 新たに発生したメモリの問題により、`opendatasets` の `pyarrow` を古いバージョン (< 0.14.0) に固定します。
    + azureml-contrib-opendatasets を azureml-opendatasets に移動します。
    + オープン データセット クラスを Azure Machine Learning ワークスペースに登録し、AML データセットの機能をシームレスに利用できるようにします。
    + 非 SPARK バージョンでの NoaaIsdWeather のエンリッチ パフォーマンスを大幅に改善しました。
  + **azureml-pipeline-steps**
    + DatabricksStep の入力と出力に対して DBFS Datastore がサポートされるようになりました。
    + Azure Batch Step の入力/出力に関するドキュメントが更新されました。
    + AzureBatchStep で、*delete_batch_job_after_finish* の既定値を *true* に変更しました。
  + **azureml-telemetry**
    +  azureml-contrib-opendatasets を azureml-opendatasets に移動します。
    + オープン データセット クラスを Azure Machine Learning ワークスペースに登録し、AML データセットの機能をシームレスに利用できるようにします。
    + 非 SPARK バージョンでの NoaaIsdWeather のエンリッチ パフォーマンスを大幅に改善しました。
  + **azureml-train-automl**
    + 実際の戻り値の型を反映し、主なプロパティの取得に関する注意事項を追加するために、get_output のドキュメントを更新しました。
    + NimbusML 依存関係を 1.2.0 バージョン (現在の最新バージョン) に更新します。
    + `automl` 出力に予期される値を追加します
  + **azureml-train-core**
    + 自動化されたハイパーパラメーター チューニングのコンピューティング ターゲットとして、文字列が受け入れられるようになりました
    + 使用されていない RunConfiguration 設定、auto_prepare_environment は非推奨としてマークされるようになっています。

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>Azure Machine Learning Data Prep SDK v1.1.9

+ **新機能**
  + http または https の url からファイルを直接読み取るためのサポートが追加されました。

+ **バグの修正と機能強化**
  + リモート ソースから Parquet データセットを読み取ろうとしたとき (現時点ではサポートされていません) に表示されるエラー メッセージが改善されました。
  + ADLS Gen 2 で Parquet ファイル形式に書き込むとき、およびパスの ADLS Gen 2 コンテナー名を更新するときのバグが修正されました。

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>ビジュアル インターフェイス
+ **プレビュー機能**
  + ビジュアル インターフェイスに "R スクリプトの実行" モジュールを追加しました。

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Azure Machine Learning SDK for Python v1.0.48

+ **新機能**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** は、**azureml-opendatasets** として使用できるようになりました。 古いパッケージも引き続き機能しますが、より高度な機能と機能強化を実現するために、今後は **azureml-opendatasets** を使用することをお勧めします。
    + この新しいパッケージを使用すると、オープン データセットを Azure Machine Learning ワークスペースのデータセットとして登録し、そのデータセットで提供される任意の機能を活用できます。
    + これには、オープン データセットを Pandas/SPARK データフレームとして使用したり、天気などの一部のデータセット用に場所を結合するなど、既存の機能も含まれます。

+ **プレビュー機能**
    + HyperDriveConfig では、パイプライン オブジェクトをパラメーターとして受け取れるようになりました。これにより、パイプラインを使用したハイパーパラメーターのチューニングがサポートされます。

+ **バグの修正と機能強化**
  + **azureml-train-automl**
    + 変換後の列の型の損失に関するバグを修正しました。
    + y_query を、先頭に None を含んだオブジェクト型にできるバグを修正しました。
    + アンサンブルの選択手順で、スコアが一定のままであっても、結果のアンサンブルが過剰に増加する問題を修正しました。
    + AutoMLStep の whitelist_models と blacklist_models の設定に関する問題を修正しました。
    + Azure ML パイプラインのコンテキストで AutoML が使用された場合に、前処理の使用が妨げられる問題を修正しました。
  + **azureml-opendatasets**
    + azureml-contrib-opendatasets を azureml-opendatasets に移動しました。
    + オープン データセット クラスを Azure Machine Learning ワークスペースに登録し、AML データセットの機能をシームレスに利用できるようにしました。
    + 非 SPARK バージョンでの NoaaIsdWeather のエンリッチ パフォーマンスを大幅に改善しました。
  + **azureml-explain-model**
    + 解釈可能性オブジェクトのオンライン ドキュメントを更新しました。
    + グローバル説明を一括でストリーミングして、モデル説明ライブラリの DecisionTreeExplainableModel の実行時間を向上させるために、`include_local=False` の場合の mimic explainer に `batch_size` を追加しました。
    + `explanation.expected_values` で、float を含むリストではなく、float が返されることがあった問題を修正しました。
    + 説明モデル ライブラリの mimic explainer の `automl` 出力に、予期される値が追加されました。
    + 生の特徴の重要度を取得するために変換引数が指定された場合の、permutation feature importance を修正しました。
  + **azureml-core**
    + AzureML CLI で DBFS データストアをアタッチする機能を追加しました。
    + データストアのアップロードに関する問題 (`target_path` が `/` で始まる場合に空のフォルダーが作成される問題) を修正しました。
    + 2 つのデータセットを比較できるようになりました。
    + モデルとイメージの削除の際、アップストリームの依存関係が原因で削除が失敗した場合に、それらに依存しているアップストリーム オブジェクトの取得に関する詳細情報が提供されるようになりました。
    + auto_prepare_environment の使用されていない RunConfiguration 設定が非推奨になりました。
  + **azureml-mlflow**
    + azureml.mlflow を使用するリモート実行のリソース使用率が向上しました。
    + azureml-mlflow パッケージのドキュメントが改善されました。
    + mlflow.log_artifacts("my_dir") で、"artifact-paths" ではなく "my_dir/artifact-paths" の下にアーティファクトが保存される問題を修正しました。
  + **azureml-pipeline-core**
    + すべてのパイプライン ステップの hash_paths パラメーターは非推奨となり、今後削除される予定です。 既定では、source_directory の内容はハッシュ化されます (.amlignore または .gitignore に記載されているファイルを除く)
    + コンピューティングの種類に固有のモジュールをサポートするために、Module および ModuleStep の改善を続けています (RunConfiguration の統合と、コンピューティングの種類に固有のモジュールをパイプラインで自由に使用できるようにするためのさらなる変更に備えるため)。
  + **azureml-pipeline-steps**
    + AzureBatchStep:入力/出力に関するドキュメントが改善されました。
    + AzureBatchStep:delete_batch_job_after_finish の既定値を true に変更しました。
  + **azureml-train-core**
    + 自動化されたハイパーパラメーター チューニングのコンピューティング ターゲットとして、文字列が受け入れられるようになりました。
    + auto_prepare_environment の使用されていない RunConfiguration 設定が非推奨になりました。
    + `conda_dependencies_file_path` および `pip_requirements_file_path` パラメーターが非推奨となり、`conda_dependencies_file` と `pip_requirements_file` がそれぞれ推奨パラメーターになりました。
  + **azureml-opendatasets**
    + 非 SPARK バージョンでの NoaaIsdWeather のエンリッチ パフォーマンスを大幅に改善しました。

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v1.1.8

+ **新機能**
 + データフロー オブジェクトを反復処理して、一連のレコードを生成できるようになりました。 `Dataflow.to_record_iterator` のドキュメントを参照してください。
  + データフロー オブジェクトを反復処理して、一連のレコードを生成できるようになりました。 `Dataflow.to_record_iterator` のドキュメントを参照してください。

+ **バグの修正と機能強化**
 + DataPrep SDK の堅牢性が向上しました。
 + 文字列以外の列インデックスを使用した、pandas DataFrames の処理が改善されました。
 + データセットでの `to_pandas_dataframe` のパフォーマンスが大幅に改善されました。
 + マルチノード環境で実行した場合に、データセットの Spark 実行が失敗するバグを修正しました。
  + DataPrep SDK の堅牢性が向上しました。
  + 文字列以外の列インデックスを使用した、pandas DataFrames の処理が改善されました。
  + データセットでの `to_pandas_dataframe` のパフォーマンスが大幅に改善されました。
  + マルチノード環境で実行した場合に、データセットの Spark 実行が失敗するバグを修正しました。

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Prep SDK v1.1.7

パフォーマンス向上のために行った変更は、Azure Databricks を使う一部のお客様で問題の原因になったため、元に戻されました。 Azure Databricks で問題が発生していた場合は、次のいずれかの方法を使ってバージョン 1.1.7 にアップグレードできます。
1. このスクリプトを実行してアップグレードします: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. クラスターを再作成します。そうすると、最新バージョンの Data Prep SDK がインストールされます。

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Azure Machine Learning SDK for Python v1.0.45

+ **新機能**
  + azureml-explain-model パッケージの説明を模倣するためのデシジョン ツリー サロゲート モデルを追加します
  + 推論イメージにインストールされる CUDA のバージョンを指定する機能。 CUDA 9.0、9.1、10.0 のサポート。
  + Azure ML トレーニングの基本イメージに関する情報を、[Azure ML コンテナーの GitHub リポジトリ](https://github.com/Azure/AzureML-Containers)および [DockerHub](https://hub.docker.com/_/microsoft-azureml) で入手できるようになります
  + パイプライン スケジュールの CLI サポートが追加されました。 詳細については "az ml pipeline -h" を実行してください
  + AKS Web サービスのデプロイ構成と CLI に、カスタム Kubernetes 名前空間パラメーターが追加されました。
  + パイプラインのすべてのステップで hash_paths パラメーターが非推奨になりました
  + Model.register で、`child_paths` パラメーターを使って複数の個別ファイルを 1 つのモデルとして登録することがサポートされるようになりました。

+ **プレビュー機能**
    + スコアリング Explainer で、より信頼性の高いシリアル化と逆シリアル化のために、必要に応じて Conda と PIP の情報を保存できるようになりました。
    + 自動機能選択のバグの修正。
    + mlflow.azureml.build_image を新しい API に更新し、新しい実装によって発生するようになったバグに修正プログラムを適用しました。

+ **バグの修正と機能強化**
  + azureml-core から `paramiko` の依存関係が削除されました。 従来のコンピューティング先のアタッチ方法に対する非推奨の警告が追加されました。
  + run.create_children のパフォーマンスが向上しました
  + バイナリ分類器を使う Mimic Explainer で、教師の確率が SHAPE 値のスケーリングに使われるときの、確率の順序が修正されました。
  + 自動化された機械学習に対するエラー処理とメッセージが改善されました。
  + 自動化された機械学習のイテレーションのタイムアウトの問題が修正されました。
  + 自動化された機械学習に対する時系列変換のパフォーマンスが向上しました。

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v1.1.6

+ **新機能**
  + 上位の値 (`SummaryFunction.TOPVALUES`) と下位の値 (`SummaryFunction.BOTTOMVALUES`) に対する集計関数が追加されました。

+ **バグの修正と機能強化**
  + `read_pandas_dataframe` のパフォーマンスが大幅に向上しました。
  + バイナリ ファイルを指し示すデータフローで `get_profile()` が失敗する原因のバグを修正しました。
  + テレメトリの収集をプログラムで有効/無効にできる `set_diagnostics_collection()` が公開されました。
  + `get_profile()` の動作が変更されました。 Min、Mean、Std、Sum に対して NaN 値が無視されるようになります。これは、Pandas の動作と一致します。


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Azure Machine Learning SDK for Python v1.0.43

+ **新機能**
  + Azure Machine Learning で、人気のある機械学習およびデータ分析フレームワークである Scikit-learn の優れたサポートが提供されるようになりました。 [`SKLearn` 見積もりツール](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) を使うと、ユーザーは Scikit-learn のモデルを簡単にトレーニングしてデプロイできます。
    + [HyperDrive を使用して Scikit-learn でハイパーパラメーター調整を実行する](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb)方法をご覧ください。
  + 再利用可能なコンピューティング ユニットを管理するための Module および ModuleVersion クラスと共に、パイプラインで ModuleStep を作成するためのサポートが追加されました。
  + ACI Web サービスで、更新の間の永続的な scoring_uri がサポートされるようになりました。 scoring_uri が IP から FQDN に変更されます。 FQDN の DNS 名ラベルは、deploy_configuration で dns_name_label を設定することによって構成できます。
  + 自動化された機械学習の新機能:
    + 予測用の STL フィーチャライザー
    + K-Means クラスタリングが機能のスイープに対して有効になります
  + AmlCompute クォータの承認が速くなりました。 しきい値内でのクォータの要求を承認するプロセスが自動化されました。 クォータの動作方法の詳細については、[クォータの管理方法](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas)に関する記事をご覧ください。

+ **プレビュー機能**
    + azureml-mlflow パッケージによる [MLflow](https://mlflow.org) 1.0.0 の追跡との統合 ([ノートブックの例](https://aka.ms/azureml-mlflow-examples))。
    + 実行として Jupyter Notebook を送信します。 [API リファレンス ドキュメント](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + azureml-contrib-datadrift パッケージによる [Data Drift Detector](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) のパブリック プレビュー ([ノートブックの例](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/monitor-models/data-drift))。 データの誤差は、モデルの精度が時間の経過と共に低下する主な理由の 1 つです。 これは運用環境のモデルに提供されるデータが、モデルがトレーニングされたデータと異なる場合に発生します。 AML Data Drift Detector は、お客様がデータの誤差を監視し、誤差が検出されるたびにアラートを送信するのに役立ちます。

+ **重大な変更**

+ **バグの修正と機能強化**
  + RunConfiguration の読み込みと保存では、以前の動作に対する完全な下位互換性を備えた完全なファイル パスの指定がサポートされます。
  + ServicePrincipalAuthentication でのキャッシュ機能が追加されました。既定ではオフになります。
  + 同じメトリック名で複数のプロットをログ記録できます。
  + Model クラスを、azureml.core (`from azureml.core import Model`) から正しくインポートできるようになりました。
  + パイプラインのステップで、`hash_path` パラメーターが非推奨になりました。 新しい動作では、.amlignore または .gitignore にリストされているファイルを除き、完全な source_directory がハッシュされます。
  + パイプラインのパッケージでは、さまざまな `get_all` および `get_all_*` メソッドが非推奨になり、それぞれ代わりに `list` および `list_*` が使用されます。
  + azureml.core.get_run では、元の実行型を返す前に、クラスをインポートする必要がなくなります。
  + WebService Update の一部の呼び出しで更新がトリガーされなかった問題を修正しました。
  + AKS Web サービスでのスコアリング タイムアウトは、5 ミリ秒から 300000 ミリ秒の間である必要があります。 スコアリング要求に対して許容される最大 scoring_timeout_ms が、1 分から 5 分に引き上げられました。
  + LocalWebservice オブジェクトに、`scoring_uri` プロパティと `swagger_uri` プロパティが追加されました。
  + 出力ディレクトリの作成と出力ディレクトリのアップロードが、ユーザー プロセスの外に移動されました。 すべてのユーザー プロセス内で、実行履歴 SDK を実行できるようになりました。 これにより、分散トレーニングの実行で発生していたいくつかの同期の問題が解決されるはずです。
  + ユーザーのプロセス名から書き込まれる azureml ログの名前に、プロセス名 (分散トレーニングの場合のみ) と PID が含まれるようになりました。

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v1.1.5

+ **バグの修正と機能強化**
  + 2 桁の年形式を使用する解釈された datetime 値では、有効な年の範囲が Windows May リリースと一致するように更新されました。 範囲が、1930 - 2029 から 1950 - 2049 に変更されました。
  + ファイルおよび設定 `handleQuotedLineBreaks=True` を読み取るときに、`\r` は改行として扱われます。
  + 場合によって `read_pandas_dataframe` が失敗する原因であったバグが修正されました。
  + `get_profile` のパフォーマンスが向上しました。
  + エラー メッセージが改善されました。

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v1.1.4

+ **新機能**
  + 次の式言語関数を使用して、datetime 値を新しい列に抽出して解析できるようになりました。
    + `RegEx.extract_record()` は datetime 要素を新しい列に抽出します。
    + `create_datetime()` は別の datetime 要素から datetime オブジェクトを作成します。
  + `get_profile()` を呼び出すとき、分位点の列に (est.) というラベルが付けられ、値が近似値であることを明確に判別できるようになりました。
  + Azure Blob Storage からの読み取り時に ** グロビングを使用できるようになりました。
    + 例: `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **バグの修正**
  + リモート ソース (Azure Blob) からの Parquet ファイルの読み取りに関連するバグを修正しました。

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine Learning SDK for Python v1.0.39
+ **変更点**
  + 実行構成の auto_prepare_environment オプションは非推奨になり、自動準備が既定になります。

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Prep SDK v1.1.3

+ **新機能**
  + PostgresSQL データベースから読み取るためのサポートが追加されました。read_postgresql を呼び出すか、データストアを使用します。
    + ハウツー ガイドの例を参照してください。
      + [データ インジェスト ノートブック](https://aka.ms/aml-data-prep-ingestion-nb)
      + [データストア ノートブック](https://aka.ms/aml-data-prep-datastore-nb)

+ **バグの修正と機能強化**
  + 列の型変換の問題が修正されました。
  + ブール値または数値の列がブール値の列に正しく変換されるようになりました。
  + 日付列に日付型を設定しようとしたときに失敗することはなくなりました。
  + JoinType 型と付属のドキュメントが改善されました。 2 つのデータフローを結合するときに、次の結合型のいずれかを指定できるようになりました。
    + NONE、MATCH、INNER、UNMATCHLEFT、LEFTANTI、LEFTOUTER、UNMATCHRIGHT、RIGHTANTI、RIGHTOUTER、FULLANTI、FULL
  + より多くの日付の形式を認識するようにデータ型の推論が改善されました。

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure portal

Azure portal で、次のことが可能になりました。
+ 自動化された ML の実験を作成して実行する
+ Notebook VM を作成して、サンプルの Jupyter ノートブック、または独自のノートブックを試す。
+ Azure Machine Learning ワークスペースのまったく新しい作成セクション (プレビュー)。自動化された機械学習、ビジュアル インターフェイス、ホストされている Notebook VM が含まれます
    + 自動化された機械学習を使用して自動的にモデルを作成する
    + ドラッグ アンド ドロップのビジュアル インターフェイスを使用して実験を実行する
    + Notebook VM を作成してデータの探索、モデルの作成、サービスのデプロイを行う。
+ 実行レポートと実行詳細ページでのライブ グラフとメトリックの更新
+ 実行詳細ページでのログ、出力、スナップショットのファイル ビューアーの更新。
+ 新しくなった、または改善された [実験] タブのレポート作成エクスペリエンス。
+ Azure Machine Learning ワークスペースの [概要] ページから config.json ファイルをダウンロードする機能の追加。
+ Azure Databricks ワークスペースからの Azure Machine Learning ワークスペース作成のサポート。

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine Learning SDK for Python v1.0.33
+ **新機能**
  + _Workspace.create_ メソッドは、CPU および GPU クラスターの既定のクラスター構成を受け入れるようになりました。
  + ワークスペースの作成に失敗する場合、依存リソースはクリーンアップされます。
  + 既定の Azure Container Registry SKU が Basic に切り替えられました。
  + Azure Container Registry は、実行またはイメージの作成に必要なときに遅延作成されます。
  + トレーニングの実行に対する環境をサポートします。

### <a name="notebook-virtual-machine"></a>Notebook Virtual Machine 

Notebook VM は Jupyter ノートブック向けのセキュリティで保護された、エンタープライズ対応のホスティング環境で使用します。ここで、機械学習の実験をプログラミングしたり、モデルを Web エンドポイントとしてデプロイしたり、Python を使用して Azure Machine Learning SDK でサポートされる他のすべての操作を実行したりすることができます。 次のような機能が提供されます。
+ 最新バージョンの Azure Machine Learning SDK と関連パッケージが含まれる、[構成済みのノートブック VM の迅速な起動](tutorial-1st-experiment-sdk-setup.md) 。
+ アクセスは、HTTPS、Azure Active Directory の認証と認可など、実績のあるテクノロジによってセキュリティ保護されます。
+ Azure Machine Learning ワークスペースの BLOB ストレージ アカウント内にある、ノートブックとコードの信頼性の高いクラウド ストレージ。 作業内容を失わずに、ノートブックの VM を安全に削除できます。
+ Azure Machine Learning の機能を使用して探索と実験を行うためにプレインストールされたサンプル ノートブック。
+ Azure VM、すべての種類の VM、すべてのパッケージ、すべてのドライバーの完全なカスタマイズ機能。 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Azure Machine Learning SDK for Python v1.0.33 がリリースされました。

+ [FPGA](how-to-deploy-fpga-web-service.md) で Azure ML Hardware Accelerated Models の一般提供が開始されました。
  + [azureml-accel-models パッケージ](how-to-deploy-fpga-web-service.md)を使用して次のことができるようになりました。
    + サポートされているディープ ニューラル ネットワーク (ResNet 50、ResNet 152、DenseNet-121、VGG-16、および SSD-VGG) の重み付けトレーニング
    + サポートされている DNN を使用した転移学習の使用
    + モデル管理サービスへのモデルの登録と、モデルのコンテナー化
    + Azure Kubernetes Service (AKS) クラスターの FPGA を使用した Azure VM へのモデルのデプロイ
  + [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) サーバー デバイスへのコンテナーのデプロイ
  + この[サンプル](https://github.com/Azure-Samples/aml-hardware-accelerated-models)を使用した gRPC エンドポイントでのデータのスコア付け

### <a name="automated-machine-learning"></a>自動化された機械学習

+ パフォーマンス最適化のため、機能を一掃し、動的に追加される:::no-loc text="featurizers":::フィーチャライザーを有効化。 新しい:::no-loc text="featurizers":::フィーチャライザー: 作業の埋め込み、証拠の重み付け、ターゲット エンコード、テキスト ターゲット エンコード、クラスターの距離
+ 自動化された ML 内でトレーニング/有効な分割を処理するスマート CV
+ わずかなメモリ最適化の変更とランタイムのパフォーマンス向上
+ モデルの説明でのパフォーマンス向上
+ ローカル実行用 ONNX モデルの変換
+ サブサンプリング サポートの追加
+ 終了条件が定義されていない場合のインテリジェントな停止
+ スタッキング アンサンブル

+ 時系列予測
  + 新しい予測関数
  + 時系列データに対してローリング オリジンのクロス検証を使用できるようになりました
  + 時系列の遅れを構成する新機能の追加
  + ローリング ウィンドウの集計機能をサポートする新機能の追加
  + 実験の設定で国番号が定義されている場合の、新しい祝日の検出とフィーチャライザー

+ Azure Databricks
  + 時系列予測と、モデルの説明可能性/解釈可能性機能を有効化しました
  + 自動化された ML の実験の取り消しと再開 (続行) ができるようになりました
  + マルチコア処理のサポートを追加しました

### <a name="mlops"></a>MLOps
+ **スコア付けコンテナーのローカル デプロイとデバッグ**<br/> ML モデルをローカルにデプロイして、スコアリング ファイルと依存関係上ですばやく反復できるようになったため、これらが確実に予期したとおりに動作するようになりました。

+ **InferenceConfig と Model.deploy() の導入**<br/> モデル デプロイで、エントリ スクリプトが含まれるソース フォルダーの指定がサポートされるようになりました (RunConfig と同様)。  また、モデル デプロイが 1 つのコマンドに簡素化されました。

+ **Git 参照の追跡**<br/> お客様から、エンドツーエンドの監査証跡の維持に役立つ、基本的な Git 統合機能についてのご希望をいただいてきました。 Microsoft は Azure ML の主要エンティティに対する Git 関連メタデータ (リポジトリ、コミット、クリーンな状態) の追跡を実装しました。 この情報は、SDK と CLI によって自動的に収集されます。

+ **モデルのプロファイルと検証のサービス**<br/> お客様からよくいただくご不満のなかに、推論サービスに対応付けるコンピューティングの適切なサイズ設定が難しいというものがありました。 Microsoft のモデル プロファイル サービスではユーザーがサンプル入力を指定できるため、16 種類の CPU/メモリ設定をプロファイルして、デプロイに最適なサイズを決定できます。

+ **推論に独自の基本イメージを使用可能**<br/> もう 1 つ多くいただいたご不満は、実験から推論の依存関係の再共有に移行することが難しいというものでした。 基本イメージの新しい共有機能により、実験の基本イメージ、依存関係などすべてを推論に再利用できるようになりました。 これにより、デプロイが高速化し、内側から外側のループへのギャップが少なくなります。

+ **Swagger スキーマの生成エクスペリエンスの向上**<br/> 以前の Swagger 生成メソッドではエラーが発生しやすく、自動化ができませんでした。 デコレーターを使用してどの Python 関数からでも Swagger スキーマをインラインで生成できるようになりました。 Microsoft はこのコードをオープンソース化しました。このスキーマ生成プロトコルは Azure ML プラットフォームと結合されていません。

+ **Azure ML CLI が一般公開 (GA)**<br/> 1 つの CLI コマンドでモデルをデプロイできるようになりました。 ユーザーから、Jupyter ノートブックから誰も ML モデルをデプロイしていないという共通したフィードバックを受け取っています。 [**CLI リファレンス ドキュメント**](https://aka.ms/azmlcli)が更新されました。


## <a name="2019-04-22"></a>2019-04-22

Azure Machine Learning SDK for Python v1.0.30 がリリースされました。

同じエンドポイントを維持しながら、新しいバージョンの公開されたパイプラインを追加するための [`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) が導入されました。

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK v1.1.2

注:Data Prep Python SDK では、`numpy` および `pandas` パッケージがインストールされなくなります。 [更新されたインストール手順](https://github.com/Microsoft/AMLDataPrepDocs)に関するページを参照してください。

+ **新機能**
  + ピボット変換を使用できるようになりました。
    + 攻略ガイド: [ピボット ノートブック](https://aka.ms/aml-data-prep-pivot-nb)
  + ネイティブ関数内に正規表現を使用できるようになりました。
    + 例 :
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + 式言語内で `to_upper`  関数と `to_lower`  関数を使用できるようになりました。
  + データ プロファイル内に各列の一意の値が表示されるようになりました。
  + 一般的に使用されるリーダー手順の一部で、`infer_column_types` 引数を渡せるようになりました。 それが `True` に設定されている場合、Data Prep では列の型の検出およびその自動変換が試みられます。
    + `inference_arguments` は非推奨となりました。
  + `Dataflow.shape` を呼び出せるようになりました。

+ **バグの修正と機能強化**
  + 省略可能な追加の引数 `validate_column_exists` が `keep_columns`  によって受け入れられるようになりました。それが渡されると、`keep_columns` の結果に任意の列が含まれているかどうかが確認されます。
  + すべてのリーダー手順 (ファイルから読み取られる) で、省略可能な追加の引数 `verify_exists` が受け入れられるようになりました。
  + Pandas データ フレームからの読み取り、およびデータ プロファイルの取得のパフォーマンスが改善されました。
  + 単一インデックスの場合にデータ フローからの単一手順のスライシングが失敗するというバグが修正されました。

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + 既存のリモート コンピューティング クラスターで実行されている既存のスクリプトを再送信できるようになりました。
  + [パイプライン] タブで、新しいパラメーターで発行されたパイプラインを実行できるようになりました。
  + 詳細の実行で、新しいスナップショット ファイル ビューアーがサポートされるようになりました。 特定の実行を送信したときのディレクトリのスナップショットを表示することができます。 また、実行を開始するために送信されたノートブックをダウンロードすることもできます。
  + Azure portal からの親の実行をキャンセルできるようになりました。

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine Learning SDK for Python v1.0.23

+ **新機能**
  + Azure Machine Learning SDK で Python 3.7 がサポートされるようになりました。
  + Azure Machine Learning DNN Estimator で、組み込みマルチバージョン サポートが提供されるようになりました。 たとえば、`TensorFlow`  Estimator は `framework_version` パラメーターを受け入れるようになり、ユーザーはバージョン '1.10' や '1.12' を指定することができます。 現在の SDK リリースでサポートされているバージョンの一覧については、目的のフレームワーク クラスで `get_supported_versions()` を呼び出します (例: `TensorFlow.get_supported_versions()`)。
  最新の SDK リリースでサポートされているバージョンの一覧については、[DNN Estimator のドキュメント](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py)を参照してください。

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK v1.1.1

+ **新機能**
  + read_* transforms を使用して複数の Datastore/DataPath/DataReference ソースを読み取ることができます。
  + 列で次の演算を実行して新しい列を作成することができます: division、floor、modulo、power、length。
  + Data Prep が Azure ML 診断スイートの一部になり、既定で診断情報がログに記録されるようになりました。
    + これをオフにするには、次の環境変数を true に設定します:DISABLE_DPREP_LOGGER

+ **バグの修正と機能強化**
  + 一般的に使用されるクラスと関数のコード ドキュメントが改善されました。
  + Excel ファイルの読み取りに失敗した auto_read_file のバグが修正されました。
  + read_pandas_dataframe のフォルダーを上書きするオプションが追加されました。
  + dotnetcore2 依存関係のインストールのパフォーマンスが改善され、Fedora 27/28 と Ubuntu 1804 のサポートが追加されました。
  + Azure Blob からの読み取りのパフォーマンスが改善されました。
  + 列の型の検出で、Long 型の列がサポートされるようになりました。
  + 一部の日付値が Python の datetime オブジェクトではなく timestamp として表示されていたバグが修正されました。
  + 一部の型カウントが integer ではなく double として表示されていたバグを修正しました。


## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine Learning SDK for Python v1.0.21

+ **新機能**
  + *azureml.core.Run.create_children* メソッドにより、1 回の呼び出しで複数の子の実行を少ない待機時間で作成することができます。

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Prep SDK v1.1.0

+ **重大な変更**
  + Data Prep パッケージの概念は非推奨になり、サポートされなくなりました。 1 つのパッケージで複数のデータフローを保持する代わりに、データフローを個別に保持することができます。
    + 攻略ガイド: [データフローのノートブックの開始と保存](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **新機能**
  + Data Prep で特定のセマンティックの種類と一致する列を認識して、適切に分割できるようになりました。 現在サポートされているセマンティックの種類には、メール アドレス、地理的座標 (緯度と経度)、IPv4 と IPv6 のアドレス、米国の電話番号、米国の郵便番号などがあります。
    + 攻略ガイド: [セマンティックの種類のノートブック](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Data Prep で、2 つの数値列から結果列を生成する減算、乗算、除算、剰余の操作をサポートするようになりました。
  + データフローで `verify_has_data()` を呼び出し、データフローが実行された場合にレコードが生成されるかどうかをチェックすることができます。

+ **バグの修正と機能強化**
  + 数値列のプロファイルで、ヒストグラムに使用するビンの数を指定できるようになりました。
  + `read_pandas_dataframe` 変換で、データフレームに文字列型またはバイト型の列名が求められるようになりました。
  + `fill_nulls` 変換の列が入力されていない場合に値が正しく入力されないバグを修正しました。

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine Learning SDK for Python v1.0.18

 + **変更点**
   + azureml-contrib-tensorboard が azureml-tensorboard パッケージに置き換えられました。
   + このリリースでは、マネージド コンピューティング クラスター (amlcompute) の作成時にユーザー アカウントを設定できます。 これは、これらのプロパティをプロビジョニング構成に渡すことで実行できます。 詳細については、[SDK リファレンス ドキュメント](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--)をご覧ください。

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Prep SDK v1.0.17

+ **新機能**
  + 式言語を使用して結果列を生成するために、2 つの数値列の追加がサポートされるようになりました。

+ **バグの修正と機能強化**
  + random_split のドキュメントとパラメーター チェックが改善されました。

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Prep SDK v1.0.16

+ **バグ修正**
  + API の変更が原因で発生していたサービス プリンシパルの認証の問題を修正しました。

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine Learning SDK for Python v1.0.17

+ **新機能**

  + Azure Machine Learning で、一般的な DNN フレームワーク Chainer のファースト クラスのサポートが提供されるようになりました。 [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) クラスを使用すると、Chainer モデルを簡単にトレーニングしてデプロイできます。
    + [ChainerMN を使用して分散トレーニングを実行する](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/training/distributed-chainer/distributed-chainer.ipynb)方法をご覧ください。
    + [HyperDrive を使用して Chainer でハイパーパラメーター調整を実行する](https://github.com/Azure/MachineLearningNotebooks/blob/b881f78e4658b4e102a72b78dbd2129c24506980/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)方法をご覧ください。
  + Azure Machine Learning パイプラインに、データストアの変更に基づいてパイプライン実行をトリガーする機能が追加されました。 この機能を紹介するために、パイプラインの[スケジュール ノートブック](https://aka.ms/pl-schedule)が更新されました。

+ **バグの修正と機能強化**
  + [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) に提供される [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) で、source_directory_data_store プロパティを必要なデータストア (Blob ストレージなど) に設定するために、Azure Machine Learning パイプラインのサポートが追加されました。 既定では、各ステップでバッキング データストアとして Azure File ストアが使用されますが、多数のステップが同時に実行されると、調整の問題が発生する可能性があります。

### <a name="azure-portal"></a>Azure portal

+ **新機能**
  + テーブル エディターでレポートを作成する際の新しいドラッグ アンド ドロップ エクスペリエンス。 ユーザーは、ウェルから、テーブルのプレビューが表示されるテーブル領域に列をドラッグできます。 列を並べ替えることができます。
  + 新しいログ ファイル ビューアー
  + アクティビティ タブから実験の実行、計算、モデル、イメージ、デプロイへのリンク

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Prep SDK v1.0.15

+ **新機能**
  + Data Prep で、データフローからのファイル ストリームの書き込みがサポートされるようになりました。 また、ファイル ストリーム名を操作して新しいファイル名を作成する機能も用意されています。
    + 攻略ガイド: [File Streams ノートブックの操作](https://aka.ms/aml-data-prep-file-stream-nb)

+ **バグの修正と機能強化**
  + 大規模なデータセットでの T-Digest のパフォーマンスが向上しました。
  + Data Prep で、DataPath からのデータの読み取りがサポートされるようになりました。
  + ブール値列と数値列でワン ホット エンコードが機能するようになりました。
  + その他の各種バグ修正。

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine Learning SDK for Python v1.0.15

+ **新機能**
  + Azure Machine Learning パイプラインに、AzureBatchStep ([ノートブック](https://aka.ms/pl-azbatch))、HyperDriveStep (ノートブック)、時間ベースのスケジューリング機能 ([ノートブック](https://aka.ms/pl-schedule)) が追加されました。
  +  Azure SQL Database および PostgreSQL 用の Azure データベース ([ノートブック](https://aka.ms/pl-data-trans)) と連携するように DataTranferStep が更新されました。

+ **変更点**
  + `PublishedPipeline.get` を優先して、`PublishedPipeline.get_published_pipeline` を非推奨にしました。
  + `Schedule.get` を優先して、`Schedule.get_schedule` を非推奨にしました。

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v1.0.12

+ **新機能**
  + Data Prep で、データ ストアを使用した Azure SQL データベースからの読み取りがサポートされるようになりました。

+ **変更点**
  + 大きなデータに対する特定の操作のメモリ パフォーマンスが向上しました。
  + `read_pandas_dataframe()` では `temp_folder` を指定することが必要になりました。
  + `ColumnProfile` での `name` プロパティは非推奨です。代わりに `column_name` を使用してください。

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine Learning SDK for Python v1.0.10

+ **変更点**:
  + Azure ML SDK では、依存関係としての azure-cli パッケージは不要になりました。 具体的には、azureml-core から azure-cli-core 依存関係と azure-cli-profile 依存関係が削除されました。 ユーザーに影響がある変更を以下に示します。
      + "az login" を実行し、その後 azureml-sdk を使用すると、この SDK はブラウザーまたはデバイス コードのログインをもう一度行います。 "az login" によって作成された資格情報の状態は使用されません。
    + "az login" を使用するなどの Azure CLI 認証の場合、_azureml.core.authentication.AzureCliAuthentication_ クラスを使用します。 Azure CLI 認証では、azureml-sdk をインストールした Python 環境で _pip install azure-cli_ を実行します。
    + 自動化のためのサービス プリンシパルを使用して "az login" を実行する場合、azureml-sdk では Azure CLI によって作成された資格情報の状態が使用されないため、_azureml.core.authentication.ServicePrincipalAuthentication_ クラスを使用することをお勧めします。

+ **バグの修正**:このリリースには主に、軽微なバグの修正が含まれます

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Prep SDK v1.0.8

+ **バグの修正**
  + データ プロファイルの取得におけるパフォーマンスが向上しました。
  + エラー報告に関連する軽微なバグが修正されました。

### <a name="azure-portal-new-features"></a>Azure portal: 新機能
+ レポート作成のためのドラッグ アンド ドロップによる新しいグラフ作成エクスペリエンス。 ユーザーは、列または属性を元の場所からグラフ領域にドラッグできます。ドラッグすると、データの種類に基づき、ユーザーに対して適切なグラフの種類が自動的に選択されます。 ユーザーは、グラフの種類を別の適用可能な種類に変更することや、属性をさらに追加することができます。

    サポートされるグラフの種類:
    - 折れ線グラフ
    - ヒストグラム
    - 積み上げ横棒グラフ
    - 箱ひげ図
    - 散布図
    - バブル プロット
+ ポータルでは、実験のレポートが動的に生成されるようになりました。 ユーザーが実験に対して実行を発行すると、ログに記録されたメトリックとグラフを含むレポートが自動的に生成され、異なる実行を比較できます。

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine Learning SDK for Python v1.0.8

+ **バグの修正**:このリリースには主に、軽微なバグの修正が含まれます

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v1.0.7

+ **新機能**
  + データストアの機能強化 ([データストアのハウツーガイド](https://aka.ms/aml-data-prep-datastore-nb)参照)
    + スケールアップ時に Azure ファイル共有と ADLS データストアに対する読み取りと書き込みを行う機能が追加されました。
    + データストアの使用時に、データの準備で対話型認証ではなくサービス プリンシパル認証の使用がサポートされるようになりました。
    + WASB URL と WASBS URL のサポートが追加されました。

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Prep SDK v1.0.6

+ **バグの修正**
  + Spark 上のパブリックに読み取り可能な Azure BLOB コンテナーからの読み取りでのバグを修正しました

## <a name="2018-12-20"></a>2018-12-20

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine Learning SDK for Python v1.0.6
+ **バグの修正**:このリリースには主に、軽微なバグの修正が含まれます

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Prep SDK v1.0.4

+ **新機能**
  + `to_bool` 関数では、一致しない値を Error 値に変換できるようになりました。 これは、`to_bool` および `set_column_types` の新しい既定の不一致動作です。一方、以前の既定の動作では一致しない値を False に変換していました。
  + `to_pandas_dataframe` を呼び出すときに、数値列の null 値/欠落値を NaN と解釈する新しいオプションがあります。
  + 一部の式の戻り値の型をチェックして、型の一貫性と失敗を早期に確認する機能が追加されました。
  + `parse_json` を呼び出して、列内の値を JSON オブジェクトとして解析し、それらを複数の列に拡張できるようになりました。

+ **バグの修正**
  + Python 3.5.2 で `set_column_types` をクラッシュしていたバグを修正しました。
  + AML イメージを使用してデータストアに接続するときにクラッシュしていたバグを修正しました。

+ **更新プログラム**
  * 使用開始チュートリアル、ケース スタディ、および攻略ガイドの[サンプルの Notebook](https://aka.ms/aml-data-prep-notebooks)。

## <a name="2018-12-04-general-availability"></a>2018-12-04: 一般公開

Azure Machine Learning の一般提供が開始されました。

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning コンピューティング
このリリースでは、[Azure Machine Learning コンピューティング](how-to-set-up-training-targets.md#amlcompute)を通じて、新しいマネージド コンピューティング エクスペリエンスを発表します。 このコンピューティング ターゲットは、Azure Machine Learning の Azure Batch AI コンピューティングの後継となります。

このコンピューティング ターゲットは、
+ モデル トレーニングとバッチ推論に使用されます
+ シングルノードからマルチノードのコンピューティングです
+ クラスター管理とジョブ スケジューリングをユーザーに代わって実行します
+ 既定で自動スケーリングを実行します
+ CPU と GPU の両方のリソースをサポートします
+ コストを抑えるために低優先度の VM を使用できるようにします

Azure Machine Learning コンピューティングは、Python、Azure portal、または CLI を使用して作成できます。 お客様のワークスペースのリージョンに作成する必要があり、他のワークスペースにアタッチすることはできません。 このコンピューティング ターゲットでは、お客様の実行に Docker コンテナーを使用しており、お客様の依存関係をパッケージ化することですべてのノードで同じ環境を再現します。

> [!Warning]
> Azure Machine Learning コンピューティングを使用するには、新しいワークスペースを作成することをお勧めします。 ユーザーが Azure Machine Learning コンピューティングを既存のワークスペースで作成しようとしてエラーが発生することはまずありません。 お客様のワークスペースにある既存のコンピューティングは、引き続き支障なく動作します。

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine Learning SDK for Python v1.0.2
+ **重大な変更**
  + このリリースでは、Azure Machine Learning で VM の作成のサポートが廃止されます。 引き続き、既存のクラウド VM またはリモートのオンプレミス サーバーをアタッチすることができます。
  + また、Batch AI のサポートも廃止となります。そのすべては今後、Azure Machine Learning コンピューティングを通じてサポートされます。

+ **[新規作成]**
  + 機械学習パイプライン:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **更新**
  + 機械学習パイプライン:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) で runconfig が使用可能になりました
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) で SQL データソースとの間のコピーが可能になりました
    + パブリッシュ済みのパイプラインの実行に関するスケジュールを作成および更新できる SDK のスケジュール機能

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning Data Prep SDK v0.5.2
+ **重大な変更**
  * `SummaryFunction.N` の名前が `SummaryFunction.Count` に変更されました。

+ **バグの修正**
  * リモート実行でデータストアに対する読み書きを行う際、最新の AML Run Token を使用します。 以前は、AML Run Token が Python で更新された場合、Data Prep ランタイムが最新の AML Run Token によって更新されませんでした。
  * より明確なエラー メッセージの追加
  * Spark によって `Kryo` シリアル化が使用される際に to_spark_dataframe() がクラッシュすることがなくなりました
  * 値のカウント インスペクターで表示できる一意の値が 1,000 件を超えました
  * ランダム分割が、元の Dataflow に名前がない場合に失敗しなくなりました

+ **詳細情報**
  * [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>ドキュメントとノートブック
+ ML パイプライン
  + パイプラインの概要、バッチ スコーピング、スタイル転送の例についての新しいノートブックと更新されたノートブック: https://aka.ms/aml-pipeline-notebooks
  + [最初のパイプラインを作成する](how-to-create-your-first-pipeline.md)方法について
  + [パイプラインを使用してバッチ予測を実行](how-to-use-parallel-run-step.md)する方法について
+ Azure Machine Learning コンピューティング ターゲット
  + 新しいマネージド コンピューティングを使用するように[サンプル ノートブック](https://aka.ms/aml-notebooks)が更新されました。
  + [このコンピューティングについて](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portal: 新機能
+ ポータルで [Azure Machine Learning コンピューティング](how-to-set-up-training-targets.md#amlcompute) タイプを作成して管理します。
+ Azure Machine Learning コンピューティングのクォータ使用率を監視し、[クォータを要求](how-to-manage-quotas.md)します。
+ Azure Machine Learning コンピューティング クラスターの状態をリアルタイムで表示します。
+ Azure Machine Learning コンピューティングと Azure Kubernetes Service の作成用に仮想ネットワークのサポートが追加されました。
+ 既存のパラメーターを使用してお客様のパブリッシュ済みのパイプラインを再実行します。
+ 分類モデル (リフト、ゲイン、キャリブレーション、モデルの説明可能性を備えた特徴重要度グラフ) と回帰モデル (残差、およびモデルの説明可能性を備えた特徴重要度グラフ) のための新しい[自動化された機械学習グラフ](how-to-understand-automated-ml.md)。
+ パイプラインを Azure portal で表示できます




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning SDK for Python v0.1.80

+ **重大な変更**
  * *azureml.train.widgets* 名前空間は、*azureml.widgets* に移動されました。
  * *azureml.core.compute.AmlCompute* では、*azureml.core.compute.BatchAICompute* クラスと *azureml.core.compute.DSVMCompute* クラスが非推奨になりました。 前者のクラスは、今後のリリースで削除されます。 現在、AmlCompute クラスには以前の定義があり、単に vm_size と max_nodes が必要で、ジョブが送信されたときにご利用のクラスターは 0 から max_nodes に自動的にスケーリングされます。 [サンプル ノートブック](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training)は、この情報で更新されており、使用方法の例が提供されます。 この単純化と今後のリリースで登場するより魅力あふれる多くの機能にご満足いただければ幸いです。

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v0.5.1

Data Prep SDK の詳細については、[リファレンス ドキュメント](https://aka.ms/data-prep-sdk)を参照してください。
+ **新機能**
   * DataPrep パッケージを実行して、データセットまたはデータフローに対するデータ プロファイルを表示するために、新しい DataPrep CLI が作成されました
   * 使いやすさを向上させるために SetColumnType API が再設計されました
   * smart_read_file から auto_read_file に名前が変更されました
   * データ プロファイルに傾斜と尖度が含まれるようになりました
   * 層化抽出法でサンプリングすることができます
   * CSV ファイルを含む zip ファイルからの読み取りが可能になりました
   * ランダム分割でデータセットを行型に分割できます (例: test-train セットに分割)
   * `.dtypes` を呼び出すことでデータフローまたはデータ プロファイルから列のデータ型をすべて取得できます
   * `.row_count` を呼び出すことでデータフローまたはデータ プロファイルから行数を取得できます

+ **バグの修正**
   * 長整数型から倍精度浮動小数点型の変換が修正されました
   * 列の追加後のアサートが修正されました
   * 場合によってはグループが検出されない FuzzyGrouping での問題が修正されました
   * 複数列の並べ替え順を優先する並べ替え関数が修正されました
   * `pandas` で処理される方法と同様になるように AND/OR 式が修正されました
   * dbfs パスからの読み取りが修正されました
   * エラー メッセージがわかりやすくなりました
   * AML トークンを使用してリモート コンピューティング先で読み取るときに、失敗しないようになりました
   * Linux DSVM 上で失敗しないようになりました
   * 文字列以外の値が文字列の述語にある場合にクラッシュしなくなりました
   * Dataflow が正常に失敗する必要があるときに、アサーション エラーが処理されるようになりました
   * Azure Databricks 上に保存場所をマウントした dbutils がサポートされるようになりました

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal
Azure Machine Learning の Azure portal では、次の更新が加えられました。
  * パブリッシュされたパイプライン用の新しい **[パイプライン]** タブ。
  * 既存の HDInsight クラスターをコンピューティング ターゲットとしてアタッチする操作が新たにサポートされました。

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine Learning SDK for Python v0.1.74

+ **重大な変更**
  * \* Workspace.compute_targets、datastores、experiments、images、models、および *webservices* が、メソッドではなく、プロパティになりました。 たとえば、*Workspace.compute_targets()* は、*Workspace.compute_targets* に置き換えます。
  * *Run.get_submitted_run* が廃止され、*Run.get_context* に置き換えられました。 前者のメソッドは、今後のリリースで削除されます。
  * *PipelineData* クラスは、データストア オブジェクトを datastore_name ではなく、パラメーターとして受け付けるようになりました。 同様に、*パイプライン*は default_datastore_name ではなく default_datastore を受け入れます。

+ **新機能**
  * Azure Machine Learning Pipelines の[サンプル ノートブック](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines)で、MPI の手順が使われるようになりました。
  * Jupyter ノートブック用の RunDetails ウィジェットが更新され、パイプラインの視覚化が表示されるようになりました。

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Prep SDK v0.4.0

+ **新機能**
  * データ プロファイルにタイプ カウントが追加されました
  * 値のカウントとヒストグラムが使用可能になりました
  * データ プロファイル内のパーセンタイルが追加されました
  * 集計で中央値が使用可能になりました
  * Python 3.7 が新たにサポートされました
  * データストアを含んだデータ フローを DataPrep パッケージに保存すると、データ ストア情報が DataPrep パッケージの一部としてを永続化されます
  * データ ストアへの書き込みが新たにサポートされました

+ **バグ修正**
  * 64 ビット符号なし整数のオーバーフローが Linux で正しく処理されるようになりました
  * smart_read でのプレーン テキスト ファイルの不適切なテキスト ラベルが修正されました
  * 文字列の列タイプがメトリック ビューで表示されるようになりました
  * タイプ カウントが修正され、各種ではなく、1 つの FieldType にマップされた ValueKinds が表示されるようになりました
  * Write_to_csv で、パスが文字列として指定されている場合に処理が失敗しないようになりました
  * Replace を使用する際に、"find" を空白のままにしても処理が失敗しないようになりました

## <a name="2018-10-12"></a>2018 年 10 月 12 日

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning SDK for Python v0.1.68

+ **新機能**
  * 新しいワークスペースの作成時、マルチ テナントがサポートされます。

+ **修正済みのバグ**
  * Web サービスをデプロイするときに、pynacl ライブラリのバージョンをピン留めする必要がなくなりました。

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning Data Prep SDK v0.3.0

+ **新機能**
  * メソッド transform_partition_with_file(script_path) が追加されました。このメソッドにより、実行する Python ファイルのパスを渡すことができます。

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning SDK for Python v0.1.65
[バージョン 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) には、いくつかの新機能、追加のドキュメント、バグ修正、および追加の[サンプル ノートブック](https://aka.ms/aml-notebooks)が同梱されています。

バグおよび対処法については、[既知の問題のリスト](resource-known-issues.md)を参照してください。

+ **重大な変更**
  * Workspace.experiments、Workspace.models、Workspace.compute_targets、Workspace.images、Workspace.web_services 返却辞書、以前返却されたリスト。 [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API ドキュメントを参照してください。

  * 自動化された機械学習で、正規化平均二乗誤差がプライマリ メトリックから削除されました。

+ **HyperDrive**
  * ベイジアンに関する HyperDrive のさまざまなバグが修正されました。また、メトリックを取得する呼び出しのパフォーマンスが改善されました。
  * Tensorflow が 1.9 から 1.10 にアップグレードされました。
  * Docker イメージがコールド スタート用に最適化されました。
  * 終了時に 0 以外のエラー コードが発生した場合でも、ジョブの状態が正確に報告されます。
  * SDK で RunConfig 属性が検証されます。
  * HyperDrive 実行オブジェクトで、通常の実行と同様のキャンセルがサポートされます。パラメーターの受け渡しは不要です。
  * 分散実行および HyperDrive 実行でドロップダウンの値の状態が維持されるようにウィジェットが改善されました。
  * TensorBoard およびその他のログ ファイルのサポートが、パラメーター サーバー用に修正されました。
  * サービス側で Intel(R) MPI がサポートされます。
  * BatchAI での検証時における、分散実行修正用のパラメーター調整のバグが修正されました。
  * コンテキスト マネージャーがプライマリ インスタンスを識別するようになりました。

+ **Azure portal での操作**
  * [Run details]\(実行の詳細\) で log_table() と log_row() がサポートされます。
  * 1、2、または 3 個の数値列、および 1 個のカテゴリ列 (オプション) を含むテーブルおよび行のグラフが自動的に作成されます。

+ **自動化された機械学習**
  * エラー処理とドキュメントが改善されました。
  * 実行プロパティ取得のパフォーマンスの問題が修正されました。
  * 実行の継続に関する問題が修正されました。
  * 反復の:::no-loc text="ensembling":::アンサンブルに関する問題が修正されました。
  * システムが応答を停止する原因となっていた macOS でのトレーニングに関するバグを修正しました。
  * カスタム検証シナリオで、マクロ平均 PR/ROC 曲線がダウンサンプリングされます。
  * 余分なインデックス ロジックが削除されました。
  * get_output API からフィルターが削除されました。

+ **パイプライン**
  * パイプラインを直接バブリッシュするメソッド Pipeline.publish() が追加され、最初の実行が不要になりました。
  * パブリッシュ済みのパイプラインから生成されたパイプライン実行をフェッチするメソッド PipelineRun.get_pipeline_runs() が追加されました。

+ **Project Brainwave**
  * FPGA 上で使用可能な新しい AI モデルのサポートが更新されました。

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning Data Prep SDK v0.2.0
[バージョン 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) には、以下の機能とバグ修正が組み込まれています。

+ **新機能**
  * ワンホット エンコードがサポートされます。
  * 分位変換がサポートされます。

+ **バグ修正:**
  * どのバージョンの Tornado でも機能します。Tornado のバージョンをダウングレードする必要はありません。
  * 上位 3 つの値だけでなく、すべての値のカウントが評価されます。

## <a name="2018-09-public-preview-refresh"></a>2018 年 9 月 (パブリック プレビューを更新)

Azure Machine Learning が更新されてリリースされました。このリリースの詳細については、 https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/ を参照してください。


## <a name="next-steps"></a>次のステップ

[Azure Machine Learning](overview-what-is-azure-ml.md) の概要をご覧ください。

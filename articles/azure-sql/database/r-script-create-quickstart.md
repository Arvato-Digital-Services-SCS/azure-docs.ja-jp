---
title: 簡単な R スクリプトを作成して実行する
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL Database Machine Learning Services (プレビュー) で簡単な R スクリプトを実行します。
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 6645b50a6cd2d2145f9510ca2e2de0ee702fc3ad
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2020
ms.locfileid: "84053070"
---
# <a name="quickstart-create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>クイック スタート:Azure SQL Database Machine Learning Services (プレビュー) で簡単な R スクリプトを作成して実行する
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

このクイックスタートでは、Azure SQL Database の Machine Learning Services (と R) を使用して、一連の R スクリプトを作成して実行します。

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>前提条件

- アクティブなサブスクリプションが含まれる Azure アカウント。 [無料でアカウントを作成できます](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
- [サーバーレベルのファイアウォール規則](firewall-create-server-level-portal-quickstart.md)がある [Azure SQL Database 内のデータベース](single-database-create-quickstart.md)
- R が有効な [Machine Learning Services](machine-learning-services-overview.md)。
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

この例では、ストアド プロシージャ [sp_execute_external_script](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) を使用して、整形式の R スクリプトをラップします。

## <a name="run-a-simple-script"></a>単純なスクリプトを実行する

R スクリプトを実行するには、それをシステム ストアド プロシージャ [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) に引数として渡します。

以降の手順では、次のサンプル R スクリプトを SQL データベースで実行します。

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. **SQL Server Management Studio** を開き、SQL データベースに接続します。

   接続についてご不明な点がある場合は、「[クイック スタート: SQL Server Management Studio を使用して Azure SQL データベースに接続しクエリを実行する](connect-query-ssms.md)」をご覧ください。

1. R スクリプト全体を [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) ストアド プロシージャに渡します。

   このスクリプトは、`@script` 引数を通して渡されます。 `@script`引数内のすべては、有効な R コードである必要があります。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   なんらかのエラーが発生した場合は、お使いの SQL データベースで Machine Learning Services のパブリック プレビューと R が有効になっていない可能性があります。 前述の「[前提条件](#prerequisites)」を参照してください。

   > [!NOTE]
   > 管理者の場合は、外部コードを自動的に実行できます。 次のコマンドを使用して他のユーザーにアクセス許可を付与することができます。
   <br>**GRANT EXECUTE ANY EXTERNAL SCRIPT TO** *\<username\>*

2. 適切な結果が計算され、R の `print` 関数から **[メッセージ]** ウィンドウに結果が返されます。

   次のように表示されます。

    **結果**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Hello World スクリプトを実行する

一般的なスクリプトの例では、文字列 "Hello World" が出力されるだけです。 次のコマンドを実行します。

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

このストアド プロシージャへの入力は次のとおりです。

| | |
|-|-|
| @language | 呼び出す言語拡張機能 (この例では R) を定義します |
| @script | R ランタイムに渡されるコマンドを定義します この引数には R スクリプト全体を Unicode テキストとして含める必要があります。 **nvarchar** 型の変数にテキストを追加して、その変数を呼び出すこともできます |
| @input_data_1 | クエリによって返されるデータ。R ランタイムに渡され、そこからデータがデータ フレームとして返されます |
|結果セットを含む | 句では、返されるデータ テーブルのスキーマを定義し、列名として "Hello World" を追加し、データ型に **int** を追加します |

このコマンドは、次のテキストを出力します。

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>入力と出力を使用する

[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) が入力として既定で受け取るのは、単一のデータセットです。これは通常、有効な SQL クエリの形式で指定します。 次に、1 つの R データ フレームを出力として返します。

ここでは、[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) の既定の入力変数と出力変数を使用しましょう: **InputDataset** と **OutputDataset**。

1. テスト データの小さなテーブルを作成します。

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. テーブルのクエリを実行するには、`SELECT` ステートメントを使用します。
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **結果**

    ![RTestData テーブルの内容](./media/r-script-create-quickstart/select-rtestdata.png)

1. 次の R スクリプトを実行します。 `SELECT` ステートメントを使用してテーブルからデータを取得し、それを R ランタイムを介して渡し、データをデータ フレームとして返します。 `WITH RESULT SETS` 句は、列名として *NewColName* を追加して、SQL Database に対して返されるデータ テーブルのスキーマを定義します。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **結果**

    ![テーブルからデータを返す R スクリプトからの出力](./media/r-script-create-quickstart/r-output-rtestdata.png)

1. 次に、入力変数と出力変数の名前を変更します。 既定の入力変数名と出力変数名は **InputDataSet** と **OutputDataSet** で、このスクリプトによって名前が **SQL_in** および **SQL_out** に変更されます。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    R では大文字と小文字が区別されることに注意してください。 この R スクリプトで使用されている入力変数と出力変数 (**SQL_out**、**SQL_in**) は、大文字と小文字の区別も含め、`@input_data_1_name` と `@output_data_1_name` で定義された値と一致している必要があります。

   > [!TIP]
   > パラメーターとして渡すことができる入力データセットは 1 つだけです。また、返すことのできるデータセットも 1 つだけです。 ただし、R コード内から他のデータセットを呼び出し、そのデータセットに加えて、他の種類の出力を返すことができます。 任意のパラメーターに OUTPUT キーワードを追加することもでき、その場合は、パラメーターに結果が返されます。

1. 入力データを含まない R スクリプトを使用して値を生成することもできます (`@input_data_1` は空白に設定されます)。

   次のスクリプトは、 "hello" と "world" というテキストを出力します。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **結果**

    ![入力として @script を使用したクエリ結果](./media/r-script-create-quickstart/r-data-generated-output.png)

## <a name="check-r-version"></a>R バージョンの確認

お使いの SQL データベースにインストールされている R のバージョンを確認する場合は、次のスクリプトを実行します。

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

R `print` 関数は、**メッセージ** ウィンドウにバージョンを返します。 下の出力例では、この SQL Database には R バージョン 3.4.4 がインストールされていることがわかります。

**結果**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>R パッケージの一覧表示

Microsoft では、SQL データベースに Machine Learning Services と共にプレインストールされる R パッケージを多数提供しています。

バージョン、依存関係、ライセンス、ライブラリパスの情報など、インストールされている R パッケージの一覧を表示するには、次のスクリプトを実行します。

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

出力は R の `installed.packages()` からのものであり、結果セットとして返されます。

**結果**

![R のインストール済みパッケージ](./media/r-script-create-quickstart/r-installed-packages.png)

## <a name="next-steps"></a>次のステップ

R を使用して SQL Database に機械学習モデルを作成するには、次のクイック スタートに従ってください。

> [!div class="nextstepaction"]
> [R で Azure SQL Database Machine Learning Services (プレビュー) を使用して予測モデルを作成およびトレーニングする](r-train-score-model-create-quickstart.md)

Azure SQL Database Machine Learning Services と R (プレビュー) の詳細については、次の記事を参照してください。

- [Azure SQL Database Machine Learning Services と R (プレビュー)](machine-learning-services-overview.md)
- [Machine Learning Services (プレビュー) を使用して Azure SQL Database に高度な R 関数を記述する](machine-learning-services-functions.md)
- [Azure SQL Database Machine Learning Services (プレビュー) での R および SQL データの処理](machine-learning-services-data-issues.md)

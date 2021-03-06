---
title: 結構化流與 .NET 的 Apache Spark 教程
description: 在本教程中，您將瞭解如何將 .NET 用於 Apache Spark 進行 Spark 結構化流式處理。
author: mamccrea
ms.author: mamccrea
ms.date: 12/04/2019
ms.topic: tutorial
ms.openlocfilehash: 125ef834da8e42c99c8080a3d5414a7927ce7636
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "79186504"
---
# <a name="tutorial-structured-streaming-with-net-for-apache-spark"></a>教程：結構化流與 .NET 的 Apache Spark

本教程教您如何使用 .NET 為 Apache Spark 調用 Spark 結構化流。 Spark 結構化流是 Apache Spark 對處理即時資料流的支援。 流處理意味著在生成即時資料時對其進行分析。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
>
> * 為 Apache Spark 應用程式創建和運行 .NET
> * 使用 netcat 創建資料流程
> * 使用使用者定義的函數和 SparkSQL 分析流資料

## <a name="prerequisites"></a>必要條件

如果這是您的第一個 .NET 用於 Apache Spark 應用程式，請從[入門教程](get-started.md)開始，瞭解基礎知識。

## <a name="create-a-console-application"></a>建立主控台應用程式

1. 在命令提示符中，運行以下命令以創建新的主控台應用程式：

   ```dotnetcli
   dotnet new console -o mySparkStreamingApp
   cd mySparkStreamingApp
   ```

   該`dotnet`命令為您創建`new`類型`console`應用程式。 該`-o`參數創建一個名為*mySparkStreamingApp*的目錄，其中存儲你的應用，並將其填充到所需的檔中。 該`cd mySparkStreamingApp`命令將目錄更改為您剛剛創建的應用目錄。

1. 要在應用中使用的阿帕奇 Spark 的 .NET，請安裝 Microsoft.Spark 包。 在主控台中，運行以下命令：

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="establish-and-connect-to-a-data-stream"></a>建立並連接到資料流程

測試流處理的一種流行方法是通過**netcat。** netcat（也稱為*nc*）允許您從網路連接讀取和寫入網路連接。 通過終端視窗與網貓建立網路連接。

### <a name="create-a-data-stream-with-netcat"></a>使用網貓創建資料流程

1. [下載網貓](https://sourceforge.net/projects/nc110/files/)。 然後，從 zip 下載中提取檔，並將提取的目錄追加到"PATH"環境變數。

2. 要啟動新連接，請打開一個新主控台並運行以下命令，該命令連接到埠 9999 上的本地主機。

   在 Windows 上：

   ```console
   nc -vvv -l -p 9999
   ```

   在 Linux 上：

   ```console
   nc -lk 9999
   ```

   Spark 程式偵聽您在此命令提示符中鍵入的輸入。

### <a name="create-a-sparksession"></a>創建火花會話

1. 將以下附加`using`語句添加到*mySparkStreamingApp*中*Program.cs*檔的頂部 ：

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using Microsoft.Spark.Sql.Streaming;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. 將以下代碼添加到方法`Main`以創建新的`SparkSession`。 Spark 會話是使用資料集和資料幀 API 程式設計 Spark 的進入點。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("Streaming example with a UDF")
        .GetOrCreate();
   ```

   調用上面創建的*火花*物件允許您在整個程式中訪問 Spark 和 DataFrame 功能。

### <a name="connect-to-a-stream-with-readstream"></a>使用 ReadStream 連接到流（）

該方法`ReadStream()`返回 可用於`DataStreamReader`讀取流資料作為`DataFrame`的 。 包括主機和埠資訊，告訴 Spark 應用在何處預期其流資料。

```csharp
DataFrame lines = spark
    .ReadStream()
    .Format("socket")
    .Option("host", hostname)
    .Option("port", port)
    .Load();
```

## <a name="register-a-user-defined-function"></a>註冊使用者定義的函數

您可以在 Spark 應用程式中使用*使用者定義的函數*UdF 對資料執行計算和分析。

將以下代碼添加到方法`Main`以註冊稱為`udfArray`的 UDF。

```csharp
Func<Column, Column> udfArray =
    Udf<string, string[]>((str) => new string[] { str, $"{str} {str.Length}" });
```

此 UDF 處理它從 netcat 終端接收的每個字串以生成包含原始字串（包含在*str*中）的陣列，然後是與原始字串長度串聯的原始字串。

例如，在 netcat 終端中輸入*Hello 世界*會產生一個陣列，其中：

* 陣列\[0+ = 你好世界
* 陣列\[1] = 您世界 11

## <a name="use-sparksql"></a>使用 SparkSQL

使用 SparkSQL 對存儲在資料幀中的資料執行各種功能。 通常將 UDF 和 SparkSQL 組合在一起，將 UDF 應用於 DataFrame 的每一行。

```csharp
DataFrame arrayDF = lines.Select(Explode(udfArray(lines["value"])));
```

此程式碼片段將*udfArray*應用於 DataFrame 中的每個值，該值表示從 netcat 終端讀取的每個字串。 應用 SparkSQL<xref:Microsoft.Spark.Sql.Functions.Explode%2A>方法將陣列的每個條目放在自己的行中。 最後，使用<xref:Microsoft.Spark.Sql.DataFrame.Select%2A>將已生成的列放在新的 DataFrame*陣列DF*中。

## <a name="display-your-stream"></a>顯示流

用於<xref:Microsoft.Spark.Sql.DataFrame.WriteStream>建立輸出的特徵，例如將結果列印到主控台並僅顯示最新的輸出。

```csharp
StreamingQuery query = arrayDf
    .WriteStream()
    .Format("console")
    .Start();
```

## <a name="run-your-code"></a>運行代碼

Spark 中的結構化流通過一系列小**批量**處理資料。  運行程式時，建立 netcat 連接的命令提示允許您開始鍵入。 每次在命令提示符中鍵入資料後按 Enter 鍵時，Spark 都會將您的條目視為一個批次處理並運行 UDF。

### <a name="use-spark-submit-to-run-your-app"></a>使用火花提交運行應用

啟動新的 netcat 會話後，打開一個新終端並運行`spark-submit`命令，類似于以下命令：

```powershell
spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /path/to/microsoft-spark-<version>.jar Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkCharacterCount localhost 9999
```

> [!NOTE]
> 請務必使用 Microsoft Spark jar 檔的實際路徑更新上述命令。 上述命令還假定您的 netcat 伺服器正在本地主機埠 9999 上運行。

## <a name="get-the-code"></a>取得程式碼

本教程使用[StructuredNetworkCharacterCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkCharacterCount.cs)示例，但 GitHub 上還有三個完整的流處理示例：

* [StructuredNetworkWordCount.cs：](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)字計數從任何源資料流的資料
* [StructuredNetworkWordCountWindowed.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCountWindowed.cs)： 具有視窗邏輯的資料的字數
* [StructuredKafkaWordCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)： 字計數從卡夫卡流的資料

## <a name="next-steps"></a>後續步驟

進入下一篇文章，瞭解如何將用於 Apache Spark 的 .NET 應用程式部署到 Databricks。
> [!div class="nextstepaction"]
> [教程：將用於 Apache Spark 應用程式的 .NET 部署到資料磚塊](databricks-deployment.md)

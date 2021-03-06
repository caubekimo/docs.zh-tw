---
title: dotnet tool uninstall 命令
description: dotnet 工具卸載命令從電腦卸載指定的 .NET Core 工具。
ms.date: 02/14/2020
ms.openlocfilehash: 0416f91019a49e17f1be14a1d928ad1fafaa736c
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463317"
---
# <a name="dotnet-tool-uninstall"></a>dotnet tool uninstall

**本文適用於:✔️** .NET 核心 2.1 SDK 和更高版本

## <a name="name"></a>名稱

`dotnet tool uninstall`- 從電腦卸載指定的[.NET 核心工具](global-tools.md)。

## <a name="synopsis"></a>概要

```dotnetcli
dotnet tool uninstall <PACKAGE_NAME> -g|--global

dotnet tool uninstall <PACKAGE_NAME> --tool-path <PATH>

dotnet tool uninstall <PACKAGE_NAME>

dotnet tool uninstall -h|--help
```

## <a name="description"></a>描述

該`dotnet tool uninstall`命令提供了一種從電腦卸載 .NET Core 工具的方法。 要使用 此指令,請指定以下選項之一:

* 要卸載安裝在預設位置的全域工具,請使用 選項`--global`。
* 要卸載安裝在自訂位置的全域工具,請使用 選項`--tool-path`。
* 要卸載本地工具,請省略和`--global``--tool-path`選項。

**本地工具可從 .NET 核心 SDK 3.0 開始。**

## <a name="arguments"></a>引數

- **`PACKAGE_NAME`**

  包含要卸載的 .NET Core 工具的 NuGet 套件的名稱/ID。 您可以使用 [dotnet tool list](dotnet-tool-list.md) 命令來找到此套件名稱。

## <a name="options"></a>選項。

- **`-g|--global`**

  指定要從使用者範圍安裝中移除此工具。 無法與 `--tool-path` 選項合併使用。 省略兩者`--global``--tool-path`並 指定要刪除的工具是本地工具。

- **`-h|--help`**

  印出命令的簡短說明。

- **`--tool-path <PATH>`**

  指定卸載工具的位置。 PATH 可為絕對路徑或相對路徑。 無法與 `--global` 選項合併使用。 省略兩者`--global``--tool-path`並 指定要刪除的工具是本地工具。

## <a name="examples"></a>範例

- **`dotnet tool uninstall -g dotnetsay`**

  卸載[dotnetsay](https://www.nuget.org/packages/dotnetsay/)全域工具。

- **`dotnet tool uninstall dotnetsay --tool-path c:\global-tools`**

  從特定的 Windows 目錄卸載[dotnetsay](https://www.nuget.org/packages/dotnetsay/)全域工具。

- **`dotnet tool uninstall dotnetsay --tool-path ~/bin`**

  從特定的 Linux/macOS 目錄中卸載[dotnetsay](https://www.nuget.org/packages/dotnetsay/)全域工具。

- **`dotnet tool uninstall dotnetsay`**

  從目前目錄卸載[dotnetsay](https://www.nuget.org/packages/dotnetsay/)本地工具。

## <a name="see-also"></a>另請參閱

- [.NET 核心工具](global-tools.md)
- [教學:使用 .NET 核心 CLI 安裝與使用 .NET 核心全域工具](global-tools-how-to-use.md)
- [教學:使用 .NET 核心 CLI 安裝與使用 .NET 核心本地工具](local-tools-how-to-use.md)

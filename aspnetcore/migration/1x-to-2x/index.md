---
title: "ASP.NET Core 1.x から 2.0 への移行"
author: scottaddie
description: "この記事では、ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に移行する前提条件と最も一般的な手順について説明します。"
keywords: "ASP.NET Core,移行"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: 7a845cec23f662dd6fe48044b819099f2c20ecb3
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>ASP.NET Core 1.x から ASP.NET Core 2.0 への移行

作成者: [Scott Addie](https://github.com/scottaddie)

この記事では、既存の ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に更新する手順を説明します。 アプリケーションを ASP.NET Core 2.0 に移行すると、[多数の新機能を利用したり、パフォーマンスを向上させたりすることができます](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0)。 

既存の ASP.NET Core 1.x アプリケーションは、バージョン固有のプロジェクト テンプレートには基づいていません。 ASP.NET Core のフレームワークの進化に伴い、プロジェクト テンプレートと、それに含まれるスタート コードも進化しています。 ASP.NET Core のフレームワークを更新するだけでなく、アプリケーションのコードも更新する必要があります。

<a name="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント
「[ASP.NET Core の概要](xref:getting-started)」を参照してください。

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>ターゲット フレームワーク モニカー (TFM) の更新
.NET Core をターゲットとするプロジェクトでは、.NET Core 2.0 以上のバージョンの [TFM](/dotnet/standard/frameworks#referring-to-frameworks) を使用する必要があります。 *.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `netcoreapp2.0` に置き換えます。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

.NET Framework をターゲットとするプロジェクトでは、.NET Framework 4.6.1 以上の TFM バージョンを使用する必要があります。 *.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `net461` に置き換えます。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET Core 1.x よりセキュリティ上外部からアクセスできるところの多い .NET core 2.0 NET Core 1.x に API がないためだけに .NET Framework をターゲットにする場合、.NET Core 2.0 をターゲットにすると機能します。

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>global.json での .NET Core SDK バージョンの更新
特定の .NET Core SDK バージョンをターゲットとするよう、ソリューションが [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) ファイルに依存する場合、コンピューターにインストールされている 2.0 バージョンを使用するよう、その `version` プロパティを更新します。

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>パッケージ参照の更新
1.x プロジェクト内の *.csproj* ファイルには、プロジェクトによって使用される各 NuGet パッケージの一覧があります。

.NET Core 2.0 をターゲットとする ASP.NET Core 2.0 プロジェクトでは、*.csproj* ファイル内の 1 つの[メタパッケージ](xref:fundamentals/metapackage)への参照によってパッケージのコレクションが置き換えられます。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

メタパッケージには、ASP.NET Core 2.0 および Entity Framework Core 2.0 のすべての機能が含まれます。

.NET Framework をターゲットとする ASP.NET Core 2.0 プロジェクトは、個々の NuGet パッケージを参照し続ける必要があります。 各 `<PackageReference />` ノードの `Version` 属性を 2.0.0 に更新します。

たとえば、.NET Framework をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される `<PackageReference />` ノードの一覧を次に示します。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>.NET Core CLI ツールの更新
*.csproj* ファイルで、`<DotNetCliToolReference />` ノードのそれぞれの `Version` 属性を 2.0.0 に更新します。

たとえば、.NET Core 2.0 をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される CLI ツールの一覧を次に示します。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Package Target Fallback プロパティの名前変更
1.x プロジェクトの *.csproj* ファイルでは、`PackageTargetFallback` ノードと変数を使用していました。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

ノードと変数の両方を `AssetTargetFallback` に名前変更します。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Program.cs の Main メソッドの更新
1.x プロジェクトでは、*Program.cs* の `Main` メソッドは次のようでした。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

2.0 プロジェクトでは、*Program.cs* の `Main` メソッドは次のように簡素化されました。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

この新しい 2.0 のパターンの導入は強く推奨され、これらの動作には、[Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) のような製品機能が必要です。 たとえば、パッケージ マネージャーのコンソール ウィンドウから `Update-Database` を実行したり、(ASP.NET Core 2.0 に変換されたプロジェクトで) コマンドラインから `dotnet ef database update` を実行したりすると、次のエラーが発生します。

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>データベース初期化コードの移動
EF Core 1.x を利用する 1.x プロジェクトで、`dotnet ef migrations add` のようなコマンドは以下を行います。
1. `Startup` インスタンスをインスタンス化する
2. `ConfigureServices` メソッドを呼び出し、依存性の注入にすべてのサービスを登録する (`DbContext` タイプを含める)
3. その必要なタスクを実行する

EF Core 2.0 を使用する 2.0 プロジェクトでは、`Program.BuildWebHost` が呼び出され、アプリケーション サービスを取得します。 1.x とは異なり、`Startup.Configure` を呼び出すという副作用が加わります。 1.x アプリがその `Configure` メソッドでデータベース初期化コードを呼び出すと、予想外の問題が発生することがあります。 たとえば、データベースがまだ存在しない場合、EF Core 移行コマンド実行前にシード処理コードが実行されます。 この問題が原因で、データベースがまだ存在しない場合に `dotnet ef migrations list` コマンドが失敗します。

*Startup.cs* の `Configure` メソッドで次の 1.x シード処理初期化コードを検討してください。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

2.0 プロジェクトでは、*Program.cs* の `Main` メソッドに `SeedData.Initialize` 呼び出しを移動します。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

2.0 以降、`BuildWebHost` で Web ホストのビルドと構成以外を行うことは正しくない使用例となります。 アプリケーションの実行に関するあらゆることは `BuildWebHost` の外側で、一般的には *Program.cs* の `Main` メソッドで処理する必要があります。

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>Razor ビュー コンパイル設定の確認
ユーザーに最も重要なことは、アプリケーションを高速に起動することとパブリッシュされたバンドル数を少なくすることです。 これらの理由から、ASP.NET Core 2.0 では [Razor ビュー コンパイル](xref:mvc/views/view-compilation)が既定で有効になっています。

`MvcRazorCompileOnPublish` プロパティを true に設定する必要がなくなりました。 ビューのコンパイルを無効にしている場合を除き、*.csproj* ファイルからこのプロパティが削除されている場合があります。

.NET Framework をターゲットとする場合、継続して *.csproj* ファイルの [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet パッケージを明示的に参照する必要があります。

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Application Insights の "Light-Up" 機能の利用
アプリケーション パフォーマンスのインストルメンテーションは、楽に設定できることが重要です。 Visual Studio 2017 のツールの新しい [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" 機能が利用できるようになりました。

Visual Studio 2017 で作成された ASP.NET Core 1.1 プロジェクトには、既定で Application Insights が追加されています。 *Program.cs* と *Startup.cs* 外で、Application Insights SDK を直接使用していない場合、次の手順に従います。

1. *.csproj* ファイルから次の `<PackageReference />` ノードを削除します。
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. *Program.cs* から `UseApplicationInsights` 拡張メソッド呼び出しを削除します。

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. *_Layout.cshtml* から Application Insights のクライアント側 API 呼び出しを削除します。 これによって、次の 2 つのコード行が作成されます。

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19)]

Application Insights SDK を直接使用している場合は、それを継続してください。 2.0 の[メタパッケージ](xref:fundamentals/metapackage)には、Application Insights の最新バージョンが含まれているので、以前のバージョンを参照している場合、パッケージのダウングレード エラーが表示されます。

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>認証/ID の機能強化の採用
ASP.NET Core 2.0 には、新しい認証モデルと ASP.NET Core ID への大幅な変更があります。 個々のユーザー アカウントを有効にしてプロジェクトを作成した場合や認証または ID を手動で追加した場合、「[ASP.NET Core 2.0 への認証と ID の移行](xref:migration/1x-to-2x/identity-2x)」を参照してください。

## <a name="additional-resources"></a>その他のリソース
- [ASP.NET Core 2.0 での互換性に影響する変更点](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
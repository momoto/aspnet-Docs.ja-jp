---
title: "ASP.NET Core の基礎"
author: rick-anderson
description: "ASP.NET Core アプリケーションの構築に関する基本概念について説明します。"
keywords: "ASP.NET Core,基礎,概要"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="792dc-104">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="792dc-104">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="792dc-105">ASP.NET Core アプリケーションは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="792dc-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="792dc-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="792dc-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="792dc-107">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHost.CreateDefaultBuilder` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="792dc-107">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="792dc-108">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="792dc-108">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="792dc-109">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="792dc-109">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="792dc-110">ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。</span><span class="sxs-lookup"><span data-stu-id="792dc-110">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="792dc-111">[HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-111">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="792dc-112">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="792dc-112">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="792dc-113">`IWebHostBuilder` (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-113">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="792dc-114">これらのメソッドの一部には、HTTP.sys でアプリをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="792dc-114">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="792dc-115">`Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="792dc-115">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="792dc-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="792dc-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="792dc-117">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHostBuilder` を使用します。</span><span class="sxs-lookup"><span data-stu-id="792dc-117">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="792dc-118">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="792dc-118">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="792dc-119">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="792dc-119">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="792dc-120">[WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-120">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="792dc-121">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="792dc-121">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="792dc-122">`WebHostBuilder` では、IIS および IIS Express でホストするための `UseIISIntegration` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` を含む、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-122">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="792dc-123">`Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="792dc-123">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="792dc-124">スタートアップ</span><span class="sxs-lookup"><span data-stu-id="792dc-124">Startup</span></span>

<span data-ttu-id="792dc-125">`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="792dc-125">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="792dc-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="792dc-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="792dc-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="792dc-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="792dc-128">`Startup` クラスでは、要求処理パイプラインを定義し、アプリで必要なサービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-128">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="792dc-129">`Startup` クラスはパブリックであり、次のメソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="792dc-129">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="792dc-130">`ConfigureServices` は、アプリで使用される[サービス](#dependency-injection-services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。</span><span class="sxs-lookup"><span data-stu-id="792dc-130">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="792dc-131">`Configure` は、要求パイプラインの[ミドルウェア](xref:fundamentals/middleware)を定義します。</span><span class="sxs-lookup"><span data-stu-id="792dc-131">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="792dc-132">詳細については、[アプリケーションの起動](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-132">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="792dc-133">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="792dc-133">Content root</span></span>

<span data-ttu-id="792dc-134">コンテンツ ルートは、ビュー、[Razor ページ](xref:mvc/razor-pages/index)、および静的資産など、アプリで使用されるコンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="792dc-134">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="792dc-135">既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルのアプリケーション基本パスと同じです。</span><span class="sxs-lookup"><span data-stu-id="792dc-135">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="792dc-136">Web ルート</span><span class="sxs-lookup"><span data-stu-id="792dc-136">Web root</span></span>

<span data-ttu-id="792dc-137">アプリの Web ルートは、CSS、JavaScript、イメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="792dc-137">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="792dc-138">依存性の注入 (サービス)</span><span class="sxs-lookup"><span data-stu-id="792dc-138">Dependency Injection (Services)</span></span>

<span data-ttu-id="792dc-139">サービスは、アプリでの一般的な使用を想定されているコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="792dc-139">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="792dc-140">サービスは[依存性の注入 (DI)](xref:fundamentals/dependency-injection) を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="792dc-140">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="792dc-141">ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (**I**nversion **o**f **C**ontrol、IoC) コンテナーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="792dc-141">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="792dc-142">必要に応じて、既定のネイティブ コンテナーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-142">You can replace the default native container if you wish.</span></span> <span data-ttu-id="792dc-143">その疎結合の利点に加え、DI はアプリ全体でサービス ([ログ](xref:fundamentals/logging/index)など) を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="792dc-143">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="792dc-144">詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="792dc-145">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="792dc-145">Middleware</span></span>

<span data-ttu-id="792dc-146">ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware)を使用して要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="792dc-146">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="792dc-147">ASP.NET Core ミドルウェアは `HttpContext` で非同期ロジックを実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="792dc-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="792dc-148">"XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出して追加されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-148">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="792dc-149">ASP.NET Core には、豊富な組み込みミドルウェアのセットが付属しています。</span><span class="sxs-lookup"><span data-stu-id="792dc-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="792dc-150">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="792dc-150">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="792dc-151">ルーティング</span><span class="sxs-lookup"><span data-stu-id="792dc-151">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="792dc-152">認証</span><span class="sxs-lookup"><span data-stu-id="792dc-152">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="792dc-153">応答圧縮ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="792dc-153">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="792dc-154">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="792dc-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="792dc-155">ASP.NET Core アプリで [OWIN](http://owin.org) ベースのミドルウェアを使用することができます。また、独自のカスタム ミドルウェアを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="792dc-155">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="792dc-156">詳細については、[ミドルウェア](xref:fundamentals/middleware)と [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-156">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="792dc-157">環境</span><span class="sxs-lookup"><span data-stu-id="792dc-157">Environments</span></span>

<span data-ttu-id="792dc-158">"開発" や "実稼働" などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数を使用して設定できます。</span><span class="sxs-lookup"><span data-stu-id="792dc-158">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="792dc-159">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-159">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="792dc-160">構成</span><span class="sxs-lookup"><span data-stu-id="792dc-160">Configuration</span></span>

<span data-ttu-id="792dc-161">ASP.NET Core は、名前と値のペアに基づく構成モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="792dc-161">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="792dc-162">この構成モデルは `System.Configuration` または *web.config* には基づきません。構成は、構成プロバイダーの順序付けされたセットから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="792dc-162">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="792dc-163">組み込みの構成プロバイダーではさまざまなファイル形式 (XML、JSON、INI) と環境変数がサポートされ、これにより環境ベースの構成が可能になります。</span><span class="sxs-lookup"><span data-stu-id="792dc-163">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="792dc-164">独自のカスタム構成プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="792dc-164">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="792dc-165">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-165">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="792dc-166">ログの記録</span><span class="sxs-lookup"><span data-stu-id="792dc-166">Logging</span></span>

<span data-ttu-id="792dc-167">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="792dc-167">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="792dc-168">組み込みプロバイダーは、1 つまたは複数の送信先へのログの送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="792dc-168">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="792dc-169">サード パーティ製のログ記録フレームワークを使用できます。</span><span class="sxs-lookup"><span data-stu-id="792dc-169">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="792dc-170">ログ</span><span class="sxs-lookup"><span data-stu-id="792dc-170">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="792dc-171">エラー処理</span><span class="sxs-lookup"><span data-stu-id="792dc-171">Error handling</span></span>

<span data-ttu-id="792dc-172">ASP.NET Core には、開発者の例外ページ、カスタム エラー ページ、静的ステータス コード ページ、起動時の例外処理を含む、アプリのエラー処理の機能が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="792dc-172">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="792dc-173">詳細については、[エラー処理](xref:fundamentals/error-handling)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-173">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="792dc-174">ルーティング</span><span class="sxs-lookup"><span data-stu-id="792dc-174">Routing</span></span>

<span data-ttu-id="792dc-175">ASP.NET Core は、ルート ハンドラーにアプリ要求をルーティングする機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="792dc-175">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="792dc-176">詳細については、[ルーティング](xref:fundamentals/routing)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-176">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="792dc-177">ファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="792dc-177">File providers</span></span>

<span data-ttu-id="792dc-178">ASP.NET Core は、プラットフォーム間でファイルを操作するための共通のインターフェイスを提供するファイル プロバイダーの使用により、ファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="792dc-178">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="792dc-179">詳細については、[ファイル プロバイダー](xref:fundamentals/file-providers)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-179">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="792dc-180">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="792dc-180">Static files</span></span>

<span data-ttu-id="792dc-181">静的ファイルのミドルウェアは、HTML、CSS、画像、JavaScript などの静的ファイルを処理します。</span><span class="sxs-lookup"><span data-stu-id="792dc-181">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="792dc-182">詳細については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-182">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="792dc-183">ホスト</span><span class="sxs-lookup"><span data-stu-id="792dc-183">Hosting</span></span>

<span data-ttu-id="792dc-184">ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="792dc-184">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="792dc-185">詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-185">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="792dc-186">セッションとアプリケーションの状態</span><span class="sxs-lookup"><span data-stu-id="792dc-186">Session and application state</span></span>

<span data-ttu-id="792dc-187">セッション状態は ASP.NET Core の機能で、これを使用することでユーザーが Web アプリを参照中にユーザー データを保存して格納することができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-187">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="792dc-188">詳細については、[セッションとアプリケーション状態](xref:fundamentals/app-state)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-188">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="792dc-189">サーバー</span><span class="sxs-lookup"><span data-stu-id="792dc-189">Servers</span></span>

<span data-ttu-id="792dc-190">ASP.NET Core のホスティング モデルは、直接要求をリッスンしません。</span><span class="sxs-lookup"><span data-stu-id="792dc-190">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="792dc-191">ホスティング モデルは HTTP サーバー実装に依存して要求をアプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="792dc-191">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="792dc-192">転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。</span><span class="sxs-lookup"><span data-stu-id="792dc-192">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="792dc-193">ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="792dc-193">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="792dc-194">Kestrel は多くの場合、[IIS](https://www.iis.net/) や [nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-194">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span> <span data-ttu-id="792dc-195">Kestrel は、エッジ サーバーとして実行することができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-195">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="792dc-196">詳細については、[サーバー](xref:fundamentals/servers/index)に関するページと、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-196">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="792dc-197">Kestrel</span><span class="sxs-lookup"><span data-stu-id="792dc-197">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="792dc-198">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="792dc-198">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="792dc-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="792dc-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="792dc-200">グローバリゼーションとローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="792dc-200">Globalization and localization</span></span>

<span data-ttu-id="792dc-201">ASP.NET Core で多言語の Web サイトを作成すると、より幅広い対象者がサイトにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="792dc-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="792dc-202">ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="792dc-202">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="792dc-203">詳細については、[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-203">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="792dc-204">要求機能</span><span class="sxs-lookup"><span data-stu-id="792dc-204">Request features</span></span>

<span data-ttu-id="792dc-205">HTTP の要求と応答に関連する Web サーバーの実装の詳細が、インターフェイスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="792dc-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="792dc-206">これらのインターフェイスは、サーバーの実装とミドルウェアがアプリのホスティングのパイプラインを作成および変更するために使用します。</span><span class="sxs-lookup"><span data-stu-id="792dc-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="792dc-207">詳細については、 [要求機能](xref:fundamentals/request-features)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-207">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="792dc-208">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="792dc-208">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="792dc-209">ASP.NET Core は、Open Web Interface for .NET (OWIN) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="792dc-209">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="792dc-210">OWIN により、Web アプリを Web サーバーから切り離すことが可能になります。</span><span class="sxs-lookup"><span data-stu-id="792dc-210">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="792dc-211">詳細については、[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-211">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="792dc-212">WebSocket</span><span class="sxs-lookup"><span data-stu-id="792dc-212">WebSockets</span></span>

<span data-ttu-id="792dc-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="792dc-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="792dc-214">チャット、株価情報、ゲームなどのアプリ、さらに Web アプリでのリアルタイムな機能が必要とされるあらゆる場所で使用されます。</span><span class="sxs-lookup"><span data-stu-id="792dc-214">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="792dc-215">ASP.NET Core は、Web ソケットの機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="792dc-215">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="792dc-216">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-216">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="792dc-217">Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="792dc-217">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="792dc-218">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="792dc-218">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="792dc-219">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="792dc-219">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="792dc-220">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="792dc-220">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="792dc-221">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="792dc-221">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="792dc-222">詳細については、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-222">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="792dc-223">.NET Core と .NET Framework ランタイム</span><span class="sxs-lookup"><span data-stu-id="792dc-223">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="792dc-224">ASP.NET Core アプリは、.NET Core または .NET Framework ランタイムを対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="792dc-224">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="792dc-225">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-225">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="792dc-226">ASP.NET Core と ASP.NET の選択</span><span class="sxs-lookup"><span data-stu-id="792dc-226">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="792dc-227">ASP.NET Core と ASP.NET の選択の詳細については、「[ ASP.NET と ASP.NET Core の選択](xref:fundamentals/choose-between-aspnet-and-aspnetcore)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="792dc-227">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
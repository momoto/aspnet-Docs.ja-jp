---
title: "ASP.NET MVC を持つコアを EF Core - 詳細 - 10 10"
author: tdykstra
description: "このチュートリアルでは、Entity Framework のコアを使用する ASP.NET web アプリケーションの開発、基本機能を越えて移動するときに認識する便利ないくつかのトピックを紹介します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: ea83e5b17df80e5615dda49335247340d1cfb016
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="a3557-103">高度なトピックの EF コア ASP.NET Core MVC のチュートリアル (10 10 の)</span><span class="sxs-lookup"><span data-stu-id="a3557-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="a3557-104">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3557-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a3557-105">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a3557-106">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a3557-107">前のチュートリアルでは、テーブルの階層あたりの継承を実装します。</span><span class="sxs-lookup"><span data-stu-id="a3557-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="a3557-108">このチュートリアルでは、Entity Framework のコアを使用する ASP.NET Core web アプリケーションの開発、基本機能を越えて移動するときに認識する便利ないくつかのトピックを紹介します。</span><span class="sxs-lookup"><span data-stu-id="a3557-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="a3557-109">生の SQL クエリ</span><span class="sxs-lookup"><span data-stu-id="a3557-109">Raw SQL Queries</span></span>

<span data-ttu-id="a3557-110">Entity Framework を使用する利点の 1 つは、コードのデータを格納する特定のメソッドを過度に結び付ける済む点です。</span><span class="sxs-lookup"><span data-stu-id="a3557-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="a3557-111">これは、SQL クエリとコマンドの生成も自分で記述することから解放することで実行します。</span><span class="sxs-lookup"><span data-stu-id="a3557-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="a3557-112">手動で作成した特定の SQL クエリを実行する必要がある場合、例外的なシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="a3557-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="a3557-113">このようなシナリオは、Entity Framework コードの最初の API には、使用すると、データベースに直接 SQL コマンドを渡すメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3557-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="a3557-114">EF Core 1.0 では、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="a3557-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="a3557-115">使用して、`DbSet.FromSql`をエンティティ型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="a3557-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="a3557-116">返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによってしない限りする[追跡をオフにする](crud.md#no-tracking-queries)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-116">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="a3557-117">使用して、`Database.ExecuteSqlCommand`非クエリ コマンド。</span><span class="sxs-lookup"><span data-stu-id="a3557-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="a3557-118">エンティティがない型を返すクエリを実行する必要がある場合は、EF によって提供されるデータベース接続で ADO.NET を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3557-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="a3557-119">返されるデータは、エンティティの種類を取得するこのメソッドを使用する場合でもデータベース コンテキストによって追跡されていません。</span><span class="sxs-lookup"><span data-stu-id="a3557-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="a3557-120">常に true web アプリケーションで SQL コマンドを実行するときに、としては、SQL インジェクション攻撃からサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3557-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="a3557-121">行うには 1 つの方法では、パラメーター化クエリを使用して、SQL コマンドとして web ページによって送信される文字列を解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a3557-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="a3557-122">このチュートリアルでは、ユーザー入力をクエリに統合するとき、パラメーター化クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="a3557-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="a3557-123">エンティティを返すクエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a3557-123">Call a query that returns entities</span></span>

<span data-ttu-id="a3557-124">`DbSet<TEntity>`クラス型のエンティティを返すクエリの実行に使用できるメソッドを提供`TEntity`です。</span><span class="sxs-lookup"><span data-stu-id="a3557-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="a3557-125">内のコードを変更するしくみを表示する、`Details`部門コント ローラーのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="a3557-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="a3557-126">*DepartmentsController.cs*で、`Details`メソッド、部門を取得するコードを置き換える、`FromSql`メソッドの呼び出し、次の強調表示されたコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="a3557-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="a3557-127">新しいコードが正しく動作することを確認するには、次のように選択します。、**部門**タブし、**詳細**部門のいずれか。</span><span class="sxs-lookup"><span data-stu-id="a3557-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門の詳細](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="a3557-129">その他の型を返すクエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a3557-129">Call a query that returns other types</span></span>

<span data-ttu-id="a3557-130">以前登録日付ごとの生徒の数が映っているバージョン情報 ページの受講者統計グリッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3557-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="a3557-131">学生のエンティティ セットからデータを取得した (`_context.Students`) の一覧に結果を射影する LINQ を使用して`EnrollmentDateGroup`モデル オブジェクトを表示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="a3557-132">自体ではなく LINQ を使用して、SQL を記述するとします。</span><span class="sxs-lookup"><span data-stu-id="a3557-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="a3557-133">SQL クエリを実行する必要があることを行うを返すエンティティ オブジェクト以外のものです。</span><span class="sxs-lookup"><span data-stu-id="a3557-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="a3557-134">EF Core 1.0 では、これを行うには ADO.NET コードを記述し、EF からデータベース接続を取得できます。</span><span class="sxs-lookup"><span data-stu-id="a3557-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="a3557-135">*HomeController.cs*、置換、`About`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="a3557-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="a3557-136">使用して、追加のステートメント。</span><span class="sxs-lookup"><span data-stu-id="a3557-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="a3557-137">アプリを実行してバージョン情報 ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a3557-137">Run the app and go to the About page.</span></span> <span data-ttu-id="a3557-138">以前と同じ、同じデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-138">It displays the same data it did before.</span></span>

![ページについて](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="a3557-140">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a3557-140">Call an update query</span></span>

<span data-ttu-id="a3557-141">Contoso 大学管理者がすべてのコースのクレジットの数の変更などのデータベースでグローバルに変更を実行するとします。</span><span class="sxs-lookup"><span data-stu-id="a3557-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="a3557-142">かかる大学のコースの数が多い場合は、できなくなるそれらすべてのエンティティとして取得し、それらを個別に変更が効率的です。</span><span class="sxs-lookup"><span data-stu-id="a3557-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="a3557-143">ユーザーが、すべてのコースのクレジットの数を変更する係数を指定できるようにする web ページを実装するこのセクションの内容と SQL UPDATE ステートメントを実行することによって変更されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="a3557-144">Web ページは、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3557-144">The web page will look like the following illustration:</span></span>

![コースのクレジットのページを更新します。](advanced/_static/update-credits.png)

<span data-ttu-id="a3557-146">*CoursesContoller.cs*、HttpGet および HttpPost UpdateCourseCredits メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3557-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="a3557-147">何も返されない、コント ローラーは、HttpGet 要求を処理するときに`ViewData["RowsAffected"]`、前の図に示すように空のテキスト ボックスと、[送信] ボタンをビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="a3557-148">ときに、**更新**ボタンがクリックされた、HttpPost メソッドを呼び出して、乗数値を保持して、テキスト ボックスに入力します。</span><span class="sxs-lookup"><span data-stu-id="a3557-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="a3557-149">コードの更新を記録するコースのビューに影響を受けた行の数を返します SQL を実行し、`ViewData`です。</span><span class="sxs-lookup"><span data-stu-id="a3557-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="a3557-150">ビューを取得すると、`RowsAffected`値、更新された行の数を表示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="a3557-151">**ソリューション エクスプ ローラー**を右クリックし、*ビュー/コース*フォルダー、およびクリック**追加 > 新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a3557-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="a3557-152">**新しい項目の追加**ダイアログ ボックスで、をクリックして**ASP.NET** **インストール**左側のウィンドウでをクリックして**MVC ビュー ページ**、ビューの名前と*UpdateCourseCredits.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="a3557-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="a3557-153">*Views/Courses/UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a3557-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="a3557-154">実行、`UpdateCourseCredits`メソッドを選択して、**コース** タブで、追加し、"/UpdateCourseCredits"ブラウザーのアドレス バーの URL の末尾に (たとえば: `http://localhost:5813/Courses/UpdateCourseCredits`)。</span><span class="sxs-lookup"><span data-stu-id="a3557-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="a3557-155">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="a3557-155">Enter a number in the text box:</span></span>

![コースのクレジットのページを更新します。](advanced/_static/update-credits.png)

<span data-ttu-id="a3557-157">**[更新]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a3557-157">Click **Update**.</span></span> <span data-ttu-id="a3557-158">影響を受ける行の数が参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3557-158">You see the number of rows affected:</span></span>

![更新コースのクレジットが影響を受ける行をページします。](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="a3557-160">をクリックして**一覧に戻る**コースのクレジットの改訂された数との一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="a3557-161">常に有効なデータの結果を更新すると実稼働コードで確実に注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3557-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="a3557-162">ここで示す簡略化されたコードでは、5 より大きい数値で発生するのに十分なクレジットの数を乗算可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3557-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="a3557-163">(、`Credits`プロパティには、`[Range(0, 5)]`属性です)。更新クエリが機能しますが、無効なデータのクレジットの数は 5 個以下であると、システムの他の部分で予期しない結果が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3557-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="a3557-164">生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://docs.microsoft.com/ef/core/querying/raw-sql)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="a3557-165">データベースに送信する SQL を確認します。</span><span class="sxs-lookup"><span data-stu-id="a3557-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="a3557-166">データベースに送信される実際の SQL クエリを表示できるようにすると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="a3557-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="a3557-167">ASP.NET Core の組み込みのログ記録機能を SQL クエリと更新プログラムを含むログ ファイルを書き込む EF コアに自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="a3557-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="a3557-168">このセクションでは、SQL ログの例をいくつかが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="a3557-169">開いている*StudentsController.cs*し、、`Details`メソッドにブレークポイントを設定する、`if (student == null)`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="a3557-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="a3557-170">デバッグ モードでアプリを実行して、受講者についての詳細 ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a3557-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="a3557-171">移動して、**出力**デバッグを表示するウィンドウは次の出力、およびクエリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3557-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="a3557-172">ここに何かされる驚くかもしれませんがわかります: SQL が最大 2 つの行を選択します (`TOP(2)`)、Person テーブルからです。</span><span class="sxs-lookup"><span data-stu-id="a3557-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="a3557-173">`SingleOrDefaultAsync`メソッドは、サーバー上の 1 行に解決しません。</span><span class="sxs-lookup"><span data-stu-id="a3557-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="a3557-174">その理由を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-174">Here's why:</span></span>

* <span data-ttu-id="a3557-175">クエリでは、複数の行を返すと、メソッドは null を返します。</span><span class="sxs-lookup"><span data-stu-id="a3557-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="a3557-176">クエリが複数の行を返すかどうかを判断するのには EF が少なくとも 2 を返すかどうかを確認する必要です。</span><span class="sxs-lookup"><span data-stu-id="a3557-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="a3557-177">デバッグ モードを使用して、出力を取得するログ記録のブレークポイントで停止がないことに注意してください、**出力**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="a3557-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="a3557-178">だけを容易に出力を確認するポイントでログ記録を停止することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a3557-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="a3557-179">かどうかしないように、ログ記録を続行しようとしているに関心があるパーツの検索に戻るまでスクロールします。</span><span class="sxs-lookup"><span data-stu-id="a3557-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="a3557-180">リポジトリと作業パターンの単位</span><span class="sxs-lookup"><span data-stu-id="a3557-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="a3557-181">多くの開発者は、Entity Framework で動作するコードをラップするラッパーとしてリポジトリと作業パターンの単位を実装するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a3557-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="a3557-182">これらのパターンは、データ アクセス層とアプリケーションのビジネス ロジック層の間で抽象化レイヤーを作成するためのものです。</span><span class="sxs-lookup"><span data-stu-id="a3557-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="a3557-183">これらのパターンを実装して、変更によって、データ ストアにアプリケーションを分離できます、自動化された単体テストまたはテスト駆動開発 (TDD) に容易にすることができます。</span><span class="sxs-lookup"><span data-stu-id="a3557-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="a3557-184">ただし、これらのパターンを実装する追加のコードの記述は常にいくつかの理由、EF を使用するアプリケーションに最適な選択肢。</span><span class="sxs-lookup"><span data-stu-id="a3557-184">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="a3557-185">EF コンテキスト クラス自体は、データ ストア固有のコードからコードを分離します。</span><span class="sxs-lookup"><span data-stu-id="a3557-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="a3557-186">EF コンテキスト クラスは、データベースの作業単位クラス更新 EF を使用してを実行することで動作できます。</span><span class="sxs-lookup"><span data-stu-id="a3557-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="a3557-187">EF には、リポジトリのコードを記述することがなく TDD を実装するための機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a3557-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="a3557-188">リポジトリと作業パターンの単位を実装する方法については、次を参照してください。[このチュートリアル シリーズの Entity Framework 5 バージョン](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)します。</span><span class="sxs-lookup"><span data-stu-id="a3557-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="a3557-189">Entity Framework Core では、テストに使用できるメモリ内のデータベース プロバイダーを実装します。</span><span class="sxs-lookup"><span data-stu-id="a3557-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="a3557-190">詳細については、次を参照してください。 [InMemory でテスト](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="a3557-191">自動の変更の検出</span><span class="sxs-lookup"><span data-stu-id="a3557-191">Automatic change detection</span></span>

<span data-ttu-id="a3557-192">Entity Framework では、元の値を持つエンティティの現在の値を比較することによってエンティティがどのように変更されたか (およびしたがって、更新プログラムがデータベースに送信する必要があります) を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3557-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="a3557-193">元の値は、エンティティがクエリを実行したり、接続されているときに格納されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="a3557-194">自動の変更の検出が発生する方法の一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3557-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="a3557-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a3557-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="a3557-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="a3557-196">DbContext.Entry</span></span>

* <span data-ttu-id="a3557-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="a3557-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="a3557-198">多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、`ChangeTracker.AutoDetectChangesEnabled`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a3557-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="a3557-199">例:</span><span class="sxs-lookup"><span data-stu-id="a3557-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="a3557-200">Entity Framework Core のソース コードと開発計画</span><span class="sxs-lookup"><span data-stu-id="a3557-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="a3557-201">Entity Framework Core のソース コードは[https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-201">The source code for Entity Framework Core is available at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="a3557-202">ソース コードだけでなくすることができます夜間ビルドを取得、懸案事項の管理、機能仕様、設計ミーティングのメモ[将来開発のためのロードマップ](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)などです。</span><span class="sxs-lookup"><span data-stu-id="a3557-202">Besides source code, you can get nightly builds, issue tracking, feature specs, design meeting notes, [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap), and more.</span></span> <span data-ttu-id="a3557-203">バグ、ファイルを作成でき、EF ソース コードに独自の機能強化に協力することができます。</span><span class="sxs-lookup"><span data-stu-id="a3557-203">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="a3557-204">ソース コードは開いているが、Entity Framework のコアが完全にマイクロソフト製品サポートします。</span><span class="sxs-lookup"><span data-stu-id="a3557-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="a3557-205">Microsoft Entity Framework チームは、コントロール コントリビューションの受け入れを保持し、各リリースの品質を保証するすべてのコード変更をテストします。</span><span class="sxs-lookup"><span data-stu-id="a3557-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="a3557-206">既存のデータベースをリバース エンジニア リング</span><span class="sxs-lookup"><span data-stu-id="a3557-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="a3557-207">既存のデータベースからのエンティティ クラスを含むデータ モデルをリバース エンジニア リングを使用して、 [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext)コマンド。</span><span class="sxs-lookup"><span data-stu-id="a3557-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="a3557-208">参照してください、[入門チュートリアル](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="a3557-209">動的な LINQ 並べ替え選択コードを簡略化を使用してください。</span><span class="sxs-lookup"><span data-stu-id="a3557-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="a3557-210">[このシリーズの第 3 のチュートリアル](sort-filter-page.md)で列名をハード コーディングでの LINQ のコードを記述する方法を示しています、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="a3557-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="a3557-211">2 つの列から選択するでは、これは正常に機能しますが、コードの詳細な取得でした多数の列がある場合。</span><span class="sxs-lookup"><span data-stu-id="a3557-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="a3557-212">問題を解決するために使用することができます、`EF.Property`を文字列として、プロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3557-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="a3557-213">このアプローチには、置換、`Index`メソッドで、`StudentsController`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="a3557-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="a3557-214">次の手順</span><span class="sxs-lookup"><span data-stu-id="a3557-214">Next steps</span></span>

<span data-ttu-id="a3557-215">これは、この一連の ASP.NET MVC アプリケーションで Entity Framework のコアを使用してチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="a3557-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="a3557-216">EF Core の詳細については、次を参照してください。、 [Entity Framework の主要なマニュアル](https://docs.microsoft.com/ef/core)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="a3557-217">本も利用できます:[アクションで Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="a3557-218">Web アプリを展開する方法については、次を参照してください。[ホストを展開および](xref:host-and-deploy/index)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="a3557-219">については、認証および承認など、ASP.NET Core MVC に関連するその他のトピックを参照してください、 [ASP.NET Core ドキュメント](https://docs.microsoft.com/aspnet/core/)です。</span><span class="sxs-lookup"><span data-stu-id="a3557-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="a3557-220">受信確認</span><span class="sxs-lookup"><span data-stu-id="a3557-220">Acknowledgments</span></span>

<span data-ttu-id="a3557-221">Tom Dykstra と Rick Anderson (twitter @RickAndMSFT) このチュートリアルを記述します。</span><span class="sxs-lookup"><span data-stu-id="a3557-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="a3557-222">Rowan Miller、Diego ヴェガおよび Entity Framework チームの他のメンバー コード レビューを支援し、チュートリアルのコードを記述おされたときに発生した問題のデバッグに役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="a3557-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="a3557-223">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="a3557-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="a3557-224">別のプロセスによって使用される ContosoUniversity.dll</span><span class="sxs-lookup"><span data-stu-id="a3557-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="a3557-225">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="a3557-225">Error message:</span></span>

> <span data-ttu-id="a3557-226">開くことができません '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 書き込み--用 '、プロセスがファイルにアクセスできません'... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 別のプロセスによって使用されているためです。</span><span class="sxs-lookup"><span data-stu-id="a3557-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="a3557-227">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a3557-227">Solution:</span></span>

<span data-ttu-id="a3557-228">IIS express サイトを停止します。</span><span class="sxs-lookup"><span data-stu-id="a3557-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="a3557-229">Windows のシステム トレイに IIS Express しそのアイコンを右クリックし、Contoso 大学サイトを選択してを見つけてクリックを参照してください**サイトの停止**です。</span><span class="sxs-lookup"><span data-stu-id="a3557-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="a3557-230">コードを使用せずにメソッドを上下スキャフォールディングされた移行</span><span class="sxs-lookup"><span data-stu-id="a3557-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="a3557-231">考えられる原因:</span><span class="sxs-lookup"><span data-stu-id="a3557-231">Possible cause:</span></span>

<span data-ttu-id="a3557-232">EF CLI コマンドしない自動的に閉じてコード ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a3557-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="a3557-233">未保存の変更を実行すると場合、`migrations add`コマンド、EF、変更は検出されません。</span><span class="sxs-lookup"><span data-stu-id="a3557-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="a3557-234">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a3557-234">Solution:</span></span>

<span data-ttu-id="a3557-235">実行、`migrations remove`コマンドをコードの変更を保存して再実行、`migrations add`コマンド。</span><span class="sxs-lookup"><span data-stu-id="a3557-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="a3557-236">実行中のデータベースの更新中にエラー</span><span class="sxs-lookup"><span data-stu-id="a3557-236">Errors while running database update</span></span>

<span data-ttu-id="a3557-237">既存のデータを含むデータベースでスキーマ変更を行うときにその他のエラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="a3557-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="a3557-238">移行エラーを解決できない場合は、いずれか、接続文字列にデータベース名を変更したり、データベースを削除できます。</span><span class="sxs-lookup"><span data-stu-id="a3557-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="a3557-239">新しいデータベースを移行するデータが存在しないと、データベースの更新コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="a3557-239">With a new database, there is no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="a3557-240">データベースの名前を変更する最も簡単な方法は、*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="a3557-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="a3557-241">次に実行したとき`database update`、新しいデータベースは作成されます。</span><span class="sxs-lookup"><span data-stu-id="a3557-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="a3557-242">SSOX でデータベースを削除するデータベースを右クリックして**削除**、し、**データベースの削除**ダイアログ ボックスの **[既存の接続を閉じる]**をクリック**[Ok]**です。</span><span class="sxs-lookup"><span data-stu-id="a3557-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="a3557-243">CLI を使用してデータベースを削除するには、実行、 `database drop` CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="a3557-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="a3557-244">SQL Server インスタンスの検索エラー</span><span class="sxs-lookup"><span data-stu-id="a3557-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="a3557-245">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="a3557-245">Error Message:</span></span>

> <span data-ttu-id="a3557-246">SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="a3557-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="a3557-247">サーバーが見つからないかアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a3557-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="a3557-248">インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a3557-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="a3557-249">(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)</span><span class="sxs-lookup"><span data-stu-id="a3557-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="a3557-250">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a3557-250">Solution:</span></span>

<span data-ttu-id="a3557-251">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="a3557-251">Check the connection string.</span></span> <span data-ttu-id="a3557-252">データベース ファイルを手動で削除する場合は、新しいデータベースを最初からやり直す構築文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="a3557-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a3557-253">前へ</span><span class="sxs-lookup"><span data-stu-id="a3557-253">Previous</span></span>](inheritance.md)
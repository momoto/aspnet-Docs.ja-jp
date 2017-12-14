---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: "ASP.NET MVC で最初の EF Database: データベースの変更 |Microsoft ドキュメント"
author: tfitzmac
description: "MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="31b54-104">ASP.NET MVC で最初の EF Database: データベースの変更</span><span class="sxs-lookup"><span data-stu-id="31b54-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="31b54-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="31b54-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="31b54-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="31b54-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="31b54-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="31b54-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="31b54-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="31b54-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="31b54-109">系列のこの部分は、データベース構造への更新を行うと、web アプリケーション全体で変更を伝達について説明します。</span><span class="sxs-lookup"><span data-stu-id="31b54-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="31b54-110">列の追加</span><span class="sxs-lookup"><span data-stu-id="31b54-110">Add a column</span></span>

<span data-ttu-id="31b54-111">データベースのテーブルの構造を更新する場合、変更内容をデータ モデル、ビュー、およびコント ローラーに反映されることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31b54-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="31b54-112">このチュートリアルでは、学生のミドル ネームを記録する、受講者テーブルに新しい列を追加します。</span><span class="sxs-lookup"><span data-stu-id="31b54-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="31b54-113">この列を追加するには、データベース プロジェクトを開き Student.sql ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="31b54-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="31b54-114">デザイナーまたは T-SQL コードのどちらかを通じてという名前の列を追加**MiddleName**を nvarchar (50) は、NULL 値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="31b54-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![ミドル ネームを追加します。](changing-the-database/_static/image1.png)

<span data-ttu-id="31b54-116">この変更をデータベース プロジェクト (または f5 キー) を起動して、ローカル データベースに配置します。</span><span class="sxs-lookup"><span data-stu-id="31b54-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="31b54-117">新しいフィールドは、テーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="31b54-117">The new field is added to the table.</span></span> <span data-ttu-id="31b54-118">表示されないこと、SQL Server オブジェクト エクスプ ローラーで、ウィンドウで [更新] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="31b54-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![新しい列を表示します。](changing-the-database/_static/image2.png)

<span data-ttu-id="31b54-120">データベース テーブルに新しい列が存在しますが、データ モデル クラス内に現在存在しません。</span><span class="sxs-lookup"><span data-stu-id="31b54-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="31b54-121">新しい列に含めるモデルを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31b54-121">You must update the model to include your new column.</span></span> <span data-ttu-id="31b54-122">**モデル**フォルダーを開き、 **ContosoModel.edmx**モデル ダイアグラムを表示するファイル。</span><span class="sxs-lookup"><span data-stu-id="31b54-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="31b54-123">学生のモデルに MiddleName プロパティが含まれていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="31b54-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="31b54-124">デザイン サーフェイス上の任意の場所を右クリックし、選択**データベースからモデルを更新**です。</span><span class="sxs-lookup"><span data-stu-id="31b54-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![モデルを更新します](changing-the-database/_static/image3.png)

<span data-ttu-id="31b54-126">更新ウィザードで選択、**更新** タブおよび**学生**テーブル。</span><span class="sxs-lookup"><span data-stu-id="31b54-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![更新ウィザード](changing-the-database/_static/image4.png)

<span data-ttu-id="31b54-128">**[完了]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31b54-128">Click **Finish**.</span></span>

<span data-ttu-id="31b54-129">データベース ダイアグラムには、新しい更新プロセスが完了したら、 **MiddleName**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="31b54-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="31b54-130">保存、 **ContosoModel.edmx**ファイル。</span><span class="sxs-lookup"><span data-stu-id="31b54-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="31b54-131">新しいプロパティに反映されるまでのこのファイルを保存する必要があります、 **Student.cs**クラスです。</span><span class="sxs-lookup"><span data-stu-id="31b54-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="31b54-132">データベースとモデルを更新したようになりました。</span><span class="sxs-lookup"><span data-stu-id="31b54-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="31b54-133">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="31b54-133">Build the solution.</span></span>

<span data-ttu-id="31b54-134">残念ながら、ビューはまだ、新しいプロパティを含んでいません。</span><span class="sxs-lookup"><span data-stu-id="31b54-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="31b54-135">ビューを更新するには、2 つのオプション - ことができますか、再生成するビューには、Student クラスのスキャフォールディングをもう一度追加しても、既存のビューに新しいプロパティを手動で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="31b54-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="31b54-136">このチュートリアルでは、スキャフォールディングを追加します再を自動的に生成されたビューにカスタマイズされた変更が加えられるいないためです。</span><span class="sxs-lookup"><span data-stu-id="31b54-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="31b54-137">ビューを変更し、それらの変更を破棄したくない場合に手動でプロパティを追加することを検討する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="31b54-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="31b54-138">ビューが再作成されたためには、削除、**受講者**下にあるフォルダー**ビュー**、および削除、 **StudentsController**です。</span><span class="sxs-lookup"><span data-stu-id="31b54-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="31b54-139">右クリックし、**コント ローラー**フォルダーのスキャフォールディングを追加し、**学生**モデル。</span><span class="sxs-lookup"><span data-stu-id="31b54-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="31b54-140">コント ローラーの名前をもう一度、 **StudentsController**です。</span><span class="sxs-lookup"><span data-stu-id="31b54-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="31b54-141">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="31b54-141">Select **OK**.</span></span>

<span data-ttu-id="31b54-142">ビューには、MiddleName プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="31b54-142">The views now contain the MiddleName property.</span></span>

![ミドル ネームを表示します。](changing-the-database/_static/image5.png)

<span data-ttu-id="31b54-144">次のセクションでは、student レコードに関する詳細を表示するビューをカスタマイズするためのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="31b54-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="31b54-145">[前へ](generating-views.md)
[次へ](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="31b54-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "JQuery UI を使用した DropDownList に新しいカテゴリを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 0cc51fbe84124a62f0c1254faab796cbcdc7efd6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="8b53a-102">JQuery UI を使用した DropDownList に新しいカテゴリを追加します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="8b53a-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8b53a-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="8b53a-104">HTML`Select`タグが固定のカテゴリのデータの一覧を表示するのに適していますが、多くの場合、新しいカテゴリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b53a-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="8b53a-105">データベース内のカテゴリをジャンル"Opera"を追加する必要があるとしますか。</span><span class="sxs-lookup"><span data-stu-id="8b53a-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="8b53a-106">このセクションを使用して、新しいカテゴリを追加 ダイアログ ボックスを追加するのに、jQuery UI を使用します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="8b53a-107">次の図は、ブラウザーで UI を提示する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8b53a-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="8b53a-108">ユーザーが選択すると、**新しいジャンルの追加**リンク、ポップアップ ダイアログ ボックスは、新しいジャンル名前 (および必要に応じて説明) はユーザーを要求します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="8b53a-109">次に示す、イメージ、**追加ジャンル**ポップアップ ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="8b53a-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="8b53a-110">新しいジャンル名が入力されている場合、**保存**ボタンが押されて、次の動作します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="8b53a-111">AJAX 呼び出しは、ジャンル コント ローラー、新しいジャンルをデータベースに保存し、JSON として、新しいジャンル情報 (ジャンルの名前と ID) を返しますの Create メソッドにデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="8b53a-112">JavaScript では、選択リストに新しいジャンル データを追加します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="8b53a-113">JavaScript は、選択した項目を新しいジャンルになります。</span><span class="sxs-lookup"><span data-stu-id="8b53a-113">JavaScript makes the new genre the selected item.</span></span>

 <span data-ttu-id="8b53a-114">下の画像で**Opera**データベースに追加されで選択されている、**ジャンル**ドロップ ダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="8b53a-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="8b53a-115">開く、 *Views\StoreManager\Create.cshtml*ファイルを開き、次のマークアップをジャンルを置き換えます次のコード。</span><span class="sxs-lookup"><span data-stu-id="8b53a-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="8b53a-116">`_ChooseGenre`部分ビューには、JavaScript と jQuery の新しいジャンルの追加 機能を実装するために使用をフックするために、すべてのロジックにが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="8b53a-117">コードが完成した後は、アーティスト UI で同じ操作を実行する単純なことはできます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="8b53a-118">ソリューション エクスプ ローラーで右クリックし、 *Views\StoreManager*フォルダーと選択**追加**、し**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="8b53a-119">**ビュー名**入力、入力`_ChooseGenre`を選択し、**追加**です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="8b53a-120">内のマークアップを置き換える、 *Views\StoreManager\\_ChooseGenre.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="8b53a-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="8b53a-121">最初の行で渡していることを宣言、`Album`モデルとビューを作成するステートメントをモデルとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="8b53a-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="8b53a-122">次の数行、**ラベル**ヘルパー マークアップ。</span><span class="sxs-lookup"><span data-stu-id="8b53a-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="8b53a-123">次の行は、 **DropDownList**ヘルパーを呼び出すと、元のビューを作成するものとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="8b53a-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="8b53a-124">次の行の名前を持つリンクを追加`Add New Genre`、およびボタンのようなスタイルします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="8b53a-125">行を含む`ValidationMessageFor`作成ビューから直接コピーされます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="8b53a-126">次の行では:</span><span class="sxs-lookup"><span data-stu-id="8b53a-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="8b53a-127">ID を持つ非表示の div を作成`genreDialog`です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="8b53a-128">JQuery を使用してフック、**追加ジャンル**ID を持つ ダイアログ ボックス`genreDialog`この位置の div で</span><span class="sxs-lookup"><span data-stu-id="8b53a-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="8b53a-129">最後の 2 つのスクリプト タグには、追加新しいジャンル機能の実装を使用して、JavaScript ファイルへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8b53a-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="8b53a-130">*/Scripts/chooseGenre.js*ファイルがプロジェクト内の見ていきますが、チュートリアルの後半で提供されています。</span><span class="sxs-lookup"><span data-stu-id="8b53a-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="8b53a-131">アプリケーションを実行し、をクリックして、**新しいジャンルの追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="8b53a-132">**追加ジャンル** ダイアログ ボックスで、入力**Opera**で、**名前**入力ボックス。</span><span class="sxs-lookup"><span data-stu-id="8b53a-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="8b53a-133">クリックして、**保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-133">Click the **Save** button.</span></span> <span data-ttu-id="8b53a-134">AJAX 呼び出しは Opera カテゴリを作成およびし、Opera、ドロップダウン リストを設定し、選択したジャンルとして Opera を設定します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="8b53a-135">アーティスト、タイトル、および価格を入力し、選択、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="8b53a-136">8.99 ドル未満の価格を入力する場合は、新しいアルバムがインデックス ビューの上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="8b53a-137">新しいアルバム エントリがデータベースに保存されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="8b53a-138">1 つだけの文字で新しいジャンルを作成してください。</span><span class="sxs-lookup"><span data-stu-id="8b53a-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="8b53a-139">次のコードで、 *Models\Genre.cs*ファイルはジャンル名の最小値と最大の長さを設定します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="8b53a-140">クライアント側の検証は、2 ~ 20 文字、文字列を入力する必要がありますを報告します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="8b53a-141">新しいジャンル方法を調べることが、データベースと、選択リストに追加されます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="8b53a-142">開く、 *Scripts\chooseGenre.js*ファイルし、コードを調べます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="8b53a-143">2 番目の行は、ID を使用して`genreDialog`の div タグのダイアログ ボックスを作成する、 *Views\StoreManager\\_ChooseGenre.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b53a-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="8b53a-144">名前付きパラメーターのほとんどは自明です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="8b53a-145">`autoOpen`パラメーターが false に設定を選択すると、**作成ジャンル**ボタンがダイアログ ボックスを明示的に開く (後者にこの説明は)。</span><span class="sxs-lookup"><span data-stu-id="8b53a-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="8b53a-146">ダイアログ ボックスが 2 つのボタン**保存**と**キャンセル**です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="8b53a-147">**キャンセル**ボタンがダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="8b53a-148">次のコードは、**保存**関数のボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="8b53a-149">`var createGenreForm`からが選択されている、 `createGenreForm` id です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="8b53a-150">`createGenreForm`で見つかった次のコードで ID が設定されて、 *Views\Genre\\_CreateGenre.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b53a-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="8b53a-151">[Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx)ヘルパーのオーバー ロードで使用される、 *Views\Genre\\_CreateGenre.cshtml*ファイルは、フォームを送信する URL を含むアクション属性を持つ HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-151">The [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="8b53a-152">これは、ブラウザーでアルバムの作成 ページを表示して、ブラウザーで表示するソースを選択して確認できます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="8b53a-153">次のマークアップでは、生成された HTML フォーム タグを含むを示します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="8b53a-154">JQuery`$.post`線は、action 属性への AJAX 呼び出しを作成 (`/StoreManager/Create`) からのデータで渡すと、**ジャンルの作成** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="8b53a-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="8b53a-155">データは、新しいジャンルとオプションの説明の名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="8b53a-156">AJAX 呼び出しが成功した場合は、ジャンルの新しい名前と値は、Select のマークアップに追加され、新しいジャンルが選択した値に設定されています。</span><span class="sxs-lookup"><span data-stu-id="8b53a-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="8b53a-157">これは動的に生成されたマークアップであるため、ブラウザーで、ソースを表示して、新しいオプションを選択を参照してくださいことはできません。</span><span class="sxs-lookup"><span data-stu-id="8b53a-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="8b53a-158">IE 9 F12 開発者ツールを使用して新しい HTML を表示できます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="8b53a-159">表示するには、新しいオプションを選択、Internet Explorer 9 では、F12 開発者ツールを起動する F12 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="8b53a-160">[作成] ページに移動し、新しいジャンルがジャンルの選択リストで選択されているために、新しいジャンルを追加します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="8b53a-161">F12 開発者ツールで。</span><span class="sxs-lookup"><span data-stu-id="8b53a-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="8b53a-162">[HTML] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="8b53a-163">更新アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="8b53a-164">[検索] ボックスで、GenreID を入力します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="8b53a-165">[次へ] のアイコンを使用</span><span class="sxs-lookup"><span data-stu-id="8b53a-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 <span data-ttu-id="8b53a-166">次の select タグに移動します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="8b53a-167">最後のオプションの値を展開します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="8b53a-168">次のコードで、 *Scripts\chooseGenre.js*ファイル、方法を示します、**新しいジャンルの追加**ボタン クリック イベントに接続方法、および**新しいジャンルの追加** ダイアログ ボックスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="8b53a-169">最初の行にアタッチされているをクリックして関数を作成する、**新しいジャンルの追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="8b53a-170">Views\StoreManager から次のマークアップ\\_ChooseGenre.cshtml ファイルの表示方法、**新しいジャンルの追加** ボタンを作成します。</span><span class="sxs-lookup"><span data-stu-id="8b53a-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="8b53a-171">Load メソッドの作成およびジャンルの追加 ダイアログを開く、し、jQuery を呼び出す`parse`メソッド、ダイアログ ボックスに入力されたデータにクライアントの検証が発生するようにします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="8b53a-172">このセクションでは、選択リストに新しいカテゴリのデータを追加するために使用するダイアログを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="8b53a-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="8b53a-173">新しいアーティストをアーティストの選択リストに追加するための UI を作成する同じ手順を実行できます。</span><span class="sxs-lookup"><span data-stu-id="8b53a-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="8b53a-174">このチュートリアルには、ASP.NET MVC の HTML ヘルパーの使用の概要から与えられた**DropDownList**です。</span><span class="sxs-lookup"><span data-stu-id="8b53a-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="8b53a-175">操作の詳細については、 **DropDownList**、以下の追加の参照セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b53a-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="8b53a-176">お知らせかどうか、このチュートリアルは便利なされました。</span><span class="sxs-lookup"><span data-stu-id="8b53a-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="8b53a-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="8b53a-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="8b53a-178">その他のリファレンス</span><span class="sxs-lookup"><span data-stu-id="8b53a-178">Additional References</span></span>

- <span data-ttu-id="8b53a-179">[ASP.NET MVC – カスケード ドロップダウン一覧チュートリアル](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)によって[Radu enuca が](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="8b53a-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="8b53a-180">[選択した](http://harvesthq.github.com/chosen/)複数選択やフィルター処理をサポートする JavaScript プラグインします。</span><span class="sxs-lookup"><span data-stu-id="8b53a-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="8b53a-181">貢献者</span><span class="sxs-lookup"><span data-stu-id="8b53a-181">Contributors</span></span>

- [<span data-ttu-id="8b53a-182">Radu enuca が</span><span class="sxs-lookup"><span data-stu-id="8b53a-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="8b53a-183">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="8b53a-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="8b53a-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="8b53a-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="8b53a-185">校閲者</span><span class="sxs-lookup"><span data-stu-id="8b53a-185">Reviewers</span></span>

- <span data-ttu-id="8b53a-186">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="8b53a-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="8b53a-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="8b53a-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="8b53a-188">Mike 教皇</span><span class="sxs-lookup"><span data-stu-id="8b53a-188">Mike Pope</span></span>
- <span data-ttu-id="8b53a-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="8b53a-189">Tom Dykstra</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8b53a-190">前へ</span><span class="sxs-lookup"><span data-stu-id="8b53a-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
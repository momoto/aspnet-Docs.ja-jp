---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "MVC データベースの最初のサイトを Azure に発行 |Microsoft ドキュメント"
author: tfitzmac
description: "MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: f75b7192b4d97c88fcbcb4ad7fef88c83157c902
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="b6cb6-104">MVC データベースの最初のサイトを Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="b6cb6-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b6cb6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b6cb6-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b6cb6-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b6cb6-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b6cb6-109">系列のこの部分は、web アプリとデータベースを Azure に発行について説明します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="b6cb6-110">実際に手順を実行する、チュートリアルの先頭から開始する必要がありますが、web アプリとデータベースの発行に関する情報については、このトピックを読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="b6cb6-111">参照してください[作業の開始](setting-up-database.md)です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="b6cb6-112">Azure で web アプリを配置します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="b6cb6-113">Azure アカウントをこのチュートリアルを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="b6cb6-114">実行できます[無料の Azure アカウントを開く](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-114">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b6cb6-115">実行できます[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-、MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="b6cb6-116">Web アプリを発行するには、プロジェクトを右クリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![サイトを発行します。](publish-to-azure/_static/image1.png)

<span data-ttu-id="b6cb6-118">Microsoft Azure web サイトを選択します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-118">Select Microsoft Azure Websites.</span></span>

![Azure を選択します](publish-to-azure/_static/image2.png)

<span data-ttu-id="b6cb6-120">Azure にサインインしていない場合は、Azure アカウント資格情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="b6cb6-121">新しい web アプリを作成する新規をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-121">Then, select New to create a new web app.</span></span>

![新しいサイト](publish-to-azure/_static/image3.png)

<span data-ttu-id="b6cb6-123">Web アプリの一意の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-123">Create a unique name for your web app.</span></span> <span data-ttu-id="b6cb6-124">わかります名前が一意では、name フィールドの右側に緑のチェック マークが表示される場合。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="b6cb6-125">Web アプリ用の領域を選択します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-125">Select a region for your web app.</span></span> <span data-ttu-id="b6cb6-126">選択**を作成する新しいサーバー**データベースの新しいデータベース サーバーのユーザー名とパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="b6cb6-127">終了したら、クリックして**作成**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-127">When finished, click **Create**.</span></span>

![サイトを作成します。](publish-to-azure/_static/image4.png)

<span data-ttu-id="b6cb6-129">接続の値がすべて設定されています。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-129">Your connection values are now all set.</span></span> <span data-ttu-id="b6cb6-130">これらの値をそのままにしておくことができます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="b6cb6-132">**[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-132">Click **Next**.</span></span>

<span data-ttu-id="b6cb6-133">設定については、2 つのデータベース接続であることに注意してくださいが指定された - ApplicationDBContext と ContosoUniversityDataEntities です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="b6cb6-134">ApplicationDBContext は、ユーザー アカウントのテーブルの接続です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="b6cb6-135">これらの値は、データベースの接続文字列のみを表示します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="b6cb6-136">サイトを発行するときに、これらのデータベースを公開することがありません。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="b6cb6-137">Web アプリを発行した後は、データベース プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="b6cb6-138">データベース接続の横にある省略記号 (...) では、接続文字列の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="b6cb6-139">ContosoUniversityDataEntities の横にある省略記号ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="b6cb6-140">データベース サーバーとデータベースの名前に注意してください。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="b6cb6-141">サーバー名がランダムに生成されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-141">The server name is randomly generated.</span></span> <span data-ttu-id="b6cb6-142">データベース名は単純なを使用してサイトの名前 **\_db**追加されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="b6cb6-143">データベースを発行するときに、両方の名前を後で必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="b6cb6-144">をクリックして**OK**データベース接続文字列 ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="b6cb6-145">[Web の発行] ウィンドウ**次**プレビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="b6cb6-146">発行するファイルの一覧を表示するプレビューの開始をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="b6cb6-147">これはこのサイトを発行した最初の時間であるため、プロジェクト内のすべての関連するファイルは表示されません。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="b6cb6-148">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-148">Click **Publish**.</span></span>

<span data-ttu-id="b6cb6-149">[出力] ペインのパブリケーションには、結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="b6cb6-150">発行した後は、サイトは、web ブラウザーで起動 immedialely がします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="b6cb6-151">サイトが展開され、サイトに新しいユーザーを登録することができます。ただし、ContosoUniversityData プロジェクトにテーブルがまだ発行されていません。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="b6cb6-152">学生のリンクの一覧をクリックすると、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="b6cb6-153">データベースを SQL Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="b6cb6-154">データベースを発行する前に、ローカル コンピューターが、データベース サーバーに接続できることを確認しての操作を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="b6cb6-155">データベース サーバーのファイアウォールは、マシンが、データベースに接続可能を制限します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="b6cb6-156">ファイアウォールの許可された IP アドレスに、コンピューターの IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="b6cb6-157">Azure ポータルで Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="b6cb6-158">新しいデータベースを選択し、選択**管理**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-158">Select your new database and select **Manage**.</span></span>

![管理します。](publish-to-azure/_static/image10.png)

<span data-ttu-id="b6cb6-160">お使いのコンピューターから接続を許可するようにデータベース サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="b6cb6-161">管理を選択すると、データベース サーバーに許可されている現在の IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="b6cb6-162">[はい] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-162">Select Yes.</span></span>

![ip アドレスを追加します。](publish-to-azure/_static/image11.png)

<span data-ttu-id="b6cb6-164">前の手順で追加した IP アドレスは、接続用に構成する必要。 唯一の IP アドレスではない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="b6cb6-165">接続を正しく設定されているかどうかをデータベースにログインしようとすることができます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="b6cb6-166">以前に作成したパスワードとユーザーを入力します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-166">Provide the user and password you created earlier.</span></span>

![ログイン](publish-to-azure/_static/image12.png)

<span data-ttu-id="b6cb6-168">エラー メッセージを受信する場合は、別の IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="b6cb6-169">エラーに関する詳細を表示するエラー メッセージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="b6cb6-170">詳細情報を追加する必要のある IP アドレスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="b6cb6-171">この IP アドレスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-171">Note this IP address.</span></span>

![許可されていません](publish-to-azure/_static/image13.png)

<span data-ttu-id="b6cb6-173">このログイン ウィンドウを閉じて、Azure ポータルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="b6cb6-174">データベースのダッシュ ボードに移動します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="b6cb6-175">をクリックして**使用できる IP アドレス管理**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-175">Click **Manage allowed IP addresses**.</span></span>

![ip アドレスを管理します。](publish-to-azure/_static/image14.png)

<span data-ttu-id="b6cb6-177">エラー メッセージから IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="b6cb6-178">エラー メッセージから 1 つを含めるに許可される IP アドレスの範囲を変更するか、別のエントリとしてその IP アドレスを追加します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![新しいアドレスを追加します。](publish-to-azure/_static/image15.png)

<span data-ttu-id="b6cb6-180">許可される IP アドレスに変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="b6cb6-181">[管理] をクリックして、データベースにログインを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="b6cb6-182">許可される IP アドレスが正しく構成されて、ファイアウォールの前に、数分を待機する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="b6cb6-183">データベースが正常にログインすることができます、ときに、データベースへの接続の設定が完了しました。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="b6cb6-184">ことができますこの管理ウィンドウを開いたまま、データベースの配置の結果を間もなくチェックされますが、します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="b6cb6-185">データベース プロジェクトに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-185">Return to your database project.</span></span> <span data-ttu-id="b6cb6-186">プロジェクトを右クリックし **発行**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-186">Right-click the project and select **Publish**.</span></span>

![データベースを発行します。](publish-to-azure/_static/image16.png)

<span data-ttu-id="b6cb6-188">データベースの公開 ウィンドウで選択**編集**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-188">In the Publish Database window, select **Edit**.</span></span>

![編集](publish-to-azure/_static/image17.png)

<span data-ttu-id="b6cb6-190">サーバーのデータベース サーバーと、認証資格情報の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="b6cb6-191">資格情報を提供すると、利用可能なデータベースの一覧から作成したデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="b6cb6-192">既定は、Visual Studio は、データベース フィールドの名前を作成したデータベースと同じである可能性がありますいないプロジェクトの名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="b6cb6-193">[OK] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-193">Click OK.</span></span>

<span data-ttu-id="b6cb6-194">今後の更新プログラムを公開するには、すべての接続情報を再度入力せずにこのプロファイルを保存するが可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="b6cb6-195">選択**プロファイルを作成する**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-195">Select **Create Profile**.</span></span>

![プロファイルを保存します。](publish-to-azure/_static/image19.png)

<span data-ttu-id="b6cb6-197">同じ名前のプロジェクトで、ファイルが作成されます**ContosoUniversityData.publish.xml**です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="b6cb6-198">次回データベースを Azure に発行するには、そのプロファイルを単に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="b6cb6-199">ここで、をクリックして**発行**を Azure にデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="b6cb6-201">実行後、しばらくの間実行結果を発行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="b6cb6-203">ここで、する戻ることができます、管理ポータルに、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="b6cb6-204">[デザイン] ビューを更新し、3 つのテーブルに自動的に入力データが配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![新しいテーブル](publish-to-azure/_static/image22.png)

<span data-ttu-id="b6cb6-206">Azure に展開されている web アプリをテストする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="b6cb6-207">(Http://contosositeexample.azurewebsites.net/) など、Azure で web アプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="b6cb6-208">学生のリストのリンクをクリックし、受講者のインデックス ビューを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-208">Click the link for List of students and you should see the index view for students.</span></span>

![ビュー](publish-to-azure/_static/image23.png)

<span data-ttu-id="b6cb6-210">場合によっては、データベースとの接続は、適切に構成するのには少し時間を必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="b6cb6-211">エラーが発生した場合、数分待ってからもう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b6cb6-212">まとめ</span><span class="sxs-lookup"><span data-stu-id="b6cb6-212">Conclusion</span></span>

<span data-ttu-id="b6cb6-213">この系列には、ユーザーが編集、更新、作成、およびデータを削除するように既存のデータベースからコードを生成する方法の簡単な例が用意されています。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="b6cb6-214">これにより、ASP.NET MVC 5 の場合、Entity Framework と ASP.NET のスキャフォールディングがプロジェクトの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="b6cb6-215">Code First の開発の導入例は、次を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="b6cb6-216">高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションを Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="b6cb6-217">DbContext API を使用するデータベースを最初にデータを扱うのために、Code First でのデータを操作するために使用する API と同じことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="b6cb6-218">Database First を使用する場合でも、コードの最初のチュートリアルからなど、同時実行の競合を処理、読み取りと、関連するデータの更新などのより複雑なシナリオを処理する方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="b6cb6-219">唯一の違いは、データベース、コンテキスト クラス、およびエンティティ クラスを作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="b6cb6-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b6cb6-220">前へ</span><span class="sxs-lookup"><span data-stu-id="b6cb6-220">Previous</span></span>](enhancing-data-validation.md)
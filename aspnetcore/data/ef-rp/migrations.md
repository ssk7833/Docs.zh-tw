---
title: "使用 EF 核心--4 的移轉 8 razor 頁面"
author: rick-anderson
description: "在本教學課程中，您啟動管理資料模型變更 ASP.NET Core MVC 應用程式中的使用 EF 核心移轉功能。"
keywords: "ASP.NET Core,Entity Framework Core,移轉"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="53ba9-104">移轉的 EF 核心 Razor 頁面教學課程 (8 個 4)</span><span class="sxs-lookup"><span data-stu-id="53ba9-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="53ba9-105">由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53ba9-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="53ba9-106">在本教學課程中，會使用管理資料模型所做變更的 EF 核心移轉功能。</span><span class="sxs-lookup"><span data-stu-id="53ba9-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="53ba9-107">如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="53ba9-108">當開發新的應用程式時，資料模型經常變更。</span><span class="sxs-lookup"><span data-stu-id="53ba9-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="53ba9-109">每次將模型變更，此模型取得與資料庫同步處理。</span><span class="sxs-lookup"><span data-stu-id="53ba9-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="53ba9-110">本教學課程，啟動設定 Entity Framework 來建立資料庫，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="53ba9-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="53ba9-111">每次資料模型變更：</span><span class="sxs-lookup"><span data-stu-id="53ba9-111">Each time the data model changes:</span></span>

* <span data-ttu-id="53ba9-112">卸除資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-112">The DB is dropped.</span></span>
* <span data-ttu-id="53ba9-113">EF 會建立一個新符合模型。</span><span class="sxs-lookup"><span data-stu-id="53ba9-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="53ba9-114">應用程式植入的測試資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="53ba9-115">這個方法讓資料庫保持同步資料模型的效果也直到您將應用程式部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="53ba9-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="53ba9-116">當應用程式在生產環境中執行時，它通常儲存需要維護的資料。</span><span class="sxs-lookup"><span data-stu-id="53ba9-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="53ba9-117">應用程式的開頭不能測試每次進行變更 （例如加入新的資料行） 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="53ba9-118">EF 核心移轉功能來解決這個問題，進而更新資料庫結構描述，而不是建立新的 DB EF 核心。</span><span class="sxs-lookup"><span data-stu-id="53ba9-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="53ba9-119">而不是卸除並重新建立資料庫的資料模型變更時，移轉更新的結構描述，並會保留現有的資料。</span><span class="sxs-lookup"><span data-stu-id="53ba9-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="53ba9-120">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="53ba9-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="53ba9-121">若要使用移轉，使用**Package Manager Console** (PMC) 或命令列介面 (CLI)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="53ba9-122">這些教學課程會示範如何使用 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="53ba9-123">Pmc 依存的相關資訊位於[本教學課程結尾](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="53ba9-124">中所提供的命令列介面 (CLI) 的 EF 核心工具[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="53ba9-125">若要安裝此套件，將它加入`DotNetCliToolReference`集合*.csproj*檔案，如下所示。</span><span class="sxs-lookup"><span data-stu-id="53ba9-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="53ba9-126">**注意：**必須安裝此套件，藉由編輯*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="53ba9-127">`install-package`命令或封裝管理員 GUI 無法用來安裝此套件。</span><span class="sxs-lookup"><span data-stu-id="53ba9-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="53ba9-128">編輯*.csproj*檔案中的專案名稱上按一下滑鼠右鍵**方案總管 中**，然後選取**編輯 ContosoUniversity.csproj**。</span><span class="sxs-lookup"><span data-stu-id="53ba9-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="53ba9-129">下列標記會顯示更新*.csproj*反白顯示的 EF 核心 CLI 工具檔：</span><span class="sxs-lookup"><span data-stu-id="53ba9-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="53ba9-130">在上述範例中的版本號碼是目前寫入教學課程時。</span><span class="sxs-lookup"><span data-stu-id="53ba9-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="53ba9-131">使用相同版本的其他封裝中的 EF 核心 CLI 工具。</span><span class="sxs-lookup"><span data-stu-id="53ba9-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="53ba9-132">變更連接字串</span><span class="sxs-lookup"><span data-stu-id="53ba9-132">Change the connection string</span></span>

<span data-ttu-id="53ba9-133">在*appsettings.json*檔案中，將資料庫的連接字串的名稱變更為 ContosoUniversity2。</span><span class="sxs-lookup"><span data-stu-id="53ba9-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="53ba9-134">變更連接字串中的資料庫名稱，會導致第一個移轉建立新的 DB。</span><span class="sxs-lookup"><span data-stu-id="53ba9-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="53ba9-135">因為具有該名稱不存在，會建立新的 DB。</span><span class="sxs-lookup"><span data-stu-id="53ba9-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="53ba9-136">變更連接字串並不需要移轉使用者入門。</span><span class="sxs-lookup"><span data-stu-id="53ba9-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="53ba9-137">變更資料庫名稱的替代方式刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="53ba9-138">使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="53ba9-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="53ba9-139">下節說明如何執行 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="53ba9-140">建立初始的移轉</span><span class="sxs-lookup"><span data-stu-id="53ba9-140">Create an initial migration</span></span>

<span data-ttu-id="53ba9-141">建置專案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-141">Build the project.</span></span>

<span data-ttu-id="53ba9-142">開啟命令視窗並瀏覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="53ba9-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="53ba9-143">專案資料夾中包含*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="53ba9-144">[命令] 視窗中輸入下列內容：</span><span class="sxs-lookup"><span data-stu-id="53ba9-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="53ba9-145">[命令] 視窗會顯示與下列類似的資訊：</span><span class="sxs-lookup"><span data-stu-id="53ba9-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="53ba9-146">如果移轉失敗，訊息 「*無法存取檔案...ContosoUniversity.dll 因為另一個處理序正在使用它。*"</span><span class="sxs-lookup"><span data-stu-id="53ba9-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="53ba9-147">會顯示：</span><span class="sxs-lookup"><span data-stu-id="53ba9-147">is displayed:</span></span>

* <span data-ttu-id="53ba9-148">停止 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="53ba9-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="53ba9-149">結束並重新啟動 Visual Studio 中，或</span><span class="sxs-lookup"><span data-stu-id="53ba9-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="53ba9-150">尋找 Windows 系統匣中的 IIS Express 的圖示。</span><span class="sxs-lookup"><span data-stu-id="53ba9-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="53ba9-151">IIS Express] 圖示，以滑鼠右鍵按一下，然後按一下 [ **ContosoUniversity > 停止站台**。</span><span class="sxs-lookup"><span data-stu-id="53ba9-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="53ba9-152">如果錯誤訊息 「 建立失敗。 」</span><span class="sxs-lookup"><span data-stu-id="53ba9-152">If the error message "Build failed."</span></span> <span data-ttu-id="53ba9-153">隨即顯示，請再次執行命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-153">is displayed, run the command again.</span></span> <span data-ttu-id="53ba9-154">如果您收到這個錯誤，將附註保留在本教學課程的底部。</span><span class="sxs-lookup"><span data-stu-id="53ba9-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="53ba9-155">檢查向上和向下方法</span><span class="sxs-lookup"><span data-stu-id="53ba9-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="53ba9-156">EF 核心命令`migrations add`產生程式碼來建立從 DB。</span><span class="sxs-lookup"><span data-stu-id="53ba9-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="53ba9-157">此移轉程式碼位於*移轉\<時間戳記 > _InitialCreate.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="53ba9-158">`Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="53ba9-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="53ba9-159">`Down`方法會刪除它們，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="53ba9-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="53ba9-160">移轉呼叫`Up`方法，以實作資料模型變更，以進行移轉。</span><span class="sxs-lookup"><span data-stu-id="53ba9-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="53ba9-161">當您輸入命令，以回復更新、 移轉呼叫`Down`方法。</span><span class="sxs-lookup"><span data-stu-id="53ba9-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="53ba9-162">上述程式碼是初始移轉。</span><span class="sxs-lookup"><span data-stu-id="53ba9-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="53ba9-163">該程式碼建立時`migrations add InitialCreate`執行命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="53ba9-164">移轉 name 參數 (在範例中為"InitialCreate 」) 用於檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="53ba9-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="53ba9-165">移轉名稱可以是任何有效的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="53ba9-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="53ba9-166">您最好選擇的單字或片語來摘要列出所要完成移轉中的作業。</span><span class="sxs-lookup"><span data-stu-id="53ba9-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="53ba9-167">例如，加入 department 資料表移轉可能會呼叫"AddDepartmentTable。 」</span><span class="sxs-lookup"><span data-stu-id="53ba9-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="53ba9-168">如果初始的移轉會建立並 DB 結束：</span><span class="sxs-lookup"><span data-stu-id="53ba9-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="53ba9-169">資料庫建立程式碼會產生。</span><span class="sxs-lookup"><span data-stu-id="53ba9-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="53ba9-170">資料庫建立程式碼不需要執行，因為資料庫已經符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="53ba9-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="53ba9-171">如果執行的 DB 建立程式碼時，它不會進行任何變更，因為資料庫已經符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="53ba9-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="53ba9-172">當應用程式部署至新的環境時，資料庫建立程式碼必須執行來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="53ba9-173">先前的連接字串已變更為使用新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="53ba9-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="53ba9-174">指定的資料庫不存在，因此，移轉會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="53ba9-175">檢查資料模型快照集</span><span class="sxs-lookup"><span data-stu-id="53ba9-175">Examine the data model snapshot</span></span>

<span data-ttu-id="53ba9-176">建立移轉*快照*中目前的資料庫結構描述*Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="53ba9-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="53ba9-177">表示目前的資料庫結構描述時，程式碼中，因為 EF 核心沒有建立移轉資料庫與互動。</span><span class="sxs-lookup"><span data-stu-id="53ba9-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="53ba9-178">當您將在移轉時，EF 核心決定變更藉由比較資料模型，以快照集檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="53ba9-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="53ba9-179">更新資料庫時，才會與 DB 互動 EF 核心。</span><span class="sxs-lookup"><span data-stu-id="53ba9-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="53ba9-180">快照集檔案必須與建立它的移轉作業的同步處理。</span><span class="sxs-lookup"><span data-stu-id="53ba9-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="53ba9-181">無法刪除名為該檔案移除移轉*\<時間戳記 > _\<migrationname >.cs*。</span><span class="sxs-lookup"><span data-stu-id="53ba9-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="53ba9-182">如果刪除該檔案時，剩餘的移轉將會與資料庫快照集檔案同步處理。</span><span class="sxs-lookup"><span data-stu-id="53ba9-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="53ba9-183">若要刪除加入的最後一個移轉，請使用[dotnet ef 移轉移除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="53ba9-184">移除 EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="53ba9-184">Remove EnsureCreated</span></span>

<span data-ttu-id="53ba9-185">早期開發，`EnsureCreated`命令的使用。</span><span class="sxs-lookup"><span data-stu-id="53ba9-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="53ba9-186">在本教學課程，請使用移轉。</span><span class="sxs-lookup"><span data-stu-id="53ba9-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="53ba9-187">`EnsureCreated`具有下列 limatitions:</span><span class="sxs-lookup"><span data-stu-id="53ba9-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="53ba9-188">會略過移轉作業，並建立資料庫和結構描述。</span><span class="sxs-lookup"><span data-stu-id="53ba9-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="53ba9-189">不會建立移轉表格。</span><span class="sxs-lookup"><span data-stu-id="53ba9-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="53ba9-190">可以*不*搭配移轉。</span><span class="sxs-lookup"><span data-stu-id="53ba9-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="53ba9-191">可供測試或快速原型化的 DB 位置卸除並重新建立的頻率。</span><span class="sxs-lookup"><span data-stu-id="53ba9-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="53ba9-192">移除從下行`DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="53ba9-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="53ba9-193">適用於資料庫開發的移轉</span><span class="sxs-lookup"><span data-stu-id="53ba9-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="53ba9-194">在 [命令] 視窗中，輸入下列命令來建立資料庫和資料表。</span><span class="sxs-lookup"><span data-stu-id="53ba9-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="53ba9-195">注意： 如果`update`命令會傳回錯誤 「 建置失敗。 」:</span><span class="sxs-lookup"><span data-stu-id="53ba9-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="53ba9-196">重新執行命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-196">Run the command again.</span></span>
* <span data-ttu-id="53ba9-197">如果再次失敗，結束 Visual Studio，然後再執行`update`命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="53ba9-198">將保留在頁面底部的訊息。</span><span class="sxs-lookup"><span data-stu-id="53ba9-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="53ba9-199">此命令的輸出是類似於`migrations add`命令輸出。</span><span class="sxs-lookup"><span data-stu-id="53ba9-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="53ba9-200">在上述命令中，會顯示記錄檔中的設定資料庫的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="53ba9-201">大部分的記錄檔會在下列範例輸出中省略：</span><span class="sxs-lookup"><span data-stu-id="53ba9-201">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="53ba9-202">若要減少的記錄檔訊息詳細程度可以變更的記錄層級*appsettings。Development.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="53ba9-203">如需詳細資訊，請參閱[簡介記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="53ba9-204">使用**SQL Server 物件總管**檢查 DB。</span><span class="sxs-lookup"><span data-stu-id="53ba9-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="53ba9-205">請注意新增`__EFMigrationsHistory`資料表。</span><span class="sxs-lookup"><span data-stu-id="53ba9-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="53ba9-206">`__EFMigrationsHistory`資料表會追蹤的哪些移轉已經套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="53ba9-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="53ba9-207">檢視中的資料`__EFMigrationsHistory`資料表，它會顯示第一次移轉一個資料列。</span><span class="sxs-lookup"><span data-stu-id="53ba9-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="53ba9-208">上述的 CLI 輸出範例中的最後一個記錄檔會顯示建立此資料列 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="53ba9-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="53ba9-209">執行應用程式，並確認一切運作。</span><span class="sxs-lookup"><span data-stu-id="53ba9-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="53ba9-210">在生產環境中套用移轉</span><span class="sxs-lookup"><span data-stu-id="53ba9-210">Appling migrations in production</span></span>

<span data-ttu-id="53ba9-211">我們建議實際執行應用程式應該**不**呼叫[Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="53ba9-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="53ba9-212">`Migrate`不應該呼叫從伺服器陣列中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53ba9-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="53ba9-213">例如，如果應用程式已部署向外 （執行的應用程式的多個執行個體） 的雲端。</span><span class="sxs-lookup"><span data-stu-id="53ba9-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="53ba9-214">資料庫移轉應該在部署中，並在受控制的方式。</span><span class="sxs-lookup"><span data-stu-id="53ba9-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="53ba9-215">包括實際執行資料庫移轉方法：</span><span class="sxs-lookup"><span data-stu-id="53ba9-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="53ba9-216">若要建立 SQL 指令碼中使用移轉，並在部署中使用的 SQL 指令碼。</span><span class="sxs-lookup"><span data-stu-id="53ba9-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="53ba9-217">執行`dotnet ef database update`從受控制的環境。</span><span class="sxs-lookup"><span data-stu-id="53ba9-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="53ba9-218">使用 EF 核心`__MigrationsHistory`資料表是否有任何移轉作業執行。</span><span class="sxs-lookup"><span data-stu-id="53ba9-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="53ba9-219">如果資料庫是最新狀態，則會執行不移轉。</span><span class="sxs-lookup"><span data-stu-id="53ba9-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="53ba9-220">命令列介面 (CLI) 與Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="53ba9-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="53ba9-221">EF 核心工具來管理移轉是可從項目：</span><span class="sxs-lookup"><span data-stu-id="53ba9-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="53ba9-222">.NET core CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="53ba9-223">Visual Studio 中的 PowerShell cmdlet **Package Manager Console** (PMC) 視窗。</span><span class="sxs-lookup"><span data-stu-id="53ba9-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="53ba9-224">本教學課程示範如何使用 CLI，有些開發人員會習慣使用 pmc 依存。</span><span class="sxs-lookup"><span data-stu-id="53ba9-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="53ba9-225">Pmc 依存的 EF 核心命令位於[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)封裝。</span><span class="sxs-lookup"><span data-stu-id="53ba9-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="53ba9-226">此套件包含在[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="53ba9-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="53ba9-227">**重要事項：**這不是相同的封裝，做為您藉由編輯安裝 CLI *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="53ba9-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="53ba9-228">這個名稱結尾`Tools`，不同於 CLI 封裝名稱中結尾`Tools.DotNet`。</span><span class="sxs-lookup"><span data-stu-id="53ba9-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="53ba9-229">如需 CLI 命令的詳細資訊，請參閱[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="53ba9-230">如需 PMC 命令的詳細資訊，請參閱[Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="53ba9-231">疑難排解</span><span class="sxs-lookup"><span data-stu-id="53ba9-231">Troubleshooting</span></span>

<span data-ttu-id="53ba9-232">下載[已完成的應用程式，為此階段](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。</span><span class="sxs-lookup"><span data-stu-id="53ba9-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="53ba9-233">應用程式會產生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="53ba9-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="53ba9-234">解決方案： 執行`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="53ba9-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="53ba9-235">如果`update`命令會傳回錯誤 「 建置失敗。 」:</span><span class="sxs-lookup"><span data-stu-id="53ba9-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="53ba9-236">重新執行命令。</span><span class="sxs-lookup"><span data-stu-id="53ba9-236">Run the command again.</span></span>
* <span data-ttu-id="53ba9-237">將保留在頁面底部的訊息。</span><span class="sxs-lookup"><span data-stu-id="53ba9-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="53ba9-238">[上一頁](xref:data/ef-rp/sort-filter-page)
[下一頁](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="53ba9-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
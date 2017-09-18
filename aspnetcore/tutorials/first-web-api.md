---
title: "使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API"
author: rick-anderson
description: "使用 ASP.NET Core MVC 和 Visual Studio for Windows 建置 Web API"
keywords: "ASP.NET Core, WebAPI, Web API, REST, HTTP, 服務, HTTP 服務"
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: c57c73c6f9c60874ef88749b838ed1cc1d353ead
ms.sourcegitcommit: 7fef13045e98d716c589a2982613dad261694a65
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="e2378-104">使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="e2378-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="e2378-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="e2378-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e2378-106">在本教學課程中，您將建置 Web API 來管理「待辦事項」項目清單，</span><span class="sxs-lookup"><span data-stu-id="e2378-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e2378-107">而不會建置 UI。</span><span class="sxs-lookup"><span data-stu-id="e2378-107">You won’t build a UI.</span></span>

<span data-ttu-id="e2378-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="e2378-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e2378-109">Windows：使用 Visual Studio for Windows 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="e2378-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="e2378-110">macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="e2378-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="e2378-111">macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="e2378-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="e2378-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="e2378-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="e2378-113">若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf)。</span><span class="sxs-lookup"><span data-stu-id="e2378-113">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e2378-114">建立專案</span><span class="sxs-lookup"><span data-stu-id="e2378-114">Create the project</span></span>

<span data-ttu-id="e2378-115">從 Visual Studio 中，選取 [檔案] 功能表 > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="e2378-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="e2378-116">選取 [ASP.NET Core Web 應用程式 (.NET Core)] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="e2378-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="e2378-117">將專案命名為 `TodoApi`，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="e2378-117">Name the project `TodoApi` and select **OK**.</span></span>

![[新增專案] 對話方塊](first-web-api/_static/new-project.png)

<span data-ttu-id="e2378-119">在 [New ASP.NET Core Web Application - TodoApi] (新增 ASP.NET Core Web 應用程式 - TodoApi) 對話方塊中，選取 [Web API] 範本。</span><span class="sxs-lookup"><span data-stu-id="e2378-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="e2378-120">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="e2378-120">Select **OK**.</span></span> <span data-ttu-id="e2378-121">請**勿**選取 [Enable Docker Support] (啟用 Docker 支援)。</span><span class="sxs-lookup"><span data-stu-id="e2378-121">Do **not** select **Enable Docker Support**.</span></span>

![已從 ASP.NET Core 範本中選取 Web API 專案範本的 [新增 ASP.NET Web 應用程式] 對話方塊](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="e2378-123">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="e2378-123">Launch the app</span></span>

<span data-ttu-id="e2378-124">在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e2378-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="e2378-125">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port/api/values`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="e2378-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="e2378-126">Chrome、 Edge 和 Firefox 顯示下列項目：</span><span class="sxs-lookup"><span data-stu-id="e2378-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="e2378-127">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="e2378-127">Add a model class</span></span>

<span data-ttu-id="e2378-128">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="e2378-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="e2378-129">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="e2378-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e2378-130">新增名為 "Models" 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2378-130">Add a folder named "Models".</span></span> <span data-ttu-id="e2378-131">在方案總管中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e2378-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="e2378-132">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="e2378-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2378-133">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e2378-133">Name the folder *Models*.</span></span>

<span data-ttu-id="e2378-134">注意：模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2378-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e2378-135">新增 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="e2378-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="e2378-136">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="e2378-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2378-137">將類別命名為 `TodoItem`，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e2378-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="e2378-138">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e2378-138">Replace the generated code with the following:</span></span>

<span data-ttu-id="e2378-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="e2378-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="e2378-140">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="e2378-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="e2378-141">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e2378-141">Create the database context</span></span>

<span data-ttu-id="e2378-142">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="e2378-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e2378-143">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="e2378-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e2378-144">新增 `TodoContext` 類別。</span><span class="sxs-lookup"><span data-stu-id="e2378-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="e2378-145">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="e2378-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2378-146">將類別命名為 `TodoContext`，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e2378-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="e2378-147">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e2378-147">Replace the generated code with the following:</span></span>

<span data-ttu-id="e2378-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="e2378-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="e2378-149">新增控制器</span><span class="sxs-lookup"><span data-stu-id="e2378-149">Add a controller</span></span>

<span data-ttu-id="e2378-150">在方案總管中，以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2378-150">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="e2378-151">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="e2378-151">Select **Add** > **New Item**.</span></span> <span data-ttu-id="e2378-152">在 [新增項目] 對話方塊中，選取 [Web API 控制器類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="e2378-152">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="e2378-153">將類別命名為 `TodoController` 。</span><span class="sxs-lookup"><span data-stu-id="e2378-153">Name the class `TodoController`.</span></span>

![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

<span data-ttu-id="e2378-155">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e2378-155">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="e2378-156">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="e2378-156">Launch the app</span></span>

<span data-ttu-id="e2378-157">在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e2378-157">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="e2378-158">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port/api/values`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="e2378-158">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="e2378-159">如果您使用的是 Chrome、Edge 或 Firefox，就會顯示資料。</span><span class="sxs-lookup"><span data-stu-id="e2378-159">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="e2378-160">如果您使用的是 IE，IE 會提示您開啟或儲存 *values.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e2378-160">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="e2378-161">巡覽至剛才建立 `http://localhost:port/api/todo` 的 `Todo` 控制器。</span><span class="sxs-lookup"><span data-stu-id="e2378-161">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

---
title: "建立 ASP.NET Core 應用程式與受保護的授權的使用者資料"
author: rick-anderson
keywords: "ASP.NET Core、 MVC、 授權、 角色、 安全性、 系統管理員"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7ce59816d106cce9fabd8393ee764454bd0f7f27
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="808ab-103">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="808ab-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="808ab-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="808ab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="808ab-105">本教學課程會示範如何建立 web 應用程式與受保護的授權的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="808ab-106">它會顯示的驗證 （登錄） 的使用者的連絡人清單已建立。</span><span class="sxs-lookup"><span data-stu-id="808ab-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="808ab-107">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="808ab-107">There are three security groups:</span></span>

* <span data-ttu-id="808ab-108">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="808ab-109">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="808ab-110">經理可以核准或拒絕連絡資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="808ab-111">只有已核准的連絡人會對使用者顯示。</span><span class="sxs-lookup"><span data-stu-id="808ab-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="808ab-112">系統管理員可以核准/拒絕及編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="808ab-113">在下列影像中，使用者 Rick (`rick@example.com`) 登入。</span><span class="sxs-lookup"><span data-stu-id="808ab-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="808ab-114">使用者 Rick 只能檢視核准連絡人和編輯/刪除他的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="808ab-115">只有最後一筆記錄，建立由 Rick、 顯示編輯和刪除連結</span><span class="sxs-lookup"><span data-stu-id="808ab-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![上面所述的映像](secure-data/_static/rick.png)

<span data-ttu-id="808ab-117">在下圖`manager@contoso.com`已登入，並在管理員角色。</span><span class="sxs-lookup"><span data-stu-id="808ab-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![上面所述的映像](secure-data/_static/manager1.png)

<span data-ttu-id="808ab-119">下圖顯示在管理員的連絡人詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="808ab-119">The following image shows the  managers details view of a contact.</span></span>

![上面所述的映像](secure-data/_static/manager.png)

<span data-ttu-id="808ab-121">只有管理員和系統管理員已核准和拒絕按鈕。</span><span class="sxs-lookup"><span data-stu-id="808ab-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="808ab-122">在下圖`admin@contoso.com`已登入，並在系統管理員的角色。</span><span class="sxs-lookup"><span data-stu-id="808ab-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![上面所述的映像](secure-data/_static/admin.png)

<span data-ttu-id="808ab-124">系統管理員將擁有所有權限。</span><span class="sxs-lookup"><span data-stu-id="808ab-124">The administrator has all privileges.</span></span> <span data-ttu-id="808ab-125">她可以讀取/編輯/刪除任何連絡人，變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="808ab-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="808ab-126">應用程式所建立[scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="808ab-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="808ab-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="808ab-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="808ab-128">A`ContactIsOwnerAuthorizationHandler`授權的處理常式可確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="808ab-129">A`ContactManagerAuthorizationHandler`授權的處理常式可讓管理員核准或拒絕的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="808ab-130">A`ContactAdministratorsAuthorizationHandler`授權的處理常式可讓系統管理員核准或拒絕的連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="808ab-131">必要條件</span><span class="sxs-lookup"><span data-stu-id="808ab-131">Prerequisites</span></span>

<span data-ttu-id="808ab-132">這不是開始教學課程。</span><span class="sxs-lookup"><span data-stu-id="808ab-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="808ab-133">您應該熟悉：</span><span class="sxs-lookup"><span data-stu-id="808ab-133">You should be familiar with:</span></span>

* [<span data-ttu-id="808ab-134">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="808ab-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="808ab-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="808ab-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="808ab-136">起始和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="808ab-136">The starter and completed app</span></span>

<span data-ttu-id="808ab-137">[下載](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final)應用程式。</span><span class="sxs-lookup"><span data-stu-id="808ab-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="808ab-138">[測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="808ab-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="808ab-139">起始應用程式</span><span class="sxs-lookup"><span data-stu-id="808ab-139">The starter app</span></span>

<span data-ttu-id="808ab-140">最好先比較已完成的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="808ab-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="808ab-141">[下載](xref:tutorials/index#how-to-download-a-sample)[入門](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter)應用程式。</span><span class="sxs-lookup"><span data-stu-id="808ab-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="808ab-142">請參閱[建立入門應用程式](#create-the-starter-app)如果您想要從頭開始建立它。</span><span class="sxs-lookup"><span data-stu-id="808ab-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="808ab-143">更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="808ab-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="808ab-144">執行應用程式，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="808ab-145">本教學課程已建立安全的使用者資料的應用程式的主要步驟。</span><span class="sxs-lookup"><span data-stu-id="808ab-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="808ab-146">您可能會發現很有幫助完成的專案參考。</span><span class="sxs-lookup"><span data-stu-id="808ab-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="808ab-147">修改應用程式，以保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="808ab-147">Modify the app to secure user data</span></span>

<span data-ttu-id="808ab-148">下列各節將有建立安全的使用者資料的應用程式的主要步驟。</span><span class="sxs-lookup"><span data-stu-id="808ab-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="808ab-149">您可能會發現很有幫助完成的專案參考。</span><span class="sxs-lookup"><span data-stu-id="808ab-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="808ab-150">將繫結到使用者的連絡資料</span><span class="sxs-lookup"><span data-stu-id="808ab-150">Tie the contact data to the user</span></span>

<span data-ttu-id="808ab-151">使用 ASP.NET[識別](xref:security/authentication/identity)的使用者識別碼以確保使用者可以編輯其資料，但沒有其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="808ab-152">新增`OwnerID`至`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="808ab-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="808ab-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="808ab-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="808ab-154">`OwnerID`這是使用者的識別碼，從`AspNetUser`資料表中[識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="808ab-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="808ab-155">`Status`欄位可讓您判斷是否為一般使用者可以檢視連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="808ab-156">為新的移轉建立結構，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="808ab-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="808ab-157">需要 SSL 和已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="808ab-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="808ab-158">在`ConfigureServices`方法*Startup.cs* file、 add [RequireHttpsAttribute](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/RequireHttpsAttribute/index.html.md#Microsoft.AspNetCore.Mvc.RequireHttpsAttribute.md)授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="808ab-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/RequireHttpsAttribute/index.html.md#Microsoft.AspNetCore.Mvc.RequireHttpsAttribute.md) authorization filter:</span></span>

<span data-ttu-id="808ab-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="808ab-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="808ab-160">如果您使用 Visual Studio，請參閱[設定 SSL/HTTPS 的 IIS Express](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps)。</span><span class="sxs-lookup"><span data-stu-id="808ab-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="808ab-161">若要將 HTTP 要求重新導向至 HTTPS，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="808ab-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="808ab-162">如果您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，針對 SSL:</span><span class="sxs-lookup"><span data-stu-id="808ab-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="808ab-163">設定`"LocalTest:skipSSL": true`中*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="808ab-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="808ab-164">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="808ab-164">Require authenticated users</span></span>

<span data-ttu-id="808ab-165">設定為需要驗證使用者的預設驗證原則。</span><span class="sxs-lookup"><span data-stu-id="808ab-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="808ab-166">您可以選擇不在控制器或動作方法以驗證`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="808ab-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="808ab-167">使用這個方法，加入任何新的控制站會自動需要驗證，也就是比信賴憑證者上包含新的控制站安全`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="808ab-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="808ab-168">將下列內容加入`ConfigureServices`方法*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="808ab-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="808ab-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="808ab-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="808ab-170">新增`[AllowAnonymous]`至主控制器，因此他們註冊之前，匿名使用者可以取得站台的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="808ab-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="808ab-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="808ab-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="808ab-172">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="808ab-172">Configure the test account</span></span>

<span data-ttu-id="808ab-173">`SeedData`類別會建立兩個帳戶，系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="808ab-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="808ab-174">使用[密碼管理員工具](xref:security/app-secrets)設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="808ab-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="808ab-175">從專案目錄 (directory 包含*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="808ab-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="808ab-176">更新`Configure`使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="808ab-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="808ab-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="808ab-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="808ab-178">新增系統管理員使用者識別碼和`Status = ContactStatus.Approved`至連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="808ab-179">只能有一個連絡人會顯示，可加入的使用者識別碼的所有連絡人：</span><span class="sxs-lookup"><span data-stu-id="808ab-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="808ab-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="808ab-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="808ab-181">建立擁有者、 管理員和系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="808ab-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="808ab-182">建立`ContactIsOwnerAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="808ab-183">`ContactIsOwnerAuthorizationHandler`會確認作用於資源的使用者擁有的資源。</span><span class="sxs-lookup"><span data-stu-id="808ab-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="808ab-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="808ab-185">`ContactIsOwnerAuthorizationHandler`呼叫`context.Succeed`如果目前已驗證的使用者是連絡人的擁有者。</span><span class="sxs-lookup"><span data-stu-id="808ab-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="808ab-186">授權的處理常式通常會傳回`context.Succeed`有符合的需求。</span><span class="sxs-lookup"><span data-stu-id="808ab-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="808ab-187">它們會傳回`Task.FromResult(0)`時不符合需求。</span><span class="sxs-lookup"><span data-stu-id="808ab-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="808ab-188">`Task.FromResult(0)`不是成功或失敗，它可讓執行其他授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="808ab-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="808ab-189">如果您需要明確地使失敗，傳回`context.Fail()`。</span><span class="sxs-lookup"><span data-stu-id="808ab-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="808ab-190">我們可以連絡的擁有者可以編輯/刪除自己的資料，所以我們不需要檢查需求參數中的作業。</span><span class="sxs-lookup"><span data-stu-id="808ab-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="808ab-191">建立授權管理員的處理常式</span><span class="sxs-lookup"><span data-stu-id="808ab-191">Create a manager authorization handler</span></span>

<span data-ttu-id="808ab-192">建立`ContactManagerAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="808ab-193">`ContactManagerAuthorizationHandler`會作用於資源的使用者是管理員確認。</span><span class="sxs-lookup"><span data-stu-id="808ab-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="808ab-194">只有經理可以核准或拒絕內容變更 （新增或變更）。</span><span class="sxs-lookup"><span data-stu-id="808ab-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="808ab-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="808ab-196">建立的系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="808ab-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="808ab-197">建立`ContactAdministratorsAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="808ab-198">`ContactAdministratorsAuthorizationHandler`會確認使用者資源上做為系統管理員。</span><span class="sxs-lookup"><span data-stu-id="808ab-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="808ab-199">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="808ab-199">Administrator can do all operations.</span></span>

<span data-ttu-id="808ab-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="808ab-201">註冊授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="808ab-201">Register the authorization handlers</span></span>

<span data-ttu-id="808ab-202">使用 Entity Framework 的核心服務必須登錄[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/Extensions/DependencyInjection/ServiceCollectionServiceExtensions/index.html.md#Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped.md)。</span><span class="sxs-lookup"><span data-stu-id="808ab-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/Extensions/DependencyInjection/ServiceCollectionServiceExtensions/index.html.md#Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped.md).</span></span> <span data-ttu-id="808ab-203">`ContactIsOwnerAuthorizationHandler`使用 ASP.NET Core[識別](xref:security/authentication/identity)，這建置在 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="808ab-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="808ab-204">登錄處理常式與服務的集合，以便將可用的它們`ContactsController`透過[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="808ab-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="808ab-205">將下列程式碼加入至結尾`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="808ab-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="808ab-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="808ab-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="808ab-207">`ContactAdministratorsAuthorizationHandler`和`ContactManagerAuthorizationHandler`會新增為 singleton。</span><span class="sxs-lookup"><span data-stu-id="808ab-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="808ab-208">它們是 singleton，因為它們不使用 EF 和所需的資訊位於`Context`參數`HandleRequirementAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="808ab-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="808ab-209">完整`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="808ab-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="808ab-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="808ab-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="808ab-211">更新以支援授權的程式碼</span><span class="sxs-lookup"><span data-stu-id="808ab-211">Update the code to support authorization</span></span>

<span data-ttu-id="808ab-212">在本節中，您會更新控制器和檢視，並加入作業需求類別。</span><span class="sxs-lookup"><span data-stu-id="808ab-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="808ab-213">更新連絡人控制站</span><span class="sxs-lookup"><span data-stu-id="808ab-213">Update the Contacts controller</span></span>

<span data-ttu-id="808ab-214">更新`ContactsController`建構函式：</span><span class="sxs-lookup"><span data-stu-id="808ab-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="808ab-215">新增`IAuthorizationService`服務存取授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="808ab-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="808ab-216">新增`Identity``UserManager`服務：</span><span class="sxs-lookup"><span data-stu-id="808ab-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="808ab-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="808ab-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="808ab-218">加入連絡人的作業需求類別</span><span class="sxs-lookup"><span data-stu-id="808ab-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="808ab-219">新增`ContactOperationsRequirements`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="808ab-220">這個類別包含需求我們的應用程式支援：</span><span class="sxs-lookup"><span data-stu-id="808ab-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="808ab-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="808ab-222">建立更新</span><span class="sxs-lookup"><span data-stu-id="808ab-222">Update Create</span></span>

<span data-ttu-id="808ab-223">更新`HTTP POST Create`方法：</span><span class="sxs-lookup"><span data-stu-id="808ab-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="808ab-224">新增使用者識別碼`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="808ab-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="808ab-225">呼叫以確認該使用者所擁有之連絡人的授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="808ab-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="808ab-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="808ab-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="808ab-227">更新編輯</span><span class="sxs-lookup"><span data-stu-id="808ab-227">Update Edit</span></span>

<span data-ttu-id="808ab-228">更新`Edit`用來驗證使用者的授權的處理常式方法擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="808ab-229">因為我們要執行的資源授權，無法使用`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="808ab-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="808ab-230">要在評估屬性時，我們不需要資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="808ab-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="808ab-231">根據資源授權必須是命令性。</span><span class="sxs-lookup"><span data-stu-id="808ab-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="808ab-232">一旦載入中我們控制站，或是載入在處理常式本身都可存取資源，必須執行檢查。</span><span class="sxs-lookup"><span data-stu-id="808ab-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="808ab-233">通常您會藉由傳遞資源索引鍵存取資源。</span><span class="sxs-lookup"><span data-stu-id="808ab-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="808ab-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="808ab-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="808ab-235">更新 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="808ab-235">Update the Delete method</span></span>

<span data-ttu-id="808ab-236">更新`Delete`用來驗證使用者的授權的處理常式方法擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="808ab-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="808ab-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="808ab-238">授權服務插入檢視</span><span class="sxs-lookup"><span data-stu-id="808ab-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="808ab-239">目前 UI 顯示編輯和刪除使用者無法修改的資料的連結。</span><span class="sxs-lookup"><span data-stu-id="808ab-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="808ab-240">我們將修正，藉由套用授權的處理常式來檢視。</span><span class="sxs-lookup"><span data-stu-id="808ab-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="808ab-241">插入中的授權服務*Views/_ViewImports.cshtml*檔案，因此將予以提供至所有的檢視：</span><span class="sxs-lookup"><span data-stu-id="808ab-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="808ab-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="808ab-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="808ab-243">更新*Views/Contacts/Index.cshtml* Razor 檢視，只顯示編輯，並刪除連結的使用者可以編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="808ab-244">新增`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="808ab-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="808ab-245">更新`Edit`和`Delete`連結，讓它們只呈現為具有權限的使用者編輯或刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="808ab-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="808ab-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="808ab-247">警告： 隱藏連結沒有編輯或刪除資料的權限的使用者不會不安全的應用程式。</span><span class="sxs-lookup"><span data-stu-id="808ab-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="808ab-248">隱藏連結，讓應用程式更多的使用者易記顯示唯一有效的連結。</span><span class="sxs-lookup"><span data-stu-id="808ab-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="808ab-249">使用者可以 hack 叫用 編輯和刪除作業沒有自己的資料產生的 Url。</span><span class="sxs-lookup"><span data-stu-id="808ab-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="808ab-250">控制器必須是安全的存取檢查重複的。</span><span class="sxs-lookup"><span data-stu-id="808ab-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="808ab-251">更新詳細資料檢視</span><span class="sxs-lookup"><span data-stu-id="808ab-251">Update the Details view</span></span>

<span data-ttu-id="808ab-252">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="808ab-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="808ab-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="808ab-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="808ab-254">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="808ab-254">Test the completed app</span></span>

<span data-ttu-id="808ab-255">如果您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，針對 SSL:</span><span class="sxs-lookup"><span data-stu-id="808ab-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="808ab-256">設定`"LocalTest:skipSSL": true`中*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="808ab-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="808ab-257">如果您有執行應用程式，並以連絡人，刪除所有記錄的`Contact`資料表，並重新啟動植入資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="808ab-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="808ab-258">如果您使用 Visual Studio，您要結束並重新啟動 IIS Express 種子資料庫。</span><span class="sxs-lookup"><span data-stu-id="808ab-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="808ab-259">註冊使用者瀏覽的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="808ab-260">若要測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 版本）。</span><span class="sxs-lookup"><span data-stu-id="808ab-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="808ab-261">在一個瀏覽器中註冊新的使用者，例如`test@example.com`。</span><span class="sxs-lookup"><span data-stu-id="808ab-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="808ab-262">以不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="808ab-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="808ab-263">驗證下列各項：</span><span class="sxs-lookup"><span data-stu-id="808ab-263">Verify the following:</span></span>

* <span data-ttu-id="808ab-264">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="808ab-265">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="808ab-266">經理可以核准或拒絕連絡資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="808ab-267">`Details`檢視會顯示**核准**和**拒絕**按鈕。</span><span class="sxs-lookup"><span data-stu-id="808ab-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="808ab-268">系統管理員可以核准/拒絕及編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="808ab-269">使用者</span><span class="sxs-lookup"><span data-stu-id="808ab-269">User</span></span>| <span data-ttu-id="808ab-270">選項</span><span class="sxs-lookup"><span data-stu-id="808ab-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="808ab-271">可以編輯/刪除自己的資料</span><span class="sxs-lookup"><span data-stu-id="808ab-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="808ab-272">可以核准/拒絕和編輯/刪除擁有資料</span><span class="sxs-lookup"><span data-stu-id="808ab-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="808ab-273">可以編輯/刪除並核准/拒絕的所有資料</span><span class="sxs-lookup"><span data-stu-id="808ab-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="808ab-274">系統管理員的瀏覽器中建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="808ab-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="808ab-275">複製的 URL 刪除和編輯從系統管理員連絡。</span><span class="sxs-lookup"><span data-stu-id="808ab-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="808ab-276">這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="808ab-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="808ab-277">建立起始應用程式</span><span class="sxs-lookup"><span data-stu-id="808ab-277">Create the starter app</span></span>

<span data-ttu-id="808ab-278">請遵循這些指示來建立起始應用程式。</span><span class="sxs-lookup"><span data-stu-id="808ab-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="808ab-279">建立**ASP.NET Core Web 應用程式**使用[Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)名為"ContactManager"</span><span class="sxs-lookup"><span data-stu-id="808ab-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) named "ContactManager"</span></span>

  * <span data-ttu-id="808ab-280">建立應用程式與**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="808ab-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="808ab-281">將類別命名"ContactManager 」 讓您的命名空間會比對命名空間中的使用範例。</span><span class="sxs-lookup"><span data-stu-id="808ab-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="808ab-282">加入下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="808ab-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="808ab-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="808ab-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="808ab-284">Scaffold`Contact`模型使用 Entity Framework Core 和`ApplicationDbContext`資料內容。</span><span class="sxs-lookup"><span data-stu-id="808ab-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="808ab-285">接受所有 scaffolding 預設值。</span><span class="sxs-lookup"><span data-stu-id="808ab-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="808ab-286">使用`ApplicationDbContext`資料內容類別將 contact 資料表放入[識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="808ab-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="808ab-287">請參閱[加入模型](xref:tutorials/first-mvc-app/adding-model)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="808ab-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="808ab-288">更新**ContactManager**錨定在*Views/Shared/_Layout.cshtml*檔案從`asp-controller="Home"`至`asp-controller="Contacts"`因此點選**ContactManager**連結會叫用的連絡人控制站。</span><span class="sxs-lookup"><span data-stu-id="808ab-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="808ab-289">原始的標記：</span><span class="sxs-lookup"><span data-stu-id="808ab-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="808ab-290">更新的標記：</span><span class="sxs-lookup"><span data-stu-id="808ab-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="808ab-291">為初始移轉建立結構，並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="808ab-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="808ab-292">測試應用程式所建立、 編輯和刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="808ab-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="808ab-293">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="808ab-293">Seed the database</span></span>

<span data-ttu-id="808ab-294">新增`SeedData`類別*資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="808ab-295">如果您已經下載範例，您可以複製*SeedData.cs*檔案*資料*入門專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="808ab-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="808ab-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="808ab-297">將反白顯示的程式碼的結尾加入`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="808ab-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="808ab-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="808ab-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="808ab-299">測試應用程式植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="808ab-299">Test that the app seeded the database.</span></span> <span data-ttu-id="808ab-300">如果連絡人 DB 中沒有任何資料列種子方法不會執行。</span><span class="sxs-lookup"><span data-stu-id="808ab-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="808ab-301">建立本教學課程中使用的類別</span><span class="sxs-lookup"><span data-stu-id="808ab-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="808ab-302">建立名為的資料夾*授權*。</span><span class="sxs-lookup"><span data-stu-id="808ab-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="808ab-303">複製*Authorization\ContactOperations.cs*檔案從已完成的專案下載，或複製下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="808ab-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="808ab-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="808ab-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="808ab-305">其他資源</span><span class="sxs-lookup"><span data-stu-id="808ab-305">Additional resources</span></span>

* <span data-ttu-id="808ab-306">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="808ab-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="808ab-307">這個實驗室會進入這個教學課程中介紹的安全性功能的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="808ab-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="808ab-308">在 ASP.NET Core 授權： 簡單、 宣告式和自訂的角色</span><span class="sxs-lookup"><span data-stu-id="808ab-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="808ab-309">以原則為基礎的自訂授權</span><span class="sxs-lookup"><span data-stu-id="808ab-309">Custom Policy-Based Authorization</span></span>](policies.md)
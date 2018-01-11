---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "設定部署屬性的目標環境 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何設定環境特有的屬性，以將範例連絡人管理員方案部署至特定目標環境..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a><span data-ttu-id="7000f-103">設定的目標環境的部署屬性</span><span class="sxs-lookup"><span data-stu-id="7000f-103">Configuring Deployment Properties for a Target Environment</span></span>
====================
<span data-ttu-id="7000f-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7000f-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7000f-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="7000f-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7000f-106">本主題描述如何設定環境特有的屬性，以將範例連絡人管理員方案部署至特定目標環境。</span><span class="sxs-lookup"><span data-stu-id="7000f-106">This topic describes how to configure environment-specific properties in order to deploy the sample Contact Manager solution to a specific target environment.</span></span>


<span data-ttu-id="7000f-107">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案 & #x 2014; 代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式Communication Foundation (WCF) 服務，與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="7000f-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="7000f-108">這些教學課程的核心的部署方法為基礎分割專案檔案描述的方法中[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="7000f-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="7000f-109">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="7000f-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="7000f-110">處理程序概觀</span><span class="sxs-lookup"><span data-stu-id="7000f-110">Process Overview</span></span>

<span data-ttu-id="7000f-111">您要用以建置和部署連絡人管理員方案的專案檔會分成兩個實體檔案：</span><span class="sxs-lookup"><span data-stu-id="7000f-111">The project file that you'll use to build and deploy the Contact Manager solution is split into two physical files:</span></span>

- <span data-ttu-id="7000f-112">另一個則包含通用建置設定和指示 ( *Publish.proj*檔案)。</span><span class="sxs-lookup"><span data-stu-id="7000f-112">One that contains universal build settings and instructions (the *Publish.proj* file).</span></span>
- <span data-ttu-id="7000f-113">另一個則包含特定環境的建置設定 (*Env Dev.proj*， *Env Stage.proj*等等)。</span><span class="sxs-lookup"><span data-stu-id="7000f-113">One that contains environment-specific build settings (*Env-Dev.proj*, *Env-Stage.proj*, and so on).</span></span>

<span data-ttu-id="7000f-114">在建置時，適當的環境特定專案檔會合併到通用*Publish.proj*形成一組完整的組建指令檔。</span><span class="sxs-lookup"><span data-stu-id="7000f-114">At build time, the appropriate environment-specific project file is merged into the universal *Publish.proj* file to form a complete set of build instructions.</span></span> <span data-ttu-id="7000f-115">您可以藉由建立或自訂特定環境的專案檔，以描述您自己的部署案例的設定來設定部署到特定目的地環境。</span><span class="sxs-lookup"><span data-stu-id="7000f-115">You can configure deployment to specific destination environments by creating or customizing environment-specific project files with settings that describe your own deployment scenario.</span></span>

<span data-ttu-id="7000f-116">許多這些值由目的地環境的設定方式 & #x 2014年; 特別是，是否目標 web 伺服器設定為使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="7000f-116">Lots of these values are determined by how your destination environment is configured&#x2014;in particular, whether your target web server is configured to use the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler.</span></span> <span data-ttu-id="7000f-117">如需有關這些方法，以及選擇您自己的環境的正確方法的指引，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-117">For more information on these approaches, and for guidance on choosing the right approach for your own environment, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>

<span data-ttu-id="7000f-118">[連絡人管理員案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)需要兩個環境特定專案檔案：</span><span class="sxs-lookup"><span data-stu-id="7000f-118">The [Contact Manager scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) requires two environment-specific project files:</span></span>

- <span data-ttu-id="7000f-119">開發人員的測試環境的部署 (*Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="7000f-119">Deployment to a developer test environment (*Env-Dev.proj*).</span></span> <span data-ttu-id="7000f-120">開發人員測試環境設定為接受遠端使用遠端代理程式的部署中所述[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-120">The developer test environment is configured to accept remote deployments using the remote agent, as described in [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="7000f-121">這個檔案必須提供遠端代理程式，端點位址，以及特定位置的設定，例如連接字串和服務端點。</span><span class="sxs-lookup"><span data-stu-id="7000f-121">This file needs to provide the remote agent endpoint address as well as location-specific settings like connection strings and service endpoints.</span></span>
- <span data-ttu-id="7000f-122">部署到預備環境 (*Env Stage.proj*)。</span><span class="sxs-lookup"><span data-stu-id="7000f-122">Deployment to a staging environment (*Env-Stage.proj*).</span></span> <span data-ttu-id="7000f-123">預備環境設定為接受使用 Web 部署處理常式的遠端部署中所述[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-123">The staging environment is configured to accept remote deployments using the Web Deploy Handler, as described in [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="7000f-124">提供 Web 部署的處理常式的端點位址以及特定位置的設定，例如連接字串和服務端點需要此檔案。</span><span class="sxs-lookup"><span data-stu-id="7000f-124">This file needs to provide the Web Deploy Handler endpoint address as well as location-specific settings like connection strings and service endpoints.</span></span>

<span data-ttu-id="7000f-125">請務必注意您在特定環境的專案檔中設定的設定不會影響網站的內容會封裝本身 & #x 2014年; 相反地，控制如何部署封裝和提供哪些參數值時擷取的封裝。</span><span class="sxs-lookup"><span data-stu-id="7000f-125">It's important to note that the settings you configure in the environment-specific project file don't affect the contents of the web package itself&#x2014;instead, they control how the package is deployed and what parameter values are provided when the package is extracted.</span></span> <span data-ttu-id="7000f-126">您正在 web 封裝，述手動匯入實際執行環境[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)和[手動安裝 Web 封裝](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)，因此它並不重要了哪些設定您在特定環境的專案檔時使用產生封裝。</span><span class="sxs-lookup"><span data-stu-id="7000f-126">You're importing the web package into the production environment manually, as described in [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md) and [Manually Installing Web Packages](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), so it doesn't matter what settings you used in the environment-specific project file when you generated the package.</span></span> <span data-ttu-id="7000f-127">網際網路資訊服務 (IIS) 管理員會提示您輸入任何參數化的值，例如連接字串和服務端點，當您匯入封裝。</span><span class="sxs-lookup"><span data-stu-id="7000f-127">Internet Information Services (IIS) Manager will prompt you for any parameterized values, like connection strings and service endpoints, when you import the package.</span></span>

<span data-ttu-id="7000f-128">若要將連絡人管理員方案部署到目標環境，您可以自訂此檔案或使用做為範本並建立您自己的檔案。</span><span class="sxs-lookup"><span data-stu-id="7000f-128">To deploy the Contact Manager solution to your own target environment, you can either customize this file or use it as a template and create your own file.</span></span>

<span data-ttu-id="7000f-129">**設定連絡人管理員解決方案的環境特定部署設定**</span><span class="sxs-lookup"><span data-stu-id="7000f-129">**To configure environment-specific deployment settings for the Contact Manager solution**</span></span>

1. <span data-ttu-id="7000f-130">Visual Studio 2010 中開啟 ContactManager WCF 方案。</span><span class="sxs-lookup"><span data-stu-id="7000f-130">Open the ContactManager-WCF solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="7000f-131">在**方案總管] 中**視窗中，展開 [**發行**資料夾中，展開**EnvConfig**資料夾，然後再按兩下**Env Dev.proj**.</span><span class="sxs-lookup"><span data-stu-id="7000f-131">In the **Solution Explorer** window, expand the **Publish** folder, expand the **EnvConfig** folder, and then double-click **Env-Dev.proj**.</span></span>

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. <span data-ttu-id="7000f-132">中的屬性值取代*Env Dev.proj*以正確的值用於測試環境中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7000f-132">Replace the property values in the *Env-Dev.proj* file with the correct values for your own test environment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7000f-133">遵循此程序的資料表會提供有關每一個屬性的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7000f-133">The table that follows this procedure provides more information on each of these properties.</span></span>
4. <span data-ttu-id="7000f-134">儲存您的工作，然後關閉*Env Dev.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="7000f-134">Save your work, and then close the *Env-Dev.proj* file.</span></span>

## <a name="choosing-the-right-deployment-properties"></a><span data-ttu-id="7000f-135">選擇正確的部署屬性</span><span class="sxs-lookup"><span data-stu-id="7000f-135">Choosing the Right Deployment Properties</span></span>

<span data-ttu-id="7000f-136">下表描述的範例環境特定專案檔中的每個屬性的目的*Env Dev.proj*，並提供一些指引您應該提供的值。</span><span class="sxs-lookup"><span data-stu-id="7000f-136">This table describes the purpose of each property in the sample environment-specific project file, *Env-Dev.proj*, and provides some guidance on the values you should provide.</span></span>

| <span data-ttu-id="7000f-137">屬性名稱</span><span class="sxs-lookup"><span data-stu-id="7000f-137">Property Name</span></span> | <span data-ttu-id="7000f-138">詳細資料</span><span class="sxs-lookup"><span data-stu-id="7000f-138">Details</span></span> |
| --- | --- |
| <span data-ttu-id="7000f-139">**MSDeployComputerName**目的地 web 伺服器或服務端點的名稱。</span><span class="sxs-lookup"><span data-stu-id="7000f-139">**MSDeployComputerName** The name of the destination web server or service endpoint.</span></span> | <span data-ttu-id="7000f-140">如果您要部署至目的地 web 伺服器上的遠端代理程式服務，您可以指定目標電腦名稱 (例如， **TESTWEB1**或**TESTWEB1.fabrikam.net**)，或者您可以指定遠端代理程式的端點 (例如， `http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。</span><span class="sxs-lookup"><span data-stu-id="7000f-140">If you're deploying to the remote agent service on the destination web server, you can specify the target computer name (for example, **TESTWEB1** or **TESTWEB1.fabrikam.net**), or you can specify the remote agent endpoint (for example, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`).</span></span> <span data-ttu-id="7000f-141">部署每個案例中的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="7000f-141">The deployment works the same way in each case.</span></span> <span data-ttu-id="7000f-142">如果您要部署至 Web 部署處理常式目的地 web 伺服器上，您應該指定服務端點，並包含的 IIS 網站做為查詢字串參數名稱 (例如， `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。</span><span class="sxs-lookup"><span data-stu-id="7000f-142">If you're deploying to the Web Deploy Handler on the destination web server, you should specify the service endpoint and include the name of the IIS website as a query string parameter (for example, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).</span></span> |
| <span data-ttu-id="7000f-143">**MSDeployAuth** Web Deploy 應該用來驗證遠端電腦的方法。</span><span class="sxs-lookup"><span data-stu-id="7000f-143">**MSDeployAuth** The method that Web Deploy should use to authenticate to the remote computer.</span></span> | <span data-ttu-id="7000f-144">這應該設定為**NTLM**或**基本**。</span><span class="sxs-lookup"><span data-stu-id="7000f-144">This should be set to **NTLM** or **Basic**.</span></span> <span data-ttu-id="7000f-145">通常，您會使用**NTLM**如果您要部署至遠端代理程式服務和**基本**如果您要部署至 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="7000f-145">Typically, you'll use **NTLM** if you're deploying to the remote agent service and **Basic** if you're deploying to the Web Deploy Handler.</span></span> <span data-ttu-id="7000f-146">如果您使用基本驗證，您也需要指定使用者名稱和密碼，才能執行這種部署應該模擬的 IIS Web Deployment Tool (Web Deploy)。</span><span class="sxs-lookup"><span data-stu-id="7000f-146">If you use basic authentication, you also need to specify the user name and password that the IIS Web Deployment Tool (Web Deploy) should impersonate in order to perform the deployment.</span></span> <span data-ttu-id="7000f-147">在此範例中，這些值透過提供**MSDeployUsername**和**MSDeployPassword**屬性。</span><span class="sxs-lookup"><span data-stu-id="7000f-147">In this example, these values are provided through the **MSDeployUsername** and **MSDeployPassword** properties.</span></span> <span data-ttu-id="7000f-148">如果您使用 NTLM 驗證，您可以省略這些屬性，或將其保留空白。</span><span class="sxs-lookup"><span data-stu-id="7000f-148">If you use NTLM authentication, you can omit these properties or leave them blank.</span></span> |
| <span data-ttu-id="7000f-149">**MSDeployUsername**如果您使用基本驗證時，Web Deploy 會使用此帳戶在遠端電腦上。</span><span class="sxs-lookup"><span data-stu-id="7000f-149">**MSDeployUsername** If you use basic authentication, Web Deploy will use this account on the remote computer.</span></span> | <span data-ttu-id="7000f-150">此格式應為*網域*\*username * (例如， **FABRIKAM\matt**)。</span><span class="sxs-lookup"><span data-stu-id="7000f-150">This should take the form *DOMAIN*\*username* (for example, **FABRIKAM\matt**).</span></span> <span data-ttu-id="7000f-151">如果您指定基本驗證，才使用這個值。</span><span class="sxs-lookup"><span data-stu-id="7000f-151">This value is only used if you specify basic authentication.</span></span> <span data-ttu-id="7000f-152">如果您使用 NTLM 驗證，則可省略的屬性。</span><span class="sxs-lookup"><span data-stu-id="7000f-152">If you use NTLM authentication, the property can be omitted.</span></span> <span data-ttu-id="7000f-153">如果提供值，將會忽略它。</span><span class="sxs-lookup"><span data-stu-id="7000f-153">If a value is supplied, it will be ignored.</span></span> |
| <span data-ttu-id="7000f-154">**MSDeployPassword**如果您使用基本驗證時，Web Deploy 將使用此密碼在遠端電腦上。</span><span class="sxs-lookup"><span data-stu-id="7000f-154">**MSDeployPassword** If you use basic authentication, Web Deploy will use this password on the remote computer.</span></span> | <span data-ttu-id="7000f-155">這是您在中指定的使用者帳戶的密碼**MSDeployUsername**屬性。</span><span class="sxs-lookup"><span data-stu-id="7000f-155">This is the password for the user account you specified in the **MSDeployUsername** property.</span></span> <span data-ttu-id="7000f-156">如果您指定基本驗證，才使用這個值。</span><span class="sxs-lookup"><span data-stu-id="7000f-156">This value is only used if you specify basic authentication.</span></span> <span data-ttu-id="7000f-157">如果您使用 NTLM 驗證，則可省略的屬性。</span><span class="sxs-lookup"><span data-stu-id="7000f-157">If you use NTLM authentication, the property can be omitted.</span></span> <span data-ttu-id="7000f-158">如果提供值，將會忽略它。</span><span class="sxs-lookup"><span data-stu-id="7000f-158">If a value is supplied, it will be ignored.</span></span> |
| <span data-ttu-id="7000f-159">**ContactManagerIisPath**您要部署連絡人管理員 MVC 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="7000f-159">**ContactManagerIisPath** The IIS path on which you want to deploy the Contact Manager MVC application.</span></span> | <span data-ttu-id="7000f-160">這應該是路徑，因為它出現在 [IIS 管理員] 中，在表單中 [*IIS 網站名稱*] / [*web**應用程式名稱*]。</span><span class="sxs-lookup"><span data-stu-id="7000f-160">This should be the path as it appears in IIS Manager, in the form [*IIS website name*]/[*web**application name*].</span></span> <span data-ttu-id="7000f-161">請記住，IIS 網站必須存在才能部署您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7000f-161">Remember that the IIS website needs to exist before you deploy your application.</span></span> <span data-ttu-id="7000f-162">比方說，如果您已建立名為 DemoSite 的 IIS 網站，您可以為 DemoSite/ContactManager 指定 MVC 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="7000f-162">For example, if you've created an IIS website named DemoSite, you could specify the IIS path for the MVC application as DemoSite/ContactManager.</span></span> |
| <span data-ttu-id="7000f-163">**ContactManagerServiceIisPath**您要部署連絡人管理員 WCF 服務的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="7000f-163">**ContactManagerServiceIisPath** The IIS path on which you want to deploy the Contact Manager WCF service.</span></span> | <span data-ttu-id="7000f-164">例如，如果您已建立名為 DemoSite 的 IIS 網站，您可以指定做為 WCF 服務的 IIS 路徑**DemoSite/ContactManagerService**。</span><span class="sxs-lookup"><span data-stu-id="7000f-164">For example, if you've created an IIS website named DemoSite, you could specify the IIS path for the WCF service as **DemoSite/ContactManagerService**.</span></span> |
| <span data-ttu-id="7000f-165">**ContactManagerTargetUrl**可以到達 WCF 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="7000f-165">**ContactManagerTargetUrl** The URL at which the WCF service can be reached.</span></span> | <span data-ttu-id="7000f-166">這需要表單 [*IIS 網站的根 URL*] / [*服務應用程式名稱*] / [*服務端點*]。</span><span class="sxs-lookup"><span data-stu-id="7000f-166">This will take the form [*IIS website root URL*]/[*service application name*]/[*service endpoint*].</span></span> <span data-ttu-id="7000f-167">例如，如果您已建立 IIS 網站在連接埠 85，URL 會的形式`http://localhost:85/ContactManagerService/ContactService.svc`。</span><span class="sxs-lookup"><span data-stu-id="7000f-167">For example, if you've created an IIS website on port 85, the URL would take the form `http://localhost:85/ContactManagerService/ContactService.svc`.</span></span> <span data-ttu-id="7000f-168">請記住，在 MVC 應用程式和 WCF 服務會部署至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7000f-168">Remember that the MVC application and the WCF service are deployed to the same server.</span></span> <span data-ttu-id="7000f-169">如此一來，從電腦安裝僅存取此 URL。</span><span class="sxs-lookup"><span data-stu-id="7000f-169">As a result, this URL is only ever accessed from the machine on which it's installed.</span></span> <span data-ttu-id="7000f-170">因此，最好是在 URL 中使用 localhost 或 IP 位址，而非電腦名稱或主機標頭。</span><span class="sxs-lookup"><span data-stu-id="7000f-170">Because of this, it's better to use localhost or the IP address, rather than the machine name or a host header, in the URL.</span></span> <span data-ttu-id="7000f-171">如果您使用的電腦名稱或主機標頭，[回送核取](https://go.microsoft.com/?linkid=9805131)在 IIS 中的安全性功能可能會封鎖的 URL，並傳回**HTTP 401.1-未經授權**錯誤。</span><span class="sxs-lookup"><span data-stu-id="7000f-171">If you use the machine name or a host header, the [loopback check](https://go.microsoft.com/?linkid=9805131) security feature in IIS may block the URL and return an **HTTP 401.1 - Unauthorized** error.</span></span> |
| <span data-ttu-id="7000f-172">**CmDatabaseConnectionString**資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="7000f-172">**CmDatabaseConnectionString** The connection string for the database server.</span></span> | <span data-ttu-id="7000f-173">連接字串會判斷這兩種認證 VSDBCMD 將用來連線到資料庫伺服器和建立資料庫和 web 伺服器應用程式集區會使用連線到資料庫伺服器，與資料庫互動的認證。</span><span class="sxs-lookup"><span data-stu-id="7000f-173">The connection string determines both the credentials that VSDBCMD will use to contact the database server and create the database and the credentials that the web server application pool will use to contact the database server and interact with the database.</span></span> <span data-ttu-id="7000f-174">基本上有兩個選擇這裡。</span><span class="sxs-lookup"><span data-stu-id="7000f-174">Essentially you have two choices here.</span></span> <span data-ttu-id="7000f-175">您可以指定**整合式安全性 = true**，在此情況下使用整合式的 Windows 驗證：**資料來源 = TESTDB1; Integrated Security = true**在此情況下，將會建立此資料庫使用執行可執行檔 VSDBCMD 的使用者和應用程式的認證會存取 web 伺服器電腦帳戶的身分識別的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7000f-175">You can specify **Integrated Security=true**, in which case integrated Windows authentication is used: **Data Source=TESTDB1;Integrated Security=true** In this case, the database will be created using the credentials of the user who runs the VSDBCMD executable, and the application will access the database using the identity of the web server machine account.</span></span> <span data-ttu-id="7000f-176">或者，您可以指定使用者名稱和 SQL Server 帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="7000f-176">Alternatively, you can specify the user name and password of a SQL Server account.</span></span> <span data-ttu-id="7000f-177">在此情況下，SQL Server 認證可用來建立資料庫 VSDBCMD 和應用程式集區與資料庫互動：**資料來源 = TESTDB1;使用者 Id = ASqlUser;密碼 = Pa$ $w0rd**本主題中的逐步解說假設您將使用整合式的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="7000f-177">In this case, the SQL Server credentials are used both by VSDBCMD to create the database and by the application pool to interact with the database: **Data Source=TESTDB1;User Id=ASqlUser; Password=Pa$$w0rd** The walkthroughs in this topic assume that you'll use integrated Windows authentication.</span></span> |
| <span data-ttu-id="7000f-178">**CmTargetDatabase**您想要提供您將資料庫伺服器建立該資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="7000f-178">**CmTargetDatabase** The name you want to give the database you'll create on the database server.</span></span> | <span data-ttu-id="7000f-179">您在此處提供的值加入 VSDBCMD 命令中，做為參數。</span><span class="sxs-lookup"><span data-stu-id="7000f-179">The value you provide here is added to the VSDBCMD command as a parameter.</span></span> <span data-ttu-id="7000f-180">它也用來建置 web 伺服器上的應用程式集區可用來與資料庫互動的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="7000f-180">It's also used to build a full connection string that the application pool on the web server can use to interact with the database.</span></span> |
  

<span data-ttu-id="7000f-181">這些範例說明如何設定這些屬性的特定部署案例。</span><span class="sxs-lookup"><span data-stu-id="7000f-181">These examples show how you might configure these properties for specific deployment scenarios.</span></span>

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a><span data-ttu-id="7000f-182">範例 1 & #x 2014年; 部署至遠端代理程式服務</span><span class="sxs-lookup"><span data-stu-id="7000f-182">Example 1&#x2014;Deployment to the Remote Agent Service</span></span>

<span data-ttu-id="7000f-183">在這個範例中：</span><span class="sxs-lookup"><span data-stu-id="7000f-183">In this example:</span></span>

- <span data-ttu-id="7000f-184">您要部署至遠端代理程式上的服務 TESTWEB1。</span><span class="sxs-lookup"><span data-stu-id="7000f-184">You're deploying to the remote agent service on TESTWEB1.</span></span>
- <span data-ttu-id="7000f-185">您正在指示為使用 NTLM 驗證的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="7000f-185">You're instructing Web Deploy to use NTLM authentication.</span></span> <span data-ttu-id="7000f-186">Web 部署將會使用您用來叫用 Microsoft Build Engine (MSBuild) 的認證來執行。</span><span class="sxs-lookup"><span data-stu-id="7000f-186">Web Deploy will run using the credentials you used to invoke the Microsoft Build Engine (MSBuild).</span></span>
- <span data-ttu-id="7000f-187">您正在使用整合式的驗證來部署**ContactManager** TESTDB1 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7000f-187">You're using integrated authentication to deploy the **ContactManager** database to TESTDB1.</span></span> <span data-ttu-id="7000f-188">資料庫會使用您用來叫用 MSBuild 的認證來進行部署。</span><span class="sxs-lookup"><span data-stu-id="7000f-188">The database will be deployed using the credentials you used to invoke MSBuild.</span></span>


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a><span data-ttu-id="7000f-189">範例 2 & #x 2014年; 部署至 Web 部署處理常式端點</span><span class="sxs-lookup"><span data-stu-id="7000f-189">Example 2&#x2014;Deployment to the Web Deploy Handler Endpoint</span></span>

<span data-ttu-id="7000f-190">在這個範例中：</span><span class="sxs-lookup"><span data-stu-id="7000f-190">In this example:</span></span>

- <span data-ttu-id="7000f-191">您要部署至 Web 部署的處理常式的服務端點上 STAGEWEB1。</span><span class="sxs-lookup"><span data-stu-id="7000f-191">You're deploying to the Web Deploy Handler service endpoint on STAGEWEB1.</span></span>
- <span data-ttu-id="7000f-192">您正在指示使用基本驗證的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="7000f-192">You're instructing Web Deploy to use basic authentication.</span></span>
- <span data-ttu-id="7000f-193">您會指定 Web Deploy 應該模擬 FABRIKAM\stagingdeployer 帳戶，在遠端電腦上。</span><span class="sxs-lookup"><span data-stu-id="7000f-193">You're specifying that Web Deploy should impersonate the FABRIKAM\stagingdeployer account on the remote computer.</span></span>
- <span data-ttu-id="7000f-194">您正在使用 SQL Server 驗證來部署**ContactManager** STAGEDB1 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7000f-194">You're using SQL Server authentication to deploy the **ContactManager** database to STAGEDB1.</span></span>


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a><span data-ttu-id="7000f-195">結論</span><span class="sxs-lookup"><span data-stu-id="7000f-195">Conclusion</span></span>

<span data-ttu-id="7000f-196">此時，您的專案檔案已完整設定建置，並將連絡人管理員方案部署到一或多個目的地環境。</span><span class="sxs-lookup"><span data-stu-id="7000f-196">At this point, your project files are fully configured to build and deploy the Contact Manager solution to one or more destination environments.</span></span>

<span data-ttu-id="7000f-197">若要使用這些專案檔做為單一步驟、 可重複執行部署處理程序的一部分，您需要執行*Publish.proj*檔案使用 MSBuild，並在特定環境的專案檔的位置做為參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="7000f-197">To use these project files as part of a single-step, repeatable deployment process, you need to execute the *Publish.proj* file using MSBuild and pass in the location of the environment-specific project file as a parameter.</span></span> <span data-ttu-id="7000f-198">您可以透過各種方式：</span><span class="sxs-lookup"><span data-stu-id="7000f-198">You can do this in various ways:</span></span>

- <span data-ttu-id="7000f-199">如需 MSBuild 及簡介自訂專案檔的概觀，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-199">For an overview of MSBuild and an introduction to custom project files, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="7000f-200">如需如何編寫執行您的自訂專案檔案的 MSBuild 命令資訊，請參閱[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-200">For information on how to formulate an MSBuild command that executes your custom project files, see [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>
- <span data-ttu-id="7000f-201">如需如何併入單一步驟、 可重複執行部署的命令檔中的 MSBuild 命令資訊，請參閱[建立和執行部署指令檔](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-201">For information on how to incorporate your MSBuild commands into a command file for single-step, repeatable deployments, see [Creating and Running a Deployment Command File](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).</span></span>
- <span data-ttu-id="7000f-202">如需如何從 Team Build 執行您的自訂專案檔資訊，請參閱[建立組建定義該支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="7000f-202">For information on how to execute your custom project files from Team Build, see [Creating a Build Definition that Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7000f-203">上一步</span><span class="sxs-lookup"><span data-stu-id="7000f-203">Previous</span></span>](creating-a-server-farm-with-the-web-farm-framework.md)
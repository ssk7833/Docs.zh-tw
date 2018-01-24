---
title: "分散式快取標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用快取標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="f8a93-103">分散式快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="f8a93-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="f8a93-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="f8a93-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="f8a93-105">分散式快取標記協助程式提供了可大幅改善其分散式快取來源的內容快取的 ASP.NET Core 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f8a93-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="f8a93-106">分散式快取標記協助程式會從快取標記協助程式位於同一個基底類別繼承。</span><span class="sxs-lookup"><span data-stu-id="f8a93-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="f8a93-107">快取標記協助程式相關聯的所有屬性也都適用於分散式標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="f8a93-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="f8a93-108">分散式快取標記協助程式遵循**明確的相依性原則**稱為**建構函式插入**。</span><span class="sxs-lookup"><span data-stu-id="f8a93-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="f8a93-109">具體來說，`IDistributedCache`介面容器傳入分散式快取標記協助專家的建構函式。</span><span class="sxs-lookup"><span data-stu-id="f8a93-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="f8a93-110">如果沒有特定的具象實作`IDistributedCache`中已建立`ConfigureServices`，通常位於 startup.cs，然後分散式快取標記協助程式會將快取的資料儲存為基本的快取標記協助程式使用相同的記憶體提供者。</span><span class="sxs-lookup"><span data-stu-id="f8a93-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="f8a93-111">分散式快取標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="f8a93-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="f8a93-112">啟用到期日到期之後到期-滑動改變-依標頭改變由-查詢改變由-路由改變-由-cookie 改變由位使用者不同依據優先順序</span><span class="sxs-lookup"><span data-stu-id="f8a93-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="f8a93-113">如需定義，請參閱快取標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="f8a93-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="f8a93-114">分散式快取標記協助程式會繼承相同快取標記協助程式類別，讓所有這些屬性都是從快取標記協助程式一般。</span><span class="sxs-lookup"><span data-stu-id="f8a93-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="f8a93-115">名稱 （必填）</span><span class="sxs-lookup"><span data-stu-id="f8a93-115">name (required)</span></span>

| <span data-ttu-id="f8a93-116">屬性類型</span><span class="sxs-lookup"><span data-stu-id="f8a93-116">Attribute Type</span></span>    | <span data-ttu-id="f8a93-117">範例值</span><span class="sxs-lookup"><span data-stu-id="f8a93-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="f8a93-118">字串</span><span class="sxs-lookup"><span data-stu-id="f8a93-118">string</span></span>    | <span data-ttu-id="f8a93-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="f8a93-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="f8a93-120">所需`name`屬性做為分散式快取標記協助程式的每個執行個體儲存該快取的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f8a93-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="f8a93-121">不同於基本根據 Razor 頁面名稱和位置標記協助程式 razor 頁面中的每個快取標記協助程式執行個體指定索引鍵的快取標記 Helper，分散式快取標記協助程式只根據它的索引鍵屬性`name`</span><span class="sxs-lookup"><span data-stu-id="f8a93-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="f8a93-122">使用範例：</span><span class="sxs-lookup"><span data-stu-id="f8a93-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="f8a93-123">分散式快取標記協助程式 IDistributedCache 實作</span><span class="sxs-lookup"><span data-stu-id="f8a93-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="f8a93-124">有兩種實作方式`IDistributedCache`內建於 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="f8a93-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="f8a93-125">其中根據**Sql Server** ，並且根據其他**Redis**。</span><span class="sxs-lookup"><span data-stu-id="f8a93-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="f8a93-126">可以在以下具名 「 使用分散式快取 」 參照的資源中找到這些實作的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f8a93-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="f8a93-127">這兩種實作牽涉到設定的執行個體`IDistributedCache`中 ASP.NET Core **startup.cs**。</span><span class="sxs-lookup"><span data-stu-id="f8a93-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="f8a93-128">沒有特別與任何特定實作方式的相關聯的標記屬性`IDistributedCache`。</span><span class="sxs-lookup"><span data-stu-id="f8a93-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="f8a93-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="f8a93-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
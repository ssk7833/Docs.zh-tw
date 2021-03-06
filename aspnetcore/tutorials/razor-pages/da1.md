---
title: "更新產生的頁面"
author: rick-anderson
description: "以更好的顯示方式更新產生的頁面。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>更新產生的頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。 我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是 **Release Date** (兩個分開的字)。

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>更新產生的程式碼

開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

以滑鼠右鍵按一下紅色曲線 > **[快速動作與重構]**。

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](da1/qa.png)

選取 `using System.ComponentModel.DataAnnotations;`。

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](da1/da.png)

  Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[上一步：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[新增搜尋](xref:tutorials/razor-pages/search)

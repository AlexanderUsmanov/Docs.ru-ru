---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: "Использование обратных передач с ReorderList (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Каждый раз, когда список переупорядочивается, po..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="2849e-104">Использование обратных передач с ReorderList (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="2849e-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="2849e-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2849e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2849e-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2849e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="2849e-107">Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="2849e-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="2849e-108">Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.</span><span class="sxs-lookup"><span data-stu-id="2849e-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="2849e-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="2849e-109">Overview</span></span>

<span data-ttu-id="2849e-110">`ReorderList` Список элемента управления в наборе элементов управления AJAX можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="2849e-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="2849e-111">Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.</span><span class="sxs-lookup"><span data-stu-id="2849e-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="2849e-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="2849e-112">Steps</span></span>

<span data-ttu-id="2849e-113">Существует несколько источников данных для `ReorderList` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="2849e-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="2849e-114">Один является применение `XmlDataSource` управления:</span><span class="sxs-lookup"><span data-stu-id="2849e-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="2849e-115">Чтобы привязать этот XML-код для `ReorderList` необходимо задать элемент управления и Включение обратной передачи, следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="2849e-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="2849e-116">`DataSourceID`: Идентификатор источника данных</span><span class="sxs-lookup"><span data-stu-id="2849e-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="2849e-117">`SortOrderField`: Свойство, по которому выполняется сортировка</span><span class="sxs-lookup"><span data-stu-id="2849e-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="2849e-118">`AllowReorder`: Следует ли разрешить пользователю изменить порядок элементов списка</span><span class="sxs-lookup"><span data-stu-id="2849e-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="2849e-119">`PostBackOnReorder`: Следует ли создавать обратной передачи, когда переупорядочить список</span><span class="sxs-lookup"><span data-stu-id="2849e-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="2849e-120">Ниже приведен соответствующий разметки для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="2849e-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="2849e-121">В пределах `ReorderList` управления определенных данных из источника данных может быть связана с помощью `Eval()` метод:</span><span class="sxs-lookup"><span data-stu-id="2849e-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="2849e-122">В случайном положении на странице метку хранения информации, когда произошло последнее изменение порядка:</span><span class="sxs-lookup"><span data-stu-id="2849e-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="2849e-123">Эта метка заполняется текстом в серверный код, обработка обратной передачи:</span><span class="sxs-lookup"><span data-stu-id="2849e-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="2849e-124">Наконец, чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления могут быть помещены на странице:</span><span class="sxs-lookup"><span data-stu-id="2849e-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="2849e-125">[![Каждый переупорядочение вызывает обратную передачу](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2849e-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="2849e-126">Каждый переупорядочение вызывает обратную передачу ([Просмотр полноразмерное изображение](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2849e-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2849e-127">[Назад](drag-and-drop-via-reorderlist-cs.md)
[Вперед](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2849e-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
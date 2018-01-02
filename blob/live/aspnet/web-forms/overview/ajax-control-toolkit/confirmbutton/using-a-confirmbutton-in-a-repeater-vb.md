---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: "С помощью ConfirmButton в повторителя (VB) | Документы Microsoft"
author: wenz
description: "Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton). Да — только тогда, когда..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 5da8491c157ad6f35c2c25803680f262a35ce6e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="e013b-104">С помощью ConfirmButton в повторителя (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="e013b-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="e013b-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e013b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e013b-106">[Загрузить код](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e013b-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="e013b-107">Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton).</span><span class="sxs-lookup"><span data-stu-id="e013b-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="e013b-108">Только если выбран параметр Да, кнопки действие выполняется, в противном случае отменена.</span><span class="sxs-lookup"><span data-stu-id="e013b-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="e013b-109">Это также можно сделать в повторитель.</span><span class="sxs-lookup"><span data-stu-id="e013b-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="e013b-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="e013b-110">Overview</span></span>

<span data-ttu-id="e013b-111">Расширения ConfirmButton в наборе элементов управления AJAX создает Да/Нет всплывающего меню, когда пользователь щелкает на кнопке (включая управления LinkButton).</span><span class="sxs-lookup"><span data-stu-id="e013b-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="e013b-112">Только если выбран параметр Да, кнопки действие выполняется, в противном случае отменена.</span><span class="sxs-lookup"><span data-stu-id="e013b-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="e013b-113">Это также можно сделать в повторитель.</span><span class="sxs-lookup"><span data-stu-id="e013b-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="e013b-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="e013b-114">Steps</span></span>

<span data-ttu-id="e013b-115">Во-первых источник данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="e013b-115">First of all, a data source is required.</span></span> <span data-ttu-id="e013b-116">В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="e013b-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="e013b-117">Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="e013b-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="e013b-118">Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="e013b-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="e013b-119">Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="e013b-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="e013b-120">Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e013b-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="e013b-121">Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="e013b-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="e013b-122">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="e013b-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="e013b-123">После этого источника данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="e013b-123">Then, a data source is required.</span></span> <span data-ttu-id="e013b-124">Для простоты извлекаются только первых пяти записей в таблице поставщиков AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="e013b-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="e013b-125">Обратите внимание, что при использовании мастера Visual Studio для создания источника данных, имя таблицы (`Vendors`) в настоящее время нет префикса правильно `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="e013b-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="e013b-126">Является правильным следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="e013b-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="e013b-127">Затем этот источник данных можно использовать в пределах повторитель.</span><span class="sxs-lookup"><span data-stu-id="e013b-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="e013b-128">Как обычно `DataBinder.Eval()` метод получает данные из источника данных.</span><span class="sxs-lookup"><span data-stu-id="e013b-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="e013b-129">`ConfirmButtonExtender` Управления затем должен находиться в пределах `<ItemTemplate>` раздел повторителя, чтобы он располагался для каждой из записей в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="e013b-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="e013b-130">[![«Подтверждение» рядом с каждой записи из источника данных](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e013b-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="e013b-131">«Подтверждение» рядом с каждой записи из источника данных ([Просмотр полноразмерное изображение](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e013b-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e013b-132">Назад</span><span class="sxs-lookup"><span data-stu-id="e013b-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
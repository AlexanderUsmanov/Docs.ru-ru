---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Динамически заполнение элемента управления (Visual Basic) | Документы Microsoft
author: wenz
description: Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-vb"></a>Динамически заполнение элемента управления (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Этого учебника показано, как настроить эту функцию.

## <a name="steps"></a>Шаги

Во-первых, необходимо использовать веб-службу ASP.NET, которая реализует метод, вызываемый `DynamicPopulate`. Класс веб-службы требуется `ScriptService` атрибут, который определен в `Microsoft.Web.Script.Services`; в противном случае ASP.NET AJAX не удалось создать клиентский прокси JavaScript для веб-службы, который в свою очередь, необходим `DynamicPopulate`.

Веб-метода следует ожидать один аргумент типа строки, которая называется `contextKey`, так как `DynamicPopulate` управления отправляет сведения о контексте один фрагмент с каждого вызова веб-службы. Следующая веб-служба возвращает текущую дату в формате, представленный `contextKey` аргумент:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Веб-служба затем сохраняется как `DynamicPopulate.vb.asmx`. Кроме того, вы можете реализовать `getDate()` метода в качестве метода страницы в фактические страницы ASP.NET с `DynamicPopulate` элемента управления.

На следующем шаге создания файла ASP.NET. Как всегда, первым шагом является добавление `ScriptManager` на текущей странице загрузки библиотеки ASP.NET AJAX, а также рабочий набор элементов управления:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Добавьте элемент управления label (например с помощью HTML-элемент управления с таким же именем или &lt; `asp:Label`  / &gt; веб-элемента управления) позже которого отображается результат вызова веб-службы.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

HTML-кнопок (как элемент управления HTML, так как мы не требуют обратную передачу на сервер) будет использоваться для запуска динамического заполнения:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Наконец, мы должны `DynamicPopulateExtender` управления связывание всех компонентов. Установит следующие атрибуты (помимо очевидными, `ID` и `runat` = `"server"`):

- `TargetControlID` для размещения результат из вызова веб-службы
- `ServicePath` путь к веб-службы (опустить, если вы хотите использовать метод страницы)
- `ServiceMethod` Имя веб-метода или метода страницы
- `ContextKey` сведения о контексте, отправляемых в веб-службы
- `PopulateTriggerControlID` элемент, который инициирует вызов веб-службы
- `ClearContentsDuringUpdate` следует ли очистить целевого элемента во время вызова веб-службы

Как видите, элемент управления требует некоторые сведения, но размещением всего подряд в месте довольно прост. Разметка для `DynamicPopulateExtender` управления ситуации:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Откройте страницу ASP.NET в браузере и нажмите кнопку "; Вы получите текущая дата в формате месяц день год.


[![Нажмите кнопку извлекает дату с сервера](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Нажмите кнопку извлекает дату с сервера ([Просмотр полноразмерное изображение](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Вперед](dynamically-populating-a-control-using-javascript-code-vb.md)

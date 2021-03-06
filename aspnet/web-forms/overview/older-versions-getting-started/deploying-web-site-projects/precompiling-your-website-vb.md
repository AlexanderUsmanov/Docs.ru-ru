---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Предварительная компиляция веб-сайта (VB) | Документы Microsoft
author: rick-anderson
description: 'Visual Studio предлагает разработчикам ASP.NET два типа проектов: проекты веб-приложений (WAPs) и веб-сайтов (WSPs). Одно из ключевых различий betwe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 7296808480fa48b4afd0b308cd27707378519747
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="precompiling-your-website-vb"></a>Предварительная компиляция веб-сайта (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) или [скачать PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio предлагает разработчикам ASP.NET два типа проектов: проекты веб-приложений (WAPs) и веб-сайтов (WSPs). Одним из ключевых различий между двух типов проектов: WAPs должны иметь код явно скомпилирована до развертывания, в то время как можно автоматически компилировать код в WSP-файла на веб-сервере. Тем не менее можно выполнить предварительную компиляцию WSP перед их развертыванием. Этот учебник изучает преимущества предварительной компиляции и показано, как для предварительной компиляции веб-сайт из среды Visual Studio и из командной строки.


## <a name="introduction"></a>Вступление

Visual Studio предлагает разработчикам ASP.NET двух различных типов проектов: проекты веб-приложения (WAP) и проекты веб-сайта (WSP). Основные различия между этими типами проектов такая необходимость WAPs *явную компиляцию* то время как использовать WSPs *автоматическую компиляцию*, по умолчанию. С WAPs, компиляции кода веб-приложения в одну сборку, которая была создана на веб-сайте `Bin` папки. Развертывание предполагает копирование содержимого разметки ( `.aspx.ascx`, и `.master` файлы) в проекте, и сборку в `Bin` папки; кода сами файлы класса не обязательно должны быть развернуты. С другой стороны развертывании WSPs путем копирования разметки страницы и соответствующих классов кода в рабочей среде. Классы кода компилируются по требованию на веб-сервере.

> [!NOTE]
> Вернуться к разделу «Явную компиляцию и автоматическую компиляцию» в [ *определения файлов, которые должны быть развернуты* учебника](determining-what-files-need-to-be-deployed-vb.md) Дополнительные сведения о различиях между проекта модели, явные и автоматической компиляции и влияние развертывания модели компиляции.


Параметр автоматического компиляции проста в использовании. Этап не явную компиляцию и файлы, которые были изменены, требуется для развертывания, в то время как явную компиляцию требуются развертывание измененные разметки страницы и просто скомпилированной сборки. Тем не менее для автоматического развертывания имеет две потенциальные недостатки:

- Так как страницы необходимо скомпилировать автоматически при их первом посещении, может существовать короткое, но заметно задержка при запросе страницы ASP.NET в первый раз после развертывания.
- Автоматическая компиляция требуется присутствие на веб-сервере как декларативные разметку и исходный код. Это может быть все параметр, если вы планируете Продажа веб-приложения для заказчиков и установить его на своих веб-серверах.

Если двух выше недостатков добавлены средства разбиения по возможности, можно либо переключиться на модель WAP или *предварительной компиляции* WSP-файла перед их развертыванием. Этот учебник проверяет параметров предварительной компиляции, лучше всего подходит для размещенного веб-сайта и описывается процесс предварительной компиляции и развертывания предварительно скомпилированного веб-сайта.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Обзор создания и компиляции кода ASP.NET

Прежде чем рассмотреть доступные варианты предварительной компиляции, сначала поговорим о создания и компиляции кода, возникает при запросе страницы ASP.NET в первый раз с момента создания или последнего обновления. Как вы знаете, страниц ASP.NET состоят из двух частей: декларативная разметка в `.aspx` файла, а также часть исходного кода, обычно в файле класса отдельный кода (`.aspx.vb`). Шаги, выполняемые средой выполнения при запросе страницы ASP.NET зависит от модели компиляции приложения.

С WAPs исходный код страницы должны явно компилироваться в одну сборку перед развертыванием. Во время развертывания этой сборки и различные разметки страницы копируются в рабочую среду. При получении запроса на веб-сервер для страницы ASP.NET, среда выполнения создает экземпляр класса кода программной части страницы и вызывает его `ProcessRequest` метод, который запускает жизненного цикла страницы и в конечном итоге приводит к возникновению ошибки содержимое страницы, которые возвращены в Инициатор запроса. Среда выполнения может работать с классом кода страницы ASP.NET, так как класс кода программной уже был скомпилирован в сборку до развертывания.

WSPs и автоматическую компиляцию еще не явную компиляцию действие перед их развертыванием. Вместо этого развертывания включает в себя копирование декларативные и содержимого исходного кода в рабочей среде. При получении запроса на веб-сервер для страницы ASP.NET в первый раз с момента создания или последнего обновления страницы, среда выполнения сначала необходимо скомпилировать класс кода в сборку. Это скомпилированную сборку сохраняется в папке `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, несмотря на то, что местоположения этой папки можно настроить через `<pages>` элемент в `Web.config`. Из-за сборки сохраняются на диске, он не требуется повторной компиляции при последующих запросах к той же странице.

> [!NOTE]
> Как и следовало ожидать, имеется небольшая задержка при запросе страницы в первый раз (или в первый раз, так как он изменен) в сайт, использующий автоматическую компиляцию, сколько потребуется некоторое время для сервера, чтобы скомпилировать код страницы и сохранить результирующей сборки диск.


Иными словами с явную компиляцию необходимо скомпилировать исходный код веб-сайта, перед развертыванием, сохранение среды выполнения от необходимости выполнять этот шаг. При использовании автоматического компиляции среда выполнения обрабатывает компиляции исходного кода страницы, но и затраты на небольшие инициализации для первом посещении страницы с момента создания или последнего обновления.

Что касается декларативный часть страницы ASP.NET ( `.aspx` файл)? Очевидно, что есть связь между `.aspx` файлы и код в своих классах кода как веб-элементов управления, определенных в декларативная разметка доступны в коде. Очевидно, содержимое `.aspx` файлов в значительной степени влияет отображения разметки, созданные на странице. Так как среда выполнения поддерживает текст, HTML, и синтаксис веб-элемента управления, определенные в `.aspx` файл для создания запрошенной страницы отображенное содержимое?

Я не хочу слишком уходите на детали реализации низкого уровня, которые зависят от WAPs и WSPs, но по сути среда выполнения автоматически создает файл класса, который содержит различные веб-элементы управления защищенные члены и методы. Этот созданный файл реализуется в виде *разделяемый класс* в соответствующий класс кода. ([Разделяемые классы](http://www.dotnet-guide.com/partialclasses.html) позволяют быть распределены между несколькими файлами содержимого одного класса.) Таким образом, класс кода определяется в двух местах: в `.aspx.vb` файл, создать и в этот класс создан, созданные средой выполнения. Этот класс создан хранится в `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` папки.

Важные убрать Вот что для страницы ASP.NET для отображения средой выполнения обоих его декларативный и частей исходного кода, должны компилироваться в сборку. С WAPs явным образом исходный код компилируется в сборку до развертывания, но декларативная разметка по-прежнему может быть преобразована в код и компилированных средой выполнения веб-сервера. С WSPs, используя автоматическую компиляцию исходный код и декларативная разметка необходимо скомпилировать веб-сервером.

Можно использовать явную компиляцию с моделью WSP-файла. Можно явно компилировать ту часть исходного кода, как с моделью WAP. Более того можно выполнить компиляцию декларативная разметка.

## <a name="precompilation-options"></a>Параметры предварительной компиляции

Платформа .NET Framework поставляется с [средства компиляции ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) , позволяет компилировать исходный код (и даже содержимое) приложения ASP.NET, созданный при помощи модели WSP-файла. Этот инструмент был выпущен с .NET Framework версии 2.0 и находится в папке `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` папку; его можно использовать из командной строки или запущено из Visual Studio через меню «Построение» параметр публикации веб-сайта.

Средство компиляции предоставляет две общие формы компиляции: с заменой предварительной компиляции и предварительной компиляции для развертывания. С предварительной компиляции на месте выполнить `aspnet_compiler.exe` средство из командной строки и укажите путь к виртуальному каталогу или физический путь веб-сайта, который находится на компьютере. Средство компиляции компилирует каждой страницы ASP.NET в проекте, хранение скомпилированная версия в `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` папки так же, как если посещения таких страниц каждого был впервые из браузера. Компиляция может ускорить первый запрос, выполненный к развернутой страниц ASP.NET на веб-узле, поскольку при этом снимается среды выполнения от необходимости выполнять этот шаг. Тем не менее компиляция неприменим к большую часть размещаемого веб-сайтов, так как требуется, можно запускать программы с веб-сервера командной строки. Этот уровень доступа не допускается в общей среде размещения.

> [!NOTE]
> Дополнительные сведения о предварительной компиляции на месте извлечь [How To: предварительной компиляции веб-сайтов ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) и [предварительной компиляции в ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Вместо компиляции страницы в веб-сайт `Temporary ASP.NET Files` папке предварительной компиляции для развертывания компилирует страниц в каталоге по своему выбору, а также в формате, который может быть развернут в рабочей среде.

Два варианта предварительной компиляции для развертывания, который мы исследуем в этом учебнике: предварительной компиляции с обновляемым пользовательский интерфейс и предварительной компиляции необновляемые пользовательский интерфейс. Предварительная компиляция с обновляемым пользовательский интерфейс оставляет декларативная разметка в `.aspx`, `.ascx`, и `.master` файлы, тем самым позволяя разработчику возможность просмотра и, при необходимости изменения декларативная разметка на рабочем сервере. Предварительная компиляция необновляемые пользовательский интерфейс, приводит к возникновению ошибки `.aspx` страниц, которые относятся к void любое содержимое, и удаляет `.ascx` и `.master` файлы, тем самым скрытие декларативная разметка и запретить изменение со разработчик Рабочая среда.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Предварительная компиляция для развертывания с обновляемым пользовательский интерфейс

Пример в действии является лучшим способом узнать предварительной компиляции для развертывания. Давайте предварительной компиляции WSP проверками книгу для развертывания с помощью обновляемых пользовательский интерфейс. Средство компиляции ASP.NET может быть вызвана из меню "Построение" Visual Studio или из командной строки. В данном разделе рассматриваются с помощью средства в Visual Studio. в разделе «Предварительная компиляция из командной строки» рассматривается запуск средства компиляции из командной строки.

Откройте WSP Просмотр книги в Visual Studio, перейдите в меню "Построение" и выберите пункт меню в публикации веб-сайта. Откроется диалоговое окно публикации веб-сайта (см. **рис. 1**), где можно указать целевое расположение ли обновить пользовательский интерфейс предварительно скомпилированному сайту и другие параметры компилятора. Целевое расположение может быть удаленный веб-сервер или FTP-сервер, но сейчас выберите папку на жестком диске компьютера. Поскольку мы хотим для предварительной компиляции узла с обновляемым пользовательский интерфейс, оставьте флажок установлен флажок «Разрешить это предварительно скомпилированному сайту быть обновляемым» и нажмите кнопку ОК.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Рис. 1**: средства компиляции ASP.NET будет предварительной компиляции веб-сайта в указанном целевом расположении  
 ([Просмотр полноразмерное изображение](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> Параметр публикации веб-сайта, в меню "Построение" недоступен в Visual Web Developer. Если вы используете Visual Web Developer, необходимо использовать версию средства компиляции ASP.NET, которая рассматривается в разделе «Предварительная компиляция из командной строки».


После предварительной компиляции веб-сайта, перейдите в целевое расположение, введенный в диалоговом окне публикации веб-сайта. Теперь пора Сравнение содержимого этой папки с содержимым веб-сайта. **На рисунке 2** показывает папку рецензий веб-сайта. Обратите внимание, что он содержит оба `.aspx` и `.aspx.cs` файлов. Кроме того, обратите внимание, что `Bin` каталог содержит только один файл `Elmah.dll`, который мы добавили в [предыдущего учебника](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**На рисунке 2**: В каталоге проекта `.aspx` и `.aspx.cs` файлы; `Bin` папка содержит только `Elmah.dll`  
 ([Просмотр полноразмерное изображение](precompiling-your-website-vb/_static/image6.png))

**Рис. 3** показано расположение папку, содержимое которого были созданы с помощью средства компиляции ASP.NET. Эта папка содержит все файлы кода. Кроме того эта папка `Bin` каталог содержит несколько сборок и два `.compiled` файлы в дополнение к `Elmah.dll` сборки.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Рис. 3**: папка расположения целевого включает файлы для развертывания  
 ([Просмотр полноразмерное изображение](precompiling-your-website-vb/_static/image9.png))

В отличие от явную компиляцию в WAPs предварительной компиляции, процесс развертывания не создавать отдельную сборку для всего сайта. Вместо этого он создает пакеты друг с другом несколько страниц в каждой сборке. Также компилирует `Global.asax` файла (при его наличии) в своей сборке, а также все классы в `App_Code` папки. Файлы, которые содержат декларативная разметка для ASP.NET, веб-страниц, пользовательские элементы управления и главные страницы (`.aspx`, `.ascx`, и `.master` файлы, соответственно) копируются в виде-имеет место в конечный каталог. Аналогичным образом `Web.config` файл копируется на, вместе с любой статические файлы, такие как изображения, PDF-файлы и классов CSS. Описание более формальных как средство компиляции обрабатывает различные типы файлов, ознакомьтесь с [файл обработки во время предварительной компиляции ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Можно настроить средство компиляции, чтобы создать одну сборку для каждой страницы ASP.NET, пользовательский элемент управления или главная страница, установив флажок «Используется фиксированное именование и сборки для каждой страницы» в диалоговом окне публикации веб-сайта. Наличие каждой страницы ASP.NET компилируются в своей сборке позволяет более точный контроль над развертывания. Например, если обновления одной веб-страницы ASP.NET и требуется развернуть это изменение, требуются только развернуть эту страницу `.aspx` файл и связанная сборка в рабочую среду. Обратитесь к [How To: создавать фиксированными именами, с помощью средства компиляции ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) для получения дополнительной информации.


Расположение целевой каталог также содержит файл, который не был частью предварительно скомпилированного веб-проекта, а именно `PrecompiledApp.config`. Этот файл информирует среды выполнения ASP.NET, что приложение было предварительно скомпилировано и является ли оно было предварительно компилировано с обновляемый или обновляемый полудня пользовательского интерфейса.

Наконец, теперь пора откройте один из `.aspx` файлы в целевом расположении, с помощью Visual Studio или в текстовом редакторе, по выбору. При предварительной компиляции для развертывания с обновляемым пользовательский интерфейс, страниц ASP.NET в целевом каталоге расположение содержат точное же разметку как соответствующие файлы на веб-сайте.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Предварительная компиляция для развертывания с необновляемые пользовательского интерфейса

Средства компилятора ASP.NET может также использоваться для предварительной компиляции узла для развертывания необновляемые пользовательский интерфейс. При предварительной компиляции узла необновляемые пользовательский интерфейс работает почти как предварительная компиляция с обновляемым пользовательским Интерфейсом, Ключевое отличие состоит в том, страниц ASP.NET, пользовательские элементы управления и главные страницы в целевом каталоге удаляются из их разметки. Для предварительной компиляции веб-сайт для развертывания с необновляемые пользовательского интерфейса, выберите параметр публикации веб-сайта из меню "Построение", но снимите флажок «Разрешить это предварительно скомпилированному сайту быть обновляемым» (в разделе **рис. 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Рис. 4**: снимите флажок «Разрешить это предварительно скомпилированному сайту быть обновляемым» параметр для использования предварительной компиляции с без возможности обновления пользовательского интерфейса  
 ([Просмотр полноразмерное изображение](precompiling-your-website-vb/_static/image12.png))

**Рис. 5** показывает расположение целевую папку после предварительной компиляции необновляемые пользовательский интерфейс.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Рис. 5**: расположение папки для развертывания с необновляемые пользовательского интерфейса  
 ([Просмотр полноразмерное изображение](precompiling-your-website-vb/_static/image15.png))

Сравнение **рис. 3** для **рис. 5**. Хотя две папки может выглядеть так же, обратите внимание, что необновляемые папку пользовательского интерфейса не имеет главной страницы `Site.master`. А **рис. 5** включает в себя различные страницы ASP.NET, если просмотреть содержимое этих файлов вы увидите, что они были удаляются их декларативная разметка и заменить текст заполнителя: «это файл маркера, созданные Средство предварительной компиляции и не может быть удалена!»

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Рис. 5**: декларативная разметка был удален из страниц ASP.NET

`Bin` Папки в **3 цифры** и **5** более существенно отличаются. В дополнение к сборкам `Bin` папки в **рис. 5** включает `.compiled` файл для каждой страницы ASP.NET, пользовательский элемент управления и главную страницу.

При предварительной компиляции узла с пользовательским Интерфейсом необновляемые полезно в ситуациях, где вы не хотите содержимое на страницах ASP.NET для изменения, лица или компании, который устанавливает или управляет веб-сайт в рабочей среде. При создании веб-приложения ASP.NET, продажи клиентам для установки на своих собственных веб-серверах, вы можете убедиться, что они не изменяют внешний вид веб-узла путем непосредственного редактирования `.aspx` страницы при их отправке. С помощью предварительной компиляции веб-сайта не обновить пользовательский интерфейс, поставляемых заполнитель `.aspx` страниц в процессе установки, тем самым предотвращая ваши клиенты из проверки или изменения их содержимого.

### <a name="precompiling-from-the-command-line"></a>Предварительная компиляция из командной строки

В фоне диалоговое окно публикации веб-сайта Visual Studio вызывает компилятор ASP.NET (`aspnet_compiler.exe`) для предварительной компиляции веб-сайта. Кроме того можно вызвать это средство из командной строки. На самом деле Если вы используете Visual Web Developer затем необходимо будет для запуска из командной строки, средства компилятора Visual Web Developer в меню "Построение" содержит параметр публикации веб-сайта.

Для использования средства компиляции из командной строки запустите путем удаления в командную строку и перехода к каталогу framework `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Затем введите следующую инструкцию в командной строке:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Приведенная выше команда запускает средство компилятора ASP.NET (`aspnet_compiler.exe`) и через `-p` переключения, требует использования предварительной компиляции веб-сайта с корнем *физической\_путь\_для\_приложения*; Это значение будет примерно `C:\MySites\BookReviews`и должны быть заключены в кавычки.

`-v` — Указывает виртуальный каталог веб-узла. Если узел зарегистрирован как веб-сайта по умолчанию в метабазе IIS, то можно опустить `-p` переключения и просто указать виртуальный каталог приложения. Если вы используете `-p` переключения продолжением значение `-v` параметр указывает на корневую папку веб-сайта и используется для разрешения ссылок на корневой каталог приложения. Для экземпляра Если указать значение `-v /MySite` в приложении, чтобы затем ссылается `~/path/file` будут разрешаться как `~/MySite/path/file`. Так как узел рецензий расположен в корневом каталоге на веб-хостинга компанию, я использовал параметр `-v /`.

`-f` Коммутатора, если он имеется, указывает, что средство компиляции перезаписать *целевой\_расположение\_папки* каталог, если он уже существует. Если не указан `-f` коммутатора и расположение конечной папке уже существует, средство компиляции, будут завершаться с ошибкой: «ошибка ASPRUNTIME: каталог назначения не пуст. Удалите его вручную или выберите другой целевой объект.»

`-u` Коммутатор, если он имеется, сообщает средство для создания обновляемых пользовательский интерфейс. Параметр для предварительной компиляции узла необновляемые пользовательский интерфейс не задан.

Наконец *целевой\_расположение\_папки* является физический путь к каталогу место назначения; это значение будет примерно `C:\MySites\Output\BookReviews`и должны быть заключены в кавычки.

## <a name="deploying-the-precompiled-website"></a>Развертывание предварительно скомпилированного веб-сайта

На этом этапе мы увидели, как использовать средство компиляции ASP.NET для предварительной компиляции веб-сайта, с помощью обоих параметры обновляемых и необновляемые пользовательского интерфейса. Тем не менее наши примеры сих имеет предкомпилированные веб-сайта в локальную папку, а не в рабочей среде. Хорошие новости является развертывание предварительно скомпилированного веб-сайт проще простого и может выполняться через Visual Studio или другой механизм копирования файлов, таких как из автономного FTP-клиента.

Диалоговое окно публикации веб-сайта (в первом отображении **рис. 1**) имеет расположение целевого параметра, который указывает, в который копируются файлы предварительно скомпилированных веб-сайта. Это расположение может быть удаленный веб-сервер или FTP-сервер. Ввод удаленный сервер в это текстовое поле компиляцию и развертывает веб-сайта к указанному серверу за один шаг. Кроме того можно предварительной компиляции веб-сайта в локальную папку и вручную скопировать содержимое этой папки в рабочей среде по FTP или некоторые другие подходы.

Наличие предварительно скомпилированного веб-сайта автоматически развертываются через диалоговое окно публикации веб-сайта Visual Studio является полезным для простых узлов там, где нет различий между средами разработки и эксплуатации конфигурации. Тем не менее, как указано в [ *распространенные конфигурации различия между разработки и эксплуатации* учебника](common-configuration-differences-between-development-and-production-vb.md) нередко для существовать подобные различия. Например рецензий веб-приложение использует другую базу данных в рабочей среде, чем в среде разработки. Если Visual Studio публикует веб-сайта на удаленном сервере слепо копирует файл сведений о конфигурации для работы в среде разработки.

Для узлов с помощью конфигурации различия между средами разработки и эксплуатации может быть лучше выполнять компиляцию в локальный каталог, скопируйте файлы конфигурации, относящиеся к рабочей среде и скопируйте содержимое предварительно скомпилированных выходных данных в рабочей среде.

О копировании файлов из среды разработки в рабочей среде см. в разделе [ *развертывание веб-сайта с помощью FTP-клиент* ](deploying-your-site-using-an-ftp-client-vb.md) и [  *Развертывание вашего веб-сайта с помощью Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) учебники.

## <a name="summary"></a>Сводка

ASP.NET поддерживает две компиляции: автоматический и явный. Как отмечалось в предыдущих учебниках, проекты веб-приложений (WAPs) использовать явную компиляцию, в то время как для проектов веб-сайтов (WSPs) по умолчанию используется автоматическая компиляция. Тем не менее можно явным образом компилировать WSP до развертывания с помощью средства компиляции ASP.NET.

Этот учебник ориентирован на средство компиляции предварительной компиляции для поддержки развертывания. При предварительной компиляции для развертывания, средство компиляции создает целевую папку расположения, компилирует исходный код указанное веб-приложение и копирует их компиляции сборки и файлы содержимого в папке целевого расположения. Средство компиляции можно настроить для создания обновляемых или необновляемые пользовательский интерфейс. При предварительной компиляции с параметром необновляемые пользовательского интерфейса, удаляется декларативная разметка в файлы содержимого. По сути предварительной компиляции дает вам возможность развертывания приложения проекта веб-сайта без включения все файлы исходного кода и удаления декларативной разметкой, при необходимости.

Программирование довольны!

### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Предварительная компиляция веб-узла ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Фонового кода и компиляции в ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Предварительная компиляция в ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Параметры предварительно скомпилированного узла в ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Назад](logging-error-details-with-elmah-vb.md)
> [Вперед](users-and-roles-on-the-production-website-vb.md)

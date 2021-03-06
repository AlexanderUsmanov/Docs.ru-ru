---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Учебник: Сервер широковещательных пакетов с помощью ASP.NET SignalR 1.x | Документы Microsoft'
author: pfletcher
description: Этот учебник показывает, как создание веб-приложения, использующего ASP.NET SignalR широковещательных функциональные возможности сервера. Сервер широковещательных означает то communic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/10/2013
ms.topic: article
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 85d40e411a7ff974da5cc4fa7fbd789b83d92201
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Учебник: Сервер широковещательных пакетов с помощью ASP.NET SignalR 1.x
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник показывает, как создание веб-приложения, использующего ASP.NET SignalR широковещательных функциональные возможности сервера. Рассылка сервера означает, что связям клиентам инициируются сервером. Данный сценарий требует другой подход программирования, чем сценарии одноранговая сеть, например чат приложений, в которых связям клиентам инициируются один или несколько клиентов.
> 
> Приложение, которое вы создадите в этом учебнике имитирует биржевых типичный сценарий для широковещательной рассылки функциональные возможности сервера.
> 
> Комментарии в этом учебнике приветствуются. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Обзор

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) пакет NuGet устанавливает образец смоделированные биржевой приложения в проекте Visual Studio. В первой части этого учебника вы создадите упрощенную версию этого приложения с нуля. В оставшейся части руководства поможет вам установить пакет NuGet и просмотрите дополнительные функции и создаваемый код.

Приложение биржевых типична вида приложения в режиме реального времени, в котором нужно периодически «принудительной отправки» или широковещательной рассылки, уведомлений с сервера на всех подключенных клиентов.

Приложение, которое мы создадим в первой части этого учебника представлена таблица со биржевых данных.

![Начальная версия StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Периодически сервер случайным образом обновляет акций и помещает обновлений для всех подключенных клиентов. В браузере чисел и символов в **изменить** и **%** динамически изменять столбцы в ответ на уведомления с сервера. При открытии дополнительные браузеры же URL-адрес, все они показаны те же данные и те же изменения в данные одновременно.

Этот учебник содержит следующие разделы:

- [Необходимые компоненты](#prerequisites)
- [Создание проекта](#createproject)
- [Добавление пакетов SignalR NuGet](#nugetpackages)
- [Настройка серверного кода](#server)
- [Настройка кода клиента](#client)
- [Тестирование приложения](#test)
- [Включить ведение журнала](#enablelogging)
- [Установка и просмотрите полный образец StockTicker](#fullsample)
- [Дальнейшие действия](#nextsteps)

> [!NOTE]
> Если вы не хотите выполнить необходимые действия по созданию приложения, можно установить пакет SignalR.Sample в новом **пустое веб-приложение ASP.NET** проекта, а также прочтите следующие действия, чтобы получить пояснения кода. Первой части этого руководства описывается подмножество SignalR.Sample кода, а вторая часть Описание ключевых возможностей дополнительные функциональные возможности в пакете SignalR.Sample.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать, убедитесь, что установлена Visual Studio 2012 или 2010 SP1 установлены на компьютере. Если у вас нет Visual Studio, см. раздел [загрузок ASP.NET](https://www.asp.net/downloads) для получения бесплатной Visual Studio 2012 Express для Web.

При наличии Visual Studio 2010, убедитесь, что [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.

<a id="createproject"></a>

## <a name="create-the-project"></a>Создание проекта

1. Из **файл** меню **новый проект**.
2. В **новый проект** диалогового окна разверните **C#** под **шаблоны** и выберите **Web**.
3. Выберите **пустое веб-приложение ASP.NET** шаблона, имя проекта *SignalR.StockTicker*и нажмите кнопку **ОК**.

    ![Диалоговое окно “Новый проект”](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Добавление пакетов SignalR NuGet

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Добавить SignalR и пакеты JQuery NuGet

SignalR функции можно добавить в проект, установка пакета NuGet.

1. Нажмите кнопку **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.
2. Введите следующую команду в диспетчер пакетов.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Пакет SignalR устанавливает ряд других пакетах NuGet в качестве зависимостей. После завершения установки у вас все серверные и клиентские компоненты, необходимые для использования в приложении ASP.NET SignalR.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Настройка серверного кода

В этом разделе можно настроить код, который выполняется на сервере.

### <a name="create-the-stock-class"></a>Создайте класс Stock

Сначала создается класс модели Stock, будет использоваться для хранения и передачи информации о акций.

1. Создайте новый файл класса в папке проекта, назовите его *Stock.cs*, а затем замените код шаблона с помощью следующего кода:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Два свойства, которые можно задать при создании акции: символ (например, MSFT Майкрософт) и цену. Другие свойства зависят от того, как и когда можно задать цены. При первом запуске задать цену, значение возвращает распространяются DayOpen. В следующий раз, если задается цена, изменения и значения свойств PercentChange вычисляются на основе разницы между ценой и DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Создание классов StockTicker и StockTickerHub

Интерфейс API концентратора SignalR будет использоваться для управления взаимодействием с сервера клиенту. StockTickerHub класс, производный от класса концентратора SignalR обрабатывающий получение вызовы методов и подключения от клиентов. Также необходимо поддерживать биржевых данных и выполнения объекта таймера для периодически запускать обновления цен, независимо от клиентских подключений. Эти функции нельзя поместить в классе концентратора, так как экземпляры концентратора являются временными. Экземпляр класса концентратора создается для каждой операции на концентраторе, такие как подключения и вызовов от клиента к серверу. Таким образом, механизм, который отслеживает биржевых данных, затем обновляет цены и передает изменения цены должна выполняться в отдельном классе назовем StockTicker.

![Широковещательная рассылка из StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Требуется только один экземпляр класса StockTicker для запуска на сервере, поэтому вам потребуется настроить ссылку из каждого экземпляра StockTickerHub StockTicker одноэлементный экземпляр. Класс StockTicker должен иметь возможность широковещательных пакетов на клиентах, так как он имеет биржевых данных и триггеров обновлений, но StockTicker не является классом концентратора. Таким образом класс StockTicker должен получить ссылку на объект контекста подключения концентратора SignalR. Затем его можно использовать объект контекста подключения SignalR рассылка для клиентов.

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и нажмите кнопку **Добавление нового элемента**.
2. При наличии Visual Studio 2012 с [ASP.NET и веб-средства 2012.2 обновление](https://go.microsoft.com/fwlink/?LinkId=279941), нажмите кнопку **Web** под **Visual C#** и выберите **класс концентратора SignalR** шаблона элемента. В противном случае выберите **класса** шаблона.
3. Имя для нового класса *StockTickerHub.cs*, а затем нажмите кнопку **добавить**.

    ![Добавить StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Замените код шаблона с помощью следующего кода:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    [Концентратора](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) класс используется для определения методов, клиенты могут вызывать на сервере. Вы определяете один метод: `GetAllStocks()`. Когда клиент изначально подключается к серверу, он вызывает этот метод, чтобы получить список всех акций с их текущей цены. Метод может выполняются синхронно и возвращать `IEnumerable<Stock>` , так как он возвращает данные из памяти. Приходилось получать данные, то, что привело бы к ожидания, например, поиск в базе данных или вызов веб-службы, выполнив метод следует указать `Task<IEnumerable<Stock>>` как возвращаемое значение асинхронной обработки. Дополнительные сведения см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - если для асинхронного выполнения](index.md).

    Атрибут HubName указывает, как концентратор будет ссылаться в коде JavaScript на стороне клиента. Имя по умолчанию на стороне клиента, если вы не используете этот атрибут — это версия стиле Camel, от имени класса, что в этом случае было бы stockTickerHub.

    Как вы увидите позже при создании класса StockTicker, одноэлементный экземпляр этого класса создается в его статическое свойство экземпляра. Одноэлементный экземпляр StockTicker остается в памяти независимо от того, сколько клиентов или разрыв подключения, что этот экземпляр является GetAllStocks метод используется для возврата текущего биржевых данных.
5. Создайте новый файл класса в папке проекта, назовите его *StockTicker.cs*, а затем замените код шаблона с помощью следующего кода:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Так как несколько потоков будет выполняться один и тот же экземпляр StockTicker кода, класс StockTicker должен быть threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Хранение одноэлементный экземпляр в статическом поле

    Этот код инициализирует статический \_экземпляр поля, поддерживающего свойство экземпляра с экземпляром класса, и это является единственным экземпляром класса, который может быть создан, поскольку конструктор помечен как частный. [Отложенная инициализация](https://msdn.microsoft.com/library/dd997286.aspx) используется для \_поле экземпляра, а не для повышения производительности, но чтобы убедитесь, что создание экземпляра threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Каждый раз, когда клиент подключается к серверу, новый экземпляр класса StockTickerHub, работающих в отдельном потоке возвращает одноэлементный экземпляр StockTicker из статическое свойство StockTicker.Instance, как было показано ранее в классе StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Хранение данных об акциях в руководство.

    Конструктор инициализирует \_коллекция акций с некоторыми образец биржевых данных и GetAllStocks возвращает акции. Как показано выше, StockTickerHub.GetAllStocks этот сервер метод в классе концентратора, который клиенты смогут вызывать в свою очередь возвращается акций этой коллекции.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    Представляет собой коллекцию акции [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) тип безопасности потока. В качестве альтернативы можно использовать [словарь](https://msdn.microsoft.com/library/xfhwa508.aspx) объекта и явно заблокировать словарь при внесении изменений в него.

    Для этого примера приложения что достаточно для хранения данных приложения в памяти и потери данных при удалении экземпляра StockTicker. В реальном приложении будет работать с хранилищем данных серверной части, например к базе данных.

    ### <a name="periodically-updating-stock-prices"></a>Периодическое обновление цен на акции

    Конструктор запускается объект таймера, который периодически вызывает методы, которые обновляют цены на основе случайной выборки.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices вызывается таймер, который передает значения NULL в параметре state. Перед обновлением цены, устанавливается блокировка \_updateStockPricesLock объекта. Код проверяет, если другой поток уже обновление цен и затем вызывает TryUpdateStockPrice для каждой из них в списке. Метод TryUpdateStockPrice решает, следует ли изменить цены акции и объем, чтобы изменить его. При изменении цены акции BroadcastStockPrice вызывается для вещания акций изменения для всех подключенных клиентов.

    \_UpdatingStockPrices флаг помечен как [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) для обеспечения доступа к нему threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    Вызовите метод TryUpdateStockPrice в реальном приложении веб-службы для поиска цены. в этом коде он использует генератора случайных чисел для изменения случайным образом.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Получение контекста SignalR, чтобы выполнить рассылку класс StockTicker клиентам

    Поскольку изменения цен здесь приходят в объекте StockTicker, это объект, который необходимо вызвать метод updateStockPrice на всех подключенных клиентов. В классе концентратора у вас есть API для вызова методов клиента, но StockTicker не является производным от класса концентратора и не имеет ссылки на любой объект концентратора. Таким образом чтобы вещания для подключенных клиентов, класс StockTicker должно получить экземпляр контекста SignalR для класса StockTickerHub и использовать его для вызова методов, на клиентах.

    Код получает ссылку на контекст SignalR, создавая одноэлементный экземпляр класса, ссылающиеся передает в конструктор и конструктор помещает его в свойстве клиентов.

    Существует две причины, почему нужно получить контекст только один раз: контекст является ресурсоемкой операцией и получения его один раз гарантирует, что предполагаемым порядок сообщений, отправленных клиентами сохраняется.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Получение свойства контекста клиентов и помещение их в свойстве StockTickerClient позволяет написать код для вызова методов клиента, который выглядит так же, как если бы в классе концентратора. Например рассылка для всех клиентов можно написать Clients.All.updateStockPrice(stock).

    Метод updateStockPrice, вызываемой в BroadcastStockPrice не существует; она будет добавлена позже при написании кода, который выполняется на клиенте. Можно ссылаться на updateStockPrice здесь, поскольку Clients.All является динамической, это означает, что выражение будет вычислено во время выполнения. При вызове этого метода выполняется, SignalR отправит имя метода и значение параметра клиента и если у клиента имеется метод с именем updateStockPrice, этот метод будет вызван, и он передается значение параметра.

    Clients.All означает отправить всем клиентам. SignalR предоставляет другие параметры, чтобы указать, какие клиенты или группы клиенты для отправки. Дополнительные сведения см. в разделе [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Регистрация маршрутов SignalR

Этот сервер должен знать, какой URL-адрес для перехвата и направления для SignalR. Сделать вы добавите код для *Global.asax* файла.

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите команду **Добавление нового элемента**.
2. Выберите **глобальный класс приложения** шаблона, а затем щелкните **добавить**.

    ![Добавить global.asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Добавить в приложение код регистрации маршрутов SignalR\_Start-метод:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    По умолчанию — базовый URL-адрес для всех типов трафика SignalR «/ signalr», и «/ signalr/концентраторов» используется для получения динамически создаваемый файл JavaScript, который определяет прокси-серверы для всех концентраторов в приложении. Метод MapHubs включает перегрузки, которые позволяют задать другой базовый URL-адрес и некоторые параметры SignalR в экземпляре [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) класса.
4. Добавить с помощью оператора в верхней части файла:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Сохраните и закройте *Global.asax* файл и выполните построение проекта.

Завершена настройка серверного кода. В следующем разделе составим клиента.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Настройка кода клиента

1. Создайте новый HTML-файл в папке проекта и назовите его *StockTicker.html*.
2. Замените код шаблона с помощью следующего кода:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    HTML создает таблицу с 5 столбцов, строку заголовка и строку данных с одна ячейка, которая распространяется на все 5 столбцов. Строка данных отображается «Загрузка» и отображается только моментально при запуске приложения. Код JavaScript будет удалить эту строку и добавьте в строках месте биржевых данных, получаемых с сервера.

    Теги сценариев укажите jQuery файл скрипта, файла сценария SignalR core, файл скрипта прокси SignalR и файл скрипта StockTicker, которое будет создано в более поздней версии. Файл скрипта прокси SignalR, который указывает URL-адрес «/ signalr/концентраторов», формируется динамически и определяет методы прокси-сервера для методов в классе концентратора, но в этом случае для StockTickerHub.GetAllStocks. При желании можно вручную создать этот файл JavaScript с помощью [SignalR программы](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) и отключить создание динамического файла в вызове метода MapHubs.
3. > [!IMPORTANT]
   > Убедитесь, что ссылки на файл JavaScript в *StockTicker.html* заданы правильно. То есть, убедитесь в версии jQuery в ваш скрипт тег (1.8.2 в примере) аналогично версии jQuery в своем проекте *сценариев* папки и убедитесь в том, что версия SignalR в ваш скрипт тег совпадает со значением SignalR версия в своем проекте *сценариев* папки. При необходимости измените имена файлов в теги сценариев.
4. В **обозревателе решений**, щелкните правой кнопкой мыши *StockTicker.html*, а затем нажмите кнопку **задать в качестве начальной страницы**.
5. Создайте новый файл JavaScript в папке проекта и назовите его *StockTicker.js*...
6. Замените код шаблона с помощью следующего кода:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection ссылается на прокси-серверы, SignalR. Этот код возвращает ссылку на прокси-сервер для класса StockTickerHub и помещает его в переменной биржевых котировок. Имя прокси-сервер имеет имя, которое было задано с помощью атрибута [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    После определения переменных и функций в последней строке кода в файле инициализирует подключения SignalR путем вызова функции запуска SignalR. Функция запуска выполняется асинхронно и возвращает [объекта отложенный jQuery](http://api.jquery.com/category/deferred-object/), что означает можно вызвать функцию Готово для указания функция, вызываемая при завершении асинхронной операции...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    Init-функция вызывает функцию getAllStocks на сервере и использует сведения, сервер возвращает для обновления таблицы акций. Обратите внимание, что по умолчанию, необходимо использовать верблюжий на клиенте несмотря на то, что имя метода в стиле Pascal на сервере. Регистрах правило применяется только к методам, не объекты. Например можно ссылаться на складе. Символ и акций. Цена, не stock.symbol или stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Если решено использовать pascal регистр на клиентском компьютере, или если вы хотите использовать имя совершенно другой метод, может декорировать метод концентратора с атрибутом HubMethodName так же, как вы внутренним классу Hub с атрибутом HubName.

    В методе init HTML-код для строки таблицы создается для каждого объекта акций, полученных с сервера вызывающего formatStock для свойства форматирования stock-объектом, и путем вызова будут отменены (который определен в верхней части *StockTicker.js*) для замены заполнителей в переменной rowTemplate со значениями свойств stock-объектом. Затем результирующий HTML добавляется таблицы акций.

    Вызовите init, передав его в качестве функции обратного вызова, который выполняется после завершения функции асинхронного запуска. При вызове init как отдельную инструкцию после начала вызова JavaScript, функция завершится сбоем, так как оно будет выполнено немедленно, не дожидаясь завершения подключения к функции запуска. В этом случае init-функция будет выполнена попытка вызвать функцию getAllStocks, прежде чем установить соединение с сервером.

    При изменении цены сервера он вызывает updateStockPrice на подключенных клиентов. Функция добавляется свойство клиента stockTicker прокси-сервера, чтобы сделать его доступным для вызовов с сервера.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    Функция updateStockPrice форматирует получил от сервера в строке таблицы так же, как функция init stock-объектом. Тем не менее вместо того чтобы добавить строку в таблицу, он находит stock текущей строки в таблице и заменяет этой строки на новую.

<a id="test"></a>

## <a name="test-the-application"></a>Тестирование приложения

1. Нажмите клавишу F5 для запуска приложения в режиме отладки.

    Таблицы акций изначально отображает «загрузка...» строки, затем после небольшой задержки появится начальной биржевых данных, и запустите цен на акции для изменения.

    ![Загрузка](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Начальный таблицы акций](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Биржевые таблица, получающая изменения с сервера](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Копировать URL-адрес из адресной строки браузера и вставьте его в новый окна браузера.

    Первоначального отображения биржевых совпадает со значением первого браузера, и изменения происходить одновременно.
3. Закройте все браузеры и откройте новый браузер, перейдите к же URL-адрес.

    Для запуска на сервере, поэтому таблицы акций показывается акций продолжил изменять возобновлена StockTicker одноэлементного объекта. (Не вижу начальной таблицу с нулем Изменение фигур.)
4. Закройте браузер.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Включение ведения журнала

SignalR имеется функция встроенного ведения журнала, которые можно включить на стороне клиента для облегчения устранения неполадок. В этом разделе включите ведение журнала и примеры, которые показывают, как журналы скажет, какой из следующих методов транспорта использует SignalR в разделе:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), поддерживаемые в IIS 8 и текущих браузерах.
- [Сервер отправил события](http://en.wikipedia.org/wiki/Server-sent_events), поддерживаемые в браузерах, отличных от Internet Explorer.
- [Непрерывно фрейма](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), поддерживаемой обозревателем Internet Explorer.
- [AJAX продолжительный опрос](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), поддерживаемые браузеры.

Для каждого соединения SignalR выбирает наилучший способ передачи, сервер и клиент поддерживают.

1. Откройте *StockTicker.js* и добавьте строку кода, чтобы включить ведение журнала непосредственно перед код, который инициализирует подключение в конце файла:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Нажмите клавишу F5, чтобы запустить проект.
3. Откройте окно средств разработчика в браузере и выберите консоль, чтобы просмотреть журналы. Может потребоваться обновить страницу, чтобы просмотреть журналы Signalr согласования метода транспорта для нового соединения.

    При запуске Internet Explorer 10 в Windows 8 (IIS 8), метод транспорта является WebSockets.

    ![Консоли IE 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    При запуске Internet Explorer 10 в Windows 7 (IIS 7.5) метода транспортировки — iframe.

    ![IE 10 консоли IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    В Firefox установите надстройку Firebug окна консоли. При использовании Firefox 19 в Windows 8 (IIS 8), метод транспорта является WebSockets.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    При использовании Firefox 19 в Windows 7 (IIS 7.5) метода транспортировки — события отправки сервером.

    ![Firefox 19 консоли IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Установка и просмотрите полный образец StockTicker

StockTicker приложения, которая устанавливается вместе с [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) пакет NuGet включает больше возможностей, чем упрощенную версию, созданный с нуля. В этом разделе учебника установите пакет NuGet и обзор новых возможностей и код, который их реализует.

### <a name="install-the-signalrsample-nuget-package"></a>Установка пакета SignalR.Sample NuGet

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и нажмите кнопку **управление пакетами NuGet**.
2. В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **Online**, введите *SignalR.Sample* в **поиск в Интернете** , а затем щелкните  **Установка** в **SignalR.Sample** пакета.

    ![Установить пакет SignalR.Sample](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. В *Global.asax* файл, закомментируйте RouteTable.Routes.MapHubs(); строка, добавленный ранее в приложение\_Start-метод.

    Код в *Global.asax* больше не требуется, поскольку пакет SignalR.Sample регистрирует маршрут SignalR в *приложения\_Start/RegisterHubs.cs* файла:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    Класс WebActivator, на который ссылается атрибут сборки включен в пакет WebActivatorEx NuGet, которая устанавливается как зависимость от пакета SignalR.Sample.
4. В **обозревателе решений**, разверните *SignalR.Sample* папку, которая была создана путем установки пакета SignalR.Sample.
5. В *SignalR.Sample* папку, щелкните правой кнопкой мыши *StockTicker.html*, а затем нажмите кнопку **задать в качестве начальной страницы**.

    > [!NOTE]
    > Установка SignalR.Sample NuGet пакета может изменить версия jQuery, имеющегося в вашей *сценариев* папки. Новый *StockTicker.html* файла, который устанавливает пакет в *SignalR.Sample* папки будут синхронизированы с версии jQuery, которая устанавливает пакет, но если вы хотите запустить исходную *StockTicker.html* файл еще раз, возможно, потребуется сначала обновить ссылку на jQuery в теге script.

### <a name="run-the-application"></a>Запуск приложения

1. Нажмите клавишу F5 для запуска приложения.

    В дополнение к таблице, которая ранее видели приложение полной биржевых показывает горизонтальной прокруткой окно, отображающее же биржевых данных. При запуске приложения в первый раз, «Марка» «закрыто», и вы увидите статических сетки и биржевых котировок окна, размеры которого не прокрутки.

    ![Экран запуска StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    При нажатии кнопки **Open Market**, **Live биржевых котировок акций** поле начинает выполнять прокрутку по горизонтали, и сервер запускается периодически передавать изменения акций на основе случайной выборки. При каждом запуске акций изменяется, оба **Live Stock таблицы** сетки и **Live биржевых котировок акций** поле обновляются. При изменения цены акций является положительным, показывается акций с зеленым фоном, и при изменении имеет отрицательное значение, акции отображается с красным фоном.

    ![Откройте приложение StockTicker рынка](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    **Закрыть рынка** кнопка останавливает изменения и останавливает прокрутки биржевых котировок и **Сброс** сбрасывает все биржевых данных в исходное состояние до начала изменения цен. Если открыть несколько окон браузера и перейдите к же URL-адрес, можно увидеть те же данные, динамически обновляется в то же время каждого браузера. Если щелкнуть одну из кнопок, все браузеры отвечать так же, как в то же время.

### <a name="live-stock-ticker-display"></a>Динамическая отображения биржевых котировок акций

**Live биржевых котировок акций** отображается неупорядоченный список элемент div, отформатированные в одну строку в стили CSS. Инициализируется биржевых котировок и обновить так же, как таблицы:, заменив заполнители в &lt;li&gt; строка шаблона и динамическое добавление &lt;li&gt; элементы &lt;ul&gt; элемент. Прокрутка выполняется с помощью функции jQuery анимировать изменение левого поля неупорядоченный список внутри div.

Биржевые сводки HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

Биржевые сводки CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Прокрутите jQuery код, который позволяет:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Дополнительные методы на сервере, который клиент может вызывать

Класс StockTickerHub определяет четыре дополнительные методы, которые клиент может вызывать:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket, CloseMarket и сброс вызываются в ответ на кнопок в верхней части страницы. Они показывают шаблон из одного клиента, активируя изменение в состоянии, немедленно распространяются на всех клиентах. Каждый из этих методов вызывает метод в классе StockTicker которые влияют на состояние рынка изменения и затем передает новое состояние.

В классе StockTicker MarketState свойство, которое возвращает значение перечисления MarketState сохраняется состояние рынка.

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Каждый из методов, изменяющих состояние рынка этого внутри блока блокировки, так как класс StockTicker должен быть threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Чтобы убедиться, что этот код threadsafe, \_marketState поля, поддерживающего свойство MarketState помечен как volatile,

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Методы BroadcastMarketStateChange и BroadcastMarketReset аналогичны BroadcastStockPrice метод, который вы уже видели, за исключением того, вызываются различные методы, определенные для клиентов:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Дополнительные функции на стороне клиента, сервер может вызывать

Функция updateStockPrice теперь обрабатывает сетки и отображения биржевых котировок и использует jQuery.Color мигать красным и зеленый цвета.

Новые функции в *SignalR.StockTicker.js* Включение и отключение кнопок на основе рынок состояния и остановить или запустить биржевых котировок окна горизонтальной прокрутки. Так как несколько функций, добавленных ticker.client, [jQuery расширить функции](http://api.jquery.com/jQuery.extend/) используется для их добавления.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Программа установки дополнительного клиента после подключения к

После клиент устанавливает соединение, он имеет некоторые дополнительные действия, чтобы сделать: проверить, является ли рынке открытых или закрытых для вызова соответствующего marketOpened или функции marketClosed и вложить вызовы методов сервера кнопок.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Методы сервера не соединены до кнопок, пока после установки подключения, чтобы код не можете вызывать методы сервера, прежде чем они станут доступными.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Следующие шаги

В этом учебнике вы узнали, как запрограммировать приложение SignalR, которое осуществляет широковещательную рассылку сообщений с сервера на все подключенные клиенты на периодической основе и в ответ на уведомления с любого клиента. Шаблон использования несколькими потоками одноэлементный экземпляр для отслеживания состояния сервера может также использоваться в сети игры скриптах многопользовательских. Пример см. в разделе [ShootR игры, который основан на SignalR](https://github.com/NTaylorMullen/ShootR).

Руководства с примерами сценариев связи одноранговая сеть, в разделе [Приступая к работе с SignalR](index.md) и [обновление в режиме реального времени с SignalR](index.md).

Для получения более сложных понятиях разработки SignalR, посетите следующие сайты для SignalR исходный код и ресурсы:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Проект SignalR](http://signalr.net/)
- [SignalR Github и образцы](https://github.com/SignalR/SignalR)
- [Вики-сайте SignalR](https://github.com/SignalR/SignalR/wiki)

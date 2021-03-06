---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Добавление нового поля | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a>Добавление нового поля
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе используется Entity Framework Code First Migrations для миграции некоторые изменения в классы моделей, изменение вступает в силу в базе данных.

По умолчанию при использовании Entity Framework Code First автоматически создать базу данных, как это было сделано ранее в этом учебнике Code First добавляет таблицу в базу данных для отслеживания, является ли синхронизация с помощью классов модели, он был создан из схемы базы данных. Если они не являются синхронизированными, Entity Framework выдает ошибку. Это упрощает для выявления проблем во время разработки, которые может в противном случае только найти (непонятные ошибки) во время выполнения.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Настройка миграции Code First для изменения модели

Перейдите в обозреватель решений. Щелкните правой кнопкой мыши *Movies.mdf* файла и выберите **удалить** удаление фильмы базы данных. Если вы не видите *Movies.mdf* файл можно, щелкнув **Показать все файлы** показано ниже в структуре красный значок.

![](adding-a-new-field/_static/image1.png)

Постройте приложение, чтобы убедиться в отсутствии ошибок.

Из **средства** меню, нажмите кнопку **диспетчера пакетов NuGet** и затем **консоль диспетчера пакетов**.

![Добавьте пакет Man](adding-a-new-field/_static/image2.png)

В **консоль диспетчера пакетов** окно в `PM>` приглашение ввести

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-Migrations** (как показано выше) команда создает *Configuration.cs* файл в новом *миграций* папки.

![](adding-a-new-field/_static/image4.png)

Visual Studio открывает *Configuration.cs* файла. Замените `Seed` метод в *Configuration.cs* файла следующим кодом:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Наведите указатель мыши на волнистую красную линию под `Movie` и нажмите кнопку `Show Potential Fixes` и нажмите кнопку **с помощью** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Это добавляет следующих инструкций:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations вызовы `Seed` метод после каждой миграции (то есть вызов **базы данных обновления** в консоли диспетчера пакетов), и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существует.
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) операцией «вставки-обновления» выполняет метод в следующем коде:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Поскольку [начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод выполняется с каждой миграции, просто нельзя вставить данные, так как вы пытаетесь добавить строки уже существует после первой миграции, который создает базу данных. «[Upsert](http://en.wikipedia.org/wiki/Upsert)«операция позволяет избежать ошибок, произойдет при попытке вставить строку, которая уже существует, но оно переопределяет любые изменения данных, внесенные во время тестирования приложения. С тестовыми данными в некоторых таблицах не имеет смысла, происходит: в некоторых случаях при изменении данных во время тестирования требуется изменения после обновления базы данных. В этом случае необходимо выполнить операцию вставки условного: вставьте строку только в том случае, если он еще не существует.   
> 
> Первый параметр, передаваемый [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод задает свойство, используемое для проверки, если строки уже существует. Для тестовых данных фильм, которому предоставляется `Title` свойство может использоваться для этой цели, так как каждый заголовок в списке является уникальным:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Данный код предполагает, что заголовки являются уникальными. Если вручную добавить дублирующийся заголовок, вы получите следующее исключение при последующем выполнении миграции.   
> 
>  *Последовательность содержит более одного элемента*  
> 
> Дополнительные сведения о [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метода, в разделе [Будьте внимательны с EF 4.3 AddOrUpdate метод](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Нажмите клавиши CTRL-SHIFT-B для сборки проекта.** (Ниже завершится ошибкой, если не на этом этапе построения.)

Следующим шагом является создание `DbMigration` класс для первоначальной миграции. Этот вид миграции создает новую базу данных, поэтому вы удалили *movie.mdf* файла в предыдущем шаге.

В **консоль диспетчера пакетов** окно, введите команду `add-migration Initial` для создания первоначальной миграции. Имя «Начального» является произвольным и используется для именования создан файл для миграции.

![](adding-a-new-field/_static/image6.png)

Code First Migrations создает еще один файл класса в *миграций* папку (с именем *{метки даты}\_Initial.cs* ), и этот класс содержит код, создающий схему базы данных. Filename миграции предварительно фиксированной с меткой времени, чтобы помочь с порядком. Изучите *{метки даты}\_Initial.cs* файл, он содержит инструкции по созданию `Movies` таблицы для DB фильма. При обновлении базы данных в инструкциях ниже это *{метки даты}\_Initial.cs* файла будет выполняться и создать схему базы данных. Затем **начальное значение** метод работает для заполнения базы данных тестовыми данными.

В **консоль диспетчера пакетов**, введите команду `update-database` для создания базы данных и выполнения `Seed` метод.

![](adding-a-new-field/_static/image7.png)

Если возникает сообщение об ошибке, указывающее таблицу, уже существует и не может быть создан, возможно, будет потому, что приложение запущено после удаления базы данных и до выполнения `update-database`. В этом случае удаление *Movies.mdf* файл еще раз и повторите `update-database` команды. Если ошибки продолжают возникать, удалите папку migrations и содержимое, а затем запустить с инструкциями в верхней части этой страницы (это delete *Movies.mdf* файла, а затем перейдите к Enable-Migrations). Если вы по-прежнему получить eror, откройте обозреватель объектов SQL Server и удалить базу данных из списка.

Запустите приложение и перейдите к */Movies* URL-адрес. Отображаются данные начальное значение.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Добавление свойства Rating в модель Movie

Начните с добавления нового `Rating` свойства для существующего `Movie` класса. Откройте *Models\Movie.cs* и добавьте `Rating` свойства следующим образом:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Полный `Movie` класса сейчас выглядит как следующий код:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Постройте приложение (Ctrl + Shift + B).

Так как вы добавили новое поле `Movie` класса, также необходимо обновить привязки *утвержденный список* таким образом, это новое свойство включено. Обновление `bind` для атрибута `Create` и `Edit` методы действий для включения `Rating` свойство:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Также необходимо обновить шаблоны представлений, чтобы реализовать отображение, создание и редактирование нового свойства `Rating` в представлении браузера.

Откройте *\Views\Movies\Index.cshtml* и добавьте `<th>Rating</th>` сразу после заголовка столбца **цены** столбца. Затем добавьте `<td>` столбец ближе к концу шаблона для отображения `@item.Rating` значение. Ниже приведен какие обновленный *Index.cshtml* Просмотр шаблона выглядит следующим образом:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Затем откройте *\Views\Movies\Create.cshtml* и добавьте `Rating` поле highlighed следующей разметкой. Это отрисовывает текстовое поле, рейтинг можно указать при создании новых фильмов.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Теперь вы обновили код приложения, поддерживающие новое `Rating` свойство.

Запустите приложение и перейдите к */Movies* URL-адрес. При этом, вы увидите одно из следующих ошибок:

![](adding-a-new-field/_static/image9.png)  
  
Модель резервного контекст «MovieDBContext» изменилось с момента создания базы данных. Рассмотрите возможность обновления базы данных с помощью Code First Migrations (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Вы видите эту ошибку, так как обновленный `Movie` класс модели в приложении теперь отличается от схемы `Movie` существующей базы данных. (В таблице базы данных отсутствует столбец `Rating`.)


Устранить эту ошибку можно несколькими способами:

1. Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели. Этот подход удобен на ранних стадиях цикла разработки, когда все действия осуществляются с тестовой базой данных. В этом случае развитие модели и схемы базы данных осуществляется одновременно. Недостатком такого подхода является потеря существующие данные в базе данных, поэтому вы *не* Чтобы использовать этот подход для производственной базы данных! При разработке приложения часто используется инициализатор для автоматического заполнения базы тестовыми данными. Дополнительные сведения об инициализаторах базы данных Entity Framework см. в разделе [ASP.NET MVC и Entity Framework Учебник](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели. Преимущество такого подхода состоит в том, что сохраняются все данные. Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.
3. Можно обновить схему базы данных с помощью Code First Migrations.


В этом руководстве используется Code First Migrations.

Обновите метод начальное значение, чтобы он предоставляет значение для нового столбца. Откройте файл Migrations\Configuration.cs и добавьте поле «Рейтинг» к каждому объекту фильма.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Постройте решение, а затем откройте **консоль диспетчера пакетов** окно и введите следующую команду:

`add-migration Rating`

`add-migration` Команда указывает среде миграции для проверки текущей модели фильма с текущей схемой фильма базы данных и создания кода, необходимые для переноса базы данных в новую модель. Имя *Оценка* является необязательным и используется для именования файл для миграции. Рекомендуется использовать понятное имя для шага миграции.

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMigration` производного класса и в `Up` метода можно просмотреть код, который создает новый столбец.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Постройте решение, а затем введите `update-database` в **консоль диспетчера пакетов** окна.

На следующем рисунке показаны выходные данные в **консоль диспетчера пакетов** окна (Отметка даты добавления перед *Оценка* будет другим.)

![](adding-a-new-field/_static/image11.png)

Повторно запустите приложение и перейдите к /Movies URL-адрес. Появится новое поле оценки.

![](adding-a-new-field/_static/image12.png)

Нажмите кнопку **создать новый** ссылку, чтобы добавить новый фильма. Обратите внимание, что можно добавить оценку.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Нажмите кнопку **Создать**. Новый фильм, включая оценку, теперь отображается список фильмов:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Теперь, когда в проекте используется миграция, не нужно будет удалить базу данных, при добавлении нового поля или в противном случае обновления схемы. В следующем разделе мы внести дополнительные изменения схемы и использовать миграции для обновления базы данных.

Также следует добавить `Rating` на изменение, сведения и удаление шаблонов представлений.

Можно ввести команду «update-database» в **консоль диспетчера пакетов** окно еще раз и без переноса кода будет выполнен, поскольку схема совпадает модели. Тем не менее, будет выполняться под управлением «update-database» `Seed` метод еще раз, и при изменении любого начальное значение данных, изменения будут потеряны, поскольку `Seed` метод upserts данных. Дополнительные сведения о `Seed` метод в Tom Dykstra популярных [ASP.NET MVC и Entity Framework Учебник](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

В этом разделе вы узнали, как можно изменять объекты модели и синхронизировать базу данных с изменениями. Вы также узнали способ заполнения вновь созданной базы данных с образцами данных, можно опробовать сценариев. Это было просто Краткое введение в Code First см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) более полный учебник по этой теме. Теперь давайте взглянем на как можно добавить расширенные логику проверки в классы модели и включить некоторые бизнес-правила для применения.

> [!div class="step-by-step"]
> [Назад](adding-search.md)
> [Вперед](adding-validation.md)

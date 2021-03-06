---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Итерации #4 — сделать приложение слабо (C#) | Документы Microsoft'
author: microsoft
description: В этой третьей итерации мы воспользоваться преимуществами нескольких шаблонов разработки программного обеспечения, чтобы упростить обслуживание и изменить приложение диспетчера контактов. Для ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 33221c6c3326c7034fe013f152579828e2fc8a3a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Итерации #4 — сделать приложение слабо (C#)
====================
по [Microsoft](https://github.com/microsoft)

[Загрузить исходный код](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> В этой третьей итерации мы воспользоваться преимуществами нескольких шаблонов разработки программного обеспечения, чтобы упростить обслуживание и изменить приложение диспетчера контактов. Например мы выполнили рефакторинг наше приложение, чтобы использовать шаблон репозитория и шаблон внедрения зависимостей.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Создание приложения ASP.NET MVC управления контактами (C#)

В этой серии учебники мы создаем всего приложения управления контактами от начала и завершения. Приложение диспетчера контактов позволяет хранить контактные данные - имена, номера телефонов и адресов электронной почты — список людей.

Мы постройте приложение в нескольких итерациях. С каждой итерацией постепенно мы улучшить приложение. Это несколько итераций предназначена для того, чтобы позволяют понять причину для каждого изменения.

- Итерации #1 - создать приложение. В первой итерации мы создадим диспетчера контактов простейшим способом невозможно. Добавлена поддержка для основных операций базы данных: создание, чтение, обновление и удаление (CRUD).

- Итерации #2 — сделать приложение поиска работы с низким приоритетом. В этой итерации нам улучшить внешний вид приложения, изменив значение по умолчанию главной страницы представления ASP.NET MVC и каскадные таблицы стилей.

- Итерации #3 - Добавление проверки формы. В третьем проходе добавим проверки базовой формы. Мы запретить отправки формы, не завершая обязательные поля. Мы также для проверки адреса электронной почты и номера телефонов.

- Итерации #4 - сделать приложение слабо. В этой третьей итерации мы воспользоваться преимуществами нескольких шаблонов разработки программного обеспечения, чтобы упростить обслуживание и изменить приложение диспетчера контактов. Например мы выполнили рефакторинг наше приложение, чтобы использовать шаблон репозитория и шаблон внедрения зависимостей.

- Итерации #5 - Создание модульных тестов. В пятой итерации сделан нашего приложения проще в обслуживании и изменить, добавив модульных тестов. Мы макета наших классов модели данных и создания модульных тестов для наших контроллеров и логику проверки.

- Итерация 6 # — с помощью управляемой тестами разработки. В итерации этого шестой мы добавим новые функциональные возможности наше приложение, сначала написание модульных тестов и писать код для модульных тестов. В этой итерации добавим групп контактов.

- Итерации #7. Добавление функциональности Ajax. В седьмой итерации мы повысить скорость реагирования и производительности приложения, добавляя поддержку Ajax.

## <a name="this-iteration"></a>Этой итерации

В этом четвертый шаг цикла приложение диспетчера контактов мы выполнили рефакторинг приложение, чтобы сделать приложение более слабо. При приложения слабо связанный, можно изменить код в одной части приложения без изменения кода в других частях приложения. Слабо связанных приложений более устойчивым к изменения.

В настоящее время все используемые приложением диспетчера контактов логики доступа и проверки данных содержится в классы контроллера. Это не рекомендуется. Каждый раз, когда необходимо внести изменения в одной части приложения, то существует риск возникновения ошибок в другой части приложения. Например если вы измените логику проверки, существует опасность возникновения новые ошибки в логике данных access или контроллера.

> [!NOTE] 
> 
> (SRP) класс никогда не должен иметь несколько причин для изменения. Смешение контроллера, проверки и логику базы данных является значительных нарушение персональной ответственности.


Существует несколько причин, которые может потребоваться изменить приложение. Может потребоваться добавить новый компонент в приложение, может потребоваться для исправления ошибки в приложении или может потребоваться изменить способ реализации функции приложения. Приложения, редко статический. Они, как правило, в развитии и изменить со временем.

Допустим, что вы решите изменить, как реализовать слой доступа к данным. Право теперь диспетчера контактов приложение использует Microsoft Entity Framework для доступа к базе данных. Тем не менее вы можете перейти на технологии доступа к данным новых или альтернативных, таких как службы ADO.NET Data Services или NHibernate. Тем не менее поскольку код доступа к данным не изолированы от кода проверки и контроллер, нет возможности для изменения кода доступа к данным в приложении без изменения другой код, который не связан непосредственно для доступа к данным.

Когда приложение слабо связанный, с другой стороны, внесения изменений в одной части приложения не затрагивая другие части приложения. Например можно переключиться технологии доступа к данным без изменения логики проверки или контроллера.

В этой итерации мы воспользоваться преимуществами нескольких шаблонов разработки программного обеспечения, позволяющие определить рефакторинг нашей приложение диспетчера контактов в более слабосвязанные приложения. После этого мы диспетчера контактов, выиграл t выполнять никаких действий, он не существовал t выполнить перед выполнением. Тем не менее мы будем изменять приложение проще в будущем.

> [!NOTE] 
> 
> Рефакторинг — это процесс перезаписи приложения таким образом, что он не потерять существующие функциональные возможности.


## <a name="using-the-repository-software-design-pattern"></a>С помощью шаблона разработки программного обеспечения репозитория

Наш первое изменение — Чтобы воспользоваться преимуществами программного обеспечения шаблон, который называется шаблон репозитория. Мы будем использовать шаблон репозитория для изоляции наш код доступа к данным от остальной части приложения.

Реализация шаблона репозитория требует выполните следующие два действия:

1. Создайте интерфейс
2. Создать конкретный класс, реализующий интерфейс

Во-первых необходимо создать интерфейс, который описывает все методов доступа к данным, которые необходимо выполнить. Интерфейс IContactManagerRepository содержится в листинге 1. Этот интерфейс описывает пять методов: CreateContact(), DeleteContact(), EditContact(), GetContact и ListContacts().

**Листинг 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Теперь нам нужны для создания конкретного класса, который реализует интерфейс IContactManagerRepository. Так как мы используем Microsoft Entity Framework для доступа к базе данных, мы создадим новый класс с именем EntityContactManagerRepository. Этот класс содержится в списке 2.

**Листинг 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Обратите внимание, что класс EntityContactManagerRepository реализует интерфейс IContactManagerRepository. Класс реализует все пять методов, описанных в этом интерфейсе.

Вы спросите, почему мы должны использоваться в интерфейсе. Зачем нам нужно создать интерфейс и класс, реализующий его?

За одним исключением остальная часть нашего приложения будет взаимодействовать с интерфейс и не конкретного класса. Вместо того чтобы вызывать методы, предоставляемые классом EntityContactManagerRepository, мы назовем методы, предоставляемые интерфейсом IContactManagerRepository.

Таким образом, мы реализовали интерфейсе с помощью нового класса без изменения оставшуюся часть нашего приложения. Например в будущем, может потребоваться для реализации класса, реализующего интерфейс IContactManagerRepository DataServicesContactManagerRepository. Класс DataServicesContactManagerRepository может использовать службы данных ADO.NET для доступа к базе данных вместо Microsoft Entity Framework.

Если интерфейс IContactManagerRepository вместо конкретного класса EntityContactManagerRepository запрограммированы наш код приложения мы можем переключиться конкретные классы без изменения любого из остальная часть нашего кода. Например мы можем переключиться из класса EntityContactManagerRepository DataServicesContactManagerRepository класса без изменения логику доступа или проверки данных.

Программирование реакции на интерфейсов (абстракций), а не конкретные классы делает более устойчивым, чтобы изменить нашего приложения.

> [!NOTE] 
> 
> Можно быстро создать интерфейс из конкретного класса в среде Visual Studio, выбрав параметр меню рефакторинга "Извлечение интерфейса". Например можно сначала создать класс EntityContactManagerRepository и затем использовать извлечение интерфейса для автоматического создания интерфейса IContactManagerRepository.


## <a name="using-the-dependency-injection-software-design-pattern"></a>С помощью шаблона разработки программного обеспечения внедрения зависимостей

Теперь, когда переходе наш код доступа к данным в отдельный класс репозитория, нам нужно изменить нашей контакт контроллера с помощью этого класса. Мы будет использовать шаблон программного обеспечения, который называется внедрение зависимостей в нашем контроллера с помощью класса репозитория.

Измененный контакт контроллера содержится в списке 3.

**Листинг 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Обратите внимание, что контроллер контакта в списке 3 имеет два конструктора. Первый конструктор передает конкретный экземпляр интерфейса IContactManagerRepository второй конструктор. Использует класс контроллера контакт *внедрения зависимостей конструктор*.

Один и только на месте, что используется класс EntityContactManagerRepository находится в первый конструктор. В оставшейся части класс использует интерфейс IContactManagerRepository вместо конкретного класса EntityContactManagerRepository.

Это позволяет легко переключаться реализации класса IContactManagerRepository в будущем. Если вы хотите использовать класс DataServicesContactRepository вместо класса EntityContactManagerRepository, просто укажите первый конструктор.

Внедрение зависимостей конструктор также делает очень тестируемых контакт класс контроллера. Модульные тесты можно создать экземпляр контроллера обратитесь в службу, передавая макетной реализации класса IContactManagerRepository. При создании модульных тестов для приложения диспетчера контактов, эта функция внедрения зависимостей будет очень важны для нас в следующей итерации.

> [!NOTE] 
> 
> Если вы хотите полностью отделить класс контроллера контакта из определенной реализации интерфейса IContactManagerRepository затем вы сможете использовать платформы, который поддерживает внедрение зависимостей, например StructureMap или Microsoft Платформа Entity Framework (MEF). Используя преимущества внедрения зависимостей платформы, не требуется ссылаться на конкретный класс в коде.


## <a name="creating-a-service-layer"></a>Создание уровня службы

Вы могли заметить, что логику проверки по-прежнему смешивать с логику контроллера в классе контроллера измененный в списке 3. По этой же причине, что рекомендуется изолировать нашей логики доступа к данным рекомендуется изолировать логику проверки.

Чтобы устранить эту проблему, можно создать отдельную [ *уровень службы*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Уровень службы находится в отдельном слое, вставим нашей контроллером и классы репозитория. Уровень службы содержит бизнес-логики вместе со всеми логику проверки.

ContactManagerService содержится в листинге 4. Он содержит логику проверки от класса controller контакта.

**Листинг 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Обратите внимание, что конструктор ContactManagerService требует ValidationDictionary. Уровень службы взаимодействует с уровнем контроллера через этот ValidationDictionary. Обсуждаются ValidationDictionary в следующем разделе подробно рассмотрена Decorator шаблон.

Кроме того, обратите внимание, что ContactManagerService реализует интерфейс IContactManagerService. Всегда следует стремиться программировать интерфейсами, а не конкретные классы. Другие классы в приложение диспетчера контактов не взаимодействуют с классом ContactManagerService напрямую. Вместо этого за одним исключением, остальная часть приложение диспетчера контактов запрограммированы IContactManagerService интерфейса.

Интерфейс IContactManagerService содержится в листинге 5.

**Список 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Измененный класс контроллера контакт содержится в 6 со списком. Обратите внимание, что контроллера контакт больше не взаимодействует с ContactManager репозитория. Вместо этого контакта контроллер взаимодействует с ContactManager службы. Каждый слой изолирована как можно больше из других слоев.

**Перечисление 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Наше приложение больше не выполняется объекта один принцип ответственности (SRP). Контроллер контакта в список 6 были вырезаны каждые ответственности, отличные от управления потоком выполнения приложения. Вся логика проверки был удален из контроллера контакта и передаче в уровень служб. Вся логика базы данных было помещено в слой репозитория.

## <a name="using-the-decorator-pattern"></a>С помощью шаблона-декоратора

Мы хотим иметь возможность полностью отделить уровень нашей службы из наших уровня контроллера. В принципе мы следует компилировать уровень нашей службы в отдельной сборке из наших уровня контроллера без необходимости добавьте ссылку в приложении MVC.

Тем не менее уровень нашей службы должен быть могут передавать сообщения об ошибках проверки обратно на уровень контроллера. Как включить уровень служб для передачи сообщений об ошибках проверки без взаимозависимость контроллера и уровень службы? Мы можем использовать шаблона разработки программного обеспечения с именем [шаблон Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Контроллер использует ModelStateDictionary, с именем ModelState для представления ошибки проверки. Таким образом может возникнуть искушение для передачи ModelState из уровня контроллера до уровня службы. Тем не менее с помощью ModelState в уровне службы будет зависеть от на уровне службы компонентов платформы ASP.NET MVC. Это может быть неверный потому, что когда-нибудь, может потребоваться использовать уровень службы с помощью приложения WPF, вместо приложения ASP.NET MVC. В этом случае вы согласитесь t требуется сослаться платформа ASP.NET MVC с помощью класса ModelStateDictionary.

Декоратор шаблон позволяет программы-оболочки для существующего класса в новый класс для реализации интерфейса. Наш диспетчера контактов проект включает ModelStateWrapper класса, содержащегося в листинг 7. Класс ModelStateWrapper реализует интерфейс в список 8.

**Листинг 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**8 - Models\Validation\IValidationDictionary.cs со списком**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Если вы не примете просмотрите список 5 вы увидите, уровень службы ContactManager исключительно использует интерфейс IValidationDictionary. Служба ContactManager не зависящее от класса ModelStateDictionary. Создавая службы ContactManager, обратитесь в службу контроллера контроллер создает оболочку для его ModelState следующим образом:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Сводка

В этой итерации мы была не добавить никаких новых функциональных возможностей приложение диспетчера контактов. Цель этой итерации был рефакторинг, чтобы он проще в обслуживании и изменить приложение диспетчера контактов.

Во-первых мы реализовали шаблон разработки программного обеспечения репозитория. Весь код доступа к данным переносятся в отдельный класс ContactManager репозитория.

Мы также изолированное логику проверки из логику контроллера. Мы создали слоя отдельную службу, содержащую все наши код проверки. Уровень контроллер взаимодействует с уровня службы, и уровень службы взаимодействует с уровня репозитория.

Создавая уровень службы, мы воспользовался Decorator шаблон, чтобы локализовать ModelState из наших уровня службы. В нашем уровень службы программировать с интерфейсом IValidationDictionary вместо ModelState.

Наконец мы воспользовался шаблон разработки программного обеспечения с именем шаблона внедрения зависимостей. Этот шаблон позволяет программировать с использованием интерфейсов (абстракций), а не конкретные классы. Реализации шаблона разработки внедрения зависимости также делает код более тестируемых. В следующей итерации мы добавим наш проект модульных тестов.

> [!div class="step-by-step"]
> [Назад](iteration-3-add-form-validation-cs.md)
> [Вперед](iteration-5-create-unit-tests-cs.md)

---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: "Проверка подлинности пользователей с проверкой подлинности Windows (VB) | Документы Microsoft"
author: microsoft
description: "Узнайте, как использовать проверку подлинности Windows в контексте приложения MVC. Вы узнаете, как включение проверки подлинности Windows в рамках приложения web co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: d4b83d99fcf1247d08ce83364cc00e738b6a16c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Проверка подлинности пользователей с проверкой подлинности Windows (Visual Basic)
====================
по [Microsoft](https://github.com/microsoft)

> Узнайте, как использовать проверку подлинности Windows в контексте приложения MVC. Вы узнаете, как включить проверку подлинности Windows в файл веб-конфигурации приложения и настройка проверки подлинности в службах IIS. Наконец вы узнаете, как использовать атрибут [Authorize] для ограничения доступа к действиям контроллера для конкретного пользователей или групп Windows.


Целью данного учебника является объясняется, как вы сможете использовать безопасности, встроенным в службах IIS для пароля защиты представления в MVC-приложениях. Вы узнаете, как разрешить действия контроллера, который должен вызываться только определенным пользователям Windows или пользователей, которые являются членами определенных групп Windows.

С помощью проверки подлинности Windows имеет смысл, если вы создаете внутренний веб-сайт компании (интрасети) и требуется, чтобы пользователи могли использовать их стандартных имен пользователей и паролей при доступе к веб-сайт. При построении к краям с веб-сайта (веб-сайта Internet) рекомендуется использовать проверку подлинности форм.

#### <a name="enabling-windows-authentication"></a>Включение проверки подлинности Windows

При создании нового приложения ASP.NET MVC по умолчанию не включена проверка подлинности Windows. Проверка подлинности форм — тип проверки подлинности по умолчанию включена для приложения MVC. Проверка подлинности Windows необходимо включить, изменив файл веб-конфигурации (web.config) приложения MVC. Найти &lt;проверки подлинности&gt; статьи и изменить его, чтобы использовать Windows вместо проверки подлинности форм следующим образом:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

При включении проверки подлинности Windows, веб-сервер становится ответственным за проверку подлинности пользователей. Как правило существует два различных типов веб-серверов, которые можно использовать при создании и развертывании приложения ASP.NET MVC.

Во-первых при разработке MVC-приложении, используйте веб-сервера разработки ASP.NET, входящих в состав Visual Studio. По умолчанию веб-сервера разработки ASP.NET выполняет все страницы в контексте текущей учетной записи Windows (учетная запись независимо от используемой для входа в Windows).

Веб-сервера разработки ASP.NET также поддерживает проверку подлинности NTLM. Можно включить проверку подлинности NTLM, щелкнув правой кнопкой мыши имя проекта в окне обозревателя решений и выбрав пункт Свойства. Затем перейдите на вкладку Web и установите флажок NTLM (см. рис. 1).

**Рис. 1 – Включение проверки подлинности NTLM для веб-сервера разработки ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Производственного веб-приложения стороны, можно использовать IIS как веб-сервера. IIS поддерживает несколько типов проверки подлинности, в том числе:

- Обычная проверка подлинности — определяется как часть протокола HTTP 1.0. Передает имена пользователей и пароли в виде открытого текста (в Base64-кодировке) через Интернет. -Дайджест-проверка подлинности — отправке хэш пароля, вместо самой пароля через Интернет. -Интегрированные Windows (NTLM) проверка подлинности — наилучший тип проверки подлинности для использования в интрасети с помощью windows. -Сертификатов проверки подлинности — включена проверка подлинности с использованием клиентского сертификата. Сертификат сопоставляется с учетной записью пользователя Windows.

> [!NOTE] 
> 
> Более подробные сведения различных типов проверки подлинности см. в разделе [https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx).


Для включения проверки подлинности определенного типа можно использовать диспетчер служб IIS. Имейте в виду, что все типы проверки подлинности недоступны в случае каждой операционной системы. Кроме того Если используется IIS версии 7.0 в Windows Vista, необходимо включить различные типы проверки подлинности Windows, прежде чем они появятся в диспетчере служб IIS. Откройте **панели управления программы, программы и компоненты, включение или отключение компонентов**и разверните узел Internet Information Services (см. рис. 2).

**Рис. 2 – функции Включение Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

С помощью служб IIS, можно включить или отключить разные типы проверки подлинности. Например на рис. 3 показаны отключить анонимную проверку подлинности и включение интеграции Windows (NTLM) проверку подлинности при использовании IIS 7.0.

**Рис. 3 – Включение встроенной проверки подлинности Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Авторизация Windows пользователей и групп

После включения проверки подлинности Windows, можно использовать &lt;авторизовать&gt; атрибут для управления доступом к контроллеров или действий контроллера. Этот атрибут может применяться для всего контроллер MVC или действия конкретном контроллере.

Например контроллер Home в листинге 1 предоставляет три действия с именем Index(), CompanySecrets() и StephenSecrets(). Любой пользователь может вызвать Index() действие. Однако только члены локальной группы Windows диспетчеры можно вызвать действие CompanySecrets(). Наконец только пользователей домена Windows с именем Стивен (в домене Redmond) можно вызвать действие StephenSecrets().

**Перечисление Controllers\HomeController.vb. 1.**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Из-за Windows (Учетных записей), при работе с Windows Vista или Windows Server 2008, в локальную группу администраторов будет вести себя иначе, чем другие группы. &lt;Авторизовать&gt; атрибута не может распознать правильно является членом локальной группы администраторов, если не изменить параметры контроля учетных Записей компьютера.


Точно что произойдет при попытке вызова действия контроллера, не имея необходимых разрешений зависит от типа проверки подлинности включена. По умолчанию, при использовании ASP.NET Development Server просто получить пустую страницу. Запрашиваемая страница будет отображена с **неавторизованных 401** состояния HTTP-ответа.

Если, с другой стороны, используются службы IIS с отключена анонимная проверка подлинности и включена обычная проверка подлинности, а затем продолжает появляться диалоговое окно входа в систему при каждом запросе страницы защищенного (см. рис. 4).

**Рис. 4 – диалоговое окно входа обычной проверки подлинности**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Сводка

Этот учебник описано, как можно использовать проверку подлинности Windows в контексте приложения ASP.NET MVC. Вы узнали, как включить проверку подлинности Windows в файл веб-конфигурации приложения и настройка проверки подлинности в службах IIS. Наконец, вы узнали, как использовать &lt;авторизовать&gt; атрибута для ограничения доступа к действиям контроллера для конкретного пользователей или групп Windows.

>[!div class="step-by-step"]
[Назад](authenticating-users-with-forms-authentication-vb.md)
[Вперед](preventing-javascript-injection-attacks-vb.md)
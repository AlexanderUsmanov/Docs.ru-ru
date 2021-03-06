---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Основы безопасности и поддержкой ASP.NET (C#) | Документы Microsoft
author: rick-anderson
description: Это в первом учебнике серию учебников, которые будут рассмотрены приемы для проверки подлинности посетителей через веб-формы авторизации доступа к partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b89a8b0dd88505c1d63054db508590c26684158
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="security-basics-and-aspnet-support-c"></a>Основы безопасности и поддержкой ASP.NET (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Это в первом учебнике серию учебников, которые будут рассмотрены приемы посетители через веб-формы проверки подлинности, авторизации доступа к определенной страницы и функциональность и управление учетными записями пользователей в приложении ASP.NET.


## <a name="introduction"></a>Вступление

Что такое единственное форумы, узлы электронной коммерции, веб-сайтов веб-почту, веб-сайты портала и сайты социальных сетей, которые имеют общие Все они предлагают *учетные записи пользователей*. Сайты, которые содержат учетные записи пользователей необходимо предоставить несколько служб. Как минимум новый посетителей необходимо иметь возможность создать учетную запись и возвращение посетители должен иметь возможность войти. Такие веб-приложения можно принять решение исходя пользователь: некоторые страницы или действия может быть ограничен только войти в пользователям или к подмножеству пользователей. другие страницы может показывать сведения о конкретных пользователю, вошедшему в систему, или может показывать больше или меньше информации в зависимости от того, какой пользователь просматривает страницы.

Это в первом учебнике серию учебников, которые будут рассмотрены приемы посетители через веб-формы проверки подлинности, авторизации доступа к определенной страницы и функциональность и управление учетными записями пользователей в приложении ASP.NET. В ходе этих учебников мы рассмотрим, как:

- Определите и входа пользователей на веб-сайт
- С помощью ASP. В .NET framework членства управление учетными записями пользователей
- Создание, обновление и удаление учетных записей пользователей
- Ограничить доступ к веб-странице, каталога или определенных функций на основе пользователя, вошедшего в систему
- С помощью ASP. В .NET framework ролей для связывания учетных записей пользователей с ролями
- Управление ролями пользователей
- Ограничить доступ к веб-странице, каталога или определенных функций на основе роли пользователя, вошедшего в систему
- Настроить и расширить ASP. В NET безопасности веб-элементов управления

Эти учебники предназначены для краткого и содержат пошаговые инструкции с множеством снимков экранов, дающих процесс визуально. В каждом учебнике доступен в C# и Visual Basic версии и загрузить полный код, используемый. (Первый учебный курс рассматриваются основные понятия безопасности с точки зрения высокого уровня и поэтому любой связанный код не содержит).

В этом учебнике мы обсудим важные вопросы безопасности и какие возможности доступны в ASP.NET, чтобы помочь в реализации форм проверки подлинности, авторизации, учетные записи пользователей и ролей. Давайте начнем!

> [!NOTE]
> Безопасность является важным аспектом любого приложения, которое охватывает физические, технологические и политики решения и требуется высокий уровень планирования и доменов базы знаний. Этот учебник ряд не предназначен как руководство для разработки безопасных веб-приложений. Вместо этого оно посвящено специально форм проверки подлинности, авторизации, учетные записи пользователей и ролей. Хотя некоторые основные понятия безопасности, вращение решения этих проблем рассматриваются в этой серии, другие остаются другую.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Проверка подлинности, авторизации, учетные записи пользователей и ролей

Проверки подлинности, авторизации, учетные записи пользователей и роли находятся четыре условия, которые будут использоваться очень часто на протяжении этого учебника ряда, так как теперь пора быстрого определить эти термины в контексте безопасности веб. В модель клиент сервер, например в Интернете существует множество сценариев, в которых этот сервер должен идентифицировать клиента, выполняющего запрос. *Проверка подлинности* — это процесс, проверка подлинности клиента. Клиент, который успешно обнаружена считается *с проверкой подлинности*. Клиент не были идентифицированы считается *без проверки подлинности* или *анонимный*.

Системы безопасной проверки подлинности используют хотя бы один из следующих трех аспектов: [известные вам сведения, имеющееся у вас оборудование или что-то являются](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Большинство веб-приложений полагаться на то, что клиент узнает, такие как пароль или ПИН-код. Сведения, используемые для идентификации пользователя - своего имени пользователя и пароля, например - называются *учетные данные*. Этого учебника посвящены *проверки подлинности форм*, который представляет собой модель проверки подлинности, где пользователи входят на сайт, указав свои учетные данные в виде веб-страницы. Мы все испытывать этого типа проверки подлинности, прежде чем. Перейдите на сайт электронной коммерции. Когда будете готовы выполнить извлечение вам будет предложено войти, введя имя пользователя и пароль в текстовые поля на веб-странице.

Помимо определения клиентов, сервер может потребоваться ограничить, какие ресурсы и функциональные возможности доступны в зависимости от клиента, выполняющего запрос. *Авторизация* — это процесс определения ли определенный пользователь имеет право доступа к определенному ресурсу или функции.

Объект *учетной записи пользователя* хранилище для сохранения информации о конкретном пользователе. Учетные записи пользователей должен включать как минимум сведений, который уникально идентифицирует пользователя, такие как имя входа пользователя и пароль. Вместе с эту важную информацию, учетные записи пользователей могут включать такие: адрес электронной почты пользователя; Дата и время создания учетной записи; Дата и время последнего входа в систему. имя и фамилия; номер телефона; и почтовый адрес. При использовании проверки подлинности форм, учетная запись пользователя обычно хранятся в реляционной базе данных, например Microsoft SQL Server.

Веб-приложений, поддерживающих учетные записи пользователей, при необходимости может группировать пользователей в *ролей*. Роль — это просто метку, которая применяется к пользователю и предоставляет абстракцию для определения правил авторизации и функциональные возможности уровня страницы. Например веб-сайт может включать роль администратора с помощью правил авторизации, которые запрещают всем, кроме администратора для доступа к определенной группы веб-страниц. Кроме того множество страниц, которые доступны для всех пользователей (включая администраторов) может отображать дополнительные данные или обеспечивают дополнительные функциональные возможности при посещении пользователями в роли "Администраторы". С помощью ролей, можно определить эти правила авторизации на роли в роли отдельно, а не пользователя по.

## <a name="authenticating-users-in-an-aspnet-application"></a>Проверка подлинности пользователей в приложении ASP.NET

Когда пользователь вводит URL-адрес в адрес окно браузера или переходов по ссылке, браузер отправляет [протокола передачи гипертекста (HTTP)](http://en.wikipedia.org/wiki/HTTP) запроса на веб-сервер для заданного содержимого, будь то ASP.NET страницы изображение JavaScript файл, или любого другого типа содержимого. Веб-сервере назначены возврат запрошенного содержимого. При этом он должен определить ряд вопросов о запросе, в том числе автора запроса и является ли удостоверение предоставляет права на получение запрошенного содержимого.

По умолчанию, браузеры отправлять HTTP-запросы, в которых не хватает каких-либо идентификационные данные. Но если браузер включают сведения о проверке подлинности веб-сервер запускает рабочий процесс проверки подлинности, которая используется для идентификации клиента, выполняющего запрос. Действия рабочего процесса проверки подлинности зависят от типа проверки подлинности, используемый веб-приложения. ASP.NET поддерживает три типа проверки подлинности: Windows, службы Passport и формы. Этого учебника посвящены форм проверки подлинности, но давайте минуты для сравнения и сравните хранилищ пользователя для проверки подлинности Windows и рабочего процесса.

### <a name="authentication-via-windows-authentication"></a>Проверка подлинности посредством проверки подлинности Windows

Рабочий процесс проверки подлинности Windows использует один из следующих способов проверки подлинности.

- Обычная аутентификация
- Дайджест-проверка подлинности
- Встроенная проверка подлинности Windows

Все три метода работы примерно таким же образом: Если несанкционированного анонимный запрос, в веб-сервер отправляет HTTP-ответа, которое указывает, что авторизации является обязательным для продолжения. Затем браузер отображает модальное диалоговое окно, окно с приглашением ввести имя пользователя и пароль (см. рис. 1). Эти сведения затем отправляется обратно в веб-сервере через заголовка HTTP.


![Модального диалогового окна запросит у пользователя его учетные данные](security-basics-and-asp-net-support-cs/_static/image1.png)

**Рис. 1**: модального диалогового окна запросит у пользователя его учетные данные


Предоставленные учетные данные проверяются относительно хранилища пользователя веб-сервера Windows. Это означает, что каждого пользователя, прошедшего проверку подлинности в веб-приложении необходимо иметь учетную запись Windows в вашей организации. Это связано с обычными в интрасети. Фактически при использовании встроенной проверки подлинности Windows в среде интрасети, браузер автоматически предоставляет веб-сервере учетные данные, используемые для входа в сеть, тем самым подавляя окно показано на рисунке 1. Проверка подлинности Windows отлично подходит для приложений в интрасети, обычно представляется возможным для веб-приложений с момента создания учетных записей Windows для каждого пользователя, зарегистрировавшийся на сайте не требуется.

### <a name="authentication-via-forms-authentication"></a>Проверка подлинности посредством проверки подлинности форм

Формы проверки подлинности, с другой стороны, идеально подходит для Интернета веб-приложений. Напомним, что проверки подлинности форм определяет пользователя, предлагает пользователю ввести учетные данные через веб-форму. Таким образом когда пользователь пытается получить доступ к ресурсу несанкционированного, они автоматически перенаправляются на страницу входа, где они могут ввести свои учетные данные. Отправленный учетные данные затем проверяются относительно хранилищу пользователей - обычно базы данных.

После проверки того, отправленной учетные данные, *подлинности форм* создается для пользователя. Этот билет означает, что пользователь прошел проверку подлинности и включает в себя идентификационные данные, такие как имя пользователя. Билет проверки подлинности форм (обычно) сохраняется как файл cookie на клиентском компьютере. Последующих посещениях веб-сайта, поэтому включать билета проверки подлинности форм в HTTP-запроса, благодаря чему веб-приложения для идентификации пользователя, когда они вошли.

На рисунке 2 показан процесс проверки подлинности форм из точки vantage высокого уровня. Обратите внимание на то, как компоненты проверки подлинности и авторизации в ASP.NET выступать в качестве двумя отдельными сущностями. Система проверки подлинности форм определяет пользователя (или сообщает, что анонимный). Система авторизации является то, что определяет, имеет ли пользователь доступ к запрошенному ресурсу. Если пользователь не имеет прав (как при попытке анонимно посетите ProtectedPage.aspx, они находятся на рис. 2), система авторизации сообщает, что пользователь отклонил, вызывая форм проверки подлинности системы автоматически перенаправлять пользователя на страницу входа.

Когда пользователь успешно выполнил вход, последующие HTTP-запросы включают билета проверки подлинности форм. Система проверки подлинности форм просто определяет пользователя — это система авторизации, которое определяет, может ли пользователь доступ к запрошенному ресурсу.


![Рабочий процесс проверки подлинности форм](security-basics-and-asp-net-support-cs/_static/image2.png)

**На рисунке 2**: рабочий процесс проверки подлинности форм


Будет подробно проверки подлинности форм гораздо более подробно в следующих учебниках[Общие сведения для проверки подлинности форм](an-overview-of-forms-authentication-cs.md) и [настройки проверки подлинности форм и дополнительные разделы](forms-authentication-configuration-and-advanced-topics-cs.md). Для получения дополнительных сведений об ASP. Параметры проверки подлинности в сети, в разделе [проверки подлинности ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Ограничение доступа к веб-страниц, каталогов и функциональных возможностей страницы

ASP.NET содержит два способа определить, является ли определенный пользователь имеет право на доступ к определенного файла или каталога:

- **Авторизация файлов** — поскольку страниц ASP.NET и веб-службы реализованы как файлы, которые находятся в файловой системе веб-сервере, доступ к этим файлам можно указать с помощью списков управления доступом (ACL). Авторизация файла чаще всего используется с проверкой подлинности Windows, так как ACL представляют собой разрешения, которые применяются к учетным записям Windows. При использовании проверки подлинности форм, все запросы системного уровня операционной системы и файл выполняемых учетную запись Windows, независимо от пользователя на узле.
- **Авторизация URL-адреса**-авторизации URL-адрес страницы разработчик указывает правила авторизации в файле Web.config. Эти правила авторизации указать, что пользователям или ролям получают доступ к или Отказано в доступе к определенным страниц или каталогов в приложении.

Авторизация файла и авторизация URL-адреса определить правила авторизации для доступа к определенной страницы ASP.NET или для всех страниц ASP.NET в конкретном каталоге. С помощью этих методов можно настроить ASP.NET, чтобы отклонять запросы на определенную страницу для конкретного пользователя или разрешить доступ к определенным пользователям и запретить доступ для всех остальных случаях. Новые сведения о сценариях, где все пользователи могут получить доступ к странице, но функциональных возможностей страницы зависит от пользователя? Например большое количество сайтов, поддерживающих учетные записи пользователей выполнять страниц отображать различное содержимое или данные для прошедших проверку пользователей и анонимных пользователей. Анонимный пользователь может появиться ссылку для входа на сайт, тогда как прошедший проверку пользователь увидит вместо этого сообщения, например и Добро пожаловать, *Username* вместе со ссылкой, чтобы выйти из системы. Другой пример: отображается при просмотре элемента на узле аукциона различные сведения в зависимости от того, являетесь ли вы bidder или компьютера, auctioning элемента.

Можно сделать такие изменения уровня страницы, декларативно или программно. Для отображения разного содержимого для анонимных прошедших проверку подлинности пользователей, просто перетащите [управления LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) на страницу и ввести его AnonymousTemplate и LoggedInTemplate шаблоны соответствующее содержимое. Кроме того, можно программно определить является ли текущий запрос проходит проверку подлинности, кем является пользователь и какие роли они принадлежат к (если таковые имеются). Эти сведения можно использовать для затем Показать или скрыть столбцы в сетку или панели на странице.

Этот ряд содержит три учебника, ориентированные на авторизации. ***Авторизация на основе пользователя***рассматривается ограничение доступа на страницу или страницы в каталоге для отдельным учетным записям пользователей; ***Авторизации на основе ролей*** просматривает указания правил авторизации для роли уровне; Наконец, ***отображение содержимого на основе — в настоящее время входа в систему пользователя*** учебника более подробно рассматривается изменение определенного содержимое страницы и функциональность на основе пользователя, посетив страницу. Для получения дополнительных сведений об ASP. Параметры авторизации NET элемента, в разделе [авторизации ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Учетные записи пользователей и ролей

ASP. NET проверки подлинности форм предоставляет инфраструктуру для пользователей для входа на сайт и свое состояние прошедшего проверку подлинности, запоминаются посещений страницы. И авторизация URL-адреса предоставляет платформу для ограничения доступа к определенные файлы или папки в приложении ASP.NET. Однако ни один из компонентов предоставляет средства для хранения учетных записей пользователей и управление ролями.

До ASP.NET 2.0 разработчики несут ответственность за создание собственных хранилищ пользователя и роли. Они также находились на обработчик для разработки пользовательских интерфейсов и написание кода для важных пользователя связанные с учетной записью страницы на страницу входа и страницу, чтобы создать новую учетную запись, в частности. Не все платформы встроенной учетной записи ASP.NET, каждый разработчик, реализующий пользователи имели чтобы попасть по его собственному проектные решения на вопросы, например, как сохранять пароли и другие конфиденциальные сведения? и какие правила, следует ли применить относительно длина пароля и надежности?

В настоящее время реализации учетных записей пользователей в приложении ASP.NET очень гораздо проще благодаря *framework членства* и встроенной входа веб-элементов управления. Платформа членства — небольшое число классов в [имен System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) , обеспечивающие функциональные возможности для выполнения задач, связанных с учетной записи пользователя необходимо. Класс ключа в структуре членства является [класс членства](https://msdn.microsoft.com/library/system.web.security.membership.aspx), имеющее методы, такие как:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Платформа членства использует [модель поставщика](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), который разделяет результирующие API платформы членства от его реализации. Это позволяет разработчикам использовать общий API, но позволяет им использовать реализацию в соответствии с потребностями своего приложения. Иными словами класс членства определяет основные функциональные возможности платформы (методы, свойства и события), но фактически не предоставляет каких-либо деталей реализации. Вместо этого методы класса членства вызвать настроенного поставщика, который является то, что выполняет фактическую работу. Например при вызове метода класса членства CreateUser класс членства не сведения о хранилище пользователя. Он не знает, если пользователи находятся в базе данных или XML-файла или другого хранилища. Класс членства проверяет конфигурацию веб-приложения, чтобы определить, какой поставщик делегировать вызов и этого класса поставщик отвечает за фактически Создание новой учетной записи пользователя в хранилище пользователя. На рисунке 3 показано это взаимодействие.

Корпорация Майкрософт предоставляет два класса поставщика членства в .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -реализует API членства в Active Directory и Active Directory Application Mode (ADAM) серверов.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -реализует API членства в базе данных SQL Server.

Этого учебника посвящены исключительно SqlMembershipProvider.


[![Поставщик модели позволяет различных реализаций быть легко подключено в Framework&lt;/ strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Рисунок 03**: поставщик модели позволяет различных реализаций быть легко подключено в Framework ([Просмотр полноразмерное изображение](security-basics-and-asp-net-support-cs/_static/image5.png))


Преимуществом модель поставщика является можно, разработанных корпорацией Майкрософт, сторонние поставщики или отдельных разработчиков и легко подключать framework членства альтернативных реализаций. Например, корпорация Майкрософт выпустила [поставщик членства для баз данных Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Дополнительные сведения о поставщиках членства, см. [Provider Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), которая содержит пошаговое руководство по поставщиков членства, образец настраиваемых поставщиков, более 100 страниц документации на модель поставщика и Полный исходный код встроенные поставщики членства (а именно: ActiveDirectoryMembershipProvider SqlMembershipProvider).

ASP.NET 2.0 также появились framework ролей. Например framework членство в роли framework строится поверх модель поставщика. Выполняется через API-интерфейса [класс ролей](https://msdn.microsoft.com/library/system.web.security.roles.aspx) и .NET Framework поставляется с тремя классами поставщика:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -управляет сведениями о роли в хранилище политик диспетчера авторизации, например Active Directory или ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -реализует роли в базе данных SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -связывает сведения о роли, группе Windows посетителя. Этот метод обычно используется проверка подлинности Windows.

Этого учебника посвящены исключительно SqlRoleProvider.

Поскольку модель поставщика включает единый интерфейс API с выходом вперед (классы членства и ролей), можно реализовать функции вокруг API, который не нужно беспокоиться о подробностях реализации — те, обрабатываются поставщиками, выбранные на странице разработчик. Этот универсальный интерфейс API позволяет корпорации Майкрософт и сторонних поставщиков для построения веб-элементов управления, взаимодействующие с платформами членства и ролей. ASP.NET поставляется вместе с количеством [входа веб-элементы управления](https://msdn.microsoft.com/library/ms178329.aspx) для реализации общих интерфейсов пользователя учетной записи пользователя. Например [входа в систему управления](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) предлагает пользователю ввести свои учетные данные, проверяет их, а затем регистрирует их в посредством проверки подлинности форм. [Управления LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) предлагает шаблоны для отображения различные разметки для анонимных пользователей и проверку подлинности пользователей или другой разметки на основе роли пользователя. И [управления CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) предоставляет пошаговые пользовательский интерфейс для создания новой учетной записи пользователя.

В системе различные элементы управления входом взаимодействовать с платформами членства и ролей. Большинство элементов управления входа может осуществляться без необходимости написания единой строки кода. Рассмотрим эти элементы управления более подробно в будущих учебниках, включая способы расширения и настройки их функциональность.

## <a name="summary"></a>Сводка

Все веб-приложений, поддерживающих учетные записи пользователей требуется аналогичные функциональные возможности, например: возможность пользователям вход и иметь их журнала состояния запоминаются посещений страницы; веб-страницы для новой посетителей создается учетная запись; и возможность разработчик страницы, чтобы указать, какой ресурс, данных и функциональные возможности будут доступны какие пользователям или ролям. Задачи проверки подлинности и авторизации пользователей и управления учетными записями пользователей и ролями значительно упрощается для выполнения в приложениях ASP.NET благодаря форм проверки подлинности и авторизация URL-адреса, а также инфраструктур членства и ролей.

В ходе следующего несколько учебников будут рассмотрены следующие аспекты путем построения поэтапное рабочий веб-приложение с нуля. В следующих двух учебнике мы рассмотрим форм проверки подлинности, подробно. Мы увидим рабочего процесса проверки подлинности форм в действии, проанализируем билета проверки подлинности с помощью форм, рассматриваются вопросы безопасности и узнать, как настроить систему проверки подлинности форм — все при построении веб-приложение, которое позволяет посетителям войти и выйти из системы.

Программирование довольны!

### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [ASP.NET 2.0 членства, ролей, проверки подлинности форм и ресурсы по безопасности](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Рекомендации по ASP.NET 2.0 безопасности](https://msdn.microsoft.com/library/ms998258.aspx)
- [Проверка подлинности ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Проверки подлинности ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Общие сведения об элементах управления входом ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Изучение ASP.NET 2.0 членства, ролей и профиля](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Как I: защитить узел с помощью членства и ролей?](https://asp.net/learn/videos/video-45.aspx) (Видео)
- [Общие сведения о членстве](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Центре разработчиков безопасности MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Профессиональные ASP.NET 2.0 безопасности, членства и управления ролями](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Набор средств поставщика](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было ряда прошел проверку многие полезные рецензентов этого учебника. Основными редакторами этого учебника включают Alicja Maziarz, Джон Suru и Мерфи Тереза д. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](an-overview-of-forms-authentication-cs.md)

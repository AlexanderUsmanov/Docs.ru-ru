---
title: Цель иерархию и Многопользовательские приложения в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о иерархии строки цели и мультитенантностью по отношению к API защиты данных ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: a1ca2c32f95a86b877cbbe94d106d23b86800443
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Цель иерархию и Многопользовательские приложения в ASP.NET Core

Поскольку `IDataProtector` неявно `IDataProtectionProvider`, целей можно связывать друг с другом. В этом смысле `provider.CreateProtector([ "purpose1", "purpose2" ])` эквивалентно `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Это обеспечивает некоторые интересные иерархические связи в системе защиты данных. В предыдущем примере для [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), можно вызвать компонент SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` один раз предварительной и кэширования результата в частном `_myProvide` поля. Будущие предохранители можно создать путем вызова метода `_myProvider.CreateProtector("User: username")`, и эти предохранители ключа, которые будут использоваться для защиты отдельных сообщений.

Это может также отражается. Рассмотрим один логический приложение какие узлы нескольких клиентов (CMS кажется разумным) и каждый клиент можно настроить свою собственную систему управления проверки подлинности и состояние. Зонтик приложение имеет один основной поставщик и вызывает `provider.CreateProtector("Tenant 1")` и `provider.CreateProtector("Tenant 2")` для предоставления каждого клиента собственную изолированной срез системы защиты данных. Клиенты затем создать собственные отдельных предохранители, исходя из своих собственных потребностей, но независимо от того, насколько сильно они пытаются выполнить их не удается создать предохранителей, которые конфликтуют с любым другим клиентом в системе. В графическом виде она представлена как показано ниже.

![Несколько целей аренды](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Это предполагает рамках элементов управления приложения, какие интерфейсы API доступны для отдельных клиентов и клиентов, которое не может выполнить произвольный код на сервере. Если клиент может выполняться произвольный код, они могут выполнять отражение закрытых прервать гарантии изоляции или может только читать материала основного непосредственно и наследовать все подразделы, что пожелает.

Система защиты данных использует своего рода Многопользовательские приложения в конфигурации по умолчанию out of box. По умолчанию материала основного хранится в папке профиля пользователя учетной записи процесса рабочего (или в реестре, для удостоверения пула приложений IIS). Но фактически довольно часто, использование одной учетной записи для выполнения нескольких приложений и таким образом эти приложения появятся общий доступ к хозяину материала. Чтобы устранить эту проблему, система защиты данных автоматически вставляет идентификатор уникальными на уровне приложений как первый элемент в цепочке общей цели. Этой цели неявное служит для [изолировать отдельные приложения](xref:security/data-protection/configuration/overview#per-application-isolation) друг от друга, эффективно рассматривая каждое приложение как уникальный клиента в системе и процесс создания предохранителя выглядит идентична на рисунке выше.

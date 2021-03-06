---
title: "Реализация синхронизации паролей с помощью Azure AD Connect Sync | Документация Майкрософт"
description: "В этой статье объясняется, как работает синхронизация паролей и как ее настроить."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0cb1b04bcfab1f1864ae0ce867be02a8bf8c827c
ms.contentlocale: ru-ru
ms.lasthandoff: 04/12/2017

---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Реализация синхронизации паролей с помощью Azure AD Connect Sync
В этой статье содержатся сведения о том, как синхронизировать пароли пользователей локального экземпляра службы Active Directory (AD) и облачного экземпляра службы Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Что такое синхронизация паролей
Чем больше у вас паролей, которые нужно помнить, тем выше вероятность того, что в какой-то момент из-за забытого пароля работа остановится. Чем больше паролей необходимо помнить, тем выше вероятность забыть один из них. Вопросы и звонки о восстановлении паролей и прочие проблемы, связанные с паролями, забирают больше всего ресурсов службы технической поддержки.

Синхронизация паролей — это функция для синхронизации пользовательских паролей между локальным экземпляром службы Active Directory и облачным экземпляром службы Azure AD.
Эту функцию можно использовать для входа в службы Azure AD, например Office 365, Microsoft Intune, CRM Online и доменные службы Azure Active Directory (Azure AD DS). Вы можете входить в службу с помощью пароля, который используется на локальном экземпляре Active Directory.

![Что такое Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Вместо большого числа паролей, пользователям нужно будет запомнить только один. Синхронизация паролей дает следующие преимущества.

* Повышение производительности пользователей.
* Снижение затрат на техническую поддержку.  

Если вы используете [службу федерации Active Directory (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), можете также настроить синхронизацию паролей. Это даст вам дополнительный резервный механизм на случай сбоя в инфраструктуре AD FS.

Синхронизация паролей — это расширение функции синхронизации каталогов, реализованное при помощи служб синхронизации Azure AD Connect. Для использования синхронизации паролей в определенной среде необходимо выполнение следующих действий.

* Установите Azure AD Connect.  
* Настройте синхронизацию каталогов между локальным экземпляром службы Active Directory и облачным экземпляром Azure Active Directory.
* Включите синхронизацию паролей.

Дополнительные сведения см. в статье [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).

> [!NOTE]
> Дополнительную информацию о настройке доменных служб Azure Active Directory для использования FIPS и синхронизации паролей, см. в разделе "Синхронизация паролей и FIPS" дальше в этой статье.
>
>

## <a name="how-password-synchronization-works"></a>Как работает синхронизация паролей
Доменная служба Active Directory хранит пароли в виде хэш-значений, полученных на основе фактических паролей. Для получения хэш-значений используется необратимая математическая функция (*алгоритм хэширования*). Не существует метода для возврата результата односторонней функции в обычную текстовую версию пароля. Хэш пароля не может использоваться для входа в локальную сеть.

Чтобы синхронизировать пароль, Azure AD Connect Sync извлекает хэш пароля из локального экземпляра Active Directory. Перед синхронизацией в службу проверки подлинности Azure Active Directory хэш пароля дополнительно обрабатывается системой безопасности. Пароли синхронизируются для каждого пользователя отдельно и в хронологическом порядке.

Фактический поток данных в процессе синхронизации паролей аналогичен синхронизации данных пользователей, таких как отображаемые имена или адреса электронной почты. Но синхронизация паролей происходит чаще, чем это обычно делается для других атрибутов. Процесс синхронизации паролей выполняется каждые 2 минуты. Нельзя изменять частоту выполнения этого процесса. При синхронизации пароль перезаписывает существующий в облачной службе.

Когда вы впервые включаете функцию синхронизации паролей, выполняется первоначальная синхронизация паролей для всех пользователей в области действия функции. Вы не можете явно выделить подмножество пользователей, для которых нужно синхронизировать пароли.

При изменении локального пароля новый пароль, как правило, синхронизируется в течение нескольких минут.
Если синхронизация паролей завершается сбоем, автоматически осуществляется новая попытка. Ошибки, возникающие во время синхронизации паролей, записываются в журнал событий.

Синхронизация пароля не влияет на пользователей Azure, находящихся в системе в момент ее выполнения.
Если изменение пароля синхронизируется во время вашего пребывания в облачной службе, оно не применяется в сеансе облачной службы немедленно. Однако, когда снова возникнет необходимость в проверке подлинности в облачной службе, потребуется указать новый пароль.

Для аутентификации в Azure AD пользователь должен вводить корпоративные учетные данные повторно, независимо от того, выполнен ли вход в корпоративную сеть. Но эту проблему можно свести к минимуму, если пользователь при входе в систему установит флажок "Оставаться в системе". Это действие создает для сеанса файл cookie, который на некоторое время позволяет обходить аутентификацию. Администратор Azure Active Directory может включить или отключить функцию "Оставаться в системе".

> [!NOTE]
> Синхронизация паролей поддерживается только для типа объектов user в Active Directory. Она не поддерживается для типа объектов iNetOrgPerson.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Подробное описание принципа действия синхронизации паролей
Ниже подробно описан принцип действия синхронизации паролей между локальным экземпляром Active Directory и Azure Active Directory.

![Подробная схема использования паролей](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Каждые две минуты агент синхронизации паролей на сервере AD Connect запрашивает хэш сохраненных паролей (атрибут unicodePwd) у контроллера домена по стандартному протоколу репликации [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx), используемому для синхронизации данных контроллеров доменов. Для получения хэша паролей учетной записи службы требуются разрешения AD "Репликация изменений каталога" и "Репликация всех изменений каталога" (предоставляется по умолчанию при установке).
2. Перед отправкой контроллер домена шифрует MD4-хэш пароля с помощью ключа — комбинации [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt)-хэша от ключа сеанса RPC и случайных данных. После этого он отправляет результат агенту синхронизации паролей по протоколу RPC. Контроллер домена также передает агенту синхронизации случайные данные, используя протокол репликации контроллера домена, чтобы агент смог расшифровать конверт.
3.  Когда агент синхронизации паролей получает зашифрованный конверт, он создает ключ для расшифровки полученных данных в исходный формат MD4, используя [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) и случайные данные. Агенту синхронизации паролей недоступен пароль в виде открытого текста. Он использует MD5 только для обеспечения совместимости протокола репликации с контроллером домена и только на локальном компьютере между контроллером домена и агентом синхронизации паролей.
4.  Агент синхронизации паролей увеличивает 16-байтовый двоичный хэш пароля до 64 байтов. Сначала он преобразует хэш в 32-байтовую шестнадцатеричную строку, а затем преобразует ее обратно в двоичный файл с кодировкой UTF-16.
5.  Агент синхронизации паролей добавляет 10-байтовые случайные данные к 64-байтовому двоичному файлу, чтобы обеспечить дополнительную защиту исходного хэша.
6.  Затем он объединяет хэш MD4 и случайные данные, а результат передает в функцию [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt). Выполняется 1000 итераций алгоритма хэширования с ключом [HMAC-SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx). 
7.  Агент синхронизации паролей принимает полученный 32-байтовый хэш, присоединяет к нему значение соли и число итераций SHA256 (для использования Azure AD), а затем полученную строку передает из Azure AD Connect в Azure AD по протоколу SSL.</br> 
8.  Когда пользователь вводит пароль для входа в Azure AD, пароль обрабатывается точно так же (MD4, случайные данные, PBKDF2 и HMAC-SHA256). Если полученный хэш совпадает с хэшем, хранящимся в Azure AD, это означает, что пользователь ввел правильный пароль и он проходит аутентификацию. 

>[!Note] 
>Оригинальный MD4-хэш не передается в Azure AD. Вместо этого передается SHA256-хэш от оригинального MD4-хэша. Это означает, что хранимый в Azure AD хэш невозможно использовать для атаки Pass-the-Hash на локальную систему, даже если удастся его получить.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Как работает синхронизация паролей с доменными службами Azure Active Directory
Функцию синхронизации паролей можно использовать для синхронизации локальных паролей с [доменными службами Azure Active Directory](../../active-directory-domain-services/active-directory-ds-overview.md). В этом сценарии доменная служба Azure AD выполняет аутентификацию пользователей в облаке, применяя все методы, доступные в локальном экземпляре Active Directory. Действия в этом сценарии аналогичны использованию инструмента миграции Active Directory Migration Tool (ADMT) в локальной среде.

### <a name="security-considerations"></a>Вопросы безопасности
При синхронизации пароли в текстовом формате не передаются в службу синхронизации паролей, Azure AD или какие-либо другие связанные службы.

Пользователь проходит аутентификацию в Azure AD, а не в корпоративном экземпляре Active Directory. Если в вашей организации строго относятся к передаче данных о пароле за ее пределы в любой форме, учтите, что хранимые в Azure AD данные о пароле (SHA256-хэш от исходного MD4-хэша) имеют существенно более высокий уровень безопасности, чем хранимые в Active Directory данные. Кроме того, так как хэш SHA256 невозможно расшифровать, его невозможно вернуть в корпоративную среду Active Directory и представить в качестве действительного пароля пользователя в ходе атаки Pass-the-Hash.





### <a name="password-policy-considerations"></a>Рекомендации по политикам паролей
Есть два типа политик паролей, которые затрагивает синхронизация паролей:

* политика сложности паролей;
* политика срока действия паролей.

#### <a name="password-complexity-policy"></a>Политика сложности паролей  
Если используется синхронизация паролей, настроенные в локальном экземпляре Active Directory политики сложности паролей переопределяют политики сложности, настроенные в облаке для синхронизированных пользователей. Все допустимые пароли из локального экземпляра Active Directory можно использовать для доступа к службам Azure AD.

> [!NOTE]
> Пароли пользователей, созданные непосредственно в облаке, и впредь регулируются политиками паролей, которые настроены в облаке.

#### <a name="password-expiration-policy"></a>политика срока действия паролей.  
Если пароль пользователя синхронизируется, для пароля облачной учетной записи устанавливается*неограниченный срок действия*.

Даже если срок действия синхронизируемого пароля в локальной среде истек, его можно использовать для входа в облачные службы. Пароль для облачной учетной записи изменяется, когда вы изменяете пароль в локальной среде.

#### <a name="account-expiration"></a>Срок действия учетной записи
Если в вашей организация используется атрибут accountExpires для управления учетными записями пользователей, учтите, что этот атрибут не синхронизируется с Azure AD. В результате учетная запись Active Directory будет всегда активной в Azure AD, даже если срок ее действия истек в той среде, где настроена синхронизация паролей. Мы рекомендуем внедрить рабочий процесс, который при истечении срока действия учетной записи будет активировать сценарий PowerShell, отключающий учетную запись пользователя в Azure AD. И наоборот, при включении учетной записи нужно отдельно включать ее в экземпляре Azure AD.

### <a name="overwrite-synchronized-passwords"></a>Перезапись синхронизированных паролей
Администратор может вручную сбросить пароль с помощью Windows PowerShell.

В таком случае новый пароль перезапишет синхронизированный пароль и к новому паролю будут применены все политики паролей, настроенные в облаке.

Если после этого вы измените локальный пароль, новый пароль синхронизируется в облако и перезапишет пароль, измененный вручную.

Синхронизация пароля не влияет на вошедших в систему пользователей Azure. Если изменение пароля синхронизируется, когда вы вошли в облачную службу, это не повлияет на открытый сеанс связи с облачной службой. Функция "Оставаться в системе" продлевает срок действия такого расхождения. Когда снова возникнет необходимость в аутентификации в облачной службе, потребуется указать новый пароль.

### <a name="additional-advantages"></a>Дополнительные преимущества

- Как правило, синхронизацию паролей реализовать проще, чем службу федерации. Для этого не требуются дополнительные серверы и высокая доступность, которая важна при аутентификации пользователей в службе федерации. 
- Кроме того, синхронизацию паролей можно использовать параллельно с федерацией. Это будет хорошим резервным механизмом на случай сбоя службы федерации.












## <a name="enable-password-synchronization"></a>Включение синхронизации паролей
Если для установки Azure AD Connect вы используете **стандартные параметры**, синхронизация паролей включается по умолчанию. Дополнительные сведения см. в статье [Приступая к работе с Azure AD Connect с использованием стандартных параметров](active-directory-aadconnect-get-started-express.md).

Если для установки Azure AD Connect вы используете пользовательские параметры, синхронизацию паролей нужно включить на странице входа в систему. Дополнительные сведения см. в статье [Выборочная установка Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Включение синхронизации паролей](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Синхронизация паролей и FIPS
Если сервер блокируется в соответствии с федеральным стандартом обработки информации (FIPS), схема MD5 отключается.

**Чтобы включить MD5 для синхронизации паролей выполните следующие действия.**

1. Перейдите к папке %programfiles%\Azure AD Sync\Bin.
2. Откройте файл miiserver.exe.config.
3. Перейдите к узлу конфигурации или среды выполнения (в конце файла конфигурации).
4. Добавьте следующий узел: `<enforceFIPSPolicy enabled="false"/>`
5. Сохраните изменения.

Этот фрагмент должен выглядеть примерно так:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Дополнительные сведения о безопасности и FIPS см. в записи блога [AAD Password Sync, Encryption and FIPS compliance](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/) (Синхронизация паролей, шифрование и соответствие FIPS в AAD).

## <a name="troubleshoot-password-synchronization"></a>Устранение неполадок с синхронизацией паролей
При возникновении проблем с синхронизацией паролей см. рекомендации в статье [Устранение неполадок синхронизации паролей с помощью службы синхронизации Azure AD Connect](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Дальнейшие действия
* [Службы синхронизации Azure AD Connect: общие сведений о синхронизации и ее настройка](active-directory-aadconnectsync-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)


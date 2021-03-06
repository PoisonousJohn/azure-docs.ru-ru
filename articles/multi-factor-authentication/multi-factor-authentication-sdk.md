---
title: "Пакет средств разработки программного обеспечения MFA для пользовательских приложений | Документация Майкрософт"
description: "В этой статье описывается, как скачать и использовать пакет SDK для Azure MFA, чтобы включить двухфакторную проверку подлинности для пользовательских приложений."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 281f9c61a30a20027f69808600373aa272255ef6
ms.contentlocale: ru-ru
ms.lasthandoff: 05/15/2017


---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Построение Multi-Factor Authentication в пользовательских приложениях (SDK)

Пакет средств разработки программного обеспечения (SDK) для многофакторной идентификации Azure позволяет встроить процедуру двухфакторной проверки подлинности прямо в процессы входа или обработки транзакции приложений в клиенте Azure AD.

Пакет SDK для многофакторной идентификации доступен для C#, Visual Basic (.NET), Java, Perl, PHP и Ruby. Пакет SDK предоставляет тонкую оболочку для двухфакторной проверки подлинности. Он включает в себя все необходимое для написания кода, в том числе закомментированные файлы исходного кода, файлы примеров и подробный файл ReadMe. Каждый пакет SDK также содержит сертификат и закрытый ключ для шифрования транзакций, уникальный для поставщика многофакторной идентификации. До тех пор пока у вас есть поставщик, пакет SDK можно загружать на всех необходимых языках и во всех требуемых форматах.

API-интерфейсы в пакете SDK для многофакторной идентификации имеют простую структуру. Вы отправляете один вызов функции в API-интерфейс с многофакторными параметрами, например режимом проверки, и данными пользователя, такими как телефонный номер для отправки вызова или ПИН-код для проверки. API-интерфейсы преобразуют вызов функции в запросы веб-служб, отправляемые в облачную службу Azure Multi-Factor Authentication. Все вызовы должны содержать ссылку на закрытый сертификат, который имеется в каждом пакете SDK.

Так как API-интерфейсы не имеют доступа к пользователям, зарегистрированным в Azure Active Directory, необходимо предоставить сведения о пользователе в файле или базе данных. Кроме того, API-интерфейсы не предоставляют функции управления регистрацией или пользователями, поэтому эти процессы необходимо встроить в приложение.

> [!IMPORTANT]
> Если вы хотите скачать пакет SDK, необходимо создать поставщик многофакторной проверки подлинности Azure, даже если у вас уже есть лицензии Azure MFA, AAD Premium или EMS. Если вы решили создать для этой цели поставщик многофакторной проверки подлинности Azure и у вас уже есть лицензии, не забудьте создать поставщик на базе модели **На включенного пользователя**. Свяжите поставщик с каталогом, содержащим лицензии Azure MFA, Azure AD Premium или EMS. При такой конфигурации с вас будет взиматься плата, только если количество уникальных пользователей пакета SDK превысит количество лицензий, которыми вы владеете.


## <a name="download-the-sdk"></a>Скачивание пакета SDK
Для скачивания пакета SDK для Azure Multi-Factor Authentication требуется [поставщик Azure Multi-Factor Authentication](multi-factor-authentication-get-started-auth-provider.md).  Для этого нужна полная версия подписки Azure, даже если у вас уже есть лицензии Azure MFA, AAD Premium или Enterprise Mobility Suite.  Чтобы скачать пакет SDK, перейдите на портал управления многофакторной идентификацией. Это можно сделать напрямую с помощью поставщика многофакторной проверки подлинности или щелкнув ссылку **Перейти на портал** на странице параметров службы MFA.

### <a name="download-from-the-azure-classic-portal"></a>Скачивание с классического портала Azure
1. Войдите на [классический портал Azure](https://manage.windowsazure.com) с учетной записью администратора.
2. Выберите **Active Directory**слева.
3. На странице Active Directory вверху выберите вкладку **Поставщики многофакторной проверки подлинности**.
4. Щелкните **Управление** внизу. Откроется новая страница.
5. Слева внизу щелкните **Пакет SDK**.
   <center>![Загрузить](./media/multi-factor-authentication-sdk/download.png)</center>
6. Выберите необходимый язык и щелкните одну из связанных ссылок для скачивания.
7. Сохраните загружаемый файл.

### <a name="download-from-the-service-settings"></a>Скачивание из параметров службы
1. Войдите на [классический портал Azure](https://manage.windowsazure.com) с учетной записью администратора.
2. Выберите **Active Directory**слева.
3. Дважды щелкните свой экземпляр Azure AD.
4. В верхней части экрана щелкните ссылку **Настроить**
5. В разделе многофакторной идентификации выберите **Управление параметрами службы**
   .![Скачивание](./media/multi-factor-authentication-sdk/download2.png)
6. На странице параметров службы в нижней части экрана щелкните ссылку **Перейти на портал**. Откроется новая страница.
   ![Загрузить](./media/multi-factor-authentication-sdk/download3a.png)
7. Слева внизу щелкните **Пакет SDK**.
8. Выберите необходимый язык и щелкните одну из связанных ссылок для скачивания.
9. Сохраните загружаемый файл.

## <a name="whats-in-the-sdk"></a>Что включает в себя пакет SDK
Пакет SDK содержит следующие элементы:

* **README**. В этом файле объясняется, как использовать API-интерфейсы для Multi-Factor Authentication в новом или существующем приложении.
* **Исходные файлы** для Многофакторной идентификации
* **Сертификат клиента** , используемый для взаимодействия со службой Multi-Factor Authentication
* **Закрытый ключ** для сертификата
* **Результаты вызова.** Список кодов результатов вызова. Чтобы открыть этот файл, используйте приложение с возможностями форматирования текста, например WordPad. Для проверки и устранения неполадок при реализации процесса Multi-Factor Authentication в приложении используйте коды результатов вызова. Они отличаются от кодов состояния проверки подлинности.
* **Примеры.** Пример кода для базовой рабочей реализации процесса Multi-Factor Authentication.

> [!WARNING]
> Сертификат клиента — это уникальный закрытый сертификат, созданный специально для вас. Не показывайте никому и не теряйте этот файл. Это ваш ключ для обеспечения безопасности вашего взаимодействия со службой Multi-Factor Authentication.

## <a name="code-sample"></a>Пример кода
В этом примере кода показано, как использовать API-интерфейсы в пакете SDK для Azure Multi-Factor Authentication для добавления в приложение проверки голосового вызова в стандартном режиме. Стандартным режимом является ситуация, при которой для ответа на телефонный вызов пользователь нажимает кнопку #.

В этом примере используется пакет SDK для многофакторной идентификации для C# .NET 2.0 в базовом приложении ASP.NET с серверной логикой, написанной на C#, но данный процесс похож на реализацию на других языках. Так как пакет SDK содержит не исполняемые, а исходные файлы, можно построить файлы и ссылаться на них или включить их непосредственно в свое приложение.

> [!NOTE]
> При реализации службы многофакторной идентификации используйте дополнительные методы (телефонный звонок или текстовые сообщения), такие как вторичную и третичную проверку, в дополнение к основному способу проверки подлинности (имя пользователя и пароль). Эти методы не предназначены для использования в качестве основных способов проверки подлинности.

### <a name="code-sample-overview"></a>Обзор примера кода
В этом примере кода для простого демонстрационного веб-приложения используется телефонный вызов, для ответа на который следует нажимать клавишу #, чтобы выполнить проверку подлинности пользователя. В рамках Multi-Factor Authentication фактор телефонного вызова называется стандартным режимом.

В клиентском коде нет никаких элементов, относящихся к Multi-Factor Authentication. Поскольку дополнительные факторы проверки подлинности не зависят от основной проверки подлинности, их можно добавлять без изменения существующего интерфейса входа. API-интерфейсы в пакете SDK для Multi-Factor Authentication позволяют настраивать взаимодействие с пользователем, но, может быть, менять ничего не будет нужно.

Серверный код добавляет проверку подлинности в стандартном режиме на шаге 2. Он создает объект PfAuthParams с параметрами, необходимыми для проверки в стандартном режиме: имя пользователя, телефонный номер, режим и путь к сертификату клиента (CertFilePath), который требуется в каждом вызове. Демонстрацию всех параметров в PfAuthParams см. в файле примера в пакете SDK.

Затем код передает объект PfAuthParams в функцию pf_authenticate(). Возвращаемое значение указывает на успешное или неудачное выполнение проверки подлинности. Выходные параметры (callStatus и код ошибки) содержат дополнительные сведения о результатах вызова. Коды результатов вызова описаны в файле результатов вызова в пакете SDK.

Такая минимальная реализация может занимать несколько строк. Однако в готовый код следует включить более сложную обработку ошибок, дополнительный код базы данных и улучшенный пользовательский интерфейс.

### <a name="web-client-code"></a>Код веб-клиента
Ниже приведен код веб-клиента для демонстрационной страницы.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Серверный код
В следующем серверном коде служба Multi-Factor Authentication настраивается и запускается на шаге 2. Стандартный режим (MODE_STANDARD) — это телефонный вызов, для ответа на который пользователь нажимает клавишу #.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }


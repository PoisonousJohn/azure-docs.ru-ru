# Обзор
## [Что такое служба автоматизации Azure?](automation-intro.md)
# Начало работы
## [Начало работы со службой автоматизации Azure](automation-offering-get-started.md)
## Руководство по модулям Runbook
### [Создание графического модуля Runbook](automation-first-runbook-graphical.md)
### [Создание модуля Runbook PowerShell](automation-first-runbook-textual-powershell.md)
### [Создание модуля Runbook рабочего процесса PowerShell](automation-first-runbook-textual.md)
# Практическое руководство
## Проверка подлинности и безопасность
### [Создание автономной учетной записи службы автоматизации](automation-create-standalone-account.md)
### [Создание учетной записи пользователя Azure AD](automation-create-aduser-account.md)
### [Настройка проверки подлинности с помощью AWS](automation-config-aws-account.md)
### [Создание учетной записи запуска от имени](automation-create-runas-account.md)
### [Проверка настройки учетной записи службы автоматизации](automation-verify-runas-authentication.md)
### [Управление доступом на основе ролей](automation-role-based-access-control.md)
### [Управление учетной записью службы автоматизации](automation-manage-account.md)
## Создание модулей Runbook
### [Типы Runbook](automation-runbook-types.md)
### [Создание и импорт модулей Runbook](automation-creating-importing-runbook.md)
### [Изменение текстовых модулей Runbook](automation-edit-textual-runbook.md)
### [Изменение графических модулей Runbook](automation-graphical-authoring-intro.md)
### [Тестирование модуля Runbook](automation-testing-runbook.md)
### [Изучение рабочего процесса PowerShell](automation-powershell-workflow.md)
### [Дочерние модули Runbook](automation-child-runbooks.md)
### [Выходные данные Runbook](automation-runbook-output-and-messages.md)
### [Интеграция системы управления версиями](automation-source-control-integration.md)
## Автоматизация
### [Запуск модуля Runbook](automation-starting-a-runbook.md)
### [Запуск модуля Runbook с помощью webhook](automation-webhooks.md)
### [Настройка входных параметров Runbook](automation-runbook-input-parameters.md)
### [Обработка ошибок в графических модулях Runbook](automation-runbook-graphical-error-handling.md)
### [Отслеживание задания Runbook](automation-runbook-execution.md)
### [Изменение параметров Runbook](automation-runbook-settings.md)
### [Управление данными службы автоматизации Azure](automation-managing-data.md)
### [Вызов модуля Runbook службы автоматизации Azure из оповещения Log Analytics](automation-invoke-runbook-from-omsla-alert.md)
### [Передача объекта JSON в модуль runbook службы автоматизации Azure](automation-pass-json-string.md)
## гибридный компонент Runbook Worker
### [Развертывание гибридной рабочей роли Runbook](automation-hybrid-runbook-worker.md)
### [Запуск модулей Runbook в рабочей роли](automation-hrw-run-runbooks.md)
## Развертывание управления конфигурацией (DSC)
### [Общие сведения о настройке требуемого состояния (DSC)](automation-dsc-overview.md)
### [Приступая к работе](automation-dsc-getting-started.md)
### [Подключение виртуальных машин для управления](automation-dsc-onboarding.md)
### [Компиляция конфигураций DSC](automation-dsc-compile.md)
### [Непрерывное развертывание с помощью Chocolatey](automation-dsc-cd-chocolatey.md)
### [Пересылка данных отчетов Azure Automation DSC в OMS Log Analytics](automation-dsc-diagnostics.md)
## Управление активами
### [Сертификаты](automation-certificates.md)
### [Подключения](automation-connections.md)
### [Учетные данные](automation-credentials.md)
### [Модули интеграции](automation-integration-modules.md)
### [Расписания](automation-schedules.md)
### [Переменные](automation-variables.md)
### [Обновление модулей Azure PowerShell](automation-update-azure-modules.md)
## Сценарии
### [Коллекция Runbook](automation-runbook-gallery.md)
### [Создание виртуальной машины Amazon Web Service](automation-scenario-aws-deployment.md)
### [Исправление неполадок с помощью оповещения о виртуальной машине Azure](automation-azure-vm-alert-integration.md)
### [Запуск и остановка виртуальной машины с помощью тегов JSON](automation-scenario-start-stop-vm-wjson-tags.md)
### [Удаление группы ресурсов](automation-scenario-remove-resourcegroup.md)
### [Интеграция системы управления версиями с помощью GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md)
### [Интеграция системы управления версиями с помощью VSTS](automation-scenario-source-control-integration-with-VSTS.md)
### [Вызов модуля Runbook службы автоматизации Azure из оповещения Log Analytics](automation-invoke-runbook-from-omsla-alert.md)
### [Развертывание шаблона Azure Resource Manager в модуле Runbook PowerShell в службе автоматизации Azure](automation-deploy-template-runbook.md)
## Решения
### [Отслеживание изменений](../log-analytics/log-analytics-change-tracking.md)
### [Управление обновлениями](../operations-management-suite/oms-solution-update-management.md)
### [Запуск и остановка виртуальных машин в нерабочее время](automation-solution-vm-management.md)
## Монитор
### [Пересылка данных задания службы автоматизации Azure в Log Analytics](automation-manage-send-joblogs-log-analytics.md)
### [Отмена привязки учетной записи службы автоматизации Azure к Log Analytics](automation-unlink-from-log-analytics.md)
## Миграция
### [Перенос из Orchestrator](automation-orchestrator-migration.md)
### [Перемещение учетной записи службы автоматизации](automation-migrate-account-subscription.md)
## Устранение неполадок
### [Устранение распространенных ошибок](automation-troubleshooting-automation-errors.md)
### [Устранение неполадок в гибридной рабочей роли Runbook](automation-troubleshooting-hybrid-runbook-worker.md)
# Справочные материалы
## [PowerShell](/powershell/module/azurerm.automation)
## [PowerShell (классическая модель)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.azure.management.automation)
## [REST](/rest/api/automation)
## [REST (классическая модель)](https://msdn.microsoft.com/library/azure/mt163781)
# Ресурсы
## [Вводный видеоролик о службе автоматизации](https://azure.microsoft.com/documentation/videos/azure-automation-101-with-powershell-and-eamon-o-reilly/)
## [Обучение работе со службой автоматизации Azure](https://mva.microsoft.com/en-US/training-courses/automating-the-cloud-with-azure-automation-8323?l=C6mIpCay_4804984382)
## [Стратегия развития Azure](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Схема обучения](https://azure.microsoft.com/documentation/learning-paths/automation/)
## [Форум MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azureautomation)  
## [Цены](https://azure.microsoft.com/pricing/details/automation/)  
## [Калькулятор цен](https://azure.microsoft.com/pricing/calculator/)
## [Заметки о выпуске](https://azure.microsoft.com/updates/?product=automation)
## [Обновления службы](https://azure.microsoft.com/updates/?product=automation)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-automation)
## [Видеоролики](https://azure.microsoft.com/documentation/videos/index/?services=automation)

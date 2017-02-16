<properties 
pageTitle="Cloud-Dienst Lebenszyklusereignisse behandeln | Microsoft Azure" 
description="Erfahren Sie, wie die Methoden Lebenszyklus einer Rolle der Cloud-Dienst in .NET verwendet werden können" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Anpassen des Lebenszyklus von einer Website oder Arbeitskollegen Rolle in .NET

Beim Erstellen einer Worker-Rolle, erweitern Sie die [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) Klasse bereitstellt, dass Methoden für Sie außer Kraft setzen, mit die Sie können auf Lebenszyklusereignisse reagieren. Diese Klasse ist für Webrollen optional,, sodass Sie diese auf Lebenszyklusereignisse reagieren verwenden müssen.

## <a name="extend-the-roleentrypoint-class"></a>Erweitern Sie die RoleEntryPoint-Klasse

Die [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) -Klasse enthält Methoden, die von Azure aufgerufen werden Wenn es **beginnt**, **ausgeführt wird**, oder **Tabstopps** einer Rolle Web oder Arbeitskollegen. Optional können Sie diese Methoden zum Verwalten von Rolle Initialisierung, Rolle war(en) folgen oder Ausführungsthread der Rolle überschreiben. 

Wenn Sie **RoleEntryPoint**erweitern, sollten Sie eines der folgenden Probleme der Methoden bekannt sein:

-   Die Methoden [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) und [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) boolesche Wert zurückgibt, sodass es möglich ist, die von diesen Methoden **false** zurück.

     Code **falsch**zurück, wird der Rolle Prozess plötzlich beendet ohne Ausführen einer beliebigen war(en) Sequenz direkte mögliche. Im Allgemeinen sollten Sie vermeiden, aus der **OnStart** -Methode **false** zurückgegeben.
     
-   Innerhalb einer Überladung der **RoleEntryPoint** Ausnahme nicht abgefangene wird als Ausnahmefehler behandelt.

     Wenn eine Ausnahme innerhalb einer der Methoden Lebenszyklus auftritt, Azure löst das Ereignis [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) , und klicken Sie dann der Vorgang beendet. Nachdem Sie Ihre Rolle im Offlinemodus in Anspruch genommen wurde, wird sie durch Azure gestartet. Wenn Ausnahmefehler auftritt, das Ereignis [beendet](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) wird nicht ausgelöst, und die **OnStop** -Methode wird nicht aufgerufen.

Wenn Ihre Rolle nicht gestartet oder zwischen Initialisierung beschäftigt, und Beenden des Staaten Wiederverwendung ist, wird der Code möglicherweise Ausnahmefehler innerhalb eines der Lebenszyklusereignisse auslösen werden jedes Mal neu gestartet, die Rolle wird. Verwenden Sie das Ereignis [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) in diesem Fall die Ursache der Ausnahme zu ermitteln und es entsprechend behandeln. Ihre Rolle möglicherweise auch aus der Methode [Ausführen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zurückgeben werden, wodurch die Rolle, neu zu starten. Weitere Informationen zur Bereitstellung Staaten finden Sie unter [Allgemeine Probleme die Ursache Rollen an Papierkorb](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Wenn Sie die **Azure-Tools für Microsoft Visual Studio** -Entwicklung Ihrer Anwendung verwenden, erweitern die Projektvorlagen Rolle die Klasse **RoleEntryPoint** für Sie in den Dateien *WebRole.cs* und *WorkerRole.cs* .

## <a name="onstart-method"></a>OnStart-Methode

Die **OnStart** wird aufgerufen, wenn Ihre Rolleninstanz von Azure online ist. Während der OnStart Code und ausgeführt wird die Instanz der Rolle als **gebucht** markiert ist, und keine externen Datenverkehr zu es durch den Lastenausgleich geleitet. Sie können diese Methode, um die Initialisierung arbeiten, z. B. Ereignishandler implementieren, und beginnen Sie [Azure-Diagnose](cloud-services-how-to-monitor.md)ausführen überschreiben.

**OnStart** **true**zurück, wird die Instanz erfolgreich und Azure Ruft die Methode **RoleEntryPoint.Run** . **OnStart** **falsch**zurück, beendet die Rolle sofort, ohne dass Sie alle geplanten war(en) folgen ausgeführt.

Im folgenden Code wird gezeigt, wie die **OnStart** -Methode außer Kraft setzen. Diese Methode konfiguriert und einen Diagnoseprotokollen Monitor geöffnet wird, wenn die Instanz der Rolle wird gestartet, und eine Datenübertragung vom Protokollierung mit einem Speicherkonto richtet:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop-Methode

Die **OnStop** wird aufgerufen, nachdem eine Instanz der Rolle von Azure offline geschaltet wurde und vor der Prozess beendet wird. Sie können diese Methode, um für Ihre Rolleninstanz ordnungsgemäß beendet erforderlichen Code anrufen überschreiben.

> [AZURE.IMPORTANT] Code, der in der **OnStop** -Methode ausgeführt hat eine begrenzte Zeit in Anspruch, wenn sie als ein manueller war(en) Gründen aufgerufen wird. Nach Ablauf dieser Zeitspanne, wird der Prozess beendet, damit dieser Code in die **OnStop** Methode können schnell ausführen müssen Sie sicherstellen oder verträglich nicht vollständig ausgeführt. Die **OnStop** wird aufgerufen, nachdem das **Beenden** -Ereignis ausgelöst wurde.


## <a name="run-method"></a>Run-Methode

Sie können die Methode **Ausführen** , um einen Thread langer für Ihre Rolleninstanz implementieren außer Kraft setzen.

Überschreiben der Methode **Ausführen** ist nicht erforderlich. die standardmäßige Implementierung wird einen Thread, der für immer ruht gestartet. Wenn Sie die Methode **Run** überschreiben, sollte der Code endlos blockieren. Wenn die Methode **Ausführen** zurückgibt, wird die Rolle automatisch ordnungsgemäß wieder verwendet; Kurzum, Azure löst das Ereignis **Beenden** und ruft die **OnStop** -Methode aus, damit Ihre war(en) folgen ausgeführt werden können, bevor die Rolle Offlineschalten.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Implementierung der ASP.NET Lebenszyklus Methoden für eine Webrolle

Die ASP.NET Lebenszyklus Methoden, neben den in der Klasse **RoleEntryPoint** können Initialisierung und war(en) folgen für eine Webrolle verwalten. Dies kann für die Kompatibilität nützlich sein, wenn Sie eine vorhandene ASP.NET-Anwendung in Azure portieren. Die ASP.NET Lebenszyklus Methoden werden aus der **RoleEntryPoint** Methoden aufgerufen. Die **Anwendung\_starten** wird aufgerufen, nachdem der **RoleEntryPoint.OnStart** Methode abgeschlossen ist. Die **Anwendung\_Ende** Methode wird aufgerufen, bevor die Methode **RoleEntryPoint.OnStop** aufgerufen wird.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie [ein Cloud-Service-Paket erstellt](cloud-services-model-and-package.md).
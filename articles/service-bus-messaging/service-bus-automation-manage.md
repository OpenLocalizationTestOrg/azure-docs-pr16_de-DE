<properties
    pageTitle="Verwalten Azure Service Bus mit Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit der Automatisierung Azure Service Azure-Dienstbus verwalten."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Verwalten von Azure Service Bus mit Azure Automatisierung

Mit diesem Leitfaden wird eine Einführung in die Automatisierung Azure Service und wie sie verwendet werden kann, um die Verwaltung von Azure Service Bus zu vereinfachen.

## <a name="what-is-azure-automation"></a>Was ist Automatisierung Azure?

[Azure Automatisierung](../automation/automation-intro.md) ist ein Azure Service zur Vereinfachung der Cloud Management durch die Automatisierung von Geschäftsprozessen und gewünschten Statuskonfiguration. Mithilfe der Azure Automatisierung, manuelle, können wiederholt, langer und fehlerhaften automatisierte Aufgaben werden zum Zuverlässigkeit, Effizienz und Zeit Wert für Ihre Organisation zu erhöhen.

Azure Automatisierung bietet eine hochgradig zuverlässigen, hoch verfügbaren Workflow Execution-Engine, die Ihren Anforderungen skaliert. Bei der Azure-Automatisierung können Prozesse manuell durch 3rd Drittanbietern Systeme oder in bestimmten Intervallen gestartet, sodass die Vorgänge erfolgen genau bei Bedarf.

Reduzieren von Verwaltungsaufwand und frei IT und DevOps Mitarbeiter auf Arbeit konzentrieren, die Business Wert hinzufügt, indem Sie Ihre Cloud-Verwaltungsaufgaben von Azure Automatisierung automatisch ausgeführt werden.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Wie kann Azure Automatisierung zum Azure-Dienstbus verwalten?

Sie können mithilfe der [Dienst Bus REST-API](https://msdn.microsoft.com/library/azure/mt639375.aspx)Dienstbus mit Azure Automatisierung verwalten. In Azure Automatisierung können Sie PowerShell Skripts so viele Dienstbus Aufgaben mithilfe der REST-API ausführen. Sie können auch diese REST-API-Aufrufe in Azure Automatisierung mit den-Cmdlets für andere Dienste Azure zum Automatisieren von komplexer Aufgaben über Azure Services und 3rd Party Systeme verbinden.

Hier sind einige Beispiele für die Verwendung der PowerShell Azure-Dienstbus verwalten:

* [Benutzerdefinierte PowerShell-Cmdlets zum Verwalten von Azure Servicebuswarteschlangen](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [So erstellen Sie Dienstbus Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skript](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Erstellen von Azure Service Bus Namespaces mithilfe der PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Das PowerShell-Modul für die Arbeit mit Azure Dienstbus in Automatisierung Runbooks kann aus dem [Katalog PowerShell](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0)heruntergeladen werden.


## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie haben gelernt, dass die Grundlagen von Azure Automatisierung und wie sie zum Verwalten von Azure Service Bus verwendet werden kann, führen Sie die folgenden Links, um weitere Informationen zur Azure-Automatisierung.

* Finden Sie unter [Erste Schritte abschließen](../automation/automation-first-runbook-graphical.md) Azure Automatisierung
* Finden Sie unter Verwalten von [Dienstbus mit PowerShell](service-bus-powershell-how-to-provision.md)

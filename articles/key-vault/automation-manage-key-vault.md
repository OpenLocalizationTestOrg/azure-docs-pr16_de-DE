<properties
    pageTitle="Verwalten Azure-Taste Tresor mit Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, wie der Dienst Azure Automatisierung zur Azure-Taste Tresor verwalten."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Verwalten von Azure-Taste Tresor mit Azure Automatisierung

Mit diesem Leitfaden wird eine Einführung in die Automatisierung Azure-Dienst und wie sie verwendet werden kann, um die Verwaltung von Tasten und vertrauliche Informationen in Azure-Taste Tresor zu vereinfachen.

## <a name="what-is-azure-automation"></a>Was ist Automatisierung Azure?

[Azure Automatisierung](../automation/automation-intro.md) ist ein Azure Service zur Vereinfachung der Cloud Management durch die Automatisierung von Geschäftsprozessen und gewünschten Statuskonfiguration. Mithilfe der Azure Automatisierung, manuelle, können wiederholt, langer und fehlerhaften automatisierte Aufgaben werden zum Zuverlässigkeit, Effizienz und Zeit Wert für Ihre Organisation zu erhöhen.

Azure Automatisierung bietet eine hochgradig zuverlässigen, hoch verfügbaren Workflow Execution-Engine, die Ihren Anforderungen skaliert. Bei der Azure-Automatisierung können Prozesse manuell durch 3rd Drittanbietern Systeme oder in bestimmten Intervallen gestartet, sodass die Vorgänge erfolgen genau bei Bedarf.

Reduzieren von Verwaltungsaufwand und frei IT und DevOps Mitarbeiter auf Arbeit konzentrieren, die Business Wert hinzufügt, indem Sie Ihre Cloud-Verwaltungsaufgaben von Azure Automatisierung automatisch ausgeführt werden.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Wie kann Azure Automation unterstützen Azure-Taste Tresor verwalten?

Schlüssel Tresor in Azure Automatisierung mithilfe von [AzureRM Schlüssel Tresor Cmdlets] verwaltet werden können (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) und [Azure klassischen Schlüssel Tresor Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). Das Azure-Modul für die Verwaltung von klassischen Schlüssel Tresor steht automatisch in Azure Automatisierung, und Sie können das [Modul AzureRM-KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) in Azure Automatisierung, importieren, so, dass Sie viele der Schlüssel Tresor Verwaltungsaufgaben innerhalb des Diensts ausführen können. Sie können auch diese Cmdlets in Azure Automatisierung mit den-Cmdlets für andere Dienste Azure zum Automatisieren von komplexer Aufgaben über Azure Services und 3rd Party Systeme verbinden.

Mit den Azure-Taste Tresor Cmdlets können Sie diese Aufgaben unter anderem ausführen: 

- Erstellen und Konfigurieren einer Key Tresor
- Erstellen Sie oder importieren Sie einen Schlüssel
- Erstellen Sie oder aktualisieren Sie einen geheimen Schlüssel
- Aktualisieren Sie die Attribute eines Schlüssels
- Erhalten von Schlüssel geheim
- Löschen eines Schlüssels oder geheim

Hier sind einige Beispiele für die Verwendung der PowerShell-Taste Tresor verwalten:  

* [Azure Key Tresor - Schritt-für-Schritt](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Einrichten und Konfigurieren einer Azure Key Tresor](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie haben gelernt, dass die Grundlagen von Azure Automation und wie sie zum Verwalten von Azure-Taste Tresor verwendet werden kann, führen Sie die folgenden Links, um weitere Informationen zur Azure Automation.

* Finden Sie unter [Erste Schritte abschließen](../automation/automation-first-runbook-graphical.md)Azure Automation.
* Finden Sie unter der [Azure Schlüssel Tresor PowerShell-Skripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

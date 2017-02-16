<properties
    pageTitle="Verwalten von Azure SQL-Datenbanken mit Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, wie der Dienst Azure Automatisierung zur bei SQL Azure-Datenbanken verwalten."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Verwalten von Azure Automatisierung Azure SQL-Datenbanken

Mit diesem Leitfaden wird eine Einführung in die Automatisierung Azure Service und wie sie verwendet werden kann, um die Verwaltung von Ihrem SQL Azure-Datenbanken zu vereinfachen.


## <a name="what-is-azure-automation"></a>Was ist Automatisierung Azure?

[Azure Automatisierung](https://azure.microsoft.com/services/automation/) ist ein Azure Service zur Vereinfachung der Cloud Management durch die Automatisierung von Geschäftsprozessen. Mit der Azure Automatisierung können langer, manuelle, fehlerhaften und häufig wiederholte automatisierte Aufgaben werden zum Zuverlässigkeit, Effizienz und Zeit Wert für Ihre Organisation zu erhöhen.

Azure Automatisierung bietet eine hochgradig zuverlässig und hoch verfügbaren Workflow Execution-Engine, die Ihren Anforderungen, wenn Ihre Organisation wächst skaliert. Bei der Azure-Automatisierung können Prozesse manuell durch 3rd Drittanbietern Systeme oder in bestimmten Intervallen gestartet, sodass die Vorgänge erfolgen genau bei Bedarf.

Tieferzustufen Verwaltungsaufwand und Freigeben von IT / DevOps Mitarbeiter auf Arbeit konzentrieren, die addiert Business Wert durch Verschieben der Cloud-Verwaltungsaufgaben von Azure Automatisierung automatisch ausgeführt werden.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Wie kann Azure Automatisierung unterstützen SQL Azure-Datenbanken verwalten?

Azure SQL-Datenbank kann in Azure Automatisierung verwaltet werden, mithilfe der [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/dn546723.aspx) , die in den [Azure PowerShell-Tools](https://msdn.microsoft.com/library/azure/jj156055.aspx)verfügbar sind. Azure Automatisierung weist diese Azure SQL-Datenbank PowerShell-Cmdlets verfügbar von im Feld, damit Sie alle Ihre SQL DB Verwaltungsaufgaben innerhalb des Diensts durchführen können. Sie können auch diese Cmdlets in Azure Automatisierung mit den-Cmdlets für andere Dienste Azure zum Automatisieren von komplexer Aufgaben über Azure Services und 3rd Party Systeme verbinden.

Azure Automatisierung kann auch direkt mit SQL Server kommunizieren, indem Sie mithilfe der PowerShell SQL-Befehle.

Der [Azure Automatisierung Runbooks Katalog](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) enthält eine Vielzahl von Produkt-Team und Community Runbooks für den Einstieg in die Verwaltung von Azure SQL-Datenbanken, andere Azure Services und 3rd Party Systeme automatisieren. Katalog Runbooks umfassen:

 * [Ausführen von SQL-Abfragen in einer SQL Server-Datenbank](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Vertikal skalieren (nach oben oder unten) einer Azure SQL-Datenbank auf einem Zeitplan](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Kürzen einer SQL-Tabelle aus, wenn die Datenbank die maximale Größe erreicht](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Indizieren Sie Tabellen in einer SQL Azure-Datenbank aus, wenn diese hochfragmentiert](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie haben gelernt, dass die Grundlagen von Azure Automatisierung und wie sie SQL Azure-Datenbanken verwalten verwendet werden kann, führen Sie die folgenden Links, um weitere Informationen zur Azure-Automatisierung.

- [Azure Automatisierung (Übersicht)](../automation/automation-intro.md)
- [Meine erste Runbooks](../automation/automation-first-runbook-graphical.md)
- [Azure Automatisierung Learning Karte](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure Automatisierung: Der SQL-Agent in der Cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 

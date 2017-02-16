<properties
    pageTitle="Neuigkeiten Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, welcher Wert Azure Automatisierung bietet, und erhalten Sie Antworten auf häufig gestellte Fragen, damit Sie beim Erstellen, loslegen können Runbooks und Azure Automatisierung DSC verwenden."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Was ist Automatisierung, Azure Automatisierung, Azure Automatisierungsbeispiele"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure Automatisierung (Übersicht)

Microsoft Azure Automatisierung bietet eine Möglichkeit für Benutzer zum Automatisieren von der manuellen, langer, fehlerhaften und häufig wiederholten Aufgaben, die häufig in einer Cloud und Enterprise-Umgebung ausgeführt werden. Sie Zeit sparen und erhöht die Zuverlässigkeit von regulären Verwaltungsaufgaben und plant auch diese in regelmäßigen Abständen automatisch ausgeführt werden sollen. Können Sie mithilfe des Runbooks Prozesse automatisieren oder Konfigurationsmanagement mit gewünscht Zustand Konfiguration automatisieren. In diesem Artikel Azure Automatisierung kurze Übersicht und finden Sie Antworten auf einige häufig gestellte Fragen. Sie können auf Weitere Artikel in dieser Bibliothek Ausführlichere Informationen zu den verschiedenen Themen verweisen.


## <a name="automating-processes-with-runbooks"></a>Automatisieren von Geschäftsprozessen mithilfe von runbooks

Eine Runbooks ist eine Reihe von Aufgaben, die einen automatisierten Prozess in Azure Automatisierung ausführen. Es ist ein einfacher Vorgang wie das Starten eines virtuellen Computers und erstellen einen Eintrag möglicherweise oder möglicherweise eine komplexe Runbooks, die kombiniert andere kleinere Runbooks, um einen komplexen Prozess über mehrere Ressourcen oder sogar mehrere Wolken und auf lokale Umgebungen ausführen müssen.  

Angenommen, Sie möglicherweise haben eine vorhandene manuellen Prozesses für Abschneiden einer SQL-Datenbank aus, wenn es bald maximale Größe, die mehrere Schritte erreicht wird wie Herstellen einer Verbindung mit dem Server, umfasst Herstellen einer Verbindung mit der Datenbank, Abrufen die aktuelle Größe der Datenbank, prüfen, ob Schwellenwert überschritten hat und klicken Sie dann abgeschnitten wird und Benutzer benachrichtigen. Anstelle manuell Durchführung der einzelnen Schritte, können Sie eine Runbooks erstellen, die alle diese Aufgaben als einzelne Prozess ausführen möchten. Klicken Sie zunächst des Runbooks, geben Sie die erforderlichen Informationen wie den SQL-Servernamen, Datenbanknamen und Empfänger e-Mail- und befinden sich dann wieder, während der Vorgang abgeschlossen ist. 


## <a name="what-can-runbooks-automate"></a>Was kann Runbooks automatisieren?

Runbooks in Azure Automatisierung basieren auf Windows PowerShell oder Windows PowerShell-Workflow, damit alles wie, die PowerShell ausführen können. Wenn eine Anwendung oder einen bestimmten Dienst eine API verfügt, kann ein Runbooks mit arbeiten. Wenn Sie ein PowerShell-Modul für die Anwendung verfügen, können Sie dieses Modul in Azure Automatisierung laden und diese Cmdlets in Ihrem Runbooks enthalten. Azure Automatisierung Runbooks führen Sie in der Cloud Azure und können Zugriff auf Cloudressourcen oder externer Ressourcen, die aus der Cloud zugegriffen werden können. Verwenden [Hybrid Runbooks Worker](automation-hybrid-runbook-worker.md), kann Runbooks in Ihrem lokalen Datencenter zum Verwalten von lokaler Ressourcen ausgeführt werden. 


## <a name="getting-runbooks-from-the-community"></a>Abrufen von Runbooks aus der community

Klicken Sie im [Katalog Runbooks](automation-runbook-gallery.md#runbooks-in-runbook-gallery) enthält Runbooks von Microsoft und die Community, Sie entweder mithilfe können in Ihrer Umgebung unverändert bei, oder passen Sie diese für eigene Zwecke. Sie sind auch als Verweise auf Weitere Informationen zum Erstellen Ihrer eigenen Runbooks hilfreich. Sie können auch eigene Runbooks zum Katalog "" beitragen, dass Sie denken, dass andere Benutzer nützlich sein könnten. 


## <a name="creating-runbooks-with-azure-automation"></a>Erstellen von Runbooks mit Azure Automatisierung 

Sie können ganz neu [Erstellen eigener Runbooks](automation-creating-importing-runbook.md) oder aus dem [Katalog Runbooks](http://msdn.microsoft.com/library/azure/dn781422.aspx) für Ihren eigenen Anforderungen Runbooks ändern. Es gibt drei verschiedene [Typen von Runbooks](automation-runbook-types.md) auf, die Sie auswählen können basierend auf Ihren Anforderungen und PowerShell-Oberfläche aus. Wenn Sie direkt mit PowerShell-Code arbeiten lieber, können Sie verwenden eines [PowerShell Runbooks](automation-runbook-types.md#powershell-runbooks) oder [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) , die Sie offline oder mit dem [Text-Editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) im Azure-Portal bearbeiten. Wenn Sie eine Runbooks bearbeiten lieber, ohne die zugrunde liegenden Code verfügbar gemacht werden, können Sie eine [grafisch Runbooks](automation-runbook-types.md#graphical-runbooks) mit dem [Editor grafisch](automation-graphical-authoring-intro.md) in Azure-Portal erstellen. 

Möchten Sie lieber auf Lesebereich heraus? Sehen Sie sich die unter Video aus Microsoft Ignite Sitzung im Mai 2015. Hinweis: Während die Konzepte und Features, die in diesem Video wird erläutert, korrekt sind, Azure Automatisierung sehr fortgeschritten ist, da in diesem Video aufgezeichnet wurde, es jetzt weist eine etwas ausführlichere Benutzeroberfläche Azure-Portal und zusätzliche Funktionen unterstützt.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Verwalten der Konfiguration mit Zustand Konfiguration gewünschte automatisieren 

[DSC PowerShell](https://technet.microsoft.com/library/dn249912.aspx) ist eine Management-Plattform, die es ermöglicht zu verwalten, bereitstellen und Konfiguration für physischen Hosts und virtuellen Computern mithilfe eines PowerShell-Deklarationssyntax erzwingen. Sie können Konfigurationen auf einem zentralen DSC extrahieren Server definieren, die Zielcomputern automatisch abrufen und anwenden können. DSC bietet eine Reihe von PowerShell-Cmdlets zum Verwalten von Konfigurationen und Ressourcen verwendet werden können.  

[Azure Automatisierung DSC](automation-dsc-overview.md) ist eine cloudbasierte Lösung für PowerShell DSC, die Dienste für Enterprise-Umgebungen erforderlich bereitstellt.  Sie können Ihre Ressourcen DSC Azure Automatisierung verwalten und Konfigurationen auf virtuellen oder physischen Maschinen, die sie von einem DSC extrahieren Server in der Cloud Azure ab anwenden.  Darüber hinaus reporting Services, die zu informieren, dass Sie über wichtige Ereignisse wie, wenn Knoten aus ihrer zugeordneten Konfiguration abwichen haben und eine neue Konfiguration angewendet wurde. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Erstellen eigener DSC Konfigurationen mit Azure Automatisierung

Geben Sie den gewünschten Status eines Knotens [DSC Konfigurationen](automation-dsc-overview.md#azure-automation-dsc-terms) an.  Mehrere Knoten können die gleiche Konfiguration, um sicherzustellen, dass sie alle einen identischen Status verwalten anwenden.  Sie können erstellen eine Konfiguration mit einem beliebigen Text-Editor auf dem lokalen Computer und importieren sie dann in der Azure-Automatisierung, wo können kompilieren, und Knoten anwenden.


## <a name="getting-modules-and-configurations"></a>Erste Module und Konfigurationen 

Sie erhalten [PowerShell-Module](automation-runbook-gallery.md#modules-in-powershell-gallery) mit Cmdlets, die Sie in Ihren Runbooks und DSC Konfigurationen aus dem [Katalog PowerShell](http://www.powershellgallery.com/)verwenden können. Sie können diesem Katalog vom Azure-Portal starten und Module direkt in Azure Automatisierung importieren oder können Sie herunterladen und manuell zu importieren. Sie können keine Module direkt vom Azure-Portal installieren, jedoch können Sie sie herunterladen wie bei einem beliebigen anderen Modul zu installieren. 


## <a name="example-practical-applications-of-azure-automation"></a>Beispiel der Praxis der Azure-Automatisierung 

Es folgen ein paar Beispiele, was die Arten von Automatisierungsszenarien mit Azure Automatisierung sind. 

* Erstellen und Kopieren von virtuellen Computern in verschiedenen Azure-Abonnements. 
* Löschen Sie die Datei, die zu einem Container Azure BLOB-Speicher von einem lokalen Computer kopiert werden. 
* Automatisieren Sie Sicherheitsfunktionen, die beispielsweise Verweigern von einem Client anfordert, wenn ein Dienstverweigerungsangriff erkannt wird. 
* Stellen Sie sicher, dass Computer ständig mit konfigurierten Sicherheitsrichtlinie ausrichten.
* Verwalten von fortlaufender Bereitstellung von Anwendungscode über Cloud und auf lokale Infrastruktur. 
* Erstellen von Active Directory-Struktur in Azure für Ihre Umgebung Übung. 
* Kürzen Sie eine Tabelle in einer SQL-Datenbank, wenn DB maximale Größe ansteht. 
* Aktualisieren Sie Remote-, die umgebungseinstellungen für eine Azure-Website ein. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Wie bezieht sich Azure Automatisierung für andere Automatisierungstools?

Automatisieren von Verwaltungsaufgaben in der private Cloud [Service Management Automatisierung (SMA)](http://technet.microsoft.com/library/dn469260.aspx) soll. Es wird in der Mitte der Daten lokal als Komponente von [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/)installiert. SMA unterstützt keine [grafischen Runbooks](automation-graphical-authoring-intro.md)SMA und Azure-Automatisierung verwenden das gleiche Runbooks Format ausgehend von Windows PowerShell und Windows PowerShell-Workflow.  

Automatisierung der lokalen Ressourcen [System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) gelten. Es verwendet eine andere Runbooks Format als Azure-Automatisierung und Service Management Automatisierung und verfügt über eine Benutzeroberfläche Runbooks erstellen, ohne dass alle scripting. Seine Runbooks bestehen Aktivitäten von Integration Packs, die speziell für Orchestrator geschrieben werden. 


## <a name="where-can-i-get-more-information"></a>Wo erhalte ich weitere Informationen? 

Eine Vielzahl von Ressourcen stehen Ihnen weitere Informationen über die Azure Automatisierung und erstellen Ihre eigenen Runbooks. 

* **Azure Automatisierungsbibliothek** ist, wo Sie sich gerade befinden. Die Artikel in dieser Bibliothek enthalten vollständige Dokumentation zur Konfiguration und Verwaltung der Azure Automatisierung und für die Erstellung eigener Runbooks. 
* [Azure-PowerShell-Cmdlets](http://msdn.microsoft.com/library/jj156055.aspx) stellt zum Automatisieren von Azure Vorgänge mithilfe der Windows PowerShell. Runbooks Verwenden dieser Cmdlets für die Arbeit mit Azure Ressourcen. 
* [Management Blog](https://azure.microsoft.com/blog/tag/azure-automation/) bietet die neueste Informationen über Azure Automatisierung und andere Technologien für die Verwaltung von Microsoft. Sie sollten diesen Blog, um mit den neuesten aus dem Team Azure Automatisierung auf dem neuesten Stand zu bleiben abonnieren. 
* [Automatisierung Forum](http://go.microsoft.com/fwlink/p/?LinkId=390561) können Sie Fragen zur Azure-Automatisierung mit Microsoft und der Community Automatisierung adressiert werden Posten. 
* [Azure Automatisierung Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx) enthält Informationen zum Automatisieren von Verwaltungsaufgaben. Enthaltenen Cmdlets zum Verwalten von Konten Automatisierung, Anlagen, Runbooks, DSC.


## <a name="can-i-provide-feedback"></a>Kann ich Feedback Geben? 

**Bitte geben Sie uns Feedback!** Wenn Sie eine Azure Automatisierung Runbooks Lösung oder ein Integrationsmodul suchen, veröffentlichen Sie eine Skript anfordern Script Center. Wenn Sie für die Automatisierung Azure Feedback oder Feature Anfragen haben, veröffentlichen Sie diese auf [Benutzer Voicemail](http://feedback.windowsazure.com/forums/34192--general-feedback). Vielen Dank! 



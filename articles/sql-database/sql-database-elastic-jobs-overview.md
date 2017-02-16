<properties
    pageTitle="Verwalten von skalierten Clouddatenbanken | Microsoft Azure" 
    description="Flexible Datenbank Auftragsdienst veranschaulicht" 
    metaKeywords="azure sql database elastic databases" 
    services="sql-database" 
    documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="managing-scaled-out-cloud-databases"></a>Verwalten von skalierten Clouddatenbanken

Zum Verwalten von skalierten sharded Datenbanken kann das Feature **flexible Datenbank Aufträge** (Preview) Sie zuverlässig Ausführen eines Transact-SQL (T-SQL)-Skripts oder Anwenden einer DACPAC ([Anwendung auf Datenebene](https://msdn.microsoft.com/library/ee210546.aspx)) über eine Gruppe von Datenbanken, einschließlich:

* eine benutzerdefinierte Auflistung von Datenbanken (siehe unten)
* alle Datenbanken in einer [Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md)
* ein Satz von Shard (mit [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md)erstellt). 
 
## <a name="documentation"></a>Dokumentation

* [Die flexible Auftrag Datenbankkomponenten installieren](sql-database-elastic-jobs-service-installation.md). 
* [Erste Schritte mit flexible Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md).
* [Erstellen und Verwalten von Aufträgen mithilfe der PowerShell](sql-database-elastic-jobs-powershell.md).
* [Erstellen und Verwalten von skalierten, Azure SQL-Datenbanken](sql-database-elastic-jobs-getting-started.md)

**Flexible Datenbank Aufträge** gibt es zurzeit einem Kunden gehostete Azure Cloud-Dienst, die ermöglicht die Ausführung von Ad-hoc- und geplante Verwaltungsaufgaben, die **Aufträge**bezeichnet werden. Bei Projekten können einfach und zuverlässig große Gruppen von Azure SQL-Datenbanken durch Ausführen der Transact-SQL-Skripts administrative Vorgänge durchführen verwalten. 

![Flexible Datenbank Auftrag service][1]

## <a name="why-use-jobs"></a>Gründe für das Verwenden von Projekten

**Verwalten**

Einfaches führen Schema ändert sich, Verwaltung von Anmeldeinformationen, Verweis Datenupdates, Sammlung von Daten oder Mandanten (Customer) werden Websitesammlung.

**Berichte**

Aggregieren von Daten aus einer Sammlung von Azure SQL-Datenbanken in einer einzelnen Zieltabelle.

**Aufwand reduzieren**

Normalerweise müssen Sie verbinden zu jeder Datenbank unabhängig voneinander um Transact-SQL-Anweisungen ausführen oder andere Verwaltungsaufgaben ausführen. Ein Projekt übernimmt die Aufgabe der Protokollierung in jeder Datenbank in der Zielgruppe. Sie auch definieren, verwalten und beibehalten werden Transact-SQL-Skripts, über eine Gruppe von Azure SQL-Datenbanken ausgeführt werden soll.

**Buchhaltung**

Aufträge führen Sie das Skript, und melden Sie sich den Status der Ausführung für jede Datenbank. Sie erhalten auch automatische Wiederholung, wenn Fehler auftreten.

**Flexibilität**

Definieren von benutzerdefinierten Gruppen von Azure SQL-Datenbanken und Zeitpläne für die Ausführung eines Auftrags definieren.

**Bereitstellung**

Bereitstellen von Datenebenen-Anwendungen (DACPACs).

> [AZURE.NOTE] Der Azure-Portal steht nur ein reduzierter Satz von Funktionen zu SQL Azure flexible Pools eingeschränkt. Verwenden Sie den PowerShell-APIs, um den vollständigen Satz von Funktionen für das aktuelle zuzugreifen.

## <a name="applications"></a>Applikationen 

* Ausführen von administrativen Aufgaben, wie etwa ein neues Schema bereitstellen.
* Allgemeine Daten-Produkt-Referenzinformationen in allen Datenbanken zu aktualisieren. Oder Zeitpläne automatisch so aktualisiert, dass jeder Wochentag, nach Stunden.
* Index zum Verbessern der abfrageleistung neu zu erstellen. Das Neuerstellen kann konfiguriert werden beispielsweise in Zeiten für eine Sammlung von Datenbanken wiederholt ausgeführt wird, klicken Sie auf ausführen.
* Sammeln Sie Abfrageergebnisse in einem Satz von Datenbanken in eine zentrale Tabelle auf eine kontinuierlich. Leistung Abfragen können fortlaufend ausgeführt und so konfiguriert, dass Trigger Weitere Aufgaben, die ausgeführt werden.
* Führen Sie mehr laufenden Datenverarbeitung Abfragen für eine große Gruppe von Datenbanken, beispielsweise die Sammlung von Kunden werden. Ergebnisse werden in einer einzelnen Zieltabelle zur weiteren Analyse zusammengefasst.

## <a name="elastic-database-jobs-end-to-end"></a>Flexible Datenbankaufträge: End-to-End 
1.  Installieren Sie die **Datenbank flexible Aufträge** Komponenten. Weitere Informationen finden Sie unter [Installieren von flexible Datenbank Aufträge](sql-database-elastic-jobs-service-installation.md). Wenn die Installation fehlschlägt, finden Sie unter [Deinstallieren](sql-database-elastic-jobs-uninstall.md).
2.  Verwenden Sie die PowerShell-APIs Zugriff auf Weitere Funktionen, beispielsweise Erstellen von benutzerdefinierten Datenbanksammlungen, Terminpläne hinzufügen und/oder tragen Ergebnismengen aus. Verwenden Sie im Portal für einfache Installation und Erstellung/Überwachung der Einzelvorgänge auf Ausführung für einen **Ressourcenpool flexible Datenbank**beschränkt. 
3.  Erstellen von verschlüsselten Anmeldeinformationen für die Ausführung des Auftrags und [dem Benutzer (oder Rolle), jede Datenbank in der Gruppe hinzufügen](sql-database-security.md).
4.  Erstellen einer Idempotent T-SQL-Skript, das für jede Datenbank in der Gruppe ausgeführt werden kann. 
5.  Wie folgt vor, um Aufträge mithilfe des Azure-Portals zu erstellen: [Erstellen und Verwalten von Aufträgen flexible Datenbank](sql-database-elastic-jobs-create-and-manage.md). 
6.  Oder mithilfe von PowerShell Skripts: [Erstellen und Verwalten von einer SQL-Datenbank flexible Datenbankaufträge mithilfe der PowerShell (Preview)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotent Skripts

Die Skripts müssen [Idempotent](https://en.wikipedia.org/wiki/Idempotence)sein. Einfach ausgedrückt bedeutet "Idempotent" an, wenn das Skript erfolgreich ist, und es erneut ausgeführt wird, das gleiche Ergebnis erfolgt. Ein Skript kann ein Fehler aufgrund einer vorübergehenden Netzwerkproblemen auftreten. In diesem Fall wird der Auftrag automatisch wiederholt das Skript eine vordefinierte Anzahl, wie oft vor dem desisting ausgeführt. Ein Skript Idempotent hat das gleiche Ergebnis aus, selbst wenn zweimal erfolgreich ausgeführt wurde. 

Eine einfache Verfahren besteht darin, das Vorhandensein eines Objekts, bevor Sie es erstellt haben.  

    IF NOT EXIST (some_object)
    -- Create the object 
    -- If it exists, drop the object before recreating it.

Auf ähnliche Weise ein Skript muss Lage, erfolgreich durch logisch für testen und Bezug auf eine beliebige Bedingungen sucht.

## <a name="failures-and-logs"></a>Fehler und Protokolle

Wenn ein Skript nach mehreren versuchen fehlschlägt, wird der Auftrag der Fehler protokolliert und weiterhin. Nach dem Ende eines Auftrags können (d. h. eines Testlaufs für alle Datenbanken in der Gruppe), die Liste der fehlgeschlagene Versuche überprüfen. Die Protokolle Bereitstellen von Details zu fehlerhaften Skripts debuggen. 

## <a name="group-types-and-creation"></a>Gruppieren Typen und Erstellung

Es gibt zwei Arten von Gruppen: 

1. Shard Datensätze
2. Benutzerdefinierte Gruppen

Verwenden die [flexible Datenbanktools](sql-database-elastic-scale-introduction.md)Shard festlegen Gruppen erstellt. Wenn Sie eine Shard festlegen Gruppe erstellen, sind Datenbanken hinzugefügt oder automatisch aus der Gruppe entfernt. Angenommen, werden eine neue Shard in der Gruppe automatisch beim Hinzufügen der Karte Shard. Ein Auftrag kann dann anhand der Gruppe ausgeführt werden.

Benutzerdefinierte Gruppen sind auf der anderen Seite fest definiert. Sie müssen explizit hinzufügen oder Entfernen von Datenbanken in benutzerdefinierten Gruppen. Wenn Sie eine Datenbank in der Gruppe gelöscht wird, versucht der Auftrag, das Skript für die Datenbank in einem tatsächlichen Fehler resultierender auszuführen. Über das Azure-Portal aktuell erstellte Gruppen sind benutzerdefinierte Gruppen. 


## <a name="components-and-pricing"></a>Komponenten und Preise
 
Die folgenden Komponenten arbeiten zusammen, um eine Azure-Cloud-Dienst zu erstellen, der Ad-hoc-Ausführung administrative Aufträge ermöglicht. Die Komponenten installiert und konfiguriert automatisch während der Installation im Rahmen Ihres Abonnements. Diese automatisch generierten gleichnamigen besitzen, können Sie die Dienste identifizieren. Der Name ist eindeutig und besteht aus dem Präfix "Edj" gefolgt von 21 erzeugten Zeichen.

* **Azure-Cloud-Dienst**: Flexible Datenbankaufträge (Preview) wurde übermittelt, als Kunden gehostete Azure-Cloud-Dienst Ausführung der anstehenden Aufgaben ausführen. Der Dienst ist im Portal bereitgestellt und in Ihr Microsoft Azure-Abonnement gehostet wird. Die Standardeinstellung bereitgestellt, mit der mindestens zwei Worker-Rollen für eine hohe Verfügbarkeit der Dienst ausgeführt wird. Standardgröße der einzelnen Worker-Rolle (ElasticDatabaseJobWorker) bei einer A0 Instanz ausgeführt wird. Preise, finden Sie unter [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL-Datenbank**: der Dienst einer Azure SQL-Datenbank bekannt als **Steuerelement Datenbank** verwendet, um alle Auftrag Metadaten zu speichern. Die Standard-Dienst Stufe ist eine S0. Preise, finden Sie unter [SQL Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure-Dienstbus**: ein Azure-Dienstbus ist für die Koordination von der Arbeit in der Azure-Cloud-Dienst. Finden Sie unter [Dienstbus Preise](https://azure.microsoft.com/pricing/details/service-bus/).
* **Azure-Speicher**: ein Azure-Speicher Konto wird verwendet, um speichern diagnostic Ausgabe Protokollierung den Fall, dass ein Problem mit weiteren Debuggen (siehe [UFI-Diagnose in Azure-Cloud-Diensten und virtuellen Computern](../cloud-services/cloud-services-dotnet-diagnostics.md)) erfordert. Preise, finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Wie funktionieren flexible Datenbank Aufträge

1.  Einer SQL Azure-Datenbank ist eine **Datenbank Steuerelement** gekennzeichnet der Daten für alle Metadaten und Zustand gespeichert.
2.  Die Datenbank Steuerelement erfolgt durch den **Auftragsdienst** starten und Nachverfolgen von Aufträgen, die ausgeführt.
3.  Zwei unterschiedliche Rollen Kommunikation mit der Datenbank steuern: 
    * Controller: Bestimmt, welche Einzelvorgänge die angeforderten Auftrag und Wiederholungsversuche fehlgeschlagene Aufträge durch Erstellen von neuen Projektaufgaben erforderliche Schritte erforderlich.
    * Ausführung des Auftrags Aufgabe: Führt die Projektaufgaben.

### <a name="job-task-types"></a>Position von Vorgangsarten

Es gibt mehrere Arten von Projektaufgaben, die Ausführung Aufträge ausführen:

* ShardMapRefresh: Die Shard-Karte, um alle als mehrere Shards hinweg verwendeten Datenbanken ermitteln in Abfragen
* ScriptSplit: Teilt das Skript über Anweisungen in Stapeln 'OK'
* ExpandJob: Untergeordnete erstellt Aufträge für jede Datenbank aus einem Auftrag, der auf eine Gruppe von Datenbanken verweisen.
* ScriptExecution: Führt ein Skript für eine bestimmte Datenbank mit definierten Anmeldeinformationen
* DACPAC: Wendet eine DACPAC zu einer bestimmten Datenbank mit bestimmten Anmeldeinformationen

## <a name="end-to-end-job-execution-work-flow"></a>End-to-End-Position Ausführung-Workflow

1.  Mithilfe des Portals oder der PowerShell-API, wird eine Position in der **Datenbank Steuerelement**eingefügt. Die Ausführung des Auftrags Anfragen der ein Transact-SQL-Skript für eine Gruppe von Datenbanken mit bestimmten Anmeldeinformationen.
2.  Der Controller kennzeichnet die neue Position. Projektaufgaben erstellt und das Skript Teilen und aktualisieren Sie die Gruppe Datenbanken ausgeführt. Schließlich wird ein neues Projekt erstellt und ausgeführt, um den Auftrag zu erweitern, und erstellen neue untergeordnete Einzelvorgänge, für jedes untergeordnete Projekt auszuführende das Transact-SQL-Skript für eine einzelne Datenbank in der Gruppe angegeben ist.
3.  Der Controller identifiziert den erstellten untergeordneten Aufträge. Für jedes Projekt der Controller erstellt und löst einen Vorgang Auftrag zum Ausführen des Skripts für eine Datenbank. 
4.  Nachdem alle Projektaufgaben abgeschlossen haben, aktualisiert der Controller die Einzelvorgänge in einer abgeschlossenen Zustand. Jederzeit während der Ausführung des Auftrags kann der PowerShell-API verwendet werden, den aktuellen Status der Ausführung des Auftrags anzeigen. Allen Zeiten von PowerShell-APIs zurückgegeben werden in UTC dargestellt. Falls gewünscht, kann eine Absage initiiert werden, um eine Position zu beenden. 

## <a name="next-steps"></a>Nächste Schritte
[Installieren Sie die Komponenten](sql-database-elastic-jobs-service-installation.md)und dann [erstellen, und fügen Sie für jede Datenbank in der Gruppe der Datenbanken angemeldet](sql-database-security.md). Weitere Position erstellen und Verwalten von finden Sie unter [Erstellen und Verwalten von Aufträgen flexible Datenbank](sql-database-elastic-jobs-create-and-manage.md). Siehe auch [Erste Schritte mit flexible Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->

 

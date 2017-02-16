<properties 
   pageTitle="Azure SDK für .NET 2.6 – Versionsinformationen" 
   description="Azure SDK für .NET 2.6 – Versionsinformationen" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK für .NET 2.6 – Versionsinformationen

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.6 Release. 

Sie können mit Azure SDK 2.6 Cloud-Anwendungen (PaaS) .NET 4.5.2 oder .NET 4.6 verwendet werden, vorausgesetzt, dass Sie das Ziel .NET Framework auf die Rolle der Cloud-Dienst manuell installieren entwickeln. Finden Sie unter [Installieren von .NET in einer Rolle der Cloud-Dienst](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Dienstbus updates

- Ereignis Hubs: 

    - Ermöglicht jetzt gezielte Access-Steuerelements, wenn Ereignisse zu senden, indem Sie zusätzliche Publisher-Endpunkt verfügbar machen, für das Ereignis Hubs.
    - Zusätzliche Stabilität und Verbesserung Ereignis Hubs Feature hinzugefügt.
    - Hinzufügen von Unterstützung von Amqp Protokoll über WebSocket für messaging und Hubs Ereignis.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools für Visual Studio-updates

- **IntelliSense-Erweiterung**: Vorschlag remote-Metadaten

    HDInsight Tools für Visual Studio unterstützt jetzt beim Abrufen des remote-Metadaten aus, wenn Sie Ihr Skript Struktur bearbeiten. Beispielsweise können Sie eingeben, * *auswählen* von** und die Tabellennamen angezeigt werden soll. Darüber hinaus werden die Spaltennamen angezeigt werden, nachdem Sie eine Tabelle angegeben.

- **HDInsight Emulator-Unterstützung**

    HDInsight Tools für Visual Studio unterstützt jetzt Herstellen einer Verbindung mit HDInsight Emulator, damit Sie Ihre Struktur Skripts lokal entwickeln könnten, ohne Einführung in einem beliebigen Kosten, führen Sie diese Skripts auf Ihrer HDInsight Cluster. 

    Weitere Informationen finden Sie in [diesem Handbuch](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **HDInsight Tools für Visual Studio-Unterstützung für generische Hadoop Cluster** (Preview)

    HDInsight Tools für Visual Studio unterstützt jetzt generische Hadoop Cluster, daher können Sie HDInsight Tools für Visual Studio für Folgendes verwenden:

    - Verbinden Sie mit Ihren Cluster, 
    - Beschriften Sie Struktur Abfrage mit erweiterten IntelliSense/AutoVervollständigen Unterstützung 
    - Zeigen Sie aller Einzelvorgänge in Ihrem Cluster mit eine intuitive Benutzeroberfläche an. 

    Weitere Informationen finden Sie in [diesem Handbuch](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>In der Rolle Cache updates

- Verwenden Sie **Microsoft Azure-Speicher SDK** Version 4.3 wurde **In Rolle Cache** aktualisiert. Bis jetzt wurde **In Rolle Cache** Azure-Speicher SDK Version 1.7 verwenden.

    Kunden verwenden Azure SDK 2,5 oder unten sollte auf Azure SDK 2.6 aktualisieren und verschieben auf die neue Version des Azure-Speicher SDK. 

    Zu diesem Zeitpunkt ist Azure-Speicher Version 2011-08-18 zu entfernenden 1 August 2016 berechnet. Alle Migration von In-Rolle Cache aus Azure SDK 2,5 oder unten müssen auf 2.6 abgeschlossen bis zu diesem Zeitpunkt. Weitere Informationen zum Ende der Verfügbarkeit von Azure-Speicher Version 2011-08-18, finden Sie unter [Microsoft Azure-Speicher Dienst Version freistellen Update: Erweiterung 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Wir haben die 30 November 2016, Rente für Azure verwaltet Cachediensts und Azure In Rolle Cache Ankündigung. Wir empfehlen, die Sie als Vorbereitung für diese Rente Azure Redis Cache migrieren. Weitere Informationen zu Datums- und Hinweise zur Migration finden Sie unter [die Azure Cache Angebot ist richtig für mich?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>App-Verwaltungsdienst Azure-Tools

Die folgenden Elemente aktualisiert wurden in der Version Azure SDK 2.6.

- Für die Veröffentlichung Azure erweitert und Azure-API Apps als Bereitstellungsziel.
- API Apps provisioning Funktionalität Benutzer mit API App erstellen und Bereitstellen von Funktionen zu aktivieren.
- Server-Explorer geändert, um neuen App-Dienst Knoten mit API, Mobile und Web apps, gruppiert nach der Ressourcengruppe entsprechen.
- Hinzufügen von Azure Client-App-API Bewegung hinzugefügt, um die meisten c#-Projekte, die automatische Erstellung von Swagger aktiviert API Apps in Azure Abonnement eines Benutzers ausgeführt werden kann.
- Apps-API Tools und App-Service-Knoten im Server-Explorer sind nur in Visual Studio 2013 verfügbar. 

##<a name="azure-resource-manager-tools-updates"></a>Azure Ressourcenmanager Tools aktualisiert.

Die Azure Resource Managertools wurden zum Einschließen von Vorlagen für virtuelle Computer, Netzwerke und Speicher aktualisiert. Um eine neue Gliederungsansicht für Vorlagen und die Möglichkeit zum Bearbeiten der Vorlagen mit JSON Codeausschnitte aufzunehmen wurde die JSON-Bearbeitung aktualisiert. Bereitstellung von Visual Studio Vorlagen verwenden Sie ein PowerShell-Skript mit dem Projekt bereitgestellt werden, sodass an das Skript vorgenommenen Änderungen von Visual Studio verwendet werden.

##<a name="diagnostics-improvements-for-cloud-services"></a>Verbesserung der Diagnose für Cloud-Dienste

Azure SDK 2.6 ist wieder eine Unterstützung für die Erfassung von Protokollen in der Azure-Serveremulator für Diagnose und zur Übertragung dieser Entwicklung Speicher. Alle Diagnose protokolliert (einschließlich der Anwendung Spur Protokolle, Tracing for Windows (ETW)-Protokolle, Leistungsindikatoren, Infrastruktur Protokolle und Windows-Ereignisprotokollen Ereignis) generiert Wenn die Anwendung im Emulator ausgeführt wird kann auf übertragen werden Entwicklung Speicherplatz zu überprüfen, ob Ihre Diagnose Protokollierung auf Ihrem lokalen Computer funktioniert. 

Das Diagnose Speicher-Konto kann jetzt in der Konfiguration (.cscfg), denen es einfacher mit anderen Diagnose Speicherkonten für die verschiedenen Umgebungen Dienstdatei angegeben werden muss. Es gibt einige wichtige Unterschiede zwischen wie die Verbindungszeichenfolge in Azure SDK 2.4 und Azure SDK 2.6 gearbeitet haben. Weitere Informationen zum Verwenden der Diagnose Speicher Verbindungs Zeichenfolge und wie sie Ihre Projekte wirkt sich auf die finden Sie unter [Konfigurieren von Diagnose für Azure-Cloud-Dienste](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Unterbrechen der Änderungen

###<a name="azure-resource-manager-tools"></a>Azure Ressourcenmanager Tools 

- Der **Cloud Deployment Projects** Projekttyp zur Verfügung, in der Azure SDK 2,5 wurde **Azure**Ressourcengruppe umbenannt.
- **Cloud Deployment Projects** Typ des in der Azure SDK 2,5 erstellte Projekte in 2.6 verwendet werden kann, aber die Vorlage von Visual Studio bereitstellen fehl. Bereitstellen von mit der PowerShell-Skript immer noch funktioniert jedoch wie zuvor ausgeführt.  Informationen zur Verwendung finden **Cloud Deployment Projects** in 2.6 dieser [Posten](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Bekannte Probleme

- Sammeln von Diagnose Protokolle im Emulator erfordert eine 64-Bit-Betriebssystem. Diagnose Protokolle werden nicht erfasst werden, wenn bei einem 32-Bit-Betriebssystem ausgeführt. Dies wirkt sich nicht auf andere Funktionen für Emulator aus. 

- Azure SDK 2.6 auf 4/29/2015 freigegeben wurden zwei Probleme: 

    - Universeller App konnte nicht in Visual Studio 2015 geladen werden, wenn Azure SDK 2.6 auf dem Computer installiert wurde.
    - Für das Debuggen eines Projekts Cloud-Dienst würde fehlschlagen in Visual Studio 2013 und Visual Studio 2015, wo Visual Studio nicht mehr reagiert und stürzt ab, wenn ein Dialogfeld mit der Meldung "Konfigurieren von Diagnose für Emulator" angezeigt.
    
    Ein Update auf Azure SDK 2.6 wurde auf 5/18/2015 zur Verfügung. Die aktualisierte Version ist 2.6.30508.1601; Es enthält zwei oben beschriebenen Probleme behoben. Sie können das Erstellen von SDK von Control Panel identifizieren -> Programme und Features -> Microsoft Azure-Tools für Microsoft Visual Studio 2013 – V 2.6. Version dieser Spalte wird die Build-Nummer angezeigt.

    Wenn Sie die oben aufgeführten Probleme weiterhin gegenüberliegende sind, installieren Sie die neueste Version des Azure 2.6 SDK für [im Vergleich mit einer 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [im Vergleich mit einer 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) oder [im Vergleich mit einer 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Siehe auch

[Support und Rente Informationen zum Microsoft Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

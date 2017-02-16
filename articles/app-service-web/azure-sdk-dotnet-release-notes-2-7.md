
<properties 
   pageTitle="Azure SDK für .NET 2.7 und .NET 2.7.1 – Versionsinformationen" 
   description="Azure SDK für .NET 2.7 und .NET 2.7.1 – Versionsinformationen" 
   services="app-service\web" 
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

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK für .NET 2.7 und .NET 2.7.1 – Versionsinformationen

##<a name="overview"></a>(Übersicht)

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.7 Release. 

Das Dokument auch die Versionsinformationen für das Azure SDK enthalten, für .NET 2.7.1 lassen Sie wieder los.

Azure SDK 2.7 ist nur in Visual Studio 2015 und Visual Studio-2013 unterstützt. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) ist, dass die letzte SDK für Visual Studio 2012 unterstützt.

Ausführliche Informationen zu dieser Version finden Sie unter [Azure SDK 2.7 Ankündigung bereitstellen](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) und [Azure SDK 2.7.1 Ankündigung bereitstellen](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK für .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Melden Sie sich für Visual Studio 2015 Verbesserungen

Azure SDK 2.7 für Visual Studio 2015 unterstützt die Funktionen für die neue Identität in Visual Studio 2015.  Dies umfasst die Unterstützung für den Zugriff auf Azure über Rolle basierend Access Control, Cloud-Lösungsanbieter, DreamSpark und andere Arten Konto und Ihr Abonnement Konten.

Im Lieferumfang von Azure SDK 2.7 Anmeldung Verbesserungen stehen nur in Visual Studio 2015. Unterstützung für Visual Studio 2013 ist in Azure SDK 2.7.1 enthalten.


###<a name="mobile-sdk"></a>Mobile SDK

Aktualisiert **Mobile-Apps** -Vorlagen, die neueste [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) und Setup-Prozess wiederzugeben.

###<a name="service-bus"></a>Dienstbus 

Allgemeine Updates und verbesserte. Details auf Updates und Features finden Sie in den Release Notes von den neuesten [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>HDInsight-Tools 

In dieser Version wurden die folgenden Updates vorgenommen. Diese Updates stehen in der Vorschau. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).

- Diagramme für Struktur Tez Einzelvorgänge Struktur
- Vollständige Struktur DML IntelliSense-Unterstützung
- Schwein Vorlage support
- Storm Vorlagen für Azure-Dienste

####<a name="breaking-changes"></a>Unterbrechen der Änderungen

- Bei Verwendung dieser Version der Tools, muss altes **Storm** Projekt aktualisiert werden. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express wird nicht mehr unterstützt. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>App-Verwaltungsdienst Azure-Tools

In dieser Version wurden die folgenden Updates und Web-Tools-Erweiterungen geführt. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) Blog. 

- Unterstützung für DreamSpark-Konten hinzugefügt
- Vollständige Änderung Azure Tools vorgenommen, um die neuen Azure Ressource Management-APIs unterstützen
- Unterstützung für App-Verwaltungsdienst Azure [Cloud Explorer](#cloud_explorer) hinzugefügt

####<a name="known-issues"></a>Bekannte Probleme

Web App-Bereitstellung Slot Knoten nicht angezeigt werden, unter dem Knoten Steckplätze im Server-Explorer, und Web App-Bereitstellung Slot untergeordnete Knoten nicht unter Cloud Explorer laden. Dieses Problem wurde gelöst und für die nächste Version von SDK vorbereitet. 


###<a name="a-namecloudexploreracloud-explorer-for-visual-studio-2015"></a><a name="cloud_explorer"></a>Cloud-Explorer für Visual Studio 2015

Azure SDK 2.7 enthält Cloud Explorer für Visual Studio 2015, wodurch Sie Azure Ressourcen anzeigen, prüfen ihre Eigenschaften und Ausführen von in Visual Studio Key Entwicklertools Aktionen. 

Cloud-Explorer unterstützt Folgendes:

- Ressourcengruppe "und" Ressourcenart Ansichten Azure Ressourcen 
- Suchen nach Ressourcen anhand des Namens (in der Ansicht Ressourcenart verfügbar)
- Unterstützung für Abonnements und Ressourcen, die Rolle basierend Access Steuerelement (RBAC) angewendet haben. 
- Integrierte Bereich Aktion die Aktionen Entwicklertools konzentrieren speziell für die ausgewählten Ressourcen anzeigt. Beispiel: Remotedebugger anfügen, für die erstellte Maschinen verwenden des Stapels Ressourcenmanager Diagnosedaten für virtuellen Computern usw. anzeigen.
- Integrierte Eigenschaftenbereich die Entwicklertools konzentrieren Eigenschaften, die häufig benötigt während Test-/anzeigt 
- Schnelles Umschalten des Kontos zu verwenden, wenn Ressourcen auflisten (verwenden Einstellungen Befehl Symbolleiste) 
- Filtern von Abonnements für die Ressourcen auflisten (verwenden Einstellungen Befehl Symbolleiste) 
- Tiefe Links für die Verwaltung von Ressourcen und Ressourcengruppe Azure-Portal 
 
 
###<a name="azure-resource-manager-tools"></a>Azure Ressourcenmanager Tools 

Die Azure Ressourcenmanager Tools wurden zum Arbeiten mit Rolle basierend Access Steuerelement (RBAC) und neue Abonnement Typen aktualisiert.  Diese Änderungen umfasst die Möglichkeit, Speicherkonten zusätzlich zur klassischen Speicherung, zu verwenden, um Elemente während der Bereitstellung speichern.  

Wenn Sie ein Projekt Azure Ressourcengruppe aus einer früheren Version des SDK mit der 2.7 SDK verwenden, wird eine neue Bereitstellungsskript bereitstellen, verwenden ein neues Speicherkonto statt klassischen Speicher benötigt.  Sie werden aufgefordert, bevor Sie Änderungen an Ihrem Projekt vorgenommen werden, das neue Skript hinzufügen.  Das alte Skript wird umbenannt werden und Sie müssen das neue Skript in irgendeiner Weise manuell vornehmen.
 
 
###<a name="storage-explorer-tools"></a>Tools für Speicher-Explorer 

- Unterstützung für die Anzeige von Blobs angefügt werden soll. Weitere Informationen in [diesem Blogbeitrag](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Unterstützung für die Anzeige von Speicher Premium-Konten über Server-Explorer. Server-Explorer wird nur Seitenblobs für Premium Speicherkonten angezeigt, sind der einzige unterstützte Typ für Premium Speicherkonten.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory-Tools für Visual Studio 

Einführung in **Azure Data Factory-Tools** für Visual Studio aus. Nachstehend sind die Features aktiviert. [Diesen Blog](http://go.microsoft.com/fwlink/?LinkId=617530) für Weitere Informationen finden Sie unter.

- **Erstellung der Vorlage basiert**: Wählen Sie Groß-/Kleinschreibung verwenden basiert, Vorlagen, Daten Bewegung Vorlagen oder Datenverarbeitung Vorlagen zum Bereitstellen einer End-to-End-Daten Integration-Lösung und erste Schritte mit Daten Factory schnell praktische. 
- **Integration in Lösung Explorer zum Erstellen und Bereitstellen von Daten Factory Einheiten**: Erstellen und Bereitstellen von Pipelines und verwandte Elemente nach Visual Studio-Projekte. 
- **Integration in Diagramm anzeigen, für die visuelle Interaktion beim authoring**: visuell verfassen Pipelines und Datasets mit Hilfe von der Diagrammansicht. 
- **Integration in Server-Explorer für das Durchsuchen und Interaktion mit bereits bereitgestellten Einheiten**: der Server-Explorer zu suchen, die bereits bereitgestellt Daten Factory und entsprechende Einheiten nutzen. Importieren Sie eine Factory bereitgestellten Daten oder eine beliebige Entität (Verkaufspipeline, verknüpfte Dienst Datasets) in Ihr Projekt. 
- **JSON-Bearbeitung mit Schema Validierung und Rich-Intellisense**: effizient konfigurieren und Bearbeiten von JSON-Dokumenten Daten Factory Einheiten mit Rich-Überprüfung von Intellisense und Schemadateien 
- **Mehrere Umgebung für die Veröffentlichung**: Veröffentlichen verfasst Pipelines für Entwickler, testen oder FA-Umgebung durch separate Config-Dateien für jede Umgebung zu erstellen.
- **Schwein, Struktur und .net Grundlage Datenverarbeitung Support**: Support für Schwein und Struktur Skripts Daten Factory-Projekt. Unterstützung für verweisen auf C#-Projekt für die Verwaltung von .net Aktivität.

##<a name="azure-sdk-for-net-271"></a>Azure SDK für .NET 2.7.1

Der folgende Abschnitt enthält Updates, die mit dem Azure SDK vorübergehende für .NET 2.7.1 lassen Sie wieder los.
###<a name="hdinsight-tools"></a>HDInsight-Tools 

Ausführlichere Erläuterung zu HDInsight Tools Updates finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=623831).

- Struktur Auftrag Operator anzeigen (ein neues Feature)

    Wenn Sie Ihre Abfrage Struktur helfen können, wurde besser, Feature für die Struktur Operator Ansicht hinzugefügt. Zum Anzeigen aller Operatoren innerhalb eines Eckpunkts Doppelklick auf die Scheitelpunkte des Diagramms Position. Um weitere Details eines bestimmten Operators anzuzeigen, zeigen Sie auf diesen Operator.
- Struktur Fehler Marker (ein neues Feature)

    Zum Anzeigen der Grammatikfehler Anzeigeberechtigungen wurde sofort, das Feature Struktur Fehler Marker hinzugefügt. Darüber hinaus Fehlermeldungen wurden verbessert und detaillierte Grammatikfehler jetzt sofort angezeigt (bis zu dieser Version hatte Sie zum Senden einer Struktur Skript zum Cluster und warten Sie einige Zeit, bevor Sie Details über Ihre Fehlermeldung erhalten).  
- Storm Suchtopologie Graph (ein neues Feature)

    Visualisieren ist sehr wichtig, wenn Sie möchten, um festzustellen, ob der Suchtopologie wie erwartet funktioniert. In dieser Version haben wir Visualisierung für Storm Diagramme hinzugefügt. Sie können die wichtige Metrik für Ihre Suchtopologie visualisieren (beispielsweise eine Farbe gibt Wetter einen bestimmten umgewandelt wird "beschäftigt" oder nicht). Sie können auch doppelklicken herstellt/Schnauze um weitere Details anzeigen klicken.

- Unterstützung für HDInsight Cluster, die im Portal Azure (Fehlerkorrektur) erstellt wurden

    Sie können nun Visual Studio anzeigen und Senden von Aufträgen an alle HDInsight Cluster unabhängig davon, wo der Cluster erstellt wurden.

- Weitere IntelliSense-Unterstützung und schneller Struktur Metadaten geladen (eine Verbesserung)

    Wir haben das IntelliSense durch Hinzufügen von weitere benutzerfreundlicheren Vorschläge verbessert. Beispielsweise kann Tabellenalias jetzt auch in IntelliSense vorgeschlagen werden, damit Sie Ihre Abfrage leichter schreiben können. Darüber hinaus wurden die Struktur Metadaten geladen, sodass nur mehrere Sekunden dauern, bis alle Datenbanken, Tabellen und Spalten mit der Struktur Metastore Liste gibt dauert verbessert.

Ausführlichere Erläuterung zu HDInsight Tools Updates finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Verbesserte Visual Studio 2013

- Azure SDK 2.7.1 ermöglicht Azure Konten und Abonnements über Rolle basierend Access Control, Cloud Solution Provider und Dreamspark Zugriff auf Visual Studio-2013.
- Mit Azure SDK 2.7.1 steht das neue Fenster der Cloud Explorer-Tool jetzt auch in Visual Studio 2013.

###<a name="known-issues"></a>Bekannte Probleme

Installieren der Azure SDK 2.6 oder 2.7.1 für Visual Studio Community 2013 auf einer nicht - englischen OS wird eine Warnung angezeigt, dass die Ressourcen für Deutsch, Russisch und nichtenglischen von Visual Studio möglicherweise falsch zugeordnet werden. Diese Warnung möglicherweise sicheres geschlossen werden. Es findet nur statt, wenn der Computer eine vorherige Installation von Visual Studio Community 2013 keine enthielt und auf einer nicht - englischen OS SDK installieren. Die Warnung wird angezeigt, nachdem Sie das Sprachpaket RTM Ressourcen für Visual Studio gilt, aber bevor sie Update 4 angewendet wird. Schließen die Warnung ermöglichen das Sprachpaket fortsetzen, und führen Sie die Update 4 Version des Inhalts Language Pack anwenden.

LightSwitch Projekte sind keine Compatibile mit dieser Version. Dieses Problem wird mit der nächsten SDK Version aufgelöst werden.

##<a name="also-see"></a>Siehe auch
[Azure SDK 2.7.1 Ankündigung Beitrag](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 Ankündigung Beitrag](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Support und Rente Informationen zum Microsoft Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

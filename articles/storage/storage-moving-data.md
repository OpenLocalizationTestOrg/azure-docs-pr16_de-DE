<properties
    pageTitle="Das Verschieben von Daten aus Azure-Speicher | Microsoft Azure"
    description="Dieser Artikel enthält eine Übersicht der verschiedenen Methoden für das Verschieben von Daten aus Azure-Speicher."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Das Verschieben von Daten aus Azure-Speicher

Wenn Sie lokaler Daten Azure-Speicher (oder umgekehrt) verschieben möchten, gibt es verschiedene Möglichkeiten zur Verfügung. Der Ansatz, der für Sie am besten geeignete hängt von Ihrem Szenario. Dieser Artikel bietet einen schnellen Überblick über die verschiedenen Szenarios und entsprechenden Angebote für jedes.

## <a name="building-applications"></a>Baustein-Anwendungen

Wenn Sie eine Anwendung erstellen, ist das Entwickeln auf die REST-API oder einer unserer viele Clientbibliotheken eine großartige Möglichkeit zum Verschieben von Daten an und von Azure-Speicher.

Azure-Speicher stellt rich Client-Bibliotheken für .NET, iOS, Java, Android, Universal Windows-Plattform (UWP), Xamarin, C++, Node.JS, PHP, Ruby und Python. Client-Bibliotheken bieten erweiterte Funktionen wie "Wiederholen" Logik, Protokollierung und parallele Uploads aus. Sie können auch direkt gegen die REST-API entwickeln die durch eine andere Sprache aufgerufen werden können, das HTTP-/HTTPS-Anfragen herstellt.

Finden Sie unter [Erste Schritte mit Azure BLOB-Speicher](storage-dotnet-how-to-use-blobs.md) , um mehr zu erfahren.

Darüber hinaus bieten wir [Azure Speicher Daten Bewegung Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) eine Bibliothek für leistungsfähige Kopieren von Daten an und von Azure vorgesehen ist. Näheres unsere Daten Bewegung Bibliothek [Dokumentation](https://github.com/Azure/azure-storage-net-data-movement) Weitere. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Schnelles Interagieren anzeigen/mit Ihren Daten

Wenn Sie möchten Ihre Daten Azure-Speicher anzeigen, während Sie haben auch die Möglichkeit zum Hochladen und Herunterladen von Daten auf einfache Weise, sollten Sie eine Azure-Speicher-Explorer verwenden.

Schauen Sie sich unsere Liste der [Azure-Speicher Explorern](storage-explorers.md) Weitere.

## <a name="system-administration"></a>Systemadministration

Wenn Sie erfordern oder mehr mit einem Befehlszeilendienstprogramm (z. B. System-Administratoren) vertraut sind, nachfolgend einige Optionen zur Auswahl:

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilendienstprogramm entwickelt für leistungsfähige Kopieren von Daten an und von Azure-Speicher. Sie können auch die Daten in einem Speicherkonto oder zwischen verschiedenen Speicherkonten kopieren.

Weitere Informationen finden Sie in der [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Diensten auf Azure bereitstellt. Es ist eine Befehlszeile Shell aufgabenbasierten und Skriptingtools Sprache durchführen möchten, besonders für Systemadministration.

Finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](storage-powershell-guide-full.md) Weitere.

### <a name="azure-cli"></a>Azure CLI

Azure CLI bietet eine Reihe von open Source Plattform-Befehle für die Arbeit mit Azure Services. Azure CLI steht unter Windows, OSX und Linux.

Finden Sie unter [Verwendung der CLI Azure mit Azure-Speicher](storage-azure-cli.md) , um mehr zu erfahren.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Verschieben Sie große Datenmengen mit einem Netzwerk langsam

Eine der die wichtigsten Probleme beim Verschieben großer Datenmengen ist die Übertragungszeit. Wenn Sie Daten aus/Azure-Speicher ohne kümmern Netzwerken Kosten oder Schreiben von Code erhalten möchten, ist Azure Importieren/Exportieren einer geeigneten Lösung.

Finden Sie unter [Azure importieren/exportieren](storage-import-export-service.md) , um mehr zu erfahren.

## <a name="backing-up-your-data"></a>Sichern Ihrer Daten

Wenn Sie einfach sichern Ihre Daten Azure-Speicher müssen, ist Azure Sicherung die beste Wahl. Dies ist eine leistungsstarke Lösung für lokale Daten und Azure-virtuellen Computern sichern.

[Azure Sicherung](../backup/backup-introduction-to-azure-backup.md) Weitere Informationen finden Sie unter.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Zugreifen auf Ihre Daten lokal und in der Cloud

Wenn Sie eine Lösung für den Zugriff auf Ihre Daten lokal und in der Cloud benötigen, dann sollten Sie mithilfe des Azure Hybrid Cloud-Speicher-Lösung, StorSimple. Diese Lösung besteht aus einem physischen StorSimple Gerät, intelligente Stores häufig verwendeten Daten SSDs, gelegentlich Daten Festplatten und inaktive/Sicherung/Archivierung von Daten auf Azure-Speicher verwendet.

[StorSimple](../storsimple/storsimple-overview.md) Weitere Informationen finden Sie unter.

## <a name="recovering-your-data"></a>Wiederherstellen Ihrer Daten

Wenn Sie lokale Auslastung und Applikationen verfügen, benötigen Sie eine Lösung, die Ihr Unternehmen weiter nach einem Ausfall ausgeführt werden können. Azure Website Wiederherstellung übernimmt Replikation, Failover und Wiederherstellung von virtuellen Computern und physische Server. Replizierte Daten werden in Azure-Speicher, so dass Sie eine sekundäre vor Ort Datacenter überflüssig gespeichert.

[Azure Website Wiederherstellung](../site-recovery/site-recovery-overview.md) Weitere Informationen finden Sie unter.


<properties
    pageTitle="Speichern Sie vertrauliche Daten nie auf benutzerdefinierte Bilder für Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über die Richtlinien zum Speichern von Daten in den benutzerdefinierten Bilder in Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="never-store-sensitive-data-on-custom-images"></a>Speichern Sie vertrauliche Daten nie auf benutzerdefinierte Bilder

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn Sie Ihre eigene Anwendung in Azure RemoteApp hosten, ist der erste Schritt erstellen Sie ein benutzerdefiniertes Bild. Wir verwenden diese benutzerdefinierte Bild zum Erstellen von virtuellen Computer Instanzen Ihrer apps für Ihre Benutzer bei den ersten Schritten. Benutzerdefinierte Bild sollte enthalten, nur Anwendungen und nie vertrauliche Daten, die verloren, wie etwa SQL-Datenbanken, Personaldateien oder besondere Datendateien wie QuickBooks Unternehmen Dateien werden können. Alle vertrauliche Daten sollten außerhalb Azure RemoteApp auf einem Dateiserver, ein anderes Azure-virtuellen Computer, oder in SQL Azure befinden. Das Bild sollte nur die Anwendung hosten, die eine Verbindung mit der Datenquelle und zeigt die Daten. Überprüfen Sie [Anforderungen für Azure RemoteApp Bilder](remoteapp-imagereqs.md) für Weitere Informationen ein. 

Um zu verstehen, warum Sie nicht vertrauliche Daten gespeichert werden soll, müssen Sie verstehen, wie Azure RemoteApp funktioniert. Wenn eine Auflistung wird erstellt oder aktualisiert, Hintergrundinformationen mehrere Klonen oder mehrere Kopien des Bilds erstellt werden. Alle Instanzen diese virtuellen Computer sind genaue Kopien von benutzerdefinierten Bilds. Wenn Benutzer Programme starten sie eine der folgenden Instanzen virtueller Computer besteht. Aber die gleiche Instanz ist nicht unbedingt und sollte unerheblich, da sie nicht beständige sind. Die virtuellen Computer Instanzen die Applikationen Hostinganbieter sind nicht beständige und gelöscht oder gelöscht werden können beispielsweise während der Websitesammlung Update basiert. 

Nachdem die Sammlung bereitgestellt wird, und Benutzer mit dem Herstellen einer Verbindung mit den virtuellen Computern beginnen, besteht Benutzerdaten beständigen und geschützt werden, da es sich bei einem separaten Speicher innerhalb einer virtuellen Festplatte gespeichert ist, dass wir einen [Benutzer Profil Datenträger (UPD)](remoteapp-upd.md), rufen Sie das Benutzerprofil in c:\users also\<Benutzerprofils >. Wenn eine Anwendung gestartet wird, ist die UPD bereitgestellt werden, und wie ein lokales Benutzerprofil vom Betriebssystem behandelt. Weitere Informationen zu wie [Azure RemoteApp speichert Benutzerdaten und Einstellungen](remoteapp-upd.md).

Beispieldaten, die im Bild nicht enthalten sein soll:

- Freigegebene für Benutzern den Zugriff auf Daten
- SQL-DB oder QuickBooks DB
- Alle Daten in D:\

Beispieldaten, die im Profil zu kopierenden in jeder Benutzer UPD befinden können:

- Von Konfigurationsdateien pro Benutzer
- Skripts, die Benutzer müssen ihre UPD beibehalten

Wichtige Informationen:

- Nie Store vertrauliche Daten, die auf das Bild verloren werden können, wenn ein benutzerdefiniertes Bild zu erstellen.
- Vertrauliche Daten sollten immer auf einem separaten Dateiserver separaten Azure virtueller Computer, auf die Cloud und immer außerhalb Ihrer Anwendung in Azure RemoteApp Hostinganbieter Instanzen virtueller Computer befinden. 
- Benutzerdaten gespeichert haben und weiterhin besteht, in dem Benutzer Profil Datenträger (UPD)



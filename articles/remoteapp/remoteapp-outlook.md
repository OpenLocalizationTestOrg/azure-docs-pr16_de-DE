<properties
    pageTitle="Verwenden von Outlook in Azure RemoteApp | Microsoft Azure" 
    description="Informationen zum Konfigurieren und Verwenden von Outlook in Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Verwenden von Microsoft Outlook in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp unterstützt Microsoft Outlook Office 365 an. Weitere Informationen zu wie [Office in Azure RemoteApp funktioniert](remoteapp-officesubscription.md). Es gibt einige empfohlene Einstellungen für Outlook bei Verwendung in Azure RemoteApp aus.

## <a name="cached-mode"></a>-Cache-Modus
Cache-Modus ist eine empfohlene Konfiguration bei Verwendung von Outlook in Azure RemoteApp. Wenn Sie ein Exchange-Cache-Modus verwenden Outlook 2013-Konto konfigurieren, funktioniert Outlook 2013 aus eine lokale Kopie der Microsoft Exchange-Postfach des Benutzers, das in einer offline--Datendatei (OST-Datei) auf dem Computer des Benutzers, zusammen mit den Offline Adressbuch (OAB) gespeichert ist. Das zwischengespeicherte Postfach und OAB werden vom Office 365-Dienst regelmäßig aktualisiert. Weitere Informationen zu [den Unterschieden zwischen Cache und online-Modus](https://technet.microsoft.com/library/jj683103.aspx).

Der Benutzer kann **Exchange-Cache-Modus** oder im **Onlinemodus** während Konto einrichten oder Ändern der Konten-Einstellungen auswählen. Sie können auch einen Modus oder anderen mithilfe der Office-Anpassungstool (Okt) oder Gruppenrichtlinien bereitstellen.  

Lesen Sie die [Anweisungen Schritt-für-Schritt-Cache-Modus aktivieren](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Suchen
Mithilfe der Suchfunktion in Outlook in Azure RemoteApp gelten Einschränkungen aus. Azure RemoteApp verwendet gepoolte virtuellen Computern, um Benutzer Sitzungen aufzunehmen. Suchindizierung, hängt von der Computer-ID, die bei anderen virtuellen Computern unterschiedlich ist. Es ist möglich, dass sie jedes Mal, wenn ein Benutzer in Azure RemoteApp anmeldet, um einen neuen virtuellen Computer weitergeleitet werden. Die würde bedeutet, dass wir lokale Suche Indexer ausgeführt, wenn aktivieren wird jedes Mal, wenn die Computer-ID ändert (wenn der Benutzer auf einen anderen virtuellen Computer ist). Je nach der Größe der. OST-Datei konnte Indexer sehr lange dauern ausgefüllt und Verwenden von Ressourcen für andere apps erforderlich. Suche nicht nur würde langsam jedoch möglicherweise keine Ergebnisse zurückgegeben. Verwenden ein Profil Onlinemodus möchten diese Einschränkung umgehen, aber Gesamtsystemleistung würde aufgrund einer lokalen Cache des fehlen fallen (Siehe den Link über weitere Informationen zu den Unterschied zwischen Cache und online-Modus). Leider indiziert/lokale Suche kann nicht deaktiviert werden und nicht Onlinesuche in Outlook 2013 standardmäßig aktiviert werden.

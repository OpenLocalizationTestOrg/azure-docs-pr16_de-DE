<properties
    pageTitle="Bewährte Methoden für Azure RemoteApp | Microsoft Azure"
    description="Bewährte Methoden zum Konfigurieren und Verwenden von Azure RemoteApp."
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

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Bewährte Methoden zum Konfigurieren und Verwenden von Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Die folgende Informationen helfen Ihnen konfigurieren und Azure RemoteApp produktiv zu verwenden.

## <a name="connectivity"></a>Konnektivität


- Verwenden Sie immer die neueste Clientversion. Verwenden von älteren Clients möglicherweise Probleme mit der Konnektivität und andere beeinträchtigt Erfahrung führen. Aktivieren von automatischen Updates für Ihr Gerät wird sichergestellt, dass die neueste Client immer installiert ist.
- Verwenden Sie immer den am häufigsten unveränderliche und zuverlässigen Internetzugang zur Verfügung.  
- Verwenden Sie nur unterstützte Proxyverbindungen für optimale Leistung.  Der SOCKS-Proxy wird nicht unterstützt.

## <a name="applications"></a>Applikationen


- Speichern Sie und schließen Sie RemoteApp Applications aus, wenn Sie mit der Anwendung fertig sind. Schließen die Anwendung nicht möglicherweise Datenverlust führen.
- Überprüfen Sie vor der Verwendung in Azure RemoteApp benutzerdefinierten Applications. Dies umfasst, um sicherzustellen, diese Arbeiten an einer Sitzung mit mehreren Plattform und unnötige Ressourcen wie Arbeitsspeicher und CPU, die ein anderer Benutzer in der gleichen Websitesammlung blockieren möglicherweise nicht nutzen. Informationen herunterladen Sie, und überprüfen Sie die [Anwendung Kompatibilität bewährte Methoden für den Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguration und Verwaltung


- Behalten Sie Ihre Vorlage Bilder auf dem neuesten Stand, Softwareupdates und weitere wichtige Updates installieren, je nach Bedarf. Dadurch wird sichergestellt, wie Azure RemoteApp Auto-Skalen um Ihre Kapazität entsprechen, jede Instanz geändert wurde.  
- Stellen Sie sicher, dass die Bereitstellung von Active Directory Federation Services (AD FS) sicher und zuverlässig ist. Andernfalls möglicherweise Client-Authentifizierung fehlschlägt, verhindern, dass Benutzer den Zugriff auf RemoteApp Azure.
- Konfigurieren Sie Vorlage Bilder mit installierten Programme, die Rollen oder Features so, dass sie keinen Status aufweisen. Sie sollten nicht auf alle Instanzen von den virtuellen Computern in einem RemoteApp-Dienst wird in einem beständigen Zustand verlassen.
    - Speichern Sie alle Benutzerdaten in Benutzerprofile oder andere Speicherorte außerhalb der Dienst, wie lokale Freigaben oder OneDrive-Datei.
    - Speichern Sie freigegebene in Speicherorten außerhalb der Dienst, wie lokale Freigaben oder OneDrive-Datei.
    - Konfigurieren Sie keine System organisationsweite Einstellungen aus, in dem Vorlagenbild und nicht für einzelne virtuelle Computer in einem Dienst.
    - Deaktivieren Sie der automatischen Softwareupdates für Applikationen veröffentlichten – stattdessen wenden Sie diese dann manuell zu dem Vorlagenbild an, und Testen Sie, bevor Sie aus der Vorlage bereitstellen.

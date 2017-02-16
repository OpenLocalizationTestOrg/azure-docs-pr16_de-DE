<properties
 pageTitle="Was ist Azure Scheduler? | Microsoft Azure"
 description="Azure Scheduler ermöglicht Ihnen, deklarativ in der Cloud auszuführenden Aktionen beschrieben. Dann plant und diese Vorgänge automatisch ausgeführt wird."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Was ist Azure Scheduler?

Azure Scheduler ermöglicht Ihnen, deklarativ in der Cloud auszuführenden Aktionen beschrieben. Dann plant und diese Vorgänge automatisch ausgeführt wird.  Scheduler macht's mithilfe [der Azure-Portal](scheduler-get-started-portal.md), Code, [REST-API](https://msdn.microsoft.com/library/mt629143.aspx)oder Azure PowerShell.

Planer erstellt, verwaltet und ruft geplante Arbeit.  Planer keine Auslastung hosten oder kein Code ausgeführt. Nur _Ruft_ Code an anderer Stelle gehostet IT – Azure, lokal oder mit einem anderen Anbieter. Ruft Sie über HTTP, HTTPS, eine Speicherwarteschlange, eine Service Bus Warteschlange oder einem Dienst Bus Thema.

Planer plant [Aufträge](scheduler-concepts-terms.md), behält der Aufzeichnung der Ergebnisse der Position Ausführung eine überprüfen kann, und deterministisch und zuverlässig plant Auslastung ausgeführt werden soll. Azure WebJobs (Bestandteil des Web Apps-Features in der App-Verwaltungsdienst Azure) und anderen Terminplanungsfunktionen Azure verwenden Scheduler im Hintergrund. Die [REST-API Scheduler](https://msdn.microsoft.com/library/mt629143.aspx) hilft die Kommunikation für diese Aktionen zu verwalten. So unterstützt Scheduler, einfach [komplexe Zeitpläne und erweiterte Serie](scheduler-advanced-complexity.md) .

Es gibt mehrere Szenarien, die sich die Verwendung der Scheduler eignen. Beispiel:

+ _Periodisch Anwendungsaktionen:_ Erfassen von Daten in regelmäßigen Abständen aus Twitter in ein Feed.
+ _Tägliche Wartung:_ Tägliche Kürzen von Protokollen Durchführen einer Sicherung und anderen Wartungsaufgaben. Beispielsweise kann ein Administrator wählen Sie zum Sichern der Datenbank 1:00 Uhr jeden Tag für den nächsten neun Monaten.

Scheduler ermöglicht das Erstellen, aktualisieren, löschen, anzeigen und verwalten Einzelvorgänge und [Position Websitesammlungen](scheduler-concepts-terms.md) programmgesteuert, mithilfe von Skripts, und klicken Sie im Portal.

## <a name="see-also"></a>Siehe auch

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)

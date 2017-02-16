<properties
   pageTitle="Stabilität technischen Leitfaden Index | Microsoft Azure"
   description="Index der technischen Artikel auf Grundlegendes zu und Erstellen eines Konzepts robuste, hochgradig verfügbar, Fehlertoleranz Anwendungen sowie Disaster Wiederherstellung und Business Continuity-Planung"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Technische Anleitung Azure Stabilität

##<a name="introduction"></a>Einführung

Hohe Verfügbarkeit und Disaster Wiederherstellung Anforderungen Besprechung sind zwei Arten von Knowledge erforderlich:

- Detailliertes technischen Verständnis der Funktionen, die einen Cloud-Plattform
- Kenntnisse über das von dem einen verteilten Dienst ordnungsgemäß Aufbau

Diese Reihe von Artikeln beschrieben, die frühere: der Stärken und Schwächen der Azure-Plattform im Hinblick auf Stabilität (auch als Geschäftskontinuität bezeichnet). Wenn Sie die zweite interessiert sind, finden Sie unter die Artikel Reihe dienten [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](https://aka.ms/drtechguide). Obwohl dieser Artikelreihe auf Architektur und den Entwurf Mustern berührt, ist nicht den Fokus der Serie. Leitfaden für die Entwurf finden Sie in der Material im Abschnitt [zusätzliche Ressourcen](#additional-resources) .

Die Informationen sind in den folgenden Artikeln:

- [Wiederherstellung aus lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md).
Physische Hardware (z. B. Laufwerke, Server und Netzwerkgeräte) kann ein Fehler auftreten. Ressourcen können verbraucht werden beim Laden, Spitzen. In diesem Artikel werden die Funktionen, die Azure Sicherstellung der hohen Verfügbarkeit unter diesen Umständen verwalten.

- [Wiederherstellung aus einer Azure Region organisationsweite dienststörung](resiliency-technical-guidance-recovery-loss-azure-region.md).
Weit verbreitet Fehlern sind selten, aber sie theoretisch möglich sind. Gesamte Regionen können aufgrund von Netzwerkfehlern isoliert werden, oder sie können physisch von Naturkatastrophen beschädigt werden. In diesem Artikel wird erläutert, wie Azure um Applications zu erstellen, die geografischen verschiedene Regionen umfassen.

- [Wiederherstellung aus lokalen in Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
Die Cloud ändert erheblich die Wirtschaftlichkeit der Wiederherstellung, UFI-Organisationen Azure verwenden, um eine zweite Website für die Wiederherstellung zu erstellen. Sie können bei der Kosten erstellen und Verwalten von einem sekundären Datencenter Dezimalbruch ausführen. In diesem Artikel wird erläutert, die Funktionen, die Azure bereitstellt, für die Erweiterung einer lokalen Datacenter in der Cloud.

- [Wiederherstellung von Beschädigung der Daten oder unbeabsichtigtes löschen](resiliency-technical-guidance-recovery-data-corruption.md).
Applikationen können Fehlern die beschädigten Daten enthalten. Operatoren können falsch wichtige Daten löschen. In diesem Artikel wird erläutert, was Azure enthält, für die Daten sichern und Wiederherstellen einer vorherigen Punkt es dauern.

##<a name="additional-resources"></a>Zusätzliche Ressourcen

- [Wiederherstellung und hohe Verfügbarkeit bei Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
In diesem Artikel wird ein detaillierter Überblick über die Verfügbarkeit und Wiederherstellung. Es werden die Herausforderung, manuelle Replikation für Verweis und Transaktionen Daten behandelt. Die endgültige Abschnitten Zusammenfassung der verschiedenen Arten von Disaster Wiederherstellung Topologien, die für die höchsten Ebene über die Verfügbarkeit von Azure Regionen umfassen.

- [Hoher Verfügbarkeit Checkliste](resiliency-high-availability-checklist.md).
In diesem Artikel wird eine Liste der Features, die Dienste und Designs dar, die Ihnen helfen können die Stabilität und die Verfügbarkeit der Anwendung vergrößern.

- [Microsoft Azure Service Stabilität Anleitungen](resiliency-service-guidance-index.md).
In diesem Artikel ist ein Index der Azure-Diensten und enthält Links zu sowohl Disaster Wiederherstellung Anleitungen und Entwurf Anleitungen.

- [Übersicht: Cloud Business Continuity und Datenbank Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md).
Dieser Artikel enthält Azure SQL-Datenbank Techniken für Verfügbarkeit. Es zentriert hauptsächlich auf Sicherung und Wiederherstellung Strategien. Wenn Sie Azure SQL-Datenbank in der Cloud-Dienst verwenden, sollten Sie dieses Dokument und deren zugehörige Ressourcen überprüfen.

- [Hohe Verfügbarkeit und Wiederherstellung für SQL Server in Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Dieser Artikel beschreibt die von Verfügbarkeitsoptionen, die Sie durchsuchen können, wenn Sie Infrastruktur als Service (IaaS) verwenden, um Ihre Datenbankdienste hosten. Es wird erläutert, AlwaysOn Verfügbarkeit Gruppen, Datenbank Spiegelung, das Protokoll Liefer- und Sicherung und Wiederherstellung. Verschiedene Lernprogramme zeigen, wie diese Verfahren verwenden.

- [Bewährte Methoden für das Entwerfen von umfangreichen Dienste Azure-Cloud-Dienste auf dem Server](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Dieser Artikel befasst sich hochgradig skalierbare Cloudarchitekturen entwickeln. Viele der Techniken, mit denen Sie zur Verbesserung der Skalierbarkeit verbessern auch Verfügbarkeit. Wenn die Anwendung höhere Auslastung skaliert nicht möglich, wird Skalierbarkeit auch ein Problem mit der Verfügbarkeit.

- [Sichern und Wiederherstellen für SQL Server in Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Dieser Artikel bietet technischen Leitfaden zum Sichern und Wiederherstellen von Microsoft SQL Server auf Azure virtuellen Computern ausgeführt.

- [Failsafe: Leitfaden für robuste Cloudarchitekturen](https://channel9.msdn.com/Series/FailSafe).
Dieser Artikel enthält Leitfäden zum Erstellen von robuste Cloudarchitekturen Leitfäden zum Implementieren dieser Architekturen auf Microsoft-Technologien und Rezepte für die Durchführung dieser Architekturen für bestimmte Szenarien.

- [Technische Fallstudie: Verwenden der Cloud-Technologien zur Verbesserung der Wiederherstellung](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Diese Fallstudie zeigt an, wie Microsoft IT Azure zur Verbesserung der Wiederherstellung verwendet.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Serie technischen Leitfaden für Azure Stabilität dienten. Wenn Sie weitere Artikel in dieser Reihe lesen interessiert sind, können Sie mit der [Wiederherstellung aus lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md)starten.

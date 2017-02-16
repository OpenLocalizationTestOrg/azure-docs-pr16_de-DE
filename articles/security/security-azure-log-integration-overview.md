<properties
   pageTitle="Einführung in Microsoft Azure Log Integration | Microsoft Azure"
   description="Informationen Sie zu Azure Log Integration, deren wichtigsten Funktionen und deren Funktionsweise."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Einführung in Microsoft Azure Log Integration (Preview)

Informationen Sie zu Azure Log Integration, deren wichtigsten Funktionen und deren Funktionsweise.

## <a name="overview"></a>(Übersicht)

Plattform als Service (PaaS) und Infrastruktur als gehostet in Azure Service (IaaS) generieren eine große Datenmenge in Sicherheitsprotokollen. Diese Protokolle enthalten wichtigen Informationen, der Intelligence und leistungsfähige Einblicke in Richtlinienverstöße, internen und externen Risiken, behördliche Konformitätsproblemen und Abweichung der Netzwerk, Host und Benutzeraktivitäten bereitstellen kann.

Integration von Azure-Protokoll ermöglicht es Ihnen unformatierte Protokollen aus Azure Ressourcen in Ihrem lokalen Informationen zu Sicherheit und Ereignis Management (SIEM) Systeme integrieren. Azure Log Integration sammelt Azure-Diagnose von Ihrem Windows *(WAD)* virtuellen Computern sowie Diagnose von partnerlösungen wie eine Web Anwendung Firewall (WAF). Diese Integration bietet ein einheitliches Dashboard für alle Anlagen, lokal oder in der Cloud, damit Sie aggregieren, koordinieren, analysieren und für Sicherheitsereignisse benachrichtigen.

![Azure Log-integration][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle können werden integriert?

Azure erzeugt umfassende Protokollierung für jede Azure Service. Diese Protokolle werden nach zwei Hauptarten kategorisiert:

- **Control Management/Protokolle**, in denen verleihen Einblick in die Vorgänge Azure Ressourcenmanager erstellen, aktualisieren und löschen. Azure Überwachungsprotokolle ist ein Beispiel für diese Art von Protokoll.
- **Daten Ebene Protokolle**, in denen Einblick in das als Teil der Verwendung einer Azure Ressource ausgelöste Ereignisse zu verleihen. Beispiele für diese Art von Log sind die Windows-ereignisprotokollierung System, Sicherheit und Anwendung in einer virtuellen Computern protokolliert.

Integration Azure-Protokoll unterstützt derzeit Integration von Überwachungsprotokollen Azure, virtuellen Computern Protokolle und Azure-Sicherheitscenter Benachrichtigungen.

Wenn Sie Fragen zur Azure Log Integration haben, senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden Azure Log Integration vorgestellt. Wenn Sie weitere Informationen zur Integration Azure Log und welche Protokolle unterstützt, probieren Sie Folgendes ein:

- [Microsoft Azure Log Integration für Azure Protokolle (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center für Details, systemvoraussetzungen und Anweisungen Azure Log Integration installieren.
- [Erste Schritte mit Azure Log Integration](security-azure-log-integration-get-started.md) – in diesem Lernprogramm führt Sie durch die Installation der Azure Log Integration und Integration von Protokollen aus Azure-Speicher, Azure Überwachungsprotokolle und Sicherheitscenter Benachrichtigungen.
- [Partner Konfigurationsschritte](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – in diesem Blogbeitrag wird gezeigt, wie Azure Log-Integration mit partnerlösungen Splunk, HP ArcSight und IBM QRadar entwickelt konfigurieren.
- [Azure Log Integration häufig gestellte Fragen (FAQS)](security-azure-log-integration-faq.md) – diese häufig gestellte Fragen finden Sie Antworten auf Fragen zur Azure Log Integration.
- [Integration von Sicherheitscenter Benachrichtigungen mit Azure melden Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – dieses Dokument wird gezeigt, wie Sicherheitscenter Benachrichtigungen, zusammen mit virtuellen Computern Sicherheitsereignisse erfasst von Azure-Diagnose und Azure Überwachungsprotokolle, mit dem Log Analytics oder SIEM-Lösung zu synchronisieren.
- [Neue Features für Azure Diagnose und Azure Überwachungsprotokolle](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – in diesem Blogbeitrag lernen Sie Überwachungsprotokolle Azure und andere Features, die Ihnen helfen gewinnen Sie Einsichten in die Vorgänge Azure Ressourcen.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png

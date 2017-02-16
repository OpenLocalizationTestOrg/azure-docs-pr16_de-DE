<properties
    pageTitle="Behandeln von Problemen mit der gemeinsamen Verbindungsprobleme mit Azure SQL-Datenbank"
    description="Schritte zum Identifizieren und Beheben von häufigen Verbindung für Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Behandeln von Verbindungsproblemen mit Azure SQL-Datenbank

Wenn die Verbindung mit Azure SQL-Datenbank fehlschlägt, werden [Fehlermeldungen](sql-database-develop-error-messages.md)angezeigt. In diesem Artikel wird ein zentrales Thema, das Sie Azure SQL-Datenbank-Verbindungsproblemen hilft. Sie eine Einführung in Verbindungsprobleme [bewirkt, dass der gemeinsamen](#cause) , empfiehlt [ein Tool zur Problembehandlung](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , die Identität des Problems hilft, und zur Problembehandlung Schritte zum [vorübergehenden](#troubleshoot-transient-errors) [beständigen oder dauerhaftes Rechtschreibfehler](#troubleshoot-the-persistent-errors)und lösen. Schließlich werden [die betreffenden Artikel für Verbindungsprobleme Azure SQL-Datenbank](#all-topics-for-azure-sql-database-connection-problems)aufgelistet.

Wenn Sie die Verbindungsprobleme auftreten, versuchen Sie die Schritte Behandeln von Problemen, die in diesem Artikel beschrieben werden.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Ursache

Verbindungsproblemen möglicherweise eine der folgenden Ursachen haben:

- Fehler beim Anwenden von best Practices und Richtlinien für den Entwurf während des Entwurfsprozesses Anwendung.  Finden Sie unter [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) , um anzufangen.
- Azure SQL-Datenbank neu konfiguriert
- Firewall-Einstellungen
- Timeout für Verbindung
- Fehler bei der Anmeldung Informationen
- Maximale Anzahl erreicht wird, klicken Sie auf einige Ressourcen Azure SQL-Datenbank

Im Allgemeinen können Verbindungsprobleme mit Azure SQL-Datenbank wie folgt klassifiziert werden:

- [Vorübergehende Fehler (kurzlebige oder bei unterbrochener)](#troubleshoot-transient-errors)
- [Beständiger oder dauerhaftes Fehler (Fehler, die regelmäßig wiederholen)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Verwenden Sie die Problembehandlung für Verbindungsprobleme Azure SQL-Datenbank

Wenn einen bestimmte Verbindungsfehler auftritt, versuchen Sie [dieses Tool](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), das Ihnen hilft schnell Identität und beheben Sie das Problem zu.

## <a name="troubleshoot-transient-errors"></a>Behandeln von Problemen mit vorübergehende Fehler
Wenn eine Anwendung vorübergehende Fehler auftreten, überprüfen Sie Tipps zur Problembehandlung und Verringern der Häufigkeit von dieser Fehler in den folgenden Themen:

- [Problembehandlung bei Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist nicht verfügbar (Fehler: 40613)](sql-database-troubleshoot-connection.md)
- [Behandeln von Problemen mit, diagnose und verhindern, dass SQL-Verbindung und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Behandeln von Problemen mit beständigen Fehlern (dauerhaftes Fehler)

Wenn die Anwendung dauerhaft keine Verbindung Azure SQL-Datenbank herstellen, bedeutet dies in der Regel ein Problem mit einem der folgenden Aktionen aus:

- Konfiguration der Firewall. Die SQL Azure-Datenbank oder clientseitige Firewall blockiert Verbindungen mit Azure SQL-Datenbank.
- Netzwerk neu konfiguriert clientseitig: beispielsweise eine neue IP-Adresse oder einen Proxyserver.
- Benutzerfehler: z. B. falsch eingegeben Verbindungsparameter, wie etwa die Servernamen in der Verbindungszeichenfolge.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Schritte zum beständigen Verbindungsprobleme zu beheben.

1.  Einrichten von [Firewall-Regeln](sql-database-configure-firewall-settings.md) der Client IP-Adresse zulassen.
2.  Alle Firewalls zwischen dem Client und im Internet stellen Sie sicher, dass Anschluss 1433 für ausgehende Verbindungen geöffnet ist. Überprüfen Sie [konfigurieren die Windows-Firewall für SQL Server-Zugriff](https://msdn.microsoft.com/library/cc646023.aspx) für zusätzliche Zeiger ein.
3.  Überprüfen Sie die Verbindungszeichenfolge und andere Verbindungseinstellungen. Finden Sie im Abschnitt Verbindungszeichenfolge [Connectivity Probleme Thema](sql-database-connectivity-issues.md#connections-to-azure-sql-database)aus.
4.  Aktivieren Sie im Dashboard Dienststatus. Wenn Sie, dass die regionalen derzeit nicht zur Verfügung denken, finden Sie unter Schritten [nach einem Ausfall wiederherstellen](sql-database-disaster-recovery.md) , um eine neue Region wiederherzustellen.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Alle Themen Verbindungsproblemen Azure SQL-Datenbank

Die folgende Tabelle enthält jeder Verbindung Problem Thema, die direkt auf den Dienst Azure SQL-Datenbank angewendet wird.


| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Behandeln von Verbindungsproblemen mit Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) | Dies ist die Startseite zur Behandlung dieses Problems Verbindungsprobleme in Azure SQL-Datenbank. Es wird beschrieben, wie zu identifizieren und Beheben von vorübergehenden und beständigen oder dauerhaftes Fehlern in Azure SQL-Datenbank. |
| 2 | [Behandeln von Problemen mit, diagnose und verhindern, dass SQL-Verbindung und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md) | Informationen Sie zum Behandeln von Problemen mit, diagnose und verhindern, dass eine SQL-Verbindung oder vorübergehende Fehler in Azure SQL-Datenbank. |
| 3 | [Allgemeine vorübergehende Fehlerbehandlung – Anleitung](best-practices-retry-general.md) | Sind allgemeine Hinweise zum vorübergehenden Fehlerbehandlung an, bei der Verbindung mit Azure SQL-Datenbank. |
| 4 | [Behandeln von Verbindungsproblemen mit Microsoft Azure SQL-Datenbank](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Dieses Tool hilft Identität bei Verbindungsfehlern bei der die Lösung Ihres Problems. |
| 5 | [Behandeln von Problemen mit "Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist zurzeit nicht verfügbar. Wiederholen Sie die Verbindung zu einem späteren Zeitpunkt ein"zurück](sql-database-troubleshoot-connection.md) | Beschreibt, wie Sie identifizieren und Beheben von einem 40613 Fehler: "Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist zurzeit nicht verfügbar. Führen Sie die Verbindung zu einem späteren Zeitpunkt erneut." |
| 6 | [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank](sql-database-develop-error-messages.md) | Stellt Informationen zu SQL-Fehlercodes für SQL-Datenbank in Clientanwendungen, z. B. häufigen Datenbank Verbindung, Probleme beim Kopieren der Datenbank und allgemeine Fehler an. |
| 7 | [Azure SQL-Datenbank Leistung Anleitungen für einzelne Datenbanken](sql-database-performance-guidance.md) | Erfahren Sie, um Ihnen helfen festzustellen, welche Dienstebene für eine Anwendung richtig ist. Auch Leitfaden zur Optimierung Ihrer Anwendungs zu optimal nutzen Azure SQL-Datenbank. |
| 8 | [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) | Enthält Links zu Codebeispielen für verschiedene Technologien, die Sie zum Herstellen einer Verbindung mit und interagieren mit Azure SQL-Datenbank verwenden können. |
| 9 | Upgrade auf Azure SQL-Datenbank v12 Seite ([Azure-Portal](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Hier erfahren Sie, für die Aktualisierung von vorhandenen Azure SQL-Datenbank V11 Server und Datenbanken auf Azure SQL-Datenbank V12 mithilfe von Azure-Portal oder PowerShell. |


## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei Leistungsproblemen Azure SQL-Datenbank](sql-database-troubleshoot-performance.md)
- [Behandeln von Problemen mit Berechtigungen Azure SQL-Datenbank](sql-database-troubleshoot-permissions.md)
- [Anzeigen Sie aller Themen für den Dienst Azure SQL-Datenbank](sql-database-index-all-articles.md)
- [Suchen der Dokumentation auf Microsoft Azure](http://azure.microsoft.com/search/documentation/)
- [Zeigen Sie die neuesten Updates für den Dienst Azure SQL-Datenbank](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md)
- [Allgemeine vorübergehende Fehlerbehandlung – Anleitung](../best-practices-retry-general.md)
- [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)
- [Der Learning Path für die Verwendung von Azure SQL-Datenbank](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Der Learning Path für die Verwendung von Tools und flexible Datenbankfunktionen](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 

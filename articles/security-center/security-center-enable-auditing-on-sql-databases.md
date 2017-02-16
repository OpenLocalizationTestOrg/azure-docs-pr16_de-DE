<properties
   pageTitle="Aktivieren Sie die Überwachung auf SQL-Datenbanken in Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie der Azure-Sicherheitscenter empfohlen, **Aktivieren Sie die Überwachung auf SQL-Datenbanken**implementieren."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Aktivieren Sie die Überwachung auf SQL-Datenbanken in Azure-Sicherheitscenter

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie aktivieren Überwachung für alle SQL-Datenbanken, wenn die Überwachung nicht bereits aktiviert ist. Überwachung helfen Ihnen bei der Einhaltung von Richtlinien verwalten, Datenbankaktivität verstehen und Einblick abweichungen und Bildschirmdarstellung auftreten, die zeigt den Business Bedenken oder vermutet Sicherheitsverstöße.

Nachdem Sie die Überwachung aktiviert haben, können Sie Erkennung Einstellungen und e-Mails zum Empfangen von Sicherheitshinweisen konfigurieren. Erkennung erkennt abweichenden Datenbankaktivitäten potenzielle Sicherheitsrisiko für Ihr der Datenbank angibt. So können Sie erkennen und Antworten möglicherweise Risiken, sobald sie auftreten.

Diese Empfehlungen gilt für den SQL Azure-Dienst. SQL, die auf Ihre virtuellen Computern nicht eingeschlossen werden.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Auf SQL-Datenbanken Überwachung aktivieren**.  Daraufhin wird das Blade **Auf SQL-Datenbanken Überwachung aktivieren** .
![Aktivieren Sie die Überwachung auf SQL-Datenbanken][1]

2. Wählen Sie aus einer SQL-Datenbank auf Überwachung aktivieren. Dadurch wird das **Überwachung und Bedrohung Erkennung** Blade geöffnet.
![Überwachung und Gefahrenprofilen Erkennung][2]
3. Wählen Sie auf die **Überwachung und Bedrohung Erkennung** Blade **auf** unter **Überwachung**aus.
![Überwachung und Gefahrenprofilen Erkennung aktivieren][3]


5. Führen Sie die Schritte in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) zu aktivieren und Konfigurieren der Erkennung und die Liste der e-Mail-Nachrichten konfiguriert werden, die von Sicherheitshinweisen bei Erkennen eines außergewöhnlichen Aktivitäten erhalten soll.

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie das Sicherheitscenter Empfehlungen implementiert "Auf SQL-Datenbanken Überwachung aktivieren". Weitere Informationen zum Schutz Ihrer SQL-Datenbank, probieren Sie Folgendes ein:

- [Sichern Ihrer SQL­Datenbank](../sql-database/sql-database-security.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes an.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png

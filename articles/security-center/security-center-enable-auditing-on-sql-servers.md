<properties
   pageTitle="Aktivieren Sie die Überwachung auf Servern SQL Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie **Aktivieren Sie die Überwachung auf Servern mit SQL**Azure-Sicherheitscenter empfohlen implementieren."
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

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Aktivieren Sie die Überwachung auf Servern SQL Azure-Sicherheitscenter

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie aktivieren für alle Datenbanken auf Ihre SQL Azure-Servern Überwachung, wenn die Überwachung nicht bereits aktiviert ist. Überwachung helfen Ihnen bei der Einhaltung von Richtlinien verwalten, Datenbankaktivität verstehen und Einblick abweichungen und Bildschirmdarstellung auftreten, die zeigt den Business Bedenken oder vermutet Sicherheitsverstöße.

Nachdem Sie die Überwachung aktiviert haben, können Sie Erkennung Einstellungen und e-Mails zum Empfangen von Sicherheitshinweisen konfigurieren. Erkennung erkennt abweichenden Datenbankaktivitäten potenzielle Sicherheitsrisiko für Ihr der Datenbank angibt. So können Sie erkennen und Antworten möglicherweise Risiken, sobald sie auftreten.

Diese Empfehlungen gilt für den SQL Azure-Dienst. Diese nicht SQL Server, die auf Ihre virtuellen Computern in Azure-Infrastrukturdiensten (Azure IaaS) enthalten sind.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Auf SQL-Servern Überwachung aktivieren**.  Daraufhin wird das Blade **Auf SQL-Servern Überwachung aktivieren** .
![Aktivieren Sie die Überwachung auf SQL Server][1]

2. Wählen Sie eine SQLServer auf Überwachung aktivieren aus. Das Blade **Überwachung Settings** wird geöffnet.
![Überwachungseinstellungen][2]
3. Wählen Sie auf der Blade **Überwachung Einstellungen** **auf** unter **Überwachung**aus.
![Überwachungseinstellungen aktivieren][3]

4. Führen Sie die Schritte zur Konfiguration von Speicher [Erste Schritte mit SQL-Datenbank Überwachung](../sql-database/sql-database-auditing-get-started.md) , wo der Überwachungsprotokolle gespeichert werden. Das Abonnement des Speicher-Konto zum Sammeln von Daten wird standardmäßig Speicher.

5. Führen Sie die Schritte in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) zu aktivieren und Konfigurieren der Erkennung und die Liste der e-Mail-Nachrichten konfiguriert werden, die von Sicherheitshinweisen bei Erkennen eines außergewöhnlichen Aktivitäten erhalten soll.

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie das Sicherheitscenter Empfehlungen implementiert "Auf SQL-Servern Überwachung aktivieren". Weitere Informationen zum Schutz Ihrer SQL-Datenbank, probieren Sie Folgendes ein:

- [Sichern Ihrer SQL­Datenbank](../sql-database/sql-database-security.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png

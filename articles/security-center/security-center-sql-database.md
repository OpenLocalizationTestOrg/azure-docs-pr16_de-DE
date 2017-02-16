<properties
   pageTitle="Azure-Sicherheitscenter und Azure SQL-Datenbank-Dienst | Microsoft Azure"
   description="In diesem Artikel wird das Sicherheitscenter die Datenbanken in Azure SQL-Datenbank sichern, helfen Sie."
   services="sql-database"
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
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure-Sicherheitscenter und Azure SQL-Datenbank-Dienst

[Azure Sicherheitscenter](https://azure.microsoft.com/documentation/services/security-center/) hilft Ihnen der verhindern, erkennen und Beantworten von Risiken. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

In diesem Artikel wird das Sicherheitscenter die Datenbanken in Azure SQL-Datenbank sichern, helfen Sie.

## <a name="why-use-security-center"></a>Gründe für die Verwendung von Sicherheitscenter

Sicherheitscenter können Sie die Daten in SQL-Datenbank zu sichern, indem Sie Einblick in die Sicherheit aller Server und Datenbanken. Mit Sicherheitscenter können Sie folgende Aktionen ausführen:

- Definieren von Richtlinien für SQL-Datenbank-Verschlüsselung und Überwachung.
- Überwachen Sie die Sicherheit des SQL-Datenbank Ressourcen über alle Ihrer Abonnements.
- Schnell erkennen und Beheben von Sicherheitsproblemen.
- Integrieren von Benachrichtigungen von [Erkennung Azure SQL-Datenbank](../sql-database/sql-database-threat-detection-get-started.md).

Nicht nur Ressourcen SQL-Datenbank zu schützen, bietet Sicherheitscenter auch Sicherheit die Überwachung und Verwaltung für Azure-virtuellen Computern, Cloud Services, App Services, virtuelle Netzwerke und vieles mehr. Erfahren Sie mehr über das Sicherheitscenter [hier](security-center-intro.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um mit Sicherheitscenter beginnen, müssen Sie ein Microsoft Azure-Abonnement verfügen. Die kostenlose Ebene der Sicherheitscenter ist mit Ihrem Abonnement aktiviert. Weitere Informationen zum Sicherheitscenter der frei / Standard Ebenen finden Sie unter [Sicherheit Center Preise](https://azure.microsoft.com/pricing/details/security-center/).

Rollenbasierte Access unterstützt das Sicherheitscenter. Weitere Informationen zu Access rollenbasierte Steuerelement (RBAC) in Azure finden Sie unter [Azure Active Directory rollenbasierte Access Control](../active-directory/role-based-access-control-configure.md). Die Sicherheit Center häufig gestellte Fragen zu enthält Informationen zur [wie Berechtigungen in Sicherheitscenter behandelt werden](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Access-Sicherheitscenter

Sie auf Sicherheitscenter aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. [Melden Sie sich mit dem Portal](https://portal.azure.com/) , und wählen Sie die **Option für das Sicherheitscenter**.

![Option für das Sicherheitscenter][1]

Das **Sicherheitscenter** Blade wird geöffnet.
![Sicherheitscenter blade][2]

## <a name="set-security-policy"></a>Festgelegte Sicherheitsrichtlinie

Eine Sicherheitsrichtlinie definiert die Steuerelemente, die für Ressourcen innerhalb des angegebenen Abonnement oder Ressourcengruppe vorgeschlagen werden. Im Sicherheitscenter definieren Sie Richtlinien für Ihre Abonnements oder Ressourcengruppen gemäß Ihres Unternehmens Sicherheit Anforderungen und Anwendungstyp oder Vertraulichkeit der Daten in jedes Abonnement.

Sie können eine Richtlinie anzeigen Empfehlungen für SQL-Überwachung und SQL-transparent Daten-Verschlüsselung (TDE) festlegen.

- Wenn Sie **SQL-Überwachung und Erkennung**aktivieren, empfiehlt Sicherheitscenter, Überwachung des Zugriffs auf Azure-Datenbank für Compliance, Erweiterte Erkennung und Untersuchung Zwecke aktiviert werden.
- Beim **SQL transparent Daten Verschlüsselung**, Sicherheitscenter empfiehlt einschalten werden, dass die Verschlüsselung statisch sind für Ihre Azure SQL-Datenbank, zugeordneten Sicherungskopien und Transaktionsprotokolldateien aktiviert.

Um eine Sicherheitsrichtlinie festzulegen, wählen Sie die **Richtlinie** Kachel auf das Sicherheitscenter Blade aus. Wählen Sie das Abonnement, an dem Sie die Sicherheitsrichtlinie aktivieren möchten, klicken Sie auf das Blade **Sicherheitsrichtlinie** . Wählen Sie **Prevention Richtlinie** aus, und **Aktivieren Sie den Mittelpunkt, die Sie auf dieses Abonnement verwenden möchten** .
![Sicherheitsrichtlinie][3]

Weitere Informationen finden Sie unter [Festlegen von Sicherheitsrichtlinien](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Verwalten der Sicherheit Empfehlungen

Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen. Die Empfehlungen führen Sie durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.

Nach dem Festlegen einer Sicherheitsrichtlinie analysiert Sicherheitscenter den Sicherheitsstatus Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken aus. Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlungen darstellt. Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie grundlegende Informationen zu den verfügbaren Empfehlungen für Azure SQL-Datenbank und Funktionsweise jedes Empfehlungen, wenn Sie es anwenden. Auswählen eines Empfehlungen wird führt zu einem Artikel, der erläutert, wie die Empfehlungen im Sicherheitscenter implementiert.

| Empfehlungen | Beschreibung |
| ----- | ----- |
| [Aktivieren Sie die Überwachung und Gefahrenprofilen Erkennung auf SQL Server](security-center-enable-auditing-on-sql-servers.md) | Empfiehlt, dass Sie die Überwachung und Gefahrenprofilen Erkennung für SQL-Datenbankserver aktivieren. (Nur für SQL-Datenbank-Dienst. Nicht sind Microsoft SQL Server, die auf Ihre virtuellen Computern.) |
| [Aktivieren Sie die Überwachung und Gefahrenprofilen Erkennung auf SQL-Datenbanken](security-center-enable-auditing-on-sql-databases.md) | Empfiehlt, dass Sie die Überwachung und Gefahrenprofilen Erkennung für Datenbanken SQL-Datenbank aktivieren. (Nur für SQL-Datenbank-Dienst. Nicht sind Microsoft SQL Server, die auf Ihre virtuellen Computern.) |
| [Aktivieren Sie die Verschlüsselung der Daten als Transparent](security-center-enable-transparent-data-encryption.md) | Empfiehlt, dass Sie die Verschlüsselung für SQL-Datenbanken aktivieren. (Nur SQL-Datenbank-Dienst.) |

Um empfohlenen für Ihre Azure-Ressourcen anzuzeigen, wählen Sie die Kachel **Empfehlungen** auf das Sicherheitscenter Blade aus. Wählen Sie auf das Blade **Empfehlungen** empfohlen, um Details anzuzeigen. Wählen Sie in diesem Beispiel uns **Überwachung aktivieren & Erkennung auf SQL-Servern**.

![Empfehlungen][4]

Wie unten dargestellt, zeigt das Sicherheitscenter, die SQL Server, Überwachung und Gefahrenprofilen Erkennung nicht aktiviert. Nachdem Sie die Überwachung aktivieren, können Sie die Einstellungen Erkennung und e-Mail-Einstellungen zum Empfangen von Sicherheitshinweisen konfigurieren. Erkennung benachrichtigt Sie, wenn das Programm erkennt abweichenden Datenbankaktivitäten, die die Datenbank potenzielle Sicherheitsrisiko für Ihr angeben. Die Benachrichtigungen werden im Sicherheitscenter Dashboard angezeigt.
![Überwachung und Gefahrenprofilen Erkennung][5]

Führen Sie die Schritte in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) zu aktivieren und Konfigurieren der Erkennung und die Liste der e-Mail-Nachrichten konfiguriert werden, die von Sicherheitshinweisen bei Erkennen eines außergewöhnlichen Aktivitäten erhalten soll.

Weitere Informationen zum Empfehlungen finden Sie unter [Verwalten von Sicherheit Empfehlungen](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Überwachen Sicherheit Systemzustands

Nachdem Sie [Sicherheitsrichtlinien](security-center-policies.md) für Ressourcen ein Abonnement aktiviert haben, wird das Sicherheitscenter die Sicherheit Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken analysieren.  Sie können den Sicherheitsstatus von Ressourcen in der Kachel **Ressource Sicherheit Systemzustand** anzeigen. Wenn Sie **Daten** in der Kachel **Ressource Sicherheit Dienststatus** klicken, wird geöffnet, das **Datenressourcen** Blade SQL-Empfehlungen für Probleme, wie z. B. ü und transparente Daten Verschlüsselung nicht aktiviert. Sie hat auch Empfehlungen für die allgemeine Integritätsstatus der Datenbank.
![Ressource Sicherheit Dienststatus][6]

Weitere Informationen finden Sie unter [Sicherheit Dienststatus überwachen](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Verwalten und Beantworten von Sicherheitshinweisen

Sicherheitscenter automatisch erfasst, analysiert und Integration von Log-Daten aus [SQL Azure Erkennung](../sql-database/sql-database-threat-detection-get-started.md)sowie weitere Azure Ressourcen, reale Risiken erkennen und falsche positive zu verringern. Eine Liste der Priorität von Sicherheitshinweisen wird im Sicherheitscenter zusammen mit den Informationen angezeigt, Sie das Problem und Vorschläge zur Behebung Angriffen wie schnell zu ermitteln müssen.

Wählen Sie Benachrichtigungen finden Sie die Kachel **von Sicherheitshinweisen** auf das Sicherheitscenter Blade aus. Wählen Sie eine Benachrichtigung, um weitere Informationen zu den Ereignissen, die ausgelöst die Benachrichtigung und wurde, wenn vorhanden, Schritte Sie zur Behebung von Angriffen durchführen müssen, klicken Sie auf das Blade **von Sicherheitshinweisen** . Wählen Sie in diesem Beispiel uns **potenzieller SQL einfügen**aus.
![Von Sicherheitshinweisen][7]

Wie unten dargestellt,-Sicherheitscenter können Sie weitere Details, die einen Einblick in was die Warnung, die Zielressource, falls zutreffend ausgelöst anbieten die Quelle IP-Adresse und Empfehlungen im Zusammenhang mit Informationen zum beheben.
![Mögliche SQL einfügen][8]

Weitere Informationen finden Sie unter [Verwaltung und Beantworten von Sicherheitshinweisen](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Nächste Schritte

- [Sicherheit Center häufig gestellte Fragen](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Leitfaden für das Sicherheitscenter Planung und Betrieb](security-center-planning-and-operations-guide.md) – folgen eine Reihe von Schritte und Aufgaben zum Optimieren Ihrer Verwendung von Sicherheitscenter basierend auf Sicherheit Anforderungen und in der Cloud Management-Modell Ihres Unternehmens.
- [Sicherheit Center-Datenschutz](security-center-data-security.md) – erfahren Sie, wie das Sicherheitscenter sammelt und verarbeitet Daten zu Ihren Azure Ressourcen, einschließlich Informationen zur Konfiguration, Metadaten, Ereignisprotokollen, Absturzabbilddateien und mehr.
- [Behandlung von Sicherheitsvorfällen](security-center-incident.md) – erfahren Sie, wie die Sicherheit benachrichtigen Möglichkeit zur Unterstützung bei der Behandlung von Sicherheitsvorfällen im Sicherheitscenter zu verwenden.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png

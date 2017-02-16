<properties
    pageTitle="Zum Verwalten der Sicherheit nach dem Wiederherstellen einer Datenbank auf einem neuen Server oder weiß nicht über eine Datenbank zum einer sekundären Datenbank kopieren | Microsoft Azure"
    description="In diesem Thema wird erläutert, Sicherheitsaspekte zum Verwalten der Sicherheit nach einer Datenbank wiederherstellen oder ein Failover."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />

# <a name="how-to-manage-azure-sql-database-security-after-disaster-recovery"></a>Zum Verwalten der Sicherheit Azure SQL-Datenbank nach der Wiederherstellung

>[AZURE.NOTE] [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) ist nun für alle Datenbanken auf allen Ebenen der Dienst verfügbar.

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a>Übersicht der authentifizierungsanforderungen für die Wiederherstellung

In diesem Thema werden zum Konfigurieren die authentifizierungsanforderungen und Steuerelement [Aktiven Geo-Replikation](sql-database-geo-replication-overview.md) und die Schritte zum Einrichten des Benutzerzugriffs auf die sekundäre Datenbank erforderlich. Darüber hinaus werden wie Aktivieren des Zugriffs auf die wiederhergestellte Datenbank nach der Verwendung von [Geo-wiederherstellen](sql-database-recovery-using-backups.md#geo-restore). Weitere Informationen Wiederherstellungsoptionen finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>Wiederherstellung mit enthaltenen Benutzer

Im Gegensatz zu herkömmlichen Benutzer, die Benutzernamen in der master-Datenbank zugeordnet sein müssen, ist ein enthaltenen Benutzer vollständig von der Datenbank selbst verwaltet. Dies hat zwei Vorteile. Die Benutzer können weiterhin für die Verbindung mit der neuen primären Datenbank im Szenario zur Wiederherstellung nach oder die Datenbank wiederhergestellt Geo-wiederherstellen, ohne zusätzliche Konfiguration, verwenden, da die Datenbank Benutzer verwaltet werden. Es gibt auch potenzielle Skalierbarkeit und Leistungsvorteile von dieser Konfiguration im Hinblick auf Login. Weitere Informationen finden Sie unter [Enthaltenen Datenbankbenutzer - ausführenden Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx). 

Das Hauptfenster Verhältnis besteht darin, Verwalten der Wiederherstellung bei schwieriger. Wenn Sie mehrere Datenbanken, die das gleiche Login verwenden haben, möglicherweise die Wartung der Anmeldeinformationen enthaltenen Benutzer in mehreren Datenbank die Vorteile von enthaltenen Benutzer Negation. Die Drehung Kennwortrichtlinien erfordert beispielsweise, dass mehrere Datenbanken konsistent geändert werden und nicht ändern des Kennworts für die Anmeldung nur ein Mal in der master-Datenbank. Daher Wenn Sie mehrere Datenbanken haben, die denselben Benutzernamen und Kennwort verwenden enthaltenen Benutzer nicht empfiehlt verwenden. 

## <a name="how-to-configure-logins-and-users"></a>So konfigurieren Sie Benutzernamen und Benutzern

Wenn Sie Benutzernamen und Benutzern verwenden (im Gegensatz zu enthaltenen Benutzer), achten Sie zusätzliche Schritte, um sicherzustellen, dass die gleichen Benutzernamen in der master-Datenbank vorhanden sein. Den folgenden Abschnitten werden die Schritte verbindet und weitere Aspekte.

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>Einrichten des Benutzerzugriffs mit einer sekundären oder wiederhergestellte Datenbank

Damit für die sekundäre Datenbank korrekten Zugriff auf die neue primäre Datenbank oder die Datenbank mit Geo-wiederherstellen wiederhergestellt sicherzustellen und als schreibgeschützt sekundäre Datenbank verwendet werden kann müssen der master-Datenbank mit dem Ziel-Server die entsprechenden Security Konfiguration vor der Wiederherstellung.

Weiter unten in diesem Thema werden die spezifischen Berechtigungen für jeden Schritt beschrieben.

Vorbereiten des Benutzerzugriffs auf einem sekundären Geo-Replikation sollte Rahmen Geo-Replikationsoptionen ausgeführt werden. Vorbereiten des Benutzerzugriffs auf die Datenbanken Geo wiederhergestellt sollte ausgeführt werden, wenn der ursprüngliche Server online ist, jederzeit (z. B. als Teil der DR Drillup).

>[AZURE.NOTE] Wenn Sie Failover oder Geo-wiederherstellen auf einem Server, der nicht ordnungsgemäß konfigurierten Benutzernamen den Zugriff darauf verfügt auf den Server-Administrator-Konto beschränkt wird.

Einrichten von Benutzernamen auf dem Zielserver umfasst drei nachfolgend beschriebenen Schritte aus:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. ermitteln Sie Benutzernamen mit Zugriff auf die primäre Datenbank:
Im erste Schritt des Prozesses ist, um zu bestimmen, welche Benutzernamen auf dem Ziel-Server dupliziert werden müssen. Dies geschieht mit zwei SELECT-Anweisungen, in der logischen master-Datenbank auf dem Quellserver und Zeile in der primären Datenbank selbst.

Der Serveradministrator oder als Mitglied der Rolle **LoginManager** Server kann die Benutzernamen auf dem Quellserver mit der folgenden SELECT-Anweisung bestimmen. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Nur ein Mitglied der Rolle Db_owner Datenbank, die Benutzeranmeldekonto oder Serveradministrator, kann alle die Datenbank Benutzer Hauptbenutzer in der primären Datenbank bestimmen.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. finden Sie die SID für den Benutzernamen, die in Schritt 1:
Vergleich der Unterschiede bei der Ausgabe der Abfragen aus dem vorherigen Abschnitt, und die SIDs übereinstimmenden, kann das Server Login Datenbankbenutzer zugeordnet werden. Benutzernamen, die einen Datenbankbenutzer mit übereinstimmende SID aufweisen haben Benutzerzugriff auf diese Datenbank als die wichtigsten Datenbankbenutzer. 

Die folgende Abfrage kann verwendet werden, um die Benutzer Hauptbenutzer und deren SIDs in einer Datenbank angezeigt. Nur ein Mitglied der Db_owner Datenbank Rolle oder Server-Administrator kann diese Abfrage ausführen.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

>[AZURE.NOTE] Die Benutzer **INFORMATION_SCHEMA** und **Sys** *NULL* SIDs haben, und der **Gast** SID ist **0 x 00**. **Dbo** SID möglicherweise über *0x01060000000001648000000000048454*, starten, wenn der Datenbankersteller der Serveradministrator, statt Mitglied **DbManager**wurde.

#### <a name="3-create-the-logins-on-the-target-server"></a>3 Erstellen Sie 3 die Benutzernamen auf dem Ziel-Server ein:
Der letzte Schritt darin ist, wechseln Sie zu der Ziel-Server oder Servern und die Benutzernamen mit den entsprechenden SIDs generieren. Die grundlegende Syntax ist wie folgt aus.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

>[AZURE.NOTE] Wenn Sie den Benutzerzugriff auf den sekundären gewähren möchten, aber nicht mit dem primären, Sie dies können, indem Sie auf dem primären Server der Anmeldung des Benutzers mithilfe der folgenden Syntax ändern.
>
>ALTER LOGIN <login name> deaktivieren
>
>Deaktivieren nicht das Kennwort ein, sodass Sie immer es aktivieren können bei Bedarf zu ändern.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Verwalten von Datenbankzugriff und Benutzernamen, finden Sie unter [Sicherheit SQL-Datenbank: Verwalten Datenbank Zugriff und Login Sicherheit](sql-database-manage-logins.md).
- Weitere Informationen in der enthaltenen Datenbankbenutzer finden Sie unter [Enthaltenen Datenbankbenutzer – Bereitstellen von Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx).
- Informationen zur Verwendung und Konfiguration der aktiven Geo-Replikation finden Sie unter [Aktive Geo-Replikation](sql-database-geo-replication-overview.md)
- Informationen zur Verwendung von Geo-Wiederherstellen finden Sie unter [Geo-wiederherstellen](sql-database-recovery-using-backups.md#geo-restore)

## <a name="additional-resources"></a>Zusätzliche Ressourcen


<properties
   pageTitle="Authentifizierung in SQL Azure Datawarehouse | Microsoft Azure"
   description="Azure Active Directory (AAD) und SQL Server-Authentifizierung in Azure SQL-Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Authentifizierung in SQL Azure Datawarehouse

> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Verbinden mit SQL Data Warehouse müssen Sie Sicherheitsanmeldeinformationen zwecks Authentifizierung übergeben. Beim Herstellen einer Verbindung, sind bestimmte Verbindungseinstellungen als Teil Ihrer Abfrage-Sitzung konfiguriert.  

Weitere Informationen zu Sicherheit und wie Sie Verbindungen zu Ihrem Datawarehouse aktivieren finden Sie unter [Secure einer Datenbank in SQL Data Warehouse][].

## <a name="sql-authentication"></a>SQL-Authentifizierung
Verbinden mit SQL Data Warehouse müssen Sie die folgende Informationen angeben:

- Vollständig qualifizierte servername
- Geben Sie die SQL-Authentifizierung
- Benutzername
- Kennwort
- Standarddatenbank (optional)

Standardmäßig wird die Verbindung mit der *master* -Datenbank und nicht die Benutzerdatenbank verbunden. Um zu Ihrer Benutzerdatenbank herstellen, können Sie auswählen, eine der beiden folgenden Aktionen ausführen:

- Geben Sie die Standarddatenbank aus, wenn den Server mit dem SQL Server-Objekt-Explorer SSDT, SSMS, oder in der Verbindungszeichenfolge Ihrer Anwendung zu registrieren. Nehmen Sie beispielsweise den InitialCatalog-Parameter für eine ODBC-Verbindung.
- Markieren Sie die Benutzerdatenbank, bevor Sie eine Sitzung in SSDT erstellen.

> [AZURE.NOTE] **Verwenden Sie meine Datenbank;** die Transact-SQL-Anweisung wird zum Ändern der Datenbank für eine Verbindung nicht unterstützt. Leitfaden für die Verbindung mit SQL Data Warehouse mit SSDT finden Sie im Artikel [Abfrage mit Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) Authentifizierung

[Azure-Active Directory] [ What is Azure Active Directory] Authentifizierung ist ein Verfahren der Herstellen einer Verbindung mit Microsoft Azure SQL-Data Warehouse Identitäten in Azure Active Directory (Azure AD) verwenden. Mit Azure-Active Directory-Authentifizierung können Sie die Identitäten der Datenbankbenutzer und anderen Microsoft-Diensten an einem zentralen Ort zentral verwalten. Zentrale Verwaltung von ID bietet eine zentrale Stelle zum Verwalten von Benutzern SQL Data Warehouse und vereinfacht die berechtigungsverwaltung. 

### <a name="benefits"></a>Vorteile

Azure Active Directory bietet folgende Vorteile:

- Stellt eine Alternative zum SQL Server-Authentifizierung.
- Hilft Beenden der zu viele von Benutzeridentitäten über Datenbankserver.
- Diese Berechtigung ermöglicht Kennwort Drehung in einem einzigen Ort
- Verwalten von externen (AAD) Entwurfsphase Datenbankberechtigungen.
- Beseitigt Speichern von Kennwörtern durch integrierte Windows-Authentifizierung und anderen Formen von Azure Active Directory unterstützten Authentifizierungsmethoden aktivieren.
- Verwendung enthaltenen Datenbankbenutzer authentifizieren Identitäten Ebene der Datenbank.
- Token-basierte Authentifizierung unterstützt für Applikationen, die Verbindung mit SQL Data Warehouse.
- Unterstützt die kombinierte Authentifizierung über Universal Active Directory-Authentifizierung für SQL Server Management Studio. Eine Beschreibung der kombinierte Authentifizierung finden Sie unter [SSMS für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse unterstützt](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure-Active Directory ist immer noch relativ neu und gelten einige Einschränkungen. Um sicherzustellen, dass Azure Active Directory für Ihre Umgebung geeignet ist, finden Sie unter [Azure AD-Features und Einschränkungen][], insbesondere der weiteren Aspekte zu berücksichtigen.

### <a name="configuration-steps"></a>Konfigurationsschritte

Wie folgt vor, um die Azure Active Directory-Authentifizierung konfigurieren.

1. Erstellen und Füllen einer Azure-Active Directory
2. Optional: Zuordnen oder Ändern von active Directory, das derzeit mit Ihrem Azure-Abonnement verknüpft ist
3. Erstellen Sie einen Azure Active Directory-Administrator für Azure SQL-Data Warehouse.
4. Konfigurieren der Clientcomputer
5. Erstellen Sie in Ihrer Datenbank zugeordneten Azure AD-Identitäten enthaltenen Datenbankbenutzer
6. Verbinden Sie mit Ihr Datawarehouse mithilfe von Azure AD-Identitäten

Azure-Active Directory-Benutzer sind derzeit nicht im SSDT Objekt-Explorer angezeigt. Anzeigen von Benutzer in [Kataloganzeige](https://msdn.microsoft.com/library/ms187328.aspx)Problem zu umgehen.
  
### <a name="find-the-details"></a>Suchen Sie die details
- Führen Sie die detaillierten Schritte aus. Die einzelnen Schritte zum Konfigurieren und Verwenden der Azure-Active Directory-Authentifizierung sind für Azure SQL-Datenbank und Azure SQL-Data Warehouse nahezu identisch. Führen Sie die detaillierten Schritte im Thema [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse durch Verwenden von Azure Active Directory-Authentifizierung](../sql-database/sql-database-aad-authentication.md)aus.
- Erstellen von benutzerdefinierten Datenbankrollen und Hinzufügen von Benutzern, die die Rollen. Weisen Sie präzise Berechtigungen für die Rollen. Weitere Informationen finden Sie unter [Erste Schritte mit der Datenbank-Engine-Berechtigungen](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Nächste Schritte

Um Ihr Datawarehouse mit Visual Studio und anderen Clientanwendungen Abfragen zu starten, finden Sie unter [Abfrage mit Visual Studio][].

<!-- Article references -->
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Abfrage mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD-Features und Einschränkungen]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations

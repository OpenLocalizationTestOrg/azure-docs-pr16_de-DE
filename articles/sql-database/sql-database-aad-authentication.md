<properties
   pageTitle="Verbinden mit SQL-Datenbank oder SQL Datawarehouse mithilfe von Azure-Active Directory-Authentifizierung | Microsoft Azure"
   description="Erfahren Sie, wie Sie mithilfe von Azure-Active Directory-Authentifizierung mit SQL-Datenbank zu verbinden."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Herstellen einer Verbindung mit SQL-Datenbank oder die Datawarehouse SQL Azure-Active Directory-Authentifizierung verwenden

Azure Active Directory-Authentifizierung wird der Herstellen einer Verbindung mit Microsoft Azure SQL-Datenbank und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Identitäten in Azure Active Directory (Azure AD) verwenden. Mit Azure-Active Directory-Authentifizierung können Sie die Identitäten der Datenbankbenutzer und anderen Microsoft-Diensten an einem zentralen Ort zentral verwalten. Zentrale Verwaltung von ID bietet eine zentrale Stelle zum Verwalten von Datenbankbenutzer und vereinfacht die berechtigungsverwaltung. Die folgenden: Vorteile

- Es stellt eine Alternative zum SQL Server-Authentifizierung.
- Hilft Beenden der zu viele von Benutzeridentitäten über Datenbankserver.
- Diese Berechtigung ermöglicht Kennwort Drehung in einem einzigen Ort
- Kunden können externe (AAD) Entwurfsphase Database-Berechtigungen verwalten.
- Diese kann Speichern von Kennwörtern auszuschließen, indem Sie integrierte Windows-Authentifizierung und anderen Formen von Azure Active Directory unterstützten Authentifizierungsmethoden aktivieren.
- Azure Active Directory-Authentifizierung verwendet enthaltenen Datenbankbenutzer authentifizieren Identitäten Ebene der Datenbank.
- Azure Active Directory unterstützt Token-basierte Authentifizierung für Applikationen Herstellen einer Verbindung mit einer SQL-Datenbank.
- Azure Active Directory-Authentifizierung unterstützt ADFS (Domänenverbund) oder native Benutzerkennwort-Authentifizierung für eine lokale Azure Active Directory ohne Domänen synchronisieren.  
- Azure-Active Directory unterstützt Verbindungen aus SQL Server Management Studio, bei denen Active Directory Universal-Authentifizierung, wozu auch mehrstufige Authentifizierung (MFA).  MFA enthält strenge Authentifizierung für eine Reihe von Überprüfungsoptionen für einfache – Anruf, Textnachricht, Smart-Karten mit Pin oder mobile-app-Benachrichtigung. Weitere Informationen finden Sie unter [SSMS für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse unterstützt](sql-database-ssms-mfa-authentication.md).

Die Konfigurationsschritte gehören die folgenden Verfahren zum Konfigurieren und Verwenden der Azure-Active Directory-Authentifizierung.

1. Erstellen und Füllen einer Azure-Active Directory.
2. Stellen Sie sicher, dass die Datenbank in Azure SQL-Datenbank V12 ist. (Nicht erforderlich für SQL Data Warehouse).
3. Optional: Zugeordnet werden soll, oder Ändern von active Directory, das derzeit mit Ihrem Azure-Abonnement verknüpft ist.
4. Erstellen Sie ein Azure Active Directory-Administrator für Azure SQL Server- oder [Azure SQL-Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)ein.
5. Konfigurieren der Clientcomputer an.
6. Erstellen Sie enthaltenen Datenbankbenutzer in Ihrer Datenbank Azure AD-Identitäten zugeordnet.
7. Verbinden Sie mit Ihrer Datenbank mithilfe von Azure AD-Identitäten.


## <a name="trust-architecture"></a>Architektur vertrauen

Das folgende auf hoher Ebene Diagramm enthält eine Übersicht über die Lösungsarchitektur der Verwendung von Azure AD-Authentifizierung mit Azure SQL-Datenbank. Die gleichen Konzepte gelten für SQL Data Warehouse. Zur Unterstützung der systemeigenen Azure Active Directory-Benutzerkennwort nur die Cloud Teil und Azure AD/Azure SQL-Datenbank eingestuft werden. Zur Unterstützung von verbundene Authentifizierung (oder Benutzerkennwort für Windows-Anmeldeinformationen), die Kommunikation mit ADFS blockieren ist erforderlich. Die Pfeile zeigen Kommunikationswege.

![AAD autorisierende Diagramm][1]

Das folgende Diagramm zeigt die Föderation, Vertrauenswürdigkeit und Hostinganbieter Beziehungen, mit denen eine Verbindung zu einer Datenbank, indem Sie ein Token an. Das Token wird von einer Azure Active Directory authentifiziert und wird von der Datenbank als vertrauenswürdig eingestuft. Kunden 1 kann es sich um eine Azure Active Directory für systemeigene Benutzer oder eine Azure-Active Directory partnerverbundkontakte-Benutzer darstellen. Kunden 2 darstellt eine mögliche Lösung, einschließlich der importierten Benutzer; In diesem Beispiel wird in Kürze aus einem verbundenen Azure Active Directory mit ADFS mit Azure Active Directory synchronisiert werden. Es ist wichtig zu verstehen, dass der Zugriff auf eine Datenbank mithilfe der Azure AD-Authentifizierung erforderlich ist, dass das Abonnement Hostinganbieter mit Azure Active Directory verknüpft ist. Im selben Abonnement muss verwendet werden, die SQL Server als Host für die Azure SQL-Datenbank oder SQL Data Warehouse erstellen.

![Abonnement Beziehung][2]

## <a name="administrator-structure"></a>Administrator Struktur

Bei Verwendung von Azure AD-Authentifizierung, gibt es zwei Administrator-Konten für den SQL-Datenbankserver; die ursprüngliche SQL Server-Administrator und Azure AD-Administrator. Die gleichen Konzepte gelten für SQL Data Warehouse. Nur der Administrator basierend auf einem Azure AD-Konto kann den ersten Azure AD-enthaltenen Datenbankbenutzer in einer Datenbank erstellen. Die Azure AD-Administratorbenutzernamen kann ein Azure AD-Benutzer oder eine Gruppe Azure AD-sein. Wenn der Administrator das Konto einer Gruppe ist, können sie durch alle Gruppenmitglieder, sodass mehrere Azure AD-Administratoren für SQL Server-Instanz verwendet werden. Mit der Gruppenkonto als Administrator kann Verwaltbarkeit verbessert werden, sodass Sie zentral hinzufügen und Entfernen von Mitglieder in Azure AD ohne Ändern der Benutzer oder der Berechtigungen in der SQL-Datenbank. Zu einem beliebigen Zeitpunkt kann nur ein Azure AD-Administrator (Benutzer oder Gruppe) konfiguriert werden.

![Admin-Struktur][3]

## <a name="permissions"></a>Berechtigungen

Um die neuen Benutzer erstellen zu können, müssen Sie die `ALTER ANY USER` Berechtigungen in der Datenbank. Die `ALTER ANY USER` jedem Datenbankbenutzer kann über die Berechtigung erteilt werden. Die `ALTER ANY USER` Berechtigung ist auch frei, die vom Server Administrator-Konten und Datenbankbenutzer mit der `CONTROL ON DATABASE` oder `ALTER ON DATABASE` Berechtigungen für die Datenbank, und durch ein Mitglied der `db_owner` Datenbankrolle.

Um einen eigenständigen Datenbankbenutzer in Azure SQL-Datenbank oder SQL Data Warehouse erstellen zu können, müssen Sie mit der Datenbank mithilfe einer Identität Azure AD-verbinden. Zum Erstellen des ersten eigenständigen Datenbankbenutzers müssen Sie Herstellen einer Verbindung mit der Datenbank mithilfe eines Azure Active Directory-Administrator (die der Besitzer der Datenbank ist). Dies wird in den Schritten 4 und 5 unten dargestellt. Alle Azure-Active Directory-Authentifizierung ist nur möglich, wenn der Azure-Active Directory-Administrator für Azure SQL-Datenbank oder die Data Warehouse SQL Server erstellt wurde. Wenn der Administrator Azure Active Directory vom Server entfernt wurde, können angelegte Azure Active Directory Benutzer zuvor innerhalb von SQL Server nicht mehr in die Datenbank mit ihren Azure Active Directory-Anmeldeinformationen verbinden.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-Features und Einschränkungen

Die folgenden Mitglieder der Azure Active Directory können in Azure SQL Server- oder SQL Data Warehouse bereitgestellt werden:

- Systemeigene Mitglieder: Mitglied in Azure AD in der verwalteten Domäne oder in einer Kundendomäne erstellt wurden. Weitere Informationen finden Sie unter [Hinzufügen von Ihren eigenen Domänennamen zu Azure AD-](../active-directory/active-directory-add-domain.md).

- Domänenmitglieder Partnersuche: Mitglied in Azure AD mit einer verbunddomäne erstellt. Weitere Informationen finden Sie unter [Microsoft Azure jetzt unterstützt Föderation mit Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Importierte Elemente aus anderen Azure-Active Directory, die Domänenmitglieder einer systemeigenen oder im Verbund sind.

- Active Directory-Gruppen, die als Sicherheitsgruppen erstellt werden.

Microsoft-Konten (z. B. outlook.com hotmail.com, live.com) oder andere Gastkonten (beispielsweise gmail.com, yahoo.com) werden nicht unterstützt. Wenn Sie zum [https://login.live.com](https://login.live.com) mit dem Konto und das Kennwort anmelden können, klicken Sie dann verwenden ein Microsoft-Konto, Sie die nicht für Azure AD-Authentifizierung für SQL Azure-Datenbank oder Azure SQL-Data Warehouse unterstützt wird.

### <a name="additional-considerations"></a>Weitere Aspekte

- Um Verwaltbarkeit zu verbessern, empfehlen wir, dass Sie eine dedizierte Azure Active Directory-Gruppe als Administrator bereitstellen.
- Nur eine Azure AD-Administrator (Benutzer oder Gruppe) kann zu einem beliebigen Zeitpunkt für die Azure SQL-Server oder Azure SQL-Data Warehouse konfiguriert werden.
- Nur ein Azure Active Directory-Administrator für SQL Server kann anfangs zur Azure SQL-Server oder Azure SQL-Data Warehouse mit einem Azure Active Directory-Konto verbinden. Active Directory-Administrator kann nachfolgende Azure Active Directory-Datenbankbenutzer konfigurieren.
- Es empfiehlt sich, Connection Timeout auf 30 Sekunden festlegen.
- SQL Server 2016 Management Studio und SQL Server Data Tools für Visual Studio 2015 (Version 14.0.60311.1April 2016 oder höher) unterstützen Azure Active Directory-Authentifizierung. (Azure-Active Directory-Authentifizierung wird unterstützt, vom **.NET Framework-Datenanbieter für SQL Server**; bei mindestens Version .NET Framework 4.6). Die neuesten Versionen dieser Tools und Datenebene Applikationen (DAC und .bacpac) können daher Azure-Active Directory-Authentifizierung verwenden.
- [ODBC-Version 13.1](https://www.microsoft.com/download/details.aspx?id=53339) unterstützt Azure-Active Directory-Authentifizierung, jedoch `bcp.exe` kann keine Verbindung herstellen mit Azure-Active Directory-Authentifizierung, weil einen älteren ODBC-Anbieter verwendet.
- `sqlcmd`unterstützt Azure Active Directory-Authentifizierung Anfang mit Version 13.1 aus dem [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)zur Verfügung.  
- SQL Server Data Tools für Visual Studio 2015 erfordert mindestens eine April 2016 Version von den Datentools (Version 14.0.60311.1). Azure-Active Directory-Benutzer sind derzeit nicht im SSDT Objekt-Explorer angezeigt. Anzeigen von Benutzer in [Kataloganzeige](https://msdn.microsoft.com/library/ms187328.aspx)Problem zu umgehen.
- [Microsoft JDBC Treiber 6.0 für SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) unterstützt Azure-Active Directory-Authentifizierung. Darüber hinaus finden Sie unter [Festlegen der Eigenschaften für die Verbindung](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase kann nicht mithilfe von Azure-Active Directory-Authentifizierung authentifizieren.
- Einige Tools, wie BI und Excel werden nicht unterstützt.
- SQL-Datenbank wird durch die Azure-Portal **Importieren** und **Exportieren Datenbank** Blades Azure Active Directory-Authentifizierung unterstützt. Importieren und Exportieren mit Azure Active Directory-Authentifizierung auch aus den PowerShell-Befehl unterstützt wird.


## <a name="1-create-and-populate-an-azure-ad"></a>1. erstellen Sie und füllen Sie einer Azure AD

Erstellen einer Azure-Active Directory, und füllen Sie es mit Benutzer und Gruppen. Azure Active Directory kann die verwaltete ursprüngliche Domäne Azure AD-Domäne sein. Azure Active Directory kann auch einer lokalen Active Directory-Domänendiensten, die Föderation besteht mit Azure Active Directory sein.

Weitere Informationen finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Fügen Sie Ihren eigenen Domänennamen zu Azure AD-](../active-directory/active-directory-add-domain.md), [Microsoft Azure jetzt unterstützt mit Windows Server Active Directory Federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten von Ihrem Verzeichnis Azure AD-](https://msdn.microsoft.com/library/azure/hh967611.aspx)und [Verwalten Azure Active Directory mithilfe von Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2 Stellen Sie sicher, dass die SQL-Datenbank Version 12 ist

In der neuesten SQL-Datenbank V12 wird Azure Active Directory-Authentifizierung unterstützt. Informationen zu SQL-Datenbank V12 und, wenn Sie wissen, ob in Ihrer Region verfügbar ist finden Sie unter [Was ist neu in der neuesten SQL-Datenbank Update V12](sql-database-v12-whats-new.md). Dieser Schritt ist nicht erforderlich für Azure SQL-Data Warehouse, da SQL Data Warehouse nur V12 zur Verfügung steht.

Wenn Sie eine vorhandene Datenbank haben, stellen Sie sicher, dass es in SQL-Datenbank V12 gehostet wird, durch Herstellen einer Verbindung mit der Datenbank (beispielsweise mit SQL Server Management Studio) und Ausführen von `SELECT @@VERSION;`. Die Ausgabe die erwartete für eine Datenbank in der SQL-Datenbank V12 ist mindestens **Microsoft SQL Azure (RTM) – 12.0**. Wenn die Datenbank nicht in der SQL-Datenbank V12 gehostet wird, finden Sie unter [Planen und Vorbereiten der ein Upgrade auf SQL-Datenbank V12](sql-database-v12-plan-prepare-upgrade.md), und gehen Sie auf das Portal Azure klassischen, um die Datenbank auf SQL-Datenbank V12 migrieren.

Alternativ können Sie in der SQL-Datenbank V12 eine neue Datenbank erstellen, gemäß die Anweisungen in [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md)aufgeführt. **Tipp**: im nächsten Schritt lesen, bevor Sie ein Abonnement für die neue Datenbank auswählen.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. Optional: Zuordnen oder Ändern von active Directory, das derzeit mit Ihrem Azure-Abonnement verknüpft ist

Um die Datenbank Azure AD-Verzeichnis für Ihre Organisation zuzuordnen, stellen Sie Verzeichnis vertrauenswürdigen Verzeichnis für das Azure Abonnement sich die Datenbank befindet. Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure AD zugeordnet sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Weitere Informationen:** Jede Azure-Abonnement verfügt über eine Vertrauensstellung mit einer Azure AD-Instanz. Dies bedeutet, dass sie dieses Verzeichnis zum Authentifizieren von Benutzern, Dienste und Geräte vertraut. Mehrere Abonnements können das gleiche Verzeichnis vertrauen, aber ein Abonnement vertraut nur ein Verzeichnis. Sie können sehen, welches Verzeichnis Ihr Abonnement unter der Registerkarte **Einstellungen** bei [https://manage.windowsazure.com/](https://manage.windowsazure.com/)vertrauenswürdig ist. Diese Vertrauensstellung, die ein Abonnement für ein Verzeichnis weist unterscheidet sich von der Beziehung, die ein Abonnement für alle anderen Ressourcen in Azure (Websites, Datenbanken usw.), enthält die untergeordneten Ressourcen eines Abonnements eher sind. Wenn ein Abonnement abläuft, reagiert Zugriff auf diese andere Ressourcen, die mit dem Abonnement verknüpft ist auch. Aber Verzeichnis in Azure bleibt, und Sie können ein weiteres Abonnement dieses Verzeichnis zuordnen und fahren Sie mit der Directory Benutzer verwalten. Weitere Informationen zu Ressourcen finden Sie unter [Grundlegendes zu Ressourcenzugriffs in Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Die folgenden Verfahren liefern Schritt-für-Schritt-Anleitung zum zugeordnete Verzeichnis für ein angegebenes Abonnement ändern.

1. Verbinden Sie Ihrer [Klassischen Azure-Portal](https://manage.windowsazure.com/) mithilfe der Abonnementadministrator Azure.
2. Wählen Sie auf der linken Banner **EINSTELLUNGEN**aus.
3. Klicken Sie im Einstellungsbildschirm werden Ihre Abonnements angezeigt. Wenn das gewünschte Abonnement nicht angezeigt wird, klicken Sie oben auf **Abonnements** , Dropdown-Liste im Feld **Filtern nach Verzeichnis** wählen Sie aus dem Verzeichnis, das Ihrer Abonnements enthält, und klicken Sie dann auf **Übernehmen**.

    ![Wählen Sie Abonnements aus.][4]
4. Klicken Sie im Bereich **Einstellungen** klicken Sie auf Ihr Abonnement, und klicken Sie dann auf **Bearbeiten DIRECTORY** am unteren Rand der Seite.

    ![AD-Einstellungen-portal][5]
5. Wählen Sie im **Verzeichnis bearbeiten** aus der Azure-Active Directory, die Ihre SQL Server- oder SQL Data Warehouse zugeordnet ist, und klicken Sie dann auf den Pfeil für weiter.

    ![Bearbeiten-Directory-auswählen][6]
6. Im Dialogfeld **bestätigen** Directory Zuordnung zu bestätigen, dass "**Alle Co-Administratoren entfernt werden.**"

    ![Verzeichnis bestätigen bearbeiten][7]
7. Klicken Sie auf das Kontrollkästchen, um das Portal neu zu laden.

> [AZURE.NOTE] Wenn Sie Zugriff auf alle CO-Administratoren, Azure Active Directory-Benutzer und Gruppen, Verzeichnis, ändern und Directory gesicherten Ressourcen Benutzer entfernt werden und sie keinen Zugriff mehr haben Abonnement oder seine Ressourcen. Nur können Sie, wie ein Dienstadministrator Access nach Hauptbenutzer basierend auf dem neuen Verzeichnis konfigurieren. Diese Änderung möglicherweise eine umfangreiche auf alle Ressourcen übertragen lange dauern. Ändern, Verzeichnis ändert sich den Azure AD-Administrator für SQL-Datenbank und SQL Data Warehouse und verbieten Datenbankzugriff für alle vorhandenen Azure AD-Benutzer. Der Administrator Azure AD-muss zurücksetzen (wie unten beschrieben) und neue Azure Active Directory-Benutzer erstellt werden müssen.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4 Erstellen Sie 4 ein Administrator Azure AD-für Azure SQL Server

Jede Azure SQL-Server (die eine SQL-Datenbank oder SQL Data Warehouse hostet) beginnt mit einer einzelnen Server-Administratorkonto, die der Administrator des gesamten Azure SQL-Servers ist. Zweiter SQL Server-Administrator muss erstellt werden, die ein Azure AD-Konto handelt. Diese Tilgungsanteile wird als einen eigenständigen Datenbankbenutzer in der master-Datenbank erstellt. Als Administratoren die Server Administrator-Konten sind Mitglied der Rolle **Db_owner** in jeder Benutzer der Datenbank, und geben Sie jeder Benutzerdatenbank als **das Benutzeranmeldekonto** ein. Weitere Informationen zu den Server-Administrator-Konten finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md) und im Abschnitt **Benutzernamen und Benutzern** [Azure SQL-Datenbank Sicherheitsrichtlinien und Einschränkungen](sql-database-security-guidelines.md).

Wenn Azure Active Directory mit Geo-Replikation verwenden zu können, muss der Azure-Active Directory-Administrator für sowohl der primären und sekundären Server konfiguriert sein. Wenn ein Server keinen Azure Active Directory-Administrator, erhalten Azure Active Directory-Benutzernamen und Benutzern ein "kann keine Verbindung Serverfehler hergestellt".

> [AZURE.NOTE] Benutzer, die auf ein Azure AD-Konto (einschließlich des Azure SQL Server-Administratorkontos) nicht basieren, Azure AD-basierten Benutzer kann nicht erstellt werden, da sie nicht über die Berechtigung zum Überprüfen Sie die vorgeschlagenen Datenbankbenutzer mit Azure AD-verfügen.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Bereitstellung von Azure Active Directory-Administrator für Ihren Azure SQL Server mithilfe des Azure-Portals

1. Klicken Sie im [Portal Azure](https://portal.azure.com/)in der oberen rechten Ecke auf eine Verbindung mit Dropdown-eine Liste von möglichen Active Directory. Wählen Sie die richtige Active Directory als Standard Azure AD aus. Dieser Schritt links die Zuordnung Abonnement mit Active Directory mit Azure SQL Server, und stellen Sie sicher, dass das gleiche Abonnement für beide verwendet wird Azure AD- und SQL Server. (Der SQL Azure-Server können entweder Azure SQL-Datenbank oder Azure SQL-Data Warehouse gehostet werden.)

    ![Wählen Sie ad][8]
2. Im linken Banner wählen Sie **SQL Server aus**, wählen Sie den **SQLServer**, und klicken Sie dann in der **SQL Server** -Blade, oben auf **Einstellungen**.

    ![AD-Einstellungen][9]
3. Klicken Sie in das Blade **Einstellungen** auf ** Active Directory-Administrator.
4. Klicken Sie in den **Active Directory-Administrator** Blade auf **Active Directory-Administrator**, und klicken Sie dann oben auf **Administrator festlegen**.
5. Wählen Sie in der Blade- **Administrator hinzufügen** , suchen Sie nach einem Benutzer aus dem Benutzer oder der Gruppe Administrator sein soll, und klicken Sie dann auf **auswählen**. (Das Active Directory-Administrator Blade zeigt alle Elemente und Gruppen von Active Directory. Benutzer oder Gruppen, die abgeblendet werden können nicht ausgewählt werden, da sie als Azure AD-Administratoren nicht unterstützt werden. (Siehe die Liste der unterstützten Administratoren in **Azure AD-Features und Einschränkungen** oben). Rollenbasierte Access-Steuerelement (RBAC) gilt nur für das Portal und ist nicht mit SQL Server verteilt.
6. Am oberen Rand der Blade **Active Directory-Administrator** klicken Sie auf **Speichern**.
    ![Wählen Sie Administrator][10]

    Die Vorgehensweise zum Ändern des Administrators kann einige Minuten dauern. Klicken Sie dann der neue Administrator im **Active Directory-Administrator** angezeigt.

> [AZURE.NOTE] Beim Einrichten des Azure AD-Administrators kann nicht der neuen Administratornamen (Benutzer oder Gruppe) als SQL Server-Authentifizierungsbenutzer bereits in der virtuelle master Datenbank vorhanden sein. Falls vorhanden, tritt die Azure AD-Administrator Einrichtung; Zurücksetzen seiner Erstellung klicken und angeben, die solche Administrator (Name) bereits vorhanden ist. Da solche SQL Server-Authentifizierung-Benutzer nicht Azure AD gehört, schlägt fehl alle Leistungsgesteuert für die Verbindung mit dem Server Azure AD-Authentifizierung über ein.

Um später-Administrator am oberen Rand der **Active Directory-Administrator** Blade entfernen klicken Sie auf **Admin entfernen**, und klicken Sie dann auf **Speichern**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Bereitstellen eines Azure AD-Administrators für Azure SQL Server mithilfe der PowerShell

Um die PowerShell-Cmdlets ausführen zu können, müssen Sie Azure PowerShell installiert sein und ausgeführt haben. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

Um ein Administrator Azure AD-bereitzustellen, führen Sie folgende Azure PowerShell Befehle aus:

- Hinzufügen von AzureRmAccount
- Wählen Sie-AzureRmSubscription


Cmdlets, die zum Bereitstellen und Verwalten von Azure AD-Administrator verwendet werden:

| Cmdlet-name                                       | Beschreibung                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Stellt ein Azure Active Directory-Administrator für Azure SQL Server- oder Azure SQL-Data Warehouse bereit. (Aus dem aktuellen Abonnement müssen.) |
| [Entfernen-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Entfernt eine Azure-Active Directory-Administrator für Azure SQL Server- oder Azure SQL-Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Gibt Informationen zur Administrator Azure Active Directory aktuell für die Azure SQL-Server oder Azure SQL-Data Warehouse konfiguriert. |

Verwenden Sie PowerShell-Befehl-Hilfe, um weitere Details für jeden der folgenden Befehle, beispielsweise finden Sie unter ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Die folgenden Skript Vorschriften eine Azure AD-Administrator-Gruppe mit dem Namen **DBA_Group** (Objekt-Id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) für die **Demo_server** Server in einer Ressourcengruppe mit dem Namen **Gruppe-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Der **DisplayName** Eingabeparameter akzeptiert den Anzeigenamen Azure AD- oder Benutzerprinzipalnamen. Beispielsweise ``DisplayName="John Smith"`` und ``DisplayName="johns@contoso.com"``. Azure AD-Gruppen nur die Azure AD-Anzeigenamen unterstützt.

> [AZURE.NOTE] Der Azure-PowerShell-Befehl ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` verhindert nicht, dass Sie Azure AD-Administratoren für nicht unterstützte Benutzer bereitgestellt. Ein nicht unterstützter Benutzer bereitgestellt werden kann, aber nicht auf eine Datenbank zugreifen kann. (Siehe die Liste der unterstützten Administratoren in **Azure AD-Features und Einschränkungen** oben).

Im folgenden Beispiel wird die optional **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Azure AD **ObjectID** ist erforderlich, wenn die **DisplayName** nicht eindeutig ist. Um die **ObjectID** und **DisplayName** abzurufen, verwenden Sie den Active Directory-Abschnitt der klassischen Azure-Portal, und zeigen Sie die Eigenschaften eines Benutzers oder einer Gruppe.

Im folgende Beispiel gibt Informationen zum aktuellen Azure AD-Administrator für Azure SQL-Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Im folgende Beispiel wird einen Administrator Azure AD-entfernt:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Sie können auch ein Azure-Active Directory-Administrator mithilfe der REST-APIs bereitstellen. Weitere Informationen finden Sie unter [Service Management REST-API-Referenz und Vorgänge für Azure SQL-Datenbanken Vorgänge für Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. Konfigurieren der Clientcomputer

Auf allen Clientcomputern, aus denen Anwendung und Benutzer Azure SQL-Datenbank oder mit Azure AD-Identitäten Data Warehouse mit SQL Azure Verbindung müssen Sie die folgende Software installieren:

- .NET Framework 4.6 oder höher aus [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure-Active Directory-Authentifizierungsbibliothek für SQLServer (**ADALSQL. DLL**) steht in mehreren Sprachen (X86 und amd64) aus dem Downloadcenter bei [Microsoft Active Directory Authentifizierungsbibliothek für Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Tools

- Installation von [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) oder [SQL Server Data Tools für Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) entweder entspricht der Anforderung zum .NET Framework 4.6 für.
- SSMS Installationen der X86 Version **ADALSQL. DLL**.
- SSDT Installationen die amd64-Version von **ADALSQL. DLL**.
- Die neuesten Visual Studio aus [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) erfüllt die Anforderung .NET Framework 4.6, aber die erforderlichen amd64-Version von **ADALSQL wird nicht installiert. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. erstellen Sie enthaltenen Datenbankbenutzer in Ihrer Datenbank zugeordneten Azure AD-Identitäten

### <a name="about-contained-database-users"></a>Informationen zu den enthaltenen Datenbankbenutzer

Azure Active Directory-Authentifizierung erforderlich ist, Datenbankbenutzer als Datenbankbenutzer enthaltenen erstellt werden. Ein eigenständigen Datenbankbenutzer basierend auf einer Identität Azure AD-ist ein Datenbankbenutzer, die nicht über einen Benutzernamen in der master-Datenbank verfügt und das entspricht, eine Identität im Verzeichnis Azure AD-, die mit der Datenbank verknüpft ist. Die Identität Azure AD-kann entweder ein einzelnes Benutzerkonto oder eine Gruppe sein. Weitere Informationen zu den enthaltenen Datenbankbenutzer finden Sie unter [Ihre Datenbankbenutzer enthaltenen Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht mithilfe von Portal erstellt werden. RBAC-Rollen werden nicht in SQL Server, SQL-Datenbank oder SQL Data Warehouse übernommen. Azure RBAC-Rollen für die Verwaltung von Azure-Ressourcen verwendet werden, und nicht für die Datenbankberechtigungen gelten. Beispielsweise gewährt der Teilnehmerrolle für **SQL Server** keinen Zugriff für die Verbindung zu der SQL-Datenbank oder SQL Data Warehouse. Direkt in die Datenbank mithilfe von Transact-SQL-Anweisungen muss die Berechtigung erteilt werden.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Verbinden Sie mit der Benutzer-Datenbank oder eines Data Warehouse mithilfe von SQL Server Management Studio oder SQL Server Data Tools

Zur Bestätigung des Azure AD-Administrators ist ordnungsgemäß eingerichtet, eine Verbindung zu der **master** -Datenbank mit dem Azure AD-Administratorkonto an.
Zum Bereitstellen ein Azure AD-basierten enthaltenen Datenbankbenutzer (mit Ausnahme der Serveradministrator, der die Datenbank besitzt), Herstellen einer Verbindung mit der Datenbank mit einer Azure AD-Identität, die auf die Datenbank zugreifen können.

> [AZURE.IMPORTANT] Unterstützung für Azure-Active Directory-Authentifizierung ist im Lieferumfang von [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015. Die Version August 2016 SSMS umfasst auch Unterstützung für Active Directory Universal-Authentifizierung, wodurch Administratoren kombinierte Authentifizierung mit einem Anruf, Textnachricht, Smart-Karten mit Pin oder mobile-app Benachrichtigung erforderlich.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Eine Verbindung mit Active Directory-integrierte Authentifizierung

Verwenden Sie diese Methode, wenn Sie in Windows unter Verwendung von einer verbunddomäne Ihrer Azure Active Directory-Anmeldeinformationen angemeldet sind.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit Server herstellen** (oder **Verbinden mit Datenbank-Engine**) im Feld **Authentifizierung** aus **Active Directory-integrierte Authentifizierung**. Kein Kennwort erforderlich ist oder eingegeben werden kann, da Ihre vorhandenen Anmeldeinformationen für die Verbindung dargestellt werden.
    ![Wählen Sie integrierte AD-Authentifizierung][11]

2. Klicken Sie auf die Schaltfläche **Optionen** , und geben Sie auf der Seite **Verbindungseigenschaften** im **Herstellen einer Verbindung Datenbank mit** den Namen der Datenbank, die, der Sie in eine Verbindung herstellen möchten.
    ![Wählen Sie den Datenbanknamen][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Eine Verbindung mit Active Directory-Kennwortauthentifizierung

Verwenden Sie diese Methode beim Herstellen einer Verbindung mit einer Azure AD-Benutzerprinzipalnamen Azure AD-Domäne verwaltet. Sie können auch für das verbundene Konto ohne Zugriff auf die Domäne, beispielsweise wenn Sie Remote arbeiten verwenden.

Verwenden Sie diese Methode, wenn Sie Windows eine Domäne, die mit Azure kein Verbund wird unter Verwendung von Anmeldeinformationen angemeldet sind, oder wenn mit Azure AD-Authentifizierung Azure AD-auf die erste oder die Clientdomäne basiert.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit Server herstellen** (oder **Verbinden mit Datenbank-Engine**) im Feld **Authentifizierung** aus **Active Directory-Kennwortauthentifizierung**.
2. Geben Sie im Feld **Benutzername** Ihren Benutzernamen Azure Active Directory im Format **username@domain.com**. Dies muss ein Konto aus Azure Active Directory oder ein Konto in einer Domäne mit Azure Active Directory zusammenarbeitet.
3. Geben Sie in das Feld **Kennwort** Ihr Benutzerkennwort für das Azure Active Directory-Konto oder verbundene Domänenkonto.
    ![Wählen Sie AD-Kennwortauthentifizierung][12]

4. Klicken Sie auf die Schaltfläche **Optionen** , und geben Sie auf der Seite **Verbindungseigenschaften** im **Herstellen einer Verbindung Datenbank mit** den Namen der Datenbank, die, der Sie in eine Verbindung herstellen möchten. (Siehe die Grafik in der vorherigen Option).


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Erstellen eines Datenbankbenutzers Azure AD-enthaltenen in einer Datenbank

Zum Erstellen eines Azure AD-basierten enthaltenen Datenbankbenutzers (mit Ausnahme der Serveradministrator, der die Datenbank besitzt) Herstellen einer Verbindung mit der Datenbank für eine Identität Azure AD-als Benutzer mit mindestens die Berechtigung **ALTER ANY USER** . Verwenden Sie dann die folgende Transact-SQL-Syntax ein:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* kann die Benutzerprinzipalnamen eines Benutzers Azure AD- oder des Anzeigenamens für eine Gruppe Azure AD-sein.

**Beispiele:** Zum Erstellen einer Datenbank enthaltenen Benutzer für eine Azure Anzeige Partnersuche oder Domänenbenutzer verwaltet:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Zum Erstellen enthaltenen Datenbankbenutzer, die eine Azure AD darstellt oder verbundene Domänengruppe Geben Sie den Anzeigenamen einer Sicherheitsgruppe aus:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

So erstellen Sie einen eigenständigen Datenbankbenutzer, eine Anwendung, die mit einer Azure AD-Token verbindet darstellt:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Weitere Informationen zum Erstellen von enthaltenen Datenbankbenutzer basierend auf Identitäten Azure Active Directory finden Sie unter [CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Entfernen der Azure-Active Directory-Administrator für Azure SQL Server verhindert, dass Benutzer alle Azure AD-Authentifizierung Herstellen einer Verbindung mit dem Server. Falls erforderlich, unbrauchbar Azure AD-Benutzern manuell von einem Administrator SQL-Datenbank abgelegt werden können.

Wenn Sie einen Datenbankbenutzer erstellen, wird dieser Benutzer erhält die Berechtigung **Verbinden** und kann die Verbindung zu dieser Datenbank als ein Mitglied der Rolle **PUBLIC** . Berechtigungen für die Rolle **PUBLIC** oder Berechtigungen erteilt die Windows-Gruppen, denen deren Mitglied Sie sind, wird anfangs die nur Berechtigungen für den Benutzer verfügbar sind. Nachdem Sie einen Azure AD-basierten enthaltenen Datenbankbenutzer bereitstellen, können Sie der Benutzer zusätzliche Berechtigungen genauso erteilen, wie Sie einen anderen Typ des Benutzers erteilt werden. In der Regel Erteilen von Berechtigungen zur Datenbankrollen und Benutzer, die Rollen hinzuzufügen. Weitere Informationen finden Sie unter [Datenbank-Engine Berechtigung Grundlagen](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Weitere Informationen zu Rollen für spezielle SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md).
Ein verbunddomäne Benutzer, der in einer Domäne verwalten importiert werden müssen die Identität verwalteten Domäne verwenden.

> [AZURE.NOTE] Azure AD-Benutzer sind in der Datenbank-Metadaten mit Typ E (EXTERNAL_USER) und für Gruppen mit Typ X (EXTERNAL_GROUPS) markiert. Weitere Informationen finden Sie unter [Kataloganzeige](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7 verbinden Sie mit Azure AD-Identitäten

Azure Active Directory-Authentifizierung unterstützt die folgenden Methoden zum Herstellen einer Verbindung mit einer Datenbank mit Azure AD-Identitäten:

- Mithilfe der integrierten Windows-Authentifizierung
- Mithilfe einer Azure AD-Benutzerprinzipalnamen und eines Kennworts
- Mithilfe der Anwendung token-Authentifizierung

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Herstellen einer Verbindung mithilfe der integrierten (Windows) Authentifizierung

Um die integrierte Windows-Authentifizierung verwenden zu können, muss Active Directory Ihrer Domäne mit Azure Active Directory verbunden sein. Auf einem Computer Domänenverbund unter Anmeldeinformationen für die Domäne eines Benutzers muss der Clientanwendung (oder einen Dienst) Herstellen einer Verbindung mit der Datenbank ausgeführt werden.

Informationen zum Verbinden mit einer Datenbank mithilfe der integrierten Authentifizierung und eine Azure AD-Identität muss das Schlüsselwort Authentifizierung in der Datenbank-Verbindungszeichenfolge Active Directory-integrierte festgelegt werden. Im folgende C#-Codebeispiel wird ADO .NET verwendet.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Beachten Sie, dass die Verbindungszeichenfolge Schlüsselwort ``Integrated Security=True`` wird zum Herstellen einer Verbindung mit Azure SQL-Datenbank nicht unterstützt.
Beachten Sie, dass beim Herstellen einer Verbindungs ODBC benötigen werden Leerzeichen entfernen und Authentifizierung zu 'ActiveDirectoryIntegrated' festlegen.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Herstellen einer Verbindung mit einer Azure AD-Benutzerprinzipalnamen und ein Kennwort
Informationen zum Verbinden mit einer Datenbank mithilfe der integrierten Authentifizierung und eine Azure AD-Identität muss das Schlüsselwort Authentifizierung Active Directory-Kennwort festgelegt werden. Die Verbindungszeichenfolge muss die Benutzer-ID-Benutzer-ID und Kennwort/PWD Schlüsselwörter und Werte enthalten. Im folgende C#-Codebeispiel verwendet ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Weitere Informationen zu Azure AD-Authentifizierungsmethoden mithilfe der Demo Codebeispielen verfügbar bei [Azure AD-Authentifizierung GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 Herstellen einer Verbindung mit einem Azure AD-token
Diese Authentifizierungsmethode ermöglicht mittleren Ebene Services eine Verbindung zur Azure SQL-Datenbank oder Azure SQL-Data Warehouse mit erhalten ein Token aus Azure Active Directory (AAD). Ausgefeiltes einschließlich Zertifikaten basierende Authentifizierung Szenarios sind möglich. Führen Sie die vier grundlegende Schritte aus, um die token Azure AD-Authentifizierung verwenden:

1. Registrieren Sie Ihrer Anwendung mit Azure Active Directory und rufen Sie die Client-Id für den Code. 
2. Erstellen Sie einen Datenbankbenutzer, die die Anwendung darstellt. (Geführte in Schritt 6).
3. Erstellen Sie ein Zertifikat auf den Client-Computer ausgeführt wird, die Anwendung.
4. Fügen Sie das Zertifikat als einen Schlüssel für eine Anwendung hinzu.

Beispiel für Verbindungszeichenfolge:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Weitere Informationen finden Sie unter [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Herstellen einer Verbindung mit sqlcmd  
Die folgenden Aussagen Verbinden mit Version 13.1 Sqlcmd, das aus dem [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)verfügbar ist.

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Siehe auch

[Verwalten von Datenbanken und Anmeldedaten in SQL Azure-Datenbank](sql-database-manage-logins.md)

[Datenbankbenutzer enthaltenen](https://msdn.microsoft.com/library/ff929071.aspx)

[Erstellen Sie Benutzer (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png


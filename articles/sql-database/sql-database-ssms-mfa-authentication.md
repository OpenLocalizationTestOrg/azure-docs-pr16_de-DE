<properties
   pageTitle="SSMS für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse unterstützt | Microsoft Azure"
   description="Verwenden Sie mehrfach aufgeteilt Authentifizierung mit SSMS für SQL-Datenbank und SQL Datawarehouse."
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
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS unterstützt für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse

Azure SQL-Datenbank und Azure SQL-Data Warehouse unterstützt jetzt Verbindungen aus SQL Server Management Studio (SSMS) *Universeller Active Directory-Authentifizierung*verwenden. Universeller Active Directory-Authentifizierung ist eine interaktive Workflow, der *Azure kombinierte Authentifizierung* (MFA) unterstützt. Azure MFA hilft Schutz Zugriff auf Daten und Applikationen während der Besprechung Benutzer bei Bedarf für ein einfacher Vorgang Anmeldung. Es stellt strenge Authentifizierung für eine Reihe von Überprüfungsoptionen für einfache – Anruf, Textnachricht, Smart-Karten mit Pin oder mobilen app Benachrichtigung – zulassen, dass Benutzer die Methode wählen sie bevorzugen. Eine Beschreibung der kombinierte Authentifizierung finden Sie unter [Kombinierte Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md).

SSMS unterstützt jetzt aus:

- Interaktive MFA mit Azure AD die möglicherweise für die Popupmenü Dialogfeld Feld Validierung.
- Nicht-interaktive Active Directory-Kennwort und Active Directory-integrierte Authentifizierung Methoden, die in vielen verschiedenen Programmen (ADO.NET, JDBC, ODBC usw.) verwendet werden können. Diese beiden Methoden zu Fehlern nie im Popup-Dialogfelder.

Wenn das Benutzerkonto für MFA konfiguriert ist erfordert eine Authentifizierung interaktiven Workflow zusätzliche Benutzeraktion bis Popup-Dialogfelder, Smartcard verwenden usw.. Wenn das Benutzerkonto für MFA konfiguriert ist, muss der Benutzer Azure universeller Authentifizierung Verbindung auswählen. Wenn das Benutzerkonto keine MFA erforderlich sind, kann der Benutzer weiterhin die beiden Azure-Active Directory-Authentifizierung Optionen verwenden.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universeller Authentifizierung Einschränkungen für SQL-Datenbank und SQL Data Warehouse

- SSMS ist der einzige Tool derzeit für MFA bis universeller Active Directory-Authentifizierung aktiviert.
- Nur ein einzelnes Azure Active Directory-Konto kann für eine Instanz von SSMS mithilfe der Universal Authentifizierung anmelden. Um die als ein anderes Azure AD-Konto angemeldet haben, müssen Sie eine andere Instanz von SSMS verwenden. (Diese Einschränkung ist auf Universal Active Directory-Authentifizierung beschränkt; Sie können melden Sie sich bei anderen Servern mit Active Directory-Kennwortauthentifizierung, Active Directory-integrierte Authentifizierung oder SQL Server-Authentifizierung).
- SSMS unterstützt universeller Active Directory-Authentifizierung für Objekt-Explorer, Abfrage-Editor und Abfrage Store Visualisierung.
- Weder DacFx noch der Schema-Designer unterstützt universeller Authentifizierung.
- MSA Konten werden universeller Active Directory-Authentifizierung nicht unterstützt.
- Universeller Active Directory-Authentifizierung wird nicht in SSMS für Benutzer unterstützt, die in der aktuellen Active Directory aus anderen Azure Active Directory importiert wurden. Diese Benutzer werden nicht unterstützt, da es eine Mandanten-ID müssten, um die Konten zu überprüfen und keine Methode zum Bereitstellen, die vorhanden ist.
- Es gibt keine weiteren softwareanforderungen für Universal Active Directory-Authentifizierung, mit dem Unterschied, dass Sie eine unterstützte Version von SSMS verwenden müssen.

## <a name="configuration-steps"></a>Konfigurationsschritte

Mehrstufige Authentifizierung implementieren erfordert vier grundlegende Schritte.

1. **Konfigurieren einer Azure Active Directory** – Weitere Informationen finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Fügen Sie Ihren eigenen Domänennamen zu Azure AD-](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure jetzt unterstützt mit Windows Server Active Directory Federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten von Ihrem Verzeichnis Azure AD-](https://msdn.microsoft.com/library/azure/hh967611.aspx)und [Verwalten Azure Active Directory mithilfe von Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Konfigurieren von MFA** – schrittweise Anweisungen finden Sie unter [Konfigurieren von Azure kombinierte Authentifizierung](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Konfigurieren der SQL-Datenbank oder SQL Data Warehouse für Azure AD-Authentifizierung** – schrittweise Anweisungen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse durch Verwenden von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).

4. **Herunterladen von SSMS** – auf dem Clientcomputer, laden Sie die neuesten SSMS (mindestens August 2016), aus dem [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Herstellen einer Verbindung mithilfe von Universal Authentifizierung mit SSMS

Die folgenden Schritte zeigen, wie mithilfe der neuesten SSMS mit SQL-Datenbank oder SQL Data Warehouse verbinden.

1. Wählen Sie zum Verbinden mit Universal Authentifizierung im Dialogfeld **Verbinden mit Server** **Universeller Active Directory-Authentifizierung**ein.
![1mfa Universal verbinden][1]

2. Wie gewohnt für SQL-Datenbank und SQL Data Warehouse müssen Sie klicken Sie auf **Optionen** , und geben Sie die Datenbank im Dialogfeld **Optionen** . Klicken Sie auf **Verbinden**.
3. Wenn das Dialogfeld **Melden Sie sich bei Ihrem Konto** angezeigt wird, stellen Sie das Konto und das Kennwort für Ihre Identität Azure Active Directory aus.
![2mfa-Anmeldung][2]

    > [AZURE.NOTE] Für die Authentifizierung ein Konto die keinen MFA benötigt Universal, verbinden Sie an diesem Punkt an. Fahren Sie für Benutzer mit Anforderung der MFA mit den folgenden Schritten.
 
4. Zwei MFA Setup Dialogfeldern wird möglicherweise angezeigt. Diesmal einen Vorgang hängt von der MFA Administrator festlegen und deshalb optional sein kann. Für eine Domäne MFA aktiviert dieser Schritt ist manchmal vordefinierte (beispielsweise die Domäne der Benutzer mit einer Smartcard und eine Pin erfordert).  
![3mfa-setup][3]

5. Die zweite möglich einmaliges Klicken Sie im Dialogfeld ermöglicht Ihnen, um die Details der Authentifizierungsmethode auszuwählen. Die möglichen Optionen werden vom Administrator konfiguriert.
![4mfa überprüfen 1][4]
 
6. Azure Active Directory sendet die Bestätigung Informationen an Sie. Wenn Sie den Überprüfungscode erhalten haben, geben Sie ihn in das Feld **EINGABETASTE Überprüfungscode** ein, und klicken Sie auf **Anmelden**.
![5mfa überprüfen 2][5]

Wenn die Überprüfung abgeschlossen ist, verbindet SSMS normal vorausgesetzt gültiger Anmeldeinformationen und Firewallzugriff.

##<a name="next-steps"></a>Nächste Schritte  

Erteilen Sie anderen Benutzern Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und Autorisierung: Erteilen von Access](sql-database-manage-logins.md)  
Sicherstellen, dass andere Personen können über die Firewall herstellen: [Konfigurieren einer Azure SQL-Datenbank-Server Ebene-Firewall-Regel mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png


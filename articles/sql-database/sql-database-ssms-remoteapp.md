<properties
    pageTitle="Herstellen einer Verbindung mit SQL Server Management Studio in RemoteApp Azure SQL-Datenbank mit | Microsoft Azure"
    description="Verwenden Sie in diesem Lernprogramm erfahren Sie, wie SQL Server Management Studio Azure RemoteApp für Sicherheit und Leistung zu verwenden, bei der Verbindung mit einer SQL-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Verwenden von SQL Server Management Studio in Azure RemoteApp Verbindung mit SQL-Datenbank

## <a name="introduction"></a>Einführung  
In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Management Studio (SSMS) in Azure RemoteApp zu verwenden, um zu SQL-Datenbank herstellen. Es führt Sie durch das Verfahren zum Einrichten von SQL Server Management Studio in Azure RemoteApp, erläutert die Vorteile und zeigt Azure Active Directory-Sicherheitsfeatures, die Sie verwenden können.

**Geschätzte Dauer:** 45 Minuten

## <a name="ssms-in-azure-remoteapp"></a>SSMS in Azure RemoteApp

Azure RemoteApp ist ein Dienst RDS Azure, die Applikationen übermittelt wird. Sie können hier erfahren Sie mehr darüber: [Neuigkeiten RemoteApp?](../remoteapp/remoteapp-whatis.md)

Ausführung in Azure RemoteApp SSMS bietet Ihnen die gleiche Oberfläche als SSMS lokal ausgeführt.

![Screenshot mit SSMS in Azure RemoteApp ausgeführt][1]



## <a name="benefits"></a>Vorteile

Es gibt viele Vorteile bei der Verwendung von SSMS in Azure RemoteApp, einschließlich:

- Port 1433 auf Azure SQL-Server hat keinen extern (außerhalb von Azure) verfügbar gemacht werden soll.
- Keine müssen hinzufügen und Entfernen von IP-Adressen in der Firewall Azure SQL Server beibehalten.
- Alle Azure RemoteApp Verbindungen auftreten, verschlüsselte Remote Desktop Protocol mit Port 443 über HTTPS
- Es wird mit mehreren Benutzern und skalieren kann.
- Es gibt eine Leistung erzielt davor SSMS in derselben Region als der SQL-Datenbank.
- Sie können mit Azure RemoteApp Premium-Version von Azure Active Directory überwachen die Benutzer Aktivitätsberichte hat.
- Sie können die kombinierte Authentifizierung (MFA) aktivieren.
- Access SSMS an einer beliebigen Stelle, wenn Sie einen der unterstützten Azure RemoteApp-Clients wozu iOS, Android, Mac, Windows Phone und Windows-PC verwenden.


## <a name="create-the-azure-remoteapp-collection"></a>Erstellen der Sammlung Azure RemoteApp

Hier werden die Schritte zum Erstellen Ihrer Azure RemoteApp-Sammlung mit SSMS aus:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Erstellen eines neuen Windows virtuellen Computers Bilds
Verwenden Sie das "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Bild aus dem Katalog, um Ihre neue virtueller Computer zu machen.


### <a name="2-install-ssms-from-sql-express"></a>2. SSMS von SQL Express installieren

Wechseln Sie auf dem neuen virtuellen Computer, und navigieren Sie zu dieser Downloadseite: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Es gibt eine Option, um nur SSMS herunterladen. Klicken Sie nach dem Herunterladen wechseln Sie in das Installationsverzeichnis, und führen Sie Setup um SSMS zu installieren.

Sie müssen SQL Server 2014 Service Pack 1 zu installieren. Hier herunterladen: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 umfasst grundlegende Funktionen für das Arbeiten mit Azure SQL-Datenbank.


### <a name="3-run-validate-script-and-sysprep"></a>3 Führen Sie 3 überprüfen Skripts und Sysprep

Auf dem Desktop mit dem virtuellen Computer heißt PowerShell-Skript überprüfen. Durch Doppelklicken auf ausführen. Es prüft, ob der virtuellen Computer verwendet werden, für den remote-hosting von Applications bereit ist. Wenn die Überprüfung abgeschlossen ist, wird es bitten Sie führen Sie Sysprep – auswählen, um Sie auszuführen.

Wenn Sysprep abgeschlossen ist, wird es den virtuellen Computer beenden.

Weitere Informationen zum Erstellen eines Bilds Azure RemoteApp finden Sie unter: [So erstellen Sie ein RemoteApp Vorlagenbild in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4 Abbild aufzuzeichnen

Wenn Sie der virtuellen Computer gestoppt wurde, finden Sie es im aktuellen-Portal zu erfassen.

Weitere Informationen zum Erfassen eines Bilds finden Sie unter [Erstellen Sie ein Bild von einer Windows Azure-virtuellen Computern erstellt, mit dem Bereitstellungsmodell klassischen](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. in Azure RemoteApp Vorlage Bilder hinzufügen

Im Abschnitt Azure RemoteApp des aktuellen Portals wechseln Sie zur Registerkarte Bilder Vorlage und dann auf Hinzufügen. Klicken Sie im Popupmenü wählen Sie aus "Ein Bild aus Ihrer Bibliothek virtuellen Computern importieren", und wählen Sie dann auf das Bild, das Sie gerade erstellt haben.



### <a name="6-create-cloud-collection"></a>6. Cloud-Websitesammlung erstellen

Erstellen Sie eine neue Azure RemoteApp Cloud-Websitesammlung im aktuellen-Portal. Wählen Sie das Bild des Vorlage, die Sie gerade importiert, mit SSMS installiert sein.

![Erstellen Sie neue Cloud-Sammlung][2]


### <a name="7-publish-ssms"></a>7. SSMS veröffentlichen

Klicken Sie auf die Veröffentlichung Registerkarte Ihrer neuen Cloud-Websitesammlung, die select veröffentlichen eine Anwendung über das Startmenü und SSMS wählen Sie aus der Liste.

![Veröffentlichen der App][5]

### <a name="8-add-users"></a>8 Hinzufügen von Benutzern

Klicken Sie auf die Registerkarte des Benutzerzugriffs können Sie die Benutzer auswählen, die Zugriff auf diese Sammlung Azure RemoteApp haben sollen, die nur SSMS enthält.

![Fügen Sie Benutzer hinzu][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9: Installieren Sie die Azure RemoteApp-Clientanwendung

Sie können herunterladen und installieren Sie hier einen Azure RemoteApp-Client: [herunterladen | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure-SQLServer zu konfigurieren

Die einzige Konfiguration erforderlich wird sichergestellt, dass Azure Services für die Firewall aktiviert ist. Wenn Sie diese Lösung verwenden, müssen Sie keine IP-Adressen zum Öffnen der Firewalls hinzufügen. Der Netzwerkverkehr, der auf dem SQL Server zulässig ist ist von anderen Diensten Azure.


![Azure zulassen][4]



## <a name="multi-factor-authentication-mfa"></a>Mehrstufige Authentifizierung (MFA)

MFA kann speziell für diese Anwendung aktiviert sein. Wechseln Sie zur Registerkarte Applications von Azure Active Directory. Für Microsoft Azure RemoteApp finden Sie einen Eintrag. Wenn Sie klicken Sie auf die Anwendung, und klicken Sie dann konfigurieren, wird die Seite unten angezeigt, in dem Sie für diese Anwendung MFA aktivieren können.

![Aktivieren Sie MFA][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Überwachen von Benutzeraktivitäten mit Azure Active Directory Premium

Wenn Sie nicht Azure AD Premium verfügen, müssen Sie es im Abschnitt Lizenzen Ihres Verzeichnisses aktivieren. Mit Premium aktiviert können Sie die Benutzer auf den Premium zuweisen.

Wenn Sie einem Benutzer in Ihrer Azure Active Directory wechseln, klicken Sie dann die Registerkarte Aktivität zu Azure RemoteApp Login Informationen finden Sie unter zu gelangen.



## <a name="next-steps"></a>Nächste Schritte

Nachdem alle der oben genannten Schritte, werden Sie den Azure RemoteApp Client ausführen und mit einem zugewiesenen Benutzer anmelden. Es wird als eine Anwendung mit SSMS angezeigt werden, und können Sie ihn ausführen, wie Sie vorgehen, wenn es auf dem Computer mit Zugriff auf Azure SQL Server installiert wurden.

Weitere Informationen zum Herstellen die Verbindung mit SQL-Datenbank, finden Sie unter [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md).


Das ist alles jetzt. Einfacheres!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png

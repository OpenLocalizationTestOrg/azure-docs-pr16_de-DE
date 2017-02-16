<properties
    pageTitle="Bereitstellen den MySQL-Anbieter für die Ressourcen auf Azure Stapel | Microsoft Azure"
    description="Ausführliche Schritte zum Azure Stapel der MySQL-Anbieter für Ressourcen bereitstellen."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Bereitstellen der Anbieter Azure Stapel zur Verwendung mit WebApps MySQL-Ressourcen

> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Verwenden Sie diesen Artikel, um die detaillierten Schritte zum Einrichten der MySQL-Anbieter für Ressourcen führen Sie auf der Azure Stapel Nachweis des Konzepts, damit Sie [mit MySQL-Datenbanken](azure-stack-mysql-rp-deploy-short.md) auf Azure Stapel, einschließlich der Verwendung von MySQL als die Back-End-WordPress Websites beginnen können mit [Azure Web Apps](azure-stack-webapps-deploy.md)erstellt.

## <a name="set-up-steps-before-you-deploy"></a>Richten Sie Schritte aus, bevor Sie bereitstellen

Bevor Sie den Ressourcenanbieter bereitstellen, müssen Sie:

- Haben Sie ein Windows Server Standardbild mit .NET 3.5
- Deaktivieren von Internet Explorer (IE) verbesserte Sicherheit
- Installieren der neuesten Version von Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Erstellen Sie ein Bild von Windows Server einschließlich .NET 3.5

Wenn Sie den Stapel Azure Bits nach 2/23/2016 heruntergeladen, da das Standardbild base Windows Server 2012 R2 .NET 3.5 Framework in diesem Download und höher umfasst, können Sie diesen Schritt überspringen.

Wenn Sie vor dem 2/23/2016 heruntergeladen haben, müssen Sie eine Windows Server 2012 R2 Datacenter virtuelle Festplatte mit .NET 3.5 Bild erstellen und Festlegen als das Standardbild im Repository Bild Plattform ist.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Deaktivieren Sie IE erweiterte Sicherheit und Aktivieren von cookies

Um eine Ressourcenanbieter bereitzustellen, führen Sie PowerShell Integrated Scripting Umgebung (ISE) als Administrator, sodass Sie Cookies und JavaScript im Internet Explorer-Profil zulassen, mit denen Sie sowohl Administrator- und melden Sie sich-ins Azure Active Directory anmelden müssen.

**Eine verbesserte Sicherheit um IE zu deaktivieren:**

1. Melden Sie sich an den Computer Azure Stapel Prüfung des Konzepts (Prüfung des Konzepts ist), als ein AzureStack-Administrator, und öffnen Sie Server-Manager.

2. Deaktivieren Sie **IE Erweiterte Sicherheitskonfiguration** für Administratoren und Benutzer aus.

3. Melden Sie sich als Administrator am virtuellen Computer **ClientVM.AzureStack.local** , und öffnen Sie Server-Manager.

4. Deaktivieren Sie **IE Erweiterte Sicherheitskonfiguration** für Administratoren und Benutzer aus.

**So aktivieren Sie Cookies:**

1. Klicken Sie auf dem Windows-Startbildschirm klicken Sie auf **Alle apps**, klicken Sie auf **Windows-Zubehör**, mit der rechten Maustaste in **Internet Explorer**, zeigen Sie auf **Weitere**, und wählen Sie dann auf **als Administrator ausführen**.

2. Wenn Sie dazu aufgefordert werden, aktivieren Sie die **Verwendung empfohlen Sicherheit**, und klicken Sie dann auf **OK**.

3. Klicken Sie in Internet Explorer auf die **Tools (Fanggeräte) Symbol** &gt; **Internetoptionen** &gt; Registerkarte **Datenschutz** .

4. Klicken Sie auf **Erweitert**, stellen Sie sicher, dass sowohl **akzeptieren** Schaltflächen ausgewählt sind, klicken Sie auf **OK**, und klicken Sie dann erneut auf **OK** .

5. Schließen Sie Internet Explorer, und starten Sie PowerShell ISE als Administrator neu.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Installieren Sie eine kompatible Stapel Azure-Version von Azure PowerShell

1. Deinstallieren Sie alle vorhandenen Azure PowerShell aus Ihrem Client virtueller Computer an.

2. Melden Sie sich bei dem Computer Azure Stapel Prüfung des Konzepts ist als eine AzureStack-Administrator.

3. Verwenden von Remote Desktop, melden Sie sich bei der **ClientVM.AzureStack.local** virtuellen Computern als Administrator.

4. Öffnen Sie die Systemsteuerung, und klicken Sie auf **Programm deinstallieren** &gt; klicken Sie auf **Azure PowerShell** &gt; klicken Sie auf **Deinstallieren**.

5. [Laden Sie die neuesten Azure PowerShell, die Azure Stapel unterstützt,](http://aka.ms/azstackpsh) und zu installieren.

    Nachdem Sie PowerShell installiert haben, können Sie diese Überprüfung PowerShell-Skript, um sicherzustellen, dass Sie Ihre Azure Stapel Instanz verbinden können (eine Web-Anmeldeseite sollte angezeigt werden) ausführen.

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Starten Sie die Ressource Anbieter Bereitstellung PowerShell

1. Herstellen einer Verbindung clientVm.AzureStack.Local mit der Remotedesktop Azure Stapel Prüfung des Konzepts ist, und melden Sie sich als Azurestack\\Azurestackuser.

2. [Laden Sie die MySQL-RP Binärdateien](http://aka.ms/masmysqlrp) ablegen und extrahieren Sie sie auf D:\\MySQLRP.

3. Führen Sie die D:\\MySQLRP\\Bootstrap.cmd Datei als Administrator (Azurestack\administrator).

    Dadurch wird die Datei Bootstrap.ps1 in PowerShell ISE geöffnet.

4. Nach Abschluss der Windows PowerShell ISE laden, klicken Sie auf die Schaltfläche "Wiedergabe", oder drücken Sie F5.

    Zwei Bereiche Registerkarten werden laden, jede enthält alle Skripts und Dateien, die Sie Ihre MySQL-Anbieter für Ressourcen bereitstellen müssen.

## <a name="prepare-prerequisites"></a>Bereiten Sie erforderliche Komponenten vor.

Klicken Sie auf die Registerkarte **Vorbereiten erforderliche Komponenten** , um:

- Erstellen der erforderlichen Zertifikate
- Laden Sie die MySQL-Binärdateien auf Ihre Azure Stapel
- Hochladen Sie Elemente mit einem Speicherkonto auf Azure Stapel
- Veröffentlichen Sie die Katalogelemente im

### <a name="create-the-required-certificates"></a>Erstellen der erforderlichen Zertifikate
Dieses Skript **Neu-SslCert.ps1** fügt die \_. AzureStack.local.pfx SSL-Zertifikat der D:\\MySQLRP\\Voraussetzungen für\\BlobStorage\\Containerordner. Das Zertifikat sichert die Kommunikation zwischen den Ressourcenanbieter und der lokalen Instanz des Azure-Managers.

1. Klicken Sie in der wichtigsten Registerkarte **Vorbereiten erforderliche Komponenten** auf die Registerkarte **Neu-SslCert.ps1** , und führen Sie es aus.

2. Geben Sie dazu aufgefordert werden, die angezeigt wird, eine PFX-Kennwort ein, die den privaten Schlüssel, und **Notieren Sie dieses Kennwort**schützen. Sie benötigen es später.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Laden Sie die MySQL-Binärdateien auf Ihre Azure Stapel

1. Wählen Sie die Registerkarte **Download-MySqlServer.ps1** und ausgeführt werden.
2. Wenn Sie dazu aufgefordert werden, klicken Sie auf **Ja** klicken Sie im Dialogfeld bestätigen um den Endbenutzer-Lizenzvertrag anzunehmen.

    Dieser Befehl fügt zwei Zip-Dateien in den Ordner D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Hochladen Sie alle Elemente mit einem Speicherkonto auf Azure Stapel

1. Klicken Sie auf der Registerkarte **Upload-Microsoft.MySql-RP.ps1** und ausgeführt werden.

2. Geben Sie im Dialogfeld Anmeldeinformationen Anforderung Windows PowerShell die Stapel Azure Service-Administratorberechtigungen.

3. Wenn Sie dazu aufgefordert werden, für die Azure Active Directory-Mandanten-ID, geben Sie Ihre Azure Active Directory-Mandanten vollqualifizierten Domänennamen ein: z. B. microsoftazurestack.onmicrosoft.com.

    Ein Popupfenster fragt Anmeldeinformationen.

    > [AZURE.TIP] Wenn das Popup angezeigt wird, Sie, die entweder IE deaktiviert noch nicht erweiterte Sicherheit, um Computer- und diese JavaScript aktivieren, oder Sie noch nicht in Internet Explorer Cookies akzeptiert. Finden Sie unter [Einrichten von Schritten vor der Bereitstellung](#set-up-steps-before-you-deploy).

4. Geben Sie Ihre Anmeldeinformationen Azure Stapel Dienstadministrator ein, und klicken Sie dann auf **Anmelden**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Veröffentlichen von Elementen aus der Galerie für das Erstellen von höher Ressourcen

Wählen Sie die Registerkarte **Veröffentlichen-GalleryPackages.ps1** und ausgeführt werden. Dieses Skript fügt zwei Marketplace-Elemente zu des Portals Azure Stapel Prüfung des Konzepts ist Marketplace, mit denen Sie Ressourcen möglichst Marketplace Elemente bereitstellen.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Bereitstellen des Anbieters MySQL-Ressourcen virtueller Computer

Jetzt, da Sie die Azure Stapel Prüfung des Konzepts ist mit der erforderlichen Zertifikate und Marketplace Elemente vorbereitet haben, können Sie eine Ressource-Anbieter für SQL Server bereitstellen. Klicken Sie auf die Registerkarte **Bereitstellen MySQL-Anbieter** , um:

   - Geben Sie Werte in einer JSON-Datei, die der Bereitstellungsprozess verweist auf
   - Bereitstellen des Anbieters für Ressourcen
   - Aktualisieren der lokalen DNS
   - Registrieren der SQL Server-Anbieter Ressourcenadapter

### <a name="provide-values-in-the-json-file"></a>Geben Sie Werte in den JSON-Datei

Klicken Sie auf **Microsoft.MySqlprovider.Parameters.JSON**. Diese Datei enthält Parameter, die die Vorlage Azure Ressourcenmanager ordnungsgemäß Azure Stapel bereitstellen muss.

1. Füllen Sie die **leeren** Parameter in die JSON-Datei:

    - Stellen Sie sicher, dass Sie die **Adminusername** und **Adminpassword** für den MySQL Ressource Anbieter virtuellen Computer bereitstellen.

    - Stellen Sie sicher, dass Sie das Kennwort für den **SetupPfxPassword** -Parameter angeben, die Sie im Schritt [Vorbereiten Prequisites](#prepare-prerequisites) Notieren Sie sich vorgenommen.

    - Stellen Sie sicher, dass Sie Parameter **BasicAuthUserName** und **BasicAuthPassword** bereitstellen. **Notieren Sie sich diese Werte.** Sie benötigen diese später registrieren den Anbieter für Ressourcen.

2. Klicken Sie auf **Speichern**.

### <a name="deploy-the-resource-provider"></a>Bereitstellen des Anbieters für Ressourcen

1. Führen Sie das Skript, und klicken Sie auf der Registerkarte **Bereitstellen-Microsoft.Mysql-provider.PS1** .
2. Geben Sie Ihren Mandanten ein, in Azure Active Directory, wenn Sie dazu aufgefordert werden.
3. Klicken Sie im Popupmenü übermitteln Sie Stapel Azure Service Administrator-Anmeldeberechtigungen.

Die endgültige Bereitstellung kann zwischen 15 und 45 Minuten auf einige hoher Auslastung Azure Stapel-Ausgereiftheit dauern.

### <a name="update-the-local-dns"></a>Aktualisieren der lokalen DNS

1. Führen Sie das Skript, und klicken Sie auf der Registerkarte **Register-Microsoft.MySQL-fqdn.ps1** .
2. Aufforderung zur Eingabe Azure Active Directory-Mandanten-ID Eingabemethoden Ihrer Azure Active Directory-Mandanten vollqualifizierten Domänennamen ein: z. B. **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Registrieren des SQL-RP-Anbieters für Ressourcen

1. Führen Sie das Skript, und klicken Sie auf der Registerkarte **Register-Microsoft.My-provider.ps1** .

2. Wenn Sie Anmeldeinformationen aufgefordert werden, verwenden Sie als die Parameter **BasicAuthUserName** und **BasicAuthPassword** notiert haben.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfen der bereitstellungs über das Azure Stapel-Portal

1. Abmelden bei der ClientVM, und melden Sie sich erneut als **AzureStack\User**.

2. Klicken Sie auf dem Desktop auf **Azure Stapel Prüfung des Konzepts ist Portal** , und melden Sie sich mit dem Portal als Administrator den Dienst an.

3. Stellen Sie sicher, dass die Bereitstellung erfolgreich war. Klicken Sie auf **Durchsuchen** &gt; **Ressourcengruppen**, klicken Sie auf die Ressourcengruppe, die Sie verwendet haben (Standard ist **MySQLRP**), und vergewissern Sie sich, dass der Essentials Teil des Blades (obere Hälfte): **Bereitstellung erfolgreich verlaufen ist lautet**.


4. Stellen Sie sicher, dass die Registrierung erfolgreich war. Klicken Sie auf **Durchsuchen** &gt; **Ressourcenanbieter**, und suchen Sie dann für **Lokale MySQL**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Erstellen Sie Ihrer ersten MySQL-Datenbank zum Testen der Bereitstellung

1. Melden Sie sich mit dem Portal Azure Stapel Prüfung des Konzepts ist als Dienst Administrator.

2. Klicken Sie auf die **+** Schaltfläche &gt; **benutzerdefinierte** &gt; **MySQL-Server und Datenbanken**.

3. Füllen Sie das Formular mit Details der Datenbank ein.

    **Notieren Sie sich die "Servername" Geben Sie ein.** Die Zeichenfolge Verbindungen für die Datenbank enthält "Servername" als Teil des Benutzernamens: z. B. ** "user@ <ServerName>"**. Sie müssen einen Benutzernamen in diesem Format Eingabemethoden, wenn Sie mit der Datenbank verbinden: Wenn Sie beispielsweise einer MySQL-Website mit dem Azure-Website Ressourcenanbieter bereitstellen


## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [Anbieter für SQL Server-Ressourcen](azure-stack-sql-rp-deploy-short.md) und den [Web Apps-Anbieter für Ressourcen](azure-stack-webapps-deploy.md)ein.

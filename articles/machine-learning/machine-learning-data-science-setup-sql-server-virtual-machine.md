<properties
    pageTitle="Einrichten einer SQL Server-virtuellen Computern als IPython Notizbuch Server | Microsoft Azure"
    description="Festlegen von einer Daten Wissenschaft virtuellen Computern mit SQL Server und IPython Server an."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Einrichten einer Azure SQL Server-virtuellen Computern als Server IPython Notizbuch für erweiterte analytics

In diesem Thema wird zum Bereitstellen und Konfigurieren einer SQL Server-virtuellen Computern als Teil einer Umgebung Wissenschaft cloudbasierten verwendet werden soll. Die Windows-Computer ist mit Unterstützung von Tools wie IPython Notizbuch, Azure-Speicher-Explorer und AzCopy sowie andere Dienstprogramme, die für Projekte, die Daten Wissenschaft hilfreich sind konfiguriert. Azure-Speicher-Explorer und AzCopy, ermöglichen das beispielsweise geeignete Hochladen von Daten in Azure Blob-Speicher von Ihrem lokalen Computer oder von Blob-Speicher auf Ihrem lokalen Computer herunterladen.

Klicken Sie im Katalog Azure-virtuellen Computern umfasst mehrere Bilder, die Microsoft SQL Server enthalten. Wählen Sie ein Bild von SQL Server virtueller Computer, die für Ihre Anforderungen Daten geeignet ist. Empfohlene Bilder sind:

- SQL Server 2012 SP2 Enterprise für kleine und mittlere Datengrößen
- SQL Server 2012 SP2 Enterprise optimiert für Data Warehousing Auslastung Groß sehr großen Datengrößen

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise Bild **einen Datenträger nicht enthält**. Sie müssen hinzufügen und/oder eine oder mehrere virtuelle Festplatten zum Speichern Ihrer Daten anfügen. Wenn Sie eine Azure-virtuellen Computern erstellen, muss es einen für das Betriebssystem zugeordneten Laufwerk C und eine temporäre Datenträger auf Laufwerk D zugeordnet. Verwenden Sie das Laufwerk D nicht zum Speichern von Daten aus. Wie der Name sagt, bietet es nur temporäre Speicherung. Keine Redundanz oder Sicherung vergleichbar, da es in Azure-Speicher befinden sich nicht.


##<a name="a-nameprovisionaconnect-to-the-azure-classic-portal-and-provision-an-sql-server-virtual-machine"></a><a name="Provision"></a>Eine Verbindung mit dem klassischen Azure-Portal und Bereitstellen einer SQL Server-virtuellen Computern

1.  Melden Sie sich mit Ihrem Konto [Klassischen Azure-Portal](http://manage.windowsazure.com/) an.
    Wenn Sie nicht über ein Azure-Konto verfügen, besuchen Sie [Azure-Testversion frei](https://azure.microsoft.com/pricing/free-trial/).

2.  Klassische Azure-Portal am linken unteren Rand der Webseite klicken Sie auf **+ neu**, klicken Sie auf **berechnen**, klicken Sie auf **virtuellen Computern**und klicken Sie dann auf **Von Katalog**.

3.  Auswählen eines Bilds seiner virtuellen Computern, die SQL Server entsprechend Ihren Anforderungen Daten enthält, und klicken Sie dann auf den Pfeil nach rechts in der unteren rechten Rand der Seite, auf der Seite **Erstellen eines virtuellen Computers** . Der aktuellste Informationen zu den unterstützten SQL Server-Bilder auf Azure finden Sie unter [Erste Schritte mit SQL Server in Azure virtuellen Computern](http://go.microsoft.com/fwlink/p/?LinkId=294720) in der Dokumentation von [SQL Server in Azure virtuellen Computern](http://go.microsoft.com/fwlink/p/?LinkId=294719) festlegen.

    ![Wählen Sie SQLServer virtueller Computer][1]

4.  Geben Sie auf der ersten Seite **Konfiguration des virtuellen Computers** die folgende Informationen ein:

    -   Geben Sie einen **NAME des virtuellen Computers**.
    -   Geben Sie im **Neuen Benutzernamen** eindeutigen Namen für das lokale Administratorkonto virtueller Computer an.
    -   Geben Sie im Feld **Neues Kennwort** ein sicheres Kennwort ein. Weitere Informationen finden Sie unter [Komplexe Kennwörter](http://msdn.microsoft.com/library/ms161962.aspx).
    -   Klicken Sie in das Feld **Kennwort bestätigen** das Kennwort erneut ein.
    -   Wählen Sie die geeignete **Größe** aus der Dropdownliste aus.

     > [AZURE.NOTE] Die Größe des virtuellen Computers wird angegeben, während der Bereitstellung: A2 ist der kleinste Schriftgrad für Produktionsarbeitslasten empfohlen. Die empfohlene Mindestgröße für einen virtuellen Computer ist A3 aus, wenn SQL Server Enterprise Edition verwenden. Wählen Sie A3 oder höher, wenn Sie SQL Server Enterprise Edition verwenden. Wählen Sie bei Verwendung von SQL Server 2012 oder 2014 Enterprise optimiert für Transaktionen Auslastung Bilder A4 aus.
Wählen Sie bei Verwendung von SQL Server 2012 oder 2014 Enterprise optimiert für Data Warehousing Auslastung Bilder A7 aus. Die ausgewählte Größe beschränkt die Anzahl der Datenträger von Daten, die Sie konfigurieren können. Aktuellste Informationen verfügbar virtuellen Computern Größen und die Anzahl der Datenträger, die Sie an einer virtuellen Computern anfügen können, finden Sie unter [Virtuellen Computern Größen für Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Preisinformationen, finden Sie unter [Virtuelle Macines Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Klicken Sie auf der nächsten Pfeil unten rechts fortsetzen.

    ![Virtueller Computer-Konfiguration][2]

5.  Konfigurieren Sie auf der zweiten Seite **Konfiguration des virtuellen Computers** Ressourcen für Netzwerke, Speicher und Verfügbarkeit aus:

    -   Wählen Sie im Feld **Cloud-Dienst** **Erstellen Sie einen neuen Clouddienst**aus.
    -   Bieten Sie den ersten Teil eines DNS-Namens Ihrer Wahl, im **Cloud-Dienst DNS-Name** , damit er einen Namen im Format **TESTNAME.cloudapp.net** abgeschlossen ist
    -   Wählen Sie im Feld **REGION Zugehörigkeit/Gruppe/virtuellen Netzwerk** einen Bereich, in dem diese Abbildung virtuellen gehostet werden soll.
    -   Wählen Sie im **Speicher-Konto**ein vorhandenes Speicherkonto aus, oder wählen Sie einen automatisch generierten aus.
    -   Wählen Sie im Feld **Verfügbarkeit festzulegen** **(keine)**aus.
    -   Lesen Sie und akzeptieren Sie die Preisinformationen.

6.  Im Abschnitt **ENDPUNKTE** klicken Sie in der Dropdownliste den leeren klicken Sie unter **NAME**auf Wählen Sie **MSSQL** , und geben Sie die Port-Nummer der Instanz der Datenbank-Engine (**1433** für die Standardinstanz).

7.  Der SQL Server virtueller Computer kann auch als IPython Notizbuch Server, dienen die wird in einem späteren Schritt konfiguriert werden.
    Hinzufügen eines neuen Endpunkts So geben Sie den Port für Ihre Server IPython Notizbuch verwendet werden soll. Geben Sie in der Spalte **NAME** einen Namen ein, wählen Sie eine Portnummer Ihrer Wahl für den öffentlichen Port, und 9999 für den Port als "Privat".

    Klicken Sie auf der nächsten Pfeil unten rechts fortsetzen.

    ![Wählen Sie MSSQL und IPython ports][3]

8.  Akzeptieren Sie die Standardoption **Installieren virtueller Computer-Agent** aktiviert ist, und klicken Sie auf der das Häkchen in der unteren rechten Ecke des Assistenten aus, um den virtuellen Computer provisioning Prozess abzuschließen.

    `![Endgültige virtueller Computer-Optionen][4]

9.  Warten Sie, während das Azure des virtuellen Computers vorbereitet. Erwarten Sie den Status virtuellen Computern fortfahren durch:

    -   (Bereitstellung) wird gestartet
    -   Beendet
    -   (Bereitstellung) wird gestartet
    -   Ausgeführt (Bereitstellung)
    -   Ausführen


##<a name="a-nameremotedesktopaopen-the-virtual-machine-using-remote-desktop-and-complete-setup"></a><a name="RemoteDesktop"></a>Öffnen des virtuellen Computers mit Remotedesktop und Setup abschließen

1.  Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf auf den Namen des virtuellen Computers zu der DASHBOARD-Seite zu wechseln. Klicken Sie am unteren Rand der Seite auf **Verbinden**.

2.  Wählen Sie zum Öffnen der Rpd-Datei, die mit dem Programm Windows-Remotedesktop (`%windir%\system32\mstsc.exe`).

3.  Geben Sie das Kennwort für das lokale Administratorkonto an, das Sie in einer früheren Schritt angegeben haben, klicken Sie im Dialogfeld **Windows-Sicherheit** .
    (Sie möglicherweise aufgefordert, die Anmeldeinformationen des virtuellen Computers zu überprüfen.)

4.  Beim ersten Anmelden mit diesem virtuellen Computer, möglicherweise mehrere Prozesse, einschließlich Einrichten von Desktop, Windows-Updates und Windows anfängliche Konfiguration Aufgaben (Sysprep) ausführen müssen. Nach Abschluss der Windows-Sysprep schließt Setup von SQL Server Aufgaben an. Die folgenden Aufgaben möglicherweise eine Verzögerung von wenigen Minuten, während der Fertigstellung. `SELECT @@SERVERNAME`möglicherweise erst zurück, den richtigen Namen SQL Server-Setup abgeschlossen ist, und SQL Server Management Studio möglicherweise nicht auf der Startseite Signalcodes ausgegeben.

Nachdem Sie den virtuellen Computer mit Windows-Remotedesktop verbunden sind, funktioniert des virtuellen Computers ähnlich wie alle anderen Computer aus. Verbinden Sie mit der Standardinstanz von SQL Server mit SQL Server Management Studio (des virtuellen Computers ausgeführt) gewohnt ein.


##<a name="a-nameinstallipythonainstall-ipython-notebook-and-other-supporting-tools"></a><a name="InstallIPython"></a>Installieren von IPython Notizbuch und andere unterstützende tools

Zum Konfigurieren Ihrer neuen SQL Server virtueller Computer um dienen als IPython Notizbuch-Server und installieren zusätzliche unterstützende Tools solche AzCopy, Azure-Speicher-Explorer, nützliche Daten Wissenschaft Python Pakete und andere ist eine spezielle Anpassung Skript Ihnen zur Verfügung gestellt. So installieren Sie:

- Mit der rechten Maustaste in des **Startmenü von Windows** -Symbol, und klicken Sie auf **Eingabeaufforderung (Admin)**
- Kopieren Sie die folgenden Befehle, und fügen Sie in die Befehlszeile eingeben.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Wenn Sie dazu aufgefordert werden, geben Sie ein Kennwort Ihrer Wahl für den Server IPython Notizbuch ein.
- Das Skript Anpassung Automatisierung mehrere Verfahren nach Abschluss der Installation, die enthalten:
    + Installation und Einrichtung des Servers IPython Notizbuch
    + Öffnen in der Windows-Firewall für die Endpunkte zuvor erstellten TCP-:
    + Für SQL Server remote connectivity
    + Für das Notizbuch IPython Server remote connectivity
    + Abrufen der Stichprobe IPython Notizbücher und SQL-Skripts
    + Herunterladen und Installieren von nützliche Daten Wissenschaft Python Pakete
    + Herunterladen und Installieren von Azure Tools wie AzCopy und Azure-Speicher-Explorer  
<br>
- Sie möglicherweise zugreifen und IPython Notizbuch aus einem beliebigen lokalen oder remote-Browser mithilfe einer URL des Formulars ausführen `https://<virtual_machine_DNS_name>:<port>`, wobei der öffentlichen IPython Port Port ist, Sie während der Bereitstellung der virtuellen Computers ausgewählt haben.
- Notizbuch IPython Server wird als Hintergrunddienst ausgeführt und wird automatisch gestartet, wenn Sie den virtuellen Computer neu starten.

##<a name="a-nameoptionalaattach-data-disk-as-needed"></a><a name="Optional"></a>Fügen Sie nach Bedarf Datenträger

Enthält das Bild virtueller Computer keinen über Datenlaufwerke, d. h., als Laufwerk C (OS Datenträger) und Laufwerk D (temporäre Datenträger), müssen Sie einen oder mehrere Daten Datenträger zum Speichern Ihrer Daten hinzufügen. Das Bild virtueller Computer für SQL Server 2012 SP2 Enterprise optimiert für Data Warehousing Auslastung kommt vorkonfiguriert zusätzliche Datenträger für Daten und Protokolldateien SQL Server.

 > [AZURE.NOTE] Verwenden Sie das Laufwerk D nicht zum Speichern von Daten aus. Wie der Name sagt, bietet es nur temporäre Speicherung. Keine Redundanz oder Sicherung vergleichbar, da es in Azure-Speicher befinden sich nicht.

Um weitere Daten Datenträger anfügen möchten, gehen Sie [So fügen Sie einen Datenträger auf einem Windows-Computer](virtual-machines-windows-classic-attach-disk.md), der Sie durch die führt beschrieben:

1. Anfügen von leeren Datenträger virtuellen Computer nach der Bereitstellung in früheren Schritten
2. Initialisierung der die neue Laufwerke des virtuellen Computers


##<a name="a-namessmsaconnect-to-sql-server-management-studio-and-enable-mixed-mode-authentication"></a><a name="SSMS"></a>Verbinden mit SQL Server Management Studio und gemischten Authentifizierungsmodus aktivieren

Die SQL Server-Datenbank-Engine können keine Windows-Authentifizierung ohne Domäne-Umgebung verwenden. Konfigurieren von SQL Server für gemischten Authentifizierungsmodus zum Verbinden mit der Datenbank-Engine von einem anderen Computer. Gemischten Authentifizierungsmodus kann sowohl SQL Server-Authentifizierung und Windows-Authentifizierung. SQL-Authentifizierungsmodus ist erforderlich, um die Daten direkt aus Ihren SQL Server virtueller Computer Datenbanken in [Azure maschinellen Learning Studio](https://studio.azureml.net) mithilfe des Moduls Daten importieren Aufnahme.

1.  Während der virtuellen Computer über Remote Desktop verbunden, verwenden Sie die Windows- **Suche** (Bereich), und geben Sie **SQL Server Management Studio** (SMSS). Klicken Sie auf diese Option, um den SQL Server Management Studio (SSMS) zu starten. Möglicherweise möchten eine Verknüpfung auf dem Desktop für eine zukünftige Verwendung SSMS hinzufügen.

    ![Starten Sie SSMS][5]

    Zum ersten Mal öffnen von Management Studio müssen sie die Benutzer Management Studio-Umgebung zu erstellen. Dies kann einige Minuten dauern.

2.  Beim Öffnen, bietet Management Studio im Dialogfeld **mit Server verbinden** . Geben Sie im Feld **Servername** den Namen des virtuellen Computers in Verbindung mit der Datenbank-Engine mit Objekt-Explorer ein.
    (Anstelle der Name des virtuellen Computers können Sie auch **(lokal)** oder ein Punkt als **Servernamen**verwenden. Wählen Sie die **Windows-Authentifizierung**, und lassen Sie ** *Ihrer\_virtuellen Computer\_Namen*\\der\_lokale\_Administrator** in das Feld **Benutzername** ein. Klicken Sie auf **Verbinden**.

    ![Verbinden mit Server][6]

    <br>

     > [AZURE.TIP] Sie können den SQL Server-Authentifizierungsmodus verwenden ein Windows-Registrierung Key ändern oder Verwenden von SQL Server Management Studio geändert werden. Um mit der Änderung der Registrierung Key-Authentifizierungsmodus zu ändern, starten Sie eine **Neue Abfrage** zu, und führen Sie Folgendes Skript:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    So ändern Sie den Authentifizierungsmodus mit SQL Server Management Studio

3.  Klicken Sie in **SQL Server Management Studio Objekt-Explorer**mit der rechten Maustaste in des Namens der Instanz von SQL Server (der Name des virtuellen Computers) aus, und klicken Sie dann auf **Eigenschaften**.

    ![Servereigenschaften][7]

4.  Klicken Sie auf der Seite **Sicherheit** , klicken Sie unter **Serverauthentifizierung**wählen Sie **SQL Server und Windows-Authentifizierungsmodus**, und klicken Sie dann auf **OK**.

    ![Authentifizierungsmodus auswählen][8]

5.  Klicken Sie im Dialogfeld **SQL Server Management Studio** klicken Sie auf **OK** , um die Anforderung zum Neustart von SQL Server zu bestätigen.

6.  Im **Objekt-Explorer**mit der rechten Maustaste in des Servers, und klicken Sie dann auf **neu starten**. (Wenn SQL Server-Agent ausgeführt wird, müssen sie auch gestartet.)

    ![Neu starten][9]

7.  Klicken Sie auf **Ja** , um zu bestätigen, dass Sie SQL Server neu starten möchten, klicken Sie im Dialogfeld **SQL Server Management Studio** .

##<a name="a-nameloginsacreate-sql-server-authentication-logins"></a><a name="Logins"></a>Erstellen von SQL Server-Authentifizierung Benutzernamen

Um die Datenbank-Engine von einem anderen Computer zu verbinden, müssen Sie mindestens eine SQL Server-Authentifizierung Login erstellen.  

Sie können neue SQL Server-Benutzernamen programmgesteuert erstellen oder SQL Server Management Studio verwenden. Starten Sie eine **Neue Abfrage** zu, und führen Sie Folgendes Skript, um einen neuen Sysadmin Benutzer mit SQL-Authentifizierung zu erstellen. Ersetzen Sie < neuen Benutzernamen\> und < Neues Kennwort\> mit Ihrer Wahl der *Benutzernamen* und *Ihr Kennwort ein*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Passen Sie die Kennwortrichtlinien Bedarf (der Stichprobe Code schaltet Richtlinie überprüfen und Ihr Kennwort ein Benutzerkennwort). Weitere Informationen zu SQL Server-Benutzernamen finden Sie unter [Erstellen einer Login](http://msdn.microsoft.com/library/aa337562.aspx).  

So erstellen Sie neue SQL Server-Benutzernamen mithilfe von SQL Server Management Studio

1.  Erweitern Sie in **SQL Server Management Studio Objekt-Explorer**den Ordner, der die Server-Instanz, in der Sie den neuen Benutzernamen erstellen möchten.

2.  Mit der rechten Maustaste in des Ordners **Sicherheit** , zeigen Sie auf **neu**, und wählen Sie **Login...**aus.

    ![Neuer Benutzername][10]

3.  Geben Sie den Namen des neuen Benutzers in das Feld **Login Name** auf der Registerkarte **Allgemein** im Dialogfeld **Anmeldung - neu** .

4.  Wählen Sie **SQL Server-Authentifizierung**aus.

5.  Geben Sie in das Feld **Kennwort** ein Kennwort für den neuen Benutzer ein. Geben Sie das Kennwort erneut in das Feld **Kennwort bestätigen** ein.

6.  Wählen Sie zum Erzwingen der Richtlinie Kennwortoptionen für Komplexität und Durchsetzung **Kennwortrichtlinien erzwingen** (empfohlen). Dies ist eine Standardoption, wenn SQL Server-Authentifizierung aktiviert ist.

7.  Wählen Sie zum Erzwingen der Richtlinie Kennwortoptionen für Ablauf **Erzwingen des Kennwortablaufs** (empfohlen) aus. Erzwingen der Kennwortrichtlinien muss aktiviert sein, um dieses Kontrollkästchen zu aktivieren. Dies ist eine Standardoption, wenn SQL Server-Authentifizierung aktiviert ist.

8.  Wenn Sie erzwingen, dass den Benutzer ein neues Kennwort nach dem ersten Mal zu erstellen, die der Benutzername verwendet wird, wählen Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** (empfohlen, ist diese Anmeldung für eine andere Person zu verwenden. Aktivieren Sie ist der Benutzername für Ihre Zwecke verwenden, führen Sie diese Option nicht.) Erzwingen des Kennwortablaufs muss aktiviert sein, um dieses Kontrollkästchen zu aktivieren. Dies ist eine Standardoption, wenn SQL Server-Authentifizierung aktiviert ist.

9.  Wählen Sie aus der Liste **Standard-Datenbank** aus einer Standarddatenbank für die Anmeldung. **Master** -Datenbank das Standardformat für diese Option ist. Wenn Sie eine Benutzerdatenbank noch nicht erstellt haben, behalten Sie die **Gestaltungsvorlage**.

10. Lassen Sie in der Liste **Standardsprache** **Standard** als Wert ein.

    ![Anmelde-Eigenschaften][11]

11. Ist dies das erste Login zu erstellende, möchten Sie möglicherweise diese Anmeldung als Administrator SQL Server zu bestimmen. Ist dies der Fall ist, aktivieren Sie auf der Seite **Serverrollen** **Sysadmin**ein.

    > [AZURE.IMPORTANT] Mitglieder der festen Rolle Sysadmin haben vollständige Kontrolle über die Datenbank-Engine. Aus Gründen der Sicherheit sollten Sie sorgfältig Mitgliedschaft in diese Rolle einschränken.

    ![sysadmin][12]

12. Klicken Sie auf OK.

##<a name="a-namednsadetermine-the-dns-name-of-the-virtual-machine"></a><a name="DNS"></a>Bestimmen Sie den DNS-Namen des virtuellen Computers

Zum Verbinden mit SQL Server-Datenbank-Engine von einem anderen Computer müssen Sie den Namen (DNS = Domain Name System) des virtuellen Computers kennen.

(Dies ist der Name, der im Internet zur Identifizierung des virtuellen Computers verwendet. Sie können die IP-Adresse verwenden, aber die IP-Adresse ändert sich möglicherweise die beim Azure Ressourcen für Redundanz oder Wartung ausgeführt werden. Der DNS-Name wird unveränderliche, da es an eine neue IP-Adresse umgeleitet werden kann.)

1.  Wählen Sie in der klassischen Azure-Portal (oder aus dem vorherigen Schritt) **virtuellen Computern**aus.

2.  Suchen Sie auf der Seite in der Spalte **NAME der DNS-** **Virtuellen Computern Instanzen** und kopieren Sie den DNS-Namen des virtuellen Computers **http://**vorangestellt wird. (Die Benutzeroberfläche möglicherweise nicht den gesamten Name angezeigt, jedoch können Sie mit der rechten Maustaste darauf, und wählen Sie kopieren.)

##<a name="a-namecdeaconnect-to-the-database-engine-from-another-computer"></a><a name="cde"></a>Verbinden Sie mit der Datenbank-Engine von einem anderen computer

1.  Öffnen Sie auf einem Computer mit dem Internet verbunden ist SQL Server Management Studio aus.

2.  Geben Sie im Dialogfeld **mit Server verbinden** oder **Verbinden mit Datenbank-Engine** in das Feld **Servername** den DNS-Namen des virtuellen Computers (festgelegt in den vorherigen Vorgang) und einer öffentlichen Endpunkt Port Zahl in das Format der *DNS-Name, Portnumber* wie **tutorialtestVM.cloudapp.net,57500**.

3.  Wählen Sie im Feld **Authentifizierung** **SQL Server-Authentifizierung**ein.

4.  Geben Sie im Feld **Benutzername** ein, die Sie in einer früheren Aufgabe erstellt Benutzername.

5.  Geben Sie im Feld **Kennwort** das Kennwort für den Benutzernamen, den die in einer früheren Aufgabe erstellt.

6.  Klicken Sie auf **Verbinden**.

##<a name="a-nameamlconnectaconnect-to-the-database-engine-from-azure-machine-learning"></a><a name="amlconnect"></a>Verbinden Sie mit der Datenbank-Engine von Learning Azure-Computern

In späteren Phasen des Prozesses Wissenschaft Team Daten verwenden Sie die [Azure maschinellen Learning Studio](https://studio.azureml.net) zu erstellen und Computer Learning Datenmodellen bereitzustellen. Um Daten aus der SQL Server virtueller Computer Datenbanken direkt in Azure maschinellen Learning für Schulung oder bewerten Aufnahme, verwenden Sie das Modul **Importieren von Daten** in einer neuen [Azure maschinellen Learning Studio](https://studio.azureml.net) experimentieren. In diesem Thema wird in weitere Details über die Team Daten Wissenschaft Prozess Leitfaden Links behandelt. Eine Einleitung, finden Sie unter [Neuigkeiten Azure maschinellen Learning Studio?](machine-learning-what-is-ml-studio.md).

2.  Wählen Sie im Bereich **Eigenschaften** des [Moduls Importieren von Daten](https://msdn.microsoft.com/library/azure/dn905997.aspx)aus der Dropdownliste **Datenquelle** **Azure SQL-Datenbank** aus.

3.  Geben Sie in das Textfeld **Datenbankservername**`tcp:<DNS name of your virtual machine>,1433`

4.  Geben Sie den SQL-Benutzernamen in das Textfeld **Server-Konto Benutzernamen** ein.

5.  Geben Sie in das Textfeld **Server des Kennworts des Benutzerkontos** Sql-Benutzerkennwort ein.

    ![Azure ML importieren von Daten][13]

##<a name="a-nameshutdownashutdown-and-deallocate-virtual-machine-when-not-in-use"></a><a name="shutdown"></a>War(en) und Freigeben von virtuellen Computern nicht in verwenden

Der Preis von Azure-virtuellen Computern sind als **bezahlen nur für was Sie verwenden**. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden sind während des virtuellen Computers nicht verwenden, muss es in den Zustand **beendet (Deallocated)** werden.

> [AZURE.NOTE] Beenden des virtuellen Computers aus innerhalb (mit Windows Power Optionen), der virtuellen Computer beendet ist aber bleibt verteilt. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden sind, beenden Sie immer virtuellen Computern vom [Klassischen Azure-Portal](http://manage.windowsazure.com/). Sie können auch den virtuellen Computer über Powershell beenden, indem Sie ShutdownRoleOperation mit "PostShutdownAction" gleich "StoppedDeallocated" aufrufen.

Beenden und des virtuellen Computers freigeben:

1. Melden Sie sich mit Ihrem Konto [Klassischen Azure-Portal](http://manage.windowsazure.com/) an.  

2. Wählen Sie in der linken Navigationsleiste **virtuellen Computern** aus.

3. Klicken Sie auf den Namen des virtuellen Computers, und wechseln Sie zu der Seite **DASHBOARD** , in der Liste von virtuellen Computern.

4. Klicken Sie am unteren Rand der Seite auf **Beenden**.

![Virtueller Computer war(en)][15]

Des virtuellen Computers werden freigegeben, aber nicht gelöscht. Sie können Ihre virtuellen Computers zu einem beliebigen Zeitpunkt vom klassischen Azure-Portal neu starten.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Ihre Azure SQL Server virtueller Computer verwendet wird: Was nächsten ist?

Ihre virtuellen Computern kann nun in Ihrer Daten Wissenschaft Übungen verwenden. Des virtuellen Computers ist auch als ein Notizbuch IPython Server verwendet, für die Untersuchung und Bearbeitung der Daten sowie zu anderen Aufgaben in Verbindung mit Azure maschinellen Lern- und das Team Daten Wissenschaft Prozess (TDSP) bereit.

Die nächsten Schritte beim Wissenschaft von Daten im [Team Daten Wissenschaft Vorgang](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet sind und Schritte, die Verschieben von Daten in HDInsight, verarbeiten und dort Vorbereitung Learning aus den Daten mit den Azure Computer vertraut machen (Beispiel) einschließen.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 

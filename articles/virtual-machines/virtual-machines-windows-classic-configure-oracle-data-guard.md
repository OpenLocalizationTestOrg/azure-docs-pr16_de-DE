<properties
    pageTitle="Konfigurieren von Oracle Data Guard in virtuellen Computern | Microsoft Azure"
    description="Durchlaufen Sie ein Lernprogramm für das Einrichten und Oracle Data Guard auf Azure-virtuellen Computern für hohe Verfügbarkeit und Disaster Wiederherstellung implementieren."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Konfigurieren von Oracle Data Guard für Azure


In diesem Lernprogramm wird veranschaulicht, wie einrichten und Oracle Data Guard in Azure-virtuellen Computern-Umgebung für hohe Verfügbarkeit und Wiederherstellung implementieren. Das Lernprogramm befasst sich mit unidirektionalen Replikation für nicht - RAC Oracle-Datenbanken auf.

Oracle Data Guard unterstützt Datenschutz und Wiederherstellung für Oracle-Datenbank. Es ist eine einfache, leistungsfähige, Technik Lösung für Wiederherstellung, Datenschutz und hohen Verfügbarkeit für die gesamte Oracle-Datenbank.

In diesem Lernprogramm wird davon ausgegangen, dass bereits theoretischen und praktischen Knowledge auf Oracle-Datenbank hohen Verfügbarkeit und Wiederherstellung Konzepte. Informationen finden Sie in der [Oracle-Website](http://www.oracle.com/technetwork/database/features/availability/index.html) und [Oracle Data Guard Konzepte und Administration Guide](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Darüber hinaus wird das Lernprogramm davon ausgegangen, dass Sie die folgenden Vorkenntnisse bereits implementiert haben:

- Sie haben bereits im Abschnitt hohe Verfügbarkeit und Disaster Wiederherstellung Aspekte im Thema [Oracle virtuellen Computern Bilder - verschiedene Aspekte](virtual-machines-windows-classic-oracle-considerations.md) überprüft. Azure unterstützt derzeit eigenständigen Oracle-Datenbank Instanzen aber nicht Oracle Real Application Cluster (Oracle RAC).


- Sie haben zwei virtuellen Computern (virtuellen Computern) in Azure Verwendung derselben Plattform bereitgestellten Oracle Enterprise Edition Bild erstellt. Stellen Sie sicher, dass die virtuellen Computern sind in der [gleichen Cloud-Dienst](virtual-machines-windows-load-balance.md) und in der gleichen virtuellen Netzwerk, um sicherzustellen, dass diese miteinander über die beständigen private IP-Adresse zugegriffen werden können. Darüber hinaus wird empfohlen, platzieren Sie die virtuellen Computern in der gleichen [Verfügbarkeit festlegen](virtual-machines-windows-manage-availability.md) dürfen Azure in separaten Fehlerstrukturanalyse Domänen zu platzieren, und Aktualisieren von Domänen. Oracle Data Guard ist nur mit einer Oracle-Datenbank Enterprise Edition verfügbar. Jeder Computer muss mindestens 2 GB Arbeitsspeicher und 5 GB Festplattenspeicher verfügen. Die aktuellste Informationen auf der Plattform bereitgestellten virtuellen Computer Größen finden Sie unter [Virtuellen Computern Größen für Azure](virtual-machines-windows-sizes.md). Wenn Sie zusätzliche Datenträger für Ihre virtuellen Computer benötigen, können Sie zusätzliche Datenträger anfügen. Informationen finden Sie unter [So fügen Sie einen Datenträger mit einem virtuellen Computer](virtual-machines-windows-classic-attach-disk.md).



- Sie haben die virtuellen Computernamen als "Machine1" für die primäre virtueller Computer und "Machine2" für den standby virtuellen Computer in der klassischen Azure-Portal festlegen.

- Sie haben die **$oracle_home** Umgebungsvariable auf derselben Oracle-Installation Stammpfad in der primären und Standby-virtuellen Computern, verweisen Sie wie festlegen `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Sie melden Sie sich bei Ihrem WindowsServer als Mitglied der Gruppe der **Administratoren** oder als Mitglied der Gruppe **ORA_DBA** .

In diesem Lernprogramm werden Sie folgende Aufgaben ausführen:

Implementieren der Umgebung physische standby-Datenbank

1. Erstellen einer primären Datenbank

2. Vorbereiten der primären Datenbank für die Erstellung von standby-Datenbanken

    1. Erzwungene Protokollierung aktivieren

    2. Erstellen Sie eine Kennwortdatei

    3. Konfigurieren Sie ein Wiederholungsprotokoll standby

    4. Aktivieren der Archivierung

    5. Festlegen der primären Datenbank Initialisierungsparameter

Erstellen Sie eine physische standby-Datenbank

1. Vorbereiten einer Initialisierung Parameter Masterdatei standby-Datenbanken

2. Konfigurieren Sie die Zuhörer und Tnsnames, um Unterstützung für die Datenbank auf primäre und standby-Computern

    1. Konfigurieren von listener.ora auf beiden Servern, die Einträge für beide Datenbanken enthalten soll.

    2. Konfigurieren von Einträgen für primäre und standby-Datenbanken enthalten soll, tnsnames.ora auf die primäre und standby-virtuellen Computern. 

    3. Starten Sie die Zuhörer, und aktivieren Sie Tnsping auf beide virtuellen Computern für beide Dienste.

3. Starten Sie die standby-Instanz Nomount Zustand

4. Verwenden Sie RMAN, um die Datenbank zu klonen und zum Erstellen einer standby-Datenbank

5. Starten Sie die physische standby-Datenbank im Wiederherstellungsmodus für verwaltete

6. Überprüfen der physischen standby-Datenbank

> [AZURE.IMPORTANT] In diesem Lernprogramm wurde einrichten und mit der folgenden Hard- und Software Konfiguration getestet:
>
>|                      | **Primäre Datenbank**                      | **Standby-Datenbanken**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle-Version**   | Oracle11g Enterprise-Version (11.2.0.4.0) | Oracle11g Enterprise-Version (11.2.0.4.0) |
>| **Name des Computers**     | Machine1                                  | Machine2                                  |
>| **Betriebssystem** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle SID**       | TEST                                      | TEST\_STBY                                |
>| **Arbeitsspeicher**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Festplattenspeicher**       | Min 5 GB                                  | Min 5 GB                                  |

Für nachfolgende Versionen von Oracle-Datenbank und Oracle Data Guard möglicherweise einige weiteren Änderungen, die implementiert werden müssen. Die aktuellste Version-spezifische Informationen finden Sie unter [Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) und [Oracle-Datenbank](http://www.oracle.com/us/corporate/features/database-12c/index.html) -Dokumentation auf Oracle-Website.

##<a name="implement-the-physical-standby-database-environment"></a>Implementieren der Umgebung physische standby-Datenbank
### <a name="1-create-a-primary-database"></a>1. erstellen Sie 1. eine primäre Datenbank

- Erstellen einer primären Datenbank "TEST" in der primären virtuellen Computern. Informationen finden Sie unter Erstellen und Konfigurieren einer Oracle-Datenbank.
- Wenn Sie den Namen der Datenbank anzuzeigen, Herstellen einer Verbindung mit Ihrer Datenbank als SYS-Benutzer mit SYSDBA Rolle in der SQL * Plus Eingabeaufforderungsfenster, und führen Sie die folgende Anweisung:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Als Nächstes Abfrage die Namen der Datenbankdateien aus der aktuellen Ansicht Dba_data_files aus:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. Vorbereiten der primären Datenbank für die Erstellung von standby-Datenbanken

Vor dem Erstellen einer Datenbank standby, empfiehlt es sich, dass Sie sicherstellen, dass die primäre Datenbank ordnungsgemäß konfiguriert ist. Im folgenden finden eine Liste der Schritte, die Sie ausführen müssen:

1. Erzwungene Protokollierung aktivieren
2. Erstellen Sie eine Kennwortdatei
3. Konfigurieren Sie ein Wiederholungsprotokoll standby
4. Aktivieren der Archivierung
5. Festlegen der primären Datenbank Initialisierungsparameter

#### <a name="enable-forced-logging"></a>Erzwungene Protokollierung aktivieren

Um eine Datenbank Standby implementieren, müssen wir "Protokollierung wird" in der primären Datenbank aktivieren. Diese Option wird sichergestellt, dass auch wenn ein Vorgang 'Nologging' Fertig ist, erzwingen, dass Protokollierung hat Vorrang vor, und alle Vorgänge die Protokolle wiederholen angemeldet sind. Daher wird sichergestellt, dass alle Elemente in der primären Datenbank angemeldet ist, und der Standby-Replikation alle Vorgänge in der primären Datenbank umfasst. Führen Sie zum Aktivieren der Protokollierung erzwingen, dass der Alter Database-Anweisung ein:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Erstellen Sie eine Kennwortdatei

Damit verschicken und archivierte Protokolle vom primären Server auf den Standby-Server anwenden, muss das Kennwort Sys auf primäre und standby-Servern identisch sein. Das war warum Sie eine Datei in der primären Datenbank erstellen und es auf dem Server Standby zu kopieren.

>[AZURE.IMPORTANT] Wenn der Oracle-Datenbank 12 c verwenden, ist es ein neuer Benutzer, **SYSDG**, die Sie verwenden können, Oracle Data Guard verwalten ein. Weitere Informationen finden Sie unter [Änderungen in 12 c Version der Oracle-Datenbank](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Darüber hinaus sicherstellen, dass die ORACLE\_Start-Umgebung, die bereits in Machine1 definiert ist. Wenn nicht, als Umgebungsvariable-, die über das Dialogfeld Umgebungsvariablen definieren. Um dieses Dialogfeld zu öffnen, starten Sie **das Programm** durch Doppelklicken auf das Symbol in der **Systemsteuerung**; Klicken Sie dann klicken Sie auf die Registerkarte **Erweitert** , und wählen Sie **Umgebungsvariablen**. Wenn die Umgebungsvariablen festlegen möchten, klicken Sie auf die Schaltfläche ' **neu** ' unter **Systemvariablen**. Nach dem Einrichten der Umgebungsvariablen, schließen Sie die vorhandenen Windows-Befehlszeile, und öffnen Sie eine neue.

Führen Sie folgende Anweisung zum Umschalten auf die Oracle\_Verzeichnis "Privat", z. B. C:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Datenbank.

    cd %ORACLE_HOME%\database

Klicken Sie dann erstellen Sie mithilfe der Kennwort Datei Erstellung Programms [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm)Kennwortdatei ein. Führen Sie den folgenden Befehl in der gleichen Windows-Befehlszeile in Machine1 durch den Wert Password als das Kennwort des **SYS**festlegen:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Dieser Befehl erstellt eine Kennwortdatei, die als PWDTEST.ora, in die ORACLE namens\_Start\\Datenbank Directory. Kopieren Sie diese Datei auf % ORACLE\_Start %\\Datenbank-Verzeichnis in Machine2 manuell.

#### <a name="configure-a-standby-redo-log"></a>Konfigurieren Sie ein Wiederholungsprotokoll standby

Klicken Sie dann müssen Sie ein Standby wiederholen Protokoll konfigurieren, damit die primäre ordnungsgemäß der Wiederholen empfangen kann, wenn es sich um eine Standby wird. Vorab hier erstellen, können auch die Protokolle standby wiederholen, klicken Sie auf der Standby automatisch erstellt werden. Es ist wichtig, Standby wiederholen Protokolle (SRL) mit der gleichen Größe wie der online Protokolle wiederholen zu konfigurieren. Die Größe der aktuellen standby wiederholen Protokolldateien muss exakt die Größe der aktuellen primären Datenbank online wiederholen Protokolldateien übereinstimmen.

Führen die folgende Anweisung in den SQL-Code\*PLUS in Machine1 Eingabeaufforderungsfenster. Die Protokolldatei V$ ist eine Ansicht "System", die Informationen zu wiederholen Protokolldateien enthält.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Beachten Sie, dass 52428800 gesetzt 50 MB ist.

Klicken Sie dann in den SQL-Code\*Plus-Fenster Ausführen die folgenden Aussagen zum standby-Datenbank eine neue standby wiederholen Log Dateigruppe hinzu, und geben Sie eine Zahl, die mit der Gruppe-Klausel Gruppe bezeichnet. Gruppe von Zahlen verwenden, kann die Verwaltung von standby wiederholen Log Dateigruppen einfacher vornehmen:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Führen Sie dann die folgenden Systemansicht zu Listeninformationen zu wiederholen Protokolldateien. Dieser Vorgang auch überprüft, ob die standby wiederholen Log Dateigruppen erstellt wurden:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Aktivieren der Archivierung

Aktivieren Sie dann, die durch Ausführen der folgenden Aussagen so setzen die primäre Datenbank im ARCHIVELOG Modus, und aktivieren Sie die automatische Archivierung Archivierung. Sie können Archiv Log-Modus aktivieren, durch die Bereitstellung der Datenbank, und klicken Sie dann auf den Archivelog-Befehl ausführen.

Zuerst, melden Sie sich als Sysdba an. Führen Sie in der Windows-Befehlszeile:

    sqlplus /nolog

    connect / as sysdba

Klicken Sie dann war(en) der Datenbank in den SQL-Code\*Plus Eingabeaufforderungsfenster:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Führen Sie dann den Befehl Start bereitstellen, die Datenbank bereitzustellen. Dadurch wird sichergestellt, dass die angegebene Datenbank Oracle die Instanz zuordnet.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Führen Sie dann:

    SQL> alter database archivelog;
    Database altered.

Klicken Sie dann Ausführen der Alter Database-Anweisung mit der Open-Klausel, um die Datenbank zum normalen Gebrauch verfügbar zu machen:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Festlegen der primären Datenbank Initialisierungsparameter

Um die Data Guard konfigurieren zu können, müssen Sie erstellen und konfigurieren die standby-Parameter auf eine normale Pfile (Initialisierung Parameter Textdatei) ersten. Wenn die Pfile fertig ist, müssen Sie es in eine Server Parameter (SPFILE) konvertieren.

Sie können mithilfe der Parameter in der Initialisierung die Data Guard-Umgebung steuern. ORA-Datei. Wenn in diesem Lernprogramm zu folgen, müssen Sie die primäre Datenbank Initialisierung zu aktualisieren. ORA, damit die It beide Rollen enthalten kann: primären oder Standby.

    SQL> create pfile from spfile;
    File created.

Als Nächstes müssen Sie zum Bearbeiten der Pfile, um die standby-Parameter hinzuzufügen. Öffnen Sie hierzu die INITTEST ein. ORA-Datei in den Speicherort der % ORACLE\_Start %\\Datenbank. Als Nächstes Anfügen der folgenden Aussagen zu der Datei INITTEST.ora ein. Die Benennungskonvention für Ihre Initialisierung. ORA Datei ist Initialisierung\<Ihr Datenbankname\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Der vorherigen Anweisungsblock umfasst drei wichtige Setup Elemente:
-   **... LOG_ARCHIVE_CONFIG:** Sie definieren die eigene Datenbank­IDs verwenden diese Anweisung.
-   **... LOG_ARCHIVE_DEST_1:** Sie definieren den lokale Archivierung Ordnerspeicherort diese Anweisung verwenden. Es empfiehlt sich, ein neues Verzeichnis für Archivierung Anforderungen Ihrer Datenbank erstellen, und geben Sie den Speicherort lokalen Archiv, verwenden diese Anweisung explizit anstelle der Verwendung von Oracle standardmäßigen Ordner % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... LGWR ASYNCHRONE...:** Sie einen asynchrone Log Autor Prozess (LGWR) Transaktion wiederholen Daten sammeln und senden es an standby Ziele definieren. Hier gibt der DB_UNIQUE_NAME einen eindeutigen Namen für die Datenbank am Ziel standby-Server an.

Nachdem die neue Parameterdatei fertig ist, müssen Sie die Spfile daraus zu erstellen.

Zuerst war(en) der Datenbank:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Führen Sie als Nächstes Start Nomount Befehl wie folgt aus:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Erstellen Sie nun eine Spfile aus:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Klicken Sie dann war(en) der Datenbank:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Verwenden Sie den Startbefehl klicken Sie dann zum Starten einer Instanz:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Erstellen Sie eine physische standby-Datenbank
In diesem Abschnitt Schwerpunkt auf die Schritte, die in Machine2 vorbereiten die physische standby-Datenbank ausgeführt werden müssen.

Zuerst müssen Sie Remotedesktop über das klassische Azure-Portal an Machine2.

Klicken Sie dann auf dem Server Standby (Machine2) erstellen Sie die erforderlichen Ordner für die standby-Datenbank, wie z. B. C:\\\<YourLocalFolder\>\\testen. Sicherzustellen Sie beim Durchführen dieses Lernprogramms, dass die Ordnerstruktur die Ordnerstruktur auf Machine1 bleiben alle notwendigen Dateien, wie z. B. Controlfile, Datendateien, Redologfiles, Udump, Bdump und Cdump Dateien entspricht. Darüber hinaus definieren den ORACLE\_HOME and ORACLE\_Basis Umgebungsvariablen in Machine2. Andernfalls definieren Sie sie als Umgebungsvariable-, die über das Dialogfeld Umgebungsvariablen ein. Um dieses Dialogfeld zu öffnen, starten Sie **das Programm** durch Doppelklicken auf das Symbol in der **Systemsteuerung**; Klicken Sie dann klicken Sie auf die Registerkarte **Erweitert** , und wählen Sie **Umgebungsvariablen**. Wenn die Umgebungsvariablen festlegen möchten, klicken Sie auf die Schaltfläche ' **neu** ' unter **Systemvariablen**. Nach dem Einrichten der Umgebungsvariablen, müssen Sie vorhandene Windows-Befehlszeile schließen und öffnen Sie eine neue auf, um die Änderungen anzuzeigen.

Als Nächstes gehen Sie folgendermaßen vor:

1. Vorbereiten einer Initialisierung Parameter Masterdatei standby-Datenbanken

2. Konfigurieren Sie die Zuhörer und Tnsnames, um Unterstützung für die Datenbank auf primäre und standby-Computern

    1. Konfigurieren von listener.ora auf beiden Servern, die Einträge für beide Datenbanken enthalten soll.

    2. Konfigurieren von tnsnames.ora auf die primäre und standby virtuellen Computern, die Einträge für primäre und standby-Datenbanken enthalten soll.

    3. Starten Sie die Zuhörer, und aktivieren Sie Tnsping auf beide virtuellen Computern für beide Dienste.

3. Starten Sie die standby-Instanz Nomount Zustand

4. Verwenden Sie RMAN, um die Datenbank zu klonen und zum Erstellen einer standby-Datenbank

5. Starten Sie die physische standby-Datenbank im Wiederherstellungsmodus für verwaltete

6. Überprüfen der physischen standby-Datenbank

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. Vorbereiten einer Initialisierung Parameter Masterdatei standby-Datenbanken

In diesem Abschnitt wird veranschaulicht, wie eine Initialisierung Parameter-Datei für die Datenbank standby vorbereiten. Dazu müssen Sie zuerst kopieren Sie die INITTEST. ORA Dateien von Computer 1 an Machine2 manuell. Sie sollten die INITTEST verdeckt werden sollen. In den Feldern % ORACLE ORA-Datei\_Start %\\Datenbankordner in den beiden Computern. Ändern Sie die Datei INITTEST.ora in Machine2, sie können die standby Rolle gemäß Angabe unter festzulegen:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Der vorherigen Anweisungsblock umfasst zwei wichtige Setup Elemente:

-   **\*. LOG_ARCHIVE_DEST_1:** müssen Sie den Ordner c:\OracleDatabase\TEST_STBY\archives in Machine2 manuell zu erstellen.
-   **\*. LOG_ARCHIVE_DEST_2:** Dies ist ein optionaler Schritt. Diese Einstellung wird nach Bedarf möglicherweise, wenn der primäre Computer befindet sich in Wartung und der Computer standby eine primäre Datenbank wird.

Klicken Sie dann müssen Sie die standby-Instanz gestartet. Geben Sie den folgenden Befehl in einer Windows-Befehlszeile zum Erstellen einer Oracle-Instanz durch erstellen einen Windows-Dienst auf dem Datenbankserver standby:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Der Befehl **Oradim** erstellt eine Instanz der Oracle kann jedoch nicht gestartet. Finden Sie es im Laufwerk C:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Verzeichnis BIN.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Konfigurieren Sie die Zuhörer und Tnsnames, um Unterstützung für die Datenbank auf primäre und standby-Computern
Bevor Sie eine standby Datenbank erstellen, müssen Sie sicherstellen, dass die primären und standby-Datenbanken in der Konfiguration miteinander kommunizieren können. Dazu müssen Sie sowohl die TNSNames die Zuhörer manuell oder mithilfe der Konfiguration-Netzwerkkonfiguration NETCA zu konfigurieren. Dies ist eine obligatorische Aufgabe, wenn Sie die Wiederherstellung-Managers (RMAN) verwenden.

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Konfigurieren von listener.ora auf beiden Servern, die Einträge für beide Datenbanken enthalten soll.

Remotedesktop Machine1 und die Datei listener.ora als unten angegebene bearbeiten. Wenn Sie die Datei listener.ora bearbeiten, stellen Sie immer sicher, dass die öffnende und schließende Klammer in derselben Spalte auszurichten. Die Datei listener.ora finden Sie im folgenden Ordner: c:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Netzwerk\\ADMIN\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Nächste, remote Desktop Machine2 und Bearbeiten der listener.ora Datei wie folgt: # listener.ora Netzwerk Konfigurationsdatei: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfigurieren von tnsnames.ora auf die primäre und standby virtuellen Computern, die Einträge für primäre und standby-Datenbanken enthalten soll.

Remotedesktop Machine1 und die Datei tnsnames.ora als unten angegebene bearbeiten. Die Datei tnsnames.ora finden Sie im folgenden Ordner: c:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Netzwerk\\ADMIN\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Remotedesktop Machine2, und bearbeiten Sie die tnsnames.ora Datei wie folgt:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Starten Sie die Zuhörer, und aktivieren Sie Tnsping auf beide virtuellen Computern für beide Dienste.

Öffnen Sie eine neue Windows-Befehlszeile virtuellen-primäre und standby, und führen Sie die folgenden Aussagen:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Starten Sie die standby-Instanz Nomount Zustand
Zum Einrichten der Umgebung zur Unterstützung von standby-Datenbanken auf standby virtuellen Computers (MACHINE2).

Zuerst Kopieren der Kennwortdatei aus dem primären Computer (Machine1) auf den standby Computer (Machine2) manuell. Dies ist erforderlich, da das Kennwort **Sys** auf beiden Computern identisch sein muss.

Klicken Sie dann öffnen Sie die Windows-Befehlszeile in Machine2 und Einrichten der Umgebungsvariablen wie folgt in die Datenbank Standby verweisen:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Als Nächstes Starten der Standby-Datenbank im Nomount Zustand und anschließend eine Spfile generieren.

Starten der Datenbank an:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Verwenden Sie RMAN, um die Datenbank zu klonen und zum Erstellen einer standby-Datenbank
Der Wiederherstellung-Managers (RMAN) können Sie um eine Sicherungskopie der primären Datenbank zum Erstellen der physischen standby-Datenbank zu optimieren.

Remotedesktop zum standby virtuellen Computern (MACHINE2) und der RMAN-Programm ausführen, indem Sie eine Verbindung für beide das Ziel (primäre Datenbank Machine1) Zeichenfolge und ZUSÄTZLICHES (standby-Datenbank, Machine2) Instanzen.

>[AZURE.IMPORTANT] Verwenden Sie nicht die Betriebssystem Authentifizierung wie keine Datenbank noch nicht auf dem Computer standby-Server ist.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Starten Sie die physische standby-Datenbank im Wiederherstellungsmodus für verwaltete
In diesem Lernprogramm wird veranschaulicht, wie eine physische standby-Datenbank zu erstellen. Informationen zum Erstellen einer logischen standby-Datenbank finden Sie unter der Oracle-Dokumentation.

Öffnen von SQL\*Plus Befehl auffordern, und aktivieren Sie die Data Guard auf standby virtuellen Computers oder der Server (MACHINE2) wie folgt:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Beim Öffnen der Datenbank standby im Modus **Bereitstellen** , das Archivierungsprotokoll weiterhin Versand und der verwalteten Wiederherstellung fortgesetzt Log der standby-Datenbank anwenden. Dadurch wird sichergestellt, dass die standby-Datenbank auf dem neuesten Stand mit der primären Datenbank erhalten bleibt. Beachten Sie, dass die standby-Datenbanken für Berichtszwecke dieses Zeitraums barrierefreien werden kann.

Wenn Sie die standby-Datenbank im **Nur-lesen** -Modus öffnen, melden Sie das Archiv Versand weiterhin. Aber den verwalteten Wiederherstellungsvorgang beendet. Dies bewirkt, dass die Datenbank standby zunehmend veralten, bis der Wiederherstellungsprozess verwalteten fortgesetzt wird. Sie können die standby-Datenbanken für Berichtszwecke dieses Zeitraums zugreifen, aber Daten möglicherweise nicht die neuesten Änderungen wider.

Es wird empfohlen, dass Sie die standby-Datenbank im Modus **Bereitstellen** , um die Daten in der Datenbank standby Stand bei ein Ausfall der primären Datenbank beibehalten. Jedoch können Sie standby-Datenbanken im Modus " **Nur lesen** " für Berichtszwecke je nach Ihrer Anwendung Anforderungen belassen. Zeigen Sie die folgenden Schritte aus, wie Aktivieren der Data Guard in den schreibgeschützten Modus mit SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Überprüfen der physischen standby-Datenbank
In diesem Abschnitt wird veranschaulicht, wie die Konfiguration der hohen Verfügbarkeit als Administrator zu überprüfen.

Öffnen von SQL\*Plus Eingabeaufforderungsfenster und Kontrollkästchen archivierte wiederholen Log auf Standby virtuellen Computers (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Öffnen von SQL * Plus Eingabeaufforderungsfenster-Fenster und wechseln Protokolldateien auf dem primären Computer (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Überprüfen Sie archivierte wiederholen Log auf Standby virtuellen Computers (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Prüfen von Lücken auf Standby virtuellen Computers (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Eine andere Überprüfungsmethode konnte werden Failover auf standby-Datenbanken und Testen ist es möglich, Failback mit der primären Datenbank. Verwenden Sie zum Aktivieren der standby Datenbank als primäre Datenbank die folgenden Aussagen aus:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Wenn Sie nicht in der ursprünglichen primären Datenbank Flashback aktiviert haben, empfiehlt es sich, die Sie löschen Sie die ursprüngliche primäre Datenbank und als Ersatz-Datenbanken neu erstellen.

Es empfiehlt sich, dass Sie Flashback auf dem primären und standby-Datenbanken aktivieren. Wenn ein Failover geschieht, kann die primäre Datenbank wieder in die Zeit vor dem Failover blinken und schnell zu einer standby-Datenbank konvertiert.

##<a name="additional-resources"></a>Zusätzliche Ressourcen
[Oracle-virtuellen Computern Bilder für Azure](virtual-machines-windows-classic-oracle-images.md)

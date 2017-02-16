<properties
    pageTitle="Konfigurieren von Oracle GoldenGate in virtuellen Computern | Microsoft Azure"
    description="Durchlaufen Sie ein Lernprogramm für das Einrichten und Implementierung von Oracle GoldenGate auf Azure Windows Server virtuellen Computern für hohe Verfügbarkeit und Disaster Wiederherstellung aus."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Konfigurieren von Oracle GoldenGate für Azure


In diesem Lernprogramm wird veranschaulicht, wie Oracle GoldenGate für Azure-virtuellen Computern-Umgebung für hohe Verfügbarkeit und Wiederherstellung einrichten. Das Lernprogramm liegt der Schwerpunkt auf [bidirektionale Replikation](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) für nicht für RAC Oracle - Datenbanken und setzt voraus, dass die beiden Websites aktiv sein sollen.

Oracle-GoldenGate unterstützt Daten Verteilung und Datenintegration von. Es ermöglicht Ihnen das Einrichten einer Synchronisierung Lösung durch die Konfiguration der Oracle-Oracle-Replikation und Verteilung von Daten und stellt eine flexible hohen Verfügbarkeit Lösung. Oracle GoldenGate ergänzen Oracle Data Guard mit seinen Replikationsfunktionen unternehmensweiten IT-Verteilung zurück und ausfallfreier Upgrades und Migration aktivieren. Ausführliche Informationen finden Sie unter [Oracle-GoldenGate mit Oracle-Daten Check verwenden](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle-GoldenGate enthält die folgenden Hauptkomponenten: extrahieren, Daten Pumpe, Replicat, Spuren oder Kontrollpunkten, Manager und Collection Dateien zu extrahieren. Um bidirektionale Replikation zwischen zwei Websites verwenden zu können, müssen Sie erstellen, und starten alle Komponenten auf beiden Websites. Ausführliche Informationen zu Oracle GoldenGate Architektur finden Sie unter [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

In diesem Lernprogramm wird davon ausgegangen, dass bereits theoretischen und praktischen Knowledge auf Oracle-Datenbank hohen Verfügbarkeit und Wiederherstellung Konzepte als auch für [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Weitere Informationen finden Sie unter der [Oracle-Website](http://www.oracle.com/technetwork/database/features/availability/index.html).

Darüber hinaus wird das Lernprogramm davon ausgegangen, dass Sie die folgenden Vorkenntnisse bereits implementiert haben:

- Sie haben bereits im Abschnitt hohe Verfügbarkeit und Disaster Wiederherstellung Aspekte im Thema [Oracle virtuellen Computern Bilder - verschiedene Aspekte](virtual-machines-windows-classic-oracle-considerations.md) überprüft. Beachten Sie, dass Azure aktuell eigenständigen Oracle-Datenbank Instanzen aber nicht Oracle Real Application Cluster (Oracle RAC) unterstützt.

- Sie haben die GoldenGate Oracle-Software von der [Oracle-Downloads](http://www.oracle.com/us/downloads/index.html) -Website heruntergeladen haben. Sie haben das Produkt Pack Oracle Fusion Middleware – Integration von Daten ausgewählt. Klicken Sie dann haben Sie Oracle GoldenGate auf Oracle v11.2.1 Media Pack für Microsoft Windows X64 (64-Bit) für eine Oracle-Datenbank 11 g ausgewählt. Als Nächstes herunterladen Sie Oracle GoldenGate V11.2.1.0.3 für Oracle 11g 64-Bit unter Windows 2008 (64-Bit).

- Sie haben in Azure Oracle Enterprise Edition unter Windows Server mit zwei virtuellen Computern (virtuellen Computern) erstellt. Stellen Sie sicher, dass die virtuellen Computern sind in der [gleichen Cloud-Dienst](virtual-machines-linux-load-balance.md) und im gleichen [Virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) , um sicherzustellen, dass diese miteinander über die beständigen private IP-Adresse zugegriffen werden können.

- Sie haben die virtuellen Computernamen als "MachineGG1" für eine Website und "MachineGG2" für die Website B in der klassischen Azure-Portal festlegen.

- Sie haben Testdatenbanken "TestGG1" auf der Website A und "TestGG2" auf der Website b erstellt

- Sie melden Sie sich bei Ihrem WindowsServer als Mitglied der Gruppe der Administratoren oder als Mitglied der Gruppe **ORA_DBA** .

In diesem Lernprogramm werden Sie folgende Aufgaben ausführen:

1. Setup-Datenbank auf Standort A und B der Website  

    1. Führen Sie die Ausgangsdaten laden

2. Vorbereiten von Standort A und B der Website für Datenbankreplikation

3. Erstellen Sie alle erforderlichen Objekte zur Unterstützung der Replikation DDL

4. Konfigurieren der GoldenGate-Manager auf Standort A und B

5. Extrahieren Gruppe erstellen und Daten drehen Prozesse auf Standort A und B der Website

    1. Erstellen von extrahieren "und" Drehen Daten Prozesse auf Website A

    2. Erstellen einer GoldenGate Wissensstand Tabelle auf Website B

    3. Hinzufügen von REPLICAT auf Standort B

    4. Erstellen von extrahieren "und" Drehen Daten Prozesse auf Website B

    5. Erstellen einer GoldenGate Wissensstand Tabelle auf A-Website

    6. Fügen Sie auf eine REPLICAT hinzu

    7. Hinzufügen von Trandata auf Standort A und B der Website

    8. Beginnen Sie extrahieren und Datapump Prozesse auf Website A

    9. Starten Sie extrahieren und Datapump Prozesse auf Website B

    10. Starten Sie REPLICAT Prozess auf Standort A

    11. Starten Sie REPLICAT Prozess auf Website B

6. Überprüfen Sie den Prozess bidirektionale Replikation

>[AZURE.IMPORTANT] In diesem Lernprogramm wurde einrichten und mit der folgenden Softwarekonfiguration getestet:
>
>|                        | **Eine Datenbank**              | **Standort B-Datenbank**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle-Version**     | Oracle11g Version 2 – (11.2.0.1) | Oracle11g Version 2 – (11.2.0.1) |
>| **Name des Computers**       | MachineGG1                       | MachineGG2                       |
>| **Betriebssystem**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle SID**         | TESTGG1                          | TESTGG2                          |
>| **Replikation Schema** | STEFAN                            | STEFAN                            |

Für nachfolgende Versionen von Oracle-Datenbank und Oracle GoldenGate möglicherweise einige weiteren Änderungen, die implementiert werden müssen. Die aktuellste Version bestimmte Informationen finden Sie unter [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) und [Oracle-Datenbank](http://www.oracle.com/us/corporate/features/database-12c/index.html) -Dokumentation auf Oracle-Website. Zum Beispiel, bei einer Quelldatenbank Version 11.2.0.4 und höher Erfassung von DDL erfolgt vom Server Logmining asynchrone und erfordert keine spezielle Trigger, Tabellen oder andere Datenbankobjekte installiert sein. Oracle GoldenGate Upgrades können ohne Beenden von Applications Benutzer durchgeführt werden. Die Verwendung von DDL Trigger und unterstützen von Objekten ist erforderlich, wenn extrahiert werden im integrierten Modus mit einer Oracle 11 g Quelldatenbank ist, die älter als Version 11.2.0.4 ist. Eine umfassende Unterstützung für finden Sie unter [Installieren und Konfigurieren von Oracle GoldenGate für Oracle-Datenbank](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. setup-Datenbank auf Standort A und B der Website
In diesem Abschnitt wird erläutert, wie die Datenbank erforderlichen Komponenten auf Standort A und b Website ausführen Führen Sie die Schritte in diesem Abschnitt, auf beiden Websites: Standort A und b der Website

Erste, remote Desktop, Standort A und B Website über das klassische Azure-Portal. Öffnen Sie ein Eingabeaufforderungsfenster Windows und erstellen Sie ein Start Verzeichnis für Oracle GoldenGate Setup-Dateien:

    mkdir C:\OracleGG

Klicken Sie dann Entzippen Sie ihn, und installieren Sie die Oracle-GoldenGate Software in diesem Ordner. Nach diesem Schritt können Sie die GoldenGate Software Befehl Interpreter (GGSCI) initiieren, indem Sie den folgenden Befehl ausführen:

    C:\OracleGG\.\ggsci

Sie können die [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) mehrere Befehle ausführen, die konfigurieren, Steuerung und Monitor Oracle GoldenGate verwenden.

Führen Sie dann den folgenden Befehl aus, um alle Unterordner aus dem Installationspaket zu erstellen:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Führen Sie zum Beenden der GGSCI Eingabeaufforderung den folgenden Befehl ein:

    GGSCI (Hostname) 1> EXIT

Klicken Sie dann müssen Sie einen Datenbankbenutzer erstellen, die durch die Prozesse Oracle GoldenGate-Manager, extrahieren und Replikation verwendet wird. Beachten Sie, dass Sie für jeden Prozess einzelner Benutzer erstellen oder nur eine allgemeinen Benutzer konfigurieren können. In diesem Lernprogramm erstellen wir eine Benutzer, die als Ggate bezeichnet wird. Klicken Sie dann erteilen wir dem Benutzer die erforderlichen Berechtigungen. Beachten Sie, dass Sie die folgenden Vorgänge auf Standort A und b der Website ausführen müssen

Öffnen von SQL\*Plus Befehlsfenster auf Standort A und B der Website mit der Datenbank Administratorrechte mit **SYSDBA**, wie:

    Enter username: / as sysdba

Und ausführen:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Suchen Sie als Nächstes die Initialisierung\<DatabaseSID\>. In den Feldern % ORACLE ORA-Datei\_Start %\\Ordner auf Standort A und B Website-Datenbank, und fügen Sie die folgenden Datenbankparameter INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Eine vollständige Liste aller Befehle für Oracle GoldenGate GGSCI finden Sie unter [Referenz für Oracle GoldenGate für Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Führen Sie die Ausgangsdaten laden

Sie können die Ausgangsdaten Laden in der Datenbank anhand verschiedener Methoden ausführen. Beispielsweise können Sie die [Oracle GoldenGate direkte Laden](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) oder reguläre exportieren und importieren Dienstprogramme verwenden so exportieren Sie Tabellendaten von Standort A nach Standort b

Um die Oracle-GoldenGate Replikationsvorgang zu veranschaulichen, wird in diesem Lernprogramm Erstellen einer Tabelle auf sowohl Standort A und B mithilfe der folgenden Befehle veranschaulicht.

Öffnen Sie zuerst von SQL\*Plus Befehl Fenster und zum Erstellen einer Tabelle Inventory auf Website A und B Website Datenbanken den folgenden Befehl ausführen:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Fügen Sie eine Einschränkung der neu erstellten Tabelle auf Standort A und B Website Datenbanken:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Klicken Sie dann gewähren Sie alle Berechtigungen für die neue Tabelle Inventory der Benutzer Ggate Standort A und Standort B:

    grant all on scott.inventory to ggate;

Als Nächstes erstellen Sie aus, und aktivieren Sie einen Datenbanktrigger INVENTORY_CDR_TRG, klicken Sie auf der neu erstellten Tabelle aus, um sicherzustellen, dass alle Transaktionen in die neue Tabelle aufgezeichnet werden, wenn der Benutzer kein Ggate ist. Führen Sie diesen Vorgang auf Standort A und b der Website

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. bereiten Sie Standort A und B der Website für Datenbankreplikation vor
In diesem Abschnitt wird erläutert, wie Standort A und B Website Datenbankreplikation vorzubereiten. Führen Sie die Schritte in diesem Abschnitt, auf beiden Websites: Standort A und b der Website

Erste, remote Desktop, Standort A und B Website über das klassische Azure-Portal. Wechseln die Datenbank in Archivelog-Modus verwenden den SQL-Code * Plus Befehlsfenster:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Aktivieren Sie dann die minimale [zusätzliche Protokollierung](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) wie folgt:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Als Nächstes Vorbereiten der Datenbank zur Unterstützung der Replikation DDL (Data Definition Language):

    SQL> alter system set recyclebin=off scope=spfile;

Dann, beenden und Starten der Datenbank:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3 Erstellen Sie 3 alle erforderlichen Objekte zur Unterstützung der Replikation DDL
Dieser Abschnitt listet die Skripts, die Sie verwenden, um alle erforderlichen Objekte zur Unterstützung der Replikation DDL erstellen müssen. Sie müssen die in diesem Abschnitt auf Standort A und b Website angegebenen Skripts ausgeführt werden

Öffnen Sie ein Windows-Eingabeaufforderungsfenster, und navigieren Sie zu der GoldenGate Oracle-Ordner, beispielsweise C:\\OracleGG. Starten Sie SQL\*Plus Eingabeaufforderungsfenster mit Datenbankadministratorrechten, z. B. **SYSDBA** auf Standort A und b Website verwenden

Führen Sie dann die folgenden Skripts aus:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle GoldenGate Tool erfordert eine Tabelle Ebene Anmeldung für DDL (Data Definition Language)-Unterstützung. Das war, warum zusätzliche Ebene der Tabelle mithilfe des Befehls hinzufügen TRANDATA Protokollierung aktivieren. Öffnen von Oracle GoldenGate Befehl Interpreter Fenster, melden Sie sich die Datenbank, und führen Sie den Befehl TRANDATA hinzufügen:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4 Konfigurieren von GoldenGate-Manager auf Standort A und B
Oracle-GoldenGate-Manager führt eine Reihe von Funktionen, wie die anderen GoldenGate Prozesse starten Spur Protokolldatei-Management und Berichte.

Sie müssen den Prozess Oracle GoldenGate-Manager auf Standort A und b Website konfigurieren Führen Sie hierzu die folgenden Schritte auf Standort A und b der Website

Geöffnete Fenster Befehl Fenster und den Oracle-GoldenGate Befehlsinterpreter initiieren:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Meldet die Sitzung GGSCI in einer Datenbank, damit Sie Befehle ausführen können, die die Datenbank beeinflussen:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Anzeigen des Status sowie positiven (sofern relevant) für alle Manager, extrahieren und Replicat Prozesse auf einem System:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Anzeigen des Status sowie positiven (sofern relevant) für alle Manager, extrahieren und Replicat Prozesse auf einem System:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Meldet die Sitzung GGSCI in einer Datenbank, damit Sie Befehle ausführen können, die die Datenbank beeinflussen:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Starten Sie den Managerprozess:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. erstellen extrahieren gruppieren und Drehen von Daten Prozesse auf Standort A und B der Website

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Erstellen von extrahieren "und" Drehen Daten Prozesse auf Website A

Sie müssen die Prozessen extrahieren und Daten zu drehen oder auf eine Website b Remote Desktop Standort A und B Website über das klassische Azure-Portal zu erstellen. GGSCI Befehl Interpreter Fenster öffnen. Führen Sie die folgenden Befehle auf der Website A:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen: GGSCI (MachineGG1) 18 > Bearbeiten Parameter ext1 EXTRAHIEREN ext1 Benutzer-ID Ggate Kennwort Ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER Ggate Tabelle scott.inventory, GETBEFORECOLS (auf UPDATE KEYINCLUDING (Prod_category, Qty_in_stock, Last_dml), auf Löschen KEYINCLUDING (Prod_category, Qty_in_stock, Last_dml))

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Erstellen einer GoldenGate Wissensstand Tabelle auf Website B

Als Nächstes müssen Sie eine Tabelle Wissensstand auf Website b hinzufügen Dazu müssen Sie ein GoldenGate Befehl Interpreter-Fenster zu öffnen und ausführen:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Und fügen Sie die Tabelle Wissensstand in der Datenbank, die Stelle, an der der Besitzer von Ggate ist:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Fügen Sie den Namen der Tabelle Kontrollkästchen Punkt in die Globaldatei auf dem Zielserver, also Standort B in diesem Schritt hinzu. Bearbeiten Sie die Datei Global Website B:

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Klicken Sie dann die vorhandene Globaldatei den Parameter CHECKPOINTTABLE anfügen:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Klicken Sie im letzten Schritt speichern Sie und schließen Sie den Parameter Globaldatei.


###<a name="add-replicat-on-site-b"></a>Hinzufügen von REPLICAT auf Standort B
In diesem Abschnitt beschrieben, wie Sie einen REPLICAT Prozess "REP2" auf der Website b hinzufügen

Verwenden Sie hinzufügen REPLICAT Befehl zum Erstellen einer Gruppe Replicat auf Website B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Erstellen von extrahieren "und" Drehen Daten Prozesse auf Website B

In diesem Abschnitt beschrieben, wie Sie eine neue extrahieren "EXT2" und eine neue Daten Pumpe Prozess "DPUMP2" auf der Website B: Erstellen

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Erstellen einer GoldenGate Wissensstand Tabelle auf A-Website

Oracle-GoldenGate Befehl Interpreter-Fenster zu öffnen Sie, und erstellen Sie eine Tabelle Wissensstand:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Sie müssen außerdem den Namen der Tabelle Punkt Überprüfen der Globaldatei auf Website a hinzufügen

Oracle-GoldenGate Befehl Interpreter-Fenster zu öffnen Sie und bearbeiten Sie die Datei Global Website A:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Speichern Sie und schließen Sie den Parameter Globaldatei.

###<a name="add-replicat-on-site-a"></a>Fügen Sie auf eine REPLICAT hinzu

In diesem Abschnitt beschrieben, wie Sie einen REPLICAT Prozess "REP1" auf a-Website hinzufügen

Mit dem folgende Befehl wird eine Replicat Gruppe rep1 mit dem Namen der eine Spur und die zugehörigen Checkpointtable erstellt.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Öffnen Sie die Parameterdatei, die mit dem Befehl Parameter bearbeiten, und fügen Sie dann die folgende Informationen:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Hinzufügen von Trandata auf Standort A und B der Website

Aktivieren Sie ergänzende Protokollierung Ebene der Tabelle mithilfe des Befehls TRANDATA hinzufügen aus. Öffnen von Oracle GoldenGate Befehl Interpreter Fenster, melden Sie sich die Datenbank, und führen Sie den Befehl hinzufügen TRANDATA.

Remotedesktop zu MachineGG1, Oracle GoldenGate Befehlsinterpreter zu öffnen, und führen Sie:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Remotedesktop zu MachineGG2, Oracle GoldenGate Befehlsinterpreter zu öffnen, und führen Sie:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Von Anzeigeinformationen über den Status der Tabelle Ebene zusätzliche Protokollierung:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Hinzufügen von Trandata auf Standort A und B der Website

Aktivieren Sie ergänzende Protokollierung Ebene der Tabelle mithilfe des Befehls TRANDATA hinzufügen. Öffnen von Oracle GoldenGate Befehl Interpreter Fenster, melden Sie sich die Datenbank, und führen Sie den Befehl TRANDATA hinzufügen.

Remotedesktop zu MachineGG1, Oracle GoldenGate Befehlsinterpreter zu öffnen, und führen Sie:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Remotedesktop zu MachineGG2, Oracle GoldenGate Befehlsinterpreter zu öffnen, und führen Sie:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Beginnen Sie extrahieren und Datapump Prozesse auf Website A

Starten Sie den Prozess-ext1 extrahiert werden auf Website A:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Starten Sie die Daten Pumpe Prozess dpump1 auf Website A:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Anzeigen von Informationen zu den extrahieren Gruppe ext1: GGSCI (MachineGG1) 32 > Info 2013-11-25 extrahieren ext1 ist EXT1 EXTRAHIEREN letzten Schritte 08:03 Status ausgeführt Wissensstand positiven 00:00:00 (aktualisierte 00:00:02 vor) Log gelesen Wissensstand Oracle wiederholen Protokolle 2013-11-25 08:03:18 Sequenz 6, RBA 3230720 SCN 0.1074371 (1074371)

Anzeigen des Status sowie positiven (sofern relevant) für alle Manager, extrahieren und Replicat Prozesse auf einem System:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Starten Sie extrahieren und Datapump Prozesse auf Website B

Starten Sie das Extrahieren Prozess ext2 auf Website B:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Starten Sie die Daten Pumpe Prozess dpump2 auf Website B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Anzeigen des Status sowie positiven (sofern relevant) für alle Manager, extrahieren und Replicat Prozesse auf einem System:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Starten Sie REPLICAT Prozess auf Standort A

In diesem Abschnitt beschrieben, wie Sie mit dem REPLICAT "REP1" auf Website a beginnen

Starten Sie den Prozess Replicat auf Website A:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Anzeigen des Status einer Gruppe Replicat an:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Starten Sie REPLICAT Prozess auf Website B

In diesem Abschnitt beschrieben, wie Sie mit dem REPLICAT "REP2" auf der Website b beginnen

Starten Sie den Prozess Replicat auf Website B:

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Anzeigen des Status einer Gruppe Replicat an:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Vergewissern Sie sich den bidirektionale Replikationsprozess

Um zu überprüfen, die Oracle-GoldenGate Konfiguration, fügen Sie eine Zeile in der Datenbank am Standort a Remotedesktop auf Website a Öffnen von SQL * Plus Befehlsfenster und ausführen: SQL > select Name V$ Datenbank;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Aktivieren Sie dann, wenn die Zeile auf Standort b repliziert wird Hierzu Remotedesktop auf Website b öffnen SQL Plus ein Fenster nach oben und ausführen:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Einfügen eines neuen Datensatzes am Standort B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Remotedesktop Standort A und überprüfen Sie, ob die Replikation durchgeführt wurde:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Zusätzliche Ressourcen
[Oracle-virtuellen Computern Bilder für Azure](virtual-machines-linux-classic-oracle-images.md)

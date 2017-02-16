<properties
    pageTitle="Optimierung der MySQL-Performance auf Linux virtuellen Computern | Microsoft Azure"
    description="Erfahren Sie, wie Sie MySQL ausgeführt wird, klicken Sie auf eine Azure-virtuellen Computern (virtueller Computer) mit Linux optimieren."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimieren der Leistung MySQL Linux Azure-virtuellen Computern

Es gibt viele Faktoren die beeinflussen, MySQL-Leistung von Azure, beide in virtuelle Hardwarekonfiguration Auswahl und Software aus. Dieser Artikel befasst sich Optimieren der Leistung durch Speicher, System und Datenbankkonfigurationen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Die Nutzung RAID-On eine Azure-virtuellen Computern
Speicher ist der wichtigsten Faktor, die Leistung der Datenbank in der Cloud-Umgebungen wirkt sich auf.  Im Vergleich zu einem einzelnen Datenträger, kann RAID schnelleren Zugriff über Parallelität bereitstellen.  Weitere Details finden Sie in der [Standardansicht Raidlevels](http://en.wikipedia.org/wiki/Standard_RAID_levels) .   

Datenträgerdurchsatz e/a und e/a-Antwort in Azure können durch RAID erheblich verbessert werden. Unsere Tests Übung anzeigen Datenträger e/a Durchsatz verdoppelt werden kann und e/a-Reaktionszeiten reduziert werden mit der Hälfte Durchschnitt, wenn die Anzahl der RAID-Datenträger verdoppelt wurde (von 2 bis 4, 4 bis 8 usw.). Details finden Sie unter [Anhang A](#AppendixA) .  

Verbessert die MySQL-Leistung sowie Datenträger e/a Wenn Sie die Ebene RAID erhöhen.  Details finden Sie unter [Anhang B](#AppendixB) .  

Sie möchten möglicherweise auch die Abschnittsgröße zu berücksichtigen. Im Allgemeinen bei einer größeren Ausschnitt erhalten unteren Aufwand, vor allem für große schreibt Sie. Jedoch, wenn der Abschnitt zu groß ist, wird es möglicherweise zusätzlichen Aufwand hinzufügen und nutzen kann nicht von der RAID. Die aktuelle Standardgröße ist 512KB, erwiesen ist optimal für die meisten allgemeinen Herstellung Umgebungen. Details finden Sie unter [Anhang C](#AppendixC) .   

Bitte beachten Sie, dass es Grenzwerte auf wie viele Datenträger, die Sie für andere virtuellen Computertypen hinzufügen können. Diese Grenzwerte werden in [virtuellen Computern und Cloud-Dienst Größen für Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)detailliert beschrieben. Sie benötigen 4 angefügten Datenfestplatten RAID Beispiel in diesem Artikel folgen, obwohl Sie konnte RAID mit wenige Datenträger einrichten festlegen.  

In diesem Artikel wird vorausgesetzt, Sie bereits eine Linux virtuellen Computern erstellt haben und MYSQL installiert und konfiguriert haben. Weitere Informationen zu den ersten Schritten Näheres zum MySQL auf Azure zu installieren.  

###<a name="setting-up-raid-on-azure"></a>Einrichten von RAID auf Azure
Die folgenden Schritte zeigen, wie RAID-On Azure erstellen über das klassische Azure-Portal. Sie können auch RAID einrichten, mithilfe von Windows PowerShell-Skripts.
In diesem Beispiel wird mit 4 Festplatten RAID 0 konfiguriert.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Schritt 1: Fügen Sie einen Datenträger zu Ihrem virtuellen Computern hinzu  

Klicken Sie im klassischen Azure-Portal auf der Seite virtuellen Computern des virtuellen Computers, dem Sie einen Datenträger hinzufügen möchten. In diesem Beispiel wird die virtuellen Computern mysqlnode1.  

![][1]

Klicken Sie auf der Seite für den virtuellen Computer auf **Dashboard**.  

![][2]


Klicken Sie in der Taskleiste auf **Anfügen**.

![][3]

Ein, und klicken Sie dann auf **leeren Datenträger anfügen**.  

![][4]

Für Daten Datenträger sollte der **Host Cache Voreinstellung** **keine**festgelegt werden.  

Dadurch wird eine leere Datenträger in Ihrem virtuellen Computern hinzugefügt. Wiederholen Sie diesen Schritt drei Mal, damit Sie für RAID 4 Daten Datenträger haben.  

Sie können die hinzugefügten Laufwerke des virtuellen Computers anzeigen, indem Sie die Nachricht Kernelprotokoll betrachtet. Damit dies Ubuntu angezeigt wird, verwenden Sie beispielsweise den folgenden Befehl aus:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Schritt 2: Erstellen von RAID mit den zusätzlichen Laufwerken
Führen Sie in diesem Artikel ausführliche RAID Setup Schritte aus:  

[Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Wenn Sie das Dateisystem XFS verwenden, führen Sie die folgenden Schritte aus, nachdem Sie RAID erstellt haben.

Um XFS in Debian, Ubuntu oder Linux MinZ installieren möchten, verwenden Sie den folgenden Befehl aus:  

    apt-get -y install xfsprogs  

Um XFS auf Fedora, CentOS oder RHEL installieren möchten, verwenden Sie den folgenden Befehl aus:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Schritt 3: Einrichten eines neuen Speicher-Pfads
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Schritt 4: Kopieren Sie die ursprünglichen Daten auf den Speicherpfad der neuen
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Schritt 5: Ändern von Berechtigungen, damit MySQL zugreifen kann (Lesen und Schreiben) der Datenträger Daten
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Anpassen des Datenträger e/a-Planung Algorithmus
Linux implementiert vier Arten von e/a Planung Algorithmen:  

-   NOOP-Algorithmus (keine Operation)
-   Stichtag Algorithmus (Endtermin)
-   Vollständig ' wissenschaftsmesse ' Warteschlange Algorithmus (CFQ)
-   Budget Periode Algorithmus (Anticipatory)  

Sie können verschiedene e/a-Planer unter verschiedenen Szenarios zur Optimierung der Performance auswählen. In einer Umgebung vollständig Direktzugriff ist ein großer Unterschied zwischen der CFQ und Stichtag Algorithmen für die Leistung nicht. Es wird empfohlen, die MySQL-Datenbank-Umgebung auf Stichtags für Stabilität festzulegen. Wenn es zahlreiche sequenzielle i/o gibt, möglicherweise CFQ Datenträger e/a-Leistung verringern.   

Für SSD und anderen Geräte kann mithilfe von NOOP oder Stichtag eine bessere Leistung als den standardmäßigen Scheduler erzielen.   

Aus dem Kernel 2,5 ist der standardmäßige e/a-Planung Algorithmus Stichtag. CFQ war aus dem Kernel 2.6.18 Anfang, der standardmäßige e/a-Planung Algorithmus anzuwenden.  Sie können diese Einstellung bei Boot Kernelzeit angeben oder diese Einstellung dynamisch zu ändern, wenn das System ausgeführt wird.  

Im folgenden Beispiel wird veranschaulicht, wie Sie überprüfen, und legen Sie den standardmäßigen Scheduler auf den Algorithmus NOOP.  

Für die Familie Debian Verteilung:

###<a name="step-1view-the-current-io-scheduler"></a>Schritt 1 Ansicht der aktuellen e/a scheduler
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Sie sehen nach Ausgabe, die festlegt, mit den aktuellen Planer.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Schritt 2. Ändern Sie das aktuelle Gerät (/ Entwicklung/Sda) e/a-Algorithmus Planung
Verwenden Sie die folgenden Befehle:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Festlegen von dies für/dev/sda allein ist nicht hilfreich. Wo befindet sich die Datenbank auf alle Daten Datenträger festgelegt werden muss.  

Die folgende Ausgabe, anzeigt, dass die grub.cfg erfolgreich neu erstellt wurde und dass die standardmäßigen Scheduler zu NOOP aktualisiert wurde, sollte angezeigt werden.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Für die Verteilung Familie Red hat benötigen Sie nur den folgenden Befehl aus:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Konfigurieren von Systemeinstellungen Datei-Vorgänge
Eine bewährte Methode besteht darin, das Protokollierungsfeature Atime Dateisystem zu deaktivieren. Atime ist die Uhrzeit der letzten Datei zugreifen. Wenn eine Datei zugegriffen wird, Einträge Dateisystem den Zeitstempel im Protokoll. Diese Informationen wird jedoch kaum verwendet. Sie können es deaktivieren, wenn Sie es benötigen, und Datenträger Access Gesamtzeit wird.  

Um Atime Protokollierung deaktivieren möchten, müssen Sie die Datei System Konfiguration Datei sich Fstab ändern und die Option **Noatime** hinzufügen.  

Bearbeiten Sie beispielsweise die Vim/etc/fstab-Datei, die Noatime wie folgt hinzufügen.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Klicken Sie dann neuen Bereitstellung das Dateisystem mit dem folgenden Befehl aus:  

    mount -o remount /RAID0

Testen Sie das geänderte Ergebnis. Beachten Sie, dass, wenn Sie die Testdatei ändern, die Access-Zeit nicht aktualisiert wird.  

Bevor Sie das Beispiel:     

![][5]

Nach dem Beispiel:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Erhöhen Sie die maximale Anzahl von System Ziehpunkte für hoher Parallelität
MySQL ist hoher Parallelität Datenbank. Die Standardanzahl von gleichzeitigen Ziehpunkte ist 1024 für Linux, welche reicht nicht immer aus. **Verwenden Sie die folgenden Schritte aus, klicken Sie zum Erhöhen der maximalen gleichzeitigen Ziehpunkte des Systems zur Unterstützung von hoher gleichzeitigen MySQL**.

###<a name="step-1-modify-the-limitsconf-file"></a>Schritt 1: Ändern Sie die Datei limits.conf
Fügen Sie die folgenden vier Zeilen /etc/security/limits.conf Datei zum Erhöhen der maximal zulässigen gleichzeitigen Ziehpunkte ein. Beachten Sie, dass 65536 ist die maximalen Zahl, die das System unterstützen kann.   

    * Weiche Nofile 65536
    * harte Nofile 65536
    * Weiche Nproc 65536
    * harte Nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Schritt 2: Aktualisieren Sie das System für die neuen Grenzwerte
Führen Sie die folgenden Befehle:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Schritt 3: Stellen Sie sicher, dass die Grenzwerte beim Start aktualisiert werden
Setzen Sie die folgenden Startbefehle in der Datei /etc/rc.local, sodass sie bei jedem Start wirksam wird.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optimierung der MySQL-Datenbank
Dieselbe Leistung Videogeräten Strategie können Sie um MySQL auf Azure als auf einem lokalen Computer zu konfigurieren.  

Die wesentlichen e/a-Optimierungsregeln sind:   

-   Die Cachegröße zu erhöhen.
-   E/a-Reaktionszeiten reduziert.  

Um MySQL-Server-Einstellungen zu optimieren, können Sie die Datei my.cnf die standardmäßige Konfigurationsdatei für Server und Clientcomputer ist aktualisieren.  

Die folgenden Elemente der Konfiguration sind die wichtigsten Faktoren, die MySQL-Leistung beeinflussen:  

-   **Innodb_buffer_pool_size**: der Puffer Ressourcenpool enthält zwischengespeicherten Daten und den Index. Dies wird in der Regel auf 70 % des physischen Speichers festgelegt.
-   **Innodb_log_file_size**: Dies ist die Größe des wiederholen. Sie verwenden wiederholen Protokolle, um sicherzustellen, dass die Vorgänge nach einem Absturz schnell, zuverlässig und wiederhergestellt werden. Dies wird auf 512 MB festgelegt, die Ihnen ausreichend Platz vermitteln für die Protokollierung Schreibvorgänge.
-   **Max_connections**: manchmal Applikationen schließen nicht Verbindungen ordnungsgemäß. Ein größer Wert kann dem Server mehr Zeit, um den Leerlauf überging Verbindungen Papierkorb erzielt werden. Die maximalen Verbindungen ist 10.000, aber die empfohlene maximale ist 5000.
-   **Innodb_file_per_table**: Diese Einstellung aktivieren oder deaktivieren Sie die Möglichkeit des InnoDB Tabellen in separaten Dateien gespeichert. Aktivieren der Option wird sichergestellt, dass mehrere erweiterte Administration Vorgänge effizient angewendet werden können. Leistung der Sicht kann es Tabelle Leerzeichen Übertragung beschleunigen und optimieren die Leistung verschmutzt Management. Somit ist die empfohlene Einstellung für diesen auf.</br>
    Aus MySQL 5.6 ist die Standardeinstellung. Daher ist keine Maßnahme erforderlich. Informationen zu anderen Versionen, die älter als 5.6 ist, Standardeinstellungen ist deaktiviert. Diese eingeschaltet ist erforderlich. Und sollte anwenden, bevor die Daten geladen werden, weil nur neu erstellte Tabellen betroffen sind.
-   **Innodb_flush_log_at_trx_commit**: Standardwert ist 1, mit dem Bereich auf 0 festgelegt ~ 2. Der Standardwert ist die am besten geeignete Option für eigenständigen MySQL DB. Die Einstellung 2 ermöglicht die meisten Datenintegrität und eignet sich für Master-Shape in MySQL-Cluster. Die Einstellung von 0 kann Daten verloren gehen, die kann Einfluss auf die Zuverlässigkeit, in einigen Fällen für eine bessere Leistung und eignet sich für untergeordnete in MySQL-Cluster.
-   **Innodb_log_buffer_size**: der Puffer für das Protokoll kann Transaktionen ausgeführt werden, ohne das Protokoll nicht auf einem Datenträger löschen, bevor die Transaktionen abzuschließen. Wenn jedoch großes binäres Objekt oder Text-Feld vorhanden ist, der Cache behandelt werden sehr schnell und häufig verwendeten Datenträger e/a ausgelöst werden soll. Es ist besser Puffer vergrößern Innodb_log_waits Zustand Variable ist nicht 0.
-   **Query_cache_size**: die beste Möglichkeit besteht darin, ihn von Anfang an zu deaktivieren. Legen Sie Query_cache_size 0 (Dies ist nun die Standardeinstellung in MySQL 5.6), und verwenden Sie andere Methoden, um Abfragen zu beschleunigen.  

Vergleich der Leistung nach der Optimierung finden Sie in [Anhang D](#AppendixD) .


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Aktivieren der MySQL langsam Abfrage Log zum Analysieren des Leistungsengpass
Der MySQL langsam Abfrage Log können Sie langsamen Abfragen für MySQL leichter identifizieren. MySQL-Tools, wie **Mysqldumpslow** können Sie nach dem Aktivieren der MySQL-Log langsam Abfrage an, um den Leistungsengpass zu identifizieren.  

Bitte beachten Sie, dass standardmäßig diese Option nicht aktiviert ist. Einschalten der Log langsam Abfrage möglicherweise einige CPU-Ressourcen beanspruchen. Daher, empfiehlt es sich, dass Sie dies vorübergehend zur Behandlung dieses Problems Leistungsengpässe aktivieren.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Schritt 1: Ändern Sie my.cnf Datei, indem Sie die folgenden Zeilen am Ende hinzufügen   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Schritt 2: Restart Mysql-server
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Schritt 3: Überprüfen Sie, ob die Einstellung mit dem Anzeigebefehl "" Effekt ausführt

![][7]   

![][8]

In diesem Beispiel können Sie sehen, dass das langsame Abfrage-Feature aktiviert wurde. Dann können das Tool **Mysqldumpslow** die Leistungsengpässe ermitteln und Optimieren der Leistung, wie etwa das Hinzufügen von Indizes.





##<a name="appendix"></a>Anlage

Im folgenden sind die Leistung Testdaten auf den Übung Umgebung gefertigt, sie jedoch die Ergebnisse unter unterschiedlichen Versionen von Umgebung oder Produkt variieren können allgemeinen Hintergrund auf der Leistung Daten Trend mit anderen Vorgehensweisen, optimieren Leistung bieten.

<a name="AppendixA"></a>Anhang A:  
**Datenträger Leistung (IOPS) mit anderen Raidlevels**


![][9]

**Test-Befehle:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Hinweis: Die Arbeitsbelastung dieser Prüfung verwendet 64, bei dem Versuch, die Obergrenze des RAID erreicht haben.

<a name="AppendixB"></a>Anhang B:  
**MySQL Leistung (Durchsatz) im Vergleich mit anderen Raidlevels**   
(XFS Dateisystem)


![][10]  
![][11]

**Test-Befehle:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL Leistung (OLTP) im Vergleich mit anderen Raidlevels**  
![][12]

**Test-Befehle:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Anhang C:   
**Vergleich von Datenträger Leistung (IOPS) bei anderen Abschnitt**  
(XFS Dateisystem)


![][13]

**Test-Befehle:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Notieren Sie sich die Dateigröße für diese Tests verwendete 30 und 1 GB ist, mit RAID 0(4 disks) XFS Speicherabbilddatei System.


<a name="AppendixD"></a>Anhang D:  
**Vergleich der Leistung (Durchsatz) MySQL vor und nach der Optimierung**  
(XFS File System)


![][14]

**Test-Befehle:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Die Einstellung Konfiguration für Standard- und Optimierung lautet wie folgt aus:**

|Parameter |Standard    |Optimierung
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Keine   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |26.    |0


Mehr detaillierte Optimierung Konfigurationsparameter, finden Sie in den offiziellen Mysql-Anweisungen.  

[http://dev.MySQL.com/doc/refman/5.6/en/InnoDB-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/InnoDB-Parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testumgebung**  

|Hardware   |Details
|-----------|-------
|CPU    |AMD Opteron(tm) Prozessor 4171 HE / 4 Kernen
|Arbeitsspeicher |14G
|Datenträger   |10G/Datenträger
|OS |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

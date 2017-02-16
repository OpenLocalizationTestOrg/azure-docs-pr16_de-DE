

## <a name="memory-preserving-updates"></a>Updates Arbeitsspeicher beibehalten

Für eine Klasse Aktualisierungen in Microsoft Azure werden Kunden Auswirkung auf ihre ausgeführten virtuellen Computern nicht angezeigt. Viele dieser Updates sind und Komponenten oder Dienste, die aktualisiert werden können, ohne dass die ausgeführte Instanz stören. Einige dieser Updates sind Plattform Infrastruktur Updates auf dem Host-Betriebssystem, das angewendet werden können, ohne vollständigen Neustart der virtuellen Computer.

Diese Updates werden mit Technologie durchgeführt, die in-situ-live-Migration, eine Aktualisierung von "Arbeitsspeicher archivieren" die Abkürzung ermöglicht. Bei der Aktualisierung wird des virtuellen Computers in "angehalten" platziert Archivieren von Speicher in RAM, während der zugrunde liegenden des Hostbetriebssystems die erforderlichen Updates und Patches empfängt. Des virtuellen Computers wird innerhalb von 30 Sekunden der Unterbrechung fortgesetzt wird. Nach dem fortsetzen, wird die Uhr des virtuellen Computers automatisch synchronisiert.

Mithilfe dieses Verfahren können nicht alle Updates bereitgestellt werden, aber die angegebene Periode kurzen Pause, Bereitstellen von Updates in dieser Weise erheblich reduziert Einfluss auf virtuellen Computern.

Mit mehreren Instanzen Updates (für virtuelle Maschinen in einem Satz Verfügbarkeit) sind angewendete ein Update Domain nacheinander.  

## <a name="virtual-machine-configurations"></a>Konfigurationen virtuellen Computern

Es gibt zwei Arten von virtuellen Computerkonfigurationen: mit mehreren Instanzen und einer Instanz. Ähnlicher virtueller Computer befinden sich in einer Konfiguration mit mehreren Instanzen in einem Satz Verfügbarkeit.

Die Konfiguration mit mehreren Instanzen bietet Redundanz über physische Computer, Power und Netzwerk, und es wird empfohlen, um sicherzustellen, dass die Verfügbarkeit der Anwendung. Alle virtuellen Computern Verfügbarkeit festlegen sollte den gleichen Zweck an Ihrer Anwendung dienen.

Weitere Informationen zum Konfigurieren Ihrer virtuellen Computern für hohe Verfügbarkeit zum [Verwalten der Verfügbarkeit von Ihrem Windows-virtuellen Computern](../articles/virtual-machines/virtual-machines-windows-manage-availability.md) oder [Verwalten Sie die Verfügbarkeit von Ihren Linux virtuellen Computern](../articles/virtual-machines/virtual-machines-linux-manage-availability.md)verweisen.

Hingegen wird eine einzelne Instanz Konfiguration für eigenständigen virtuellen Computern verwendet, die nicht in einem Satz Verfügbarkeit gebracht werden. Diesen virtuellen Computern berechtigen nicht für Servicelevel (Vereinbarung zum SERVICELEVEL), die erforderlich ist, dass zwei oder mehr virtuellen Computern unter demselben Satz Verfügbarkeit bereitgestellt werden.

Weitere Informationen zu SLAs finden Sie im Abschnitt "Cloud Services, virtuellen Computern und virtuellen Netzwerk" [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).


## <a name="multi-instance-configuration-updates"></a>Mit mehreren Instanzen Konfiguration updates

Während der geplanten Wartung aktualisiert die Azure-Plattform zuerst den Satz von virtuellen Computern, die in einer Konfiguration mit mehreren Instanzen gehostet werden. Dadurch wird einen Neustart zu diesen virtuellen Computern mit Ausfall etwa 15 Minuten.

In einer Aktualisierung mit mehreren Instanzen der Konfiguration werden virtuellen Computern in einer Weise aktualisiert, die behält Availability über den gesamten Prozess, unter der Voraussetzung, dass jeder virtuelle Computer eine ähnliche Funktion wie die anderen in der Gruppe fungiert.

Jeder virtuelle Computer in einer Gruppe Verfügbarkeit ist eine Domäne aktualisieren und eine Fehlerstrukturanalyse-Domäne der zugrunde liegenden Azure-Plattform zugewiesen. Jede Update-Domäne ist eine Gruppe von virtuellen Computern, die in der gleichen Zeitfensters neu gestartet wird. Jede Fehlerstrukturanalyse-Domäne ist eine Gruppe von virtuellen Computern, die eine allgemeine Power Quell- und Netzwerk wechseln freigeben.

Weitere Informationen zum Aktualisieren und Fehlerstrukturanalyse-Domänen finden Sie unter [Festlegen von mehreren virtuellen Computern in einer Verfügbarkeit Redundanzgründen konfigurieren](../articles/virtual-machines/virtual-machines-windows-manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).

Um zu verhindern, dass Domänen aktualisieren Wechsel in den Offlinemodus zur gleichen Zeit, erfolgt die Wartung durch Sie jedem virtuellen Computer in einer Domäne aktualisieren beenden, das Update auf den Hostcomputern anwenden, einen Neustart von den virtuellen Computern und klicken Sie auf der nächsten Aktualisierung Domäne verschieben. Der geplanten Wartung Ereignis endet, nachdem alle Domänen aktualisieren wurden aktualisiert.

Die Reihenfolge der Update-Domänen, die neu gestartet werden müssen dürfen nicht sequenziell während der geplanten Wartung fortfahren, aber jeweils nur ein Update Domain neu gestartet wird. Heute, bietet Azure 1 Wochen erweiterte Benachrichtigung für die geplante Wartung von virtuellen Computern in der Konfiguration mit mehreren Instanzen.

Nach der Wiederherstellung eines virtuellen Computers hier ein Beispiel für was Ihre Windows-Ereignisanzeige angezeigt werden:

<!--Image reference-->
![][image2]

Mit dem Viewer können Sie bestimmen, welche virtuellen Computern in einer mit mehreren Instanzkonfiguration mithilfe der Azure-Portal, Azure PowerShell oder Azure CLI konfiguriert sind. Um festzustellen, welche virtuellen Computern in einer Konfiguration mit mehreren Instanzen befinden, können Sie beispielsweise die Liste der virtuellen Computern mit der Verfügbarkeit festlegen Spalte im Dialogfeld virtuellen Computern hinzugefügt durchsuchen. Im folgenden Beispiel werden den Beispiel-VM1 und Beispiel-VM2 virtuellen Computern in einer Konfiguration Muilti-Instanz aus:

<!--Image reference-->
![][image4]

## <a name="single-instance-configuration-updates"></a>Konfiguration einzelner Instanz updates

Nachdem die Updates mit mehreren Instanzen Konfiguration abgeschlossen sind, wird Azure Updates Konfiguration einzelner Instanz ausgeführt. Dieses Update verursacht auch einen Neustart Ihren virtuellen Computern, die Verfügbarkeit gebildeten nicht ausgeführt werden.

Bitte beachten Sie, dass auch wenn Sie nur eine Instanz in einem Satz Verfügbarkeit ausgeführt haben, die Azure-Plattform es als Update Konfiguration mit mehreren Instanzen behandelt.

Bei virtuellen Computern in einer Konfiguration einzelner Instanz werden virtuellen Computern aktualisiert, indem Sie den virtuellen Computern beenden, das Update auf den Hostcomputer anwenden, und einen Neustart von den virtuellen Computern, etwa 15 Minuten Ausfall. Diese Updates werden auf allen virtuellen Maschinen in einem Bereich in einem einzelnen Wartungsfenster ausgeführt.

Dieses Ereignis geplanten Wartung wirkt sich auf die Verfügbarkeit der Anwendung für diese Art von Konfiguration des virtuellen Computers. Azure bietet eine erweiterte Benachrichtigung 1 Wochen für die geplante Wartung von virtuellen Computern in der Konfiguration einzelner Instanz.

## <a name="email-notification"></a>E-Mail-Benachrichtigung

Einzelner Instanz und mit mehreren Instanzen virtuellen Computern Konfigurationen nur, Azure sendet e-Mail-Kommunikation im Voraus zu benachrichtigen über die bevorstehende geplante Wartung (im Voraus 1 Woche). Diese e-Mail wird das Abonnement und Co-Administratoren-e-Mail-Konten gesendet werden. Hier ist ein Beispiel für diese Art von e-Mails aus:

<!--Image reference-->
![][image1]

## <a name="region-pairs"></a>Region-Paare

Bei der Ausführung von Wartung aktualisiert Azure nur die Instanzen virtuellen Computern in einer einzelnen Region dessen Paar. Beispielsweise wird beim Aktualisieren von den virtuellen Computern in US North Central Azure nicht virtuellen Computern in Süd zentralen uns zur gleichen Zeit aktualisieren. Dies wird jeweils einer separaten geplant Failover oder Lastenausgleich zwischen Regionen aktivieren. Andere Regionen wie North Europa können jedoch zur gleichen Zeit als ostasiatischen US unter Wartung sein.

Lizenzinformationen finden Sie in der folgenden Tabelle Informationen zur aktuellen Region paarweise angegeben werden:

Region 1 | Region 2
:----- | ------:
Nord-zentralen US | Süd zentralen US
Ostasiatische US | Westen US
US-OST 2 | Zentrale US
North Europa | Westen Europa
Süd Ostasien | Ostasien
Ostasiatische China | North China
Japan OST | Japan "Westen"
Brasilien Süd | Süd zentralen US
Australien oder | Australien OST
Indien Central | Indien Süd
Indien "Westen" | Indien Süd
US Gov Iowa | US Gov Virginia

<!--Anchors-->
[image1]: ./media/virtual-machines-common-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-common-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-common-planned-maintenance/AvailabilitySetExample.png


<!--Link references-->
[Virtual Machines Manage Availability]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md

[Understand planned versus unplanned maintenance]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md#Understand-planned-versus-unplanned-maintenance/


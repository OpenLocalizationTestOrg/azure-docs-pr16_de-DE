<properties
   pageTitle="Ausführen von mehreren virtuellen Computern | Bezug auf Architektur | Microsoft Azure"
   description="So führen Sie mehrere Instanzen von virtuellen Computer auf Azure für Skalierbarkeit, Stabilität, Sicherheit und verwaltbarkeit."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Ausführen von mehreren virtuellen Computern auf Azure für Skalierbarkeit und Verfügbarkeit 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel enthält eine Reihe von bewährten Methoden für mehrere Instanzen von virtuellen Computern (virtueller Computer) zur Verbesserung der Skalierbarkeit, Verfügbarkeit, Sicherheit und verwaltbarkeit ausführen.   

In dieser Architektur wird die Arbeitsbelastung auf die Instanzen virtueller Computer verteilt. Es wird eine einzelne öffentliche IP-Adresse und Internet-Datenverkehr wird mit den virtuellen Computern, die mit einem Lastenausgleich verteilt. Diese Architektur kann für eine einzelne Ebene-app, beispielsweise eine statusfreie Web app oder Speicher Cluster verwendet werden. Es ist auch einen Baustein für mehrstufige Applikationen. 

In diesem Artikel zum [Ausführen eines einzelnen virtuellen Computers auf Azure]erstellt[single vm]. Die Ratschläge in diesem Artikel gelten auch für diese Architektur.

## <a name="architecture-diagram"></a>Architekturdiagramm

Virtuellen Computern in Azure erfordern unterstützende Netzwerk- und Ressourcen.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Bei diesem Diagramm handelt, klicken Sie auf der "berechnen - Multi virtueller Computer Registerkarte." 

![[0]][0]

Die Architektur weist die folgenden Komponenten: 

- **Verfügbarkeit festzulegen.** Die [Verfügbarkeit festlegen] [ availability set] enthält die virtuellen Computern und ist für die Unterstützung der [Verfügbarkeit Vereinbarung zum SERVICELEVEL für Azure-virtuellen Computern]erforderlich, vor[vm-sla].

- **VNet**. Jeder virtueller Computer in Azure wird in einem virtuellen Netzwerk (VNet) bereitgestellt, das wiederum in **Subnetze**unterteilt ist.

- **Azure Lastenausgleich.** Der [Lastenausgleich] verteilt eingehende Internetanfragen der Instanzen des virtuellen Computer in einem Satz Verfügbarkeit. Lastenausgleich enthält einige zugehörigen Ressourcen:

  - **Öffentliche IP-Adresse.** Eine öffentliche IP-Adresse ist für den Lastenausgleich zum Empfangen von Internet-Datenverkehr erforderlich.

  - **Front-End-Konfiguration.** Ordnet die öffentliche IP-Adresse mit einem Lastenausgleich.

  - **Back-End-Adresse Ressourcenpool.** Enthält die Netzwerk-Schnittstellen (NICs) für den virtuellen Computern, die den eingehenden Datenverkehr empfangen werden.

- **Laden Sie Lastenausgleich Regeln.** Zum Verteilen von Netzwerkverkehr auf alle virtuellen Computern im Back-End-Adresspool verwendet. 

- **NAT-Regeln.** Für die Weiterleitung Verkehr auf einen bestimmten virtuellen Computer verwendet. Angenommen, damit remote desktop-Protokoll (RDP) mit den virtuellen Computern, erstellen Sie die einer separaten Netzwerk Adresse (Netzwerkadressübersetzung) Regel für jeden virtuellen Computer. 

- **Netzwerk-Schnittstellen (NICs)**. Jeder virtueller Computer hat einen Netzwerkadapter für die Verbindung mit dem Netzwerk.

- **Speicher.** Speicherkonten halten Sie die virtuellen Computer Bilder und andere Datei bezogene Ressourcen, z. B. virtueller Computer diagnostic Daten von Azure erfasst.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure Ressourcenmanager, um die Verweis-Architektur zu installieren, die unten aufgeführten Ratschläge folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen, sollten Sie diese Empfehlungen folgen, es sei denn, Sie haben eine bestimmte Anforderung, die eine Empfehlungen nicht unterstützt. 

### <a name="availability-set-recommendations"></a>Verfügbarkeit festlegen Empfehlungen

Sie müssen mindestens zwei virtuellen Computern erstellen, in die Verfügbarkeit eingestellt, um die [Verfügbarkeit Vereinbarung zum SERVICELEVEL für Azure-virtuellen Computern]unterstützen[vm-sla]. Beachten Sie, dass Azure Lastenausgleich muss auch Lastenausgleich virtuellen Computern auf dieselben Verfügbarkeit angehören.

### <a name="network-recommendations"></a>Netzwerk Empfehlungen

Die virtuellen Computern hinter den Lastenausgleich sollte alle innerhalb der gleichen Subnetz platziert werden. Machen Sie nicht die virtuellen Computern direkt mit dem Internet, aber stattdessen Geben Sie eine private IP-Adresse jede virtueller Computer. Verbinden von Clients mithilfe der öffentlichen IP-Adresse des Lastenausgleich.

### <a name="load-balancer-recommendations"></a>Laden Sie Lastenausgleich Empfehlungen

Fügen Sie alle virtuellen Computern in der Verfügbarkeit von dem Pool Back-End-Adresse des Lastenausgleich festlegen aus.

Definieren Sie laden Lastenausgleich Regeln für direkte Netzwerkverkehr mit den virtuellen Computern an. Beispielsweise um HTTP-Verkehr zu aktivieren, Erstellen einer Regel, die Port 80 aus der Front-End-Konfiguration auf Port 80 auf dem Pool Back-End-Adressen zugeordnet ist. Wenn Lastenausgleich eine Anforderung an Port 80 der öffentlichen IP-Adresse erhält, wird er die Anforderung an Port 80 auf einem der NICs in die Back-End-Adresse Ressourcenpool weiterleiten.

Definieren Sie NAT-Regeln für die Weiterleitung Verkehr auf einen bestimmten virtuellen Computer an. Angenommen, damit RDP mit den virtuellen Computern erstellen eine separate NAT Regel für jeden virtuellen Computer. Jede Regel sollte eine Zahl distinct Port Port 3389, der Standardport für RDP zuordnen. (Beispielsweise verwenden Sie den Port 50001 für "VM1", Port 50002 für "VM2," usw.) Weisen Sie die NAT-Regeln mit den NICs auf den virtuellen Computern an. 

### <a name="storage-account-recommendations"></a>Empfehlungen für Speicher-Konto

Erstellen Sie separate Azure-Speicherkonten für jeden virtuellen Computer auf virtuellen Festplatten (virtuelle Festplatten), um zu vermeiden, drücken die Eingabe/Ausgabe-Vorgänge pro zweiten [(IOPS) Grenzwerte] halten[ vm-disk-limits] für Speicherkonten. 

Erstellen Sie eine Speicherkonto für Diagnoseprotokolle. Dieses Speicherkonto kann von allen virtuellen Computern freigegeben werden.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Es gibt zwei Optionen für die Skalierung auf virtuellen Computern in Azure ein: 

- Verwenden Sie ein Lastenausgleich zum Verteilen von Netzwerkdatenverkehr auf eine Reihe von virtuellen Computern an. Zum Zweck einer Skalierung bereitstellen Sie zusätzliche virtuellen Computern, und setzen sie hinter den Lastenausgleich. 

- Verwenden [virtuellen Computern skalieren Sätze][vmss]. Ein Maßstab enthält eine angegeben-Anzahl von identischen virtuellen Computern hinter einem Lastenausgleich. Virtueller Computer Maßstab legt Support automatische Skalierung basierend auf Leistungswerte. Zunehmender die Last auf den virtuellen Computern sind zusätzliche virtuellen Computern Lastenausgleich automatisch hinzugefügt. 

In den nächsten Abschnitten vergleichen Sie diese beiden Optionen.

### <a name="load-balancer-without-vm-scale-sets"></a>Lastenausgleich ohne virtuellen Computer Maßstab Datensätze

Ein Lastenausgleich nimmt eingehende Netzwerkanfragen und verteilt diese auf die NICs im Adresspool Back-End. Zum Skalieren horizontal, weitere virtueller Computer Instanzen der Verfügbarkeit Gruppe hinzufügen (oder abwärts skalieren virtuelle Computer freigeben). 

Nehmen Sie beispielsweise an, dass Sie einen Webserver ausführen. Sie möchten eine laden Lastenausgleich Regel für Port 80 und/oder Port 443 (SSL) hinzufügen. Wenn ein Client eine HTTP-Anforderung sendet, wählt der Lastenausgleich eine Back-End-IP-Adresse mit einem [hashing Algorithmus] [ load balancer hashing] , die die Quelle IP-Adresse enthält. Auf diese Weise werden Client-Anfragen auf alle virtuellen Computern verteilt. 

> [AZURE.TIP] Beim Hinzufügen eines neuen virtuellen Computers zu einer Verfügbarkeit festlegen, stellen Sie sicher einen Netzwerkadapter für den virtuellen Computer zu erstellen, und die NIC an den Adresspool Back-End-auf dem Lastenausgleich hinzufügen. Andernfalls wird nicht Internetdatenverkehr an den neuen virtuellen Computer weitergeleitet werden.

Jedes Abonnement Azure hat Standardgrenzwerte eingeführt, einschließlich eine maximale Anzahl von virtuellen Computern pro Region. Sie können die Beschränkung erhöhen, indem Sie eine Supportanfrage Ablage. Weitere Informationen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen][subscription-limits].  

### <a name="vm-scale-sets"></a>Virtueller Computer Maßstab Datensätze 

Virtueller Computer Maßstab Sätze helfen Ihnen zum Bereitstellen und Verwalten von eine Reihe von identischen virtuellen Computern bei. Mit allen virtuellen Computern identisch konfiguriert, virtueller Computer Maßstab Sätze unterstützen WAHR automatisch skalieren, ohne vorab provisioning virtuellen Computern, die zur Erstellung von umfangreicher Diensten verwendet, große berechnen, big Data und Sammelartikeleinheit Auslastung zu erleichtern. 

Weitere Informationen zu virtuellen Computer Maßstab Mengen, finden Sie unter [Übersicht des virtuellen Computern Umfang legt][vmss].

Aspekte der Verwendung von virtuellen Computer skalieren Sets:

- Erwägen Sie Maßstab Mengen, wenn Sie schnell virtuellen Computern zu skalieren oder müssen automatisch skalieren müssen. 

- Maßstab Datensätze unterstützt Daten Datenträger derzeit nicht. Die Optionen zum Speichern von Daten sind Azure Dateispeicher, das Laufwerk OS, das Temp-Laufwerk oder einem externen Speicher, z. B. Azure-Speicher. 

- Alle Instanzen von virtuellen Computer innerhalb einer Skala, die automatisch festgelegt werden auf dieselben Verfügbarkeit, mit 5 Fehlerstrukturanalyse und 5 Update-Domänen gehören.

- Standardmäßig verwenden Maßstab Sätze "Bereitstellung überproportional vieler," Was bedeutet, dass Maßstab festlegen Anfangs Weitere virtuellen Computern als Ihnen abgefragte Vorschriften und dann löscht die zusätzlichen virtuellen Computern. Dadurch wird die generelle Erfolgsrate verbessert, beim Bereitstellen von der virtuellen Computern. 

- Es empfiehlt sich nicht mehr, und klicken Sie dann als 20 virtuellen Computern pro Storage Konto mit Bereitstellung überproportional vieler aktivierte oder nicht mehr als 40 virtuellen Computern mit Bereitstellung überproportional vieler deaktiviert.  

- Sie können Ressourcenmanager Vorlagen finden, für die Bereitstellung von Maßstab in den [Azure Schnellstart Vorlagen]legt fest[vmss-quickstart].

- Es gibt zwei einfache Möglichkeiten zum Konfigurieren von virtuellen Computern in einer Gruppe von Farben-Skala bereitgestellt: Erstellen Sie ein benutzerdefiniertes Bild oder Erweiterungen verwenden, um den virtuellen Computer zu konfigurieren, nachdem sie bereitgestellt wurde.

    - Ein Maßstab festlegen auf ein benutzerdefiniertes Bild erstellt muss alle OS Datenträger virtuelle Festplatten in einem Speicherkonto erstellen. 

    - Mit benutzerdefinierten Bilder müssen Sie das Bild auf dem neuesten Stand zu bleiben.

    - Mit der Erweiterung kann dies für einen neu eingerichtete virtuellen Computer von dreht mehr dauern.

Weitere Aspekte zu beachten, finden Sie unter [Entwerfen virtueller Computer Maßstab Datensätze für Skalieren][vmss-design].

> [AZURE.TIP]  Wenn Sie eine Lösung automatische Skalierung verwenden zu können, testen Sie es auch im voraus mit Herstellung Ebene Arbeit lädt. 

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Die Verfügbarkeit macht festlegen Ihrer app mehr flexibel in Bezug auf die geplante und nicht geplante Wartung Ereignisse.

- _Geplante Wartung_ tritt auf, wenn Microsoft die zugrunde liegenden Plattform, aktualisiert manchmal bewirken, dass virtuelle Computer neu gestartet werden. Azure sicher macht die virtuellen Computern in einer Menge Verfügbarkeit sind nicht alle zur gleichen Zeit gestartet, mindestens eine gehalten wird ausgeführt, während andere neu gestartet werden.

- _Nicht geplante Wartung_ passiert, wenn ein Hardwarefehler vorliegt. Azure wird sichergestellt, dass virtuelle Computer in einem Satz Verfügbarkeit über mehrere Server den Shapes für Gestelle bereitgestellt werden. Dadurch wird den Einfluss der Hardwarefehler zu verringern, Netzwerkausfälle, power Interruptions usw..

Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern][availability set]. Das folgende Video verfügt auch über einen guten Überblick über die Verfügbarkeit von Mengen: [Wie kann ich eine Verfügbarkeit festlegen zu skalieren virtuellen Computern konfigurieren][availability set ch9]. 

> [AZURE.WARNING]  Vergewissern Sie sich so konfigurieren Sie die Verfügbarkeit festgelegt, wenn Sie den virtuellen Computer bereitstellen. Derzeit gibt es Möglichkeit keine zum Hinzufügen einer VM Ressourcenmanager zu einer Verfügbarkeit festgelegt, nachdem Sie der virtuellen Computer bereitgestellt wird.

Lastenausgleich verwendet [Gesundheit untersucht] , um die Verfügbarkeit der Instanzen virtueller Computer zu überwachen. Wenn ein Prüfpunkt eine Instanz innerhalb einer bestimmten Zeit erreicht werden kann, stoppt Lastenausgleich Senden von Datenverkehr an diesen virtuellen Computer. Jedoch weiterhin Lastenausgleich Prüfpunkt, und wenn Sie der virtuellen Computer wieder verfügbar ist, der Lastenausgleich Lebensläufen Senden von Datenverkehr an diesen virtuellen Computer.

Hier sind einige Vorschläge auf Laden Lastenausgleich Gesundheit Prüfpunkte aus:

- Prüfpunkte können entweder HTTP oder TCP testen. Wenn Ihre virtuellen Computern Ausführen einer HTTP-Server (IIS, Nginx, Node.js-app und usw.), einer HTTP-Prüfpunkt erstellen. Erstellen Sie andernfalls einen Prüfpunkt TCP ein.

- Geben Sie den Pfad für den HTTP-Endpunkt für eine HTTP-Prüfpunkt. Der Prüfpunkt überprüft für eine Antwort HTTP 200 aus diesem Pfad. Dies kann es sich um Stammpfad ("/") oder eine Überwachung des Systemzustands Endpunkt, der implementiert einige benutzerdefinierten Logik, um die Integrität der Anwendung zu überprüfen. Der Endpunkt muss anonyme HTTP-Anfragen zulassen.

- Der Prüfpunkt wird gesendet wird aus einer [bekannte] [ health-probe-ip] 168.63.129.16 IP-Adresse. Stellen Sie sicher, dass Sie den Datenverkehr in den oder aus diese IP-Adresse in einem beliebigen Firewall-Richtlinien oder Netzwerk Sicherheit Gruppe (NSG) Regeln blockieren nicht.

- [Gesundheit Prüfpunkt Protokolle] verwenden[ health probe log] um den Status der Dienststatus Prüfpunkte anzuzeigen. Aktivieren der Protokollierung in der Azure-Portal für jede Lastenausgleich. Protokolle werden Azure Blob-Speicher geschrieben. Die Protokolle der anzeigen wie viele virtuellen Computern Back-End Netzwerkdatenverkehr aufgrund der fehlerhaften Prüfpunkt Antworten werden nicht empfangen.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Mit mehreren virtuellen Computern wird es wichtig, Prozesse, automatisieren, damit diese zuverlässig und wiederholt werden. Sie können mithilfe der [Azure Automatisierung] [ azure-automation] Bereitstellung, OS Patch sowie zu anderen Aufgaben automatisieren. Azure Automatisierung ist eine Automatisierung-Dienst, der bei Azure ausgeführt wird, und basiert auf Windows PowerShell. Beispiel für Automatisierungsskripts stehen aus dem [Katalog Runbooks] auf TechNet.

## <a name="security-considerations"></a>Zur Sicherheit

Virtuelle Netzwerke sind eine Datenverkehr Isolation Begrenzungslinie in Azure. Virtuellen Computern in einer VNet können nicht direkt auf virtuellen Computern in einer anderen VNet kommunizieren. Virtuellen Computern innerhalb des gleichen VNet kann kommunizieren, es sei denn, Sie [Netzwerk Sicherheitsgruppen] erstellen[ nsg] (NSGs), um den Datenverkehr zu beschränken. Weitere Informationen finden Sie unter [Microsoft Cloud-Diensten und Netzwerk-Sicherheit][network-security].

Für eingehende Internet-Datenverkehr definieren die laden Lastenausgleich Regeln an, welche Datenverkehr Back-End erreichen kann. Jedoch nicht laden Lastenausgleich Regeln mithilfe der IP-, unterstützen können, wenn Sie weißen bestimmte öffentliche IP-Adressen, eine NSG mit dem Subnetz hinzufügen möchten.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Eine Bereitstellung für eine Verweis-Architektur, die diese Empfehlungen implementiert ist auf GitHub verfügbar. Dieser Bezug Architektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Computern (virtuelle Computer).

Die Verweis-Architektur kann entweder mit Windows oder Linux virtuellen Computern unter den folgenden Anweisungen folgen bereitgestellt werden: 

1. Mit der rechten Maustaste unten auf der Schaltfläche, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Vorhandene verwenden** und geben Sie `ra-multi-vm-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Wählen Sie den **Betriebssystem-Typ** aus der Dropdown-Feld, **Windows** oder **Linux**. 
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Die Parameterdateien enthalten eine hartcodierte Administrator-Benutzernamen und Ihr Kennwort ein, und es wird dringend empfohlen, dass Sie beide sofort ändern. Klicken Sie auf dem virtuellen Computer mit dem Namen auf `ra-multi-vm1` Azure-Portal. Klicken Sie dann auf **Kennwort zurücksetzen** in das Blade **Support + Problembehandlung** auf. Wählen Sie im Dropdownmenü **Modus** **Kennwort zurücksetzen** aus, und wählen Sie dann einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort speichern. Wiederholen Sie dies für den virtuellen Computer mit dem Namen `ra-multi-vm2`.

Klicken Sie auf Weitere Methoden zum Bereitstellen dieser Bezug Architektur Informationen finden Sie unter der Infodatei in die [Anleitungen-Single-virtuellen Computer] [ github-folder] GitHub Ordner. 

## <a name="next-steps"></a>Nächste Schritte

Platzieren mehrere virtuelle Computer hinter einem Lastenausgleich ist ein Baustein für Architekturen mit mehreren Ebenen erstellen. Weitere Informationen finden Sie unter [Ausführen des Windows virtuellen Computern für eine mehrstufige Architektur auf Azure] [ n-tier-windows] und [Ausführen des Linux virtuellen Computern für eine mehrstufige Architektur auf Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[Gesundheit Prüfpunkte]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[Lastenausgleich]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Runbooks-Katalog]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architektur einer Lösung Multi-virtueller Computer auf Azure, umfasst eine Gruppe mit zwei virtuellen Computern und ein Lastenausgleich Verfügbarkeit"

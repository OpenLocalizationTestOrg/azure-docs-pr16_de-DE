Dieser Artikel enthält eine Reihe von bewährten Methoden zum Ausführen einer Linux virtuellen Computern (virtueller Computer) auf Azure, achten Sie dabei auf Skalierbarkeit, Verfügbarkeit, Sicherheit und verwaltbarkeit. Azure unterstützt verschiedene gängige Linux Verteilung, einschließlich CentOS, Debian, Red Hat Enterprise, Ubuntu- und FreeBSD ausgeführt. Weitere Informationen finden Sie unter [Azure und Linux][azure-linux].

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. In diesem Artikel wird Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt verwendet.

Nicht empfohlen mithilfe eines einzelnen virtuellen Computers für Produktionsarbeitslasten, da es keine Ebene nach oben / Uhrzeit-Servicevertrag (Vereinbarung zum SERVICELEVEL) für die einzelnen virtuellen Computern auf Azure ist. Wenn Sie der Vereinbarung zum SERVICELEVEL erhalten möchten, müssen Sie mehrere virtuellen Computern in einer [Verfügbarkeit festlegen]bereitstellen[availability-set]. Weitere Informationen finden Sie unter [Ausführen von mehreren virtuellen Computern auf Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Architekturdiagramm

Bereitstellung eines virtuellen Computers in Azure umfasst mehr gleitenden Teile als nur den virtuellen Computer selbst. Datenverarbeitung, Netzwerke und Speicher Elemente, die Sie berücksichtigen müssen, sind vorhanden.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Bei diesem Diagramm handelt, klicken Sie auf "Berechnen - einzelnen virtuellen Computer" Seite.

![[0]][0]

- **Ressourcengruppe.** Eine [_Ressourcengruppe_] [ resource-manager-overview] ist ein Container, zugehörige Ressourcen enthält. Erstellen Sie eine Ressourcengruppe zum Halten Sie der Ressourcen für diese virtuellen Computer an.

- **VM**. Sie können einen virtuellen Computer aus einer Liste von veröffentlichten Bildern oder aus einer virtuellen Festplatte (virtuelle Festplatte)-Datei, die Sie in Azure Blob-Speicher hochladen bereitstellen.

- **OS Datenträger.** Der Datenträger OS ist eine virtuelle Festplatte [Azure-Speicher]gehörende Kehrmatrix[azure-storage]. Dies bedeutet, dass er beibehalten, auch wenn der Hostcomputer-fällt aus. Der Datenträger OS ist `/dev/sda1`.

- **Temporärer Speicherplatz.** Der virtuellen Computer wird mit einem temporären Datenträger erstellt. Auf einem physischen Laufwerk, auf dem Host wird diese Datenträger gespeichert. Es ist _nicht_ in Azure-Speicher gespeichert und möglicherweise abwesend während nach einem Neustart und anderen virtuellen Computer Lebenszyklusereignisse wechseln. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Seite oder austauschen. Der temporäre Datenträger ist `/dev/sdb1` und ist bei bereitgestellt `/mnt/resource` oder `/mnt`.

- **Datenfestplatten.** Einen [Datenträger] [ data-disk] einer beständigen virtuellen für Anwendungsdaten verwendet wird. Datenträger mit Daten werden in Azure-Speicher, wie der OS Datenträger gespeichert.

- **Virtuelles Netzwerk (VNet) und Subnetz.** Jeder virtueller Computer in Azure bereitgestellt wird, in (VNet), welche weiteren ist in Teilnetze unterteilt.

- **Öffentliche IP-Adresse.** Eine öffentliche IP-Adresse ist erforderlich, um die Kommunikation mit dem virtuellen Computer&mdash;beispielsweise über SSH.

- **Netzwerk-Benutzeroberfläche (NIC)**. Die NIC aktiviert den virtuellen Computer zur Kommunikation mit dem virtuellen Netzwerk.

- **Netzwerk-Sicherheitsgruppe (NSG)**. Die [NSG] [ nsg] wird verwendet, um Zulassen/Verweigern Netzwerkdatenverkehr mit dem Subnetz. Sie können eine NSG mit einer einzelnen NIC oder mit einem Subnetz zugeordnet werden soll. Wenn Sie mit einem Subnetz zuordnen, wenden Sie die Regeln NSG auf alle virtuellen Computern in diesem Subnetz aus.
 
- **Diagnose.** Diagnoseprotokolle ist entscheidend für die Verwaltung und Problembehandlung den virtuellen Computer.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen sollten Sie diese Empfehlungen folgen, es sei denn, Sie einen bestimmten benötigen, die nicht empfohlen passen. 

### <a name="vm-recommendations"></a>Virtueller Computer Empfehlungen

Wir empfehlen DS und GS-Serie, da diese Computer Größen [Premium Speicher]unterstützt[premium-storage]. Wählen Sie eine der folgenden Größen Computer, wenn Sie eine spezialisierte Arbeitsbelastung wie High Performance computing haben. Details finden Sie unter [virtuellen Computern Größen][virtual-machine-sizes]. Beim Verschieben von einer bestehenden Arbeitsbelastung in Azure, beginnen Sie mit der virtueller Speicher, die auf Ihre Server lokal am nächsten kommt. Messen Sie die Leistung von Ihrer tatsächlichen Arbeitsbelastung betreffend CPU, Arbeitsspeicher und Festplatten/Ausgang Vorgänge pro Sekunde (IOPS), und passen Sie bei Bedarf die Größe. Wenn Sie mehrere Netzwerkkarten benötigen, achten Sie zudem die NIC Grenzwert für jede Größe.  

Wenn Sie den virtuellen Computer und anderen Ressourcen bereitstellen, müssen Sie einen Speicherort angeben. Wählen Sie einen Speicherort, der interne Benutzer oder Kunden am nächsten ist im Allgemeinen aus. Nicht alle virtuellen Computer Größen können jedoch an allen Standorten verfügbar sein. Weitere Informationen finden Sie unter [Services nach Region][services-by-region]. Führen Sie folgenden Befehl Azure line Interface (CLI) aus, um die Liste der an einem bestimmten Speicherort verfügbaren virtuellen Computer-Größen:

```
    azure vm sizes --location <location>
```

Informationen zum Auswählen eines veröffentlichten virtueller Computer Bilds, finden Sie unter [Navigieren und Bilder wählen Azure-virtuellen Computern][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Empfehlungen im Zusammenhang mit dem Datenträger und Speicher

Zur Optimierung der Systemleistung e/a-, empfehlen wir [Premium Speicher][premium-storage], die Daten auf Solid Laufwerken (SSDs) speichert. Kosten basiert auf die Größe des bereitgestellte Festplatte. IOPS und Durchsatz (d. h., Daten zu Rate übertragen) auch abhängen Datenträger abgesehen, also wenn Sie einen Datenträger bereitstellen, sollten Sie alle drei Faktoren (Kapazität, IOPS und Durchsatz). 

Ein Speicher-Konto kann 1 bis 20 virtuellen Computern unterstützt.

Fügen Sie einen oder mehrere Daten Datenträger hinzu. Wenn Sie eine virtuelle Festplatte erstellen, ist es nicht formatiert. Melden Sie sich an den virtuellen Computer so formatieren Sie den Datenträger aus. Festplatten mit den Daten anzeigen als `/dev/sdc`, `/dev/sdd`und so weiter. Sie können ausführen `lsblk` der Geräte sperren, einschließlich der Laufwerke aufgelistet. Verwenden Sie einen Datenträger, eine Partition und ein Dateisystem erstellen, und hängen Sie die Festplatte. Beispiel:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Wenn Sie viele Daten Datenträger haben, achten Sie darauf, dass Sie von der Gesamtsumme e/a-Rahmen des Speicherkontos. Weitere Informationen finden Sie unter [Virtuellen Computern Speichergrenze][vm-disk-limits].

Wenn Sie einen Datenträger hinzufügen, wird der Datenträger eine logische Einheit Zahl (LUN) ID zugewiesen. Optional können Sie angeben, die LUN-ID &mdash; angenommen, Sie sind Ersetzen einer Festplatte und die gleichen LUN-ID beibehalten möchten, oder Sie haben eine app, die für eine bestimmte LUN ID. sieht aus Beachten Sie jedoch, dass LUN-IDs für jeden Datenträger eindeutig sein müssen.

Kann der Scheduler e/a-, für die Leistung von Solid Laufwerken (SSDs) (verwendet von Premium Speicher) optimiert ändern möchten. Wird eine allgemeine empfohlen, den Scheduler NOOP für SSDs verwendet, aber Sie sollte Verwenden eines Tools, wie z. B. [Iostat] Datenträger e/a-Leistung für Ihre bestimmten Arbeitsbelastung überwachen.

- Erstellen Sie zur Optimierung der Systemleistung einer separaten Speicher-Kontos zum Halten von Diagnoseprotokollen. Ein Standardkonto Lokales redundante Speicher (LRS) ist für Diagnoseprotokolle ausreichend.


### <a name="network-recommendations"></a>Netzwerk Empfehlungen

Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Die Standardeinstellung ist dynamische.

- Reservieren Sie eine [statische IP-Adresse] [ static-ip] Wenn Sie eine feste IP-Adresse benötigen, die nicht ändert &mdash; angenommen, müssen Sie einen A-Eintrag in DNS erstellen oder muss die IP-Adresse um weißen Liste hinzugefügt werden.

- Sie können auch einen vollqualifizierten Domänennamen (FQDN) für die IP-Adresse erstellen. Sie können dann einen [CNAME-Eintrag] registrieren[ cname-record] in DNS, die auf den vollqualifizierten Domänennamen verweist. Weitere Informationen finden Sie unter [Erstellen eines vollqualifizierten Domänennamen Azure-Portal][fqdn].

Alle NSGs enthalten eine Reihe von [Regeln für das standardmäßige][nsg-default-rules], einschließlich einer Regel, die alle eingehenden Datenverkehrs blockiert. Die Regeln für die nicht gelöscht werden, aber andere Regeln können sie überschreiben. Um den Datenverkehr im Internet zu aktivieren, erstellen Sie Regeln, die eingehenden Datenverkehr für bestimmte Ports zulassen &mdash; beispielsweise port 80 für HTTP.  

Um SSH zu aktivieren, fügen Sie eine Regel, die den eingehenden Datenaustausch TCP-22 NSG aus.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Sie können einen virtuellen Computer nach oben oder unten skalieren, durch [Ändern der Größe des virtuellen Computer][vm-resize]. 

Um horizontal skalieren, setzen Sie mindestens zwei virtuellen Computern in einer Verfügbarkeit hinter einem Lastenausgleich festlegen. Details finden Sie unter [Ausführen von mehreren virtuellen Computern auf Azure][multi-vm].

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Wie bereits erwähnt, ist es keine Vereinbarung zum SERVICELEVEL für einen einzelnen virtuellen Computer an. Wenn Sie der Vereinbarung zum SERVICELEVEL erhalten möchten, müssen Sie mehrere virtuelle Computer in einem Satz Verfügbarkeit bereitstellen.

Ihre virtuellen Computer beeinträchtigt werden durch [geplante Wartung] [ planned-maintenance] oder [nicht geplante Wartung][manage-vm-availability]. [Virtueller Computer neu gestartet Protokolle] können[ reboot-logs] feststellen, ob ein Neustart virtueller Computer nach der geplanten Wartung verursacht wurde.

Virtuelle Festplatten werden von [Azure-Speicher]gesicherten[azure-storage], die für Zuverlässigkeit und Verfügbarkeit repliziert ist.

Um unbeabsichtigtes Schutz vor Datenverlust bei normalen Vorgängen (z. B. aufgrund Benutzerfehler), sollten Sie auch Point-in-Time Sicherung mit [Blob Momentaufnahmen] implementieren[ blob-snapshot] oder einem anderen Tool.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

**Ressourcengruppen.** Setzen eng verknüpfte Ressourcen, die denselben Lebenszyklus in derselben [Ressourcengruppe]freigeben[resource-manager-overview]. Ressourcengruppen können Sie bereitstellen und Überwachen von Ressourcen als Gruppe Abrechnung Kosten nach Ressourcengruppe zugeschlagen. Sie können auch Ressourcen löschen, die als eine Reihe, also für Test-Bereitstellungen sehr hilfreich. Geben Sie Ressourcen aussagekräftige Namen zu. Die vereinfacht das zum Suchen nach einer bestimmten Ressource und Grundlegendes zu seiner Rolle. [Empfohlene Benennungskonvention für Azure Ressourcen]finden Sie unter[naming conventions].

**SSH**. Bevor Sie eine Linux VM erstellen, Generieren einer 2048-Bit RSA öffentlich-privaten Schlüssel. Verwenden Sie die Datei für den öffentliche Schlüssel aus, wenn Sie den virtuellen Computer erstellen. Weitere Informationen finden Sie unter [So verwenden Sie SSH mit Linux und Mac auf Azure][ssh-linux].

**Virtueller Computer-Diagnose.** Überwachung aktivieren und Diagnose, einschließlich grundlegende Gesundheit Kennzahlen, Diagnose Infrastrukturprotokolle und [Boot Diagnose][boot-diagnostics]. Boot Diagnose hilft Ihnen ein Boot-Fehler diagnostizieren, wenn Ihre virtuellen Computer in einem nicht startfähige Zustand erhält. Weitere Informationen finden Sie unter [Aktivieren Sie die Überwachung und Diagnose][enable-monitoring].  

Der folgende CLI-Befehl ermöglicht Diagnose:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Beenden eines virtuellen Computers an.** Azure wird unterschieden zwischen den Status "Angehalten" und "Deallocated". Sie unterliegen, wenn Sie der virtuellen Computer Status "angehalten". Sie unterliegen nicht, wenn Sie der virtuellen Computer freigegeben.

Verwenden Sie den folgenden Befehl aus CLI ein virtuellen Computers freigeben:

```
    azure vm deallocate <resource-group> <vm-name>
```

- Die Schaltfläche **Beenden** der Azure-Portal freigegeben auch den virtuellen Computer an. Jedoch wenn Sie fahren Sie über das Betriebssystem während angemeldet, der virtuellen Computer ist beendet, aber _nicht_ freigegeben, werden daher müssen Sie immer noch in Rechnung gestellt.

**Löschen eines virtuellen Computers an.** Wenn Sie einen virtuellen Computer löschen, werden die virtuellen Festplatten nicht gelöscht. Dies bedeutet, dass Sie den virtuellen Computer problemlos löschen können, ohne Daten zu verlieren. Sie werden jedoch weiterhin für Speicher Rechnung gestellt. Wenn Sie die virtuelle Festplatte löschen möchten, löschen Sie die Datei aus dem [Blob-Speicher][blob-storage].

Wenn Sie nicht versehentliches löschen möchten, verwenden Sie eine [Ressource sperren] [ resource-lock] die gesamte Ressource Gruppe oder Sperren einzelnen Ressourcen, z. B. den virtuellen Computer sperren. 

## <a name="security-considerations"></a>Zur Sicherheit

Automatisieren von Updates für OS mit der Erweiterung [OSPatching] virtueller Computer. Wenn Sie den virtuellen Computer bereitstellen, installieren Sie diese Erweiterung. Sie können so oft Patches installiert und ob Sie nach Patch abstürzt angeben.

Verwenden Sie die [Steuerung des Benutzerzugriffs rollenbasierte] [ rbac] (RBAC) den Zugriff auf den Azure Ressourcen steuern, die Sie bereitstellen. RBAC können Sie Mitgliedern Ihres Teams DevOps Autorisierungsrollen zuweisen. Angenommen, kann die Rolle Leser Azure Ressourcen anzeigen, aber nicht erstellen, verwalten oder löschen ist möglich. Einige Rollen gelten nur für bestimmte Azure Ressourcentypen zur Verfügung. Beispielsweise kann Teilnehmerrolle für das virtuellen Computern neu starten oder freigeben ein virtuellen Computers, Zurücksetzen des Administratorkennworts, erstellen einen virtuellen usw.. Anderen [integrierten RBAC-Rollen] [ rbac-roles] die nützlich sein für diese Architektur Verweis einschließen [DevTest Labs Benutzer] [ rbac-devtest] und [Netzwerk Mitwirkender][rbac-network]. 

Ein Benutzer kann mehrere Rollen zugewiesen werden, und Sie können benutzerdefinierte Rollen für noch mehr abgestimmte Berechtigungen erstellen.

> [AZURE.NOTE] RBAC bedeutet keine Beschränkung der Aktionen, die ein Benutzer, um einen virtuellen Computer ausführen kann. Diese Berechtigungen werden durch den Kontotyp auf dem Gast OS bestimmt.   

Verwenden der [Überwachungsprotokolle] [ audit-logs] provisioning Aktionen und anderen virtuellen Computer Ereignisse angezeigt.

- Erwägen Sie die [Verschlüsselung der Azure-Festplatten] [ disk-encryption] Wenn Sie die Datenträger OS und Daten verschlüsseln müssen. 

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Eine [Bereitstellung] [ github-folder] für einen Verweis Architektur, die bewährten veranschaulicht verfügbar ist. Dieser Bezug Architektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG) und eines einzelnen virtuellen Computers (virtueller Computer).

Es gibt mehrere Methoden zum Bereitstellen dieser Architektur Bezug ein. Am einfachsten die folgenden Schritte ausführen: 

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen".
[![Bereitstellen für Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-single-vm-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Wählen Sie den **Betriebssystem-Typ** aus der Dropdown-Feld **Linux**.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Die Parameterdateien enthalten eine hartcodierte Administrator-Benutzernamen und Ihr Kennwort ein, und es wird dringend empfohlen, dass Sie beide sofort ändern. Klicken Sie auf dem virtuellen Computer mit dem Namen auf `ra-single-vm0 `im Portal Azure. Klicken Sie dann auf **Kennwort zurücksetzen** im Abschnitt **Support + Problembehandlung** auf. Wählen Sie im Dropdownmenü **Modus** **Kennwort zurücksetzen** aus, und wählen Sie dann einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort beibehalten werden.

Klicken Sie auf Weitere Methoden zum Bereitstellen dieser Bezug Architektur Informationen finden Sie unter der Infodatei in die [Anleitungen-Single-virtuellen Computer] [ github-folder] Github Ordner.

## <a name="customize-the-deployment"></a>Anpassen der bereitstellungs

Zum Ändern der bereitstellungs entsprechend Ihren Anforderungen, folgen Sie den Anweisungen in die [Anleitungen-Single-virtuellen Computer] [ github-folder] Seite.

## <a name="next-steps"></a>Nächste Schritte

In der Reihenfolge für die [Vereinbarung zum SERVICELEVEL für virtuellen Computern] [ vm-sla] zum anwenden möchten, müssen Sie mindestens zwei Instanzen in einer Verfügbarkeit festlegen bereitstellen. Weitere Informationen finden Sie unter [Ausführen von mehreren virtuellen Computern auf Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Einzelne Linux VM Architektur in Azure"





Dieser Artikel behandelt einige häufig gestellte Fragen, bitten Sie den Benutzer zur Azure-virtuellen Computern mit dem klassischen Bereitstellungsmodell erstellt.

## <a name="can-i-migrate-my-vm-created-in-the-classic-deployment-model-to-the-new-resource-manager-model"></a>Kann ich meine virtueller Computer im Bereitstellungsmodell klassischen zum neuen Ressourcenmanager Modell erstellt migrieren?

Ja. Anweisungen zum Migrieren finden Sie unter:

- [Migrieren von klassischen zu Azure-Ressourcenmanager Azure PowerShell verwenden](../articles/virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md).

- [Migrieren von klassischen zu Azure-Ressourcenmanager Azure CLI verwenden](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich eine Azure-virtuellen Computers werden ausgeführt?

Alle Abonnenten können Server-Software auf eine Azure-virtuellen Computern ausgeführt werden. Sie können die neuesten Versionen von Windows Server als auch eine Vielzahl von Linux-Versionen ausführen. Support-Details finden Sie unter:

• Für Windows virtuelle Computer – [Microsoft-Software unterstützen für Azure virtuellen Computern](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Für Linux virtuellen Computern – [Linux auf Verteilung Azure unterstützt](http://go.microsoft.com/fwlink/p/?LinkId=393551)

Für Windows-Client-Bilder sind bestimmte Versionen von Windows 7 und Windows 8.1 MSDN Azure Vorteile und MSDN-Entwickler und Testen nutzungsbasierte Abonnenten, für die Entwicklung und Testaufgaben zur Verfügung. Ausführliche Anweisungen und Einschränkungen, einschließlich finden Sie in [Windows-Client-Bilder für MSDN-Abonnenten](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).

## <a name="why-are-affinity-groups-being-deprecated"></a>Warum sind Gruppen die nicht mehr wird unterstützt?

Zugehörigkeit Gruppen handelt es sich um ein älteres Konzept für eine geografische Gruppierung von einem Kunden Cloud-Dienst Bereitstellungen und Speicherkonten in Azure. Sie wurden zur Verbesserung der Leistung des Netzwerks virtueller Computer-zu-virtuellen Computer in der frühen Azure Netzwerkentwürfen ursprünglich bereitgestellt. Sie unterstützt auch die erste Version von virtuellen Netzwerken (VNets), die auf eine kleine Gruppe von Hardware in einem Bereich eingeschränkt wurden.

Das aktuelle Azure Netzwerk innerhalb eines Bereichs dient, sodass die Gruppen die nicht mehr benötigt werden. Virtuelle Netzwerke sind auch Landes-/ Bereich, damit eine Gruppe für die Zugehörigkeit nicht mehr benötigt wird, wenn Sie ein virtuelles Netzwerk verwenden. Aufgrund der folgenden Verbesserungen empfehlen wir nicht mehr Gruppen die verwenden, da sie in einigen Szenarien eingeschränkt werden können. Zugehörigkeit Entwurfsphase kann unnötigerweise Ihrer virtuellen Computer auf bestimmte Hardware zugeordnet werden, die die Wahl der virtuellen Computer Größen beschränkt, die Ihnen zur Verfügung stehen. Es möglicherweise auch auf Kapazität-bezogene Fehler führen, wenn Sie versuchen, eine neue virtuellen Computern hinzufügen, wenn die spezielle Hardware, die die Zugehörigkeit Gruppe zugeordneten fast überschritten hat.

Zugehörigkeit Gruppenfunktionen sind bereits im Bereitstellungsmodell Azure Ressourcenmanager und Azure-Portal veraltet. Für die klassischen Azure-Portal haben wir veralteter Unterstützung für das Erstellen von Gruppen und Erstellen von Speicherressourcen, die an eine Gruppe für die Zugehörigkeit angeheftet werden. Sie benötigen keine vorhandenen Clouddienste zu ändern, die eine Gruppe Zugehörigkeit verwenden. Jedoch sollten Sie keine Gruppen die für neue Clouddienste verwenden, es sei denn, eine Azure Supportmitarbeiter sie empfiehlt.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann ich mit einem virtuellen Computer verwenden?

Jeder Datenträger kann bis zu 1 TB sein. Die Anzahl der Datenträger von Daten, die Sie verwenden können, hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-sizes.md).

Ein Konto Azure-Speicher bietet Speicherplatz für den Datenträger Betriebssystem und alle Datenlaufwerke. Jeder Datenträger ist eine .vhd-Datei als Seitenblob gespeichert. Preise Details, finden Sie unter [Speicher Preise Details](http://go.microsoft.com/fwlink/p/?LinkId=396819).

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Welche Typen von virtuellen Festplatte kann ich verwenden?

Azure unterstützt nur dadurch, virtuellen Festplatten virtuelle Festplatte-Format. Wenn Sie eine VHDX, die Sie in Azure verwenden möchten verfügen, müssen Sie zuerst mithilfe von Hyper-V-Manager oder das Cmdlet [Konvertieren-virtuelle Festplatte](http://go.microsoft.com/fwlink/p/?LinkId=393656) konvertieren. Nachdem Sie werden, verwenden Sie [Hinzufügen-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) -Cmdlet (im Modus für Servicemanagement) die virtuelle Festplatte mit einem Speicherkonto in Azure hochladen, daher können Sie es mit virtuellen Computern verwenden.

- Informationen zu Linux finden Sie unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](../articles/virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md).

- Windows-Anweisungen finden Sie unter [Erstellen und Hochladen einer Windows Server virtuellen in Azure](../articles/virtual-machines/virtual-machines-windows-classic-createupload-vhd.md).

## <a name="are-these-virtual-machines-the-same-as-hyper-v-virtual-machines"></a>Sind Sie diesen virtuellen Computern Hyper-V-virtuellen Computern identisch?

Auf vielfältige Weise sie ähnlich wie "Generation 1" Hyper-V virtuelle Computer befinden, jedoch sind nicht genau identisch. Beide Typen bieten virtualisierten Hardware und virtuellen Festplatten virtuelle Festplatte-Format kompatibel sind. Dies bedeutet, dass Sie diese zwischen Hyper-V und Azure verschieben können. Drei wichtigsten Unterschiede, die manchmal überraschen Sie Hyper-V Benutzer sind:

- Azure nicht Console-Zugriff auf virtuellen Computers zu ermöglichen. Es gibt keine Möglichkeit, einen virtuellen zugreifen, bis sie fertig ist Boot.
- Azure virtuellen Computern in den meisten [Größen](../articles/virtual-machines/virtual-machines-linux-sizes.md) haben nur 1 virtuellen Netzwerkadapter, was bedeutet, dass sie auch nur 1 externe IP-Adresse verfügen, können. (Die A8 und A9 Größen verwenden Sie einen zweiten Netzwerkadapter für die Anwendungskommunikation zwischen Instanzen in nur in bestimmten Szenarien.)
- Azure-virtuellen Computern unterstützt nicht der zweiten Generation 2 Hyper-V virtueller Computer Features. Details zu diesen Funktionen finden Sie unter [Virtuellen Computern Spezifikationen für Hyper-V](http://technet.microsoft.com/library/dn592184.aspx) und [Generation 2 virtuellen Computern Übersicht] (https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Können diesen virtuellen Computern meine vorhandene, lokale Netzwerke Infrastruktur?

Azure-virtuellen Netzwerk können Sie für im Bereitstellungsmodell klassischen erstellte Maschinen erweitern Ihre vorhandene Infrastruktur. Der Ansatz ist wie eine Zweigstelle einrichten. Sie können bereitstellen und verwalten virtuelle private Netzwerke (VPN) in Azure sowie sicher, Herstellen einer Verbindung mit lokalen IT-Infrastruktur. Details finden Sie unter [Übersicht über Virtual Netzwerk](../articles/virtual-network/virtual-networks-overview.md).

Sie müssen das Netzwerk angeben, das gewünschte des virtuellen Computers beim Erstellen des virtuellen Computers gehören soll. Sie können keine vorhandenen virtuellen Computer zu einem virtuellen Netzwerk teilnehmen. Sie können jedoch umgehen, indem Sie die virtuelle Festplatte (virtuelle Festplatte) vom vorhandenen virtuellen Computer trennen und ihn verwenden, um einen neuen virtuellen Computer mit der Netzwerkkonfiguration gewünschte zu erstellen.

## <a name="how-can-i-access--my-virtual-machine"></a>Wie kann ich meine virtuellen Computers zugreifen?

Sie müssen eine remote-Verbindung zu virtuellen Computer mit Remotedesktop-Verbindung für einen virtuellen Windows oder eine Secure Shell (SSH) für eine Linux VM anmelden. Anweisungen finden Sie unter:

- [So melden Sie sich an einen virtuellen Computer mit WindowsServer](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md). Bis zu 2 aktiven Verbindungen werden unterstützt, es sei denn, der Server als Host Sitzung Remote Desktop Services konfiguriert ist.  
- [So melden Sie sich an einen virtuellen Computer mit Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md). Standardmäßig kann SSH bis zu 10 gleichzeitige Zugriffe. Sie können diese Nummer erhöhen, indem Sie die Konfigurationsdatei bearbeiten.


Wenn Sie Probleme mit Remotedesktop oder SSH Probleme auftreten, installieren Sie und verwenden Sie die Erweiterung [VMAccess](../articles/virtual-machines/virtual-machines-windows-extensions-features.md) , um zur Behebung des Problems.

Für Windows-virtuellen Computern die enthalten zusätzliche Optionen aus:

- Klicken Sie im Portal Azure klassischen suchen Sie den virtuellen Computer und dann auf **RAS zurücksetzen** aus der Befehlsleiste.
- Überprüfen Sie [Remotedesktop Problembehandlung bei Verbindungen zu einer Windows Azure virtuellen-Computer](../articles/virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md)an.
- Verwenden von Windows PowerShell Remote in Verbindung mit dem virtuellen Computer, oder erstellen Sie zusätzliche Endpunkte für andere Ressourcen in Verbindung mit dem virtuellen Computer. Weitere Informationen finden Sie unter [Festlegen von Endpunkte eines virtuellen Computers](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Wenn Sie Hyper-V vertraut sind, suchen Sie möglicherweise für eine ähnliche VMConnect Tool. Azure bietet kein ähnliches Tool, da Console-Zugriff auf einen virtuellen Computer nicht unterstützt wird.

## <a name="can-i-use-the-temporary-disk-the-d-drive-for-windows-or-devsdb1-for-linux-to-store-data"></a>Kann ich den temporären Datenträger (Laufwerk D: für Windows) oder /dev/sdb1 für Linux verwenden zum Speichern von Daten?

Sie dürfen nicht den temporären Datenträger (Laufwerk D: standardmäßig für Windows, oder /dev/sdb1 für Linux) verwenden, um Daten zu speichern. Sie sind nur temporäre Speicherung, damit Sie riskieren würde Verlust von Daten, die nicht wiederhergestellt werden können. Dies kann geschehen, wenn des virtuellen Computers zu einem anderen Host bewegt wird. Ändern der Größe eines virtuellen Computers, sind Aktualisieren der Host oder ein Hardwarefehler auf dem Host einige Gründe, die einen virtuellen Computer verschieben können.

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?

Klicken Sie auf einem Windows-Computer Sie können den Buchstaben des Laufwerks ändern, indem Sie die Seite Datei verschieben und Neuzuweisen Laufwerkbuchstaben, aber um sicherzustellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen müssen. Anweisungen finden Sie unter [Ändern Sie den Buchstaben des dem temporären Windows-Datenträger](../articles/virtual-machines/virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-upgrade-the-guest-operating-system"></a>Wie kann ich das Gast-Betriebssystem aktualisieren?

Die Begriff der Aktualisierung bedeutet im Allgemeinen auf eine neuere Version des Betriebssystems, während Sie auf der gleichen Hardware zur Vermeidung verschieben. Für Azure-virtuellen Computern unterscheidet sich dieser Vorgang zum Verschieben von auf eine neuere Version Linux und Windows:

- Verwenden Sie für Linux virtuelle Computer Paket Tools zum Projektmanagement und Verfahren entsprechende für die Verteilung aus.
- Einem Windows-Computer müssen Sie auf den Server verwenden, etwa Windows Server-Migrationstools migrieren. Versuchen Sie nicht, Gast-BS zu aktualisieren, während sie auf Azure gespeichert ist. Es aufgrund des Risikos von Verlust des Zugriffs auf virtuellen Computers werden nicht unterstützt. Wenn während des Upgrades Probleme auftreten, Sie verlieren die Möglichkeit zum Starten einer Sitzung Remote Desktop und würde nicht in der Lage, die Probleme zu beheben.

Allgemeine Informationen zu Tools und Prozesse für die Migration von einem Windows-Server finden Sie unter [Migrieren von Rollen und Features zu Windows Server](http://go.microsoft.com/fwlink/p/?LinkId=396940).



## <a name="whats-the-default-user-name-and-password-on-the-virtual-machine"></a>Was ist der Standard-Benutzernamen und das Kennwort des virtuellen Computers?

Die Bilder von Azure bereitgestellten keiner vorkonfigurierten Benutzernamen und Ihr Kennwort ein. Beim Erstellen von virtuellen Computern, die mit einer der diesen Bildern müssen Sie angeben, einen Benutzernamen und das Kennwort, das Sie melden Sie sich am virtuellen Computer verwenden möchten.

Wenn Sie den Benutzernamen vergessen haben oder Kennwörter haben und der Agent virtueller Computer installiert haben, können Sie installieren und verwenden die Erweiterung [VMAccess](../articles/virtual-machines/virtual-machines-windows-extensions-features.md) zur Behebung des Problems.

Weitere Details:


- Für die Bilder Linux Wenn Sie das klassische Azure-Portal verwenden, 'Azureuser' wird als Standardbenutzername angegeben, aber Sie können dies dokumentkatalog ' von ' anstelle von 'Symbolleiste erstellen' als die Methode zum Erstellen des virtuellen Computers ändern. Mithilfe von 'From Gallery' ermöglicht Ihnen außerdem entscheiden, ob Sie ein Kennwort, einen Schlüssel SSH oder beides verwenden Sie bei der Anmeldung beim. Das Benutzerkonto ist ein Benutzer ohne Berechtigungen, der zum Ausführen von berechtigten Befehle 'Sudo' zugreifen. Das Konto "Root" ist deaktiviert.


- Für Windows-Bilder müssen Sie einen Benutzernamen und ein Kennwort angeben, wenn Sie den virtuellen Computer erstellen. Das Konto wird zur Gruppe Administratoren hinzugefügt.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Werden Azure Anti-Virus auf meine virtuellen Computern ausgeführt kann?

Azure bietet mehrere Optionen für Anti-Virus Lösungen, aber besteht darin, Sie ihn verwalten. Angenommen, Sie möglicherweise ein gesondertes Abonnement für Antischadsoftware benötigen, und Sie müssen entscheiden, wann Ihrer ausführen und Updates installieren. Sie können Anti-Virus Support mit der Erweiterung virtueller Computer für Microsoft Antimalware, Symantec Endpunkt Schutz oder TrendMicro Tiefe Security Agent bei der Erstellung von eines Windows-Computers oder zu einem späteren Zeitpunkt hinzufügen. Die Symantec und TrendMicro Extensions können Sie eine kostenlose Testversion zeitlich oder einem vorhandenen Enterprise-Abonnement verwenden. Microsoft Antimalware ist kostenlos. Weitere Informationen finden Sie unter:

- [So installieren und Konfigurieren von Symantec Endpunkt Schutz einer Azure-virtuellen Computers](http://go.microsoft.com/fwlink/p/?LinkId=404207)
- [So installieren und Konfigurieren von Trend Micro Tiefe Security als Dienst eine Azure-virtuellen Computers](http://go.microsoft.com/fwlink/p/?LinkId=404206)
- [Bereitstellen von Modul Lösungen auf Azure-virtuellen Computern](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Was sind die Optionen für die Sicherung und Wiederherstellung habe?

Azure Sicherung ist als Vorschau in bestimmte Regionen zur Verfügung. Weitere Informationen finden Sie unter [Azure-virtuellen Computern sichern](../articles/backup/backup-azure-vms.md). Andere Lösungen stehen zertifizierte Partner zur Verfügung. Um herauszufinden, was derzeit verfügbar ist, suchen Sie die Azure Marketplace.

Eine weitere Option besteht darin, die Funktionen Momentaufnahme Blob-Speicher verwenden. Dazu müssen Sie den virtuellen Computer, bevor Sie einen Vorgang beenden, das auf einen Blob Snapshot basiert. Dies schreibt ausstehenden Daten speichert und zusammengeführt werden im Dateisystem in einem konsistenten Zustand.

## <a name="how-does-azure-charge-for-my-vm"></a>Funktionsweise von Azure Gebühr für meine virtueller Computer?

Azure Gebühren einen stündlichen Preis basierend auf Größe und Betriebssystem des virtuellen Computers. Teilweise Stunden Azure Gebühren nur für die Anzahl der Minuten verwenden. Wenn Sie den virtuellen Computer mit einer virtuellen Computer Bild mit bestimmten vorinstallierter Software erstellen, möglicherweise zusätzliche stündlich Software anfallen. Azure Gebühren für den Speicher für das Betriebssystem und Daten Datenträger des virtuellen Computers getrennt. Temporäre Festplattenspeicher ist kostenlos.

Sie unterliegen, wenn der Status virtueller Computer ausgeführt oder angehalten wird, aber Sie unterliegen nicht, wenn der Status virtueller Computer beendet wird (Aufheben der Reservierung). Um einen virtuellen im Zustand beendet (ohne Zuordnung) zu setzen, führen Sie eine der folgenden Aktionen aus:

- Fahren Sie oder löschen Sie den virtuellen Computer vom klassischen Azure-Portal.
- Verwenden des Stopp-AzureVM-Cmdlets Azure PowerShell-Modul.
- Verwenden Sie den Vorgang war(en) Rolle in der Dienst Management REST-API und geben Sie StoppedDeallocated für das Element PostShutdownAction an.

Weitere Informationen hierzu finden Sie unter [Virtuellen Computern Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Wird Azure meiner virtuellen Computer für die Wartung neu gestartet?

Azure startet manchmal Ihrer virtuellen Computer als Teil der regelmäßigen, geplanten Wartung Aktualisierungen in den Azure Datencentern neu.

Nicht geplante Wartung Ereignisse können auftreten, wenn Azure ein Hardwareproblem schwerwiegende erkennt, die Ihre virtuellen Computer wirkt sich auf. Weitere Informationen zu ungeplanten Ereignissen Azure automatisch migriert den virtuellen Computer mit einem fehlerfrei Host und startet den virtuellen Computer neu.

Für alle eigenständigen virtueller Computer (also der virtuellen Computer ist nicht Teil einer Menge Verfügbarkeit), Azure benachrichtigt des Abonnements des Dienstadministrator per e-Mail mindestens eine Woche vor der geplanten Wartung, da die virtuellen Computern während der Aktualisierung neu gestartet werden können. Anwendung, die auf den virtuellen Computern ausgeführt werden können Ausfallzeiten kommen.

Auch können Sie über die klassischen Azure-Portal oder Azure PowerShell die Protokolle Neustart anzeigen als der Neustart aufgrund von auftrat geplante Wartung. Details finden Sie unter [Anzeigen Protokolle für virtuellen Computer neu zu starten](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

Damit redundante verwendet werden, setzen Sie mindestens zwei ähnlich konfigurierte virtuellen Computern in demselben Satz Verfügbarkeit aus. Dadurch wird sichergestellt, dass mindestens ein virtueller Computer während der geplanten oder nicht geplanten Wartung verfügbar ist. Azure garantiert bestimmte Ebenen virtueller Computer Verfügbarkeit für diese Konfiguration. Details finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern](../articles/virtual-machines/virtual-machines-windows-manage-availability.md).



## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Informationen zum Azure-virtuellen Computern](../articles/virtual-machines/virtual-machines-linux-about.md)

[Andere Methoden zum Erstellen von virtuellen Linux-Computer](../articles/virtual-machines/virtual-machines-linux-creation-choices.md)

[Andere Methoden zum Erstellen von einem Windows-Computer](../articles/virtual-machines/virtual-machines-windows-creation-choices.md)

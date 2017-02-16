Bevor Sie die CLI Azure mit Ressourcenmanager Befehlen und Vorlagen zur Bereitstellung von Azure Ressourcen und Auslastung Entwurfsphase Ressource verwenden können, benötigen Sie ein Konto mit Azure. Wenn Sie nicht über ein Konto verfügen, können Sie eine [kostenlose Azure Testversion](https://azure.microsoft.com/pricing/free-trial/)erhalten.

Wenn Sie noch nicht bereits installiert Azure CLI und bei einer Verbindung zu Ihrem Abonnement, finden Sie unter [Installieren der CLI Azure](../articles/xplat-cli-install.md) der Modus `arm` mit `azure config mode arm`, und das Herstellen einer Verbindung mit Azure mit der `azure login` Befehl.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Ressourcenmanager Azure Basisbefehlen in Azure CLI

Dieser Artikel behandelt das Basisbefehlen, verwenden Sie zum Verwalten von und interagieren mit der Cloud Ressourcen (hauptsächlich virtuellen Computern) in Ihrem Abonnement Azure mit Azure CLI werden soll.  Ausführlichere Hilfe mit bestimmten Befehlszeilenoptionen und Optionen können den Befehl online-Hilfe und Optionen durch Eingeben der `azure <command> <subcommand> --help` oder `azure help <command> <subcommand>`.

> [AZURE.NOTE] In diesen Beispielen enthalten nicht Vorlage basierende Vorgänge, die in der Regel für virtuellen Computer Bereitstellungen in Ressourcenmanager vorgeschlagen werden. Informationen finden Sie unter [Verwenden der Azure CLI Azure Ressourcenmanager](../articles/xplat-cli-azure-resource-manager.md) und [Bereitstellen und Verwalten von virtuellen Computern mithilfe von Azure Ressourcenmanager Vorlagen und Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md).

Aufgabe | Ressourcenmanager
-------------- | ----------- | -------------------------
Erstellen Sie den grundlegendsten virtueller Computer | `azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Erhalten die `image-urn` aus der `azure vm image list` Befehl. Finden Sie [in diesem Artikel](../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md) Beispiele.)
Erstellen eines Linux virtuellen Computers | `azure  vm create [options] <resource-group> <name> <location> -y "Linux"`
Erstellen eines Windows virtueller Computer | `azure  vm create [options] <resource-group> <name> <location> -y "Windows"`
Liste virtuellen Computern | `azure  vm list [options]`
Erhalten von Informationen zu eines virtuellen Computers | `azure  vm show [options] <resource_group> <name>`
Starten eines virtuellen Computers | `azure vm start [options] <resource_group> <name>`
Beenden eines virtuellen Computers | `azure vm stop [options] <resource_group> <name>`
Freigeben eines virtuellen Computers | `azure vm deallocate [options] <resource-group> <name>`
Starten eines virtuellen Computers | `azure vm restart [options] <resource_group> <name>`
Löschen eines virtuellen Computers | `azure vm delete [options] <resource_group> <name>`
Erfassen eines virtuellen Computers | `azure vm capture [options] <resource_group> <name>`
Erstellen eines virtuellen Computers aus einem Benutzer Bild | `azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>`
Erstellen eines virtuellen Computers von einem speziellen Datenträger | `azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>`
Fügen Sie einen Datenträger hinzu eines virtuellen Computers | `azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]`
Entfernen Sie einen Datenträger eines virtuellen Computers | `azure  vm disk detach [options] <resource-group> <vm-name> <lun>`
Fügen Sie eine generische Erweiterung hinzu eines virtuellen Computers |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>`
Fügen Sie Zugriff virtueller Computer Erweiterung hinzu eines virtuellen Computers | `azure vm reset-access [options] <resource-group> <name>`
Hinzufügen von Docker Erweiterung zu eines virtuellen Computers | `azure  vm docker create [options] <resource-group> <name> <location> <os-type>`
Entfernen einer Erweiterung virtueller Computer | `azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>`
Verwendung der virtuellen Computer Ressourcen abrufen | `azure vm list-usage [options] <location>`
Alle verfügbare virtuellen Computer Größen abrufen | `azure vm sizes [options]`


## <a name="next-steps"></a>Nächste Schritte

* Weitere Beispiele für die CLI-Befehle, die jenseits grundlegende Verwaltung von virtuellen Computern finden Sie unter [Verwenden der Azure CLI Azure Ressourcenmanager](../articles/virtual-machines/azure-cli-arm-commands.md).

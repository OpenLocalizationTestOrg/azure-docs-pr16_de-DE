


Dieser Artikel wird beschrieben, wie Sie:

- Fügen Sie Daten in einer Azure-virtuellen Computern (virtueller Computer), wenn es bereitgestellt wird.

- Rufen sie für Windows und Linux.

- Verwenden Sie spezielle Tools zur Verfügung bei einigen Systemen zu erkennen und benutzerdefinierte Daten automatisch zu behandeln.

> [AZURE.NOTE] Dieser Artikel beschreibt, wie benutzerdefinierte Daten mithilfe eines virtuellen Computers erstellt, mit der Azure Service Management-API eingefügt werden können. Informationen zum Verwenden der Azure Ressourcen Management-API finden Sie unter [der Beispielvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata)finden Sie unter.

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Einfügen von benutzerdefinierten Daten in Ihrer Azure-virtuellen Computern

Dieses Feature ist nur in der [Azure Line Schnittstelle](https://github.com/Azure/azure-xplat-cli)derzeit unterstützt. Hier erstellen wir eine `custom-data.txt` Datei, die unsere Daten enthält, und fügen Sie, die in auf den virtuellen Computer während der Bereitstellung. Zwar können Sie eine der Optionen für möglicherweise die `azure vm create` Befehl die folgenden veranschaulicht eine sehr einfache Möglichkeit:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Verwenden von benutzerdefinierten Daten des virtuellen Computers

+ Wenn Ihre Azure-virtuellen Computer ein Windows-basiertem virtueller Computer ist, und klicken Sie dann in die benutzerdefinierten Datendatei gespeichert wird `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Obwohl es base64-codierte aus dem lokalen Computer in den neuen virtuellen Computer übertragen wurde, möglich wird automatisch decodiert und geöffnet oder sofort verwendet.

   > [AZURE.NOTE] Wenn die Datei vorhanden ist, wird sie überschrieben. Die Sicherheit auf Verzeichnis wird auf **System: Vollzugriff** und **Administratoren: Vollzugriff**festgelegt.

+ Ist der Azure-virtuellen Computer einen virtuellen Linux-basierten, wird die benutzerdefinierten Datendatei in eine der folgenden Stellen je nach der Distro befinden. Die Daten sein base64-codierte, damit Sie müssen zuerst die Daten entschlüsseln können:

    - `/var/lib/waagent/ovf-env.xml`
    - `/var/lib/waagent/CustomData`
    - `/var/lib/cloud/instance/user-data.txt` 



## <a name="cloud-init-on-azure"></a>Klicken Sie auf Azure der Cloud Initialisierung

Ist Ihre Azure-virtuellen Computer von einem Bild Ubuntu oder CoreOS, können Sie CustomData verwenden, um einen Cloud-Config an Cloud der Initialisierung zu senden. Oder ist die benutzerdefinierten Datendatei ein Skript, dann Cloud der Initialisierung kann einfach ausgeführt wird.

### <a name="ubuntu-cloud-images"></a>Ubuntu Cloud Bilder

In den meisten Azure Linux Bilder, bearbeiten Sie "/ etc/waagent.conf" Konfigurieren Sie die Festplatte temporäre Ressource und Datei austauschen. Weitere Informationen finden Sie unter [Azure Linux Agent-Benutzerhandbuch](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) .

Klicken Sie auf die Ubuntu Cloud Bilder, Sie jedoch müssen verwenden Cloud der Initialisierung so konfigurieren Sie den Ressource Datenträger (d. h., die "temporärer" Datenträger) und Partition austauschen. Auf dem Ubuntu-Wiki Weitere Informationen hierzu finden Sie in der folgenden Seite: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Nächste Schritte: Verwenden der Initialisierung Cloud

Weitere Informationen finden Sie in der [Cloud der Initialisierung der Dokumentation Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Hinzufügen von Rolle Service Management REST-API-Referenz](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure Line-Benutzeroberfläche](https://github.com/Azure/azure-xplat-cli)


1. Melden Sie sich bei Ihrem Azure-Abonnement mit den Schritten in [Verbindung mit Azure über die Befehlszeile Azure herstellen](../articles/xplat-cli-connect.md).

2. Vergewissern Sie sich, dass Sie mit der Bereitstellung klassischen Modus aufweisen:

        azure config mode asm

3. Informieren Sie sich das Linux-Bild, das Sie aus der verfügbaren Bilder laden möchten.

        azure vm image list | grep "Linux"

   Verwenden Sie in einem Windows-Eingabeaufforderungsfenster anstelle von Grep **Suchen** .

4. Verwenden Sie `azure vm create` zum Erstellen eines neuen virtuellen Computers mit dem Bild Linux aus der vorherigen Liste aus. Dieser Schritt erstellt ein neues Cloud-Dienst und Speicher-Konto an. Sie können auch dieses virtuellen Computers Herstellen einer Verbindung mit einem vorhandenen Clouddienst mit einem `-c` Option. Er erstellt auch einen Endpunkt SSH zum Anmelden bei der Linux virtuellen Computern mit der `-e` Option.

        ~$ azure vm create "MyTestVM" b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB -g adminUser -p P@ssw0rd! -z "Small" -e -l "West US"
        info:    Executing command vm create
        + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
        + Looking up cloud service
        info:    cloud service MyTestVM not found.
        + Creating cloud service
        + Retrieving storage accounts
        + Creating VM
        info:    vm create command OK

    >[AZURE.NOTE] Für eine Linux virtuellen Computern, geben Sie die `-e` option in `vm create`. Es ist nicht möglich, SSH nach dem Erstellen des virtuellen Computers zu aktivieren. Detaillierte Informationen zur SSH, [So verwenden Sie SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md)lesen.

    Das Bild *b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB* ist das Element, das wir aus der Bildliste im vorherigen Schritt ausgewählt haben. *MyTestVM* ist der Name des unserer neuen virtuellen Computern und *AdminUser* ist der Benutzername SSH in des virtuellen Computers. Sie können diese Variablen gemäß Ihrer Anforderung ersetzen. Besuchen Sie für Weitere Informationen zu diesen Befehl die [Verwendung der Azure CLI mit klassischen Bereitstellungsmodell](virtual-machines-command-line-tools.md)aus.

5. Der neu erstellten Linux virtuellen Computern wird in der Liste angegebenen durch:

        azure vm list

6. Sie können die Attribute des virtuellen Computers mithilfe des Befehls überprüfen:

        azure vm show MyTestVM

7. Der neu erstellten virtuellen Computern bereitsteht zunächst die `azure vm start` Befehl.

## <a name="next-steps"></a>Nächste Schritte
Details auf alle Befehle diese CLI Azure-virtuellen Computern lesen Sie die [Verwendung der Azure CLI mit API für die klassischen Bereitstellung](../articles/virtual-machines-command-line-tools.md)aus.
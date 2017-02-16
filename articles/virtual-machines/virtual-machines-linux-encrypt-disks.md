<properties
   pageTitle="Verschlüsseln Datenträger auf einer Linux VM | Microsoft Azure"
   description="So verschlüsseln Datenträger auf einer Linux VM mithilfe der Azure CLI und das Modell zur Bereitstellung von Ressourcenmanager"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Verschlüsseln von Datenträger auf einer Linux VM mithilfe der Azure-CLI
Für erweiterte virtuellen Computern (virtueller Computer) Sicherheit und Einhaltung von Vorschriften können virtuelle Laufwerke in Azure statisch verschlüsselt werden. Datenträger verschlüsselt sind, mithilfe von cryptographic Keys, die auf einer Azure-Taste Tresor gesichert werden. Sie steuern diese cryptographic Schlüssel und deren Verwendung überwachen können. In diesem Artikel ausführlich erläutert, wie virtuelle Laufwerke auf einer Linux VM mithilfe der Azure CLI und das Modell zur Bereitstellung von Ressourcenmanager verschlüsseln.


## <a name="quick-commands"></a>Symbolleiste Befehle
Wenn Sie schnell die Aufgabe, die folgenden Abschnitt Details die Basis ausführen müssen Befehle zum Verschlüsseln der virtuellen Laufwerke Ihrer virtuellen Computers. Ausführlichere Informationen und den Kontext für die einzelnen Schritte können im weiteren Verlauf des Dokuments, [beginnen Sie hier](#overview-of-disk-encryption)gefunden werden.

Sie benötigen die [Neuesten Azure CLI](../xplat-cli-install.md) installiert und angemeldet mit den Ressourcenmanager Modus wie folgt aus:

```
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `myKeyVault`, und `myVM`.

Zunächst aktivieren Sie den Azure-Taste Tresor Anbieter innerhalb Ihres Abonnements Azure, und erstellen Sie eine Ressourcengruppe. Im folgenden Beispiel wird eine Gruppe Ressourcenname `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Erstellen einer Azure Key Tresor. Im folgenden Beispiel wird ein Schlüssel Tresor mit dem Namen `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Erstellen Sie einen cryptographic Schlüssel in Ihrem Tresor-Taste, und für die Verschlüsselung der Datenträger aktivieren. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Erstellen Sie einen Endpunkt Azure Active Directory für Umgang mit der Authentifizierung sowie den Austausch von cryptographic Tasten aus Tresor Schlüssel verwenden. Die `--home-page` und `--identifier-uris` tatsächliche geroutet Adresse werden nicht mehr. Für die höchste Sicherheitsstufe sollte Client Kennwörter anstelle von Kennwörtern verwendet werden. Die CLI Azure kann nicht aktuell geheimen Schlüssel Clients generieren. Geheimen Schlüssel Clients können nur im Portal Azure generiert werden. Im folgenden Beispiel wird einen Azure Active Directory-Endpunkt mit dem Namen `myAADApp` und dem Kennwort verwendet `myPassword`. Geben Sie Ihr eigenes Kennwort wie folgt ein:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Hinweis Die `applicationId` in der Ausgabe des voranstehenden Befehls angezeigt. Diese Anwendung-ID wird in den folgenden Schritten verwendet:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Fügen Sie einen Datenträger zu einem vorhandenen virtuellen Computer an. Im folgende Beispiel fügt einen Datenträger eines virtuellen Computers mit dem Namen `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Überprüfen Sie die Details für den Tresor-Taste und die Taste, die Sie erstellt haben. Benötigen Sie die Taste Tresor-ID, die URI und die Taste URL im letzten Schritt. Im folgende Beispiel prüft die Details für ein Schlüssel Tresor mit dem Namen `myKeyVault` und Schlüssel mit dem Namen `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Ihrer Datenträger eingeben Ihrer eigenen Parameternamen in der gesamten wie folgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Die CLI Azure zur ausführliche Fehler bei der Verschlüsselung nicht Verfügung. Weitere Informationen zur Problembehandlung finden Sie in `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Wie der vorherige Befehl viele Variablen weist und erhalten Sie möglicherweise keine auf viel Angabe anderem Ort tätig Antwort, warum schlägt der Prozess fehl, würde ein Beispiel vollständigen Befehl wie folgt aussehen:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Überprüfen Sie schließlich den Verschlüsselungsstatus erneut aus, um zu bestätigen, dass Ihre virtuellen Laufwerke nun verschlüsselt wurden. Im folgende Beispiel wird der Status eines virtuellen Computers mit dem Namen überprüft `myVM` in der `myResourceGroup` Ressourcengruppe:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Übersicht über Datenträger-Verschlüsselung
Virtuelle Laufwerke auf Linux virtuellen Computern verschlüsselt sind bei Rest [dm-Crypt](https://wikipedia.org/wiki/Dm-crypt)verwenden. Es gibt keine Gebühr für die Verschlüsselung von virtuellen Laufwerken in Azure. Cryptographic Tasten in Azure-Taste Tresor Software-Schutz mit gespeichert sind, oder Sie importieren oder die Tasten in Hardware Security Module (HSMs) zertifiziert, FIPS 140-2 Ebene 2 Standards generieren können. Sie haben weiterhin Kontrolle über diese cryptographic Schlüssel und deren Verwendung überwachen können. Verschlüsseln und Entschlüsseln mit Ihrer virtuellen Computer verbundenen virtuellen Datenträger, werden diese cryptographic Schlüssel verwendet. Ein Endpunkt Azure Active Directory bietet secure dafür, diese cryptographic Schlüssel ausgeben, wie virtuelle Computer eingeschaltet ist und deaktiviert werden.

Der Vorgang zum Verschlüsseln eines virtuellen Computers sieht wie folgt aus:

1. Erstellen Sie einen cryptographic Key in einer Azure-Taste Tresor.
2. Konfigurieren Sie die Taste cryptographic, um zum Verschlüsseln von Datenträger verwendet werden.
3. Wenn die Taste cryptographic aus dem Azure-Taste Tresor lesen möchten, erstellen Sie einen Endpunkt Azure Active Directory mit den entsprechenden Berechtigungen verwenden.
4. Geben Sie den Befehl zum Verschlüsseln der virtuelle Laufwerke mit Angabe von Azure Active Directory-Endpunkt und entsprechende cryptographic Key verwendet werden.
5. Der Endpunkt Azure Active Directory fordert den erforderlichen cryptographic Schlüssel aus Azure-Taste Tresor.
6. Die virtuellen Laufwerke verschlüsselt sind, verwenden die bereitgestellte cryptographic-Taste.


## <a name="supporting-services-and-encryption-process"></a>Unterstützung von Diensten und Verschlüsselungsprozess
Verschlüsselung Festplatten beruht auf die folgenden zusätzlichen Komponenten:

- **Azure Schlüssel Tresor** - verwendet, um die Tasten cryptographic schützen und Kennwörter für den Datenträger Verschlüsselung/entschlüsseln Prozess verwendet. 
  - Sofern eines vorhanden ist, können Sie eine vorhandene Azure-Taste Tresor verwenden. Sie müssen kein Schlüssel Tresor reserviert sein soll, mit der Verschlüsselung von Festplatten.
  - Um administrative Grenzen und Key Sichtbarkeit zu trennen, können Sie eine dedizierte Schlüssel Tresor erstellen.
- **Azure Active Directory** - behandelt den sicheren Austausch von erforderlichen cryptographic Schlüssel und Authentifizierung für angeforderten Aktionen. 
  - Sie können eine vorhandene Azure Active Directory-Instanz in der Regel für eine Anwendung mit verwenden. 
  - Die Anwendung ist mehr von außen liegenden Tabellenblättern für die Taste Tresor und virtuellen Computern Dienste anfordern und erhalten die entsprechenden cryptographic Schlüssel ausgestellt. Entwickeln Sie nicht ist-Anwendung, die mit Azure Active Directory integriert.


## <a name="requirements-and-limitations"></a>Anforderungen und Einschränkungen
Unterstützte Szenarien und Anforderungen für die Verschlüsselung der Datenträger:

- Die folgenden Linux Server SKUs - Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES) und Red Hat Enterprise Linux.
- Alle Ressourcen (z. B. Schlüssel Tresor, Speicher-Konto und virtueller Computer) muss in der gleichen Azure Region und Abonnements.
- Standard-A, D, DS, G und GS Reihe virtuellen Computern.

Verschlüsselung Festplatten wird in den folgenden Szenarien derzeit nicht unterstützt:

- Grundlegende Ebene virtuellen Computern.
- Virtuelle Computer mit dem Modell zur Bereitstellung von klassischen erstellt.
- Deaktivieren von OS Datenträger Verschlüsselung auf Linux virtuellen Computern aus.
- Aktualisieren die cryptographic Tasten auf eine bereits verschlüsselte Linux VM.


## <a name="create-the-azure-key-vault-and-keys"></a>Erstellen der Azure-Taste Tresor und Schlüssel
Wenn die Division von diesem Leitfaden abgeschlossen haben, benötigen Sie die [Neuesten Azure CLI](../xplat-cli-install.md) installiert und angemeldet mit den Ressourcenmanager Modus wie folgt aus:

```
azure config mode arm
```

Ersetzen Sie in den Befehlsbeispielen alle Beispiel Parameter durch eigene Namen, Speicherort und Schlüsselwerte ein. Die folgenden Beispiele verwenden eine Messe von `myResourceGroup`, `myKeyVault`, `myAADApp`usw..

Der erste Schritt besteht im Erstellen einer Azure-Taste Tresor zum Speichern Ihrer cryptographic Tasten. Tasten, Schlüssel oder Kennwörter, mit denen Sie sicher in Ihre Anwendungen und Dienste implementiert wird, können Azure-Taste Tresor speichern. Für die Verschlüsselung der virtuelle Laufwerk verwenden Sie Schlüssel Tresor einen cryptographic Key gespeichert, der zum Verschlüsseln oder entschlüsseln Ihrer virtuellen Laufwerke verwendet wird. 

Aktivieren Sie den Anbieter Azure-Taste Tresor in Ihrem Abonnement Azure, und dann erstellen Sie eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Der Azure-Taste Tresor, enthält die cryptographic Schlüssel und die zugehörigen berechnen von Ressourcen wie Speicher und die virtuellen Computer selbst muss sich in derselben Region befinden. Im folgenden Beispiel wird eine Azure-Taste Tresor mit dem Namen `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Sie können cryptographic Tasten mit Software oder Hardware Security Model (HSM) Schutz speichern. Verwenden eine HSM erfordert eine Premium-Taste Tresor. Es gibt eine Mehrkosten für das Erstellen einer Premium Schlüssel Tresor statt der standard-Taste Tresor, in der Software geschützt Schlüssel gespeichert. Fügen Sie zum Erstellen einer Premium Schlüssel Tresor im vorherigen Schritt `--sku Premium` zum Befehl. Im folgenden Beispiel wird die Tasten Software geschützt, da wir eine standard-Taste Tresor erstellt haben. 

Für beide Modelle Schutz muss die Azure-Plattform die cryptographic Schlüssel anfordern, wenn die virtuellen Laufwerke entschlüsseln startet der virtuellen Computer Zugriff gewährt. Erstellen eines Verschlüsselungsschlüssels in Ihrem Tresor-Taste, und klicken Sie dann Aktivieren der Arbeitsmappe für die Verwendung mit Verschlüsselung virtuelle Festplatten. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myKey` und anschließend für die Verschlüsselung der Datenträger wird aktiviert:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Erstellen der Azure-Active Directory-Anwendungs
Wenn virtuelle Laufwerke verschlüsselt oder entschlüsselt werden, verwenden Sie einen Endpunkt, die Authentifizierung und den Austausch von cryptographic Tasten aus Tresor Schlüssel behandeln. Diesen Endpunkt, eine Anwendung Azure Active Directory ermöglicht die Azure-Plattform so fordern Sie die entsprechenden cryptographic Schlüssel für den virtuellen Computer an. Eine Standardinstanz Azure Active Directory steht in Ihrem Abonnement, obwohl viele Organisationen Azure Active Directory Verzeichnisse dedizierte verfügen.

Wie Sie eine vollständige Azure Active Directory-Anwendung nicht erstellen, werden die `--home-page` und `--identifier-uris` Parameter im folgenden Beispiel muss nicht tatsächliche geroutet Adresse sein. Im folgende Beispiel gibt auch ein Kennwort-basierten Geheimnis statt Generieren von Tastenkombinationen aus dem Azure-Portal. Zum Generieren Schlüssel als diesmal kann nicht über die Befehlszeile Azure erfolgen. 

Erstellen Sie eine Anwendung Azure Active Directory. Im folgenden Beispiel wird eine Anwendung mit dem Namen `myAADApp` und dem Kennwort verwendet `myPassword`. Geben Sie Ihr eigenes Kennwort wie folgt ein:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Notieren Sie die `applicationId` in der Ausgabe des voranstehenden Befehls zurückgegeben wird. Diese Anwendung-ID wird in einige der verbleibenden Schritte verwendet. Erstellen Sie anschließend einen Dienst (Dienstprinzipalname) so, dass die Anwendung in Ihrer Umgebung zugegriffen werden kann. Um erfolgreich verschlüsseln oder entschlüsseln virtuelle Laufwerke, müssen Berechtigungen für den cryptographic Schlüssel Schlüssel Tresor gehörende Kehrmatrix um zuzulassen, dass die Anwendung Azure Active Directory, lesen die Tasten festgelegt werden. 

Erstellen Sie den SPN, und legen Sie die geeigneten Berechtigungen wie folgt aus:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Hinzufügen eines virtuellen Datenträgers und Prüfstatus Verschlüsselung
So verschlüsseln Sie einige virtuelle Laufwerke, tatsächlich einen Datenträger zu einem vorhandenen virtuellen Computer hinzufügen können. Fügen Sie einen Datenträger 5Gb zu einer vorhandenen virtuellen Computer wie folgt ein:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Die virtuellen Laufwerke werden derzeit nicht verschlüsselt. Überprüfen Sie den aktuellen Verschlüsselungsstatus Ihrer virtuellen Computer wie folgt ein:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Verschlüsseln von virtuellen Laufwerken
Jetzt die virtuellen Laufwerke Verschlüsselung, kombinieren Sie alle vorherigen Komponenten:

1. Geben Sie die Anwendung Azure Active Directory und das Kennwort ein.
2. Geben Sie die Taste Tresor zum Speichern der Metadaten für Ihre verschlüsselte Datenträger an.
3. Geben Sie die cryptographic Schlüssel für die tatsächliche Verschlüsselung und Entschlüsseln verwendet werden soll.
4. Geben Sie an, ob der Datenträger OS, Festplatten mit den Daten oder alle verschlüsseln werden soll.

Überprüfen Sie die Details für den Azure-Tresor-Taste und die Taste, die Sie erstellt haben, wie Sie die Taste Tresor-ID, URI benötigen, und klicken Sie dann URL im letzten Schritt wichtiger, die bietet folgende Möglichkeiten:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Ihre virtuellen Laufwerke unter Verwendung der Ausgabe von der `azure keyvault show` und `azure keyvault key show` Befehle wie folgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Wie der vorherige Befehl viele Variablen hat, wird im folgenden Beispiel wird der vollständige Befehl zu Referenzzwecken:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Die CLI Azure zur ausführliche Fehler bei der Verschlüsselung nicht Verfügung. Weitere Informationen zur Problembehandlung überprüfen `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` des virtuellen Computers sind Sie verschlüsseln.

Schließlich können Sie die Verschlüsselung Prüfstatus erneut aus, um zu bestätigen, dass Ihre virtuellen Laufwerke nun verschlüsselt wurden:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Fügen Sie weitere Daten Datenträger hinzu
Nachdem Sie Ihre Daten Datenträger verschlüsselt haben, können Sie später weitere virtuelle Laufwerke zu Ihrem virtuellen Computer hinzufügen und diese auch verschlüsseln. Beim Ausführen der `azure vm enable-disk-encryption` Befehl, Erhöhen der Sequenz Version mithilfe der `--sequence-version` Parameter. Für diesen Parameter der Sequenz-Version können Sie wiederholten Vorgänge auf dem gleichen virtuellen Computer ausführen.

Beispielsweise können Ihre virtuellen Computer eine zweite virtuelle Festplatte wie folgt hinzufügen:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Führen Sie den Befehl zum Verschlüsseln der virtuellen Laufwerke erneut aus dieser, die zum Hinzufügen der `--sequence-version` Parameter, und erhöhen den Wert von unserem ersten ausführen wie folgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Verwalten von Azure-Taste Tresor, einschließlich löschen cryptographic Tasten und Depots finden Sie unter [Verwalten von Key Tresor verwenden CLI](../key-vault/key-vault-manage-with-cli.md).
- Weitere Informationen zu Verschlüsselung Festplatten, wie beispielsweise die Vorbereitung einer verschlüsselten benutzerdefinierten virtuellen Computer hochladen in Azure, finden Sie unter [Azure Datenträger Verschlüsselung](../security/azure-security-disk-encryption.md).
<properties
    pageTitle="Bereitstellen von Azure Stapel Prüfung des Konzepts ist | Microsoft Azure"
    description="Informationen Sie zum Vorbereiten der Azure Stapel Prüfung des Konzepts ist, und führen Sie das Skript PowerShell zum Bereitstellen von Azure Stapel Prüfung des Konzepts ist."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Bereitstellen von Azure Stapel Prüfung des Konzepts ist
Um die Azure Stapel Prüfung des Konzepts ist bereitstellen zu können, müssen Sie zuerst [Vorbereiten des Computers Bereitstellung](#prepare-the-deployment-machine) , und klicken Sie dann auf [das Bereitstellungsskript PowerShell ausgeführt](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Herunterladen Sie und extrahieren Sie Microsoft Azure Stapel Prüfung des Konzepts ist TP2

Bevor Sie beginnen, stellen sicher, dass Sie mindestens 85 GB Speicherplatz.

1. Das Herunterladen von Azure Stapel Prüfung des Konzepts ist TP2 besteht eine Zip-Datei mit den folgenden 12-Dateien, insgesamt ~ 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Überprüfen Sie im Bildschirm Lizenzvertrag und die Informationen des Assistenten Self extrahieren, und klicken Sie dann auf **Weiter**.
3. Überprüfen Sie die Datenschutzbestimmungen Bildschirm und die Informationen des Assistenten Self extrahieren, und klicken Sie dann auf **Weiter**.
4. Wählen Sie das Ziel für die Dateien extrahiert werden, klicken Sie auf **Weiter**.
    - Die Standardeinstellung ist: <drive letter>:\<aktuellen Ordner > \Microsoft Azure Stapel Prüfung des Konzepts ist
5. Überprüfen Sie die Ziel-Speicherort Bildschirm und die Informationen des Assistenten Self extrahieren, und klicken Sie dann auf **extrahieren** , um die CloudBuilder.vhdx (~44.5 GB) und ThirdPartyLicenses.rtf-Dateien zu extrahieren.

> [AZURE.NOTE] Nachdem Sie die Dateien zu extrahieren, können Sie die Zip-Datei, um Speicherplatz auf dem Computer wieder löschen. Alternativ können Sie Zip-Datei in einen anderen Ort, sodass, sollten Sie Sie erneut bereitstellen, müssen die Zip-Dateien erneut herunterladen müssen nicht verschieben.

## <a name="prepare-the-deployment-machine"></a>Vorbereiten des Computers für Bereitstellung

1. Stellen Sie sicher, dass Sie können physisch auf den Computer für die Bereitstellung verbinden oder physischen Konsole (wie z. B. KVM zugreifen). Sie benötigen solche nach einem des Computers Bereitstellung in Schritt 9 Neustart.

2. Stellen Sie sicher, dass der Computer für die Bereitstellung die [Mindestanforderungen](azure-stack-deploy.md)erfüllt. Die [Bereitstellung der Rechtschreibprüfung für Azure Stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) können Sie um Ihren Anforderungen zu bestätigen.

3. Melden Sie sich als lokaler Administrator an Ihrem Computer Prüfung des Konzepts ist.

4. Kopieren Sie die Datei CloudBuilder.vhdx in C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Wenn Sie nicht das empfohlene Skript zum Vorbereiten des Computers Host Prüfung des Konzepts ist (Schritt 5 – Schritt 7) verwenden, geben Sie eine beliebige Lizenz-Taste auf der Aktivierungsseite nicht. Ist eine Testversion von Windows Server 2016 Bild enthalten, und Ablauf Warnung Nachrichten versucht einen License Product Key eingeben.

5. Führen Sie auf dem Computer Prüfung des Konzepts ist das folgende PowerShell-Skript zum Herunterladen der Azure Stapel TP2 Support-Dateien aus:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Dieses Skript downloads Azure Stapel TP2-Support-Dateien in den Ordner, indem Sie den Parameter $LocalPath angegeben.
    
6. Öffnen Sie eine erhöhte PowerShell-Konsole, und wechseln Sie zu der Stelle, an der Sie die Dateien kopiert haben.
    - 11 MicrosoftAzureStackPOC-N.BIN (N ist 1-11)
7. Mit der rechten Maustaste auf die MicrosoftAzureStackPOC.EXE > als Administrator ausführen.

8. Führen Sie das Skript PrepareBootFromVHD.ps1. Dieses Skript und die Dateien für unbeaufsichtigtes sind mit anderen Support bereitgestellten Skripts zusammen mit diesem Build verfügbar.
    Es gibt fünf Parameter für dieses Skript PowerShell aus:
    - CloudBuilderDiskPath (erforderlich) – den Pfad für die CloudBuilder.vhdx auf dem HOST.
    - (Optional) – DriverPath können Sie zusätzliche Treiber für den Host in der virtuellen HD hinzufügen.
    - ApplyUnattend (optional) – Geben Sie diesen Parameter wechseln, um die Konfiguration des Betriebssystems automatisieren. Wenn angegeben, muss der Benutzer bereitstellen, die AdminPassword, um das Betriebssystem beim Start zu konfigurieren (sofern öffentlichem unattend_NoKVM.xml Datei erfordert).
    Wenn Sie nicht diesen Parameter verwenden, wird die Datei unattend.xml generische ohne weiteren Anpassung verwendet. Sie benötigen KVM abgeschlossen-Anpassung nach dem Neustart.
    - AdminPassword (optional) – nur verwendet, wenn der Parameter ApplyUnattend festgelegt ist, muss mindestens sechs Zeichen.
    - VHDLanguage (optional) – gibt die virtuelle Festplatte Sprache, die standardmäßig auf "En-US" an.
    Das Skript wird beschrieben, und enthält Beispiel-Verwendung, obwohl die am häufigsten verwendet wird:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Wenn Sie diesen genauen Befehl ausführen, müssen Sie AdminPassword an der Eingabeaufforderung eingeben.

9. Wenn das Skript abgeschlossen ist, müssen Sie den Neustart zu bestätigen. Wenn andere Benutzer angemeldet sind, schlägt dieser Befehl fehl. Wenn der Befehl fehlschlägt, führen Sie den folgenden Befehl aus:`Restart-Computer -force` 

10. Dem Neustart des Servers in das Betriebssystem von der CloudBuilder.vhdx, wo die Bereitstellung weiterhin.

> [AZURE.IMPORTANT] Azure Stapel erfordert Zugriff auf das Internet, direkt oder durch einen transparenten Proxy. Die Bereitstellung TP2 Prüfung des Konzepts ist unterstützt genau ein NIC Netzwerke. Wenn Sie mehrere Netzwerkkarten haben, stellen Sie sicher, dass nur von einem aktiviert ist (und alle anderen deaktiviert sind), bevor Sie das Bereitstellungsskript im nächsten Abschnitt ausführen.

## <a name="run-the-powershell-deployment-script"></a>Führen Sie das Bereitstellungsskript PowerShell

1. Melden Sie sich als lokaler Administrator an Ihrem Computer Prüfung des Konzepts ist. Verwenden Sie die Anmeldeinformationen, die in den vorherigen Schritten angegeben.

2. Öffnen einer erhöhten PowerShell-Konsole an.

3. Führen Sie in der PowerShell diesen Befehl aus:`cd C:\CloudDeployment\Configuration`

4. Führen Sie den Bereitstellungsbefehl:`.\InstallAzureStackPOC.ps1`

5. **Eingabe des Kennworts** aufgefordert werden Geben Sie ein Kennwort ein, und bestätigen Sie es. Dies ist das Kennwort mit den virtuellen Computern. Achten Sie darauf, dass die Aufzeichnung.

6. Geben Sie die Anmeldeinformationen für Ihr Konto Azure Active Directory. Diese Benutzer müssen dem globalen Administrator im Verzeichnis Mandanten.

7. Die Bereitstellung kann ein paar Stunden dauern während, die Neustart des Systems automatisch einmal.

    > [AZURE.IMPORTANT] Wenn Sie den Fortschritt der Bereitstellung überwachen möchten, melden Sie sich als Azurestack\AzureStackAdmin. Wenn Sie als lokalen Administrator melden Sie sich nach der Computer die Domäne hinzugefügt wird, wird den Fortschritt der Bereitstellung nicht angezeigt. Führen Sie die Bereitstellung erneut aus, stattdessen melden Sie sich als Azurestack\AzureStackAdmin zu überprüfen, ob er läuft nicht.

    Wenn die Bereitstellung erfolgreich ist, zeigt die PowerShell-Konsole: **abgeschlossen: Aktion 'Bereitstellung'**.

    Wenn die Bereitstellung fehlschlägt, versuchen Sie zu [erneut ausführen zu müssen](azure-stack-rerun-deploy.md). Alternativ können Sie [erneut bereitstellen](azure-stack-redeploy.md) von Grund auf neu.

### <a name="deployment-script-examples"></a>Beispiele für die Bereitstellung Skripts

Ihre Identität AAD ist nur eine AAD Verzeichnis zugeordnet:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Wenn Ihre Identität AAD größer als eine AAD Verzeichnis zugeordnet ist:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Wenn Ihre Umgebung DHCP aktiviert hat, müssen Sie die folgenden zusätzlichen Parameter auf eine der Optionen oben (Beispiel: Einsatz bereitgestellten) einschließen:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Optionale Parameter InstallAzureStackPOC.ps1

| Parameter | Erforderlich/Optional | Beschreibung |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Optional | Legt die Azure Active Directory-Benutzernamen und Ihr Kennwort ein. Diese Anmeldeinformationen Azure können entweder ein Organisations-ID oder einem Microsoft-Account sein. Wenn Microsoft Account Anmeldeinformationen verwenden möchten, lassen Sie für diesen Parameter in das Cmdlet Weg. Für diesen Parameter auslassen fordert das Popup Azure-Authentifizierung während der Bereitstellung. Dadurch wird die Anmelde- und aktualisieren Token verwendet während der Bereitstellung erstellt. |
| AADDirectoryTenantName | Erforderlich | Legt das Verzeichnis Mandanten. Verwenden Sie diesen Parameter ein bestimmtes Verzeichnis angeben, in dem das Konto AAD Berechtigungen zum Verwalten mehrerer Verzeichnisse hat. Vollständiger Name von einem Mandanten AAD Verzeichnis, in das Format des <directoryName>. onmicrosoft.com. |
| AdminPassword | Erforderlich | Legt das lokale Administratorkonto und alle anderen Benutzerkonten auf allen virtuellen Computern, die im Rahmen der Prüfung des Konzepts ist Bereitstellung erstellt wurden. Dieses Kennwort muss auf dem Host des aktuellen lokalen Administratorkennworts übereinstimmen. |
| AzureEnvironment | Optional | Wählen Sie aus der Azure-Umgebung, mit denen Sie diese Stapel Azure-Bereitstellung erfassen möchten. Die Optionen umfassen *Öffentlichen Azure*, *Azure - China*, *Azure - US-Regierung*. |
| EnvironmentDNS | Optional | Ein DNS-Server wird als Teil der Bereitstellung Azure Stapel erstellt. Damit Computer innerhalb der Lösung zum Auflösen von Namen außerhalb der Stempel können, Ihren vorhandenen Infrastruktur DNS-Server zur Verfügung. Der in der letzten Bearbeitung DNS-Server leitet unbekannten Namen mit einer Auflösung von Besprechungsanfragen auf diesem Server. |
| NatIPv4Address | Für die Unterstützung von DHCP NAT erforderlich | Legt eine statische IP-Adresse für MAS-BGPNAT01 an. Verwenden Sie diesen Parameter nur, wenn DHCP nicht zuordnen eine gültige IP-Adresse auf das Internet zugreifen kann. |
| NatIPv4DefaultGateway | Für die Unterstützung von DHCP NAT erforderlich | Legt das Standard-Gateway mit der statischen IP-Adresse für MAS-BGPNAT01 verwendet. Verwenden Sie diesen Parameter nur, wenn DHCP nicht zuordnen eine gültige IP-Adresse auf das Internet zugreifen kann.  |
| NatIPv4Subnet | Für die Unterstützung von DHCP NAT erforderlich | IP-Subnetz Präfix über NAT-Unterstützung für DHCP verwendet. Verwenden Sie diesen Parameter nur, wenn DHCP nicht zuordnen eine gültige IP-Adresse auf das Internet zugreifen kann.  |
| PublicVLan | Optional | Legt die VLAN-ID an. Verwenden Sie diesen Parameter nur, wenn die Host und MAS-BGPNAT01 VLAN-ID zum Zugreifen auf das physische Netzwerk (und Internet) konfiguriert werden müssen. Beispielsweise`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Erneut ausführen | Optional | Verwenden Sie diese Kennzeichnung Bereitstellung erneut ausgeführt.  Alle vorherigen Eingabe wird verwendet. Zuvor bereitgestellte erneut eingeben von Daten werden nicht unterstützt, da verschiedene eindeutige Werte generiert und für die Bereitstellung verwendet werden. |
| TimeServer | Optional | Verwenden Sie diesen Parameter, wenn Sie einen bestimmte Uhrzeit Server angegeben werden müssen. |

## <a name="next-steps"></a>Nächste Schritte

[Verbinden mit Azure Stapel](azure-stack-connect-azure-stack.md)

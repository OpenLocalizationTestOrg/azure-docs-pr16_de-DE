<properties
pageTitle="Aktivieren des Remote Desktop-Verbindung für eine Rolle in Azure Cloud Services mithilfe der PowerShell"
description="So konfigurieren Sie Ihre Azure-Cloud-Service-Anwendung mithilfe von PowerShell remote desktop-Verbindungen zulassen"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Aktivieren des Remote Desktop-Verbindung für eine Rolle in Azure Cloud Services mithilfe der PowerShell

>[AZURE.SELECTOR]
- [Azure klassischen-portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Remotedesktop können Sie auf den Desktop einer Rolle in Azure ausgeführt zugreifen. Eine Remote Desktop-Verbindung können zum Beheben und Diagnostizieren von Problemen mit der Anwendung während der Ausführung.

Dieser Artikel beschreibt, wie Sie Ihre Cloud-Dienstverwaltungsrollen mithilfe der PowerShell remote Desktop aktivieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel erforderlich. PowerShell verwendet die Remote Desktop-Erweiterung, sodass Sie Remotedesktop aktivieren können, nachdem die Anwendung bereitgestellt wird.


## <a name="configure-remote-desktop-from-powershell"></a>Konfigurieren von PowerShell Remotedesktop

Das Cmdlet " [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) " können Sie Remote Desktop angegebenen Rollen oder aller Rollen von der Cloud-Service-Bereitstellung zu aktivieren. Das Cmdlet können Sie den Benutzernamen und das Kennwort des Benutzers für den remote desktop durch den *Credential* -Parameter angeben, die ein Objekt PSCredential akzeptiert.

Wenn Sie PowerShell interaktiv verwenden, können Sie das Objekt PSCredential einfach, indem Sie das Cmdlet " [Get-Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) " festlegen.

```
$remoteusercredentials = Get-Credential
```

Dieser Befehl zeigt das Dialogfeld ermöglicht es Ihnen, geben Sie den Benutzernamen und das Kennwort für den entfernten Benutzer auf sichere Weise.

Da PowerShell in Szenarios für die Automatisierung unterstützt, können Sie auch das Objekt **PSCredential** so einrichten, die keine Interaktion mit dem Benutzer erforderlich sind. Zuerst müssen Sie ein sicheres Kennwort einrichten. Sie beginnen mit einer nur-Text-Kennwort wandeln Sie es in eine sichere Zeichenfolge mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)angeben. Als Nächstes müssen Sie diese sichere Zeichenfolge in einer verschlüsselten standard Zeichenfolge mit [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)konvertieren. Jetzt können Sie diese verschlüsselte standard-Zeichenfolge mithilfe der [Set-Inhalte](https://technet.microsoft.com/library/ee176959.aspx)in eine Datei speichern.

Sie können auch eine Datei sicheres Kennwort erstellen, damit Sie nicht jedes Mal das Kennwort eingeben. Darüber hinaus ist eine sicheres Kennwortdatei besser als eine nur-Text-Datei. Verwenden Sie die folgende PowerShell, um eine Datei sicheres Kennwort erstellen:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Wenn Sie das Kennwort festlegen möchten, stellen Sie sicher, dass Sie die [Komplexität Anforderungen](https://technet.microsoft.com/library/cc786468.aspx)entsprechen.

Um das Anmeldeinformationsobjekt aus der Datei sicheres Kennwort erstellen, müssen Sie lesen den Inhalt der Datei und wieder in eine sichere Zeichenfolge mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)konvertiert werden.

Das Cmdlet " [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) " akzeptiert auch einen *Ablauf* -Parameter, bei dem **"DateTime"** gibt an, an dem das Benutzerkonto abläuft. Beispielsweise könnten Sie das Konto ein paar Tagen ab das aktuelle Datum und die Uhrzeit abläuft festlegen.

Diese PowerShell-Beispiel zeigt, wie die Remote Desktop-Erweiterung auf einen Clouddienst festlegen:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Sie können optional auch angeben, die Bereitstellung Slot und Rollen, denen Sie auf den Remotedesktop aktivieren möchten. Wenn diese Parameter nicht angegeben sind, ermöglicht das Cmdlet remote Desktop aller Rollen in der **Herstellung** Bereitstellung Slot aus.

Die Remote Desktop-Erweiterung ist eine Bereitstellung zugeordnet. Wenn Sie eine neue Bereitstellung für den Dienst erstellt haben, müssen Sie die Bereitstellung remote Desktop aktivieren. Wenn Sie immer Remotedesktop aktiviert haben möchten, müssen Sie berücksichtigen der PowerShell-Skripts in den Bereitstellungsworkflow zu integrieren.


## <a name="remote-desktop-into-a-role-instance"></a>Remotedesktop in eine Instanz der Rolle
Das Cmdlet " [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) " wird in der Cloud-Dienst eine bestimmte Rolleninstanz mit Remotedesktop verwendet. Sie können den Parameter *LocalPath* lokal die RDP-Datei herunterladen verwenden. Oder verwenden Sie den Parameter *Starten* , um das Dialogfeld Remotedesktop-Verbindung zum Zugreifen auf der Instanz der Rolle des Cloud-Dienst direkt zu starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Überprüfen Sie, ob auf einem Dienst Remote Desktop-Erweiterung aktiviert ist
Das Cmdlet " [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) " zeigt an, die Remotedesktop aktiviert oder auf einer Service-Bereitstellung deaktiviert ist. Das Cmdlet gibt der Benutzername für den remote desktop-Benutzer und die Rollen, denen für die remote desktop-Erweiterung aktiviert ist. Standardmäßig in diesem Fall auf den Slot Bereitstellung, und Sie können auch den staging Slot stattdessen nicht verwenden.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Remotedesktop Erweiterung aus einem Dienst entfernen
Wenn Sie die remote desktop-Erweiterung auf eine Bereitstellung bereits aktiviert haben, und die remote desktop-Einstellungen aktualisieren müssen, entfernen Sie zuerst die Erweiterung. Und dann wieder aktivieren, mit die neuen Einstellungen. Beispielsweise, wenn Sie ein neues Kennwort für das Konto Remotebenutzer festlegen möchten, oder das Konto ist abgelaufen. Auf diese Weise muss auf vorhandenen Bereitstellungen, die die remote desktop-Erweiterung aktiviert haben. Für die Bereitstellung neuer können Sie einfach die Erweiterung direkt anwenden.

Um die Bereitstellung die remote desktop-Erweiterung entziehen möchten, können Sie das Cmdlet [AzureServiceRemoteDesktopExtension entfernen](https://msdn.microsoft.com/library/azure/dn495280.aspx) . Sie können optional auch angeben, die Bereitstellung Slot und Rolle aus der Sie die remote desktop-Erweiterung entfernen möchten.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Wenn die Erweiterung Konfiguration vollständig entfernen möchten, sollten Sie das Cmdlet *Entfernen* , mit dem Parameter **UninstallConfiguration** aufrufen.
>
>Der Parameter **UninstallConfiguration** deinstalliert alle Erweiterung-Konfiguration, die auf den Dienst angewendet wird. Jeder Erweiterung Konfiguration, die die Dienstkonfiguration zugeordnet ist. Aufrufen des Cmdlets *Entfernen* , ohne **UninstallConfiguration** hebt die Zuordnung der <mark>Bereitstellung</mark> aus der Erweiterung Konfiguration, sodass effektiv die Erweiterung entfernen. Die Konfiguration der Erweiterung bleibt jedoch mit dem Dienst verknüpft.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

[So konfigurieren Sie Cloud Services](cloud-services-how-to-configure.md)

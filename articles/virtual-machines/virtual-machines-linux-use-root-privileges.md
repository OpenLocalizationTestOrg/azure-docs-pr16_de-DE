<properties 
    pageTitle="Verwenden Sie Administrator-Zugriffsrechten auf Linux virtuellen Computern | Microsoft Azure" 
    description="Erfahren Sie, wie mit Administrator-Zugriffsrechten auf einem Computer in Azure Linux." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Verwenden von Administrator-Zugriffsrechten auf in Azure-virtuellen Computern Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Standardmäßig die `root` Benutzer deaktiviert ist, klicken Sie auf in Azure-virtuellen Computern Linux. Benutzer können mit erhöhten Befehle ausführen, indem Sie mit der `sudo` Befehl. Die Oberfläche kann jedoch variieren je nachdem, wie das System bereitgestellt wurde.

1. **SSH Schlüssel und das Kennwort nur oder Kennwort** - des virtuellen Computers mit entweder ein Zertifikat bereitgestellt wurde (`.CER` Datei) oder SSH Schlüssel als auch ein Kennwort oder nur einen Benutzernamen und Kennwort. In diesem Fall `sudo` wird für das Kennwort des Benutzers auffordern, bevor der Befehl ausgeführt.

2. **Nur Schlüssel SSH** - des virtuellen Computers mit einem Zertifikat bereitgestellt wurde (`.cer`, `.pem`, oder `.pub` Datei) oder SSH Schlüssel, aber kein Kennwort.  In diesem Fall `sudo` **wird nicht** verwendendes Kennwort des Benutzers, bevor Sie den Befehl ausführen.


## <a name="ssh-key-and-password-or-password-only"></a>SSH-Taste und Ihr Kennwort oder nur ein Kennwort

Melden Sie sich bei der Linux virtuellen Computern SSH-Taste oder Kennwortauthentifizierung verwenden, und führen Sie die Befehle mit `sudo`, beispielsweise:

    # sudo <command>
    [sudo] password for azureuser:

In diesem Fall wird der Benutzer zur Eingabe eines Kennworts aufgefordert werden. Nach der Eingabe des Kennworts `sudo` wird der Befehl mit ausgeführt `root` Berechtigungen.

Sie können auch passwordless Sudo durch Bearbeiten aktivieren der `/etc/sudoers.d/waagent` ablegen, beispielsweise:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Passwordless Sudo vom Benutzer "Azureuser" wird diese Änderung zulässig ist.

## <a name="ssh-key-only"></a>SSH nur wichtiger

Melden Sie sich bei der Linux virtuellen Computern mithilfe der wichtigsten SSH-Authentifizierung, und führen Sie die Befehle mit `sudo`, beispielsweise:

    # sudo <command>

In diesem Fall wird der Benutzer **nicht** zur Eingabe eines Kennworts aufgefordert werden. Nach dem Drücken `<enter>`, `sudo` wird der Befehl mit ausgeführt `root` Berechtigungen.

 

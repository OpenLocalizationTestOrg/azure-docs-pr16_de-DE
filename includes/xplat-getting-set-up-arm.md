<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Verwenden von Azure CLI mit Azure Ressourcenmanager (Cloud)

Bevor Sie die CLI Azure mit Ressourcenmanager Befehle und Vorlagen zur Bereitstellung von Azure Ressourcen und Auslastung Entwurfsphase Ressource verwenden können, benötigen Sie ein Konto mit Azure (natürlich). Wenn Sie nicht über ein Konto verfügen, können Sie eine [kostenlose Azure Testversion](https://azure.microsoft.com/pricing/free-trial/)erhalten.

> [AZURE.NOTE] Wenn ein MSDN-Abonnement-Abonnement besitzen, jedoch kein Azure-Konto bereits, können Sie kostenlose erhalten Azure Gutschriften durch die Aktivierung der [MSDN-Abonnementvorteile hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) – oder Sie können ein kostenlose Konto. Entweder funktionieren für Azure-Zugriff.

### <a name="step-1-verify-the-azure-cli-version"></a>Schritt 1: Überprüfen Sie, ob die CLI Azure-version

Zum Verwenden von Azure CLI für Befehle und Cloud Vorlagen, muss mindestens Version 0.8.17. Wenn Ihre Version überprüfen möchten, geben Sie ein `azure --version`. Sie sollte ungefähr wie folgt angezeigt:

    $ azure --version
    0.8.17 (node: 0.10.25)

Wenn Sie Ihre Version von Azure CLI aktualisieren müssen, finden Sie unter [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Schritt 2: Vergewissern Sie sich bei der Verwendung einer Arbeit oder Schule Identität mit Azure

Sie können nur den Cloud Befehl-Modus verwenden, wenn Sie einem [Mandanten Azure Active Directory](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) oder einen [Dienst Tilgungsanteile Namen](https://msdn.microsoft.com/library/azure/dn132633.aspx)verwenden. (Diese werden auch *Organisations-Ids*bezeichnet.)

Um festzustellen, ob Sie über einen verfügen, melden Sie sich durch Eingeben der `azure login` und mit Ihren Arbeit oder Schule Benutzernamen und Ihr Kennwort ein, wenn Sie dazu aufgefordert werden. Wenn Sie eine haben, sollte ungefähr wie folgt angezeigt werden:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Wenn Sie nicht angezeigt werden, müssen Sie einen neuen Mandanten (oder den Dienst, die wichtigsten) für die Identität des Microsoft-Konto erstellen. (Dies ist häufig der Fall bei persönlichen MSDN-Abonnements oder kostenlosen Testabonnements.) Zum Erstellen einer Arbeit oder Schule Id von Ihrem Azure-Konto mit einem Microsoft-Id erstellt wurden, finden Sie unter [einem Azure AD-Verzeichnis mit einem neuen Azure-Abonnement zuordnen](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Wenn Sie, dass Sie bereits eine Organisations-Id sein soll denken, müssen Sie möglicherweise sprechen Sie mit der Person, die das Konto für Sie erstellt hat.

### <a name="step-3-choose-your-azure-subscription"></a>Schritt 3: Auswählen der Azure-Abonnement

Wenn Sie nur ein Abonnement in Ihrem Azure-Konto besitzen, ordnet Azure CLI selbst dieses Abonnement standardmäßig. Wenn Sie mehr als ein Abonnement besitzen, müssen Sie das Abonnement auswählen, indem Sie mit der Eingabe verwenden möchten `azure account set <subscription id or name> true` , in dem _Abonnement-Id oder den Namen_ ist entweder die Abonnement-Id oder dem Namen des Abonnements, die Sie in der aktuellen Sitzung mit arbeiten möchten.

Sie sollte ungefähr wie folgt angezeigt:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Schritt 4: Setzen Sie Ihre Azure CLI in der Cloud-Modus

Geben Sie den Modus Azure Ressource Management (Cloud) zur Verwendung mit Azure CLI `azure config mode arm`. Sie sollte ungefähr wie folgt angezeigt:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Sie können wechseln, wechseln Sie wieder zur Azure Service Management-Befehle durch Eingeben der verwenden `azure config mode asm`.

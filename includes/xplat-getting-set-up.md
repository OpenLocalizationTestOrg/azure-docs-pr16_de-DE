<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Verwenden von Azure CLI

Die folgenden Schritte helfen Azure CLI einfach mit der neuesten Version und die richtige Abonnement verwenden. Wenn Sie Azure CLI installieren und verbinden Sie es zuerst bei Ihrem Konto müssen, finden Sie unter der [Azure Line Interface (CLI Azure)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Schritt 1: Update Azure CLI version

Um Azure CLI für Befehle mit Service Management-Modus verwenden zu können, sollten Sie nach Möglichkeit eine neuere Version verfügen. Wenn Ihre Version überprüfen möchten, geben Sie ein `azure --version`. Sie sollte ungefähr wie folgt angezeigt:

    $ azure --version
    0.8.17 (node: 0.10.25)

Wenn Sie Ihre Version von Azure CLI aktualisieren möchten, finden Sie unter [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Schritt 2: Konfigurieren der Azure-Konto und Ihr Abonnement

Nachdem Sie Ihre Azure CLI mit dem Konto, die Sie verwenden möchten verbunden haben, müssen Sie mehrere Abonnements möglicherweise. In diesem Fall sollten Sie den verfügbaren Abonnements für Ihr Konto überprüfen, indem Sie mit der Eingabe `azure account list`, und wählen Sie dann das Abonnement zu verwenden, mit der Eingabe `azure account set <subscription id or name> true` , in dem _Abonnement-Id oder den Namen_ ist entweder die Abonnement-Id oder dem Namen des Abonnements, die Sie in der aktuellen Sitzung mit arbeiten möchten. Sie sollte ungefähr wie folgt angezeigt:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Wenn ein MSDN-Abonnement-Abonnement besitzen, jedoch kein Azure-Konto bereits, können Sie kostenlose erhalten Azure Gutschriften durch die Aktivierung der [MSDN-Abonnementvorteile hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) – oder Sie können ein kostenlose Konto. Entweder funktionieren für Azure-Zugriff.

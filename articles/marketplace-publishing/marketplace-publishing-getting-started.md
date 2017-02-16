<properties
   pageTitle="Übersicht über das Erstellen und Bereitstellen eines Angebots zum Marketplace | Microsoft Azure"
   description="Grundlegendes zu den Schritten erforderlich machen eines genehmigten Microsoft Developer und erstellen und Bereitstellen einer virtuellen Computerabbild, Vorlage, Datendienst oder Entwicklertools-Dienst in der Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/22/2016"
   ms.author="hascipio" />

# <a name="how-to-publish-and-manage-an-offer-in-the-azure-marketplace"></a>So veröffentlichen und Verwalten eines Angebots in dem Azure Marketplace
In diesem Artikel wird einen Entwickler erstellen, bereitstellen und Verwalten von den dazugehörigen Lösungen in Azure Marketplace für andere Azure Kunden und Partner, zum Kauf und zur Nutzung aufgelisteten helfen bereitgestellt.

## <a name="what-is-the-azure-marketplace"></a>Was ist der Azure Marketplace?
Die Azure Marketplace ist, in dem Abonnent von Azure befinden zu erleichtern die Entwicklung von lokal oder basierend Lösungen und Applikationen cloud-Dienste. Und verwenden Sie [Zertifiziert Azure](http://azure.com/certified) Services als Bausteine, um schnell eine innovative Anwendung oder Dienst für Ihren geschäftlichen und andere Azure Abonnenten entwickeln.

Als eine Azure Publisher die Azure Marketplace ist, wie Sie verteilen und verkaufen Ihre innovative Lösung oder einen bestimmten Dienst an andere Entwickler, ISVs, und IT-Profis, schnell möchten, ihre Cloud-basierten Anwendungen und mobile Lösungen entwickeln.

## <a name="supported-types-of-offers"></a>Unterstützte Datentypen von Angeboten
Erstes gewünschten führen ungeändert ein Herausgebers zu definieren, welche Art von Lösung ist Ihr Unternehmen anbietet. Die Azure Marketplace unterstützt drei Arten von angeboten:

- **Virtuellen Computern Bilder** vorkonfigurierten Bilder mit einem vollständig installierten Betriebssystem und einer oder mehrerer Anmeldungen enthalten sind. Abbild eines virtuellen Computers stellt die erforderlichen Informationen für das Erstellen und Bereitstellen von virtuellen Computern im Dienst Azure-virtuellen Computern an.

    >[AZURE.NOTE] Sie haben **beispielsweise** als eine Azure Publisher erstellt und überprüft ein virtuellen Computers mit einer innovative Datenbankdienst, der genug ansprechenden ist, dass andere Azure Abonnenten beschaffen und Bereitstellen von diesem virtuellen Computer in die Cloud-Service-Umgebungen bereit sein sollte.

- **Developer Services** vollständig verwaltete Dienste in anwendungsverwaltung Entwicklung oder im externen System verwendet werden. Sie bieten Funktionen, die schnelle Entwicklung von Cloud-Skala Clientanwendungen auf Azure zu aktivieren.

    >[AZURE.NOTE] **Beispielsweise** als eine Azure Publisher entwickelt Sie einen API zugegriffen werden-Dienst (gehostet Azure oder an anderer Stelle), der basierte auf zurückliegende Daten Vorhersagen bietet. Und dies ist ein Dienst, den andere Azure Abonnenten, die Lösungen zu erstellen, sind möglicherweise nutzen möchten. Sie können diesen Dienst bereitstellen, zum Azure Marketplace für andere suchen, beschaffen und Benutzer in ihrer jeweiligen Service.

- **Lösungsvorlage** ist eine Datenstruktur, in denen eine verwiesen kann oder mehr distinct Azure Dienste, wozu auch Dienste von anderen Verkäufer, Azure Abonnenten einen oder mehrere Angebote auf eine einzelne, koordinierte Weise bereitstellen aktivieren veröffentlicht.

    >[AZURE.NOTE] **Beispielsweise** als eine Azure Publisher haben Sie Pakete eine Reihe von Diensten aus über Azure, das sie schnell einen sichere, hohen Verfügbarkeit Cloud-Dienst mit wenigen Mausklicks Lastenausgleich bereitstellen herstellt. Andere Azure Abonnenten konnte Wert in Zeit sparen, indem Sie diese Lösungsvorlage lieber manuell Beschaffung detaillierter identifizieren und Konfigurieren der gleichen oder ähnlicher Azure-Dienste.

Einige Schritte sind zwischen den verschiedenen Arten von Lösungen freigegeben. Dieser Artikel enthält eine kurze Übersicht über welche Schritte Sie werden für einen beliebigen Lösung ausführen müssen.

## <a name="1-pre-requisites"></a>1. erforderlichen Komponenten

> [AZURE.NOTE] Bevor Sie etwaige Änderungen auf dem Azure Marketplace beginnen, müssen Sie die [vorab genehmigte](http://azure.com/certified)sein.

1. [Für Microsoft Azure zertifiziert vorab genehmigt anwenden](marketplace-publishing-azure-certification.md)
2. [Erstellen und Registrieren eines Microsoft Developer-Kontos](marketplace-publishing-accounts-creation-registration.md)
3. [Erfüllen technische erforderlichen Komponenten](marketplace-publishing-pre-requisites.md)

## <a name="2-publishing-your-offer"></a>2. veröffentlichen Ihr Angebot
### <a name="21-complete-offer-specific-technical-pre-requisites"></a>2.1 vollständige Angebot spezifische technische erforderlichen Komponenten
- [Virtueller Computer technische erforderlichen Komponenten](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Lösung Vorlage technische erforderlichen Komponenten](marketplace-publishing-solution-template-creation-prerequisites.md)

### <a name="22-create-your-offer"></a>2.2 Erstellen Sie Ihr Angebot
1. Erstellen Sie Ihr Angebot mit den folgenden bestimmte Angebot Führungslinien.
    - [Erstellen Sie Ihr Angebot virtueller Computer](marketplace-publishing-vm-image-creation.md)
    - [Erstellen Sie Ihr Angebot Lösungsvorlage](marketplace-publishing-solution-template-creation.md)
2. [Erstellen Sie Ihr Angebot marketing-Inhalt](marketplace-publishing-push-to-staging.md)

### <a name="23-test-your-offer-in-staging"></a>2.3 Testen Sie 2.3 Ihr Angebot in staging
- [Testen Sie Ihr Angebot virtueller Computer Staging](marketplace-publishing-vm-image-test-in-staging.md)
- [Testen Sie Ihr Angebot Lösungsvorlage Staging](marketplace-publishing-solution-template-test-in-staging.md)

### <a name="24-deploy-your-offer-to-the-marketplace"></a>2.4 Bereitstellen Sie Ihr Angebot zum Marketplace
- [Bereitstellen von Ihr Angebot zum Azure Marketplace](marketplace-publishing-push-to-production.md)

### <a name="virtual-machine-image-specific"></a>Abbildung des virtuellen Computers bestimmte ###
- [Erstellen ein Bild virtueller Computer lokal](marketplace-publishing-vm-image-creation-on-premise.md)
- [Erstellen Sie einen virtuellen Computer mit Windows Azure Preview-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


- [Allgemeine Probleme beim Veröffentlichen auf der Marketplace Problembehandlung](marketplace-publishing-support-common-issues.md)
- Weitere Informationen zu den Portals verwendet, finden Sie unter [Portals benötigen werden](marketplace-publishing-portals.md).


## <a name="3-post-publishing-management-of-your-offer"></a>3. Veröffentlichung nach der Verwaltung von Ihr Angebot
- [Nach der Herstellung Leitfaden für virtuellen Computern bietet.](marketplace-publishing-vm-image-post-publishing.md)
- [So aktualisieren Sie die technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku)
- [So aktualisieren Sie technische Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#1-how-to-update-the-technical-details-of-a-sku)
- [So fügen Sie eine neue SKU unter einem aufgelisteten Angebot](marketplace-publishing-vm-image-post-publishing.md#3-how-to-add-a-new-sku-under-a-listed-offer)
- [Zum Ändern der Anzahl der Daten Datenträger für eine aufgelisteten SKU](marketplace-publishing-vm-image-post-publishing.md#4-how-to-change-the-data-disk-count-for-a-listed-sku)
- [So löschen Sie eine aufgelisteten Angebot aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#5-how-to-delete-a-listed-offer-from-the-azure-marketplace)
- [So löschen Sie eine aufgelistete SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#6-how-to-delete-a-listed-sku-from-the-azure-marketplace)
- [So löschen Sie die aktuelle Version von einer aufgelisteten SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace)
- [So Auflistung Preis Herstellung Werte wiederherstellen](marketplace-publishing-vm-image-post-publishing.md#8-how-to-revert-listing-price-to-production-values)
- [So Abrechnung Modell Herstellung Werte wiederherstellen](marketplace-publishing-vm-image-post-publishing.md#9-how-to-revert-billing-model-to-production-values)
- [Zum Wiederherstellen der für die Sichtbarkeit von einer aufgelisteten SKU der Herstellung Wert](marketplace-publishing-vm-image-post-publishing.md#10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value)
- [Zum Ändern der Cloud Solution Provider Reseller incentive](marketplace-publishing-csp-incentive.md)
- [Grundlegendes zu Ihrem Verkäufer Einsichten reporting](marketplace-publishing-report-seller-insights.md)
- [Grundlegendes zu Ihrem Auszahlung reporting](marketplace-publishing-report-payout.md)
- [Anfordern von Unterstützung als eines Herausgebers](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
- [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)

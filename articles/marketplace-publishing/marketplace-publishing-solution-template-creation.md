<properties
   pageTitle="Leitfaden zum Erstellen einer Lösungsvorlage für Marketplace | Microsoft Azure"
   description="Wenn Sie ausführliche Anweisungen zum Erstellen, zertifizieren und Bereitstellen einer Multi-virtuellen Computer Bild Lösung Vorlage für kaufen auf Azure Marketplace."
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
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Leitfaden zum Erstellen einer Lösungsvorlage für Azure Marketplace
Nach Abschluss der Schritt 1, [Konto erstellen und registrieren][link-acct-creation], schrittweise wir Sie bei der Erstellung einer Azure-kompatiblen Lösung Vorlage bei [technischen Voraussetzung für die Erstellung einer Lösungsvorlage](marketplace-publishing-solution-template-creation-prerequisites.md). Nachdem Sie die Schritte zum Erstellen einer Lösungsvorlage für mehrere virtuelle Computer, auf dem [Veröffentlichungsportal] Installationsprozess wird[ link-pubportal] von Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Erstellen Sie Ihr Angebot der Lösung Vorlage in der Veröffentlichungsportal
Wechseln Sie zu [https://publish.windowsazure.com](http://publish.windowsazure.com). Bei der Anmeldung zum ersten Mal [Veröffentlichungsportal](https://publish.windowsazure.com/)verwenden Sie das gleiche Konto mit dem Ihres Unternehmens Verkäuferprofil registriert wurde. Später können Sie einen beliebigen Mitarbeiter Ihres Unternehmens als co-Administrator in der Veröffentlichungsportal hinzufügen.

### <a name="1-select-solution-templates"></a>1. Wählen Sie "Lösung" Vorlagen"

  ![Zeichnung][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. erstellen Sie eine neue Lösung-Vorlage

  ![Zeichnung][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3 beginnen Sie 3 mit Topologien
Eine Lösungsvorlage ist "Parent" alle zugehörigen Topologien. Sie können mehrere Topologien in einem Angebot/Lösung Vorlage definieren. Wenn ein Angebot ans Staging verschoben wird, wird es mit allen zugehörigen Topologien abgelegt. Gehen Sie folgendermaßen vor, um Ihr Angebot zu definieren:     

- Erstellen einer Suchtopologie: "Suchtopologie Erkennungszeichen" ist normalerweise der Name der Suchtopologie für die Lösungsvorlage. Der Suchtopologie Bezeichner wird in der URL verwendet, wie unten dargestellt:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Fügen Sie eine neue Version hinzu.

### <a name="4-get-your-topology-versions-certified"></a>4 holen Sie 4 sich Ihre Suchtopologie Versionen zertifiziert
Hochladen einer Zip-Datei, die alle erforderliche Dateien zum Bereitstellen dieser speziellen Version des der Suchtopologie enthält. Diese Zip-Datei muss Folgendes enthalten:

- *mainTemplate.json* und *createUiDefinition.json* Datei entsprechenden Stammverzeichnis.
- Alle verknüpften Vorlagen und alle erforderlichen Skripts.

  > [AZURE.TIP] Während Ihre Entwickler arbeiten, erstellen die Lösung Vorlage Topologien und gebracht, zertifiziert, die Business können auf den Inhalt marketing und rechtlichen marketing und/oder rechtliche Abteilungen Ihres Unternehmens arbeiten.

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie Ihre Lösung Vorlage erstellt und Zip-Datei hochgeladen haben, führen Sie bitte die in der [Marketplace-marketing-Content Leitfaden](marketplace-publishing-push-to-staging.md) vor dem Laden des Angebots zum Staging. Zum Anzeigen sämtlicher Marketplace Veröffentlichen von Artikeln finden Sie auf [Erste Schritte: So veröffentlichen ein Angebots zu Azure Marketplace](marketplace-publishing-getting-started.md).

Sie können auch die folgenden verwandten Artikel interessant sein:

- Virtueller Computer Bilder: [Informationen zu virtuellen Computern Bilder in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Virtueller Computer Erweiterungen: [virtueller Computer Agent und Erweiterungen – Überblick über](https://msdn.microsoft.com/library/azure/dn832621.aspx) und [Azure-virtuellen Computer-Erweiterungen und Funktionen](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure Ressourcenmanager: [Authoring Azure-Cloud-Vorlagen](../resource-group-authoring-templates.md) und [Beispiele für einfache Cloud-Vorlage](https://github.com/rjmax/ArmExamples)
- Steuerung von Speicher-Konto: [wie Monitor für Speicher Konto Begrenzungsebene](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) und [Premium-Speicher](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com

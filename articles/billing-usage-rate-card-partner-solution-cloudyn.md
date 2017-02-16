<properties
   pageTitle="Microsoft Azure Verwendung und RateCard APIs aktivieren Cloudyn ITFM für Kunden bereitstellen | Microsoft Azure"
   description="Stellt eine eindeutige Perspektive aus Microsoft Azure Abrechnung Partner Cloudyn, deren Erfahrung Integration der Abrechnung Azure-APIs in deren Produkt.  Dies ist für Kunden mit Azure und Cloudyn, die mit/ausprobieren von Cloudyn für Azure Services interessiert sind besonders hilfreich."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Aktivieren Microsoft Azure Verwendung und RateCard APIs Cloudyn ITFM für Kunden bereitstellen

Cloudyn, einem Microsoft-Entwicklungspartner und einem führenden Anbieter von Cloud Verwaltungsfunktionen, wurde für eine private Vorschau der neuen Microsoft Azure Ressource: Einsatz und RateCard APIs ausgewählt.  Die Verwendung-API ermöglicht den Zugriff auf geschätzte Azure Verbrauchsdaten für ein Abonnement. Die RateCard-API bietet vollständige Preisinformationen aller Azure Dienste für nicht - Enterprise Agreement EA Kunden. Diese APIs bieten integriert zusammen, die vollständige Informationen Grundlage für die Eingabe in IT Financial Management (ITFM)-Tools, wie etwa die von Cloudyn bereitgestellten.

## <a name="introduction"></a>Einführung

Die so genannte "Multiplikation" von Daten aus der Verwendung-API mit Daten aus der RateCard-API (Verwendung [Einheiten] Preis [$unit] = detaillierte Verwendung und Kosten) die am häufigsten präzise, präzise und verlässliche Abrechnungsinformationen verfügbar für Azure heute erstellt.

![ITFM (Übersicht)][1]

Diese APIs Verarbeitung enthält wichtige Informationen zur Verwendung und Kosten, gleicht Cloudyn Kundenkonten in eine einfache, programmgesteuerten Weise analysieren und verschiedene ITFM Aufgaben für ihren Kunden ausführen Kunden.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integration von Cloudyn mit dem RateCard und die Verwendung von APIs
Die RateCard-API sind mehrere Eingabeparameter – wie Region Info "," Währung "und" Gebietsschema – erforderlich, aber die wichtigste ist OfferDurableID, die angibt, dass der Typ des Azure Geschenk den Kunden (nutzungsbasierte, legacy 6 und 12-Monats-Zusicherung Pläne, MSDN Angebote, MPN Angebote, Angebote und andere) verwendet wird. Die OfferDurableID kann in der [Verwendung von Azure und Abrechnung Portal](https://account.windowsazure.com/Subscriptions), unter der "anbieten-ID" für das angegebene Abonnement gefunden werden.

Bei der Registrierung für [Cloudyn für Azure](https://www.cloudyn.com/microsoft-azure/) Services können Kunden ihre OfferDurableID Code Hinzufügen der Cloudyn, deren Preisgestaltung relevante Informationen über die RateCard-API ziehen können.  Informationen über die verschiedenen Arten von Angeboten finden Sie eine Detailseite [Microsoft Azure anbieten](https://azure.microsoft.com/support/legal/offer-details/) .

![Cloudyn ITFM-Engine (Übersicht)][2]

Cloudyn verwendet die Verwendung und RateCard APIs, sowie die Leistung Azure-API, erstellen Sie zusätzliche Ebenen der Visualisierung, Analytics, Warnung, Berichterstattung, Verwaltung von Kosten und einige Vorschläge, Bereitstellen von einer zuverlässigen Enterprise Cloud ITFM Tool Azure Kunden.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Verwenden Sie Cloudyn ITFM Fällen durch Verwendung und RateCard API-Integration aktiviert
Allgemeine Cloudyn ITFM verwenden aktivierte, indem Sie Verwendung Fällen und RateCard APIs enthalten:

+ Ermöglicht **Cost Analysis** - cloud-Kosten zu einem beliebigen systemeigenen Erkennungszeichen Dimension (Anbieter, Service, Firma, Region usw.) gelöst wird. Die Verwendung von Azure RateCard APIs diese einfache Aufgabe, indem Sie die am häufigsten präzise Projektstrukturplan-Codes der Verwendung machen und Kostendaten pro Konto, das dann gruppiert und gefiltert nach Cloudyn und dem Benutzer, einer Grafik oder Tabellarisch Formular angezeigt.

![Kosten Analysis-aus-Kreis-Diagramm][3]

+ **Kosten Zuteilung 360** - ermöglicht Finanzen und die ist-Kosten Projektstrukturplan-Codes, die Treiber und Trends von deren Cloudbereitstellung für eine Steigerung IT-Manager. Sie können weiteren Manager leicht Bereitstellung Ausgaben Business Einheiten, Abteilungen, Regionen und mehr, bietet beispiellose Einblicke in die Cloud Kosten und Erleichterung Enterprise Chargebacks und Showbacks zuordnen. Die Verwendung von Azure und RateCard APIs dienen als Eingabe für Cloudyns Kosten Zuteilung-Engine, die die APIs ergänzen, indem Sie definieren Methoden und Geschäftslogik für ohne Tags oder untaggable Ressourcen zuordnen.

![Kosten Zuteilung 360-Diagramm][4]

+ **Kostengünstige Ziehpunkts** - Leitfaden rechts-Ziehpunkt für genutzter virtuellen Computern, wodurch der vom Kunden Ausgaben auf zu große oder zu viel bereitgestellte Autos. Dies geschieht durch Untersuchen virtuellen Computern CPU und RAM Kennzahlen (über Leistung-API), Stunden Laufzeit (über Verwendung API) und Kosten (über RateCard-API). Cloudyn rechts-Ziehpunkts Leitfaden basierend auf genutzter RAM oder CPU-Ressourcen (Performance) und geschätzte Spareinlagen durch Multiplizieren des Preis Deltas (RateCard) zwischen den virtuellen Computern durch die tatsächliche Zeit-Auslastung-(Verwendung) des Computers genutzter berechnet.

![Kostengünstiger Ziehpunkt][5]

+ **Cloud Portieren Empfehlungen** - enthält finanzmathematische Ratschläge Cloud portieren. Es eines Benutzers aktuelle Kosten untersucht Cloud Ressourcen, die auf die wichtigsten Cloud Lieferanten bereitgestellt werden, und vergleicht ihn mit den Soll-Kosten eine entsprechende Weitergabe auf Azure. Klicken Sie dann präzise, bietet pro-Ressource, wertmäßig-basierten Empfehlungen in Azure portieren. Nach Beurteilung der entsprechende Weitergabe auf Azure (auf der Grundlage der Leistung Metrik und Benutzer Einstellungen) erforderlich, verwendet Cloudyn die RateCard-API für die Kosten für die entsprechende Weitergabe auf Azure ausgewertet werden soll.

+ **Berichte zur Performance** - aktivierte Azure der Leistung API, bieten diese Berichte eine Reihe von Features von CPU und RAM-Nutzung zu Optimierung Empfehlungen. Nachfolgend ist ein Instanz Auslastung Bericht-Beispiel, Instanz Projektstrukturplan-Codes durch die durchschnittliche CPU-Auslastung präsentieren.

![Performance-Berichte][6]

+ **Kategorie-Manager** - ein leistungsfähiges Feature in Cloudyn, die Reihenfolge für nicht organisierte Cloudressourcen bereitstellt. Es ermöglicht Benutzern der Freiheitsgrade im eigenen eindeutigen Kategorien (Kategorien) für effektiven messen und Berichte erstellen, die an Geschäftsabläufe ausgerichtet ist. Darüber hinaus können Benutzer einfach Regeln und kategorisieren inkonsistente tagging (d. h. Tippfehler und andere abweichungen) und ohne Tags Ressourcen für genau Kosten Abschreibung automatisch erkennen.

![Kategorie-Manager][7]

## <a name="video"></a>Video

Hier ist ein kurzes Video, das zeigt, wie ein Azure-Kunde Cloudyn für Azure und Azure-APIs Abrechnung um zu gewinnen Einsichten von deren Azure Verbrauchsdaten verwenden kann.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Nächste Schritte

+ Starten Sie eine kostenlose Testversion von [Cloudyn für Azure](https://www.cloudyn.com/microsoft-azure/) um anzuzeigen, wie Sie für die Bereitstellung Ihrer Microsoft Azure Cloud Kosten Transparenz mit angepassten Berichterstattung und Analytics abrufen können.
+ Eine Übersicht über die Azure Ressource: Einsatz und RateCard APIs finden Sie unter [Einblicke in Ihrer Microsoft Azure Ressourcenverbrauch zu erhalten](billing-usage-rate-card-overview.md) .
+ Schauen Sie sich die [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für Weitere Informationen zu beiden APIs, die Teil der Satz von APIs vom Azure-Manager bereitgestellt werden.
+ Wenn Sie rechts in der Stichprobe Code kümmern möchten, schauen Sie sich unsere Microsoft Azure Abrechnung API Codebeispielen auf [Azure Codebeispielen](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Weitere Informationen
+ Weitere Informationen zu Microsoft Azure Enterprise Agreement (EA) Angebote zu finden, besuchen Sie [Azure für die Enterprise-Lizenzierung] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Finden Sie im Hilfeartikel [Azure Ressourcenmanager Übersicht](azure-resource-manager/resource-group-overview.md) erfahren Sie mehr über Azure-Manager aus.
+ Weitere Informationen über die Sammlung von Tools, die erforderlich sind, um die Hilfe in der Cloud Kennenlernen verbringen, finden Sie im Artikel der Gartner [Market-Leitfaden für Tools IT Financial Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png

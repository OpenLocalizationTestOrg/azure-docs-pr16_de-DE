<properties
   pageTitle="Gewinnen Sie Einsichten in Ihrer Microsoft Azure Ressourcenverbrauch | Microsoft Azure"
   description="Bietet eine konzeptionelle Übersicht der Azure Abrechnung Verwendung und RateCard-APIs, die zum Einblicke in Azure Ressourcenverbrauch und Trends verwendet werden."
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

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Gewinnen Sie einen Einblick in Ihre Microsoft Azure Ressourcenverbrauch

Kunden und Partner müssen in der Lage exakt zu prognostizieren und deren Azure Kosten verwalten.  Wie sie zu einem Modell Opex aus einer Capex verschieben möchten, benötigen sie auch die Möglichkeit, führen Sie Showback im Vergleich zu Chargeback Analyse sowie Modus Genauigkeit in Abschätzung und Abrechnung, vor allem für große Cloud-Bereitstellungen bereitstellen.

Die Azure Ressource: Einsatz und Zins Karte APIs erörtert in diesem Artikel Adresse diese Anforderungen, indem Sie neue Einblicke in Ihre Verbrauch Azure Ressourcen.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Einführung in die Azure Ressourceneinsatz und RateCard-APIs

Die Azure Ressource: Einsatz und RateCard APIs werden als Ressourcenanbieter, als Teil der Familie APIs verfügbar gemacht werden vom Azure-Manager implementiert werden.  

### <a name="azure-resource-usage-api-preview"></a>Azure Ressourceneinsatz API (Preview)
Kunden und Partner können die Azure Ressource: Einsatz-API verwenden, können Sie um ihre geschätzten Azure Verbrauchsdaten zu gelangen. Die Features umfassen:

- **Rollenbasierte Azure Access Control** - Kunden und Partner können ihre Access-Richtlinien konfigurieren, auf der [Azure-Portal](https://portal.azure.com) oder über [Azure PowerShell-Cmdlets](powershell-install-configure.md) zum Zugriff auf das Abonnement des Verwendungsdaten aufrufen können, welche Benutzer oder Anwendungen angeben. Anrufer müssen standard Azure Active Directory-Token für die Authentifizierung verwenden. Anrufer muss auch der Reader, Besitzer oder Mitwirkender Rolle für den Zugriff auf die Verwendungsdaten für ein bestimmtes Azure-Abonnement hinzugefügt werden.

- **Stündlich oder täglich Aggregationen** - Anrufer können angeben, ob sie ihre Azure Verwendungsdaten stündlich Buckets oder tägliche Buckets werden sollen. Die Standardeinstellung ist tägliche.

- **Bereitgestellten Instanzmetadaten (einschließlich Ressource Kategorien)** – Instanz Ebene Details wie den vollqualifizierten Ressource Uri (/subscriptions/ {Abonnement-Id} /...), zusammen mit der Ressource Gruppe Informations- und Ressourcen-Tags in der Antwort bereitgestellt werden. Dies wird programmgesteuert Verwendung durch die Kategorien zuordnen, für Fallstudien wie Cross Erhebung und Kunden deterministisch helfen.

- **Ressourcenmetadaten zur Verfügung gestellt** – Einzelheiten von Ressourcen wie Meter Name, Meter Kategorie, Unterkategorie Meter, Einheit und Region wird auch in der Antwort, um den Anrufer zu verleihen besser verstehen, was verbraucht wurde übergeben. Wir arbeiten auch im Portal Azure Ressource Metadaten Terminologie folienmasterebene Azure Verwendung CSV und EA Abrechnung CSV und andere öffentlich zugänglichen Erfahrung, damit Kunden Daten über Erfahrung zu koordinieren können.

- **Verwendung bei allen Angebotstypen** – Verwendungsdaten zugegriffen werden für alle Typen anderem nutzungsbasierte, MSDN, monetäre Zusicherung, monetäre Kreditkarte und EA anbieten.

### <a name="azure-resource-ratecard-api-preview"></a>Azure Ressource RateCard API (Preview)
Kunden und Partner können der Azure Ressourcen RateCard-API Sie die Liste der verfügbaren Ressourcen Azure, zusammen mit abrufen geschätzte Preisinformationen für jede. Die Features umfassen:

- **Rollenbasierte Azure Access Control** - Kunden und Partner können ihre Access-Richtlinien konfigurieren, auf der [Azure-Portal](https://portal.azure.com) oder über [Azure PowerShell-Cmdlets](powershell-install-configure.md) zum Zugriff auf die Daten RateCard aufrufen können, welche Benutzer oder Anwendungen angeben. Anrufer müssen standard Azure Active Directory-Token für die Authentifizierung verwenden. Anrufer muss auch der Reader, Besitzer oder Mitwirkender Rolle für den Zugriff auf die Verwendungsdaten für ein bestimmtes Azure-Abonnement hinzugefügt werden.

- **Unterstützung für nutzungsbasierte, MSDN, monetäre Zusicherung, und Geldbeträge Kreditkarte bietet (EA nicht unterstützt)** – diese API bietet Azure Angebot Ebene Zins Informationen im Vergleich zu Abonnement Ebene.  Anrufer dieser API muss alle Angebotsinformationen abzurufenden Ressourcendetails und Sätzen übergeben.  Wie EA Angebote Kostensätze pro Registrierung angepasst haben, können wir nicht die EA Sätzen zu diesem Zeitpunkt zur Verfügung zu stellen.

## <a name="scenarios"></a>Szenarien

Hier sind einige der Szenarien, die die Kombination der Verwendung und die RateCard-APIs möglich vorgenommen werden:

- **Azure aufwenden während des Monats** - Kunden können die Verwendung und RateCard APIs kombinierter abzurufenden bessere Einblicke in die Cloud aufwenden während des Monats, durch die Analyse der Stunden- und tägliche Buckets der Verwendung und Gebühr schätzt.

- **Einrichten von Benachrichtigungen** – Kunden und Partner können Ressourcen oder monetäre-basierten Benachrichtigungen für ihren Cloud Verbrauch einrichten durch die geschätzte Verwendung und Gebühr Schätzung verwenden, und die Verwendung der RateCard-API abrufen.

- **Predict Rechnung** – Kunden und Partner können ihre geschätzten Verbrauch erhalten und Cloud aufwenden und Computer Learning Algorithmen, um am Ende der Abrechnungszyklus wäre ihre Rechnung Vorhersagen anwenden.

- **Pre Verbrauchskosten-Analyse** – Kunden können auch gewünscht verwenden die RateCard-API, wie viel ihre Rechnung Vorhersagen wäre wären sie deren Auslastung in Azure von Bereitstellung verschieben Verwendung Zahlen. Wenn Kunden vorhandene Auslastung in anderen Wolken oder private Wolken verfügen, können sie auch zuordnen, auf deren Verwendung mit der Azure aufwenden Sätzen, um eine bessere Schätzung von deren geschätzte Azure zu erhalten. Dies bietet eine erweiterte Ansicht was über die [Azure Preise Taschenrechner](https://azure.microsoft.com/pricing/calculator/), abgerufen werden kann wie (zum Beispiel) unsere Abrechnung Partner die Möglichkeit zum Pivotieren auf Angebot und vergleichen/Kontrast zwischen verschiedenen Angebot Arten jenseits nutzungsbasierte, einschließlich monetäre Zusicherung und monetäre Kreditkarte. Die APIs bieten auch die Möglichkeit, die Kosten Abschätzung Änderungen nach Region, aktivieren den Typ der Was-wäre-wenn-Analyse erforderlich, damit Bereitstellung Entscheidungen, Bereitstellen von Ressourcen in verschiedenen DCs auf der ganzen Welt einen direkten Einfluss auf die Gesamtkosten haben können.

- **Was-wäre-wenn-Analyse** -

    - Kunden und Partner können festlegen, ob es kostengünstiger wäre, deren Auslastung in einem anderen Region oder in einer anderen Konfiguration der Azure Ressource auszuführen. Azure Ressource, die Kosten unterscheiden sich möglicherweise auf Grundlage der Azure Region, in der sie ausgeführt werden, und auf diese Weise können Kunden und Partner können Kosten Optimierungen zu gelangen.

    - Kunden und Partner können auch festzustellen, ob eine andere Art von Azure Angebot eine bessere Zins für eine Ressource Azure bietet.

## <a name="partner-solutions"></a>Partnerlösungen

[Microsoft Azure Verwendung und RateCard APIs aktivieren Cloudyn zum Bereitstellen von ITFM für Kunden](billing-usage-rate-card-partner-solution-cloudyn.md) beschrieben, die Integration Oberfläche Azure Abrechnung API Partners [Cloudyn](https://www.cloudyn.com/microsoft-azure/).  Dieser Artikel enthält detaillierte von ihrer Erfahrung, einschließlich eines kurzen Videos wird, das anzeigt, wie ein Azure-Kunde Cloudyn und Abrechnung Azure-APIs zum Gewinne Einsichten von deren Azure Verbrauchsdaten verwenden kann.

[Cloud Cruiser und Integration von Microsoft Azure Abrechnung-API](billing-usage-rate-card-partner-solution-cloudcruiser.md) beschreibt die Funktionsweise [der Cloud Cruiser Express für Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) direkt aus dem Portal WAP ermöglichen Kunden Betrieb und finanzielle Aspekte der private oder gehostete öffentliche Cloud ihren Microsoft Azure nahtlos über eine einzige Benutzeroberfläche verwalten.   

## <a name="next-steps"></a>Nächste Schritte
+ Schauen Sie sich die [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für weitere Details auf beide APIs, die Teil der Satz von APIs vom Azure-Manager bereitgestellt werden.
+ Wenn Sie rechts in der Stichprobe Code kümmern möchten, schauen Sie sich unsere Microsoft Azure Abrechnung API Codebeispielen auf [Azure Codebeispielen](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Weitere Informationen
+ Finden Sie im Hilfeartikel [Azure Ressourcenmanager Übersicht](azure-resource-manager/resource-group-overview.md) erfahren Sie mehr über Azure-Manager aus.
+ Weitere Informationen über die Sammlung von Tools, die erforderlich sind, um die Hilfe in der Cloud Kennenlernen verbringen, finden Sie im Artikel der Gartner [Market-Leitfaden für Tools IT Financial Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

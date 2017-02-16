<properties
   pageTitle="Cloud Cruiser und Microsoft Azure-API Integration Abrechnung | Microsoft Azure"
   description="Stellt eine eindeutige Perspektive aus Microsoft Azure Abrechnung Partner Cloud Cruiser, deren Erfahrung Integration der Abrechnung Azure-APIs in deren Produkt.  Dies ist besonders hilfreich für Azure und Cruiser Cloud-Kunden, die mit/ausprobieren von Cloud Cruiser für Microsoft Azure Pack interessiert sind."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser und Microsoft Azure Abrechnung API-Integration

Dieser Artikel beschreibt, wie die von den neuen Microsoft Azure Abrechnung APIs erfassten Informationen werden in der Cloud Cruiser für Workflow Kosten Simulation und Analyse verwendet werden kann.

## <a name="azure-ratecard-api"></a>Azure RateCard-API
Die RateCard-API stellt Zins aus Azure. Nach der Authentifizierung mit den richtigen Anmeldeinformationen, können Sie die API zum Sammeln von Metadaten über die verfügbaren Dienste auf Azure, zusammen mit den Sätzen Ihrer anbieten ID zugeordnet Abfragen.

Im folgenden finden Sie eine Stichprobe Antwort aus der-API die Preise für die A0 mit Instanz (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser der Benutzeroberfläche zum Azure RateCard API
Cloud Cruiser kann die RateCard-API Informationen auf unterschiedliche Weise zu nutzen. In diesem Artikel werden wir gezeigt, wie sie verwendet werden kann, IaaS vornehmen Arbeitsbelastung Simulation und Analyse Kosten.

Um diese Anwendungsfall-veranschaulichen, stellen Sie vor einem Arbeitsbelastung mehrerer Instanzen auf Microsoft Azure Pack (WAP) ausgeführt. Das Ziel ist dieser dieselbe Arbeitsbelastung auf Azure zu reproduzieren, und die Kosten des ausführen solche Migration schätzen. Dieser Simulation erstellen, gibt es zwei Wichtigste Aufgaben ausgeführt werden:

1. **Importieren und Prozess gesammelten Informationen für den Dienst aus der RateCard-API.** Diese Aufgabe ausgeführt wird auch auf die Arbeitsmappen, wobei die extrahiert werden aus der RateCard-API transformiert und zu einem neuen Zins Plan veröffentlicht. Diese neuen Zins Plan wird auf der Simulationen schätzen die Preise Azure verwendet werden.

2. **Normalisieren Sie WAP Services und Azure Services für IaaS.** Standardmäßig WAP Services basieren Services basieren auf einzelne Ressourcen (CPU, Arbeitsspeichergröße, Datenträgergröße usw.) während Azure Instanz abgesehen (A0, A1, A2 usw.). Dieser erste Aufgabe ausgeführte Arbeit von der Cloud Cruiser ETL-Engine, aufgerufen Arbeitsmappen, wo diese Ressourcen auf Instanz an Papiergrößen, analog zur Azure-Instanz Services gebündelten werden wechseln.

### <a name="import-data-from-the-ratecard-api"></a>Importieren von Daten aus der RateCard-API

Cloud Cruiser Arbeitsmappen bieten eine automatisierte Möglichkeit zum Sammeln und Verarbeiten von Informationen aus der RateCard-API.  ETL (Extrahieren Transformation laden) Arbeitsmappen können Sie die Auflistung, Transformation und Veröffentlichung von Daten in die Cloud Cruiser Datenbank konfigurieren.

Jede Arbeitsmappe kann eine oder mehrere Websitesammlungen, so dass Sie zu koordinieren Informationen aus verschiedenen Quellen ergänzen oder erweitern die Verwendungsdaten haben. Die folgenden zwei Screenshots zeigen, wie eine neue *Websitesammlung* in eine vorhandene Arbeitsmappe, und Importieren von Informationen in die *Sammlung* von der RateCard-API zu erstellen:

![Abbildung 1: Erstellen einer neuen Sammlung][1]

![Abbildung 2: Importieren von Daten aus der neuen Sammlung][2]

Nach dem Import der Daten in der Arbeitsmappe ein, ist es möglich, mehrere Schritte und Prozesse Transformation, ändern und Modellieren von Daten zu erstellen. In diesem Beispiel, da wir nur interessiert Infrastructure-as-a-Service (IaaS sind) wir die Transformation Schritte verwenden können, um entfernen unnötige Zeilen oder Einträge, im Zusammenhang mit Services als IaaS.

Das folgende Bildschirmabbild zeigt die Transformation Schritte zum Verarbeiten von RateCard-API gesammelten Daten verwendet werden:

![Abbildung 3: Transformation Schritte zum Verarbeiten gesammelter Daten aus RateCard-API][3]

### <a name="defining-new-services-and-rate-plans"></a>Definieren neue Dienste und Zins-Pläne

Es gibt verschiedene Möglichkeiten, um die Dienste auf Cloud Cruiser definieren. Ist eine der Optionen aus den Verwendungsdaten der Dienste zu importieren. Diese Methode ist im Allgemeinen zur Arbeit mit öffentliche Wolken, in dem die Dienste bereits vom Anbieter definiert sind.

A Zins Plan ist eine Reihe von Sätzen oder Preise, die für andere Dienste, basierend auf aktuelles Datum oder eine Gruppe von Kunden, unter den anderen Optionen angewendet werden können. Zins Pläne können auch auf Cloud Cruiser Simulation oder Szenarios "Was-wäre-wenn" zu verstehen, wie sich Dienste die Gesamtkosten für einen Arbeitsbelastung auswirken können verwendet werden.

In diesem Beispiel werden wir die Informationen aus der RateCard-API verwenden, um neue Dienste in der Cloud Cruiser definieren. Auf die gleiche Weise können wir die Sätzen verknüpft ist, auf die Dienste verwenden, um Planen eines neuen Zins Cloud Cruiser erstellen.

Am Ende einer Transformation ist es möglich, einen neuen Schritt erstellen und veröffentlichen die Daten aus der RateCard-API als neue Dienste und Sätzen.

![Abbildung 4: veröffentlichen die Daten aus der RateCard-API als neue Dienste und Kostensätzen][4]

### <a name="verify-azure-services-and-rates"></a>Vergewissern Sie sich Azure und-Kosten

Nach dem Veröffentlichen der Dienste und Sätzen, können Sie die Liste der importierten Dienste in der Cloud Cruiser *Services* Registerkarte überprüfen:

![Abbildung 5: überprüfen die neuen Dienste][5]

Aktivieren Sie auf der Registerkarte *Zins Pläne* den neuen Zins Plan namens "AzureSimulation" mit den Sätzen aus der RateCard-API importiert.

![Abbildung 6: Überprüfen des neuen Zins planen und der zugehörigen Sätzen][6]

### <a name="normalize-wap-and-azure-services"></a>Normalisieren Sie WAP und Azure Services

Standardmäßig bietet WAP Verwendungsinformationen basierend auf der Verwendung von berechnen, Arbeitsspeicher und Netzwerk-Ressourcen. Definieren Sie in der Cloud-Cruiser Ihrer Dienste basierend direkt auf die Zuteilung oder getaktete Verwendung dieser Ressourcen. Angenommen, können Sie einen einfachen Satz für jede Stunde der CPU-Auslastung oder entstehen die GB des Arbeitsspeichers für eine Instanz.

In diesem Beispiel benötigen Kosten zwischen WAP und Azure, Vergleichen wir die Ressource: Einsatz auf WAP in bündeln, aggregieren, die dann Azure Services zugeordnet sein können. Diese Transformation kann problemlos in die Arbeitsmappen implementiert werden:

![Abbildung 7: WAP Datentransformation um Services normalisieren][7]

Der letzte Schritt darin, auf die Arbeitsmappe ist, die Daten in der Cloud Cruiser Datenbank veröffentlichen. In diesem Schritt die Verwendungsdaten jetzt gebündelten in Services (die mit den Diensten Azure zuordnen) und verknüpft zu Standard-Sätzen, um die Gebühren zu erstellen.

Nach dem Beenden der Arbeitsmappe, können Sie die Verarbeitung der Daten durch Hinzufügen eines Vorgangs auf der Scheduler und angeben, Häufigkeit und Zeitpunkt für die Arbeitsmappe ausführen automatisieren.

![Abbildung 8 - Planung der Arbeitsmappe][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Erstellen von Berichten für die Arbeitsbelastung Kosten Simulation Analyse

Nach die Verwendung gesammelt werden und Gebühren in der Cloud Cruiser Datenbank geladen sind, können wir das Cloud Cruiser Einsichten Modul zum Erstellen der Arbeitsbelastung Simulation, die wir gewünschte Kosten Nutzen.

Um dieses Szenario zu veranschaulichen, erstellt wir den folgenden Bericht:

![Vergleich der Kosten][9]

Das obere Diagramm zeigt einen Kostenvergleich von Diensten Vergleich des Preises die Arbeitsbelastung für jeden Dienst bestimmten zwischen WAP (Dunkelblau) und Azure (hellblau) ausführen.

Im unteren Diagramm sind die gleichen Daten aber aufgeschlüsselt nach Abteilung. Zeigt die Kosten für jede Abteilung, deren Arbeitsbelastung in WAP und Azure, sowie die Differenz zwischen den beiden in der Leiste Spareinlagen (Grün) ausführen.

## <a name="azure-usage-api"></a>Verwendung der Azure-API


### <a name="introduction"></a>Einführung

Microsoft eingeführt zuletzt Azure Verwendung-API, gleicht Abonnenten programmgesteuert abonnierte Nutzungsdaten Einblicke in ihren Verbrauch zu erhalten. Dies ist eine gute Neuigkeiten für Cruiser Cloud-Kunden, die über diese API verfügbaren aussagefähigere Datasets nutzen können.

Cloud Cruiser kann die Integration mit der Verwendung-API auf verschiedene Weise nutzen. Die Genauigkeit (stündlich Verwendungsinformationen) und Informationen zur Ressource Metadaten über die API zur Verfügung stellt das notwendigen Dataset aus, um flexible Showback oder Chargeback Modelle. 

In diesem Lernprogramm präsentiert wir ein Beispiel dafür, wie Cloud Cruiser Verwendung API Informationen profitieren kann. Wir werden genauer, erstellen eine Ressourcengruppe auf Azure, ordnen Sie Tags für die Kontostruktur, und beschreiben die Vorgehensweise zum Abrufen und Verarbeiten der Kategorie Informationen in die Cloud Cruiser.
 
Das endgültige Ziel ist in der Lage, Kosten und Verbrauch basierend auf der Kontostruktur aufgefüllt, indem Sie die Kategorien analysieren und Erstellen von Berichten, wie im folgenden Lage sein.

![Abbildung 10 - Bericht mit Abstürzen mithilfe von Kategorien][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-Kategorien

Die Daten über die Azure-API Verwendung verfügbar enthält nicht nur Verbrauchsinformationen, sondern auch Ressourcenmetadaten einschließlich beliebige Tags zugeordnet. Kategorien eine einfache Möglichkeit zum Organisieren Ihrer Ressourcen, doch um den gewünschten bereitstellen zu können, müssen Sie sicherstellen, dass:

- Tags werden richtig auf den Ressourcen bereitstellen gleichzeitig angewendet.
- Tags werden ordnungsgemäß auf den Prozess Showback/Chargeback verwendet, um die Verwendung zu Konto-Struktur der Organisation zu verbinden.

Beide diese Anforderungen können anspruchsvoll, sein, besonders, wenn eines manuellen Prozesses auf die Erbringung oder die Seite geladen. Schreibfehler, falsche oder sogar fehlende Tags sind allgemeine Beschwerden von Kunden aus, wenn Sie mithilfe von Tags und dieser Fehler Nutzungsdauer auf der Seite wird geladen sehr erschweren kann.

Mit der neuen Azure Verwendung API können Cloud Cruiser tagging Ressourceninformationen extrahieren, und beheben über ein anspruchsvolle ETL-Tool Arbeitsmappen aufgerufen, diese häufigen tagging. Durch Umwandlung mithilfe von regulären Ausdrücken und Daten Korrelationskoeffizienten Cloud Cruiser falsch markierte Ressourcen identifizieren und anwenden die richtigen Kategorien, um die richtige Zuordnung der Ressourcen mit dem Consumer:

Klicken Sie auf der Seite wird geladen können Cloud Cruiser Automatisierung den Prozess Showback/Chargeback und die Kategorie Informationen zum Einbinden der Verwendung an den entsprechenden Consumer (Abteilung, Division, Project, usw.). Diese Automatisierung bietet eine große Verbesserung und kann einen konsistenten und überwachten wird geladen Prozess sicherzustellen.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Erstellen einer Ressourcengruppe mit Microsoft Azure Kategorien
In diesem Lernprogramm der erste Schritt besteht im Erstellen einer Ressourcengruppe im Azure-Portal erstellen Sie neue Kategorien, die Ressourcen zuordnen möchten. In diesem Beispiel wir erstellen die folgenden Tags: Abteilung,-Umgebung, Besitzer, Projekt.

Das Bildschirmabbild unten zeigt eine Stichprobe Ressourcengruppe mit den zugehörigen Tags.

![Abbildung 11: Ressourcengruppe mit zugeordneten Kategorien Azure-portal][11]

Im nächsten Schritt wird, die Informationen aus der Verwendung-API in der Cloud Cruiser ziehen. Die Verwendung-API umfasst derzeit von Antworten im JSON-Format. Hier ist ein Beispiel für die Daten, die abgerufen:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Importieren von Daten aus der Verwendung-API in der Cloud Cruiser

Cloud Cruiser Arbeitsmappen bieten eine automatisierte Möglichkeit zum Sammeln und Verarbeiten von Informationen aus der Verwendung-API. Eine ETL-Arbeitsmappe (Extrahieren Transformation laden) können Sie die Auflistung, Transformation und Veröffentlichung von Daten in die Cloud Cruiser Datenbank konfigurieren.

Jede Arbeitsmappe kann eine oder mehrere Websitesammlungen verfügen. So können Sie zu koordinieren Informationen aus verschiedenen Quellen ergänzen oder erweitern die Verwendungsdaten. In diesem Beispiel werden wir ein neues Blatt in der Arbeitsmappe Azure Vorlage erstellen (_UsageAPI)_ , und legen Sie eine neue _Websitesammlung_ zum Importieren von Informationen aus der Verwendung-API.

![Abbildung 3 - API Verwendungsdaten in das Blatt UsageAPI importiert wurden][12]

Beachten Sie, dass diese Arbeitsmappe bereits Arbeitsblätter Services aus Azure (_ImportServices_) zu importieren und verarbeiten Verbrauchsinformationen aus der Abrechnung-API (_PublishData_) enthält.

Weiter Wir verwenden die API Verwendung des Blatts _UsageAPI_ gefüllt wird, und diese Informationen mit der Verbrauchsdaten aus der Abrechnung-API auf dem Blatt _PublishData_ .

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verarbeiten der Kategorie Informationen aus der Verwendung-API

Nach dem Import der Daten in der Arbeitsmappe ein, wird wir Transformation Schritte auf dem Blatt _UsageAPI_ erstellen, um die Informationen aus der-API zu verarbeiten. Erste Schritt besteht einen "JSON Teilen" Prozessor verwenden, um die Tags aus einem einzigen Feld Extrahieren Erstellen von Feldern für jede einzelne davon (Abteilung, Project, Besitzer und Umgebung).

![Abbildung 4: Erstellen von neuen Felder für die Tag-Informationen][13]

Beachten Sie der Dienst "Netzwerke" fehlt die Kategorie Informationen (gelben Feld), aber wir können überprüfen, ob es Teil der gleichen Ressourcengruppe ist, indem Sie das Feld _ResourceGroupName_ ein. Da wir die Tags für die anderen Ressourcen aus dieser Ressourcengruppe haben, können wir diese Informationen verwenden, um die fehlende Tags Ressource später im Prozess anwenden.

Im nächste Schritt besteht im Erstellen einer Nachschlagetabelle wird die Informationen aus der Kategorien auf der _ResourceGroupName_. Diese Nachschlagetabelle wird im nächsten Schritt in der Verbrauchsdaten Kategorie Informationen bereichern verwendet werden.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Hinzufügen der Kategorie Informationen zur der Verbrauchsdaten

Nun können wir Springen zum _PublishData_ Blatt, das von der Abrechnung-API Verbrauchsinformationen verarbeitet, und fügen Sie die Felder aus der Kategorien extrahiert. Dieser Prozess wird ausgeführt, indem Sie die Nachschlagetabelle mit der _ResourceGroupName_ als Schlüssel für die Suchvorgänge im vorherigen Schritt erstellt.

![Abbildung 5: die Kontostruktur mit den Informationen aus der Suchvorgänge Auffüllen][14]

Beachten Sie, dass die entsprechende Konto Strukturfelder für den Dienst "Netzwerke" angewendet wurden, beheben das Problem mit den fehlenden Tags. Wir aufgefüllt auch Konto Strukturfelder für Ressourcen als unsere Ziel Ressourcengruppe mit "Andere", um diese auf die Berichte zu unterscheiden.

Jetzt müssen wir nur einen Schritt aus, um die Verwendungsdaten veröffentlichen hinzufügen. In diesem Schritt werden die entsprechenden Sätzen für jeden Dienst an unserem Zins planen, die definiert auf die Informationen zur Verwendung mit der resultierende Gebühr in die Datenbank geladen angewendet werden.

Das beste ist, dass Sie dieses Verfahren einmal durchgehen müssen. Wenn die Arbeitsmappe abgeschlossen ist, müssen Sie nur der Scheduler hinzugefügt und wird sie stündlich oder täglich zum geplanten Zeitpunkt ausgeführt. Klicken Sie dann ganz einfach eine Frage der neue Berichte erstellen oder Anpassen der bereits vorhandenen Dateien und Analysieren der Daten, um aussagekräftige Einsichten aus der Cloud Verwendung zu erhalten.

### <a name="next-steps"></a>Nächste Schritte

+ Detaillierte Informationen zum Erstellen von Arbeitsmappen Cloud Cruiser und Berichte finden Sie in der Cloud Cruiser online [Dokumentation](http://docs.cloudcruiser.com/) (gültige Anmeldung erforderlich).  Weitere Informationen zu Cloud Cruiser, wenden Sie sich an [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Eine Übersicht über die Azure Ressource: Einsatz und RateCard APIs finden Sie unter [Einblicke in Ihrer Microsoft Azure Ressourcenverbrauch zu erhalten](billing-usage-rate-card-overview.md) .
+ Schauen Sie sich die [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für Weitere Informationen zu beiden APIs, die Teil der Satz von APIs vom Azure-Manager bereitgestellt werden.
+ Wenn Sie rechts in der Stichprobe Code kümmern möchten, schauen Sie sich unsere Microsoft Azure Abrechnung API Codebeispielen auf [Azure Codebeispielen](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Weitere Informationen
+ Finden Sie im Hilfeartikel [Azure Ressourcenmanager Übersicht](azure-resource-manager/resource-group-overview.md) erfahren Sie mehr über Azure-Manager aus.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Abbildung 1: Erstellen einer neuen Sammlung"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Abbildung 2: Importieren von Daten aus der neuen Sammlung"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Abbildung 3: Transformation Schritte zum Verarbeiten gesammelter Daten aus RateCard-API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Abbildung 4: veröffentlichen die Daten aus der RateCard-API als neue Dienste und Kostensätzen"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Abbildung 5: überprüfen die neuen Dienste"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Abbildung 6: Überprüfen des neuen Zins planen und der zugehörigen Sätzen"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Abbildung 7: WAP Datentransformation um Services normalisieren"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Abbildung 8 - Planung der Arbeitsmappe"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Abbildung 9: Beispielbericht für die Arbeitsbelastung Kosten Vergleich Szenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Abbildung 10 - Bericht mit Abstürzen mithilfe von Kategorien"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Abbildung 11: Ressourcengruppe mit zugeordneten Kategorien Azure-portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Abbildung 12 - API Verwendungsdaten in das Blatt UsageAPI importiert wurden"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Abbildung 13: Erstellen von neuen Felder für die Tag-Informationen"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Abbildung 14: die Kontostruktur mit den Informationen aus der Suchvorgänge Auffüllen"

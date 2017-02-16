<properties
   pageTitle="Auswerten von Berichten mithilfe der JavaScript-API | Microsoft Azure"
   description="Power BI eingebettete, Auswerten von Berichten mithilfe der JavaScript-API"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Interagieren Sie mit Power BI-Berichte mithilfe der JavaScript-API

Die Power BI-JavaScript-API können Sie ganz einfach Power BI-Berichten in Ihre Apps einbetten. Mit der-API können Ihre Programme programmgesteuert mit anderen Berichtselemente wie Seiten und Filter interagieren. Diese Interaktion macht Power BI-Berichte einen Weitere integrierten Teil Ihrer Anwendung.

Sie einbetten mithilfe von einem Iframe, der gehostet wird als Teil der Anwendung ein Berichts von Power BI in Ihrer Anwendung. Der Iframe fungiert als eine Begrenzungslinie zwischen der Anwendung und den Bericht, wie Sie in der folgenden Abbildung sehen können. 

![Power BI eingebettete Iframe ohne Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Der Iframe vereinfacht den Einbetten von Prozess sehr ähnlich, aber ohne die JavaScript-API den Bericht und Ihrer Anwendung können nicht miteinander interagieren. Der Bericht nicht wirklich Teil der Anwendung ist Gefühl stellen diese fehlende Interaktion. Der Bericht und die Anwendung müssen wirklich hin und her, wie die folgende Abbildung kommunizieren.

![Power BI eingebettete Iframe mit Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Die Power BI-JavaScript-API ermöglicht es Ihnen, Code zu schreiben, die die Begrenzungslinie Iframe sicher passieren kann. Dies ermöglicht Ihrer Anwendung programmgesteuert eine Aktion in einem Bericht ausführen und Ereignisse von Aktionen zu überwachen, die Benutzer innerhalb des Berichts vornehmen.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Was können Sie mit der Power BI-JavaScript-API?
Mit der JavaScript-API können Sie Berichte verwalten, navigieren Sie zu Seiten in einem Bericht, Filtern eines Berichts und das Einbetten von Ereignissen zu behandeln. Das folgende Diagramm veranschaulicht die Struktur der API.

![Power BI-JavaScript-API-Diagramm](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Verwalten von Berichten
Die Javascript-API können Sie Verhalten auf der Ebene Berichts und die Seitenzahl verwalten:

- Einbetten eines bestimmten Power BI-Berichts sicher in Ihrer Anwendung - versuchen Sie es der [Anwendung Demo einbetten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Access-Token festlegen
- Konfigurieren des Berichts
  - Aktivieren und Deaktivieren der Filterbereich und Seitennavigationsbereich: versuchen Sie die [Einstellungen Demo Anwendung aktualisieren](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Festlegen der Standardeinstellungen für Seiten und Filter - versuchen Sie, das [Festlegen von Standardeinstellungen demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Geben Sie ein, und Beenden des Vollbildmodus

[Weitere Informationen zum Einbetten eines Berichts](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Navigieren Sie zu Seiten in einem Bericht
Die JavaScript-API Enbales Sie alle Seiten in einem Bericht zu ermitteln und die aktuelle Seite einrichten. Versuchen Sie die [Navigation Demo-Anwendung](http://azure-samples.github.io/powerbi-angular-client/#/scenario3)aus.

[Erfahren Sie mehr über die Seitennavigation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtern eines Berichts
Die JavaScript-API bietet Standard- und erweiterte Filterfunktionen für eingebettete Berichte und Berichtseiten. Versuchen Sie die [Filtern Demo-Anwendung](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), und überprüfen Sie hier einige Einführung Code.  


#### <a name="basic-filters"></a>Einfache Filter
Ein einfacher Filter auf eine Spalte oder Hierarchie Ebene platziert wird und enthält eine Liste mit Werten zur ein- oder auszuschließen.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Erweiterte Filter
Erweiterte Filter verwenden Sie den logischen Operator und oder oder, und eine oder zwei Bedingungen, jede mit ihren eigenen Operator und Wert annehmen. Unterstützte Bedingungen sind:

- Keine
- LessThan
- LessThanOrEqual
- Größer als
- GreaterThanOrEqual
- Enthält
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Ist
- IsNot
- ISTLEER
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Weitere Informationen zum Filtern](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Behandeln von Ereignissen
Zusätzlich zum Senden von Informationen in den Iframe, empfängt die Anwendung kann auch Informationen über die folgenden Ereignisse aus den Iframe stammen:

- Einbinden
  - geladen
  - Fehler
- Berichte
  - pageChanged
  - DataSelected (in Kürze verfügbar)

[Weitere Informationen zum Verarbeiten von Ereignissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Power BI-JavaScript-API die folgenden Links zu Artikeln:

- [Wiki-JavaScript-API](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Objektmodellreferenz](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Beispiele
  - [Winkel](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Live-demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)

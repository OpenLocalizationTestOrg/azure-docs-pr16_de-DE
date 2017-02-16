<properties
    pageTitle="Fragen zum Verwendung | Microsoft Azure"
    description="Liste der Azure Stapel Meter, Vergleich Azure Verwendung API, Verwendung Zeit und Zeit gemeldet, Fehlercodes."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Azure Stapel Verwendung API häufig gestellte Fragen
Dieser Artikel beantwortet häufig gestellten Fragen zur Verwendung Stapel Azure-API.

## <a name="what-meter-ids-can-i-see"></a>Welche Meter IDs kann ich sehen?

Verwendung wird derzeit für das Netzwerk, Speicher und berechnen Ressourcenanbieter gemeldet.

| **Anbieter für Ressourcen** | **Meter-ID** |**Meter Namen** | **Einheit** | **Weitere Informationen** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Netzwerk** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Verwendung von öffentlichen IP-Adresse | IP-Adresse |                    
| **Speicher**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*Stunden | Gesamte Kapazität verbraucht von Tabellen |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*Stunden | Gesamte Kapazität von Seitenblobs verbraucht |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*Stunden  | Gesamte Kapazität von Warteschlange verbraucht |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*Stunden  | Gesamte Kapazität von blockieren Blobs verbraucht |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Zählen in 10,000s anfordern   | Tabelle Serviceanfragen (10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Eingehende Daten in GB | Tabelle Dienst Daten eingehende in GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress in GB | Tabelle Dienst Daten Ausgang in GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Besprechungsanfragen zählen in 10,000s | BLOB Serviceanfragen (10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Eingehende Daten in GB          | BLOB-Dienst Daten eingehende in GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress in GB              | BLOB-Dienst Daten Ausgang in GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Besprechungsanfragen zählen in 10,000s   | Warteschlange Serviceanfragen (10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Eingehende Daten in GB         | Warteschlange-Dienst Daten eingehende in GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress in GB  | Warteschlange-Dienst Daten Ausgang in GB 
| **Berechnen** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | Virtueller Computer Größe Stunden | Größe des virtuellen Computers |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Wie vergleichen gehen Sie wie folgt die Azure Stapel Verwendung APIs mit der [Azure Verwendung API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (aktuell in public Preview-Version)?

-   Die Mandanten Verwendung-API ist konsistent mit der Azure-API, mit einer Ausnahme: die Kennzeichnung *ShowDetails* wird derzeit nicht in Azure Stapel unterstützt.

-   Die Verwendung Provider-API gilt nur für Azure Stapel.

-   Die [RateCard-API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) , die in Azure zur Verfügung ist derzeit nicht in Azure Stapel verfügbar.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Was ist der Unterschied zwischen der Verwendung Zeit und gemeldet?

Daten Verwendungsberichte stehen zwei Hauptfenster Zeitwerte an:

-   **Zeit gemeldet**. Die Uhrzeit, die das Ereignis für die Verwendung des Systems für die Verwendung der Eingabe

-   **Zeitpunkt der Verwendung**. Die Uhrzeit, wann die Ressource Azure Stapel verbraucht wurde

Sie möglicherweise eine Differenz in Werte für Verwendung Zeit gemeldet für eine bestimmte Verwendung Ereignis angezeigt. Die Verzögerung kann mehrere Stunden in jeder Umgebung lang sein.

Derzeit können Sie *nur nach Zeiten gemeldet*Abfragen.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Was bedeuten diese Fehlercodes Verwendung API?

| **HTTP-Statuscode** | **Fehlercode** | **Beschreibung** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400-Ungültige Anforderung        | *NoApiVersion*     | Der Abfrageparameter *api-Version* ist nicht vorhanden.
| 400-Ungültige Anforderung        | *InvalidProperty*  | Eine Eigenschaft fehlt oder weist einen ungültigen Wert. Die Nachricht in der Fehlercode in den Textkörper der Antwort bezeichnet die fehlende Eigenschaft.
| 400-Ungültige Anforderung        | *RequestEndTimeIsInFuture*  | Der Wert für *ReportedEndTime* ist in der Zukunft aus. Werte sind für dieses Argument in der Zukunft nicht zulässig.
| 400-Ungültige Anforderung        | *SubscriberIdIsNotDirectTenant*    | Ein Anruf Anbieter API verwendet eine Abonnement-ID, die nicht auf einen gültigen Mandanten des Anrufers ist.
| 400-Ungültige Anforderung        | *SubscriptionIdMissingInRequest*   | Die Anrufer-ID Abonnement ist nicht vorhanden.
| 400-Ungültige Anforderung        | *InvalidAggregationGranularity*   | Ein ungültiges Aggregation Genauigkeit es wurde angefordert. Gültige Werte sind tägliche und pro Stunde an.
| 503                    | *ServiceUnavailable*   | Behebbarer Fehler, da der Dienst ausgelastet ist oder der Anruf beschränkt. |

## <a name="next-steps"></a>Nächste Schritte
[Kunden Abrechnung und Chargeback in Azure Stapel](azure-stack-billing-and-chargeback.md)

[Anbieter Ressource: Einsatz API](azure-stack-provider-resource-api.md)

[Ressource: Einsatz API-Mandanten](azure-stack-tenant-resource-usage-api.md)

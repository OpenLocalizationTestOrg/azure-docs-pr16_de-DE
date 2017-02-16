<properties 
    pageTitle="Wie melde Ereignisse Azure Ereignis Hubs in Azure-API Management | Microsoft Azure" 
    description="Erfahren Sie, wie Ereignisse in Azure Ereignis Hubs in Azure-API Management protokollieren." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Wie melde Azure Ereignis Hubs in Azure-API Management Ereignisse

Azure Ereignis Hubs ist eine hochgradig skalierbare eingehende Datendienst, der Millionen Ereignisse pro Sekunde Aufnahme kann, damit Sie verarbeiten und großer Datenmengen, von Ihrer verbundenen Geräte und Applikationen erstellte analysieren können. Hubs Ereignisses als "Vordertür" für ein Ereignis Verkaufspipeline fungiert, und nachdem die Daten in einem Ereignis-Hub erfasst werden, umgewandelt werden können und mithilfe eines beliebigen Anbieters in Echtzeit Analytics oder Batchverarbeitung/Speicher Netzwerkadapter gespeichert. Ereignis Hubs trennt die Herstellung eines Streams von Ereignissen aus der Verarbeitung diese Ereignisse, sodass Ereignisempfänger der Ereignisse in ihrem eigenen Zeitplan zugreifen können.

In diesem Artikel ist eine Ergänzung zu [Integrieren Azure-API Management mit Ereignis Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) Videos und beschreibt, wie Sie mit Azure Ereignis Hubs API Management-Ereignisse protokollieren.

## <a name="create-an-azure-event-hub"></a>Erstellen Sie einen Ereignis Azure-Hub

Zum Erstellen einer neuen Ereignis Hub [Azure klassischen Portal](https://manage.windowsazure.com) anmelden, und klicken Sie auf **neu**->**App Services**->**Dienstbus**->**Ereignis Hub**->**Symbolleiste erstellen**. Geben Sie einen ein Ereignis Hub Region, wählen Sie ein Abonnement aus, und wählen Sie einen Namespace. Wenn Sie einen Namespace zuvor erstellt haben, können Sie eine durch eingeben einen Namen in das Textfeld **Namespace** erstellen. Nachdem Sie alle Eigenschaften konfiguriert haben, klicken Sie auf **erstellen ein neues Ereignis Hub** , um das Ereignis Hub zu erstellen.

![Erstellen von Ereignis-hub][create-event-hub]

Als Nächstes navigieren Sie zur Registerkarte **Konfigurieren** für Ihr neues Ereignis Hub und erstellen Sie zwei **freigegebenen-Richtlinien**. Benennen Sie das erste Bild **Senden** , und probieren Sie es mit Berechtigungen **Senden** .

![Senden die Richtlinie][sending-policy]

Benennen Sie den zweiten **empfangen**, probieren Sie es mit **Abhören** Berechtigungen, und klicken Sie auf **Speichern**.

![Empfängt eine Richtlinie][receiving-policy]

Jede freigegebenen Zugriffsrichtlinie ist die Anwendung senden und Empfangen von Ereignissen an und von den Hub Ereignis. Für den Zugriff auf die Verbindungszeichenfolgen für diese Richtlinien navigieren Sie zu der Registerkarte ' **Dashboard** ' Ereignis-Hub, und klicken Sie auf die **Verbindungsinformationen**.

![Verbindungszeichenfolge][event-hub-dashboard]

Die Verbindungszeichenfolge **Senden** wird beim Protokollieren von Ereignissen verwendet, und die Verbindungszeichenfolge **empfangen** wird verwendet, wenn Ereignisse vom Ereignis Hub herunterladen.

![Verbindungszeichenfolge][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Erstellen einer API Management-Protokollierung

Jetzt, da Sie ein Ereignis Hub haben, besteht der nächste Schritt [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx) in Ihrer API Verwaltungsdienst konfigurieren, damit es Ereignisse an den Hub Ereignis anmelden kann.

Projektmanagement-API zur Aufzeichnung der Tastatureingabe werden mithilfe der [API Management REST-API](http://aka.ms/smapi)konfiguriert. Überprüfen Sie bevor Sie verwenden die REST-API zum ersten Mal die [erforderlichen Komponenten](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) , und stellen Sie sicher, dass Sie [Zugriff auf die REST-API aktiviert](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI)haben.

Stellen Sie eine setzen HTTP-Anforderung mithilfe der folgenden URL-Vorlage zum Erstellen einer Protokollierung.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Ersetzen Sie `{your service}` mit dem Namen der Ihrer API Management Services-Instanz.
-   Ersetzen Sie `{new logger name}` mit den gewünschten Namen für Ihre neue Protokollierung. Dieser Name wird verwiesen werden, wenn Sie die Richtlinie [Log-zu-Eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) konfigurieren

Die folgenden Überschriften der Anforderung hinzufügen.

-   Inhaltstyp: Anwendung/json
-   Autorisierung: SharedAccessSignature Uid =...
    -   Anweisungen zum Generieren der `SharedAccessSignature` finden Sie unter [Azure API Management REST API Authentifizierung](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Geben Sie den Text der Anforderung mit der folgenden Vorlage an.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`muss auf festgelegt sein `AzureEventHub`.
-   `description`Stellt eine optionale Beschreibung für die Protokollierung und falls gewünscht eine Zeichenfolge der Länge 0 (null) werden können.
-   `credentials`enthält die `name` und `connectionString` von Ihrer Azure-Hub Ereignis.

Wenn vorgenommenen die Anforderung, wenn die Protokollierung ein Codes Status des erstellt wurde `201 Created` zurückgegeben. 

>[AZURE.NOTE] Andere mögliche Rückgabewerte und deren Gründe finden Sie unter [Erstellen einer Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Sehen wie andere Vorgänge ausführen, wie z. B. Listen-, aktualisieren und löschen, die [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx) Entität Dokumentation finden Sie unter.

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurieren der Log-Eventhubs-Richtlinien

Nachdem eine Protokollierung in API Management konfiguriert ist, können Sie Ihre Log-Eventhubs-Richtlinien zum Melden Sie die gewünschten Ereignisse konfigurieren. Die Log-Eventhubs-Richtlinie kann entweder im Abschnitt eingehende Richtlinie oder im Abschnitt Richtlinie für ausgehenden Datenverkehr verwendet werden.

Klicken Sie zum Konfigurieren von Richtlinien [Azure klassischen Portal](https://manage.windowsazure.com)anmelden navigieren Sie Ihrer API Verwaltungsdienst, und klicken Sie entweder **Publisher-Portal** oder **Verwalten** , um das Publisher-Portal zugreifen.

![Publisher-portal][publisher-portal]

Klicken Sie im Menü API Management auf der linken Seite auf **Richtlinien** , wählen Sie das gewünschte Produkt und API, und klicken Sie auf **Richtlinie hinzufügen**. In diesem Beispiel haben wir an die **Echo-API** in das Produkt **unbeschränkt** eine Richtlinie hinzufügen.

![Richtlinie hinzufügen][add-policy]

Positionieren Sie den Cursor in die `inbound` Richtlinie Abschnittregister, und klicken Sie auf die Richtlinie **Log to EventHub** zum Einfügen der `log-to-eventhub` Richtlinie Abrechnungsvorlage.

![Gruppenrichtlinien-editor][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Ersetzen Sie `logger-id` mit dem Namen der Protokollierung-API Management Sie im vorherigen Schritt konfiguriert.

Sie können ein Ausdruck, der eine Zeichenfolge, als der Wert für zurückgibt die `log-to-eventhub` Element. In diesem Beispiel wird eine Zeichenfolge mit dem Datum und Uhrzeit, Service Name, Id anfordern, Anforderung IP-Adresse und Name des Vorgangs protokolliert.

Klicken Sie auf **Speichern** , um die Konfiguration aktualisierte Richtlinie zu speichern. Sobald sie gespeichert haben, ist die Richtlinie ist aktiv, und Ereignisse im vorgesehenen Ereignis Hub angemeldet sind.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zu Azure Ereignis Hubs
    -   [Erste Schritte mit Azure Ereignis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Empfangen von Nachrichten mit EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Ereignis Hubs programming guide](../event-hubs/event-hubs-programming-guide.md)
-   Erfahren Sie mehr über die Integration von Management-API und Ereignis Hubs
    -   [Ein Verweis auf Protokollierung Entität](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Log-Eventhub-Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Überwachen Sie Ihre APIs mit Azure-API Management, Ereignis Hubs und Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Schauen Sie sich eine video Exemplarische Vorgehensweise

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png







<properties 
    pageTitle="Empfehlungen für maschinelle Schulung: JavaScript-Integration | Microsoft Azure" 
    description="Azure maschinellen Learning Empfehlungen - Integration von JavaScript - Dokumentation" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Lernen Empfehlungen - JavaScript-Integration Azure-Computern

>[AZURE.NOTE]Starten Sie mithilfe von Empfehlungen-API kognitive Service statt dieser Version. Kognitive Empfehlungen Dienst ersetzt werden soll diesen Dienst, und alle neuen Features werden es entwickelt werden. Es hat neue Funktionen wie Batchverarbeitung Support, ein besseres API-Explorer, eine übersichtlichere API einbinden, konsistenter beim Registrieren/Abrechnung Erfahrung usw. aus.
> Weitere Informationen zum [Migrieren zu den neuen kognitive Dienst](http://aka.ms/recomigrate)


Dieses Dokument darzustellen, wie Sie Ihrer Website mithilfe von JavaScript zu integrieren. Das JavaScript können Sie zum Senden von Daten Acquisition Ereignisse und Empfehlungen nutzen, nachdem Sie ein Empfehlungen-Modell zu erstellen. Alle Vorgänge abgeschlossen über JS können auch über serverseitigen erfolgen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. allgemeine Übersicht
Integration Ihrer Website mit Azure ML Empfehlungen auf 2 Phasen bestehen aus:

1.  Senden Sie Ereignisse in Azure ML Empfehlungen. Dadurch wird um ein Empfehlungen-Modell zu erstellen.
2.  Nutzen Sie die Empfehlungen ein. Nach der Erstellung des Modells können Sie die Empfehlungen nutzen. (Lesen Sie dieses Dokument erklärt nicht so erstellen Sie ein Modell, das Schnellstarthandbuch, um weitere Informationen zu erhalten).


<ins>Phase I</ins>

In der ersten Phase fügen Sie in Ihren HTML-Seiten eine kleine JavaScript-Bibliothek, die ermöglicht das Zeichenblatt, um Ereignisse zu senden, sobald sie auf der Seite HTML-Code in Azure ML Empfehlungen-Servern (über Data Market) auftreten:

![Zeichnung1][1]

<ins>Phase II</ins>

In der zweiten Phase, wenn die Empfehlungen auf der Seite angezeigt werden sollen, wählen Sie eine der folgenden Optionen aus:

1.Klicken der Server (auf der Phase des Renderings von Seiten) Ruf Azure ML Empfehlungen-Server (über Data Market), um Empfehlungen abzurufen. Die Ergebnisse enthalten eine Liste der Elemente-Id an. Der Server muss die Ergebnisse mit den Elementen Metadaten (z. B. Bilder, Beschreibung) bereichern und Senden der erstellten Seite an den Browser.

![Drawing2][2]

2.Klicken eine weitere Möglichkeit besteht darin, kleine JavaScript-Datei aus einer Phase eine verwenden, können Sie um eine einfache Liste mit empfohlenen Elemente zu gelangen. Die hier empfangenen Daten sind als der Zeile in die erste Option schlankere.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. Voraussetzungen für

1. Erstellen eines neuen Modells mithilfe der APIs. Finden Sie unter Erste Schritte auf Vorgehensweise ein.
2. Codieren Ihrer &lt;DataMarketUser&gt;:&lt;DataMarketKey&gt; mit base64. (Dies wird für die Standardauthentifizierung verwendet zum Aktivieren des Codes JS die APIs aufrufen).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3 senden Sie 3 Ihren Daten Acquisition Ereignisse mithilfe von JavaScript
Die folgenden Schritte zu sendende Ereignisse erleichtern:

1.  JQuery-Bibliothek in Ihren Code aufnehmen. Sie können ihn von Nuget in der folgenden URL herunterladen.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Einschließen die Empfehlungen JavaScript-Bibliothek aus den folgenden URL: http://aka.ms/RecoJSLib1

3.  Initialisierung Azure ML Empfehlungen-Bibliothek mit den entsprechenden Parametern an.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Senden Sie das entsprechende Ereignis. Finden Sie ausführliche Abschnitt unten auf alle Arten von Ereignissen (Beispiel klicken Sie auf Ereignis)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Einschränkungen und Browserunterstützung
Dies ist eine Implementierung verweisen, und es wird angegeben, denn damit ist. Sie sollten alle gängigen Browser unterstützen.

###<a name="32-type-of-events"></a>3,2. Arten von Ereignissen
Es gibt 5 Arten von Ereignis, die die Bibliothek unterstützt: Klicken Sie auf, klicken Sie auf Empfehlungen, hinzufügen oder professionellem Einkaufswagen daraus Kopiershop Einkaufswagen und erwerben. Es ist ein weiteres Ereignis, mit dem den Benutzerkontext mit dem Namen Login festlegen.

####<a name="321-click-event"></a>3.2.1. Klicken Sie auf Ereignis
Dieses Ereignis sollte jederzeit ein Benutzer geklickt haben, klicken Sie auf ein Element verwendet werden. In der Regel, wenn Benutzer auf ein Element klickt wird eine neue Seite mit den Elementdetails geöffnet. auf dieser Seite sollte das Ereignis ausgelöst wurde.

Parameter:
- Ereignis (String, obligatorischer) – "klicken Sie auf"
- Element (String, obligatorischer) - eindeutige ID des Elements
- Elementname (String, optional) – der Name des Elements
- ItemDescription (String, optional) – die Beschreibung des Artikels
- ItemCategory (String, optional) – der Kategorie des Elements
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Oder optionale Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Empfehlungen klicken Sie auf Ereignis
Dieses Ereignis sollte die jederzeit ein Benutzer geklickt haben, klicken Sie auf ein Element, das aus Azure ML Empfehlungen als empfohlen Artikel empfangen wurde verwendet werden. In der Regel, wenn Benutzer auf ein Element klickt wird eine neue Seite mit den Elementdetails geöffnet. auf dieser Seite sollte das Ereignis ausgelöst wurde.

Parameter:
- Ereignis (String, obligatorischer) - "Recommendationclick"
- Element (String, obligatorischer) - eindeutige ID des Elements
- Elementname (String, optional) – der Name des Elements
- ItemDescription (String, optional) – die Beschreibung des Artikels
- ItemCategory (String, optional) – der Kategorie des Elements
- basiert (Zeichenfolgenarray, optional) - Ausgangswerte für den, der die Abfrage Empfehlungen generiert.
- RecoList (Zeichenfolgenarray, optional) – das Ergebnis der Anfrage empfohlen, die das Element generiert, auf die geklickt wurde.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Oder optionale Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Warenkorb Einkaufswagen Ereignis hinzufügen
Sollte dieses Ereignis verwendet werden, wenn der Benutzer ein Element zu einem Einkaufswagen hinzufügen.
Parameter:
* Ereignis (String, obligatorischer) - "Addshopcart"
* Element (String, obligatorischer) - eindeutige ID des Elements
* Elementname (String, optional) – der Name des Elements
* ItemDescription (String, optional) – die Beschreibung des Artikels
* ItemCategory (String, optional) – der Kategorie des Elements
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Entfernen Sie Einkaufs-Einkaufswagen Ereignis
Dieses Ereignis sollte verwendet werden, wenn ein Benutzer ein Element aus dem Einkaufskorb entfernt.

Parameter:
* Ereignis (String, obligatorischer) - "Removeshopcart"
* Element (String, obligatorischer) - eindeutige ID des Elements
* Elementname (String, optional) – der Name des Elements
* ItemDescription (String, optional) – die Beschreibung des Artikels
* ItemCategory (String, optional) – der Kategorie des Elements
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Language Pack für-Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer seinen Einkaufswagen erworben haben.

Parameter:
* Ereignis (String) – "erwerben"
* bestellten ([]) - Matrix, halten einen Eintrag für jedes Element erworben haben.<br><br>
Erworbenen Format:
    * Element (String) - eindeutige ID des Elements.
    * zählen Sie (Ganzzahl oder Zeichenfolge) - Anzahl der gekauften Artikel an.
    * Kurs (Pufferzeiten oder Zeichenfolge) – optional Feld - den Preis des Elements.

Das folgende Beispiel zeigt den Kauf von 3 Elemente (33, 34, 35), zwei mit alle Felder aufgefüllt (Element, Anzahl, Kurs) und (Artikel 34) ohne einen Preis.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Benutzer Login Ereignis
Azure ML Empfehlungen Ereignis Bibliothek erstellt und verwenden ein Cookies akzeptieren, um Ereignisse zu identifizieren, die im selben Browserfenster stammen. Um die Modellergebnisse zu verbessern, die Azure ML Empfehlungen ermöglicht eine eindeutige Benutzer-ID festlegen, die die Verwendung von Cookies überschrieben werden.

Dieses Ereignis sollte nach der Anmeldung des Benutzers zu Ihrer Website verwendet werden.

Parameter:
* Ereignis (String) – "Userlogin"
* Benutzer (String) - eindeutige Kennung des Benutzers.
        <script>
  Wenn (Typeof AzureMLRecommendationsEvent == "Undefiniert") {AzureMLRecommendationsEvent = [;]} AzureMLRecommendationsEvent.push ({Ereignis: "Userlogin," Benutzer: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. Empfehlungen über JavaScript nutzen
Der Code, der empfohlen wird durch den Kundennamen Webseite durch einige JavaScript-Ereignis ausgelöst. Empfehlungen Antwort enthält die empfohlenen Elemente Ids, deren Namen und deren Bewertungen. Es empfiehlt sich, diese Option nur für die Listenanzeige einer für die empfohlenen Elemente zu verwenden: komplexere Nachrichtenbehandlung (wie etwa das Hinzufügen von Metadaten des Elements) auf der Seite Server-Integration erfolgen sollte.

###<a name="41-consume-recommendations"></a>4.1 nutzen Sie Empfehlungen
Um nutzen Empfehlungen, die, denen Sie, um benötigen, die erforderlichen JavaScript-Bibliotheken in Ihrer Seite und einbeziehen AzureMLRecommendationsStart anrufen Siehe Abschnitt 2.

Empfehlungen für ein oder mehrere Elemente, die Sie eine Methode namens anrufen müssen beanspruchen: AzureMLRecommendationsGetI2IRecommendation.

Parameter:
* Elemente (Array von Zeichenfolgen -) eines oder mehrerer Elemente können Sie Empfehlungen für zu gelangen. Wenn Sie eine eigene Fbt nutzen, können Sie hier nur ein Element festlegen.
* NumberOfResults (Int) - Anzahl der Ergebnisse erforderlich.
* IncludeMetadata (boolesch, optional) – Wenn festlegen auf "True" gibt an, dass das Metadatenfeld im Ergebnis gefüllt werden muss.
* Funktion - eine Funktion, die die Empfehlungen behandelt Verarbeitung zurückgegeben. Die Daten werden als Array von zurückgegeben:
    * Element - Element einmalige Nr.
    * Name - Element Namen (wenn im Katalog vorhanden)
    * Bewertung - Empfehlungen Bewertung
    * Metadaten - eine Zeichenfolge, die Metadaten des Elements darstellt.

Beispiel: Der folgende Code anfordert 8 Empfehlungen für Element "64f6eb0d-947a-4c18-a16c-888da9e228ba" (und nicht angeben IncludeMetadata - er implizit besagt, dass keine Metadaten erforderlich ist), anschließend verkettet die Ergebnisse in einen Puffer.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 

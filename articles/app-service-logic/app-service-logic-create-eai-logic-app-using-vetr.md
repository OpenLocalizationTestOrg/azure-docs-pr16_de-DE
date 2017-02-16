<properties
   pageTitle="Erstellen von EAI Logik App VETR Logik Apps in Azure-App-Dienst verwenden | Microsoft Azure"
   description="Überprüfen, codieren und Funktionen von BizTalk XML Services transformieren"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Erstellen von EAI Logik App mit VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Die meisten Enterprise Application Integration (EAI) Szenarien Daten zwischen einer Quelle und einem Ziel zu vermitteln. Solche Szenarien sind häufig ein Standardsatz von Anforderungen an:

- Stellen Sie sicher, dass die Daten aus anderen Systemen richtig formatiert sind.
- Führen Sie "Suche" auf die richtigen Entscheidungen treffen eingehende Daten ein.
- Konvertieren von Daten von einem Format in ein anderes. Konvertieren von Daten aus einem CRM-System Datenformat beispielsweise in einem ERP-System-Datenformat.
- Weiterleitung von Daten in die gewünschte Anwendung oder im externen System.

Dieser Artikel zeigt Ihnen ein gemeinsames Integration Muster: "unidirektionale Nachricht Vermittlung" oder VETR (Gültigkeitsüberprüfung, Enrich, Transformation, Routing). Das Muster VETR vermittelt Daten zwischen einer Quellentität und eine Zielentität. In der Regel werden Quell-als auch auf Datenquellen.

Erwägen Sie eine Website, die Bestellungen akzeptiert. Benutzer Posten Bestellungen an das System über HTTP. Das System im Hintergrund überprüft die eingehenden Daten für die Richtigkeit, es normalisiert und in einer Warteschlange Dienstbus für die weitere Verarbeitung so lange bestehen. Das System nimmt Bestellungen aus der Warteschlange, wird es in einem bestimmten Format erwartet. Auf diese Weise ist der End-to-End-Fluss aus:

**HTTP** → **Überprüfen** → **Transformieren** → **Dienstbus**

![Grundlegende VETR Fluss][1]

Die folgenden BizTalk-API Apps unterstützen dieses Muster zu erstellen:

* **Auslösen von HTTP** - Datenquelle verfügen, um die Nachrichtenereignis auslösen
* **Prüfen** - überprüft die Richtigkeit der eingehenden Daten
* **Transformieren** – Sie können Daten aus eingehende Format zu formatieren, entfernte System erforderlich machen
* **Service Bus Connector** - Zielentität, in dem Daten gesendet werden


## <a name="constructing-the-basic-vetr-pattern"></a>Bauen das grundlegende VETR Muster
### <a name="the-basics"></a>Die Grundlagen

Im Portal Azure wählen Sie aus **+ neu**, wählen Sie **Web + Mobile**, und wählen Sie dann auf **App Logik**. Wählen Sie einen Namen, Speicherort, Abonnement, Ressourcengruppe und Speicherort, der funktioniert. Ressourcengruppen dienen als Container für Ihre apps; alle Ressourcen für Ihre app wechseln Sie zu der gleichen Ressourcengruppe.

Als Nächstes fügen wir Trigger und Aktionen hinzu.


## <a name="add-http-trigger"></a>HTTP-Trigger hinzufügen
1. Wählen Sie **Logik App-Vorlagen** **von Grund auf Erstellen**.
1. Wählen Sie aus dem Katalog zum Erstellen einer neuen Zuhörer **HTTP Zuhörer** aus. Rufen Sie ihn **HTTP1**.
2. Festlegen der **Antwort automatisch senden?** auf "falsch" festlegen. Konfigurieren Sie die Aktion auslösen von _HTTP-Methode_ zum _Bereitstellen_ und _Relativen URL_ -Einstellung zu _/OneWayPipeline_:  
    ![HTTP-Trigger][2]
3. Wählen Sie den grünen Häkchen zum Abschließen des Triggers aus.

## <a name="add-validate-action"></a>Hinzufügen von Aktion überprüfen

Jetzt, geben Sie uns die Aktionen, die ausgeführt werden, wenn der Trigger ausgelöst wird – d. h., jederzeit ein Anruf empfangen wird, klicken Sie auf den HTTP-Endpunkt.

1. Hinzufügen von **BizTalk XML-Bestätigung** aus dem Katalog aus, und nennen Sie es _(Validate1)_ zum Erstellen einer Instanz.
2. Konfigurieren Sie ein XSD-Schema, um die eingehenden XML-Nachrichten zu überprüfen. Wählen Sie die Aktion _Überprüfen_ , und wählen Sie _Triggers('httplistener').outputs. Inhalt_ als Wert für den Parameter _InputXml_ .

Nun kann die Gültigkeit Aktion die erste Aktion nach dem Zuhörer HTTP: 

![BizTalk XML-Bestätigung][3]

Lassen Sie uns ebenso den Rest der Aktionen hinzufügen. 

## <a name="add-transform-action"></a>Hinzufügen von Transformation Aktion
Lassen Sie uns konfigurieren können, um die eingehenden Daten standardisiert werden soll.

1. Fügen Sie **Transformieren BizTalk-Dienst** aus dem Katalog hinzu.
2. Zum Konfigurieren einer Transformation, um die eingehenden XML-Nachrichten transformieren, legen Sie die Aktion **Transformieren** der Aktion für die Durchführung, wenn diese API aufgerufen wird. Wählen Sie ```triggers(‘httplistener’).outputs.Content``` als Wert für _InputXml_. *Karte* ist ein optionaler Parameter, da die eingehenden Daten werden mit einer Transformation für alle konfigurierten verglichen, und nur diejenigen, die im Schema entsprechen angewendet werden.
3. Schließlich führt die Transformation nur, wenn Gültigkeitsüberprüfung erfolgreich ist. Um diese Bedingung zu konfigurieren, wählen Sie oben rechts auf das Zahnradsymbol, und wählen Sie _Hinzufügen eine Bedingung erfüllt sein_. Legen Sie die Bedingung auf ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![Sie können BizTalk][4]


## <a name="add-service-bus-connector"></a>Hinzufügen von Service Bus Verbinder
Als Nächstes fügen Sie das Ziel – ein Service Bus Warteschlange – zum Schreiben von Daten.

1. Hinzufügen eines **Service Bus Verbinder** aus dem Katalog an. Legen Sie die **Namen** auf _Servicebus1_, legen Sie die **Verbindungszeichenfolge** in der Verbindungszeichenfolge Ihrer Service Bus Instanz, festlegen Sie **Namen der Entität** an _Warteschlange_und überspringen Sie **Abonnementname**.
2. Wählen Sie die **Nachricht senden** Aktion aus, und legen Sie das Feld " **Inhalt** " für die Aktion auf _Actions('transformservice').outputs. OutputXml_.
3. Legen Sie das Feld " **Inhaltstyp** " **Application/XML fest:  

![Dienstbus][5]


## <a name="send-http-response"></a>HTTP-Antwort senden
Nach Abschluss der Verkaufspipeline Verarbeitung zurücksenden einer HTTP-Antwort für Erfolg und Fehler mit den folgenden Schritten:

1. Fügen Sie einer **HTTP Zuhörer** aus dem Katalog hinzu aus, und wählen Sie die Aktion **HTTP-Antwort senden** .
2. Legen Sie die **Antwort-ID** zum Senden der *Nachricht*.
2. Legen Sie **Inhalte Antwort** *Verkaufspipeline Verarbeitung abgeschlossen*fest.
3. **Antwort Status Code** *200* an, dass HTTP 200 OK.
4. Wählen Sie im Dropdownmenü auf der rechten oberen Ecke aus, und wählen Sie **Hinzufügen eine Bedingung erfüllt sein**.  Legen Sie die Bedingung auf den folgenden Ausdruck ein:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Wiederholen Sie diese Schritte aus, um bei einem Fehler auch eine HTTP-Antwort zu senden. Ändern Sie **Bedingung** auf den folgenden Ausdruck ein:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Wählen Sie **OK** und dann **Erstellen**aus.



## <a name="completion"></a>Abschluss
Jedes Mal, wenn jemand eine Nachricht an den HTTP-Endpunkt sendet, löst die app und führt die Aktionen aus, die Sie soeben erstellt haben. Zum Verwalten von apps, die Logik solche erstellen, wählen Sie im Portal Azure **Durchsuchen** und wählen Sie **Logik Apps**. Wählen Sie die app, um weitere Informationen anzuzeigen.

Einige sehr nützliche Hilfethemen:

[Verwalten Sie und überwachen Sie Ihrer API Apps und Verbinder](app-service-logic-monitor-your-connectors.md)  <br/>
[Überwachen Sie Ihrer Apps Logik](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG

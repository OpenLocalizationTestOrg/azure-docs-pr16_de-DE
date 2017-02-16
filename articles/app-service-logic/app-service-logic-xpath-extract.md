<properties
   pageTitle="Verwenden die BizTalk XPath extrahieren in Logik apps in Azure-App-Verwaltungsdienst | Microsoft Azure"
   description="BizTalk XPath extrahieren"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajram"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="biztalk-xpath-extractor"></a>BizTalk XPath extrahieren

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


Verbinder BizTalk XPath extrahieren können Ihre app nachschlagen und Extrahieren von Daten aus XML-Inhalt auf Grundlage einer angegebenen XPath.

## <a name="using-the-biztalk-xpath-extractor"></a>Verwenden der BizTalk Xpath extrahieren
1. Zum Extrahieren der BizTalk-Xpath verwenden zu können, müssen Sie zuerst eine Instanz der app BizTalk Xpath extrahieren API zu erstellen. Dies kann entweder Inline beim Erstellen einer app Logik oder durch Auswahl der app-API BizTalk Xpath-Extrahieren aus dem Azure Marketplace erfolgen.

    >[AZURE.NOTE] Es gibt keine Konfiguration Einstellungen BizTalk Xpath extrahieren zugeordnet.
2. [Erstellen einer neuen Logik app]. Öffnen Sie "Trigger und Aktionen" innerhalb der App Logik, um Apps Logik zum Konfigurieren Ihrer Fluss Designer zu öffnen.
3. Designer aufgelistet im rechte Bereich der API Apps erstellen Ihrer Fluss zur Verfügung. Suchen Sie nach "BizTalk XPath extrahieren". Auswählen der Fluss der Xpath-extrahieren hinzugefügt und Vorschriften, die eine Instanz von ihm.
4. Nach der Bereitstellung wird der Designer von BizTalk XPath extrahieren API App zugeordnete Aktion dargestellt:  
    ![Wählen Sie BizTalk XPath extrahieren Aktion][1]

5. Wählen Sie "Extrahieren XPath-Ausdruck" aus. "Extrahieren verwenden XPath" ergibt Eingabewerte Xpath-Ausdruck auf einem angegebenen XML-Eingabe:  
    ![BizTalk XPath extrahieren Eingabe][2]

    Parameter|Typ|Beschreibung des Parameters
---|---|---
XPath|Zeichenfolge|Abfragepfad innerhalb der XML.
XML-Eingabe|Zeichenfolge|XML-Inhalt als Eingabe.

Die Aktion gibt das Ergebnis als Zeichenfolge - Ergebnis. Ergebnis enthält den Wert zu innerhalb der XML-Abfrage.

<!-- References -->
[1]: ./media/app-service-logic-xpath-extract/ChooseAction.PNG
[2]: ./media/app-service-logic-xpath-extract/ConfigureInput.PNG

<!-- Links -->
[Erstellen einer neuen Logik-App]: app-service-logic-create-a-logic-app.md

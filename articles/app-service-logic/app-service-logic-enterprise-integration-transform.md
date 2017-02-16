<properties 
    pageTitle="Übersicht über Enterprise Integration Pack | Microsoft Azure-App-Verwaltungsdienst | Microsoft Azure" 
    description="Verwenden der Features von Enterprise Integration Pack Business Process und Integration Szenarien mithilfe von Microsoft Azure App Service aktivieren" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise-Integration mit einer XML-Transformation

## <a name="overview"></a>(Übersicht)
Der Enterprise-Integration Transformation Verbinder werden Daten von einem Format in ein anderes Format konvertiert. Beispielsweise können Sie eine eingehende Nachricht verfügen, die das aktuelle Datum im Format YearMonthDay enthält. Eine Transformation können Sie um das Datum im Format MonthDayYear werden neu zu formatieren.

## <a name="what-does-a-transform-do"></a>Welche Funktion hat eine Transformation?
Eine Transformation, die also auch bekannt als ein Schema, besteht aus einer Quelle XML-Schema (die Eingabe) und ein Ziel XML-Schema (die Ausgabe). Verschiedene integrierte Funktionen können Sie besser bearbeiten oder steuern, die Daten, einschließlich Zeichenfolgen, bedingte Zuordnungen, arithmetische Ausdrücke, Datum Uhrzeit Formatierungsprogramme und sogar Endloswiedergabe Konstrukte.

## <a name="how-to-create-a-transform"></a>So erstellen Sie eine Transformation?
Sie können eine Transformation-Karte mit dem Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas)erstellen. Sie abschließend erstellen und Testen der Transformation, laden Sie die Transformation in Ihr Konto Integration hoch. 

## <a name="how-to-use-a-transform"></a>So verwenden Sie eine Transformation
Nachdem Sie die Transformation in Ihr Konto Integration hochgeladen haben, können Sie es zum Erstellen einer app Logik verwenden. Die app Logik wird dann Ihre Transformationen ausgeführt, wenn die app Logik wird ausgelöst (und Eingabe der Inhalte, die umgewandelt werden muss vorhanden ist).

**Hier sind die Schritte zum Verwenden von einer Transformation**:

### <a name="prerequisites"></a>Erforderliche Komponenten 
In der Vorschau müssen Sie:  

-  [Erstellen eines Containers Azure-Funktionen] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Erstellen eines Containers Azure-Funktionen")  
-  [Hinzufügen einer Funktion zum Container Azure-Funktionen] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Diese Vorlage erstellt eine Grundlage Webhook c# Azure Funktion mit Transformation Funktionen zur Verwendung in Logik apps Integration Szenarien")    
-  Erstellen Sie ein Konto Integration, und fügen Sie eine Karte hinzu  

>[AZURE.TIP] Notieren Sie die den Namen des Containers Azure-Funktionen als auch die Azure-Funktion, Sie benötigen sie im nächsten Schritt.  

Jetzt, da Sie die erforderlichen Komponenten durchgeführt haben, ist es Zeit für Ihre app Logik zu erstellen:  

1. Erstellen eines app Logik und [Verknüpfen Sie es mit Ihrem Konto Integration](./app-service-logic-enterprise-integration-accounts.md "erhalten grundlegende Informationen zu einer Firma Integration einer app Logik verknüpfen") , die die Karte enthält.
2. Hinzufügen eines Triggers **Anforderung – Wenn ein HTTP-Anforderung empfangen wird** zu Ihrer Anwendung Logik  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Die Aktion **Transformieren XML-** vom ersten Auswählen eines **Hinzufügen einer Aktion** hinzufügen   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. In das Suchfeld eingeben der Word- *Transformieren* , um alle Aktionen auf den Filtern, die Sie verwenden möchten.  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Wählen Sie die Aktion **XML transformieren**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Wählen Sie die **Funktion CONTAINER** , die die Funktion enthält, die Sie verwenden möchten. Dies ist der Name des Containers Azure-Funktionen, die Sie zuvor in den folgenden Schritten erstellt haben.
7. Wählen Sie die **Funktion** , die Sie verwenden möchten. Dies ist der Name der Azure-Funktion, die Sie zuvor erstellt haben.
8. Fügen Sie der XML- **Inhalt** , mit denen Sie transformiert werden. Beachten Sie, dass Sie alle XML-Daten verwenden können, die Sie in der HTTP-Anforderung als die- **Inhalten**zu erhalten. Wählen Sie in diesem Beispiel den Hauptteil der HTTP-Anforderung, die die Logik app ausgelöst wurde.
9. Wählen Sie den Namen der **Karte** , die Sie zum Durchführen der Transformation verwenden möchten. Die Karte muss sich bereits in Ihrem Konto Integration. In einer früheren Schritt konnten Sie bereits Ihren Logik app Zugriff zu Ihrer Integration-Konto, die Karte enthält.
10. Speichern Sie Ihre Arbeit  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Jetzt sind Sie Fertig zum Einrichten der Karte. In einer realen Anwendung möchten Sie möglicherweise umgewandelten Daten in einer LOB-Anwendung wie Vertrieb zu speichern. Sie können ganz einfach als eine Aktion, die Ausgabe der Transformation an Vertrieb zu senden. 

Sie können jetzt Ihre Transformation testen, indem er eine Anforderung an den HTTP-Endpunkt.  

## <a name="features-and-use-cases"></a>Features und Verwenden von Fällen

- Die Transformation erstellt in einer Karte kann einfach sein, beispielsweise kopieren einen Namen und eine Adresse aus einem Dokument in ein anderes sein. Oder Sie können komplexere Skripttransformationen die Out-of-Box-Karte Vorgänge erstellen.  
- Mehrere Karte Vorgänge oder -Funktionen sind jederzeit verfügbar sind, einschließlich Zeichenfolgen, Datum Uhrzeitfunktionen und So weiter.  
- Sie können eine Datenkopie der direkten zwischen den Schemas ausführen. Dies ist in der Zuordnung im SDK enthalten ist so einfach wie das Zeichnen einer Linie, die die Elemente in der Quellschema mit ihren Gegenstücken im Zielschema eine Verbindung herstellt.  
- Beim Erstellen einer Karte anzeigen grafisch der Karte, die anzeigen, alle Beziehungen und Links, die Sie erstellen.
- Verwenden Sie das Feature Zuordnung testen, um eine XML-beispielmitteilung hinzuzufügen. Sie können mit einem einfachen testen die Karte, die Sie erstellt haben und finden Sie unter generierte Ausgabe.  
- Hochladen von vorhandenen Karten  
- Das XML-Format unterstützt.


## <a name="learn-more"></a>Weitere Informationen
- [Erfahren Sie mehr über das Enterprise-Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Pack für Enterprise-Integration")  
- [Weitere Informationen über Karten] (./app-service-logic-enterprise-integration-maps.md "Erfahren Sie mehr über Enterprise-Integration zugeordnet ist.")  
 
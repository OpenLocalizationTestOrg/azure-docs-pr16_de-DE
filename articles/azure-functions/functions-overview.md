<properties
   pageTitle="Azure Funktionen Übersicht | Microsoft Azure"
   description="Verstehen Sie, wie Azure-Funktionen verwenden, um asynchrone Auslastung in Minuten optimieren."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Übersicht über Azure-Funktionen

Azure Funktionen ist eine Lösung für die Ausführung von Code oder "Funktionen" für kleinere einfach in der Cloud. Sie können nur den Code schreiben, die, den Sie für das Problem zur hand, ohne Sorgen über eine gesamte Anwendung oder die Infrastruktur, um Sie auszuführen müssen. Umso Entwicklung, die noch produktiver, und die Entwicklungssprache Wahl, z. B. c#, F Nr., Node.js, Python oder PHP können. Bezahlen Sie nur für die Uhrzeit, zu der Code ausgeführt wird und Azure-Trust, je nach Bedarf zu skalieren.

Dieses Thema enthält eine Übersicht über Funktionen Azure. Wenn Sie gleich und erste Schritte mit Azure Funktionen möchten, beginnen Sie mit [Erstellen Ihrer ersten Azure-Funktion](functions-create-first-azure-function.md). Wenn Sie weitere technische Informationen zu Funktionen suchen, finden Sie unter den [Entwicklerreferenz](functions-reference.md).

## <a name="features"></a>Features

Hier sind einige der wichtigen Features von Azure-Funktionen:
    
* **Wahl der Sprache** - schreiben Funktionen von c#, F Nr., Node.js, Python, PHP, Stapel, Bash, Java oder eine beliebige ausführbare Datei.
* **Bezahlung pro Einsatz Preisgestaltung Modell** – Bezahlung nur für die Zeit für Ihren Code ausführt. Planen von dynamischen App Service Option in die [Preise Abschnitt](#pricing) unter angezeigt.  
* **Zeigen Sie Ihre eigenen Abhängigkeiten** - Funktionen unterstützt NuGet und NPM, daher können Sie Ihre bevorzugten Bibliotheken verwenden.  
* **Integrierte Sicherheit** - Funktionen mit OAuth Anbieter wie Azure Active Directory, Facebook, Google, Twitter und Microsoft Account schützen HTTP-ausgelöst wurde.  
* **Vereinfachte Integration** – einfach Azure Services und Software-as-a-Service (SaaS) Angebote nutzen. Finden Sie im [Abschnitt Integration](#integrations) unter einige Beispiele für ein.  
* **Flexible Entwicklung** - Fehlercode Ihrer Funktionen rechts im Portal oder fortlaufende Integration einrichten und Bereitstellen von Code durch GitHub, Visual Studio Team Services und andere [Entwicklungstools unterstützt](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open-Source** - Funktionen der Laufzeit ist Source-öffnen und [auf GitHub zur Verfügung](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Was kann ich mit den Funktionen?

Azure Funktionen ist eine großartige Lösung für die Verarbeitung von Daten, Integration von Betriebssystemen, arbeiten mit Internet-von-Maßnahmen (IoT), und erstellen einfache APIs und Microservices. Erwägen Sie die Funktionen für Aufgaben wie das Bild oder die Reihenfolge Verarbeitung, Wartung, langer Aufgaben, die Sie in einem Hintergrundthread ausführen möchten, oder für alle Vorgänge, die in einem Zeitplan ausgeführt werden sollen. 

Funktionen bietet Vorlagen, die Ihnen beim Einstieg mit Schlüsselszenarien, einschließlich der folgenden:

* **BlobTrigger** - Prozess Azure-Speicher Blobs wenn Container hinzugefügt werden. Sie können dies für Bildgröße verwenden.
* **EventHubTrigger** - Antworten auf Ereignisse an ein Ereignis Azure-Hub übermittelt wurden. In Anwendungsinstrumentation, Bearbeitung des Benutzers Erfahrung oder einen Workflow und Internet der Dinge (IoT) Szenarien besonders hilfreich.
* **Generische Webhook** - Prozess Webhook HTTP fordert von jedem Dienst, der Webhooks unterstützt.
* **GitHub Webhook** - Antworten auf Ereignisse, die in Ihrem GitHub Repositorys auftreten. Ein Beispiel finden Sie unter [Erstellen eines Webhook oder API-Funktion](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - Trigger die Ausführung des Codes mithilfe einer HTTP-Anforderung.
* **QueueTrigger** - Antworten auf Nachrichten beim Eintreffen einer Warteschlange Azure-Speicher. Ein Beispiel finden Sie unter [Erstellen einer der in einer Azure Service bindet Azure-Funktion](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - Verbinden von Code mit anderen Diensten Azure oder lokal Services durch Nachrichten überwachen. 
* **ServiceBusTopicTrigger** - Verbinden von Code mit anderen Diensten Azure oder lokal Services durch Themen abonnieren. 
* **TimerTrigger** - Aufräumen oder anderen Vorgängen Stapel nach einem vordefinierten Zeitplan ausführen. Ein Beispiel finden Sie unter [Erstellen eines Ereignisses processing (Funktion)](functions-create-an-event-processing-function.md).

Azure Funktionen unterstützt *Trigger*, welche Möglichkeiten zum Starten der Ausführung des Codes sind, und *Bindungen*, die Methoden zum Schreiben von Code für Eingabe- und Daten vereinfachen sind. Eine detaillierte Beschreibung der Trigger und Bindungen, die Azure Funktionen bereitstellt, finden Sie unter [Trigger Azure-Funktionen und Bindungen Entwicklerreferenz](functions-triggers-bindings.md).


## <a name="a-nameintegrationsaintegrations"></a><a name="integrations"></a>Integration

Azure Funktionen arbeitet mit einer Vielzahl von Azure und 3rd Party-Dienste. Sie können diese Auslösen der Funktion und starten Sie die Ausführung oder Sie dienen als Eingabe und Ausgabe für den Code. Die folgenden Service-Integration werden von Azure-Funktionen unterstützt. 

* Azure DocumentDB
* Azure Ereignis Hubs 
* Azure Mobile-Apps (Tabellen)
* Azure Benachrichtigung Hubs
* Azure Service Bus (Warteschlangen und Themen)
* Azure-Speicher (Blob, Warteschlangen und Tabellen) 
* GitHub (Webhooks)
* Lokal (mit Dienstbus)

## <a name="a-namepricingahow-much-does-functions-cost"></a><a name="pricing"></a>Kostet Funktionen Kosten?

Azure Funktionen weist zwei Arten von Preise Pläne, und wählen Sie das Element, das Ihren Bedürfnissen entspricht: 

* **Dynamische Hostinganbieter Plan** - bei der Ausführung der Funktion, Azure enthält alle notwendigen berechnete Ressourcen. Müssen Sie nicht über ressourcenverwaltung Gedanken machen, und Sie Zahlen nur für die Uhrzeit, zu der der Code ausgeführt wird. Vollständige Informationen zur Preisgestaltung sind auf der [Seite Funktionen Preise](/pricing/details/functions)verfügbar. 

* **App-Serviceplan** – ausführen die Funktionen wie Ihre Web, Mobile und API apps. Wenn Sie bereits für anderen Anwendungsfenstern App-Dienst verwenden, können Sie Ihre Funktionen auf den gleichen Plan Mehrkosten ausführen. Alle Details finden Sie auf der [Seite App Preisen](/pricing/details/app-service/).

Weitere Informationen zum Skalieren Ihrer Funktionen finden Sie unter [Skalieren Azure-Funktionen](functions-scale.md).

##<a name="next-steps"></a>Nächste Schritte

+ [Erstellen Sie Ihrer ersten Azure-Funktion](functions-create-first-azure-function.md)  
Gleich, und erstellen Sie Ihre erste Funktion Schnellstart Azure-Funktionen verwenden. 
+ [Azure Funktionen Entwicklerreferenz](functions-reference.md)  
Stellt weitere technische Informationen über die Funktionen Azure Runtime und einen Verweis für Codieren von Funktionen und Trigger und Bindungen definieren.
+ [Testen der Azure-Funktionen](functions-test-a-function.md)  
Beschreibt verschiedene Tools und Verfahren zum Testen der Funktionen.
+ [Zum Skalieren Azure-Funktionen](functions-scale.md)  
Werden Servicepläne erhältlich Azure-Funktionen, einschließlich der dynamischen Serviceplan und wie Sie den richtigen Plan auswählen. 
+ [Weitere Informationen zu Azure-App-Verwaltungsdienst](../app-service/app-service-value-prop-what-is.md)  
Azure-Funktionen verwendet, die App-Verwaltungsdienst Azure-Plattform für Kernfunktionalität wie Bereitstellungen, Umgebungsvariablen und Diagnose wird. 
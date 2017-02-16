<properties 
    pageTitle="Allgemeine Operationen in der Computer Learning Empfehlungen-API | Microsoft Azure" 
    description="Azure ML Empfehlungen Beispiel-Anwendung" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Empfehlungen-API Stichprobe Anwendung Exemplarische Vorgehensweise

>[AZURE.NOTE]Starten Sie mithilfe von Empfehlungen-API kognitive Service statt dieser Version. Kognitive Empfehlungen Dienst ersetzt werden soll diesen Dienst, und alle neuen Features werden es entwickelt werden. Es hat neue Funktionen wie Batchverarbeitung Support, ein besseres API-Explorer, eine übersichtlichere API einbinden, konsistenter beim Registrieren/Abrechnung Erfahrung usw. aus.
> Weitere Informationen zum [Migrieren zu den neuen kognitive Dienst](http://aka.ms/recomigrate)

##<a name="purpose"></a>Zweck

Dieses Dokument zeigt die Verwendung der Azure maschinellen Learning Empfehlungen API über eine [Beispiel-Anwendung](https://code.msdn.microsoft.com/Recommendations-144df403).

Diese Anwendung kann nicht vollständige Funktionen einbeziehen verwendet, noch es alle APIs. Es veranschaulicht einige allgemeine Vorgänge ausführen, wenn Sie zuerst den empfehlungsdienst Computer Schulung zum Ausprobieren verwenden möchten. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Einführung in empfehlungsdienst Computer-Schulung

Empfehlungen über den Computer Learning empfehlungsdienst aktiviert sind, wenn Sie ein Empfehlungen-Modell, basierend auf den folgenden Daten zu erstellen:

* Ein Repository für die Elemente, wird empfohlen, auch bekannt als Katalog werden soll
* Daten, die die Verwendung der Elemente pro Benutzer oder eine Sitzung (diese kann über einen Zeitraum über Daten Acquisition, nicht als Teil der Stichprobe app erworben) darstellen.

Nachdem ein Empfehlungen Modell erstellt wurde, können Sie sie Elemente, die ein Benutzer möglicherweise interessiert, nach einem Satz von Elementen (oder ein einzelnes Element) des Benutzers Vorhersagen markiert.

Zum vorherige Szenario aktivieren möchten, gehen Sie in den empfehlungsdienst Computer Schulung:

* Erstellen ein Modells: Dies ist ein logischer Container, die Daten (Katalog und die Verwendung) und der Vorhersage Modelle enthält. Über eine eindeutige ID, die bei der Erstellung zugeordnet wurde, wird jedes Modellcontainer identifiziert. Diese ID heißt die Modell-ID, und es wird durch die meisten APIs verwendet. 
* Upload Katalog: beim Erstellen ein Containers Modell, Sie können zuordnen zu einen Katalog.

**Hinweis**: mit einem Katalog hoch- und Erstellen eines Modells sind in der Regel einmal für das Modell Lebenszyklus durchgeführt.

* Verwendung hochladen: Dadurch wird das Modellcontainer Verwendungsdaten hinzugefügt.
* Erstellen ein Modells empfohlen: Nachdem Sie genügend Daten haben, können Sie das Modell Empfehlungen erstellen. Dieser Vorgang verwendet die oberen maschinellen Learning Algorithmen zum Erstellen eines Empfehlungen-Modells an. Jede erstellen ist eine eindeutige ID zugeordnet Sie müssen diese ID aufzeichnen, da es für die Funktionsweise von einige APIs erforderlich ist.
* Überwachen der Erstellungsvorgang: die Erstellung eines Empfehlungen-Modells ist eine asynchrone Operation, und es kann aus mehrere Minuten dauern, mehrere Stunden, je nach der Datenmenge (Katalog und die Verwendung) und die Parameter erstellen. Daher müssen Sie den Build zu überwachen. Nur, wenn seine zugeordneten Build erfolgreich abgeschlossen wurde, wird ein Empfehlungen-Modell erstellt.
* (Optional) Wählen Sie die Erstellung einer aktiven Empfehlungen-Modells: Dieser Schritt ist nur erforderlich, wenn Sie mehr als ein Empfehlungen-Modell in Ihr Modellcontainer erstellt haben. Jede Aufforderung zum Empfehlungen erhalten, ohne das Modell aktiven Empfehlungen angibt wird durch das System, um die aktive Standard-Generator automatisch umgeleitet. 

**Hinweis**: ein Modell aktiven Empfehlungen ist Herstellung bereit und für die Herstellung Arbeitsbelastung basiert. Dies unterscheidet sich von ein nicht aktiven Empfehlungen-Modell, das bleibt in einer Test-ähnliche-Umgebung (auch als Staging bezeichnet).

* Lassen Sie sich: Nachdem Sie ein Modell Empfehlungen haben, können Sie Empfehlungen für ein einzelnes Element oder eine Liste von Elementen, die Sie auswählen auslösen. 

Sie werden in der Regel erhalten Empfehlungen für einen bestimmten Zeitraum aufrufen. In diesem Zeitraum können Sie den Computer Learning Empfehlungen-System, Verwendungsdaten umleiten, die diese Daten zur angegebenen Modellcontainer hinzugefügt. Wenn Sie über genügend Verwendungsdaten verfügen, können Sie ein neues Empfehlungen-Modell erstellen, der die Verwendungsdaten der weiteren. 

##<a name="prerequisites"></a>Erforderliche Komponenten

* Visual Studio 2013
* Zugriff auf das Internet 
* Die Empfehlungen-API (https://datamarket.azure.com/dataset/amla/recommendations)-Abonnement.

##<a name="azure-machine-learning-sample-app-solution"></a>Azure maschinellen Learning Stichprobe app-Lösung

Diese Lösung enthält Quellcode, Beispiel für die Verwendung, Katalogdatei, und Richtlinien, die die Pakete herunterzuladen, die für die Kompilierung erforderlich sind.

##<a name="the-apis-used"></a>Die APIs verwendet

Die Anwendung verwendet maschinellen Learning Empfehlungen Funktionalität über eine Teilmenge der verfügbaren APIs. Die folgenden APIs werden in der Anwendung veranschaulicht:

* Erstellen von Modell: erstellen einen logischen Container zum Speichern von Daten und Empfehlungen Modelle. Ein Modell wird durch einen Namen gekennzeichnet, und Sie können nicht mehr als ein Modell mit demselben Namen erstellen.
* Katalogdatei hochladen: Verwenden Sie zum Hochladen von Daten im Katalog.
* Verwendung Datei hochladen: Verwenden Sie zum Hochladen von Verwendungsdaten.
* Auslösen erstellen: Verwenden Sie zum Erstellen eines Modells empfohlen.
* Überwachen der Ausführung erstellen: verwenden, um den Status der Erstellung eines Empfehlungen-Modells zu überwachen.
* Wählen Sie ein erstellte Modell für Empfehlungen: verwenden, um anzugeben, welche Modell Empfehlungen für eine bestimmte Modellcontainer standardmäßig verwendet werden soll. Dieser Schritt ist erforderlich, nur, wenn Sie mehr als ein Empfehlungen Modell haben und Sie einen nicht aktiven Build als die aktive Empfehlungen Modell aktivieren möchten.
* Erste Empfehlungen: verwenden, um empfohlene Elemente nach ein bestimmtes einzelnes Element oder eine Gruppe von Elementen abzurufen. 

Eine vollständige Beschreibung der APIs finden Sie in der Dokumentation Microsoft Azure Marketplace. 

**Hinweis**: ein Modell mehrere Builds (nicht gleichzeitig) über einen Zeitraum haben kann. Jeder Build wird mit derselben oder einer aktualisierten Katalog und zusätzliche Verwendungsdaten erstellt.

## <a name="common-pitfalls"></a>Häufige Probleme

* Sie müssen Ihren Benutzernamen und Ihren Microsoft Azure Marketplace primären kontoschlüssel zum Ausführen der Stichprobe app bereitstellen.
* Beispiel-app fortlaufend ausgeführt, schlägt fehl. Der Anwendung Fluss umfasst erstellen, hochladen, erstellen den Monitor und Empfehlungen aus einem vordefinierten Modell erste; Es wird daher aufeinander folgenden Ausführung fehl, wenn der Name des Modells zwischen den Aufrufen nicht zu ändern.
* Empfehlungen möglicherweise ohne Daten zurück. Beispiel-app verwendet eine geringe Katalog und die Verwendung der Datei. Einige Elemente aus dem Katalog müssen daher keine empfohlene Elemente.

## <a name="disclaimer"></a>Verzichtserklärung
Beispiel-app ist nicht in einer Umgebung für die Herstellung ausgeführt werden soll. Die Daten im Katalog sind sehr kleine werden, und es keines Modells aussagekräftigen empfohlen. Die Daten werden als ein Demo bereitgestellt. 
 

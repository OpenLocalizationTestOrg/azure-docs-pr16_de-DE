<properties
    pageTitle="Erstellen eines Modells mit der UI Recommnendations | Microsoft Azure"
    description="Azure maschinellen Learning Empfehlungen - Erstellen eines Modells mit der Benutzeroberfläche Empfehlungen"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Erstellen eines Modells mit der Benutzeroberfläche Empfehlungen

Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel besteht darin, Sie die erforderlichen Schritte zum Schulen eines Modells mithilfe der [Benutzeroberfläche von Empfehlungen](https://recommendations-portal.azurewebsites.net/)durchzuführen.
Die Übung durchgehen, können Sie grundlegende Informationen zu den Prozess zum Erstellen eines Modells aus, bevor Sie programmgesteuert ausführen. Es werden vertraut auch Sie mit der Benutzeroberfläche, welche praktischen ist, wie Sie den Dienst verwenden.

In dieser Übung dauert etwa 30 Minuten.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Schritt 1: Melden Sie sich in der Benutzeroberfläche Empfehlungen ##

1. Wenn Sie dies noch nicht getan haben, müssen Sie [Anmeldung](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) für ein neues [Empfehlungen-API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) -Abonnement. Sie können für den Dienst auf **Azure** am [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) und melden Sie sich mit Ihrem Azure-Konto anmelden. Detaillierte Informationen zum den Anmeldevorgang ab stehen in *Aufgabe 1* der [Leitfaden für erste Schritte](cognitive-services-recommendations-quick-start.md) zur Verfügung. 

1. Nachdem Sie einen **Schlüssel** für Ihr Abonnement Empfehlungen-API erhalten haben, wechseln Sie zu [Empfehlungen-Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/). 

1. Melden Sie sich mit dem Portal durch Eingabe der Taste in das Feld **Kontoschlüssel** ein, und klicken Sie dann auf **die Anmeldeschaltfläche** .

    ![Empfehlungen-Benutzeroberfläche: Melden Sie sich im Dialogfeld.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Schritt 2 – Wir sammeln einiger Schulungsdaten ##

Bevor Sie einen Build erstellen können, benötigt die-Engine zweierlei Informationen: eine Katalogdatei und eine Reihe von Dateien Verwendung. 

Die Katalogdatei enthält Informationen zu den Elementen, dass Sie an Ihren Kunden anbieten. Eine Verwendung-Datei enthält Informationen wie diese Elemente verwendet werden, oder die Transaktionen aus Ihrem Unternehmen.

Normalerweise würden Sie Ihre Datenbank Store für diese Teile der Informationen Abfragen. Heute, haben wir einige Beispieldaten für Sie bereitgestellt, damit Sie lernen, wie die Empfehlungen-API verwenden können.

Sie können die Daten von [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData)herunterladen. Kopieren Sie und Entpacken Sie die **Books.Zip** -Datei in einem Ordner auf Ihrem lokalen Computer. Beispielsweise **c:\data**.

Ausführliche Informationen zu das Schema der Dateien Katalog und die Verwendung finden Sie im Artikel [Sammeln von Daten in Ihr Modell Schulen](cognitive-services-recommendations-collecting-data.md) .
 
In dieser Übung werden wir eine kleine Datei verwenden, sodass Sie nicht sehr Schulung zu lange warten müssen. Wenn Sie eine weitere realistische Datei ausprobieren möchten, haben wir auch **MsStoreData.zip** platziert, die Stichprobe Transaktionen aus dem Microsoft Store an [derselben Stelle](http://aka.ms/RecoSampleData)enthält.

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Schritt 3: Erstellen eines Projekts und Hochladen Katalog und die Verwendung von Daten ##

Nach der Anmeldung an die [Empfehlungen-Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/)wird auf der Seite Projekte. Wenn Sie alle Projekte zuvor erstellt haben, sollten Sie diese hier angezeigt.
Ein Projekts (auch bekannt als *ein Modell* im Bezug API) ist ein Container für Ihre Daten Katalog und die Verwendung. Sie können mehrere *erstellt* innerhalb des Projekts erstellen. Wir führt Sie durch den Vorgang in den nächsten Schritten fort.

1. Um ein neues Projekt zu erstellen, geben Sie den Namen in das Textfeld (etwa "MyFirstModel" arbeiten möchten), und klicken Sie auf **Projekt hinzufügen**.
 
    ![Empfehlungen-Benutzeroberfläche: Auf der Seite Projekte.][reco_projects]

1. Nachdem das Projekt erstellt wird, klicken Sie auf die Schaltfläche **Suchen Sie nach der Datei** , in dem Abschnitt **Hinzufügen einer Datei Katalog** . Laden Sie den Katalog aus, den Sie in Schritt 2 auf Ihrem System hoch. Wenn Sie es mit *c:\data*gespeichert haben, müssen Sie zu diesem Ordner zu navigieren.

    ![Empfehlungen-Benutzeroberfläche: Hinzufügen von Daten zu einem Projekt.][reco_firstmodel]

1. Nachdem der Katalog hochgeladen wurde, klicken Sie auf die Schaltfläche **Suchen Sie nach der Datei** , in dem Abschnitt **Verwendung Dateien hinzufügen** . Fügen Sie die Datei usage_large.txt hinzu.

> **Was geschieht, wenn ich eine große Datei von Verwendungsdaten habe?**
>
> Nur Verwendung Dateien kleiner als 200 MB hochgeladen werden kann. Dies bedeutet dass, kann das System einrichten auf enthalten 2 GB Einheiten im Wert von Transaktionsdaten, damit Sie bei Bedarf mehr als eine Datei hochladen können.
> Dieser Datenmenge erstellen Sie ein gute Modell möglicherweise nicht erforderlich, es ist nicht nur die Größe der Daten, die wichtig ist, aber die Qualität der Daten. Es ist üblich Verwendungsdaten anzeigen, wobei die meisten der Transaktionen sind, klicken Sie einfach auf ein paar Beliebteste Elemente und ist es für andere Elemente "etwas signal".

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Schritt 4 – einige Schulung los geht's! ##

Jetzt, da Sie sowohl den Katalog und die Nutzungsdaten hochgeladen haben, können wir die-Engine Schulen, damit es Muster aus unserem Daten finden kann.

1.  Klicken Sie auf die Schaltfläche **Neue erstellen** .

1.  Wählen Sie **Empfehlungen** als erstellen aus. Beachten Sie, dass wir Unterstützung Rangfolgen Erstellen einer FBT (häufig gekauft zusammen) erstellen sowie Datentypen.

    ![Empfehlungen-Benutzeroberfläche: Erstellen von Dialogfeld.][reco_build_dialog.png]


    Erstellen einer FBT können Sie Muster für Produkte zu identifizieren, die in der Regel gekauft/verbraucht in derselben Transaktion sind.
    Eine Rangfolge erstellen wird verwendet, um relevante Features zu identifizieren. 
    Wir Gehe nicht sehr tief auf FBT Rangfolge diesem erstellt, aber wenn Sie sich interessieren sollten sich die [Typen und Modell Qualität Dokumentationsseite erstellen](cognitive-services-recommendations-buildtypes.md).

1. Klicken Sie auf die **Generator** -Schaltfläche. Der erstellen Vorgang dauert etwa fünf Minuten aus, wenn Sie die Daten Bücher verwenden. Es dauert länger auf größere Datensets.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Schritt 5 – wir finden Sie heraus, was gelernt, dass der Computer! ##

Nachdem Sie Ihre eigene abgeschlossen ist, sehen Sie die Erstellung ein neues in der Liste Builds. Empfehlungen im Zusammenhang mit Element und Benutzer kann diesen Build abgefragt werden.

1. Nachdem Sie Ihre eigene abgeschlossen ist, klicken Sie auf **Punktzahl**. So können Sie mit dem Modell wiedergeben, und sehen, welche Elemente vorgeschlagen werden.

    ![Empfehlungen Benutzeroberfläche: Schaltfläche Ergebnis][reco_score_button]

1. Wählen Sie ein Element aus, um anzuzeigen, welche Elemente als Empfehlungen für dieses Element zurückgegeben werden. Beachten Sie, dass stehen nicht genügend Transaktionen Vorhersagen eines Empfehlungen für ein bestimmtes Element, das System keine Empfehlungen für dieses Element zurückgegeben wird.  Wenn aus irgendeinem Grund ein Element, die keine Empfehlungen zurückgibt verfügen, versuchen Sie, andere Elemente bewerten.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Schritt 6: nächste Schritte ##
Herzlichen Glückwunsch! Sie haben gelernt ein Modells, und klicken Sie dann auf Ihrem System Empfehlungen aus, dieses Modell.  Die Empfehlungen-Benutzeroberfläche ist ein nützliches Tool, das ermöglicht es Ihnen, den Status Ihrer Projekte anzuzeigen und erstellt. 

Jetzt, da Sie ein Modell haben, sollten Sie die Informationen zum programmgesteuerten führen Sie die obigen Schritte. Müssen, um zu erfahren, die API programmgesteuert aufrufen, möchten wir Ihnen, überprüfen Sie den [Empfehlungen-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348) , und Laden Sie die [Empfehlungen Beispiel Anwendung](http://go.microsoft.com/fwlink/?LinkID=759344)einladen.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png

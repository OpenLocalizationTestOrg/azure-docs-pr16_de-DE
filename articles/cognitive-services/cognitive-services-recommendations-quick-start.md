<properties
    pageTitle="Schnellstarthandbuch: Computer Learning Empfehlungen-API | Microsoft Azure"
    description="Azure maschinellen Learning Empfehlungen – Schnellstarthandbuch"
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
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Schnellstarthandbuch für die kognitive Services Empfehlungen-API

Dieses Dokument beschreibt, wie zur integrierten Ihrem Dienst oder die Anwendung, um die [Empfehlungen-API](http://go.microsoft.com/fwlink/?LinkId=759710)verwendet werden.
Klicken Sie auf die Empfehlungen-API und andere Dienste kognitive Weitere Details finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=759709). In diesem Handbuch möglicherweise finden Sie auch die [Empfehlungen-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348) praktischen.


<a name="Overview"></a>
## <a name="general-overview"></a>Allgemeine Übersicht

Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel ist Ihnen Schritt für Schritt durch die Schritte zum Schulen von einem Datenmodell, und klicken Sie auf Ressourcen zu verweisen, mit der Sie das Modell aus Ihrer Umgebung Herstellung nutzen können.

In dieser Übung wird ungefähr 30 Minuten dauern.

Um [Empfehlungen-API](http://go.microsoft.com/fwlink/?LinkId=759710)verwenden, müssen Sie die folgenden Schritte durchführen:

1. Erstellen eines Modells – ein Modell ist ein Container für Ihre Verwendungsdaten, Katalogdaten und Modell Empfehlungen.
1. Importieren von Katalogdaten - Katalog enthält Metadateninformationen über die Elemente an.
1. Importieren Verwendungsdaten - Verwendungsdaten können 2 Arten (oder beides) hochgeladen werden:
  -  Durch Hochladen einer Datei, die die Verwendungsdaten enthält.
  -  Senden von Daten Acquisition Ereignissen.
  Normalerweise hochladen Sie eine Datei für die Verwendung und möglicherweise ein Modell anfänglichen Empfehlungen (bootstrap) erstellen und diese verwenden, bis das System genügend Daten mithilfe des Acquisition Datenformats sammelt.
1. Erstellen eines Modells Empfehlungen: Dies ist eine asynchrone Operation, in der das System Empfehlungen nimmt die Verwendungsdaten und erstellt ein Empfehlungen-Modell. Dieser Vorgang kann einige Minuten oder mehrere Stunden dauern, abhängig von der Größe der Daten und die eigene Konfigurationsparameter dauern. Wenn Sie den Build auslösen, erhalten Sie eine eigene ID. Verwenden Sie diese um zu aktivieren, wenn der Buildvorgang beendet ist, bevor Sie anfangen, Empfehlungen nutzen.
1. Nutzen Sie Empfehlungen - Get-Empfehlungen für ein bestimmtes Element oder eine Liste mit Elementen an.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Lassen Sie uns beginnen

Erstellen eines Modells Empfehlungen beginnt. Klicken Sie dann werden wir Ihnen zum Verwenden der Ergebnisse mithilfe des Modells zu Power Empfehlungen auf einer Website e-Commerce erstellte behilflich sein.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Aufgabe 1: Sie können die Empfehlungen-API signierenden ####

In dieser Aufgabe werden Sie für den Empfehlungen-API-Dienst anmelden, und erstellen Sie ein Empfehlungen-Modell.

1. Wechseln Sie zu der **Azure-Portal** unter [http://portal.azure.com/](http://portal.azure.com/) und melden Sie sich mit Ihrem Azure-Konto an.

1.  Klicken Sie auf **+ neue**.

1. Wählen Sie die Option **Intelligence** aus.

1. Wählen Sie das Produkt **Kognitive Services-APIs** .
Dieses Produkt können Sie ein Abonnement für die kognitive Dienste APIs (Smiley, Text Analytics Computer Vision, usw.) beginnen. Heute Schwerpunkt die Empfehlungen-API auf.

1. Geben Sie auf der Startseite der kognitive Webdienste-API den **Kontonamen** für Ihr Abonnement Empfehlungen ein. (Für Instanz: "MyRecommendations"). Dieser Name darf keine Leerzeichen darin haben.

1. Wählen Sie auf **Typ API** **Empfehlungen**ein.

1. Klicken Sie auf **Preise Ebene**können Sie einen Plan auswählen. Sie können die Ebene **Free** für 10.000 Transaktionen/Monat auswählen. Dies ist eine kostenlose Plan, daher es eine gute Möglichkeit ist, starten Sie das System versuchen. Nachdem Sie zu der Herstellung wechseln, empfehlen wir Sie erwägen Sie die Lautstärke der Anfrage und den Plantyp entsprechend ändern. (Notiz: können Sie jeweils nur ein kostenlosen Ebene-Abonnement haben)

1. Wählen Sie eine **Ressourcengruppe**aus, oder Erstellen eines neuen Kontos aus, wenn Sie eine bereits besitzen.

1. Sie können andere Elemente im Dialogfeld erstellen geändert werden. Es sollte darauf hingewiesen, dass der Anbieter für Ressourcen aus den Vereinigten Staaten Data Centers heute nur unterstützt wird.

1. Nachdem Sie mit getroffene fertig sind, klicken Sie auf **Erstellen**.

1. Warten Sie einige Minuten für die Ressource bereitgestellt werden soll.
Nachdem sie bereitgestellt wird, können Sie zum Abschnitt **Tasten** in das Blade **Einstellungen** wechseln, wo Sie einen primären und sekundären Schlüssel der-API bereitgestellt werden, werden.  Kopieren Sie den Primärschlüssel, wie Sie es beim Erstellen Ihrer ersten Modells benötigen werden.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Aufgabe 2: haben Sie die Daten wieder abrufen? ####

Die Empfehlungen-API Lernen aus Ihrem Katalog und Ihre Transaktionen um Empfehlungen im Zusammenhang mit guten Produkts bereitstellen zu können. Das heißt, müssen Sie es mit guten Daten zu Ihren Produkten (Wir nennen dies als **Katalog** -Datei) und eine Reihe von Transaktionen genügend Speicherplatz für sie interessante Muster des Verbrauchs finden (Wir nennen diese **Verwendung**) feed.

1. Normalerweise möchten Sie die Transaktionsdatenbank für diese Teile der Informationen Abfragen.
Falls Sie diese praktische besitzen, haben wir einige Beispieldaten für Sie basierend auf Microsoft Store Transaktionsdaten bereitgestellt.

 Sie können die Daten aus [hier](http://aka.ms/RecoSampleData)herunterladen. Kopieren Sie und Entpacken Sie die MsStoreData.Zip in einem Ordner auf Ihrem lokalen Computer.

 > **Hinweis:** Beispiel-Code, den Sie herunterladen und Ausführen in Aufgabe 3 weist Beispieldaten, die bereits eingebetteten – die darin enthaltenen, damit diese Aufgabe optional ist.  Allerdings werden diese Aufgabe ermöglichen es Ihnen, eine realistische Datasets herunterladen und ermöglichen es Ihnen, die Eingaben in die Empfehlungen-API besser zu verstehen.

1.  Jetzt werfen Sie einen Blick auf die Katalogdatei. Navigieren Sie zu dem Speicherort, in dem Sie die Daten kopiert.
 Öffnen Sie die Katalogdatei in **Editor**ein.

 Sie sehen, dass die Katalogdatei recht einfach ist. Es weist das folgende format`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32/64 E34 Online DwnLd, Office <br>
 > AAA-04303, OfficeLangPack 2013 32/64 Set Online DwnLd, Office  <br>
 > C9F-00168, KRUSELL Kiruna spiegeln Deckblatt für Nokia Lumia 635 - Kamel, Zubehör

 Wir sollten darauf hinweisen, dass eine Katalogdatei kann wesentlich umfassendere sein, beispielsweise können Sie Metadaten zu den Produkten (Wir nennen diese *Features Element*) hinzufügen. Klicken Sie auf das Format der sollte Abschnitt [Format](http://go.microsoft.com/fwlink/?LinkID=760716) des Katalogelements im Bezug API Weitere Details angezeigt werden.

1. Lassen Sie uns führen Sie den gleichen mit den Verwendungsdaten. Sie können feststellen, dass das Datum für die Verwendung des Formats `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, Einkauf 0003BFFDD4C2148C, 297833400, 2015/08/04T11:02:50, Einkauf 0003BFFDD4C2118D, 297833300, 2015/08/04T11:02:40, Einkauf 00030000D16C4237, 297833300, 2015/08/04T11:02:37, Einkauf 0003BFFDD4C20B63, 297833400, 2015/08/04T11:02:12, Einkauf 00037FFEC8567FB8, 297833400, 2015/08/04T11:02:04, kaufen

Beachten Sie, dass die ersten drei Elemente obligatorisch sind. Der Ereignistyp ist optional. Sie können das [Format der Verwendung](http://go.microsoft.com/fwlink/?LinkID=760712) für Weitere Informationen zu diesem Thema Auschecken.

 > **Wie viele Daten benötigen Sie?**
 <p>
Tja, hängt er die Verwendungsdaten selbst. Das System lernt, wenn Benutzer verschiedene Elemente kaufen. Für einige Build wie FBT ist es wichtig zu wissen, welche Elemente in der gleichen Transaktionen erworben werden. (Wir nennen diese *gemeinsame Vorkommen*). Eine gute Faustregel ist, dass die meisten Elemente in mindestens 20 Transaktionen werden, damit Wenn Sie 10.000 Elemente in Ihrem Katalog hatten, wir empfehlen möchten aktuell mindestens 20 Mal diese Zahl oder Info 200.000 Transaktionen.
Dies ist erneut eine Faustregel. Sie müssen so experimentieren Sie mit Ihren Daten.
</p>

<a name="Ex1Task3"></a>
####Aufgabe 3: Erstellen eines Empfehlungen-Modells####

Jetzt, da Sie verfügen über ein Konto aus, und Sie haben die Daten, uns Erstellen Ihrer erste Modell.

Verwenden Sie die Anwendung Stichprobe in dieser Aufgabe erstellen Ihrer erste Modell.

1. Zunächst einmal sollten Ihnen die [Empfehlungen-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348)bekannt sein.

1. Herunterladen der [Stichprobe Anwendung](http://go.microsoft.com/fwlink/?LinkID=759344) in einem lokalen Ordner.

1. Öffnen Sie in Visual Studio die **RecommendationsSample.sln** Lösung befindet sich im Ordner **c#** aus.

1. Öffnen Sie die Datei **SampleApp.cs** . Beachten Sie, dass in der Datei die folgenden Schritte durch:
 + Erstellen eines Modells
 + Hinzufügen einer Katalogdatei
 + Hinzufügen einer Datei Verwendung
 + Auslösen eines Builds für das Modell
 + Abrufen einer Empfehlungen basierend auf ein Paar von Elementen
<p></p>

1. Ersetzen Sie den Wert für das Feld **AccountKey** mit der Vorgang 1-Taste.

1. Schritt durch die Lösung, und Sie sehen, wie ein Modell erstellt wird.

1. Versuchen Sie, ersetzen die Katalog und die Verwendung-Dateien, die Sie gerade heruntergeladen haben, um ein neues Modell für Microsoft Store, oder Empfehlungen Adressbuch erstellen. Sie müssen als auch der Name des Modells und die Elemente, für die Sie Empfehlungen anfordern, ändern.

1. Wenn das Modell erstellt wird, achten Sie darauf, der **Modell-ID** , wie Sie diesen benötigen, wenn Sie Empfehlungen in Ihrem Unternehmen anfordern.

>  Erfahren Sie weitere Informationen zu Datentypen und wie die Qualität des Builds ausgewertet erstellen [können](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Modell bringen in die Herstellung! ###

Nachdem Sie nun wissen, wie ein Modell erstellen und Empfehlungen nutzen, besteht der nächste Schritt legen Sie es auf Ihrer Website mobilen Anwendung Herstellung ab oder in Ihrem CRM oder ERP-System integrieren.
Offensichtlich, würde jede diese Implementierungen unterscheiden. Da die Empfehlungen-API als einem Webdienst angefordert werden, sollten Sie einfach in jedem der folgenden verschiedenen Umgebungen anrufen sein.

**Wichtige:**  Wenn Sie Empfehlungen aus einem öffentlichen Client (beispielsweise Ihre e-Commerce-Website) anzeigen möchten, sollten Sie einen Proxyserver zum Angeben der Empfehlungen erstellen. Dies ist wichtig, damit Ihre API-Schlüssel für Personen mit externen (möglicherweise nicht vertrauenswürdig) nicht verfügbar gemacht wird.

Hier sind einige Ideen zu Orte, wo Sie Empfehlungen verwenden können:

### <a name="product-page"></a>Produkt (Seite)

**Element Empfehlungen**
<p>Wenn das Modell auf Einkäufe angewiesen wurde, können Ihren Kunden zum *Ermitteln von Produkten, die sind wahrscheinlich relevante an Personen, die das Quellelement erworben haben*.</p>
<p>Wenn Sie das Modell auf angewiesen wurde, klicken Sie auf Daten, wird es Ihren Kunden zu *finden, sind wahrscheinlich von Personen besucht werden sollen, die das Quellelement besucht haben, Produkte*zulassen. Diese Art von Modell möglicherweise ähnliche Artikel zurück.</p>

**Häufig erworben zusammen Empfehlungen**
<p>A zusammen erstellen häufig gekauft gelernt werden konnte, damit Sie können Sätze von Elemente sind wahrscheinlich zusammen mit diesen Artikel erworben werden.</p>

### <a name="check-out-page"></a>Schauen Sie sich die Seite

**Element Empfehlungen**
<p>Ein Empfehlungen Modell als ausführen konnte Eingabemethoden eine Liste von Elementen. Daher könnten Sie alle Elemente im Warenkorb als Eingabe für Empfehlungen erhalten übergeben.
In diesem Fall wird das Modell Empfehlungen bereitstellen, die relevante alle Elemente im Warenkorb angegeben sind.
</p>

### <a name="landing-page"></a>Startseite

**Empfehlungen für Benutzer**
<p>
Ein Empfehlungen Modell als ausführen kann Eingabemethoden die Benutzer-Id an.  Hiermit wird im Verlauf der Transaktionen, die von diesem Benutzer verwendet, individuelle Empfehlungen auf der angegebene Benutzer bereit.
</p>

Schauen Sie sich das [Erste Element Empfehlungen Dokumentation](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Wie geht's weiter?
Herzlichen Glückwunsch, wenn Sie es dies vorgenommen haben weit! Wenn Sie erfahren möchten, dass weitere Sie die vollständige [Empfehlungen-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348) besuchen können, wenn Sie Fragen haben, zögern Sie nicht am Kontaktmlapi@microsoft.com

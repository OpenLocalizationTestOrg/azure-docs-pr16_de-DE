<properties 
    pageTitle="Einrichten und Verwenden der Computer Learning Empfehlungen-API | Microsoft Azure" 
    description="Häufig gestellte Fragen zur Azure maschinellen Learning integriert Microsoft EMPFEHLUNGEN-API" 
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

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Einrichten und Verwenden von maschinellen Learning Empfehlungen-API häufig gestellte Fragen


**Was ist EMPFEHLUNGEN?**

>[AZURE.NOTE]Starten Sie mithilfe von Empfehlungen-API kognitive Service statt dieser Version. Kognitive Empfehlungen Dienst ersetzt werden soll diesen Dienst, und alle neuen Features werden es entwickelt werden. Es hat neue Funktionen wie Batchverarbeitung Support, ein besseres API-Explorer, eine übersichtlichere API einbinden, konsistenter beim Registrieren/Abrechnung Erfahrung usw. aus.
> Weitere Informationen zum [Migrieren zu den neuen kognitive Dienst](http://aka.ms/recomigrate)

Organisationen und Unternehmen, die auf Empfehlungen zu Cross-verkaufen und Weiterverkauf Produkte und Dienste an ihre Kunden verlassen, EMPFEHLUNGEN Azure Computer interessante bietet eine Empfehlungen Self-service-Engine. Es ist eine Implementierung der Onlinezusammenarbeit filtern, die Matrix Factorization als seinen Core Algorithmus verwendet. Entwickler können mithilfe von REST-APIs EMPFEHLUNGEN zugreifen. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Was kann ich mit empfohlenen Verfahren?**

EMPFEHLUNGEN erfordert als Eingabe ein Element oder eine Gruppe von Elementen und eine Liste der relevanten Empfehlungen zurückgegeben. Beispiel: ein Kunde von einem online-Händler klickt ein Produkts. Online Einzelhändler dieses Produkts als Eingabe für EMPFEHLUNGEN sendet, erhält eine Liste der Produkte im Gegenzug und feststellen, welche der folgenden Produkte an den Kunden angezeigt werden. Sie Empfehlungen im Zusammenhang mit Ihren Onlinespeicher optimieren oder informieren Sie Ihren internen verwenden möchten sales Abteilung oder einen Anruf Center.

**Gibt es Einschränkungen Verwendung?**

Empfehlungen weist die folgenden Einschränkungen für die Verwendung:
* Maximale Anzahl von Datenmodellen pro Abonnement: 10
* Maximale Anzahl von Elementen, die ein Katalog enthalten kann: 100,000
* Die maximale Anzahl von Punkten Verwendung, die aufbewahrt werden, ist ~ 5.000.000. Die älteste wird gelöscht werden, wenn neue hochgeladen oder gemeldet werden.
* Maximale Größe von Daten, die in einer e-Mail (z. B. Katalogdaten importieren, importieren Verwendungsdaten) gesendet werden können, ist 200 MB
* Anzahl der Transaktionen pro Sekunde (TPS) für die Erstellung eines Empfehlungen-Modells, das nicht aktiv ist, ist ~ 2 TPS. Die Erstellung eines Empfehlungen-Modells, das aktiv ist, kann bis zu 20 TPS enthalten.

##<a name="purchase-and-billing"></a>Einkaufs- und Abrechnung 


**Was kostet Empfehlungen Kosten während des Zeitraums Launch?**

Empfehlungen ist ein Abonnement-basierten-Dienst. Belasten basiert auf Lautstärke Transaktionen pro Monat. Sie können die [Angebot Seite] überprüfen (https://datamarket.azure.com/dataset/amla/recommendations) auf Microsoft Azure Marketplace Kundenberater.

**Es sind, alle Kosten im Zusammenhang mit Empfehlungen Probleme nachverfolgen und Speichern von Benutzeraktivitäten für mich?**

Nicht im Moment.

**Verfügt Empfehlungen kostenlose Testversion?**

Es gibt eine kostenlose Spur also auf 10.000 Transaktionen pro Monat beschränkt.

**Wann wird ich Empfehlungen in Rechnung werden gestellt?**

Ein kostenpflichtiges Abonnement ist alle Abonnements eine monatliche Gebühr vorhanden ist. Wenn Sie ein kostenpflichtiges Abonnement kaufen, unterliegen Sie sofort für den ersten Monat verwenden. Sie unterliegen den Betrag ein, der das Angebot auf der Abonnementseite (plus anwendbare steuern) zugeordnet ist. Diese monatliche Gebühr besteht jeden Monat auf das gleiche Kalenderdatum als Ihren ursprünglichen Kauf, bis Sie das Abonnement kündigen. 

**Wie kann ich auf einer höheren Ebene Dienst aktualisieren?**

Sie können kaufen oder aktualisieren Sie Ihr Abonnement von [Angebot Seite] (https://datamarket.azure.com/dataset/amla/recommendations) Seite auf Microsoft Azure Marketplace.

Wenn Sie ein Abonnement aktualisieren:

* Transaktionen, die auf Ihrem alten Abonnement verbleibende sind, werden nicht auf Ihr neues Abonnement hinzugefügt. 
* Bezahlen Sie vollen Preis für das neue Abonnement, obwohl Sie haben Ihr altes Abonnement nicht verwendete Transaktionen.

Prozess ein Abonnement zu aktualisieren:

* Nevigate [Angebot Seite] (https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich bei dem Marketplace, wenn Sie nicht bereits angemeldet sind.
* Im rechten Bereich werden alle verfügbaren Pläne aufgeführt. Klicken Sie auf das Optionsfeld für den Plan aus, dem Sie zum aktualisieren möchten.
* Wenn Sie aktualisieren möchten, klicken Sie auf **OK**. Wenn Sie nicht aktualisieren möchten, klicken Sie auf **Abbrechen**.

**Wichtige** Lesen Sie im Dialogfeld sorgfältig, bevor Sie ein upgrade, da es Abrechnung und Verwendung Auswirkungen gibt.

**Wann läuft mein Abonnement zu Empfehlungen?**

Wenn Sie die Abonnement kündigen beenden Ihres Abonnements. Wenn Sie Ihre Abonnements aufheben möchten, finden Sie unter den folgenden Anweisungen.

**Wie breche ich mein Abonnement Empfehlungen ab?**

Gehen Sie folgendermaßen vor, um Ihr Abonnement kündigen. Ist Ihres aktuellen Abonnements ein kostenpflichtiges Abonnement, bleibt Ihr Abonnement wirksam, bis zum Ende der aktuellen Abrechnungszeitraum. Wenn Sie die Kündigung sofort wirksam werden benötigen, erreichen Sie uns [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Notiz** Keine Erstattung wird angegeben, wenn Sie einen Abrechnungszeitraum vor dem Ende eines Abrechnungszeitraum oder nicht verwendete Transaktionen kündigen.

* Navigieren Sie zu [Angebot Seite] (https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich bei dem Marketplace, wenn Sie nicht bereits angemeldet sind.
* Klicken Sie auf **Abbrechen** , um rechts neben der DatasetName und Status. Sie können dieses Abonnement bis zum Ende der aktuellen Abrechnungszeitraum oder Transaktion maximale Anzahl erreicht ist (je nachdem, was zuerst eintritt).

Wenn Sie Ihr Abonnement kündigen sofort, daher können Sie ein neues Abonnement kaufen möchten, die Datei ein Ticket bei [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Erste Schritte mit Empfehlungen

**Ist Empfehlungen für mich?** 

Empfehlungen Computer interessante ist für Organisationen und Unternehmen, die auf Empfehlungen zu Cross-verkaufen und Weiterverkauf Produkten oder Diensten, damit ihre Kunden aufsetzen. Wenn Sie einen Kunden zugänglichen Website, ein Vertriebsmitarbeiter, eine innere haben Vertriebsmitarbeiter oder einem Callcenter, und wenn Sie einen Katalog von mehr als ein paar Dutzend Produkten oder Dienstleistungen anbieten, der untere Zeile möglicherweise nutzbringend verwenden Empfehlungen. 

Ein wenig mit Empfehlungen soll recht einfach sein. Die aktuelle Version von API-basierte erfordert grundlegende programming Kenntnisse. Wenn Sie Hilfe benötigen, wenden Sie sich an den Lieferant, der Ihre Website entwickelt. Wenn Sie eine interne IT-Abteilung oder einer für Entwickler haben, sollten sie keine Empfehlungen für Ihre Arbeitsweise abrufen. 

**Was sind die erforderlichen Komponenten für Empfehlungen einrichten?**

Empfehlungen müssen Sie ein Protokoll der Auswahlmöglichkeiten für Benutzer im Zusammenhang mit Ihrem Katalog verfügen. Wenn Sie keine Nachteile für Ihr solche ein Protokoll haben und Sie verfügen über eine Kunden zugänglichen Website, Empfehlungen können Benutzeraktivitäten für Sie sammeln. 

Empfehlungen erfordert auch einen Katalog von Ihren Produkten oder Dienstleistungen. Wenn Sie keine Nachteile für Ihr Katalog haben, Empfehlungen im Zusammenhang mit die tatsächlichen Kunden Verwendungsdaten verwenden und destillieren Katalog können. Ein impliziter Katalog wird nicht Elemente enthalten, die nicht als Teil des Benutzertransaktionen gemeldet wurden.

**Wie richte ich Empfehlungen zum ersten Mal ein?**

Nach [abonnieren] (https://datamarket.azure.com/dataset/amla/recommendations) zu Empfehlungen, sollten Sie die API-Dokumentation im Microsoft [Azure maschinellen Learning Empfehlungen Schnellstarthandbuch](machine-learning-recommendation-api-quick-start-guide.md) verwenden, um den Dienst einrichten.

**Wo finde ich die API-Dokumentation?** 

Die API-Dokumentation ist [Azure maschinellen Learning Empfehlungen Schnellstarthandbuch](machine-learning-recommendation-api-quick-start-guide.md)an.

**Welche Optionen habe ich Katalog und die Verwendung von Daten in Empfehlungen hochladen?**

Sie haben zwei Optionen für Ihre Daten Katalog und die Verwendung hochladen: Sie können die Daten aus Ihrem System CRM oder andere Protokolle exportieren und Hochladen auf Empfehlungen oder Hinzufügen von Kategorien zu Ihrer Website, die Benutzeraktivitäten verfolgt. Wenn Sie die zweite Methode verwenden, werden die Daten in Azure gespeichert werden.

##<a name="maintenance-and-support"></a>Wartung und support

**Wie groß kann meine Datenmenge sein?**

Jede Datengruppe zurück können bis zu 100.000 Katalogelementen enthalten und bis zu 2048 MB mit Verwendungsdaten.
Darüber hinaus kann ein Abonnement bis zu 10 Datensätze (Modelle) enthalten.

**Wo finde ich den technischen Support für Empfehlungen?**

Technischen Support steht auf der Website [Microsoft Azure unterstützen](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Wo finde ich die nutzungsbestimmungen?**

[Empfehlungen-API Vertragsbedingungen Learning Microsoft Azure-Computer](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 

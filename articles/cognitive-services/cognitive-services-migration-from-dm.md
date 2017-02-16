
<properties
    pageTitle="Migrieren zu Azure kognitive Services Empfehlungen API aus der DataMarket Empfehlungen-API | Microsoft Azure"
    description="Learning Empfehlungen – Migrieren nach Empfehlungen kognitive Dienst Azure-Computern"
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
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migrieren Sie zu Azure kognitive Services Empfehlungen API aus der DataMarket Empfehlungen-API
In diesem Artikel wird veranschaulicht, wie aus der [Microsoft DataMarket Empfehlungen-API](https://datamarket.azure.com/dataset/amla/recommendations) an die [Microsoft Azure kognitive Services Empfehlungen-API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)migrieren.

Die DataMarket Empfehlungen-API wird gestoppt akzeptiert neue Kunden 31 Dezember 2016, und wird als veraltet 28 Februar 2017 markiert.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Wie starten kann ich die Verwendung der Azure kognitive Services Empfehlungen-API?

Um auf die kognitive Services Empfehlungen-API migrieren, führen Sie folgende Schritte aus:

1.  Wenn Sie bereits über ein Azure-Abonnement für eine [Anmelden](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) besitzen. 

1.  Erhalten Sie eine schrittweise Anleitung aus dem [Schnellstarthandbuch](cognitive-services-recommendations-quick-start.md) für die kognitive Services Empfehlungen-API anmelden und diese programmgesteuert verwenden. 

1.  Wechseln Sie zu der [kognitive Webdienste-API Empfehlungen Startseite](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) erfahren Sie mehr über den Dienst und Dokumentation zu finden.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Ich Hiermit wird der Benutzeroberfläche Empfehlungen um meine Modelle zu erstellen. Gibt ein ähnliches Tool für die kognitive Services Empfehlungen-API?

Ist es auch! Die URL für die neue [Benutzeroberfläche Empfehlungen](http://recommendations-portal.azurewebsites.net/) ist http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Ihre Anmeldeinformationen DataMarket funktionieren hier nicht. Anmelden für ein Abonnement im Portal Azure, und erhalten Sie die Konto-Taste zum Verwenden der neuen [Empfehlungen-Benutzeroberfläche](http://recommendations-portal.azurewebsites.net/)erforderlich sind.
Wenn Sie einen Schlüssel-Konto besitzen, finden Sie unter Aufgabe 1 der [Schnellstarthandbuch](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Hat neue API Format die DataMarket Empfehlungen-API identisch?

Die-API unterstützt die gleichen Szenarien und Prozess Zahlungen als diese Szenarien, die in der DataMarket-Version unterstützt, aber die ist-API Benutzeroberfläche wurde aktualisiert, um den [Microsoft-Richtlinien für REST-API](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)entsprechen. Die APIs sind konsistenter und Arbeit besser mit tools, die die Swagger unterstützen.

Um jede API zu verstehen, schauen Sie sich die [API-Explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Verwenden Sie die *versuchen Sie* diese Schaltfläche, um einen Videoanruf API zu testen. Das Format von Dateien, die von der Empfehlungen-API (Katalog und die Verwendung Dateien) verbraucht wurde nicht geändert, sodass Sie verwenden können weiterhin die gleichen Dateien und/oder Infrastruktur, die Sie erstellt haben, um diese Dateien zu generieren.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Was sind einige neue Features in der kognitive Services Empfehlungen-API?

In den letzten zwei Monaten haben wir die neue Funktionen für die kognitive Services Empfehlungen-API veröffentlicht:
-   Empfehlungen Benutzeroberfläche für Schulung und Testen Modelle ohne schreiben eine einzelne Zeile mit code
-   Stapel bewerten, damit Tausende von Empfehlungen gleichzeitig bereitgestellt werden kann
-   Erstellen Sie Kennzahlen Support Abfrage die Qualität des Empfehlungen-Modelle
-   Unterstützung für Business-Regeln
-   Möglichkeit zum Auflisten und Verwendung und Katalog-Dateien herunterladen
-   Die Qualität des Elements Features in einem Modell Empfehlungen Abfragen Rangwerte Build-support
-   Zusätzlichen Möglichkeit, für ein Produkt im Katalog suchen

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Wann wird Microsoft beendet, der die DataMarket Empfehlungen-API unterstützt?

[Empfehlungen-API auf DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) nicht mehr neue Kunden nach dem 31 Dezember 2016 akzeptiert und werden durch 28 Februar 2017 vollständig veraltet sein. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Was geschieht, wenn ich die Daten enthalten, die ich möchte meine Modelle in die kognitive Services Empfehlungen-API neu erstellen?

Wir möchten für Sie diesen Übergang so einfach wie möglich gestalten. Wir helfen Ihnen Ihre alten Modelle aus Ihrem Konto DataMarket auf Ihr neues Azure kognitive Services-Empfehlungen-API Abonnement zu verschieben. Wenden Sie sich an uns unter [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Wir bitten Sie Ihre DataMarket Abonnement-ID und Ihr Abonnement kognitive Services-ID, bereitstellen, bevor wir Ihr neues Konto der Modelle zugeordnet wird.

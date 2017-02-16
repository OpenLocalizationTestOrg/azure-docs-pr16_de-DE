<properties
    pageTitle="Erste Schritte mit Azure Suchen in NodeJS | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Durchgehen Sie, erstellen eine Anwendung suchen auf einen gehosteten Cloud Suchdienst auf Azure NodeJS als Programmiersprache verwenden."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Erste Schritte mit Azure Suchen in NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Erfahren Sie, wie Sie eine benutzerdefinierte NodeJS Suche Anwendung zu erstellen, die Azure-Suche für die Suchfunktion verwendet. In diesem Lernprogramm verwendet die [Azure Suche Dienst REST-API](https://msdn.microsoft.com/library/dn798935.aspx) und mit einer der Objekte in dieser Übung verwendete Vorgänge an.

Wir verwendet [NodeJS](https://nodejs.org) und NPM, [Sublime Text 3](http://www.sublimetext.com/3)und Windows PowerShell auf Windows 8.1 zu entwickeln und Testen Sie den Code.

Um dieses Beispiel ausführen zu können, müssen Sie einen Azure-Suchdienst verfügen, die Sie für die [Azure-Portal](https://portal.azure.com)anmelden können. Eine schrittweise Anleitung finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) .

## <a name="about-the-data"></a>Informationen zu den Daten

Diese Anwendung verwendet die Daten aus den [Vereinigten Staaten geologischen Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), auf den Status der Rhode Insel verringern die Datasetgröße gefiltert. Wir verwenden diese Daten zum Erstellen einer Suche-Anwendungs, die Shapes für markante Gebäude Gebäude wie Krankenhäuser und Schulen sowie geologischen Features wie Streams, Seen und Summits zurückgibt.

In dieser Anwendung das **DataIndexer** Programm erstellt und lädt den Index mithilfe einer [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) Operator, Abrufen von gefilterten USGS Dataset aus einer öffentlichen Azure SQL-Datenbank. Anmeldeinformationen und die Verbindung zur Datenquelle online Informationen in den Programmcode. Es ist keine weitere Konfiguration erforderlich.

> [AZURE.NOTE] Wir ein Filters auf Dataset zu bleiben unter 10.000 Dokument Limit der kostenlosen Preisgestaltung Ebene angewendet werden. Wenn Sie die standardmäßige Ebene verwenden, wird diese Beschränkung nicht angewendet. Details für jede Ebene Preisgestaltung Kapazität finden Sie unter [suchgrenzwerte-Dienst](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Suchen Sie nach der Dienst- und Azure Suchdienst api-Schlüssel

Nach dem Erstellen des Diensts zurück zum Abrufen der URL-Portal oder `api-key`. Verbindungen mit Suchdienst erfordern, dass Sie sowohl die URL haben und eine `api-key` um den Anruf zu authentifizieren.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie in der Leiste Sprung auf **Suchdienst** , um eine Liste aller Dienste Azure-Suche nach der Bereitstellung für Ihr Abonnement.
3. Wählen Sie den Dienst, den Sie verwenden möchten.
4. Auf dem Dashboard Service sehen Sie Kacheln für wichtige Informationen und die wichtigsten Symbol für den Zugriff auf die Admin-Taste.

    ![][3]

5. Kopieren Sie die URL, ein Administrator Key und eine Abfrage-Taste. Sie benötigen alle drei später, wenn Sie die Datei config.js hinzufügen.

## <a name="download-the-sample-files"></a>Herunterladen der Beispieldateien

Verwenden Sie entweder eine der folgenden Vorgehensweisen zum Herunterladen des Beispiels aus.

1. Wechseln Sie zu [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Klicken Sie auf **Herunterladen eines ZIP**, speichern Sie die ZIP-Datei, und extrahieren Sie alle darin enthaltenen Dateien.

Anhand der Dateien in diesem Ordner werden alle nachfolgenden Datei Änderungen und Ausführen Anweisungen vorgenommen werden.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Aktualisieren der config.js. mit der URL für die suchen und api-Taste

Verwendung der URL und api-Taste, die Sie zuvor, kopiert haben, geben Sie die URL, Admin-Taste und Abfrage-Taste in Konfigurationsdatei.

Administrator Tasten erteilen Vollzugriff auf Dienst JOIN-Operationen, einschließlich erstellen oder Löschen eines Indexes und Dokumente geladen. Im Gegensatz dazu sind Abfrage Schlüssel für schreibgeschützte Vorgänge, die von Clientanwendungen, die Verbindung mit Azure Suchen in der Regel verwendet.

In diesem Beispiel gehören wir die Abfrage eingeben, um die bewährte Methode der Verwendung von des Abfrage Schlüssels in Clientanwendungen stärken.

Das folgende Bildschirmabbild zeigt **config.js** öffnen in einem Text-Editor mit den betreffenden Posten abgegrenzt, damit Sie sehen können, wo Sie die Datei mit den Werten aktualisieren, die für Ihre Suchdienst gültig sind.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host für die Stichprobe eine Runtime-Umgebung

Das Beispiel erfordert einen HTTP-Server mit global Npm installiert werden kann.

Verwenden Sie ein PowerShell-Fenster für die folgenden Befehle aus.

1. Navigieren Sie zu dem Ordner, der die Datei **package.json** enthält.
2. Typ `npm install`.
2. Typ `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Erstellen Sie den Index, und führen Sie die Anwendung

1. Typ `npm run indexDocuments`.
2. Typ `npm run build`.
3. Typ `npm run start_server`.
4. Direkte Browser am`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Klicken Sie auf USGS Daten suchen

USGS Datenmenge enthält Datensätze, die zum Zustand Rhode Insel relevant sind. Wenn Sie die **Suche** auf eine leere Suchfeld klicken, erhalten Sie die obersten 50 Einträge den Standardwert.

Einen Suchbegriff eingeben erhalten Suchmaschine etwas auf Gehe zu. Versuchen Sie, einen regionalen Namen eingeben. "Roger Williams" wurde die erste Kontrolle von Rhode Insel. Namen von zahlreichen Parks, Gebäude und Schulen nach ihm.

![][9]

Sie können auch einen dieser Begriffe versuchen:

- Pawtucket
- Pembroke
- Gans + Kap


## <a name="next-steps"></a>Nächste Schritte

Dies ist der erste Azure Suche Lernprogramm basierend auf NodeJS und das Dataset USGS. Im Laufe der Zeit verlängern wir dieses Lernprogramm aus, um zusätzliche Suchfunktionen veranschaulichen, die Sie vielleicht in Ihrer benutzerdefinierten Lösungen verwenden möchten.

Wenn Sie bereits einige Hintergrund in Azure Suche verfügen, können Sie in diesem Beispiel als Sprungbrett verwenden, Dank Suggesters (während der Eingabe oder AutoVervollständigen-Abfragen), Filter und facettierten Navigation. Sie können auch auf der Seite mit den Suchergebnissen verbessern, indem Sie Zähler hinzufügen und Batchverarbeitung von Dokumenten, sodass die Benutzer die Ergebnisse blättern können.

Neu bei Azure suchen? Es empfiehlt sich, versuchen andere Lernprogramme zum Vertrautmachen mit, was Sie erstellen können. Besuchen Sie unseren [Dokumentationsseite](https://azure.microsoft.com/documentation/services/search/) , um weitere Ressourcen finden Sie unter. Sie können auch die Links in unseren [Video und zusammengehörenden Liste](search-video-demo-tutorial-list.md) Weitere Informationen zum Zugreifen auf anzeigen.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png

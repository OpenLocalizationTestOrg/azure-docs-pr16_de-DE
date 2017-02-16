<properties
    pageTitle="Erste Schritte mit Azure Suchen in Java | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="So erstellen Sie eine Cloud gehosteten Suche Anwendung auf Azure Java als Programmiersprache verwenden."
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

# <a name="get-started-with-azure-search-in-java"></a>Erste Schritte mit Azure-Suche in Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Erfahren Sie, wie Sie eine benutzerdefinierte Java-Suche-Anwendung zu erstellen, die Azure-Suche für die Suchfunktion verwendet. In diesem Lernprogramm verwendet die [Azure Suche Dienst REST-API](https://msdn.microsoft.com/library/dn798935.aspx) und mit einer der Objekte in dieser Übung verwendete Vorgänge an.

Um dieses Beispiel ausführen zu können, müssen Sie einen Azure-Suchdienst verfügen, die Sie für die [Azure-Portal](https://portal.azure.com)anmelden können. Eine schrittweise Anleitung finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) .

Wir verwendet die folgende Software zum Erstellen und Testen in diesem Beispiel:

- [Ellipse IDE für Java EE-Entwickler](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Achten Sie darauf, die EE Version herunterzuladen. Einer der Überprüfungsschritte erfordert ein Feature, das nur in dieser Edition gefunden wird.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Informationen zu den Daten

Diese Anwendung verwendet die Daten aus den [Vereinigten Staaten geologischen Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), auf den Status der Rhode Insel verringern die Datasetgröße gefiltert. Wir verwenden diese Daten zum Erstellen einer Suche-Anwendungs, die Shapes für markante Gebäude Gebäude wie Krankenhäuser und Schulen sowie geologischen Features wie Streams, Seen und Summits zurückgibt.

In dieser Anwendung das **SearchServlet.java** Programm erstellt und lädt den Index mithilfe einer [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) Operator, Abrufen von gefilterten USGS Dataset aus einer öffentlichen Azure SQL-Datenbank. Vordefinierte Anmelde- und Verbindungsinformationen zur Datenquelle online sind im Programmcode bereitgestellt. Im Hinblick auf Daten zugreifen ist keine weitere Konfiguration erforderlich.

> [AZURE.NOTE] Wir ein Filters auf Dataset zu bleiben unter 10.000 Dokument Limit der kostenlosen Preisgestaltung Ebene angewendet werden. Wenn Sie die standardmäßige Ebene verwenden, diese Beschränkung gilt nicht, und Sie können den Code zur Verwendung eines vergrößern Datasets bearbeiten. Details für jede Ebene Preisgestaltung Kapazität finden Sie unter [Grenzwerte und Einschränkungen](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Über die Programmdateien

Die folgende Liste beschreibt die Dateien, die in diesem Beispiel relevant sind.

- Search.jsp: Die Benutzeroberfläche bereit.
- SearchServlet.java: Bietet Methoden (vergleichbar mit einem Controller in MVC)
- SearchServiceClient.java: Ziehpunkte HTTP-Anfragen
- SearchServiceHelper.java: Eine Helper-Klasse, die statische Methoden bereitstellt
- Document.Java: Stellt das Datenmodell
- config.Properties: Legt die URL für die suchen und api-Taste
- Pom.XML: Eine Maven Abhängigkeit

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Suchen Sie nach der Dienst- und Azure Suchdienst api-Schlüssel

Alle übrigen API Anrufe in Azure Suchen erfordern, dass Sie die URL und eine api-Key bereitstellen. 

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie in der Leiste Sprung auf **Suchdienst** , um eine Liste aller Dienste Azure-Suche nach der Bereitstellung für Ihr Abonnement.
3. Wählen Sie den Dienst, den Sie verwenden möchten.
4. Auf dem Dashboard Service sehen Sie Kacheln für wichtige Informationen und die wichtigsten Symbol für den Zugriff auf die Admin-Taste.

    ![][3]

5. Kopieren Sie die URL und einen Administrator Schlüssel. Sie werden diese später benötigen, wenn Sie die Datei **config.properties** hinzufügen.

## <a name="download-the-sample-files"></a>Herunterladen der Beispieldateien

1. Wechseln Sie zu [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) , klicken Sie auf Github.

2. Klicken Sie auf **Herunterladen eines ZIP**, speichern Sie die ZIP-Datei auf einem Datenträger und extrahieren Sie die darin enthaltenen Dateien. Erwägen Sie das Extrahieren der Dateien in den Arbeitsbereich Java zu vereinfachen das Projekt später nicht finden.

3. Die Beispieldateien sind schreibgeschützt. Mit der rechten Maustaste Ordnereigenschaften, und deaktivieren Sie das Attribut schreibgeschützt.

Anhand der Dateien in diesem Ordner werden alle nachfolgenden Datei Änderungen und Ausführen Anweisungen vorgenommen werden.  

## <a name="import-project"></a>Importieren von project

1. Wählen Sie in "Ellipse", **Datei** > **Import** > **Allgemeine** > **Vorhandene Projekte in Arbeitsbereich**.

    ![][4]

2. **Wählen Sie Stammverzeichnis aus**navigieren Sie zu dem Ordner mit Beispieldateien. Wählen Sie den Ordner, der den Ordner des Projekts enthält. Das Projekt sollte als ein ausgewähltes Element in der Liste der **Projekte** angezeigt werden.

    ![][12]

3. Klicken Sie auf **Fertig stellen**.

4. Verwenden Sie **Projekt-Explorer** zum Anzeigen und bearbeiten die Dateien aus. Wenn sie noch nicht geöffnet ist, klicken Sie auf **Fenster** > **Anzeigen** > **Projekt-Explorer** , oder verwenden Sie die Verknüpfung, um ihn zu öffnen.

## <a name="configure-the-service-url-and-api-key"></a>Konfigurieren Sie die URL der Dienst und api-Taste

1. Doppelklicken Sie im **Projekt-Explorer**auf **config.properties** zum Bearbeiten der Konfiguration Einstellungen mit den Servernamen und api-Taste.

2. Schlagen Sie in den Schritten in diesem Artikel, wo gefunden der URL und api-Taste im [Azure-Portal](https://portal.azure.com), um die Werte abzurufen, die Sie jetzt in **config.properties**eingeben werden.

3. Ersetzen Sie "Key-Api" in **config.properties**mit dem api-Schlüssel des Diensts. Als Nächstes ersetzt Dienstnamen (die erste Komponente der URL http://servicename.search.windows.net) "Service Name" in derselben Datei.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfigurieren Sie die Projekt, erstellen und Runtime-Umgebungen

1. "Ellipse" im Projekt-Explorer mit der Maustaste des Projekts > **Eigenschaften** > **Aspekte des Projekts**.

2. Wählen Sie **dynamische Web-Modul**, **Java**und **JavaScript**.

    ![][6]

3. Klicken Sie auf **Übernehmen**.

4. Wählen Sie **im Fenster** > **Voreinstellungen** > **Server** > **Runtime-Umgebungen** > **hinzufügen.**.

5. Erweitern Sie Apache, und wählen Sie die Version des Servers Apache Tomcat, die Sie zuvor installiert haben. Auf unserem System installiert wir 8-Version.

    ![][7]

6. Geben Sie auf der nächsten Seite das Tomcat-Installationsverzeichnis aus. Auf einem Windows-Computer wird dies wahrscheinlich c:\Programme\Microsoft Files\Apache Software Foundation\Tomcat *Version*sein.

6. Klicken Sie auf **Fertig stellen**.

7. Wählen Sie **im Fenster** > **Voreinstellungen** > **Java** > **JREs installiert** > **Hinzufügen**.

8. **JRE hinzufügen**wählen Sie **Standard virtueller Computer**.

10. Klicken Sie auf **Weiter**.

11. Klicken Sie in der Definition JRE im JRE Start auf **Verzeichnis**.

12. Navigieren Sie zu **Programme** > **Java** und wählen Sie aus den JDK Sie zuvor installiert haben. Es ist wichtig, den JDK als JRE auswählen.

13. Wählen Sie die **JDK**installiert JREs aus. Ihre Einstellungen sollte ähnlich wie der folgende Screenshot aussehen.

    ![][9]

14. Wählen Sie optional **Fenster** > **Webbrowser** > **Internet Explorer** , um die Anwendung in einer externen Browserfenster zu öffnen. Mit einem externen Browser bietet Ihnen Web Anwendung optimal.

    ![][8]

Sie haben nun die Konfiguration Vorgänge abgeschlossen. Als Nächstes werden Sie erstellen und Ausführen des Projekts.

## <a name="build-the-project"></a>Erstellen Sie das Projekt

1. Klicken Sie im Projekt-Explorer mit der rechten Maustaste in des Projektnamen ein, und wählen Sie **Ausführen als** > **Maven erstellen...** , zu das Projekt konfigurieren.

    ![][10]

8. Geben Sie in der Konfiguration bearbeiten in Ziele "Neuinstallation" ein, und klicken Sie dann auf **Ausführen**.

Statusmeldungen sind Ausgaben für das Console-Fenster an. Erstellen von Erfolg, der angibt, des Projekts ohne Fehler erstellt sollte angezeigt werden.

## <a name="run-the-app"></a>Führen Sie die app

In diesem letzten Schritt führen Sie die Anwendung in einer lokalen Server Runtime-Umgebung.

Wenn Sie noch eine Server Runtime-Umgebung in "Ellipse" angegeben haben, müssen Sie zuerst das ausführen zu können.

1. Erweitern Sie im Projekt-Explorer **Inhaltsordner**aus.

5. Mit der rechten Maustaste **Search.jsp** > **als ausführen** > **Server ausgeführt**. Wählen Sie den Apache Tomcat-Server, und klicken Sie dann auf **Ausführen**.

> [AZURE.TIP] Wenn Sie einen Arbeitsbereich nicht standardmäßige verwendet, um Ihr Projekt zu speichern, müssen Sie so ändern Sie die **Konfiguration ausführen** , zeigen Sie auf den Speicherort des Projekts auf einen Server Start Fehler zu vermeiden. Klicken Sie im Projekt-Explorer mit der Maustaste **Search.jsp** > **Ausführen als** > **Konfigurationen ausführen**. Wählen Sie den Apache Tomcat-Server. Klicken Sie auf **Argumente**. Klicken Sie auf **Arbeitsbereich** oder **File System** , um den Ordner mit dem Projekt festzulegen.

Wenn Sie die Anwendung ausführen, sollten Sie ein Browserfenster, finden Sie unter ein Suchfeld für die Eingabe von Ausdrücken bereitstellen.

Warten Sie etwa eine Minute vor dem Klicken auf **Suchen** , um die Steuerung der Dienst Zeitaufwands zum Erstellen und laden den Index. Wenn ein HTTP 404-Fehler ausgegeben wird, müssen Sie nur ein wenig mehr warten, bevor Sie versuchen erneut.

## <a name="search-on-usgs-data"></a>Klicken Sie auf USGS Daten suchen

USGS Datenmenge enthält Datensätze, die zum Zustand Rhode Insel relevant sind. Wenn Sie die **Suche** auf eine leere Suchfeld klicken, erhalten Sie die obersten 50 Einträge den Standardwert.

Einen Suchbegriff eingeben erhalten Suchmaschine etwas auf Gehe zu. Versuchen Sie, einen regionalen Namen eingeben. "Roger Williams" wurde die erste Kontrolle von Rhode Insel. Namen von zahlreichen Parks, Gebäude und Schulen nach ihm.

![][11]

Sie können auch einen dieser Begriffe versuchen:

- Pawtucket
- Pembroke
- Gans + Kap

## <a name="next-steps"></a>Nächste Schritte

Dies ist der erste Azure Suche Lernprogramm basierend auf Java und dem USGS Dataset aus. Im Laufe der Zeit verlängern wir dieses Lernprogramm aus, um zusätzliche Suchfunktionen veranschaulichen, die Sie vielleicht in Ihrer benutzerdefinierten Lösungen verwenden möchten.

Wenn Sie bereits einige Hintergrund in Azure Suche haben, können in diesem Beispiel als Sprungbrett für weitere experimentieren, Sie zum Erweitern der [Suchseite](search-pagination-page-layout.md)oder [facettierten](search-faceted-navigation.md)zu implementieren. Sie können auch auf der Seite mit den Suchergebnissen verbessern, indem Sie Zähler hinzufügen und Batchverarbeitung von Dokumenten, sodass die Benutzer die Ergebnisse blättern können.

Neu bei Azure suchen? Es empfiehlt sich, versuchen andere Lernprogramme zum Vertrautmachen mit, was Sie erstellen können. Besuchen Sie unseren [Dokumentationsseite](https://azure.microsoft.com/documentation/services/search/) , um weitere Ressourcen finden Sie unter. Sie können auch die Links in unseren [Video und zusammengehörenden Liste](search-video-demo-tutorial-list.md) Weitere Informationen zum Zugreifen auf anzeigen.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png

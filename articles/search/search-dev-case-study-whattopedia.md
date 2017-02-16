<properties 
    pageTitle="Azure Suche Entwicklertools-Fallstudie: Wie WhatToPedia einer Infomedia-Portal auf Microsoft Azure aufgebaut | Microsoft Azure | Cloud gehosteten Suchdienst" 
    description="Informationen Sie zum Erstellen einer Informationen Portal und Metatag Suchmaschine eines Suchdiensts Cloud gehosteten für Entwickler Azure Suche verwenden." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Suchen von Azure Developer-Fallstudie

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Vorgehensweise [WhatToPedia.com](http://whattopedia.com/) ein Infomedia Portal auf Microsoft Azure erstellen

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Die große Idee</font> 


Unsere Idee besteht darin, ein Informationsportal zu erstellen, mit der herstellen von Verbindungen mit Einzelhändler über eine hohe Relevanz, ausgelegte suchen zur Verfügung steht, ähnlich wie Portals Übereinstimmung Touristen bis mit Hotels, Restaurants und Unterhaltung im Gebiet herangewagt Reisekosten Kunden. 

Im Portal, die, das wir stellen, eine Ausnahmefällen hoher Qualität Suchfunktion über Einzelhändler Daten in einem bestimmten Markt ausliefern möchten, bietet helfen Kunden Stores Speicherort und andere zum Einzelhändler anhand finden. Wir werden die Suchmaschine mit einem initial Dataset Startwert, aber im Laufe der Zeit mit Hilfe der Einzelhändler Abonnenten, die Informationen über ihr Unternehmen veröffentlicht tieferen Wert erstellt werden. Werbung, neue waren, verwendeten Marken für spezielle Services-– alle sind Beispiele für Daten, die Wert unserer Website hinzufügt. Diese Daten ist Self gemeldet und der Suche trennen integriert, sobald der Einzelhändler als Abonnent anmeldet. Werbung sowie das Modell Abonnement erläutern die Einnahmen verloren für unsere neue Business.

Suche wird auf eine reine Cloud Interaktionsmodell überwiegend Benutzer sein. Aus Gründen der Skalierung und Low-Kosten werden nahezu jedes Element, die wir, aus der Portal-Oberfläche auf Datenquellen-Steuerelements ausführen, über einen Onlinedienst. Ein Suchmodul, die Funktionen bereitstellt, die wir müssen, mit einsatzbereit, können wir eine Anwendung Suche schnell erstellen, ohne zu erstellen und verwalten ein Suchbereichs-engine uns.

## <a name="what-we-built"></a>Was wir entwickelt

WhatToPedia ist ein Start Infomedia Unternehmen. Wir erstellten [WhatToPedia.com](http://whattopedia.com/) – – derzeit in Test-Market in Nord Europa mit einem Datum Go live 2 Februar 2015. Unsere Kundenbasis ist hauptsächlich Baustein Onlinepräsenz Geschäfte, die eine kostengünstiger Onlineanwesenheit, die einfach benötigen zu verwalten und zu warten.

Unsere Aufgabe besteht darin, Kunden durch eine hervorragende Onlinesuche Erfahrung gewinnen, verstärken Ergebnisse basierend auf Ort oder Netzwerk-Umgebung, Marken, speichern Sie die Namen oder Datentypen gespeichert. Sodass Kunden weist eine Wechselwirkung motivieren Einzelhändler sich unsere Portal abonnieren. Abonnements sind gebührenpflichtige, mit kostengünstiger Geschwindigkeit.

 ![][7] 

Nach der Anmeldung für ein Abonnement hat ein Einzelhändler vorhandenen Profil (erstellt Anfangs uns aus erworbenen Daten), es für weitere Daten zum Werbung, bereitgestellten Marken oder Ankündigungen aktualisiert. Für Funktionen, wie z. B. Sprachen, Währungen akzeptiert, Steuerfreier Einkaufs-, kann sein Self gemeldeten Kunden besser zu gewinnen, die für diese Amenities gefunden werden.

## <a name="who-we-are"></a>Wer wir sind

Mein Name ist Thomas Segato (Microsoft Consulting), und ich mit Jesper Boelling, Lead Entwicklertools am WhatToPedia, zum Entwerfen der Lösung gearbeitet. 

WhatToPedia ist eine Starts Test marketing-Portals neue Geschäfte in Schweden, wo die meisten der 60.000 Einzelhändler Baustein Onlinepräsenz KMU (kleine und mittlere Unternehmen) sind. Da wir wissen, dass eine Person in Europa Einkaufs-mehrere Sprachen spricht und mehrere Währungen führt, erstellen wir Lösungen, die eine mehrsprachige Kunden Tabellenbereich an. Wir erforderlich und gefunden, wird ein Suchmodul, das unsere mehrsprachigen Anforderungen in Azure-Suche unterstützt.

Azure Groß-und Kleinschreibung wird ein Spiel-Wechsler für unsere Projekt. Vor der Verfügbarkeit von Azure suchen aufgewendet wir erheblichen Aufwand über die Schwachstellen der Erstellung eigener Suchmaschine arbeiten. Probleme Azure suchen an, wie ein Onlinedienst die wichtigste Ihnen bei technische und administrative aus unsere Lösung entfernt, vorgesehen, in dem schneller und eine weitere robuste Umgebung vermarkten aufrufen.  

## <a name="how-we-did-it"></a>Wie wir konnten

Unsere Vision wurde, um eine vollständige Infrastruktur basierend auf nur Cloud-Dienste zu erstellen. Microsoft wurde als strategische Plattform ausgewählt, da sie den Anbieter war, die die erforderlichen angeboten Services (für Zusammenarbeit und Entwicklung), bei Bedarf und kostengünstiger Preise skalieren.
 
### <a name="high-level-components"></a>Komponenten auf hoher Ebene

Wir aufgebaut ein Unternehmens am effektivsten, nicht nur auf einer Website. Unterstützung des gesamten Projekts erforderlich ist, eine Vielzahl von Tools und Anwendungen. Wir angenommenen Visual Studio und Visual Studio Team Services für die Entwicklung, Team Foundation Service (TFS) Online für Datenquellen-Steuerelements und Office 365 für Kommunikation und Zusammenarbeit und natürlich Microsoft Azure für alle Website datenbezogene Vorgänge und Speicher Scrum Verwaltung. Mit Visual Studio bereitgestellten IDE direkte provisioning in Azure, mit Integration zu TFS Online Bereitstellen einer zusätzlichen Produktivität.

Im nachstehenden Diagramm wird die auf hoher Ebene Komponenten in die Infrastruktur WhatToPedia verwendet werden.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Wie verwenden wir Microsoft Azure

Betrachten den grünen Feldern im vorherigen Diagramm aus, sehen Sie, dass die Lösung WhatToPedia auf diese Dienste erstellt wurde:

- [Azure suchen](https://azure.microsoft.com/services/search/)
- [Azure Websites mit MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs für geplante Vorgänge](../app-service-web/websites-webjobs-resources.md)
- [SQL Azure-Datenbank](https://azure.microsoft.com/services/sql-database/)
- [Azure BLOB-Speicher](https://azure.microsoft.com/services/storage/)
- [SendGrid e-Mail-Übermittlung](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Zentrale die Lösung ist, Daten und suchen. Der Datenfluss aus den Anbieter Reseller an den Kunden Ende ist unten dargestellt:

  ![][9]

Primärer Datenspeicher ist die Reseller und Buchhaltungsdaten in Azure SQL-Datenbank. Diese besteht aus den anfänglichen Dataset sowie Einzelhändler-spezifische Daten über einen Zeitraum hinzugefügt. Wir verwenden eine Azure WebJob, Updates von SQL-Datenbank gesendet werden, um die Suche trennen in Azure suchen.

### <a name="presentation-layer"></a>Präsentation layer

Das Portal ist eine Website Azure in MVC 4 und [Twitter-Bootstrap](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29)implementiert. Wir haben MVC entschieden, weil einen viel übersichtlicher Ansatz HTML als ASP.NET formularbasierte Entwicklung verfügbar sind. Um zu vermeiden, dass apps für mehrere Geräte erstellen und Verwalten von mehreren mobilen Plattformen, wurde Twitter-Bootstrap gewählt, um alle Geräte und Plattformen unterstützen.

### <a name="authentication-permissions-and-sensitive-data"></a>Authentifizierung, Berechtigungen und vertrauliche Daten

Kunden Durchsuchen der Website anonym aus. Als solche, es gibt keine Anmeldung Anforderungen für Kunden noch speichern wir Consumer Daten. 

Einzelhändler sind anders. Hier speichern wir Profilinformationen öffentlich zugängliche, Abrechnungsinformationen und Medieninhalte, die auf der Website verfügbar gemacht werden sollen. Jeder, der die Website abonniert Einzelhändler erhalten Sie ein Benutzerkonto, das zum Authentifizieren des Benutzers vor dem Bereitstellen von Updates für des Abonnenten Profil verwendet.  Wir speichern sicher alle Abonnentendaten in Azure SQL-Datenbank und Azure BLOB-Speicher.
Wir entschieden für eine Authentifizierungsmodell basierend auf .NET formularbasierte Authentifizierung. Wir haben entschieden, dieser Ansatz zur zugehörigen Vereinfachung; Wir haben die Rollen, UI Supports und anderen irrelevante Features, die im Zusammenhang mit anderen Vorgehensweisen benötigen. 

Um sicherzustellen, dass Einzelhändler nur die Daten angezeigt, die sie gehört, erstellt wir einem Einzelhändler ID für jeden Einzelhändler, die auf alle nachfolgend verwendet wird lesen und Schreiben von Operationen, bei denen Einzelhändler-spezifische Daten. Bei diesem Ansatz gefunden wir, dass wir nicht einzelne Einzelhändler Datenbankberechtigungen erteilen müssen. Alle Einzelhändler interagieren mit dem System unter einer einzelnen Datenbankrolle, mit der Einzelhändler ID als unsere Daten Isolation Methode.

Da unser Unternehmen zu lediglich können die untergeordneten Effekte (steuernde Weitere Business an Einzelhändler, erstellen Incentive ankündigen und Abonnieren) wir die Linie am Behandlung Einkäufe über das Web zeichnen. Wird nicht finden Sie als solche einen Einkaufswagen auf unsere Website, die um unser Anforderungen Sicherheit zu vereinfachen. 

Eine andere Vereinfachung, die wir beschäftigt wurde unsere Abrechnung und Konten zu zahlenden Vorgänge überlassen. Durch routing Zahlung Kundeninformationen direkt an einer Drittanbieter-([SveaWebPay](http://www.sveawebpay.se/)), wir zuordnen zu speichern und schützen wichtige Daten in unseren Datenspeicher Risiken zu verringern. 

### <a name="search-engine"></a>Suchmaschine

Das Herzstück unserer Lösung ist Suchmaschine auf Azure Suchdienst erstellt. Zunächst wir erstellt eine benutzerdefinierte Suchmaschine, aber während dieses Vorgangs realisiert wir die Komplexität und Aufwand sehr hoch war und, die aufgefordert uns andere Alternativen zu berücksichtigen. 

Grundlegende Features, die wichtigsten uns enthalten wurden:

- Filter
- Facettierten navigation
- Steigerung der Ergebnisse
- Seitennavigation durch AJAX

Internet-Suche uns auf das folgende Video an, die uns Azure Suche auszuprobieren inspired gebracht: [Aneignen bei TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Nachdem Sie sich das Video ansehen, wurden wir bereit sind, basierend auf wir haben gesehen Prototypen erstellen. Da wir bereits ein Datenmodell in MVC verwendet haben, wurde die Prototypen erstellen recht einfach, da die Daten durchsuchbare Ausdrücke enthalten, und wir bereits erforderlichen für wie wir wollten Vertriebsstrategie, Sortieren und Filtern der Daten im gearbeitet hatten. 

Dies ist die Vorgehensweise wir die Prototypen erstellen.

**Konfigurieren von Azure Suchdienst**

1. Melden Sie sich klassischen Azure-Portal und den Suchdienst zu Ihrem Abonnement hinzugefügt. Wir verwendet die gemeinsam genutzte Version (mit unseren Abonnement kostenlos).
2. Erstellen eines Indexes. Für die Prototypen verwendet wir im Portal Benutzeroberfläche definieren die Felder für die Suche und die Punktzahl Profile erstellen. Unsere Punktzahl Profil basiert auf Standortdaten: Land | Ort | Adresse (finden Sie unter: Punktzahl Profile hinzufügen).
3. Kopieren Sie die URL der Dienst und Admin-api-Taste, um unser Konfigurationsdateien. Dieser Schlüssel ist auf der Seite Suchen im Portal und wird verwendet, um den Dienst authentifizieren.
    
**Entwickeln eines Suche Indexer Auftrags – Windows-Konsole**

1. Gelesen Sie alle Händler aus Datenbank.
2. Aufrufen der Suche Azure API zum Hochladen von Wiederverkäufern nacheinander (finden Sie unter: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Festlegen einer Eigenschaft in der Datenbank, die Händler für inkrementell Indizierung indiziert ist. Dazu haben wir Hinzufügen eines Felds für "Indexer", in dem die Indizierungsstatus eines Profils (oder nicht indiziert) gespeichert. 

Finden Sie im Anhang für den Codeausschnitt, der den Auftrag Indexer erstellt werden soll.

**Entwickeln einer Suche Web-Portal – MVC**

1. Anrufen Azure Suchdienst, um alle Dokumente aus Suchvorgängen zu erhalten (siehe: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Extrahiert werden folgende Antwort des Suche (mithilfe von json.net http://james.newtonking.com/json)
   - Ergebnisse
   - Aspekte
   - Ergebnisanzahl
   - Entwickeln Sie eine Benutzeroberfläche für die Anzeige von Suchergebnissen, Aspekte und zählt (Dies gab es bereits bereits).

Dies ist der Code, den wir verwendet, um die Ergebnisse bei Azure-Suche zu erhalten:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Nach Ort verstärken**

Wahrscheinlich enthalten die wichtigste Anforderung zur Überprüfung in die Prototypen ein Suchschlüsselwort Speicherort der Abfrage hinzufügen. Unbedingt unsere-Portal an, wenn ein Benutzer gibt einen Ortsnamen in das Feld Suchen ein, die Abfrage, die die Händler in den angegebenen Ort höher als Wiederverkäufer Probleme das Schlüsselwort Ort in der Beschreibung Rangfolge würden. Für diese Anforderung verwendet haben wir Punktzahl Profil in das Feld "Ort" höher als andere Felder Rangfolge.

**Unterstützung für mehrere Sprachen**

Wir zum richtigen Suchergebnisse im korrekten Sprachen anzuzeigen und bieten eine Option zum Suchen nach die gleichen Ergebnisse in verschiedenen Sprachen erforderlich sind. Die beiden Seiten dieses Problems wurden: 

- Suchen nach Wörtern in mehreren Sprachen
- Anzeigen von Suchergebnissen in die richtige Sprache

Wir gelöst das Webpart Präsentation durch Hinzufügen eines Dokuments für die einzelnen Sprachen mit lokalisierten Text und einer Eigenschaft für die Sprache aus. Wenn ein Benutzer einen Suchbegriff eingibt wir Benutzer `$filter` mithilfe von Ausdrücken auf die Sprache des Benutzers Filtern ausgewählt hat.

Jedes Dokument verfügt über eine ausgeblendete Eigenschaft mit dem Namen "Orte" auf den Typ der Websitesammlung erstellt. Diese Eigenschaft speichert Stadtnamen in allen Sprachen, durch den Benutzer in mehreren Sprachen zu suchen.

###<a name="data-storage"></a>Speichern von Daten

Alle Daten (Profil, Abonnement und Buchhaltung) in SQL-Datenbank gespeichert. Alle Mediendateien werden in Azure BLOB-Speicher, einschließlich Bildern und Videos von der Einzelhändler bereitgestellten gespeichert. Verwenden separate BLOB-Speicher isoliert die Effekte des Dateiuploads; Dateien werden nie gemischte mit der Website, damit wir nicht die Website neu zu erstellen, sobald wir Dateien hinzufügen.

Ein wichtiger Vorteil unserer Entwurfsphase Speicher ist, dass mehrere Entwickler einer einzelnen Entwicklung Speicherplatz freigeben können. Eine Voraussetzung für das Projekt WhatToPedia wurde ein Entwicklungsumgebung innerhalb von 15 Minuten, einschließlich Reseller Daten, Bilder und Videos erstellen können. Abrufen der neusten Daten aus TFS Online Ausführen eines SQL-Skripts und den Importvorgang ausgeführt, kann eine vollständige Umgebung im Handumdrehen gar gemeistert werden. Diese Methode wird auch den staging-Prozess verbessert.

###<a name="webjobs"></a>WebJobs

Wir verwenden Azure WebJobs zum Aktualisieren von Daten auf den Index. Durch das Erstellen einer Suche Indexer Position, wurde das Webpart Indizierung sehr einfach in unsere Lösung integrieren. Der einzige Code ändern wir vorgenommen wurde, um den Auftrag Indexer aufzunehmen wurde Hinzufügen einer `Indexed` Feld unsere Datenmodell, um den Indexstatus anzugeben. Wenn ein neues Profil hinzugefügt oder aktualisiert wird, wird die `Indexed` Feld falsch festgelegt ist. Dasselbe gilt, wenn der Einzelhändler vertretene Profildaten über das Portal geändert wird.  

Der Auftrag gesucht, der alle Zeilen Probleme `Indexed` auf falsch festgelegt. Wenn die Zeile gefunden wird, wird das Dokument auf Azure Suche gebucht und dann die `Indexed` Feld festgelegt ist true. Wir haben für das Hinzufügen und Aktualisieren von Daten, da der Azure Suchdienst tatsächlich dafür sorgt planen müssen. Wenn Sie ein Dokument, die bereits vorhanden ist hinzufügen, wird der Dienst automatisch ein Update ausführen.

Alle Aufträge des Web wurden als Console Applikationen entwickelt, die geladen Azure Websites als ZIP-Dateien, entpackt haben und dann geplant werden können.

5 Minuten als geplanten Web Aufgaben ausführen ist der Auftrag geplant. Wir berechnet, dass der Dienst zum Hochladen von Dokumenten 3.000, etwa drei Minuten dauert die in unseren Anforderungen wurde. 

> [AZURE.NOTE] Es ist ein Prototypen Indexer-Feature, das vor kurzem in Azure suchen eingeführt wurde. Dieses Feature befindet sich zu spät für uns in unseren-Updates nutzen, jedoch scheint es lösen des gleichen Problems Inanspruchnahme wir unsere Indexer Aufgabe, welche ist zu laden Datenvorgänge zu automatisieren.


###<a name="backup-strategy"></a>Zusätzliche Strategie

Wir so gestaltet mehreren Ebenen Sicherung Strategie aus einem Bereich von schwerwiegenden Fehler nach unten bis zum Wiederherstellen eines einzelnen Geschäfts Szenarien wiederherstellen. Die zu schützende Ressourcen gehören drei Arten von Daten (-Website, Abonnentendaten und Mediendateien). 

Zuerst, indem Sie halten den Website-Quellcode in TFS Online, wir wissen, wenn die Website nach unten geht, wir es neu erstellen können, indem Sie erneut veröffentlichen aus TFS. 

Abonnentendaten in SQL Azure-Datenbank ist die am häufigsten vertrauliche Anlage. Wir wieder dies mithilfe der integriertes-Funktion (siehe [Azure SQL-Datenbank sichern und Wiederherstellen](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Der Sicherungsdatei Zeitplan zeigt vollständige Sicherung wöchentlich, Differenz Datenbanksicherungskopien einmal täglich und Transaktion Log Sicherungskopien 5 Minuten.  Ausgehend von der Größe der Daten, ist diese Lösung für unsere sofortige und geplanten Datenbestände mehr als ausreichend.

Speichern Sie, Bild-und Videodateien in Azure BLOB-Speicher. Wir werden weiterhin den ultimativen Sicherung Plan für diesen Daten Auswertung in Betracht ziehen Cloudberry Explorer für Azure als mögliche Lösung. Jetzt verwenden wir eine WebJob, um Bilder und Videos an eine andere Stelle kopieren.

##<a name="what-we-learned"></a>Wir haben gelernt

Da wir Daten bereits verwendet haben, wurde die einfache Prüfung des Konzepts herstellen. Innerhalb von Stunden hatten wir Prototypen mit Aspekten Indikatoren, Auslagern, eingestuft Profile und Suchergebnisse. Die Suchergebnisse wurden so präzise, wir entschieden, die an den Kunden Ende präsentiert Filter zu entfernen. 

Die wichtigsten plötzlich für uns wurde, wie schnell wir Azure Suche weitere konnte und wie viel wir wieder erhalten haben. Literal, wir festgestellt Prüfung des Konzepts in ein paar Stunden (siehe Anmerkung unten), 500 Codezeilen mit 3 Codezeilen in der front-End-Anwendung (plus einer neuen WebJob) ersetzen und bessere Ergebnisse zurückgegeben. 

In früheren Versionen implementiert unserem Code Seitennavigation, zählt und andere Verhaltensweisen, die Standardansicht zu suchen. Über die Suche Azure gehören die Ergebnisse, die wir zurückzukehren die Suche Zugriffe, Aspekte, die Daten, zählt – privates erforderlich, und wir uns angeben müssen wurden auszulagern. Es auch enthalten verstärken und integrierte facettierten Navigation, die wir in unserer ursprünglichen Lösung installiert hatten.

Die größte Herausforderung während der Implementierung wurde, dass es einer Vorschauversion wurde und Suchen von Informationen und freigegebenen Erfahrung schwierig war. Sobald wir ein paar Punkte verbunden sind, haben wir, dass mit Azure Suchdienst ziemlich einfach aufgrund der REST-API und JSON-Datenformat wurde gefunden. Wir konnten rufen Sie Rahmen direkt am häufigsten open Source-Plug-Ins wie JQuery JSON.Net und konnten wir-Tools, wie Fiddler für schnelle experimentieren und für das Debuggen verwenden. 

> [AZURE.NOTE] Abgesehen von den Daten vorbereitet konnten wir, dass uns die Prototypen bereits erstellen Etikettenlayout verstanden Technologie funktioniert, dass wir produktiver und weitere appreciative der integrierten Features wie suchen. Wenn Sie auf die Abfrage Konstruktion Suchen von abheben müssen, facettierten Navigation, Filter usw. Prototypen länger dauern erwarten. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Aspekte, die in die Präsentation Suchseite steuern

Eines der unsere Befunde während der Prüfung des Konzepts war Aspekte sorgfältig vorab zu planen. Nach dem Laden einer große Datenmenge in die Lösung, gesehen haben, einfach die Menge der Aspekte zu hoch, um den Benutzern präsentieren wurde. 

Wir dies durch Einschränken des Vertriebsstrategie Count-Parameters gelöst. Der Count-Parameter begrenzt eine harte die Anzahl der Aspekte, die an den Benutzer zurückgegeben. Ein Link, der eine Diskussion des Count-Parameters finden Sie [hier](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs für die Planung von Vorgängen

Azure Suche war das nur ansprechende plötzlich für uns nicht. Wir festgestellt, dass WebJobs unsere laden Datenvorgänge zu Azure automatisieren mit deutlich überlegen unserem vorherigen Ansatz, wurde die umfasste mithilfe eines dedizierten virtuellen Computers Windows-Scheduler mit geplanten Vorgängen zum Aktualisieren des Suchindexes ausgeführt. WebJobs wurde einfacher zu konfigurieren und leichter zu debuggen und natürlich viele kostengünstiger als einen dedizierten virtuellen bezahlen müssen.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB-Speicher-Explorer zum Aktualisieren von Bildern

Wir gefunden, die mit [Azure BLOB-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) (verfügbar auf Codeplex) in Bild und Video-Updates zu der Website verwalten sehr hilfreich sein. Wir verwenden diese als Entwicklertools manuell aktualisiert werden, Bilder und Videos, die unsere Hauptfenster Website gehören. Wir ermittelt, dass flexibler als Bereitstellen von Änderungen auf das Portal werden, und eine vollständige Testiteration entfällt, Bedarf ein Bild zu aktualisieren. 

##<a name="a-few-final-words"></a>Einige abgeschlossen Wörter

Danke klicken, um die hervorragende Mitarbeiter bei WhatToPedia möchten uns ihre Geschichte freigeben!  

Wir hoffen, dass Sie diese Fallstudie nützlich gefunden. Wenn Sie zur Azure-Suche verwenden jetzt, empfiehlt ich einige Ressourcen, die Sie entlang beschleunigen:

- [Dedizierter zu Azure-Forum im MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow verfügt auch über eine Kategorie](http://stackoverflow.com/questions/tagged/azure-search)
- [Dokumentationsseite Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure Search-Dokumentation auf MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Anlage: Suchen Indexer WebJob

Mit dem folgende Code erstellt Indexer erwähnt im Abschnitt darüber, wie die Prototypen erstellen.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 

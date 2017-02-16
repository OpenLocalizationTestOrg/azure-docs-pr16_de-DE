<properties 
    pageTitle="So verwenden Sie bewerten Profile in Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst" 
    description="Optimieren von Rangfolgen bis bewerten Profile in Azure suchen, einen gehosteten Cloud Suchdienst auf Microsoft Azure suchen." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>So verwenden Bewertungssystem Profile in Azure-Suche

Punktzahl Profile sind ein Feature von Microsoft Azure suchen, die die Berechnung der Ergebnisse der Suche, anpassen beeinflussen, wie die Elemente in einer Liste der Suchergebnisse eingestuft werden. Sie können sich vorstellen bewerten Profile als eine Möglichkeit zum Modell Relevanz, durch Steigerung der Elemente, die vordefinierte Kriterien entsprechen. Angenommen Sie, dass die Anwendung einer online Hotel Reservierung-Website ist. Durch verstärken die `location` Feld Suchbegriffe, die einen Ausdruck wie Seattle Ergebnis wird in höheren Ergebnisse für Elemente enthalten, die in Seattle müssen die `location` Feld. Beachten Sie, dass mehrere Punktzahl Profil angepasst oder keine überhaupt, werden kann, ist die standardmäßige Punktzahl für die Anwendung ausreichend.

Experimentieren Sie mit der Punktzahl Profile helfen, können Sie eine Stichprobe Anwendung herunterladen, die Punktzahl Profile wird verwendet, um die Rangfolge von Suchergebnissen zu ändern. Im Beispiel ist eine Console-Anwendung – vielleicht nicht sehr realistische für die Entwicklung praktisches – aber geeignet trotzdem lernen bereit. 

Die Stichprobe-Anwendung veranschaulicht Punktzahl Verhaltensweisen fiktive Daten aufgerufen, mit der `musicstoreindex`. Beispiel-app einfach zu verwendenden erleichtert das Bewertungssystem Profile und Abfragen zu ändern, und klicken Sie dann die sofortige Effekte Rang bestellt angezeigt, wenn das Programm ausgeführt wird.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Die Anwendung Beispiel ist in c# geschrieben mit Visual Studio 2013. Versuchen Sie die kostenlose [Visual Studio 2013 Express Edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) , wenn Sie bereits eine Kopie der Visual Studio besitzen.

Sie benötigen ein Azure-Abonnement und einen Azure Suchdienst für dieses Lernprogramm erforderlich. Hilfe bei der Einrichtung von den Dienst finden Sie unter [erstellen ein Suchdiensts im Portal](search-create-service-portal.md) .

[AZURE. EINSCHLIEßEN [benötigen Sie ein Azure-Konto zum Bearbeiten dieses Lernprogramms:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Herunterladen der Stichprobe-Anwendungs

Wechseln Sie zu [Azure Suche bewerten Profile Demo](https://azuresearchscoringprofiles.codeplex.com/) , auf Codeplex herunterladen die Stichprobe-Anwendung, die in diesem Lernprogramm beschrieben.

Klicken Sie auf der Registerkarte Quellcode auf **herunterladen** , um eine Zip-Datei der Lösung zu erhalten. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config bearbeiten

1. Nachdem Sie die Dateien zu extrahieren, öffnen Sie die Lösung in Visual Studio für die Konfigurationsdatei bearbeiten.
1. Doppelklicken Sie im Explorer-Lösung auf **app.config**. Diese Datei gibt an, der Endpunkt und ein `api-key` zum Authentifizieren der Anforderung verwendet. Sie können diese Werte vom klassischen-Portal zu erhalten.
1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
1. Wechseln Sie zu dem Dienst Dashboard, für die Suche Azure.
1. Klicken Sie auf die Kachel **Eigenschaften** , um die URL zu kopieren
1. Klicken Sie auf die Kachel **Tasten** zum Kopieren der `api-key`.

Wenn Sie die URL hinzugefügt haben und `api-key` zu app.config, Anwendungseinstellungen sollte wie folgt aussehen:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Untersuchen der Anwendungs

Sie fast bereit sind, zu erstellen, und führen Sie die app, aber bevor Sie ausführen möchten, schauen Sie sich die JSON-Dateien erstellen und Auffüllen des Indexes verwendet.

**Schema.JSON** definiert Index, einschließlich der Punktzahl Profile, die hervorgehoben werden in dieser Demo an. Beachten Sie, dass das Schema alle Felder im Index definiert, einschließlich nicht durchsuchbare Felder, wie z. B. verwendet `margin`, die Sie in einem Punktzahl Profil verwenden können. Hinzufügen [einer Punktzahl-Profil, um eine Azure Suchindex](http://msdn.microsoft.com/library/azure/dn798928.aspx)wird bewerten Profilsyntax beschrieben.

**Daten1-3.json** enthält die Daten, die über ein paar Genres 246 Alben an. Die Daten ist eine Kombination der tatsächlichen Album und Interpreten Informationen mit fiktive Feldern wie `price` und `margin` verwendet, um Suchabfragen zu veranschaulichen. Die Datendateien entsprechen den Index und Azure Suchdienst geladen sind. Nachdem die Daten geladen und indiziert ist, können Sie Abfragen für diese ausgeben.

**Program.cs** führt die folgenden Vorgänge:

- Öffnet ein Console-Fenster.

- Stellt eine Verbindung mit Azure-Suchvorgänge, die mit der URL und `api-key`.

- Löscht die `musicstoreindex` falls vorhanden.

- Erstellt eine neue `musicstoreindex` mithilfe der Datei schema.json.

- Füllt den Index verwenden die Datendateien.

- Den Index mit vier Abfragen in Abfragen. Beachten Sie, dass die Punktzahl Profile als Abfrageparameter angegeben sind. Alle Abfragen suchen 'am besten' für die gleichen Ausdruck. Die erste Abfrage veranschaulicht Standard bewerten. Verbleibenden drei Abfragen verwenden Bewertungssystem Profil.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Erstellen Sie, und führen Sie die Anwendung

Um die Konnektivität oder bei der Assemblys Bezug auszuschließen, erstellen Sie, und führen Sie die Anwendung, um sicherzustellen, dass es liegen keine Probleme entwickelt sich zuerst aus. Es sollte eine Console-Anwendung öffnen im Hintergrund angezeigt. Alle vier Abfragen führen Sie nacheinander ohne Anhalten fortzusetzen. In vielen Systemen führt das gesamte Programm in weniger als 15 Sekunden ein. Enthält die Console-Anwendung eine Meldung, die besagt "abgeschlossen. Drücken Sie die EINGABETASTE zum Fortsetzen", das Programm wurde erfolgreich abgeschlossen. 

Zum Vergleichen von Abfrage wird ausgeführt, können kennzeichnen-kopieren und Einfügen die Abfrageergebnissen aus der Konsole und fügen Sie sie in einer Excel-Datei. 

Die folgende Abbildung zeigt die Ergebnisse aus der ersten drei Abfragen-nebeneinander. Alle Abfragen verwenden Sie den gleichen Suchbegriff 'am besten' die in zahlreichen Albumtiteln angezeigt wird.

   ![][10]

Die erste Abfrage verwendet die standardmäßige bewerten. Da der Suchbegriff nur in Albumtiteln wird und keine anderen Kriterien angegeben ist, werden Artikel optimal' mit dem im Albumtitel in der Reihenfolge zurückgegeben, in denen Sie der Suchdienst findet. 

Die zweite Abfrage ein Bewertungssystem Profil verwendet, aber beachten Sie, dass das Profil keine Auswirkung hatte. Die Ergebnisse sind identisch mit denen der ersten Abfrage. Dies ist, da das Profil Punktzahl steigert die ein Felds ("Genre" "), das nicht von der Abfrage Belang ist. Der Suchbegriff ist "beste" nicht in ein beliebiges Feld 'Genre' für jedes Dokument vorhanden. Wenn ein Bewertungssystem Profil keine Auswirkung hat, sind die Ergebnisse der standardmäßigen bewerten identisch.  

Die dritte Abfrage ist der ersten Nachweis Profil Einfluss bewerten. Der Suchbegriff empfiehlt weiterhin '', damit wir arbeiten mit demselben Satz von Alben, aber da das Bewertungssystem Profil bietet zusätzliche Kriterien, die steigert die 'Bewertung' und 'letzten-Aktualisierung', einige Elemente sind Verkehr höher in der Liste.

Die nächste Abbildung zeigt die Abfrage vierte und letzte, nach 'Gewinnspanne' erhöht. Das Feld 'gesamt' kann nicht durchsucht werden und nicht in den Suchergebnissen zurückgegeben werden. Randwerte wurde manuell hinzugefügt, mit dem Tabellenblatt zu verdeutlichen, damit die Nummer, die mit höheren Seitenränder Elemente weiter oben in der Liste der Suchergebnisse angezeigt. 

   ![][9]

Jetzt, da Sie mit Punktzahl Profile, ändern Sie das Programm mit anderen Abfragesyntax, Testen vertraut gemacht haben bewerten Profile oder umfangreichere Daten. Links im nächsten Abschnitt finden Sie weitere Informationen.

<a id="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Benutzerprofilen bewerten. Details finden Sie unter [Hinzufügen einer Punktzahl-Profil, um eine Azure Suchindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Weitere Informationen zum Suchen die Formelsyntax und die Abfrage Parameter. Details finden Sie unter [Dokumente durchsuchen (Azure Suche REST-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Schritt zurück, und erfahren mehr über indexerstellung müssen? [Schauen Sie sich dieses Video an](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) , zu verstehen, die Grundlagen.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 
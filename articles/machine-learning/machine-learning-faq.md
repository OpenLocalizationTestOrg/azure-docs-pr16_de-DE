<properties
    pageTitle="Lernen häufig gestellte Fragen zur Azure-Computern | Microsoft Azure"
    description="Azure maschinellen Learning Einführung: häufig gestellte Fragen zur darüber liegendem Abrechnung, Funktionen und Schwächen einen Cloud-Service für optimierten Vorhersage Modellierung."
    keywords="maschinelle learning Einführung, Vorhersage Modellierung, was maschinellen learning ist"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="garye"/>

# <a name="azure-machine-learning-frequently-asked-questions-faq-billing-capabilities-limitations-and-support"></a>Azure maschinellen Learning häufig gestellte Fragen (FAQ): Abrechnung, Funktionen, Einschränkungen und Support

Hier finden Sie Antworten auf Fragen zur Azure maschinellen Schulung, einen Cloud-Dienst für Vorhersage Modelle entwickeln und operationalizing Solutions über Webdienste. Hier behandelt Fragen zu den Dienst verwenden, einschließlich des Modells, Funktionen, Einschränkungen und Support.

## <a name="general-questions"></a>Allgemeine Fragen

**Was ist Azure maschinellen Schulung?**

Azure maschinellen Learning ist ein vollständig verwaltete-Dienst, den Sie erstellen, testen, Steuerung und Verwalten von Vorhersage analytisches Lösungen in der Cloud verwenden können. Mit nur einem Browser können Sie Anmeldung, Daten hochladen und Computer Learning Versuche sofort zu starten. Vorhersagen Modellierung ziehen und ablegen, eine große Palette der Module und einer Bibliothek Vorlagen macht allgemeine maschinellen Learning Aufgaben einfach und schnell zu starten.  Weitere Informationen finden Sie unter der [Übersicht über den Computer-Schulung Azure](https://azure.microsoft.com/services/machine-learning/). Eine Einführung von maschinellen Learning darüber liegendem wichtige Terminologie und Konzepte finden Sie unter [Einführung zu Azure Computer lernen](machine-learning-what-is-machine-learning.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Was ist maschinelle Learning Studio?**

Maschinelle Learning Studio ist eine Arbeitsfläche-Umgebung, die Sie über einen Webbrowser zugreifen. Maschinelle Learning Studio hostet eine Palette von Modulen mit eine visuelle Komposition-Benutzeroberfläche, die Sie zum Erstellen einer End-to-End-, Daten Wissenschaft Workflow in Form von einem Versuch ermöglicht.

Weitere Informationen zur maschinellen Learning Studio, finden Sie unter [Neuigkeiten maschinellen Learning Studio?](machine-learning-what-is-ml-studio.md)

**Was ist der Computer Learning-API Service?**

Der Dienst maschinellen Learning-API ermöglicht es Ihnen Vorhersage Modellen bereitstellen wie die integrierte maschinelle Learning Studio als skalierbare, Fehlertoleranz, Webdienste. Die Webdienste vom Computer Learning-API Dienst erstellt werden REST-APIs, die eine Schnittstelle für die Kommunikation zwischen externen Anwendungen und Ihre Vorhersageanalytik Datenmodellen bereitzustellen.

Weitere Informationen finden Sie unter [Verbinden mit einem Computer Learning-Webdienst](machine-learning-connect-to-azure-machine-learning-web-service.md).

**In der werden aufgelistet Meine klassischen Webdienste? In der werden aufgelistet meine neue Azure Ressourcenmanager Grundlage Webdienste?**

Basierend auf Web Services im Portal von [Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net/) aufgelistet werden, klassische Webdienste und neue Azure Ressourcenmanager. 

Klassische Webdienste auch im [Computer Learning Studio](http://studio.azureml.net) auf der Registerkarte des Web Services aufgeführt.

## <a name="microsoft-azure-machine-learning-web-service-questions"></a>Microsoft Azure maschinellen Learning-Webdiensts Fragen

**Was sind Azure maschinellen Learning-Webdiensten?**

Maschinelle Learning-Webdiensten stellen die Schnittstelle zwischen Anwendung und ein Modell wird maschinellen Learning Workflow bewertet. Mithilfe des Azure maschinellen Learning-Webdiensts, können Sie eine externe Anwendung mit einem Computer Learning Workflow Punktzahl Modell in Echtzeit kommunizieren. Ein Computer Learning Web Service-Anruf gibt Vorhersageergebnisse an eine externe Anwendung. Um einen Computer Learning Webdienst anrufen zu machen, übergeben Sie einen API-Schlüssel, der erstellt wurde, wenn Sie den Webdienst bereitgestellt. Der Computer Learning Webdienst basiert auf REST, eine beliebte Architektur der Auswahlmöglichkeiten für Web programming Projekte.

Azure maschinellen Learning weist zwei Arten von Services:

* Anforderung / Antwort-Dienst (RRS) – eine niedrige Wartezeit, hochgradig skalierbare-Dienst, bietet eine Benutzeroberfläche für die statusfreie Modelle erstellt und bereitgestellt aus dem Computer Learning Studio.
* Stapel Ausführung Service (l) – eine asynchrone service die Ergebnisse einer Stapel für Datensätze.

Es gibt mehrere Methoden, um die REST-API nutzen und den Zugriff auf den Webdienst. Beispielsweise können Sie eine Anwendung schreiben, in c#, R oder Python mit dem Beispielcode für Sie generiert wird, wenn Sie den Webdienst bereitgestellt.

Der Code zeigt auf zur Verfügung: Nutzen der Seite für den Webdienst im Portal Azure maschinellen Learning-Webdiensten.
Die API Hilfeseite im Web Service Dashboard in Computer Learning Studio.

Oder Sie können die Stichprobe Microsoft Excel-Arbeitsmappe, die für Sie erstellt (auch im Web Service Dashboard in Studio verfügbar) verwenden.

**Was sind die wichtigsten Updates mit den neuen Azure ML-Webdiensten?**

Weitere Informationen zu den neuen Azure maschinellen Learning-Webdiensten finden Sie in der [zugehörigen Dokumentationsdateien](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>Maschinelle Learning Studio Fragen

### <a name="importing-and-exporting-data-for-machine-learning"></a>Importieren und Exportieren von Daten für maschinelle-Schulung

**Welche Datenquellen unterstützt Computer Schulung?**

Laden von Daten in einem Computer Learning Studio experimentieren in drei verschiedene Arten: Hochladen einer lokalen Datei als Dataset, experimentieren Sie mithilfe eines Moduls zum Importieren von Daten aus der Cloud Datendiensten, oder importieren ein Dataset aus einem anderen gespeichert. Weitere Informationen zum unterstützte Dateiformate finden Sie unter [Importieren von Daten in Computer Learning Studio Schulung](machine-learning-data-science-import-data.md).


#### <a name="a-idmodulelimitahow-large-can-the-data-set-be-for-my-modules"></a><a id="ModuleLimit"></a>Wie große sein Datenmenge kann für meine Module?

Module in Computer Learning Studio unterstützen Datasets von bis zu 10 GB dicht numerischen Daten für allgemeine verwenden. Wenn ein Modul mehr als eine Eingabe akzeptiert, wird 10 GB die Summe aller Eingabewerte Größen. Sie können auch Beispiele für größere Datensets über Struktur noch Azure SQL-Datenbank Abfragen oder von Learning durch zählt vor Aufnahme vorab verarbeiten.  

Die folgenden Arten von Daten können in größeren Datasets während Feature Normalisierung erweitern und kleiner als 10 GB beschränkt sind:

- Gering
- Kategorieliste
- Zeichenfolgen
- Binäre Daten

Die folgenden Bereiche sind auf Datasets kleiner als 10GB beschränkt:

- Recommender Module
- SMOTE-Modul
- Scripting Module: R Python, SQL
- Module, wobei die Größe der Ausgabe Daten größer als Eingabedatengröße, beispielsweise zum Beitreten zu oder Feature Hashing werden kann.
- Übergreifende Überprüfung, optimieren Modell Hyperparameters, englische Regression und eine im Vergleich mit einer alle Multiclass, wenn die Anzahl der Iterationen ist sehr lang.

Für ein Dataset mehr als ein paar GB sollten Sie Daten in Azure-Speicher oder Azure SQL-Datenbank oder verwenden HDInsight, anstatt direkt aus einer lokalen Datei hochladen hochladen.


####<a name="a-iduploadlimitawhat-are-the-limits-for-data-upload"></a><a id="UploadLimit"></a>Was sind die Grenzwerte für die Daten hochladen?
Hochladen von Daten für ein Dataset einige GB maximal zur Azure-Speicher oder Azure SQL-Datenbank oder verwenden HDInsight, anstatt direkt aus einer lokalen Datei hochladen.

**Können Daten aus Amazon S3 werden gelesen?**

Wenn Sie eine kleine Datenmenge haben und sie über eine HTTP-URL verfügbar machen möchten, können Sie die [Daten importieren] [ import-data] Modul. Für eine größeren Datenmengen zuerst auf Azure-Speicher übertragen, und verwenden Sie dann die [Daten importieren] [ import-data] Modul, um es in Ihre experimentieren zu übertragen.
<!--
<SEE CLOUD DS PROCESS>
-->

**Gibt es eine integrierte Bild Eingabewerte Funktion?**

Sie können Informationen zu Bild Eingabewerte Videofunktionen [Bilder importieren] [ image-reader] verweisen.

### <a name="modules"></a>Module

**Der Algorithmus, Datenquelle, Datenformat oder Daten Transformationsvorgang, die ich Suche nicht in Azure maschinellen Learning Studio vorhanden ist. Was sind die Optionen habe?**

Sie können die [Benutzer Feedback Forum](http://go.microsoft.com/fwlink/?LinkId=404231) zum Feature Besprechungsanfragen, die wir nachverfolgt werden, finden Sie unter besuchen. Fügen Sie Ihre Stimme zu einer Besprechungsanfrage, wenn bereits eine Funktion, die gesuchte angefordert wurde. Wenn Sie die Möglichkeit, die gesuchte nicht vorhanden ist, erstellen Sie eine neue Besprechungsanfrage. Sie können auch den Status der Anfrage in diesem Forum anzeigen. Wir diese Liste genau verfolgen und aktualisieren den Status der Verfügbarkeit von Features häufig. Mit den integrierten Unterstützung für R und Python können darüber hinaus benutzerdefinierte Transformationen erstellt werden, je nach Bedarf.


**Können meine vorhandenen Code in Computer Learning Studio werden übertragen?**

Ja, können Sie zeigen Sie den vorhandenen R oder Python Code maschinellen Learning Studio, in dies mit Azure maschinellen Learning Teilnehmern t auszuführen und die Lösung als Webdienst über Azure maschinellen Learning bereitstellen. Weitere Informationen finden Sie unter [Erweitern Ihrer Experimentieren mit R](machine-learning-extend-your-experiment-with-r.md) und [Python ausführen maschinellen learning Skripts in Azure maschinellen Learning Studio](machine-learning-execute-python-scripts.md).

**Ist es möglich, etwa [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) zum Definieren eines Modells verwenden?**

Nein, wird, die nicht unterstützt, jedoch benutzerdefinierter R und Python-Code zum Definieren eines Moduls verwendet werden kann.

**Wie viele Module kann ich parallel in meinem experimentieren ausgeführt?**  

Sie können bis zu vier Module in einem Versuch parallel ausführen.


### <a name="data-processing"></a>Datenverarbeitung

**Gibt es eine Möglichkeit zum Visualisieren von Daten (neben R Visualisierungen) interaktiv innerhalb der experimentieren?**

Durch Klicken auf die Ausgabe eines Moduls, können Sie die Daten visualisieren und Statistik abrufen.

**Vorschau Ergebnisse oder Daten im Browser, ist die Anzahl der Zeilen und Spalten beschränkt, warum?**

Da die Daten an den Browser übertragenen und möglicherweise groß, ist die Datengröße zu verhindern, dass Computer Learning Studio verlangsamt beschränkt. Um alle Daten/Ergebnis zu visualisieren, empfiehlt sich die Daten herunterladen und verwenden Sie Excel oder ein anderes Tool.

### <a name="algorithms"></a>Algorithmen

**Welche vorhandenen Algorithmen maschinellen Learning Studio unterstützt werden?**

Maschinelle Learning Studio bietet Algorithmen der Art, wie zum Beispiel skalierbare verstärkt Entscheidung Bäume, Bayesische Empfehlungen Systeme, Tiefe neuronale Netzwerke und Entscheidung Jungles entwickelt von Microsoft Research. Skalierbare öffnen Source-Computer Learning-Paketen wie Vowpal Wabbit sind ebenfalls enthalten. Maschinelle Learning Studio unterstützt Computer Learning Algorithmen für multiclass und binäre Klassifizierung, Regression und Cluster. Die vollständige Liste der [Computer-Learning-Module]finden Sie unter[machine-learning-modules].

**Schlagen Sie automatisch rechts maschinellen Learning Algorithmus für meine Daten verwendet werden soll?**

Nein, es gibt jedoch verschiedene Arten in Computer Learning Studio vergleichen Sie die Ergebnisse jeder Algorithmus, um die richtige Auswahl für Ihr Problem zu bestimmen.

**Haben Sie alle Richtlinien für einen Algorithmus über ein anderes für die bereitgestellten Algorithmen auswählen?**
Informationen Sie [zum Auswählen eines Algorithmus](machine-learning-algorithm-choice.md).

**Werden die bereitgestellten Algorithmen sind in R oder Python geschrieben?**

Nein, sind diese Algorithmen hauptsächlich in kompilierte Sprachen höhere Leistung geschrieben.

**Sind keine Angaben zu den Algorithmen, die zur Verfügung gestellt?**

Die Dokumentation enthält einige Informationen zu den Algorithmen, und die Parameter optimieren vorgesehenen zum Optimieren des Algorithmus für Ihre Verwendung beschrieben werden.  

**Gibt es keine Unterstützung für online-Schulung?**

Nein, wird das Umschulung nur programmgesteuerten unterstützt.

**Kann ich die Ebenen eines neuronalen Netto Modells mithilfe des integrierten Moduls darstellen?**

Nein.

**Kann ich meinen eigenen Module in c# oder eine andere Sprache erstellen?**

Aktuell neue benutzerdefinierte Module können nur in r erstellt werden

### <a name="r-module"></a>R-Modul

**Welche R-Paketen sind in Computer Learning Studio verfügbar?**

Maschinelle Learning Studio unterstützt 400 + R CRAN Pakete heute, und es folgt eine [Liste der aktuellen](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) alle darin enthaltenen Pakete. Siehe auch [Erweitern Ihrer Experimentieren mit R](machine-learning-extend-your-experiment-with-r.md) erfahren, wie diese Liste selbst abrufen. Wenn das gewünschte Paket nicht in dieser Liste enthalten ist, geben Sie den Namen des [Benutzers Feedback-Forum](http://go.microsoft.com/fwlink/?LinkId=404231)-Pakets.

**Ist es möglich, ein benutzerdefiniertes R Modul erstellen?**

Ja, finden Sie unter [Autor benutzerdefinierte R Module Azure Computer interessante](machine-learning-custom-r-modules.md) , Weitere Informationen zu erhalten.

**Gibt es eine REPL-Umgebung für R?**

Nein, es ist keine REPL Umgebung für R in Studio.

### <a name="python-module"></a>Python-Modul

**Ist es möglich, ein benutzerdefiniertes Python Modul erstellen?**

Zurzeit, nicht jedoch mindestens [Python-Skript ausführen] können[ python] Module das gleiche Ergebnis erhalten.

**Gibt es eine REPL-Umgebung für Python?**

Sie können die Jupyter Notizbücher in Computer Learning Studio verwenden. Weitere Informationen finden Sie unter [Einführung in Jupyter Notizbücher in Azure maschinellen Learning Studio] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Webdienst

###<a name="retraining-models-programmatically"></a>Modelle programmgesteuert Umschulung

**Wie kann ich neu trainieren Azure maschinellen Learning Modelle programmgesteuert?**

Verwenden Sie die Retraining-APIs. Weitere Informationen finden Sie unter [neu trainieren maschinellen Learning Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md). Beispiel-Code steht auch in [Microsoft Azure maschinellen Learning Umschulung Demo](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Erstellen

**Kann ich das Modell lokal oder in einer Anwendung ohne Verbindung zum Internet werden bereitgestellt?**

Nein.


**Gibt es eine geplante Wartezeit, die für alle Webdienste erwartet wird?**

Finden Sie unter der [Azure-Abonnement beschränkt.](../azure-subscription-service-limits.md)

### <a name="use"></a>Verwendung

**Wenn würde ich möchte meine Vorhersage Modell als Dienst Stapel Ausführung im Vergleich zu einer Antwort anfordern Dienst ausgeführt?**

Der Dienst anfordern Antwort (RRS) ist ein Low-Wartezeit, hochgradig skalierbar Webdienst, mit dem eine Schnittstelle zu statusfreie Datenmodellen bereitzustellen, die erstellt und aus der Umgebung experimentieren bereitgestellt werden. Der Dienst Stapel Datenausführungsverhinderung (l) ist ein Dienst zur Bewertung asynchrone einer Reihe von Datensätzen. Die Eingabe für l ist ähnlich wie die Dateneingabe in RRS verwendet werden. Der wichtigste Unterschied ist, dass l: eine Gruppe von Datensätzen aus einer Vielzahl von Quellen wie der Blob-Dienst und Tabelle Dienst in SQL Azure-Datenbank, Azure HDInsight (Stock Abfrage), und HTTP-Quellen lautet. Weitere Informationen finden Sie unter [So maschinellen Learning-Webdiensten nutzen](machine-learning-consume-web-services.md).

**Wie aktualisieren kann ich das Modell für den bereitgestellten Webdienst?**

Aktualisieren einer Vorhersage Modell für eine bereits bereitgestellten Dienst ist so einfach wie das Ändern und erneutes Ausführen des Versuchs, den Sie zum Verfassen und speichern Sie das Modell ausgebildete verwendet. Nachdem Sie eine neue Version des ausgebildeten Modells verfügbar haben, fragt Computer Learning Studio Sie, ob Ihr Webdienst aktualisiert werden sollen. Weitere Informationen zum Aktualisieren eines bereitgestellten Webdiensts, finden Sie unter [Bereitstellen von einem Webdienst maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

Sie können auch die APIs Umschulung verwenden.
Weitere Informationen finden Sie unter [neu trainieren maschinellen Learning Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md). Beispiel-Code steht auch in [Microsoft Azure maschinellen Learning Umschulung Demo](https://azuremlretrain.codeplex.com/).

**Wie werden meine Webdienst in Herstellung bereitgestellt überwacht?**

Nachdem ein Vorhersage Modell bereitgestellt wurde, können Sie es in der Standardansicht Azure überwachen-Portal (nur klassische Web Services) oder des Portals Azure maschinellen Learning-Webdiensten. Jede bereitgestellte Dienst verfügt über eine eigene Dashboard, wo Sie sehen können, für die Überwachung Informationen für diesen Dienst. Weitere Informationen zum Verwalten Ihrer Webdienste bereitgestellten finden Sie unter [Verwalten von einem Webdienst im Portal Azure maschinellen Learning-Webdiensten verwenden](machine-learning-manage-new-webservice.md) und [Verwalten einer Azure maschinellen Learning-Arbeitsbereich](machine-learning-manage-workspace.md).

**Gibt es ein Ort, wo ich die Ausgabe Meine RRS/l sehen können?**

Für RRS wird bei Antwort des Web in der Regel, wo Sie das Ergebnis sehen. Sie können auch auf Azure Blob-Speicher schreiben. Für l wird die Ausgabe in ein Blob standardmäßig geschrieben werden. Sie können auch Schreiben der Ausgabe in einer Datenbank oder Tabelle mit den [Daten exportieren] [ export-data] Modul.

**Kann ich nur von Datenmodellen in Computer Learning Studio erstellten Webdienste erstellen?**

Nein, können Sie auch Webdienste direkt aus Jupyter Notizbüchern und RStudio erstellen.

**Wo finde ich Informationen zu Fehlercodes?**

Eine Liste der Fehlercodes und Beschreibungen finden Sie unter [Computer Learning Modul Fehlercodes](https://msdn.microsoft.com/library/azure/dn905910.aspx) .

## <a name="scalability"></a>Skalierbarkeit

**Was ist der Skalierbarkeit Webdienst?**

Aktuell, wird der Standardendpunkt mit 20 gleichzeitige RRS Anforderungen pro Endpunkt bereitgestellt. Sie können dies 200 gleichzeitige Anforderungen pro Endpunkt skalieren und können Sie jeden Webdienst zu 10.000 Endpunkten pro Webdienst in [einem Webdienst Skalierung](machine-learning-scaling-webservice.md)beschriebenen skalieren. Jeder Endpunkt ermöglicht Verarbeitung von Besprechungsanfragen 40 nacheinander l und zusätzliche Anfragen über 40 Serviceanfragen. Diese Anforderungen in der Warteschlange werden automatisch ausgeführt, als der Warteschlange Kanalisation gelangen lassen.


**Werden R Aufträge sind auf Knoten verteilt?**

Nein.  


**Wie viele Daten kann ich für Schulung verwenden?**

Module in Computer Learning Studio unterstützen Datasets von bis zu 10 GB dicht numerischen Daten für allgemeine verwenden. Wenn ein Modul mehr als eine Eingabe akzeptiert, ist die Gesamtgröße für alle Eingaben zusammen 10 GB. Sie können auch Beispiele für größere Datensets über Struktur noch Azure SQL-Datenbank Abfragen oder von vorab verarbeiten mit [Lernen mit zählt] [ counts] Module vor Aufnahme.  

Die folgenden Arten von Daten können in größeren Datasets während Feature Normalisierung erweitern und kleiner als 10 GB beschränkt sind:

- gering
- Kategorieliste
- Zeichenfolgen
- binäre Daten

Die folgenden Bereiche sind auf Datasets kleiner als 10 GB beschränkt:

- Recommender Module
- SMOTE-Modul
- Scripting Module: R Python, SQL
- Module, wobei die Größe der Ausgabe Daten größer als Eingabedatengröße, beispielsweise zum Beitreten zu oder Feature Hashing werden kann.
- Cross überprüfen, optimieren Modell Hyperparameters, englische Regression und eine im Vergleich mit einer alle Multiclass, wenn die Anzahl der Iterationen ist sehr lang.

Für ein Dataset mehr als ein paar GB sollten Sie Daten in Azure-Speicher oder SQL Azure-Datenbank, oder verwenden Sie HDInsight, anstatt direkt aus einer lokalen Datei hochladen hochladen.


**Gibt es eine Vektor Größe Einschränkungen?**

Zeilen und Spalten sind jeweils auf .NET Einschränkung des Max Int beschränkt: 2.147.483.647.

**Werden die Größe des virtuellen Computers verwendet wird, um den Webdienst ausführen kann angepasst?**

Nein.  

## <a name="security-and-availability"></a>Sicherheit und Verfügbarkeit

**Wer hat Zugriff auf den HTTP-Endpunkt für den Webdienst standardmäßig? Wie einschränken ich an den Endpunkt Zugriff?**

Nach ein Webdienst bereitgestellt wird, wird ein Standardendpunkt für diesen Dienst erstellt. Der Standardendpunkt kann mithilfe des Schlüssels API aufgerufen werden. Zusätzliche Endpunkte können aus dem klassischen Azure-Portal oder programmgesteuert mithilfe der Web Service Management-APIs mit eigene Schlüssel hinzugefügt werden. Zugriffstasten sind erforderlich, um den Webdienst anzurufen. Weitere Informationen finden Sie unter [Verbinden mit einem Computer Learning-Webdienst](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Was passiert, wenn mein Konto Azure-Speicher nicht gefunden werden kann?**

Maschinelle Learning Studio basiert auf einer bereitgestellten Azure-Speicher Benutzerkonto temporären Daten gespeichert werden, wenn den Workflow ausgeführt. Dieses Speicherkonto erfolgt zu Computer Learning Studio gleichzeitig ein Arbeitsbereich erstellt wurde. Nachdem der Arbeitsbereich, erstellt wurde Wenn das Speicherkonto wird gelöscht und kann nicht mehr gefunden werden, wird der Arbeitsbereich nicht mehr funktioniert und sämtliche Versuche in diesem Arbeitsbereich schlägt fehl.

Wenn Sie das Speicherkonto versehentlich gelöscht haben, erstellen Sie das Speicherkonto mit demselben Namen in der gleichen Region als das Speicherkonto gelöschte neu. Synchronisieren Sie anschließend die Zugriffstaste neu.


**Was passiert, wenn mein Speicher Konto Zugriffstaste synchron sind?**

Maschinelle Learning Studio basiert auf einer bereitgestellten Azure-Speicher Benutzerkonto temporären Daten gespeichert werden soll, wenn den Workflow ausgeführt. Dieses Speicherkonto ist auf Computer Learning Studio gleichzeitig ein Arbeitsbereich erstellt wird und Zugriffstasten diesem Arbeitsbereich zugeordnet sind. Wenn der Tastenkombinationen, geändert werden nachdem der Arbeitsbereich erstellt wurde, kann den Arbeitsbereich nicht mehr Speicher-Konto zugreifen. Es wird nicht mehr funktioniert und sämtliche Versuche in diesem Arbeitsbereich schlägt fehl.

Wenn Sie Speicherplatz Konto Zugriffstasten geändert haben, synchronisieren Sie die Tastenkombinationen im Arbeitsbereich über das klassische Azure-Portal neu.  


## <a name="azure-marketplace"></a>Azure Marketplace

Finden Sie im [häufig gestellte Fragen zum Veröffentlichen und die Verwendung von apps auf dem Computer Learning Marketplace](machine-learning-marketplace-faq.md)aus.

## <a name="support-and-training"></a>Support und Schulung

**Wo finde ich Schulungen für Azure maschinellen Schulung?**

[Azure Arbeitsplatzes Learning Dokumentation](https://azure.microsoft.com/services/machine-learning/) hostet video Lernprogramme und unterstützenden Führungslinien. Hier erläuterten bieten eine Einführung in die Dienste und Wissenschaft Lebenszyklus Daten importieren von Daten, Bereinigen von Daten, Erstellen von Vorhersagen Modellen und deren Herstellung mit Azure maschinellen Learning Bereitstellung durchgehen.

Learning Arbeitsplatzes kontinuierlichen Produktlizenzierung, wir neues Material hinzufügen. Sie können die Anforderung von zusätzlichen Lern-und Material Learning Arbeitsplatzes am [Benutzer Feedback Forum](https://windowsazure.uservoice.com/forums/257792-machine-learning)senden.

Sie können auch Schulung bei [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning)suchen.

**Wie erhalte ich Unterstützung für Azure maschinellen Schulung?**

Um den technischen Support für Azure maschinellen Learning erhalten möchten, wechseln Sie zur [Unterstützung von Azure](/support/options/) , und wählen Sie **Computer Schulung**.

Azure maschinellen Schulung, weist ebenfalls einer Community-Forum im MSDN, in dem Sie Fragen im Zusammenhang mit Azure maschinellen Learning bitten können. Das Forum wird durch das Team Azure maschinellen Learning überwacht. Besuchen Sie [Azure-Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Fragen zur Abrechnung

**Wie funktioniert der Computer Learning Abrechnung?**

Es gibt zwei Komponenten zum Dienst Azure maschinellen Learning aus. Die Learning Studio Computer und den Computer Learning-Webdiensten.

Während Sie den Computer Learning Studio bewerten möchten, können Sie die kostenlose Abrechnung Ebene.  Die kostenlose Ebene können auch einen klassischen Webdienst mit eingeschränkter Kapazität bereitstellen.

Nachdem Sie entschieden haben, dass Azure maschinellen Learning Ihren Anforderungen entspricht, können Sie für die standardmäßige Leiste registrieren. Um sich zu registrieren, müssen Sie ein Microsoft Azure-Abonnement verfügen.

Sie sind in der Standardansicht Ebene Abrechnung eine monatliche Gebühr für jeden Arbeitsbereich, die Sie in den Computer Learning Studio definieren. Wenn Sie einem Versuch in Studio ausführen, werden Sie beim Ausführen einer experimentieren für Ressourcen berechnen Abrechnung. Beim Bereitstellen eines klassischen Webdiensts Transaktionen und zu berechnen, dass die Stunden auf Basis Quellenbesteuerung (PAYG) berechnet werden.

Stellen Sie den neuen Computer Learning-Webdiensten vor Abrechnung Pläne, die bessere Voraussagbarkeit im Kosten zulässig ist. Gestufte Preise bietet unverzinslichen Sätzen für Kunden, die eine große Datenmenge Kapazität benötigen.

Wenn Sie einen Plan erstellen, werden Sie in Fixkosten, die mit einer darin enthaltenen Menge von API berechnen Stunden und API Transaktionen stammen übernehmen. Wenn Sie mehr enthalten Mengen benötigen, können Sie zusätzliche Instanzen zu Ihrem Plan hinzufügen. Wenn Sie viel mehr enthalten Mengen benötigen, können Sie einen höheren Ebene Plan auswählen, der erheblich bereitstellt, dass weitere Mengen und einen besser unverzinslichen Satz eingefügt.

Nachdem die darin enthaltenen Mengen vorhandenen Instanzen von verwendet werden, werden zusätzliche Verwendung mit einer veralteten Geschwindigkeit der Abrechnung Plan Ebene zugeordnet in Rechnung gestellt.

Hinweis: Mengen einbezogen werden alle 30 Tage zugewiesen und nicht verwendete enthalten Mengen nicht in die nächste Periode verschoben werden.

Zusätzliche Abrechnung und Preisinformationen finden Sie unter [Computer Learning Preise](https://azure.microsoft.com/pricing/details/machine-learning/).

**Verfügt maschinellen Learning kostenlose Testversion?**

 Azure maschinellen Learning verfügt über die Option kostenloses Abonnement (Details finden Sie unter [Computer Learning Preise](https://azure.microsoft.com/pricing/details/machine-learning/) ), und Computer Learning Studio hat eine schnelle Auswertung von 8 Stunden Testversion verfügbar (Melden Sie sich bei [Maschinellen Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2) für diese Testversion).

 Wenn Sie sich für eine Azure kostenlose Testversion registrieren, können Sie alle Azure Dienste für einen Monat versuchen. Weitere Informationen zu den Azure kostenlose Testversion finden Sie auf [Azure kostenlose Testversion häufig gestellte Fragen](/pricing/free-trial-faq/).

**Was ist eine Transaktion?**

Eine Transaktion stellt einen API Anruf, dem auf Azure maschinellen Learning reagiert. Transaktionen aus Anforderung / Antwort-Dienst (RRS) und Stapel Ausführung Service (l) Anrufe aggregiert und mit dem Plan Abrechnung belastet.

**Kann ich die darin enthaltenen Transaktion Mengen in einen Plan für sowohl RRS und l Transaktionen verwenden?**

Ja, Ihre Transaktionen auf Ihrem RRS und l aggregiert und mit dem Plan Abrechnung belastet.

**Was ist eine API berechnen Stunde?**

Eine API berechnen Stunde ist, dass die Abrechnung Einheit für die Uhrzeit API-Aufrufe ausführen, verwenden die ML berechnen Ressourcen nutzen. Alle Ihre Anrufe sind für Zwecke Abrechnung aggregiert.

**Wie lange dauert ein typische Herstellung API Anruf?**

Zeiten der Herstellung API Anrufe können variieren erheblich, im Allgemeinen im Bereich von Hundert Millisekunden bis einige Sekunden dauern, aber erfordern möglicherweise Minuten abhängig von der Komplexität der Verarbeitung von Daten und Modell für maschinelle lernen. Die beste Methode zum schätzen Zeiten der Herstellung API Anrufe ist zu analysieren ein Modells maschinellen Learning-Dienst.

**Was ist eine Stunde Studio berechnen?**

Eine Stunde Studio berechnet ist, dass die Abrechnung Maßeinheit zum Aggregieren Mal Ihre Versuche verwendeten Ressourcen Studio berechnen.

**Was ist für die Ebene Test-/vorgesehen, in die neue Webdienste?**

Neuer Webdienste Azure ML bieten mehrere Ebenen, die Sie zum Bereitstellen des Projektplans verwenden können. Die Test-/Ebene ist eine Ebene, die begrenzte enthalten Mengen bereitstellt, mit die Sie Ihre experimentieren als neue Webdienst ohne dort Kosten testen können. Sie haben die Möglichkeit, "Kick der Reifen" zu sehen, wie es funktioniert.

**Gibt es separate Speicher Gebühren?**

Die Computer Learning kostenlosen Ebene ist nicht erforderlich oder separaten Speicherung zulässig. Die Computer Learning Standard Ebene muss Benutzer über ein Konto Azure-Speicher verfügen. Azure-Speicher ist [separat in Rechnung gestellt](https://azure.microsoft.com/pricing/details/storage/).

**Wie funktioniert der Computer Learning Support hoher Verfügbarkeit?**

Zeiten der Herstellung API Anrufe können variieren erheblich, im Allgemeinen im Bereich von Hundert Millisekunden bis einige Sekunden dauern, aber erfordern möglicherweise Minuten abhängig von der Komplexität der Verarbeitung von Daten und Modell für maschinelle lernen. Die beste Methode zum schätzen Zeiten der Herstellung API Anrufe ist zu analysieren ein Modells maschinellen Learning-Dienst.

**Welche bestimmte Arten von Ressourcen berechnen können meine Anrufe Herstellung API auf werden ausgeführt?**

Der Computer Learning Service ist ein mandantenfähigen und Berechnen von ist-Ressourcen verwendet wird, klicken Sie auf die Back-End-variieren und sind für die Leistung und Vorhersagbarkeit optimiert.

### <a name="management-of-new-web-services"></a>Verwaltung von neuen Webdienste

**Was passiert, wenn ich meinen Plan löschen?**

Der Plan aus Ihrem Abonnement entfernt, und Sie für eine anteilige Verwendung in Rechnung gestellt werden.

Hinweis: Einen Plan, der von einem Webdienst verwendet wird, kann nicht gelöscht werden. Um den Plan löschen möchten, müssen Sie einen neuen Plan Webdienst zuweisen oder den Webdienst löschen.

**Was ist eine Instanz planen?**

Eine Plan-Instanz ist eine Einheit enthalten Mengen, die Sie zu Ihrem Abrechnung Plan hinzufügen können. Wenn Sie eine Abrechnung Ebene für Ihr Abrechnung Plan auswählen, enthält sie nur eine Instanz. Wenn Sie mehr enthalten Mengen benötigen, können Sie zu Ihrem Plan Instanzen der ausgewählten Abrechnung Stufe hinzufügen.

**Wie viele Plan Instanzen können ich hinzufügen?**

Sie können eine Instanz der Test-/Stufe ein Abonnement haben.

Ebenen S1, S2 und S3 können Sie beliebig viele Systeme hinzufügen.

Hinweis: Je nach der erwarteten Verwendung, ist es wahrscheinlich kostengünstiger zum upgrade auf eine höhere enthalten Mengen Stufe, anstatt Instanzen hinzufügen, um die aktuelle Ebene.

**Was passiert bei einer Änderung Plan Ebenen (Upgrade / downgrade)?**

Der alte Plan wird gelöscht und die aktuelle Verwendung auf Basis anteilige in Rechnung gestellt wird. Ein neuer Plan mit den vollständigen darin enthaltenen Mengen der Stufe Upgrade/Downgrade wird für die restlichen Periode erstellt.

Hinweis: Werden darin enthaltenen Mengen pro Periode zugewiesen und nicht verwendete Mengen nicht verschoben werden.

**Was geschieht, wenn ich die Instanzen in einen Plan zu erhöhen?**

Mengen pro anteilige fallen und dauert 24 Stunden wirksam werden.

**Was geschieht beim Löschen einer Instanz von einem Plan?**

Die Instanz aus Ihrem Abonnement entfernt, und Sie für eine anteilige Verwendung in Rechnung gestellt werden.


### <a name="signing-up-for-new-web-services-plans"></a>Anmelden für neue Webdienste-Pläne

**Wie melde ich mich für einen Plan?**

Sie haben zwei Methoden zum Erstellen von Abrechnung Pläne.

Wenn Sie zuerst einen neuen Webdienst bereitstellen, können Sie einen vorhandenen Plan auswählen oder erstellen ein neues Plans.

Pläne, die auf diese Weise erstellt werden, in Ihrem standardmäßigen Region und Ihren Webdienst in diesem Bereich bereitgestellt.

Wenn Sie als Ihr Standard-Bereich Regionen Services bereitstellen möchten, sollten Ihre Abrechnung Pläne zu definieren, bevor Sie mit den Dienst bereitstellen,

Sie können in diesem Fall melden Sie sich bei im Portal Azure maschinellen Learning-Webdiensten und navigieren Sie zu der Seite Pläne. Von dort können Sie hinzufügen und löschen Pläne sowie vorhandene Pläne ändern.

**Welchen Plan sollte ich auswählen, um mit zu starten, deaktivieren?**

Wir empfehlen Ihnen, die Sie mit der standardmäßigen S1 beginnen Stufen des Diensts für die Verwendung überwachen. Wenn Sie, dass Sie Ihre darin enthaltenen Mengen schnell verwenden feststellen, können Sie Instanzen hinzufügen oder verschieben in eine höhere Stufe und besser unverzinslichen Sätzen abrufen. Sie können Ihr Plan Abrechnung anpassen, Bedarf in der gesamten Ihrer Abrechnungszyklus.

**Welche Bereiche sind die neue Pläne in verfügbar?**

Die neue Abrechnung Pläne sind in den drei Herstellung Regionen, in denen das neue Web Services unterstützt, wir sind, verfügbar:

* Süd zentralen US
* Westen Europa
* Süd Ostasien

**Ich habe Webdienste in mehreren Bereichen an. Benötige ich einen Plan für jede Region?**

Ja. Plan Preise je nach Region unterschiedlich. Wenn Sie einen Webdienst in eine andere Region bereitstellen, müssen Sie die Region einen bestimmten Plan zuzuweisen.

### <a name="new-web-services---overages"></a>Neue Webdienste - Überschüsse angezeigt

**Wie kann ich meine Webverwendung von Diensten in Überschuss ist Einchecken?**

Sie können die Verwendung auf alle Ihre Pläne, die auf der Seite Pläne im Portal Azure maschinellen Learning-Webdiensten anzeigen. Melden Sie sich mit dem Portal, und klicken Sie auf die Menüoption Pläne.

In den Transaktionen und Berechnen von Spalten der Tabelle, sehen Sie die darin enthaltenen Mengen an den Plan und den Prozentsatz verwendet.

**Was passiert, wenn ich nach oben in der Ebene Test-/Mengen einschließen verwende?**

Dienste, die eine Test-/Stufe zugewiesen haben, bis die nächste Periode beendet werden, oder Sie auf einen der kostenpflichtigen leisten verschieben.

**Bei klassischen Webdienste und der neuen Webdienste Überschüsse angezeigt werden wie Preise für anfordern Antwort (RRS) und Stapel (l) Auslastung berechnet?**

Für eine Arbeitsbelastung RRS unterliegen Sie für jede Transaktion API-Anruf, die Sie vornehmen und zum Berechnen Mal diese Anfragen zugeordnet. Stellen Sie Ihre RRS Herstellung API Transaktion, die Kosten berechnet werden, wie die Gesamtzahl der API, die Sie Anrufe durch den Preis pro 1.000 Transaktionen (anteilig durch einzelne Transaktion) multipliziert. Die Zeitdauer für jede API Anruf anzuwendende erforderlich multipliziert mit der Gesamtzahl der API Transaktionen multipliziert mit dem Einzelpreis Herstellung API berechnen Stunde werden Ihre RRS-API Herstellung API berechnen Stunde Kosten berechnet.

Beispielsweise für Standard S1 Überschuss, 1.000.000 API Transaktionen, die 0,72 Sekunden ausführen Ausführen ergibt (1.000.000 *$0,50 / 1K API Transaktionen) in 500 Herstellung API Transaktion Kosten und (1.000.000* 0,72 sec * $2 / h) $400 in Herstellung API berechnen Stunden für insgesamt $ 900 ein.

Für eine Arbeitsbelastung l Sie auf die gleiche Weise unterliegen, jedoch die API Transaktionskosten entsprechen der Anzahl der Stapelverarbeitung abgeschlossen, die Sie senden, und die Kosten berechnen dieser Stapel Einzelvorgänge zugeordnete berechnen Zeit darstellen. Die Gesamtzahl der Aufträge übermittelt mit den Kurs pro 1.000 Transaktionen (anteilig durch einzelne Transaktion) multipliziert werden Ihre l Herstellung API Transaktion Kosten berechnet. Die Zeitdauer für jede Zeile in Ihrem Auftrag multipliziert die Ausführung erforderliche durch die Gesamtzahl der Zeilen in Ihrem Auftrag multipliziert mit der Gesamtzahl der Einzelvorgänge multipliziert mit dem Einzelpreis Herstellung API berechnen Stunde werden Ihre l API Herstellung API berechnen Stunde Kosten berechnet. Wenn Sie den Computer Learning Rechner verwenden zu können, der Transaktion Meter die Anzahl der Einzelvorgänge, die Sie senden möchten und die Uhrzeit pro Feld Transaktion darstellt zusammengefassten Zeit, die für alle Zeilen in jeder Auftrag zum Ausführen erforderlich sind.

Beispielsweise mit Standard S1 Überschuss, wenn Sie 100 Stellen pro Tag, dass bestehen aus 500, die Zeilen jeder 0,72 Sekunden dauern, bis, und klicken Sie dann die monatlichen veralteten Kosten wäre übermitteln (100 Stellen pro Tag = 3,100 Aufträge/Mo *$0,50 / 1K API Transaktionen) $1.55 in Herstellung API Transaktion Kosten und (500 Zeilen* 0,72 sec *3,100 Aufträge* $2/h) $620 Herstellung API berechnen Stunden , für insgesamt $621.55.

### <a name="azure-ml-classic-web-services"></a>Azure ML klassischen Webdienste

**Ist der Quellenbesteuerung jedoch verfügbar?**
Ja, sind klassische Webdienste Azure Computer interessante jedoch verfügbar.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Lernen kostenlose und den Standardversionen Ebene Azure-Computern

**Was ist in der Azure maschinellen Learning kostenlosen Ebene enthalten?**

Die Azure maschinellen Learning kostenlosen Ebene soll eine umfassende Einführung in die Azure maschinellen Learning Studio bereitstellen. Sie müssen lediglich ein Microsoft-Konto, um sich anzumelden. Die kostenlose Stufe umfasst kostenlosen Zugriff auf eine Azure maschinellen Learning Studio-Arbeitsbereich pro [Microsoft-Konto](https://www.microsoft.com/account/default.aspx)an. Sie enthält die Möglichkeit zur Verwendung von bis zu 10 GB Speicher und die Möglichkeit, Modelle als staging-APIs Prozessen umsetzen. Kostenlose Ebene Auslastung fallen nicht unter einer Vereinbarung zum SERVICELEVEL und für die Entwicklung und nur die persönliche Verwendung vorgesehen sind. Kostenfreie Ebene Auslastung übersteigen€™ t Zugriff auf Daten durch Herstellen einer Verbindung mit einer lokalen SqlServer.

**Was ist in der Azure maschinellen Learning Standard Ebene und Pläne enthalten?**

Die Azure maschinellen Learning Standard Ebene ist ein kostenpflichtiges Herstellung Version Azure maschinellen Learning Studio. Die monatliche Gebühr für ML Azure Service Studio auf in Rechnung gestellt wird eine pro Arbeitsbereich pro Monat Basis und anteilige teilweise Monate. Azure ML Studio experimentieren-Stunden werden pro Stunde berechnen, für die aktive experimentieren berechnet. Abrechnung ist für teilweise Stunden anteilig.  

Der Dienst Azure ML-API ist in Rechnung gestellt je nachdem, ob es sich um eine klassische Webdienste oder einen neuen Webdienst handelt.

Die folgenden Gebühren sind pro Arbeitsbereich für Ihr Abonnement aggregiert.

* Maschinelle Learning Arbeitsbereich-Abonnement - ist der Computer Learning Arbeitsbereich-Abonnement eine monatliche Gebühr, die bietet Zugriff auf ein Arbeitsbereich ML Studio und Versuche, die sowohl in der Studio und Nutzung der Herstellung APIs ausführen erforderlich ist.
* Studio experimentieren Stunden – diese Meter Aggregate alle berechnen Gebühren aufgelaufene durch Versuche in ML Studio ausgeführt und Herstellung API Anrufe in das staging-Umgebung ausgeführt.
* Access-Daten durch Herstellen einer Verbindung mit einer lokalen SQLServer in Ihrer Modelle für Ihre Schulung und bewerten.
* Für klassische Webdienste:
    * Herstellung API berechnen Stunden – diese Meter berechnen Gebühren nach der Herstellung ausgeführten Webdiensten aufgelaufene enthält.
    * Herstellung API Transaktionen (in 1000 s) – enthält diese Anzeige pro Anruf an Ihr Webdienst Herstellung aufgelaufene Gebühren.

Abgesehen von der vorhergehenden Gebühren, wenn neue Web Services sind Gebühren auf den ausgewählten Plan zusammengefasster:

* Standard S1/S2/S3-API planen (Einheiten) – diese Meter stellt den Typ des für neue Webdienste ausgewählte Instanz
* Standard-API S1/S2/S3 von veralteten berechnen Stunden – diese Meter umfasst berechnen Gebühren aufgelaufene durch den neuen-Webdiensten Herstellung ausgeführt werden, nachdem die darin enthaltenen Mengen in vorhandene Instanzen von verwendet werden. Die weitere Verwendung werden in Rechnung gestellt, bei der overate Zins S1/S2/S3 Plan Ebene zugeordnet.
* Standard S1/S2/S3 Überschuss API Transaktionen (in 1.000) – enthält diese Anzeige Gebühren pro Anruf an Ihre Herstellung neuen Webdienst aufgelaufene, nachdem die darin enthaltenen Mengen in vorhandene Instanzen von verwendet werden. Die weitere Verwendung werden in Rechnung gestellt, bei der overate Zins S1/S2/S3 Plan Ebene zugeordnet.
* Darin enthaltenen Menge API berechnen darstellt Stunden – mit der neuen Webdienste Anzeige die darin enthaltenen Anzahl der API berechnen Stunden
* Darin enthaltenen Menge API Transaktionen (in 1.000) – mit den neuen-Webdiensten Anzeige darstellt, die darin enthaltenen Menge der API Transaktionen


**Wie melde ich mich für Azure ML kostenlosen Leiste?**

Sie müssen lediglich ein Microsoft-Konto. Wechseln Sie zur [Azure maschinellen Learning-Startseite](https://azure.microsoft.com/services/machine-learning/)aus, und klicken Sie auf **Jetzt beginnen**. Melden Sie sich mit Ihrem Microsoft-Konto und einen Arbeitsbereich in Free Ebene für Sie erstellt wird. Sie können beginnen, zu durchsuchen und Computer Learning Versuche sofort zu erstellen.

**Wie melde ich mich für Azure ML Standard-Leiste?**

Zugriff auf ein Azure-Abonnement müssen zuerst einen Standard ML Arbeitsbereich erstellen. Sie können melden Sie sich bei einem 30 Tage kostenlose Testversion Azure Abonnement und höher Aktualisierung in ein kostenpflichtiges Abonnement für Azure oder ein kostenpflichtiges sofortiges Azure-Abonnement kaufen. Sie können einen Arbeitsbereich maschinellen Learning dann nach den Zugriff auf das Abonnement vom klassischen Microsoft Azure-Portal erstellen. Anzeigen der [eine schrittweise Anleitung](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Alternativ können Sie durch einen Besitzer des Arbeitsbereichs Standard ML eingeladen, an der Besitzer des Arbeitsbereichs zugreifen.

**Kann ich meinen eigenen Azure Blob-Speicher-Konto zur Verwendung mit der kostenlosen Ebene angeben?**

Nein, ist die standardmäßige Ebene entspricht der Version des Diensts maschinellen Schulung, die vor die Ebenen vorübergehende verfügbar war.

**Kann meinem Computer learning Modelle als APIs in der kostenlosen Ebene werden bereitgestellt?**

Ja, können Sie Computer Learning-Modelle auf das staging API Services als Teil der kostenlosen Ebene Prozessen umsetzen. Um den staging API-Dienst in Betrieb genommen und einen Endpunkt Herstellung für den Dienst operationalized erhalten, müssen Sie die standardmäßige Ebene verwenden.

**Was ist der Unterschied zwischen Azure kostenlose Testversion und Azure maschinellen Learning kostenlosen Ebene?**

Die [kostenlose Testversion von Microsoft Azure](https://azure.microsoft.com/free/) bietet Gutschriften, die auf einer beliebigen Azure Service für einen Monat angewendet werden können. Die Azure maschinellen Learning kostenlosen Ebene bietet kontinuierlichen Zugriff auf Azure maschinellen Learning Dienst nicht Produktionsarbeitslasten speziell.

**Wie verschiebe ich eine experimentieren aus der Free Ebene in der Standardansicht Ebene?**

Um Ihre Versuche das kostenlose kopieren Ebene in der Standardansicht Ebene:

1.  Melden Sie sich bei Azure maschinellen Learning Studio, und stellen Sie sicher, dass Sie sowohl die kostenlose Arbeitsbereich und der Standard-Arbeitsbereich in der Ansichtsauswahl Arbeitsbereich in der oberen Navigationsleiste sehen können.
2.  Wechseln Sie zur kostenlosen Arbeitsbereich, wenn Sie in der Standard-Arbeitsbereich sind.
3.  Wählen Sie in der Listenansicht experimentieren einem Versuch aus, die Sie kopieren, und klicken Sie auf die Schaltfläche kopieren möchten.
4.  Wählen Sie aus dem Popupmenü Dialogfeld des Standard-Arbeitsbereichs aus, und klicken Sie auf die Schaltfläche kopieren.
    Alle zugeordneten Datasets, ausgebildeten Modell, usw. werden zusammen mit den Versuch in den Standard-Arbeitsbereich kopiert.
6.  Sie müssen den Versuch erneut ausführen, und erneut veröffentlichen Ihren Webdienst im Standard-Arbeitsbereich.

### <a name="studio-workspace"></a>Studio-Arbeitsbereich

**Werden andere Rechnung für unterschiedliche Arbeitsbereiche finden Sie?**

Arbeitsbereich Gebühren werden separat für jede anwendbare Meter auf einer einzelnen Rechnung fehlerhaft.

**Welche bestimmte Arten von Ressourcen berechnen können meine Versuche auf werden ausgeführt?**

Der Computer Learning Service ist ein mandantenfähigen und Berechnen von ist-Ressourcen verwendet wird, klicken Sie auf die Back-End-variieren und sind für die Leistung und Vorhersagbarkeit optimiert.

### <a name="guest-access"></a>Gast access

**Was ist Gastzugriff auf Azure maschinellen Learning Studio?**

Gast Access ist einer eingeschränkten zum Testen zur Verfügung steht, die es ermöglicht zu erstellen und Ausführen von Versuche in Azure maschinellen Learning Studio kostenlos und ohne Authentifizierung. Gast Sitzungen sind nicht beständige (können nicht gespeichert werden kann) und auf 8 Stunden begrenzt. Andere Einschränkungen sind fehlender R und Python Support fehlende staging-APIs und eingeschränkten Dataset Größe und Speicherkapazität. Benutzer, die sich mit einem Microsoft-Konto anmelden auswählen haben im Vergleich Vollzugriff auf die Free Ebene von maschinellen Learning Studio beschrieben, über die ein beständiger Arbeitsbereich und umfassendere Funktionen enthält. Wählen Sie Ihre kostenlosen maschinellen Learning Erfahrung, indem Sie **Erste Schritte** auf [https://studio.azureml.net](https://studio.azureml.net)und Auswählen von Access Raten oder melden Sie sich mit einem Microsoft-Konto an.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx

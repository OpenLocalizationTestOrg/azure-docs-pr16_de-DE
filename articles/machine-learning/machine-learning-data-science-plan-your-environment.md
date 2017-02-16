<properties
    pageTitle="So Szenarien identifizieren und Planen für erweiterte Verarbeitung von Analysen Daten | Microsoft Azure"
    description="Plan für die erweiterte Analyse durch eine Reihe von Key Fragen nachdenken."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>So erkennen Sie Szenarien und Planen für erweiterte Analytics Datenverarbeitung

Welche Ressourcen sollten Sie planen, aufnehmen möchten, beim Einrichten einer Umgebung erweiterte Analytics processing auf ein Dataset tun? In diesem Artikel wird eine Reihe von Fragen an, die die Vorgänge und Ressourcen relevant Erkennen von Ihrem Szenario vorgeschlagen. Die Reihenfolge der allgemeinen Schritte Vorhersageanalytik ist umrandet [Was das Team Daten Wissenschaft Prozess (TDSP) ist?](data-science-process-overview.md). Der einzelnen Schritte erfordert bestimmte Ressourcen für die Aufgaben, die für das spezifische Szenario relevanten. Die wichtigsten Fragen zum Identifizieren von Ihrem Szenarios betreffen Daten Logistik, Merkmale, die Qualität der Datasets und Tools und Sprachen, die Sie es vorziehen, führen Sie die Analyse.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistic Fragen: von Standorten und Bewegung
Die logistischen Fragen betreffen des Orts für die **Datenquelle**, die das **Ziel adressieren** in Azure und Anforderungen zum Verschieben von Daten aus, einschließlich der Terminplan, Betrag und Ressourcen, die zur Verwaltung. Möglicherweise müssen die Daten mehrmals während des Prozesses Analytics verschoben werden. Häufig wird lokale Daten in eine Form von Speicher in Azure und dann in der Computer Learning Studio zu verschieben.

1. **Was ist der Datenquelle?** Handelt es sich um lokale oder in der Cloud? Beispiel:
    - Die Daten sind eine HTTP-Adresse öffentlich verfügbar.
    - Die Daten befinden sich in einer lokalen Datei/Netzwerkspeicherort.
    - Die Daten sind in einer SQL Server-Datenbank.
    - Die Daten werden in einem Container Azure-Speicher gespeichert.

2. **Was ist das Ziel des Azure?** Benötigt es, in dem für die Verarbeitung oder modeling werden? Beispiel:
    - Azure Blob-Speicher
    - SQL Azure-Datenbanken
    - SQLServer Azure-virtuellen Computers
    - HDInsight (Hadoop auf Azure) oder Struktur Tabellen
    - Learning Azure-Computern
    - Bereitgestellt werden können Azure virtuelle Festplatten.

3. **Wie möchten Sie die Daten zu verschieben?** Die Verfahren und Ressourcen, die Aufnahme oder Laden von Daten in einer Vielzahl von anderen Speicherplatz und Verarbeitung Umgebungen werden in den folgenden Themen beschrieben.

    -  [Laden von Daten in Speicher-Umgebungen für analytics](machine-learning-data-science-ingest-data.md)
    -  [Importieren Ihrer Daten Schulung in Azure maschinellen Learning Studio aus unterschiedlichen Datenquellen](machine-learning-data-science-import-data.md).

4. **Benötigt die Daten in regelmäßigen Abständen verschoben oder während der Migration geändert werden?** Erwägen Sie Azure Daten Factory (ADF), wenn Daten müssen ständig migriert werden, insbesondere dann, wenn ein Hybrid-Szenario, die beide lokal greift und Cloudressourcen betroffen ist oder wenn die Daten Transaktionen oder müssen geändert werden oder Geschäftslogik im Verlauf migrierende hinzugefügt haben. Weitere Informationen finden Sie unter [Verschieben von Daten aus einer mit lokalen SQLServer in SQL Azure mit Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)

5. **Wie viele Daten ist in Azure verschoben werden?** Datasets, die sehr groß sind möglicherweise die Speicherkapazität von bestimmter Umgebungen überschreiten. Ein Beispiel finden Sie die Diskussion der Größengrenzwerte für maschinelle Learning Studio im nächsten Abschnitt. In diesem Fall kann ein Beispiel für die Daten während der Analyse verwendet werden. Details zum unten Stichproben ein Dataset in verschiedenen Umgebungen Azure, finden Sie unter [Beispieldaten im Team Daten Wissenschaft Prozess](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Daten Merkmale Fragen: Typ, Format und die Größe
Diese Fragen werden, um zu Ihrer Speicher Planung und Verarbeitung Umgebungen, von denen jede eignen sich auf verschiedene Arten von Daten und von denen jede bestimmte Einschränkungen haben.

1. **Was sind die Datentypen?** Beispiel:
    - Numerische
    - Kategorieliste
    - Zeichenfolgen
    - Binäre

2. **Wie werden die Daten formatiert?** Beispiel:
    - Durch Trennzeichen getrennt (CSV) oder Tabstopps getrennten (TSV) flachen Dateien
    - Komprimiert oder nicht komprimiert
    - Azure blobs
    - Hadoop Struktur Tabellen
    - SQL Server-Tabellen

2. **Wie groß ist Ihre Daten?**
    - Klein: weniger als 2GB
    - Mittel: Größer als 2GB und kleiner als 10GB
    - Groß: Größer als 10GB

Nehmen Sie zum Beispiel der Azure maschinellen Learning Studio-Umgebung:

- Eine Liste der von Datenformaten und Typen von Azure maschinellen Learning Studio finden Sie unter [unterstützte Formate und der Datentypen](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) Abschnitt unterstützt.
- Klicken Sie auf Daten Einschränkungen für Azure maschinellen Learning Studio Informationen finden Sie unter der **wie große Datenmenge kann für meine Module?** Abschnitt [Importieren und Exportieren von Daten für maschinelle Schulung](machine-learning-faq.md#machine-learning-studio-questions)

Informationen über die Einschränkungen für andere Azure Dienste im Analytics Prozess verwendet finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Daten Qualität Fragen: Untersuchung und vorab verarbeiten

1. **Was wissen Sie über Ihre Daten?** Durchsuchen von Daten, wenn Sie ein Grundlegendes zu die grundlegenden Eigenschaften erhalten müssen. Was Muster oder trends es Anlagen Neuigkeiten Ausreißern hat oder wie viele Werte fehlen. Dieser Schritt ist wichtig, um den Umfang der vorab verarbeiten benötigt, für die Formulierung Hypothesen, die am besten geeigneten Features vorschlagen konnte, oder geben Sie der Analyse und für die Formulierung Pläne für zusätzliche Datensammlung festlegen. Populationskenngrößen Berechnung und Darstellung von Visualisierungen sind hilfreiche Techniken für die Überprüfung von Daten aus. Details zum Untersuchen Sie ein Dataset in verschiedenen Azure-Umgebungen finden Sie unter [Durchsuchen von Daten im Team Daten Wissenschaft Prozess](machine-learning-data-science-explore-data.md).

2. **Erfordert die Daten vorab Verarbeitung oder bereinigen?**
Vorbereitung und Bereinigen von Daten sind wichtige Aufgaben, die in der Regel durchgeführt werden müssen, bevor Dataset für Computer Learning effektiv verwendet werden kann. Unformatierte Daten ist häufig laut und unzuverlässigen und Werte fehlt möglicherweise. Verwenden diese Daten für die Modellierung kann irreführend Ergebnissen. Eine Beschreibung finden Sie unter [Erlernen von Aufgaben an, um Daten für erweiterte Rechner vorbereiten](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Tools und Sprachen Fragen
Es gibt zahlreiche Optionen hier je nachdem, welche Sprachen und Entwicklung Umgebungen oder Tools Sie benötigen oder am häufigsten conformable verwenden werden.

1. **Welche Sprachen bevorzugen Sie für die Analyse verwenden?**  
    - R
    - Python
    - SQL

2. **Welche Tools sollten Sie für die Datenanalyse verwenden?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - eine Skript-Sprache, die zur Verwaltung von Ressourcen in einer anderen Skriptsprache Azure verwendet.
    - [Azure-Computern Learning Studio](machine-learning-what-is-ml-studio/)
    - [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python-Tools für Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter Notizbücher](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Identifizieren von Ihrem Szenario erweiterte analytics
Nachdem Sie die Fragen im vorherigen Abschnitt beantwortet haben, sind Sie bereit, um zu bestimmen, welche optimale Szenario Ihr Fall passt. [Szenarien für erweiterte Analytics Azure Computer interessante](machine-learning-data-science-plan-sample-scenarios.md)sind die Beispielszenarien umrandet.

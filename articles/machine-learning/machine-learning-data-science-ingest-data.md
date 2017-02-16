<properties 
    pageTitle="Laden Sie die Daten in Speicher-Umgebungen für Analytics | Microsoft Azure" 
    description="Verschieben von Daten an und von Azure Blob-Speicher" 
    services="machine-learning,storage" 
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

# <a name="load-data-into-storage-environments-for-analytics"></a>Laden von Daten in Speicher-Umgebungen für analytics

Das Team Daten Wissenschaft Prozess setzt voraus, dass Daten aufgenommen oder in einer Vielzahl von verschiedenen Speicher-Umgebungen verarbeitet oder in die am besten geeignete Methode in den einzelnen Phasen des Prozesses analysiert werden geladen werden. Gängige für die Verarbeitung von Datenziele einbeziehen Azure BLOB-Speicher, SQL Azure-Datenbanken, SQL Server auf Azure-virtuellen Computer, HDInsight (Hadoop) und Azure maschinellen Learning 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Diese **Menü** enthält Links zu Themen, die beschreiben, wie Daten in diese Ziel-Umgebungen Aufnahme, wo die Daten gespeichert und verarbeitet werden.

Technische und geschäftliche Anforderungen als auch die ursprüngliche Position, formatieren, und Größe Ihrer Daten bestimmt die Ziel-Umgebungen, in denen die Daten zum Erreichen der Ziele der Analyse aufgenommen werden müssen. Es ist nicht selten für ein Szenario zum Anfordern von Daten zwischen mehreren Umgebungen erzielen Sie die Reihe von Aufgaben, die zum Erstellen eines Modells Vorhersagen erforderlich verschoben werden soll. Diese Abfolge von Vorgängen kann beispielsweise Durchsuchen von Daten, vorab verarbeiten, bereinigen, Down-Stichproben und Modell Schulung einbeziehen.

<properties
    pageTitle="Verschieben von Daten an und von Azure Blob-Speicher | Microsoft Azure"
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
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Verschieben von Daten an und von Azure Blob-Speicher

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Anleitungen zum Verschieben von Daten von und/oder nach Azure Blob-Speicher verwendeten Technologien hier verknüpft sind:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Welches Verfahren für Sie am besten geeignet ist, hängt von Ihrem Szenario ab. [Szenarien für erweiterte Analytics Azure Computer interessante](machine-learning-data-science-plan-sample-scenarios.md) Artikel können Sie die Ressourcen bestimmen, die Sie für eine Vielzahl von Daten Wissenschaft Workflows in den erweiterten Analytics-Prozess verwendet werden müssen.

> [AZURE.NOTE] Finden Sie eine vollständige Einführung Azure Blob-Speicher [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure Blob-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Alternativ können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) zu verwenden: 

- Erstellen Sie und Planen Sie eine Verkaufspipeline, die Daten aus Azure Blob-Speicher heruntergeladen, 
- an einem veröffentlichten Azure maschinellen Learning Webdienst übergeben, 
- die Ergebnisse Vorhersageanalytik erhalten und 
- Laden Sie die Ergebnisse auf Speicher. 

Weitere Informationen finden Sie unter [Erstellen Vorhersage Pipelines mit Azure Data Factory und Azure-Computer-Schulung](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird davon ausgegangen, dass Sie ein Azure-Abonnement, ein Konto Speicherplatz und den entsprechenden Speicherschlüssel für dieses Konto verfügen. Vor dem Upload/Download Daten, müssen Sie den Azure-Speicher-Konto Servername und die Kontoinformationen Key kennen.

- Zum Einrichten eines Azure-Abonnements finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/)aus.
- Erstellen eines Kontos Speicher Anweisungen sowie erste Konto und wichtige Informationen finden Sie unter [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).

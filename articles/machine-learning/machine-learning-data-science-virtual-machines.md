<properties
    pageTitle="Daten Wissenschaft virtuellen Computern in Azure | Microsoft Azure"
    description="Festlegen von einer Daten Wissenschaft virtuellen Computern"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Daten Wissenschaft virtuellen Computern in Azure

Anweisungen dienen so, dass die Informationen zum Einrichten einer Azure-virtuellen Computer und eine Azure-virtuellen Computer mit SQL Server-Dienst als IPython Notizbuch Servern beschreiben. Die Windows-Computer ist mit Unterstützung von Tools wie IPython Notizbuch, Azure-Speicher-Explorer und AzCopy sowie andere Dienstprogramme, die für Projekte, die Daten Wissenschaft hilfreich sind konfiguriert. Azure-Speicher-Explorer und AzCopy, ermöglichen das beispielsweise geeignete Hochladen von Daten in Azure-Speicher von Ihrem lokalen Computer oder von Speicher auf Ihrem lokalen Computer herunterladen. 

Diese Menü enthält Links zu Themen zum Einrichten der verschiedenen Wissenschaft-Umgebungen durch das [Team Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md)verwendet werden.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Verschiedene Typen von Azure-virtuellen Computern Bereitstellung und so konfiguriert, dass Sie als Teil eines cloudbasierten Daten Wissenschaft Umgebung verwendet werden. Die Entscheidung über die Art des virtuellen Computers verwenden, hängt von Art und Menge der Daten zu mit maschinellen Lern- und der Zielbereich für die Daten in der Cloud entworfen werden. 

* Fragen zu berücksichtigen ist bei dieser Entscheidung Anleitungen finden Sie unter [Planen Ihrer Azure-Computer Learning Daten Wissenschaft-Umgebung](machine-learning-data-science-plan-your-environment.md). 
* Katalog der einige Szenarien, die bei der erweiterten Analytics auftreten können, finden Sie unter [Szenarien für den erweiterten Analytics Prozess Azure Computer interessante Technologie](machine-learning-data-science-plan-sample-scenarios.md)

Zwei Sätze von Anweisungen stehen zur Verfügung:

* [Einrichten einer Azure-virtuellen Computern als Server IPython Notizbuch für erweiterte Analytics](machine-learning-data-science-setup-virtual-machine.md) wird gezeigt, wie Bereitstellen einer Azure-virtuellen Computern mit IPython Notizbuch und andere Tools verwendet, um Daten Wissenschaft für Fälle führen, in denen ein Formular als SQL Azure-Speicher zum Speichern der Daten verwendet werden kann.

* [Einrichten einer Azure SQL Server-virtuellen Computern als Server IPython Notizbuch für erweiterte Analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md) wird gezeigt, wie Bereitstellen einer Azure SQL Server-virtuellen Computern mit IPython Notizbuch und andere Tools verwendet, um Daten Wissenschaft Fällen führen Sie in der SQL-Datenbank zum Speichern der Daten verwendet werden kann.

Nachdem bereitgestellt und konfiguriert ist, können diesen virtuellen Computern für die Verwendung als IPython Notizbuch Server für die Untersuchung und Bearbeitung der Daten und für andere Aufgaben in Verbindung mit Azure maschinellen Lern- und das Team Daten Wissenschaft Prozess (TDSP) erforderlich. Die nächsten Schritte beim Wissenschaft von Daten in den [TDSP Learning Pfad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet sind und Schritte, die Daten in SQL Server oder HDInsight verschieben, verarbeiten und dort Vorbereitung Learning aus den Daten mit den Azure Computer vertraut machen (Beispiel) einschließen.


> [AZURE.NOTE] Der Preis von Azure-virtuellen Computern sind als **bezahlen nur für was Sie verwenden**. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden sind während des virtuellen Computers nicht verwenden, muss es in den Zustand **beendet (Deallocated)** aus dem [Klassischen Azure-Portal](http://manage.windowsazure.com/)werden. Eine schrittweise Anleitung oder wie Sie virtuellen Computern freigeben, finden Sie unter [war(en) und Freigeben von virtuellen Computern nicht in verwenden](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 

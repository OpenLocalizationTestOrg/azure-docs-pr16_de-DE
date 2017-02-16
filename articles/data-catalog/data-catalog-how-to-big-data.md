<properties
   pageTitle="Zum Arbeiten mit Datenquellen 'big Data' | Microsoft Azure"
   description="Gewusst wie-Artikel Hervorheben von Mustern für die Verwendung von Azure Datenkatalog mit 'big Data' Datenquellen, einschließlich Azure BLOB-Speicher, Azure Daten Sees und Hadoop HDFS."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Zum Arbeiten mit Azure Datenkatalog große Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient. Kurzum, ist **Azure Datenkatalog** alle zu Hilfe Personen ermitteln, verstehen und Verwenden von Datenquellen und Hilfe Organisationen, um größeren Nutzen aus ihrer vorhandenen Datenquellen, einschließlich große Daten zu ziehen.

**Azure Datenkatalog** unterstützt die Registrierung von Azure-Blog-Speicher Blobs und Verzeichnisse als auch Hadoop HDFS Dateien und Verzeichnisse durchsuchen. Die teilweise strukturierte Art der folgenden Datenquellen bietet eine umfassende Flexibilität, aber dies bedeutet auch, dass Benutzer wie die Datenquellen angeordnet sind, um den meisten Nutzen aus **Azure Datenkatalog**Ihre Registrierung berücksichtigt werden müssen.

## <a name="directories-as-logical-data-sets"></a>Verzeichnisse durchsuchen, wie logische Datensätze

Ein übliches Schema zum Organisieren von große Datenquellen besteht darin, Verzeichnisse als logische Datensätze zu behandeln. Verzeichnisse der obersten Ebene dienen zum Definieren von einer Datengruppe zurück, während die Unterordner definieren Partitionen und die darin enthaltenen Dateien speichern, die Daten selbst.

Ein Beispiel für dieses Muster möglicherweise:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In diesem Beispiel darstellen Vehicle_maintenance_events und Location_tracking_events logische Datensätze aus. Jeder dieser Ordner enthält Datendateien die nach Jahr und Monat in Unterordner angeordnet sind. Jeder dieser Ordner könnte potenziell Hunderte oder Tausende von Dateien enthalten.

Bei diesem Muster ist die registrieren einzelner Dateien mit **Azure Datenkatalog** wahrscheinlich nicht sinnvoll. Stattdessen registrieren Sie sich mit der Verzeichnisse durchsuchen, die die Datensätze darstellen, die an die Benutzer arbeiten mit den Daten aussagekräftiger werden sollen.

## <a name="reference-data-files"></a>Verweis-Datendateien

Ein Muster Komplement ist Datasets Bezug als einzelne Dateien gespeichert. Diese Datasets möglicherweise als der Seite 'klein' große Datenmengen vorstellen, und ähneln oft Dimensionen in einem Modell Analysedaten. Verweis Datendateien enthalten Datensätze, die verwendet werden, um Kontext für die Massen an anderer Stelle im große Datenspeicher gespeicherten Daten Dateien bereitzustellen.

Ein Beispiel für dieses Muster möglicherweise:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Wenn eine Scientist Analysten oder Daten mit den Daten in den größeren Directory Strukturen arbeitet, können die Daten in diese Dateien Bezug Ausführlichere Informationen für Personen bereitzustellen, die in dem größeren DataSet nur nach Name oder ID bezeichnet werden, verwendet werden.

In diesem Muster wird es sinnvoll die einzelnen Referenz Datendateien mit **Azure Datenkatalog**registrieren. Jede Datei darstellt, einer Datengruppe zurück, und jeweils einzeln erkannt und kommentiert werden kann.

## <a name="alternate-patterns"></a>Alternative Muster

Die oben beschriebenen Muster werden nur auf zwei Arten möglich, die ein große Datenspeicher angeordnet werden kann, aber wird jede Implementierung unterschiedlich sein. Konzentrieren Sie unabhängig davon, wie Ihre Datenquellen strukturiert werden beim große Datenquellen mit **Azure Datenkatalog**registrieren, zum Registrieren der Dateien und Verzeichnisse durchsuchen, die die Datensätze darstellen, die von Value an andere Personen in Ihrer Organisation. Registrieren alle Dateien und Verzeichnisse Datenmüll den Katalog, wodurch es schwieriger zu finden, was sie benötigen.

## <a name="summary"></a>Zusammenfassung
Registrieren von Datenquellen mit **Azure Datenkatalog** sind sie leichter zu erkennen und zu verstehen. Durch die Registrierung und Stapels große Datendateien und Verzeichnisse durchsuchen, die logische Datensätze darstellen, können Benutzer suchen und verwenden Sie die große Datenquellen benötigten helfen.

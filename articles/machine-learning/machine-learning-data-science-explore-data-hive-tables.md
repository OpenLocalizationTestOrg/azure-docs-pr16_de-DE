<properties
    pageTitle="Untersuchen von Daten in die Struktur Tabellen mit Struktur Abfragen | Microsoft Azure"
    description="Untersuchen von Daten in die Struktur von Tabellen mit Struktur Abfragen."
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Untersuchen von Daten in die Struktur Tabellen mit Struktur Abfragen

Dieses Dokument enthält Stichprobe Struktur Skripts, mit denen Daten in die Struktur von Tabellen in einem Cluster HDInsight Hadoop durchsuchen.

Die folgenden **Menü** Links zu Themen, für die Verwendung von Tools zum Durchsuchen von Daten aus verschiedenen Speicher-Umgebungen beschrieben.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie:

* Erstellt ein Azure-Speicher-Konto an. Wenn Sie die Anweisungen benötigen, finden Sie unter [Erstellen eines Kontos Azure-Speicher](../storage/storage-create-storage-account.md#create-a-storage-account)
* Nach der Bereitstellung eines angepassten Hadoop Clusters mit dem Dienst HDInsight. Wenn Sie die Anweisungen benötigen, finden Sie unter [Anpassen Azure HDInsight Hadoop Cluster für erweiterte Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* Die Daten wurde die Struktur Tabellen in Azure HDInsight Hadoop Cluster hochgeladen. Wenn dies nicht der Fall, folgen Sie den Anweisungen in [Erstellen und Laden der Daten in Tabellen Struktur](machine-learning-data-science-move-hive-tables.md) zuerst Hochladen von Daten in Tabellen Struktur.
* Aktiviert Remotezugriff auf Cluster an. Wenn Sie die Anweisungen benötigen, finden Sie unter [Zugriff die Hadoop Cluster Kopf Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Wenn Sie die Anweisungen zum Übermitteln der Struktur Abfragen benötigen, finden Sie unter [So übermitteln Struktur Abfragen](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Beispiel für Struktur Abfrage Skripts für das Durchsuchen von Daten

1. Rufen Sie die Anzahl der Beobachtungen pro partition `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Rufen Sie die Anzahl der Beobachtungen pro Tag `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Abrufen der Ebenen in einem kategorisierten Spalte  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Die Anzahl der Ebenen in Kombination aus zwei kategorisierten Spalten abrufen `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Abrufen der Verteilung für numerische Spalten  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Extrahieren von Datensätzen aus Verknüpfen von zwei Tabellen

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Zusätzliche Abfrage Skripts für Taxi Geschäftsreise Datenszenarien

Beispiele für Abfragen, die speziell für Szenarios [NYC Taxi Geschäftsreise Daten](http://chriswhong.com/open-data/foil_nyc_taxi/) sind, werden ebenfalls im [Github Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)bereitgestellt. Diese Abfragen bereits Datenschema angegeben haben und sind bereit, die auszuführenden vorzulegen.

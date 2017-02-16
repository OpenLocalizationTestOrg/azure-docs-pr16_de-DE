<properties
    pageTitle="Herstellen einer Verbindung mit Power Query Hadoop mit Excel | Microsoft Azure"
    description="Erfahren Sie, wie Sie nutzen von Business Intelligence-Komponenten und Power Query für Excel Access-Daten in Hadoop auf HDInsight gespeichert."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Verbinden von Excel mit Hadoop mithilfe von Power Query

Eine der wichtigsten Features der Microsoft-Lösung big Data ist die Integration von Microsoft Business Intelligence (BI)-Komponenten mit Hadoop Cluster in Azure HDInsight. Beispiel einer primären für diese Integration ist die Möglichkeit, die mit den Daten, die Ihren Cluster Hadoop zugeordnet sind, mithilfe von Microsoft Power Query für Excel-add-in, das Konto Azure-Speicher Excel Verbindung. In diesem Artikel führt Sie durch die zum Einrichten und Verwenden von Power Query zum Abfragen von Daten mit einem Hadoop Cluster verwaltete mit HDInsight verknüpft ist.

> [AZURE.NOTE] Während Sie die Schritte in diesem Artikel mit einem Linux oder Windows-basierten HDInsight Cluster verwendet werden können, ist Windows für Clientcomputer erforderlich.

### <a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein HDInsight Cluster**. Um eine konfigurieren, finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
- **A Arbeitsstationen** , die Windows 7, Windows Server 2008 R2 oder höher Betriebssystem auf dem ausgeführt wird.
- **Office 2013 Professional Plus, Office 365 ProPlus, Office 2010 Professional Plus oder Excel 2013 eigenständigen**.


## <a name="install-power-query"></a>Installieren von Power Query

Power Query kann verwendet werden, zum Importieren von Daten aus einer Vielzahl von Datenquellen in Microsoft Excel, wo die BI-Tools, wie PowerPivot und Power View eingeschaltet werden kann. Insbesondere importieren Power Query Daten, die die Ausgabe wurde oder, die von einem Hadoop Auftrag ausführen auf einem Cluster HDInsight generiert wurde.

Microsoft Power Query für Excel aus dem [Microsoft Download Center] herunterladen[ powerquery-download] und zu installieren.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight Daten in Excel importieren

Das Power Query-add-in für Excel erleichtert das Importieren von Daten aus HDInsight Cluster in Excel, wobei BI-Tools, wie PowerPivot und Power Map verwendet werden können, um zu überprüfen, analysieren und Präsentieren von Daten.

**So importieren Sie Daten aus einem Cluster HDInsight**

1. Öffnen Sie Excel.

2. Erstellen Sie eine neue leere Arbeitsmappe.

3. Klicken Sie auf das Menü **Power Query** auf **Aus Azure**, und klicken Sie dann auf **Aus Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Hinweis:** Wenn Sie das **Power Query** -Menü angezeigt werden, wechseln Sie zu der **Datei** > **Optionen** > **-Add-Ins**, und wählen Sie **COM-Add-ins** aus dem Dropdownmenü **Verwalten** Feld am unteren Rand der Seite. Wählen Sie die Schaltfläche **wechseln...** , und stellen Sie sicher, dass das Kontrollkästchen für die Power Query für Excel-add-ins geprüft wurde.

    **Hinweis:** Power Query können Sie Daten aus HDFS importieren, indem Sie auf **Aus anderen Quellen**.

3. Geben Sie den Namen für das Konto ein Azure Blob Storage Ihren Cluster zugeordnet **Kontonamen**, und klicken Sie dann auf **OK**. Dies kann das [Standardkonto-Speicher](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) oder ein Speicherkonto verknüpfte sein.  Das Format ist *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Kontoschlüssel**Geben Sie die Taste für das Blob-Speicher-Konto, und klicken Sie dann auf **Speichern**. (Sie müssen Sie diese Speicher zugegriffen werden nur beim ersten Mal Aktion.)

5. Doppelklicken Sie im Bereich **Navigator** auf der linken Seite des Abfrage-Editors auf den BLOB-Speicher Containername. Standardmäßig ist der Containername denselben Namen wie den Clusternamen.

6. Suchen Sie nach **"hivesampledata.txt"** in der Spalte **Name** (der Pfad ist **... / Struktur/warehouse/Hivesampletable/**), und klicken Sie dann auf **binäre** auf der linken Seite des "hivesampledata.txt". "Hivesampledata.txt" enthält alle Cluster. Optional können Sie eine eigene Datei.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Wenn Sie möchten, können Sie die Spaltennamen umbenennen. Wenn Sie bereit sind, klicken Sie auf **Schließen, und Laden**.  Mit Ihrer Arbeitsmappe weist die Daten geladen wurden:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel gelernt Sie Power Query zum Abrufen von Daten aus HDInsight in Excel verwenden. Auf ähnliche Weise können Sie Daten aus HDInsight in SQL Azure-Datenbank abrufen. Es ist auch möglich, Daten in HDInsight hochladen. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Herstellen einer Verbindung mit dem Microsoft-Struktur ODBC-Treiber HDInsight mit Excel][hdinsight-ODBC]
* [Hochladen von Daten mit HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689

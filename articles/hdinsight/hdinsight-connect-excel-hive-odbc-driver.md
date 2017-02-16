<properties
   pageTitle="Herstellen einer Verbindung mit der Struktur ODBC-Treiber Hadoop mit Excel | Microsoft Azure"
   description="Informationen Sie zum Einrichten und verwenden den Microsoft-Struktur ODBC-Treiber für Excel zum Abfragen von Daten in einem Cluster HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Verbinden von Excel mit Hadoop mit dem Microsoft-Struktur ODBC-Treiber

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft Big Data Lösung integriert Microsoft Business Intelligence (BI)-Komponenten mit Apache Hadoop Cluster, die durch die Azure HDInsight bereitgestellt wurden. Ein Beispiel für diese Integration ist die Möglichkeit, eine Verbindung mit einer Hadoop Cluster in HDInsight mit dem Microsoft Struktur Open Database Connectivity (ODBC) Treiber Stock Datawarehouse Excel.

Es ist auch möglich, eine HDInsight Cluster und anderen Datenquellen, einschließlich anderer (nicht HDInsight) Hadoop Cluster, aus Excel mithilfe von Microsoft Power Query-add-in für Excel zugeordneten Daten zu verbinden. Informationen zum Installieren und Verwenden von Power Query finden Sie unter [Verbinden von Excel mit HDInsight mithilfe von Power Query][hdinsight-power-query].

> [AZURE.NOTE] Während Sie die Schritte in diesem Artikel mit einem Linux oder Windows-basierten HDInsight Cluster verwendet werden können, ist Windows für Clientcomputer erforderlich.

**Voraussetzungen für**:

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein HDInsight Cluster**. Um eine zu erstellen, finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
- **A Arbeitsstationen** mit Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 eigenständigen oder Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Installieren von Microsoft-Struktur ODBC-Treiber

Herunterladen und Installieren von Microsoft Struktur ODBC-Treiber aus dem [Download Center][hive-odbc-driver-download].

Dieser Treiber kann unter 32-Bit- oder 64-Bit-Versionen von Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 und Windows Server 2012 installiert werden und lässt die Verbindung mit Azure HDInsight (Version 1.6 und höher) und Azure HDInsight Emulator (v.1.0.0.0 und höher). Installieren Sie die Version, die die Version der Anwendung entspricht, an dem Sie den ODBC-Treiber verwenden. In diesem Lernprogramm wird der Treiber von Office Excel verwendet werden.

##<a name="create-hive-odbc-data-source"></a>Erstellen Sie eine Struktur ODBC-Datenquelle

Die folgenden Schritte gezeigt, wie eine Struktur ODBC-Datenquelle erstellen.

1. Windows 8 oder Windows 10 drücken Sie die Windows-Taste, um den Startbildschirm zu öffnen, und geben Sie dann auf **Datenquellen**.
2. Klicken Sie auf **ODBC-Datenquellen (32-Bit) einrichten** oder **Einrichten von ODBC-Datenquellen (64-Bit)** , abhängig von Ihrer Version von Office. Wenn Sie Windows 7 arbeiten, wählen Sie **unter Verwaltung** **ODBC-Datenquellen (32-Bit)** oder **ODBC-Datenquellen (64-Bit)** . Dadurch wird das Dialogfeld **ODBC-Datenquellen-Administrator** .

    ![ODBC Datenquellen-administrator][img-hdi-simbahiveodbc-datasource-admin]

3. Vom Benutzer DNS klicken Sie auf **Hinzufügen** , um den Assistenten **Neue Datenquelle erstellen** zu öffnen.
4. Wählen Sie **Microsoft Struktur ODBC-Treiber**aus, und klicken Sie dann auf **Fertig stellen**. Im Dialogfeld **Microsoft Struktur ODBC-Treiber DNS-Setup** wird gestartet.

5. Geben Sie ein, oder wählen Sie die folgenden Werte aus:

    Eigenschaft|Beschreibung
    ---|---
    Name der Datenquelle|Geben Sie einen Namen in der Datenquelle
    Host|Geben Sie &lt;HDInsightClusterName >. azurehdinsight.net. Beispielsweise myHDICluster.azurehdinsight.net
    Port|Verwenden Sie <strong>443</strong>ein. (Dieser Anschluss wurde von 563 in 443 geändert wurde.)
    Datenbank|Verwenden Sie <strong>Standard</strong>.
    Struktur Server-Datentyp|Wählen Sie die <strong>Struktur der Server 2</strong> aus.
    Verfahren|Wählen Sie <strong>Azure HDInsight-Dienst</strong>
    HTTP-Pfad|Lassen Sie ihn leer.
    Benutzername|Geben Sie HDInsight Cluster Benutzer Benutzernamen ein. Dies ist der Benutzername, der beim Bereitstellen von Cluster erstellte. Wenn Sie die Option Symbolleiste erstellen verwendet haben, ist der Standard-Benutzername <strong>Admin</strong>.
    Kennwort|Geben Sie HDInsight Cluster Benutzerkennwort.
    </table>

    Es gibt einige wichtige Parameter Achten Sie beim **Erweiterte Optionen,**klicken Sie auf:

    Parameter|Beschreibung
    ---|---
    Verwenden Sie die ursprüngliche Abfrage|Wenn es ausgewählt ist, wird der ODBC-Treiber keinesfalls TSQL in HiveQL konvertieren. Sie sind nur verwenden, wenn Sie sind sicher, dass Sie reine HiveQL Anweisungen senden (100 %). Bei der Verbindung mit SQL Server- oder SQL Azure-Datenbank sollten Sie es deaktiviert lassen.
    Zeilen, die abgerufen pro blockieren|Wenn Sie eine große Menge von Datensätzen abholen, möglicherweise für diesen Parameter optimieren müssen Sie optimale aufführungen sicherzustellen.
    Standard-Spalte Zeichenfolgenlänge, binäre Spaltenlänge, Decimal-Spalte skalieren|Der Datentyp, Länge und geht möglicherweise beeinflussen, wie Daten zurückgegeben werden. Diese bewirkt, dass falsche Informationen zurückgegeben werden, weil Verlust der Genauigkeit und/oder abgeschnitten.


    ![Erweiterte Optionen][img-HiveOdbc-DataSource-AdvancedOptions]

6. Klicken Sie auf **Testen** , um die Datenquelle zu testen. Wenn die Datenquelle richtig konfiguriert ist, zeigt *überprüft wurde erfolgreich abgeschlossen!*.
7. Klicken Sie auf **OK** , um das Dialogfeld Test zu schließen. Die neue Datenquelle sollte jetzt auf den **ODBC-Datenquellen-Administrator**aufgeführt sein.
8. Klicken Sie auf **OK** , um den Assistenten zu beenden.

##<a name="import-data-into-excel-from-hdinsight"></a>Importieren von Daten in Excel aus HDInsight

Die folgenden Schritte beschreiben die Möglichkeit zum Importieren von Daten aus einer strukturtabelle in einer Excel-Arbeitsmappe, die unter Verwendung der ODBC-Datenquelle, die Sie in den vorstehenden Schritten erstellt haben.

1. Öffnen einer neuen oder vorhandenen Arbeitsmappe in Excel.
2. Der Registerkarte **Daten** auf **Aus anderen Datenquellen**, und klicken Sie dann auf **Vom Datenverbindungs-Assistenten** aus, um den **Datenverbindungs-Assistenten**zu starten.

    ![Öffnen Datenverbindungs-Assistenten][img-hdi-simbahiveodbc.excel.dataconnection]

3. Wählen Sie als Datenquelle **ODBC-DSN** aus, und klicken Sie dann auf **Weiter**.
4. ODBC-Datenquellen wählen Sie aus der Name der Datenquelle, die Sie im vorherigen Schritt erstellt haben, und klicken Sie dann auf **Weiter**.
5. Geben Sie das Kennwort für den Cluster im Assistenten erneut ein, und klicken Sie dann auf **Testen** , um die Konfiguration erneut zu überprüfen, falls erforderlich.
6. Klicken Sie auf **OK** , um das Testdialogfeld zu schließen.
7. Klicken Sie auf **OK**. Warten Sie im Dialogfeld **Datenbank und Tabelle wählen** zu öffnen. Dies kann einige Sekunden dauern.
8. Wählen Sie die Tabelle, die Sie importieren möchten, und klicken Sie dann auf **Weiter**. Die *Hivesampletable* ist eine Stichprobe strukturtabelle, die mit HDInsight Cluster stammen.  Wenn Sie eine erstellt haben, können Sie es auswählen. Weitere Informationen zum Ausführen Struktur Abfragen und Struktur Tabellen erstellen, finden Sie unter [Verwendung mit HDInsight Struktur][hdinsight-use-hive].
8. Klicken Sie auf **Fertig stellen**.
9. Klicken Sie im Dialogfeld **Daten importieren** können Sie ändern, oder geben die Abfrage. Klicken Sie hierzu auf **Eigenschaften**. Dies kann einige Sekunden dauern.
10. Klicken Sie auf die Registerkarte **Definition** und dann an die Struktur select-Anweisung in das Textfeld **Befehlstext** fügen Sie **Beschränkung 200 an** . Die Änderung wird die Menge der zurückgegebenen Datensatz auf 200 beschränkt.

    ![Verbindungseigenschaften][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klicken Sie auf **OK** , um das Dialogfeld Verbindungseigenschaften zu schließen.
12. Klicken Sie auf **OK** , um das Dialogfeld **Daten importieren** zu schließen.  
13. Geben Sie das Kennwort erneut ein, und klicken Sie dann auf **OK**. Es dauert einige Sekunden dauern, bevor Sie Daten nach Excel importiert werden.

##<a name="next-steps"></a>Nächste Schritte

In diesem Artikel gelernt Sie den Microsoft-Struktur ODBC-Treiber zum Abrufen von Daten aus dem HDInsight Service in Excel verwenden. Auf ähnliche Weise können Sie Daten aus dem HDInsight Service in SQL-Datenbank abrufen. Es ist auch möglich, Daten in einer HDInsight Service hochladen. Weitere Informationen finden Sie unter:

- [Analysieren Sie HDInsight mit Verzögerung Flugdaten][hdinsight-analyze-flight-data]
- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png

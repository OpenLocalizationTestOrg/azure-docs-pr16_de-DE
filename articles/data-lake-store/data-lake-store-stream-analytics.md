<properties
   pageTitle="Übertragen Sie Daten aus Stream Analytics in Lake Datenspeicher | Azure"
   description="Verwenden von Azure Stream Analytics zum Streamen von Daten in Azure Lake Datenspeicher"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Streaming von Daten aus Azure-Speicher Blob in Lake Datenspeicher mit Azure Stream Analytics

In diesem Artikel erfahren Sie, wie Azure Lake Datenspeicher als ein Ergebnis für ein Projekt Azure Stream Analytics verwendet werden. Dieser Artikel beschreibt ein einfaches Szenario, das Daten aus einer Speicher Azure Blob (Eingabe) liest und schreibt die Daten in Lake Datenspeicher (Ausgabe).

>[AZURE.NOTE] Erstellung und Konfiguration der Lake Datenspeicher Ausgaben für Stream Analytics wird zu diesem Zeitpunkt nur in der [Klassischen Azure-Portal](https://manage.windowsazure.com)unterstützt. Daher werden einige Elemente dieses Lernprogramms im klassischen Azure-Portal verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Aktivieren Sie Ihr Abonnement Azure** für Daten dem Store Public Preview. [Anweisungen](data-lake-store-get-started-portal.md#signup)finden Sie unter.

- **Speicher Azure-Konto**. Sie verwenden ein Containers Blob von diesem Konto, Daten für ein Projekt Stream Analytics einzugeben. In diesem Lernprogramm wird davon ausgegangen Sie, dass Sie ein Speicherkonto namens **Datalakestoreasa** und einen Container in das Konto namens **Datalakestoreasacontainer**erstellen. Nachdem Sie den Container erstellt haben, laden Sie eine Beispiel-Datendatei zu hoch. Sie können eine Stichprobe Datendatei aus dem [Azure Daten dem Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)erhalten. Verschiedene Clients, wie z. B. [Azure-Speicher-Explorer](http://storageexplorer.com/), können Sie die Daten zu einem Container Blob hochladen.

    >[AZURE.NOTE] Wenn Sie das Konto aus dem Azure-Portal erstellen, stellen Sie sicher, dass Sie sie mit **der Option Klassisch Bereitstellung** erstellt haben. Dadurch wird sichergestellt, dass das Speicherkonto im klassischen Azure-Portal zugegriffen werden kann, da dies ist, was wir verwenden Sie zum Erstellen eines Auftrags Stream Analytics. Anweisungen zum Erstellen eines Kontos Speicher vom Azure-Portal mit der Bereitstellung klassischen finden Sie unter [Erstellen eines Kontos Azure-Speicher](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Oder erstellen Sie ein Speicherkonto vom klassischen Azure-Portal.

- **Azure dem Datenspeicher-Konto**. Folgen Sie den Anweisungen bei [den ersten Schritten mit Azure dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md)an.  


## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics Auftrag

Erstellen zunächst Sie einen Stream Analytics Auftrag, der eine Eingabe Quell-als auch eine Ausgabe enthält. In diesem Lernprogramm die Quelle einer Azure Blob Container und das Ziel des Sees Datenspeicher.

1. Melden Sie sich auf die [Azure klassischen Portal](https://manage.windowsazure.com).

2. Klicken Sie in der unteren linken Rand des Bildschirms auf **neu**, **Datendiensten**, **Stream Analytics**, **Schnellen Erstellen**. Geben Sie die Werte aus, wie unten dargestellt, und klicken Sie dann auf **Stream Analytics-Projekt erstellen**.

    ![Erstellen Sie einen Stream Analytics Auftrag] (./media/data-lake-store-stream-analytics/create.job.png "Erstellen eines Auftrags Stream Analytics")

## <a name="create-a-blob-input-for-the-job"></a>Erstellen Sie eine Eingabesprache Blob für das Projekt

1. Öffnen Sie die Seite für den Auftrag Stream Analytics, klicken Sie auf der Registerkarte **Eingaben** , und klicken Sie dann auf **Hinzufügen einer Eingabesprache** , um einen Assistenten zu starten.

2. Klicken Sie auf der Seite **Hinzufügen einer Eingabe in Ihrem Auftrag** markieren Sie **Stream Daten**, und klicken Sie dann auf den Pfeil weiter.

    ![Hinzufügen einer Eingabe für Ihre Arbeit] (./media/data-lake-store-stream-analytics/create.input.1.png "Hinzufügen einer Eingabe für Ihre Arbeit")

3. Klicken Sie auf der Seite **Hinzufügen einer Datenstream auf Ihre Arbeit** **Blob-Speicher**wählen Sie aus, und klicken Sie dann auf den Pfeil weiter.

    ![Hinzufügen eines Streams Daten zu den Auftrag] (./media/data-lake-store-stream-analytics/create.input.2.png "Hinzufügen eines Streams Daten zu den Auftrag")

4. Geben Sie auf der Seite **Einstellungen für Blob-Speicher** Details für Blob-Speicher, den Sie als die Eingabedatenquelle verwendet werden soll.

    ![Bereitstellen der Blob-Speicher Einstellungen] (./media/data-lake-store-stream-analytics/create.input.3.png "Bereitstellen der Blob-Speicher Einstellungen")

    * **EINGABETASTE Eingabewerte Alias**. Dies ist ein eindeutiger Name, den Sie für den Auftrag Eingabemethoden bereitstellen.
    * **Wählen Sie ein Speicherkonto**. Vergewissern Sie sich das Speicherkonto ist in der gleichen Region wie der Stream Analytics Auftrag oder Sie Verschieben von Daten zwischen Regionen zusätzliche Kosten entstehen.
    * **Bereitstellen eines Containers Speicher**. Sie können auch einen neuen Container erstellen, oder wählen Sie einen vorhandenen Container.

    Klicken Sie auf den Pfeil weiter.

5. Klicken Sie auf der Einstellungsseite **Serialisierung** legen Sie das Format als **CSV**und Trennzeichen als **Registerkarte**Codierung als **UTF8**, und klicken Sie dann auf das Häkchen.

    ![Angeben die Serialisierungseinstellungen] (./media/data-lake-store-stream-analytics/create.input.4.png "Angeben die Serialisierungseinstellungen")

6. Sobald Sie mit dem Assistenten fertig sind, wird die Eingabe Blob eingefügt werden soll, klicken Sie auf der Registerkarte **Eingaben** , und die Spalte **Diagnose** **OK**anzeigen. Sie können die Verbindung mit der Eingabe auch explizit testen, mithilfe der Schaltfläche " **Verbindung testen** " unten.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Erstellen einer Sees Datenspeicher Ausgabe für das Projekt

1. Öffnen Sie die Seite für den Auftrag Stream Analytics, klicken Sie auf der Registerkarte **Ausgaben** , und klicken Sie dann auf **Hinzufügen ein Ergebnis** , um einen Assistenten zu starten.

2. Klicken Sie auf der Seite **Hinzufügen einer Ausgabe in Ihrem Auftrag** wählen Sie aus **Dem Datenspeicher**, und klicken Sie dann auf den Pfeil weiter.

    ![Hinzufügen einer Ausgabe auf Ihre Arbeit] (./media/data-lake-store-stream-analytics/create.output.1.png "Hinzufügen einer Ausgabe auf Ihre Arbeit")

3. Auf der Seite **Autorisieren Verbindung** Wenn Sie bereits ein Konto Lake Datenspeicher erstellt haben, klicken Sie auf **Jetzt autorisieren**. Klicken Sie andernfalls auf **Jetzt registrieren** , um ein neues Konto erstellen. In diesem Lernprogramm gehen wir davon aus, dass Sie bereits ein Lake Datenspeicher-Konto (wie die Voraussetzung angegeben) erstellt haben. Sie werden automatisch mit den Anmeldeinformationen, mit denen Sie in der klassischen Azure-Portal angemeldet, berechtigt sein.

    ![Dem Datenspeicher autorisieren] (./media/data-lake-store-stream-analytics/create.output.2.png "Dem Datenspeicher autorisieren")

4. Geben Sie die Informationen auf der Seite **Einstellungen für die Daten dem-Store** wie auf dem folgenden Bildschirmfoto dargestellt.

    ![Geben Sie dem Datenspeicher-Einstellungen] (./media/data-lake-store-stream-analytics/create.output.3.png "Geben Sie dem Datenspeicher-Einstellungen")

    * **Eingabe eines Ausgabealias**. Dies ist ein eindeutiger Name, den Sie für die Ausgabe der Position ein.
    * **Angeben eines Kontos dem Datenspeicher**. Sie sollten bereits, erstellt haben wie die Voraussetzung angegeben.
    * **Angeben einer Pfad Präfixmuster**. Dies ist erforderlich, um die Ausgabedateien zu identifizieren, die durch den Auftrag Stream Analytics Lake Datenspeicher geschrieben werden. Da der Ausgänge, indem Sie den Auftrag geschrieben Titeln in einem Format GUID sind, einschließlich Präfix können die geschriebene Ausgabe identifiziert werden. Wenn eine Datums- und Uhrzeitstempel als Teil des Präfixes enthalten sein sollen Vergewissern Sie sich Sie aufnehmen `{date}/{time}` in das Präfixmuster. Wenn Sie dies einschließen, wird die Felder **Datum **und **Uhrzeitformat** aktiviert sind, und Sie können das Format der Auswahl auswählen.

    Klicken Sie auf den Pfeil weiter.

5. Klicken Sie auf der Einstellungsseite **Serialisierung** legen Sie das Format als **CSV**und Trennzeichen als **Registerkarte**Codierung als **UTF8**, und klicken Sie dann auf das Häkchen.

    ![Geben Sie das Ausgabeformat] (./media/data-lake-store-stream-analytics/create.output.4.png "Geben Sie das Ausgabeformat")

6. Sobald Sie mit dem Assistenten fertig sind, die Ausgabe Lake Datenspeicher hinzugefügt werden sollen, klicken Sie auf der Registerkarte **Ausgaben** und die **Diagnose** Spalte sollte **OK**anzeigen. Sie können die Verbindung zur Ausgabe auch explizit testen, mithilfe der Schaltfläche " **Verbindung testen** " unten.

## <a name="run-the-stream-analytics-job"></a>Führen Sie die Stapelverarbeitung Stream Analytics

Zum Ausführen eines Auftrags Stream Analytics müssen Sie über die Registerkarte Abfrage eine Abfrage ausführen. In diesem Lernprogramm können Sie die Stichprobe Abfrage ausführen, indem Sie die Platzhalter mit den Auftrag Eingabemethoden und Aliase, ausgeben, wie auf dem folgenden Bildschirmfoto dargestellt.

![Ausführen der Abfrage] (./media/data-lake-store-stream-analytics/run.query.png "Ausführen der Abfrage")

Klicken Sie auf **Speichern** , vom unteren Rand des Bildschirms, und klicken Sie dann auf **Start**. Klicken Sie im Dialogfeld Wählen Sie **Benutzerdefiniert**aus, und wählen Sie dann ein Datum aus der Vergangenheit, beispielsweise **1/1/2016**. Klicken Sie auf das Häkchen, um den Auftrag zu starten. Es kann einige Minuten dauern das Projekt starten.

![Festlegen der Position Zeit] (./media/data-lake-store-stream-analytics/run.query.2.png "Festlegen der Position Zeit")

Nachdem Sie der Auftrag gestartet wird, klicken Sie auf der Registerkarte **Monitor** , um anzuzeigen, wie die Daten verarbeitet wurden.

![Monitor Position] (./media/data-lake-store-stream-analytics/run.query.3.png "Monitor Position")

Schließlich können Sie das [Azure-Portal](https://portal.azure.com) Öffnen des Sees Datenspeicher-Kontos ein, und überprüfen, ob die Daten mit dem Konto erfolgreich geschrieben wurde.

![Die Ausgabe überprüfen] (./media/data-lake-store-stream-analytics/run.query.4.png "Die Ausgabe überprüfen")

Klicken Sie im Daten-Explorer ausgeben Mitteilung, die die Ausgabe in einen Ordner gemäß Angabe im Datenspeicher Lake geschrieben wird Einstellungen (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Siehe auch

* [Erstellen eines HDInsight Clusters um Lake Datenspeicher verwenden](data-lake-store-hdinsight-hadoop-use-portal.md)

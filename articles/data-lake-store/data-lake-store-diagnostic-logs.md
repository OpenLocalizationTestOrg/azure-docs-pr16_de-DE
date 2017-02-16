<properties 
   pageTitle="Anzeigen von Diagnoseprotokollen für Azure Lake Datenspeicher | Microsoft Azure" 
   description="Grundlagen Sie zum Einrichten und Diagnoseprotokolle für Azure Lake Datenspeicher zugreifen " 
   services="data-lake-store" 
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Zugreifen auf Diagnoseprotokolle für Azure Lake Datenspeicher

Informationen Sie zu wie Diagnoseprotokoll für Ihr Konto Lake Datenspeicher aktivieren und wie Sie die Protokolle für Ihr Konto erfassten anzeigen.

Organisationen können aktivieren Diagnoseprotokoll für ihre Azure Lake Datenspeicher-Konto zum Sammeln von Daten Access Überwachungsprotokolle, Informationen Liste von Benutzern den Zugriff auf die Daten bietet, wie häufig die Daten zugegriffen werden, wie viele Daten gespeichert werden, in dem Konto usw..

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Azure dem Datenspeicher-Konto**. Folgen Sie den Anweisungen bei [den ersten Schritten mit Azure dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md)an.

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Aktivieren Sie das Diagnoseprotokoll für Ihr Konto Lake Datenspeicher

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie Ihr Konto Lake Datenspeicher und klicken Sie dann auf **Diagnoseeinstellungen**aus Ihrem Lake Datenspeicher Konto Blade, klicken Sie auf **Einstellungen**.

3. Nehmen Sie in den **Diagnoseprotokollen** Blade die folgenden vor Diagnoseprotokoll konfigurieren.

    ![Diagnoseprotokollierung aktivieren] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

    * Festlegen Sie **Status** , **Klicken Sie auf** diagnoseprotokollierung aktivieren.
    * Sie können auch Store/die Daten auf zwei verschiedene Arten Prozess.
        * Wählen Sie die Option zum **Exportieren nach Ereignis Hub** Log Daten als Stream an einen Azure Ereignis-Hub an. In den meisten Fällen verwenden Sie diese Option, wenn Sie eine untergeordnete Verarbeitung Verkaufspipeline eingehende Protokolle in Echtzeit analysieren haben. Wenn Sie diese Option auswählen, müssen Sie die Details für das Ereignis Azure-Hub bereitstellen, die Sie verwenden möchten.
        * Wählen Sie die Option zum **Exportieren nach Speicher-Konto** zum Speichern von Protokollen mit einer Firma Azure-Speicher. Verwenden Sie diese Option, wenn Sie möchten die Daten zu archivieren, die zu einem späteren Zeitpunkt die Stapel verarbeitet werden sollen. Wenn Sie diese Option auswählen, müssen Sie ein Konto Azure-Speicher, um die Protokolle speichern bereitstellen.
    * Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides erhalten möchten.
    * Geben Sie die Anzahl der Tage, für die die Daten erhalten bleiben müssen.
    * Klicken Sie auf **Speichern**.

Nachdem Sie diagnoseeinstellungen aktiviert haben, können Sie die Protokolle der Registerkarte **Diagnoseprotokolle** anschauen.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Diagnoseprotokolle für Ihr Konto Lake Datenspeicher anzeigen

Es gibt zwei Methoden, um die Log-Daten für Ihr Konto Lake Datenspeicher anzuzeigen.

* Überprüfen Sie aus dem Konto Lake Datenspeicher Einstellungen anzeigen.
* Aus dem Azure-Speicher-Konto, in dem die Daten gespeichert werden

### <a name="using-the-data-lake-store-settings-view"></a>Verwenden die Daten dem Store Einstellungen anzeigen

1. Klicken Sie aus Ihrem Lake Datenspeicher Konto **Einstellungen** Blade auf **Diagnoseprotokolle**.

    ![Anzeigen von Diagnoseprotokollen Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Diagnoseprotokolle anzeigen") 

2. Das Blade **Diagnoseprotokolle** sollten Sie die Protokolle nach **Überwachungsprotokolle** und **Anfordern von Protokollen**kategorisierte angezeigt.
    * Anforderungsprotokolle erfassen jeder API Anforderung auf das Konto Lake Datenspeicher.
    * Überwachungsprotokolle ähneln anfordern Protokolle aber bieten eine viel ausführlichere Projektstrukturplan-Codes die Vorgänge auf das Konto Lake Datenspeicher ausgeführt wird. Beispielsweise kann mehrere "Anfügen" Vorgänge in der Überwachungsprotokolle ein einzelnen Upload-API Anruf in Anforderungsprotokollen führen.

3. Klicken Sie auf den Link **herunterladen** für jeden Eintrag Log die Protokolle herunterladen.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Aus dem Azure-Speicher-Konto enthält, die Log-Daten

1. Öffnen Sie das Azure-Speicher Konto Blade Lake Datenspeicher zugeordnet sind, für die Protokollierung, und klicken Sie dann auf Blobs. Der **Blob-Service** -Karte vorausgesetzt enthält zwei Container.

    ![Anzeigen von Diagnoseprotokollen Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnoseprotokolle anzeigen")

    * Der Container **Einsichten Protokolle Audit** enthält der Überwachungsprotokolle.
    * Die Container **Einsichten-Protokolle-Anfragen** enthält die Protokolle der Anfrage.

2. Innerhalb dieser Container werden die Protokolle unter den folgenden Struktur gespeichert.

    ![Anzeigen von Diagnoseprotokollen Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Diagnoseprotokolle anzeigen")

    Beispielsweise konnte der vollständige Pfad Überwachungsprotokoll werden`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, der vollständige Pfad in ein Anforderungsprotokoll könnte`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Verstehen der Struktur der Log-Daten

Die Protokolle Audit und Anfrage sind in einem JSON-Format. In diesem Abschnitt werden schauen Sie sich die Struktur der JSON für Anforderung und Überwachungsprotokolle.

### <a name="request-logs"></a>Anfordern von Protokollen

Hier ist ein Beispiel für-Eintrag in das JSON-formatierte Anforderungsprotokoll ein. Jede Blob weist eine Stammobjekt genannte **Datensätze** , die ein Array von Objekten Log enthält.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Log Schema anfordern

| Namen            | Typ   | Beschreibung                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| Zeit            | Zeichenfolge | Der Zeitstempel (in UTC), der das Protokoll                                              |
| resourceId      | Zeichenfolge | Platzieren Sie auf die Nummer der Ressource, die Operation durchgeführt wurde                            |
| Kategorie        | Zeichenfolge | Die Kategorie Protokoll. Beispielsweise **Serviceanfragen**.                                   |
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise Getfilestatus.              |
| ResultType-Wert      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| callerIpAddress | Zeichenfolge | Die IP-Adresse des Clients die Anforderung ausführenden                                |
| correlationId   | Zeichenfolge | Die Id für das Protokoll, das können verwendet werden, um eine Reihe von verknüpften Protokolleinträge zu gruppieren |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Details finden Sie unter                                                          |

#### <a name="request-log-properties-schema"></a>Log Eigenschaftsschema anfordern

| Namen                 | Typ   | Beschreibung                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Zeichenfolge | Die HTTP-Methode, die für den Vorgang verwendet werden. Beispielsweise erhalten. |
| Pfad                 | Zeichenfolge | Der Pfad der Vorgang wurde durchgeführt                   |
| RequestContentLength | Ganzzahl    | Die Länge des Inhalts der HTTP-Anforderung                    |
| ClientRequestId      | Zeichenfolge | Die Id, die diese Anforderung identifiziert              |
| Startzeit            | Zeichenfolge | Die Uhrzeit, zu der der Server die Anforderung empfangen         |
| Endzeit              | Zeichenfolge | Die Uhrzeit, zu der der Server eine Antwort gesendet              |

### <a name="audit-logs"></a>Überwachungsprotokolle

Hier ist ein Beispiel für-Eintrag in das JSON-formatierte Überwachungsprotokoll ein. Jede Blob weist genannte **Datensätze** , die ein Array von Objekten Log enthält ein Stammobjekt

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Audit Log schema

| Namen            | Typ   | Beschreibung                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| Zeit            | Zeichenfolge | Der Zeitstempel (in UTC), der das Protokoll                                              |
| resourceId      | Zeichenfolge | Platzieren Sie auf die Nummer der Ressource, die Operation durchgeführt wurde                            |
| Kategorie        | Zeichenfolge | Die Kategorie Protokoll. Beispielsweise **Audit**.                                      |
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise Getfilestatus.              |
| ResultType-Wert      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| correlationId   | Zeichenfolge | Die Id für das Protokoll, das können verwendet werden, um eine Reihe von verknüpften Protokolleinträge zu gruppieren |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Details finden Sie unter                                                          |

#### <a name="audit-log-properties-schema"></a>Audit Log Eigenschaftsschema

| Namen       | Typ   | Beschreibung                              |
|------------|--------|------------------------------------------|
| StreamName | Zeichenfolge | Der Pfad der Vorgang wurde durchgeführt  |


## <a name="samples-to-process-the-log-data"></a>Beispiele zum Verarbeiten der Log-Daten

Azure Lake Datenspeicher enthält ein Beispiel zum Verarbeiten und protokollierten Daten zu analysieren. Sie können die Stichprobe am [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)suchen. 


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure Lake Datenspeicher](data-lake-store-overview.md)
- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)


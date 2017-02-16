<properties 
   pageTitle="Anzeigen von Diagnoseprotokollen für Azure Daten dem Analytics | Microsoft Azure" 
   description="Grundlagen Sie zum Einrichten und Diagnoseprotokolle für Azure Daten Lake Analytics zugreifen " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Zugreifen auf Diagnoseprotokolle für Azure Daten dem Analytics

Informationen Sie zu wie Diagnoseprotokoll für Ihr Konto Daten dem Analytics aktivieren und wie Sie die Protokolle für Ihr Konto erfassten anzeigen.

Organisationen können Diagnoseprotokoll für ihr Konto Azure Daten dem Analytics zum Sammeln von Daten Access Überwachungsprotokolle aktivieren. Diese Protokolle erhalten Sie Informationen wie:

* Eine Liste der Benutzer, die Zugriff auf die Daten.
* Wie häufig die Daten erfolgt.
* Wie viele Daten in das Konto gespeichert ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Abonnement Azure** für Daten dem Analytics Public Preview. [Anweisungen](data-lake-analytics-get-started-portal.md#signup)finden Sie unter.
- **Azure Daten dem Analytics-Konto**. Folgen Sie den Anweisungen bei [den ersten Schritten mit Azure Daten dem Analytics über das Azure-Portal](data-lake-analytics-get-started-portal.md)an.

## <a name="enable-logging"></a>Aktivieren der Protokollierung

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie Ihre Daten dem Analytics-Konto, und klicken Sie dann auf **Diagnoseeinstellungen**aus Ihrer Daten dem Analytics Konto Blade, klicken Sie auf **Einstellungen**.

3. Nehmen Sie in den **Diagnoseprotokollen** Blade die folgenden vor Diagnoseprotokoll konfigurieren.

    ![Diagnoseprotokollierung aktivieren] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

    * Festlegen Sie **Status** , **Klicken Sie auf** diagnoseprotokollierung aktivieren.
    * Sie können auch Store/die Daten auf zwei verschiedene Arten Prozess.
        * Wählen Sie **Exportieren nach Ereignis Hub** Log Daten als Stream an einen Hub an Azure Ereignis aus. Verwenden Sie diese Option, wenn Sie eine untergeordnete Verarbeitung Verkaufspipeline eingehende Protokolle in Echtzeit analysieren haben. Wenn Sie diese Option auswählen, müssen Sie die Details für das Ereignis Azure-Hub bereitstellen, die Sie verwenden möchten.
        * Wählen Sie zum Speichern von Protokollen mit einer Firma Azure-Speicher **Exportieren in Speicher-Konto** aus. Verwenden Sie diese Option, wenn Sie die Daten archivieren möchten. Wenn Sie diese Option auswählen, müssen Sie ein Konto Azure-Speicher, um die Protokolle speichern bereitstellen.
    * Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides erhalten möchten.
    * Geben Sie die Anzahl der Tage, für die die Daten erhalten bleiben müssen.
    * Klicken Sie auf **Speichern**.

Nachdem Sie diagnoseeinstellungen aktiviert haben, können Sie die Protokolle der Registerkarte **Diagnoseprotokolle** anschauen.

## <a name="view-logs"></a>Anzeigen von Protokollen

Es gibt zwei Methoden, um die Log-Daten für Ihr Konto Daten dem Analytics anzuzeigen.

* Über die Daten dem Analytics-kontoeinstellungen
* Aus dem Azure-Speicher-Konto, in dem die Daten gespeichert werden

### <a name="using-the-data-lake-analytics-settings-view"></a>Verwenden die Daten dem Analytics Einstellungen anzeigen

1. Klicken Sie aus Ihrer Daten dem Analytics Konto **Einstellungen** Blade auf **Diagnoseprotokolle**.

    ![Anzeigen von Diagnoseprotokollen Protokollierung] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Diagnoseprotokolle anzeigen") 

2. Das Blade **Diagnoseprotokolle** sollten Sie die Protokolle nach **Überwachungsprotokolle** und **Anfordern von Protokollen**kategorisierte angezeigt.
    * Anforderungsprotokolle erfassen jeder API Anforderung für die Daten dem Analytics-Konto an.
    * Überwachungsprotokolle ähneln anfordern Protokolle aber bieten eine viel ausführlichere Projektstrukturplan-Codes die Vorgänge für die Daten dem Analytics-Konto ausgeführt wird. Beispielsweise kann mehrere "Anfügen" Vorgänge in der Überwachungsprotokolle ein einzelnen Upload-API Anruf in Anforderungsprotokollen führen.

3. Klicken Sie auf den Link **herunterladen** , um einen Eintrag die Protokolle herunterladen.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Aus dem Azure-Speicher-Konto enthält, die Log-Daten

1. Öffnen Sie das Azure-Speicher Konto Blade zugeordneten Daten dem Analytics für Protokollierung, und klicken Sie dann auf Blobs. Der **Blob-Service** -Karte vorausgesetzt enthält zwei Container.

    ![Anzeigen von Diagnoseprotokollen Protokollierung] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnoseprotokolle anzeigen")

    * Der Container **Einsichten Protokolle Audit** enthält der Überwachungsprotokolle.
    * Die Container **Einsichten-Protokolle-Anfragen** enthält die Protokolle der Anfrage.

2. Innerhalb dieser Container werden die Protokolle unter den folgenden Struktur gespeichert.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] Die `##` Einträge in den Pfad enthalten, Jahr, Monat, Tag und Stunde, in dem das Protokoll erstellt wurde. Daten Lake Analytics erstellt eine Datei stündlich, also `m=` enthält immer einen Wert von `00`.

    Beispielsweise könnte der vollständige Pfad Überwachungsprotokoll:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Auf ähnliche Weise könnte der vollständige Pfad in ein Anforderungsprotokoll:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Log-Struktur

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
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
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise GetAggregatedJobHistory.              |
| ResultType-Wert      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| callerIpAddress | Zeichenfolge | Die IP-Adresse des Clients die Anforderung ausführenden                                |
| correlationId   | Zeichenfolge | Die Id des das Protokoll. Dieser Wert kann verwendet werden, um eine Reihe von verknüpften Protokolleinträge zu gruppieren |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Im nächsten Abschnitt (Anforderung Log Eigenschaftsschema) Weitere Informationen finden Sie unter |

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
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
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
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise JobSubmitted.              |
| ResultType-Wert | Zeichenfolge | Eine untergeordnete Status für den Projektstatus (OperationName). |
| resultSignature | Zeichenfolge | Weitere Informationen über den Projektstatus (OperationName). |
| Identität      | Zeichenfolge | Der Benutzer, der die angeforderte Operation. Beispielsweise susan@contoso.com.                                 |
| Eigenschaften      | JSON   | Finden Sie im nächste Abschnitt Details (Audit Log Eigenschaftsschema) |

> [AZURE.NOTE] __ResultType-Wert__ __ResultSignature__ finden Sie Informationen zu dem Ergebnis eines Vorgangs und nur einen Wert enthalten, wenn ein Vorgang abgeschlossen wurde. Angenommen, sie einen Wert enthalten Wenn __OperationName__ __JobStarted__ oder __JobEnded__Wert enthält.

#### <a name="audit-log-properties-schema"></a>Audit Log Eigenschaftsschema

| Namen       | Typ   | Beschreibung                              |
|------------|--------|------------------------------------------|
| Auftrags-IDs | Zeichenfolge | Die ID, die den Auftrag zugewiesen ist  |
| JobName | Zeichenfolge | Der Name, der für den Auftrag bereitgestellt wurde |
| JobRunTime | Zeichenfolge | Die Laufzeit verwendet, um den Auftrag zu verarbeiten |
| SubmitTime | Zeichenfolge | Die Uhrzeit (in UTC), die im Auftrag gesendet wurde. |
| Startzeit | Zeichenfolge | Die Uhrzeit, die der Auftrag gestartet nach dem Absenden (in UTC). |
| Endzeit | Zeichenfolge | Die Zeit, die das Projektende. |
| Parallelität | Zeichenfolge | Die Anzahl der Daten dem Analytics Einheiten angefordert für dieses Projekt beim Senden. |

> [AZURE.NOTE] __SubmitTime__, __Startzeit__, __Endzeit__ und __Parallelität__ finden Sie Informationen zu einem Vorgang, und nur einen Wert enthalten, wenn ein Vorgang gestartet oder abgeschlossen wurde. Beispielsweise enthält __SubmitTime__ einen Wert aus, nach __OperationName__ zeigt __JobSubmitted__an.

## <a name="process-the-log-data"></a>Die Log Daten nicht verarbeiten

Azure Daten dem Analytics enthält ein Beispiel zum Verarbeiten und protokollierten Daten zu analysieren. Sie können die Stichprobe am [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)suchen. 


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die Lake Analytics Azure-Daten](data-lake-analytics-overview.md)



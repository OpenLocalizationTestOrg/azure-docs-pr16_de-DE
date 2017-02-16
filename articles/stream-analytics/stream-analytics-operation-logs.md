<properties 
    pageTitle="Vorgang mit Debuggen und Dienstprotokolle in Stream Analytics | Microsoft Azure" 
    description="Verwenden von unterstützenden Stream Analytics Vorgang Protokolle" 
    keywords="von Dienstprotokollen"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debuggen Sie Stream Analytics-Aufträge mithilfe von Dienst und Betrieb von Protokollen

Alle Azure Dienste angeben Betrieb Protokollierungsnachrichten an Benutzer Datensatzdetails im Zusammenhang mit Management-Vorgänge. Diese Informationen können verwendet werden, für das Debuggen z. B. Anzeigen des Projektstatus, des Projektstatus, Azure Stream Analytics und Fehlermeldungen zu einer Position im Laufe der Zeit von überwachen Verarbeitung zur Ausgabe beginnen.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Suchen nach Vorgang im Verwaltungsportal Azure Protokolle

Vorgang Protokolle können Sie auf zwei Arten zugreifen:  

- Dashboard des Streams Analytics Auftrags  
- Management Services im klassischen Azure-Portal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard des Streams Analytics Auftrags

Klicken Sie auf die Position des Dashboard-Registerkarte wird eine Verknüpfung zu den entsprechenden Protokollen eines Auftrags Stream Analytics angezeigt. Wenn Sie auf diesen Link klicken, wird der Filter so festlegen, dass sie die neuesten Protokolle für diesen Auftrag zeigt.

  ![Wählen Sie Protokolle Management Services](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Management Services

Manuell zu den Vorgang Protokolle für Stream Analytics und andere Dienste im klassischen Azure-Portal navigieren:

1.  Klicken Sie auf **Management Services** im [klassischen Azure-Portal](https://manage.windowsazure.com).
2.  Wählen Sie **Stream Analytics** für **Typ** und den Namen des Projekts für **Service Name**aus.  

  ![Wählen Sie Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Suchen der Überwachungsprotokolle Azure-Portal ##

Um Betrieb Protokolle für den Job Stream Analytics Azure-Portal zu finden, klicken Sie auf **Durchsuchen** , und wählen Sie dann **Überwachungsprotokolle**.

  ![Wählen Sie Stream Analytics-Azure-portal](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Dadurch wird eine Blade Anzeigen von Ereignissen aus der letzten 7 Tage für alle Ressourcen in Ihrem Abonnement geöffnet.  Sie können filtern, um Ereignisse eines Typs angeben oder Zeitrahmens anzuzeigen, indem Sie auf den Befehl **Filtern** .

  ![Wählen Sie Stream Analytics-Azure-portal](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Hier erhalten Sie log

Sie können nach Zeitraums und Status zum Anzeigen der Protokolle für den Job filtern.

Klicken Sie im Azure-Verwaltungsportal auf die Schaltfläche " **Details** " am Fuß des Fensters, um weitere Details zu einem ausgewählten Ereignis anzeigen. 

  ![Wählen Sie Details aus.](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klicken Sie im Azure-Portal auf einen Eintrag in die darin enthaltenen detaillierte Ereignisse finden Sie unter.

  ![Azure-Portal Details auswählen](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Hier können Sie das **Details** Blade öffnen, indem Sie auf das Ereignis.

  ![Azure-Portal Details auswählen](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Einen fehlerhaften Auftrag Debuggen

Klicken Sie im Azure-Verwaltungsportal auf das Symbol suchen und Typ 'failed'. Dies ergibt aller Protokolle mit Fehlern. 

  ![Für das Debuggen eines fehlerhaften Auftrags](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Im Azure-Portal können Sie nach Ebene der Nachricht anzeigen **kritisch** Ereignisse filtern.

  ![Azure Portals Debuggen](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Sie können wählen Sie eine der Fehler aus, und klicken Sie auf die **Details** für Weitere Informationen zu diesem Fehler auf.  Einige Fehlermeldungen enthalten auch Informationen zum Beheben des Problems. 

  ![Vorgangsdetails](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Für den Fall, dass Sie sich an den [Support](https://azure.microsoft.com/support/options/) oder Informationen an das Team über das [Forum im MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)müssen, beachten Sie die Details des Vorgangs, insbesondere hat die **Korrelations-ID**. 

## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

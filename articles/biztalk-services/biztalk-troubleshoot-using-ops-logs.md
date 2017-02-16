<properties 
    pageTitle="Behandeln von Problemen mit BizTalk-Dienste verwenden Vorgang Protokolle | Microsoft Azure" 
    description="Behandeln von Problemen mit BizTalk-Dienste Vorgang Protokolle verwenden. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk-Dienste: Problembehandlung bei der Verwendung von Vorgang Protokolle

## <a name="what-are-the-operation-logs"></a>Was sind die Protokolle der Vorgang
Vorgang Protokolle handelt es sich um ein Feature Management Services im Azure klassischen-Portal an, die Sie anzeigen zurückliegenden Protokolle von Vorgängen auf Ihre Azure Dienste, einschließlich der BizTalk-Dienste ausgeführt werden können. So können Sie zurückliegende Daten, die im Zusammenhang mit Management Operationen für Ihr Abonnement BizTalk Service bis zurück zum 180 Tage angezeigt.

> [AZURE.NOTE]Dieses Feature erfasst werden nur die Protokolle für Management Vorgänge BizTalk-Dienste, wie z. B., wenn der Dienst gestartet wurde, gesicherten bis usw.. Solche Vorgänge werden nachverfolgt, unabhängig davon, ob sie vom klassischen Azure-Portal oder mithilfe der [REST-APIs von BizTalk Dienst](http://msdn.microsoft.com/library/azure/dn232347.aspx)ausgeführt werden. Eine vollständige Liste der Vorgänge, die mit Management Services erfasst werden, finden Sie unter [Vorgänge nachverfolgten mithilfe von Azure Management Services](#bizops).<br/><br/>
Dies erfasst nicht die Protokolle für Aktivitäten im Zusammenhang mit BizTalk Service-Laufzeit (z. B. Nachricht Brücken usw. verarbeiteten.). Verwenden Sie zum Anzeigen dieser Protokolle der Ansicht "Verlauf" aus dem Portal BizTalk-Dienste. Weitere Informationen finden Sie unter [Nachrichten nachverfolgen](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Anzeigen von BizTalk Vorgang Protokolle Services
1. Klicken Sie im Portal Azure klassischen wählen Sie **Management Services**, und wählen Sie dann auf die Registerkarte **Vorgang Protokolle** .
2. Sie können die Protokolle basierend auf verschiedenen Parametern wie Abonnement, Datumsbereich, Diensttyp (z. B. BizTalk Services), Dienst- oder Status des Vorgangs (erfolgreich, fehlgeschlagen) filtern.
3. Wählen Sie das Kontrollkästchen zum Anzeigen der gefilterten Liste aus. Die folgende Abbildung zeigt die Aktivitäten in Bezug auf Testbiztalkservice:  ![Vorgang Protokolle anzeigen][ViewLogs] 
4. Um weitere Informationen zu einem bestimmten Vorgang anzeigen möchten, markieren Sie die Zeile, und klicken Sie in der Taskleiste unten auf **Details** .


## <a name="a-namebizopsaoperations-tracked-using-azure-management-services"></a><a name="bizops"></a>Vorgänge mit Azure Management Services erfasst
Die folgende Tabelle enthält die Vorgänge, die mit der Azure Management Services erfasst werden:

Name des Vorgangs | Aufgabe
--- | ---
CreateBizTalkService | Der Vorgang zum Erstellen einer neuen BizTalk Service
DeleteBizTalkService | Vorgang zum Löschen einer BizTalk Service
RestartBizTalkService | Vorgang, eine BizTalk Service neu zu starten.
StartBizTalkService | Für den Beginn eines BizTalk Service
StopBizTalkService | So beenden Sie eine BizTalk Service Vorgang
DisableBizTalkService | So deaktivieren Sie eine BizTalk Service Vorgang
EnableBizTalkService | Vorgang an einem BizTalk Service aktivieren
BackupBizTalkService | Sichern einer BizTalk Service Vorgang
RestoreBizTalkService | Eine BizTalk Service aus der angegebenen Sicherung Wiederherstellung
SuspendBizTalkService | Vorgang unterbrechen eines BizTalk Service
ResumeBizTalkService | Der Vorgang zum Fortsetzen eines BizTalk Service
ScaleBizTalkService | Vorgang an einem BizTalk Service nach oben oder unten skalieren
ConfigUpdateBizTalkService | Aktualisieren Sie die Konfiguration von einer BizTalk Service Vorgang
ServiceUpdateBizTalkService | Der Vorgang zum Aktualisieren oder Herabstufen einer BizTalk Service auf eine andere version
PurgeBackupBizTalkService | Vorgang an der BizTalk Service außerhalb der Aufbewahrungszeitraum Sicherungen bereinigen


## <a name="see-also"></a>Siehe auch
- [Sichern der BizTalk-Dienst](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Stellen Sie BizTalk-Dienst aus einer Sicherung wieder her](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium-Editionen Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk-Dienste: Provisioning Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk-Dienste: Begrenzungsebene](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 

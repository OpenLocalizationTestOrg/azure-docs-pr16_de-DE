<properties 
    pageTitle="Aufgaben, die in verschiedenen Bundesländern oder Status BizTalk-Dienste zulässig | Microsoft Azure" 
    description="Die Aktionen/Vorgänge in anderen MABS Status zulässig: beenden, starten, neu starten, anzuhalten, fortzusetzen, löschen, skalieren, aktualisieren Konfiguration und Sichern von" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk-Dienste: Dienst Zustand Diagramm
Je nach den aktuellen Status des Diensts BizTalk gibt es Vorgänge, die Sie können oder BizTalk-Dienst nicht möglich.

Angenommen, bereitgestellt in der klassischen Azure-Portal einen neuen BizTalk-Dienst. Wenn es erfolgreich abgeschlossen wurde, wird der BizTalk-Dienst Status "aktiv". In den Zustand Aktiv können Sie den BizTalk-Dienst beenden. Wenn beenden erfolgreich ist, geht der BizTalk-Dienst in einem angehaltenen Zustand. Wenn beenden fehlschlägt, geht der BizTalk-Dienst in einem StopFailed Zustand. In den Zustand StopFailed können Sie den BizTalk-Dienst neu starten. Wenn Sie einen Vorgang, die nicht zulässig ist versuchen, tritt wie Lebenslauf Dienst BizTalk folgender Fehler auf:

**Vorgang nicht zulässig.**

Um einen BizTalk-Service bereitstellen möchten, wechseln Sie zu [BizTalk-Dienste: klassischen Provisioning verwenden Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Den folgenden Tabellen sind die Vorgänge oder Aktionen, die durchgeführt werden können, wenn der BizTalk Service in einem bestimmten Zustand befindet. Ein ✔ bedeutet, dass der Vorgang in dieser Zustand zulässig ist. Ein leerer Eintrag bedeutet, dass der Vorgang in dieser Zustand ausgeführt werden kann.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Starten Beenden Sie, und starten Sie, unterbrechen, Lebenslauf, Löschvorgängen
<table border="1">
<tr>
        <th colspan="15">Vorgang oder eine Aktion</th>
</tr>

<tr>
        <th rowspan="18">BizTalk-Dienststatus</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Starten</th>
        <th>Beenden</th>
        <th>Neu starten</th>
        <th>Anhalten</th>
        <th>Lebenslauf</th>
        <th>Löschen</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktive</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Deaktiviert</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Unterbrochen</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Beendet</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service Update fehlgeschlagen ist</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Dezimalstellen, Aktualisierungskonfiguration und zusätzliche Vorgänge
<table border="1">
<tr>
        <th colspan="15">Vorgang oder eine Aktion</th>
</tr>

<tr>
        <th rowspan="18">BizTalk-Dienststatus</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Skalieren</th>
        <th>Aktualisieren der Konfiguration</th>
        <th>Sicherung</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktive</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Deaktiviert</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Unterbrochen</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Beendet</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service Update fehlgeschlagen ist</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium-Editionen Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Dienste: Sichern und Wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Begrenzungsebene](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 

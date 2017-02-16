<properties 
    pageTitle="Erstellen, und stellen Sie eine Sicherungskopie BizTalk-Dienste | Microsoft Azure" 
    description="BizTalk-Dienste enthält sichern und wiederherstellen. Informationen Sie zum Erstellen und Wiederherstellen einer Sicherung und bestimmen Sie, welche gesichert wird. MABS, WABS" 
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


# <a name="biztalk-services-backup-and-restore"></a>BizTalk-Dienste: Sichern und Wiederherstellen

Azure BizTalk Services enthält Funktionen, die sichern und wiederherstellen. In diesem Thema beschrieben, wie Sicherung und Wiederherstellung über das klassische Azure-Portal BizTalk-Dienste.

Sie können auch mithilfe der [BizTalk Services REST-API](http://go.microsoft.com/fwlink/p/?LinkID=325584)BizTalk-Dienste sichern. 

> [AZURE.NOTE] Hybrid-Verbindungen werden nicht unabhängig von der Edition gesichert. Sie müssen Ihre Verbindungen Hybrid neu erstellen.

## <a name="before-you-begin"></a>Bevor Sie beginnen

- Sichern und Wiederherstellen möglicherweise nicht zur Verfügung für alle Editionen. Finden Sie unter [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md).

- Mit der klassischen Azure-Portal an, können Sie eine Sicherung On Demand erstellen oder eine geplante Sicherung erstellen. 

- Zusätzliche Inhalte kann in der gleichen BizTalk Service oder in einen neuen BizTalk Service wiederhergestellt werden. Zum Wiederherstellen der BizTalk Service denselben Namen der vorhandenen BizTalk Service gelöscht werden müssen, und der Name muss verfügbar sein. Nachdem Sie einen BizTalk-Service gelöscht haben, kann dies länger als für den gleichen Namen verfügbar sein sollte dauern. Wenn Sie den bestehenden Namen zur Verfügung warten können, klicken Sie dann in einen neuen BizTalk Service wiederherstellen.

- BizTalk-Dienste können auf die gleiche Edition oder eine höhere Edition wiederhergestellt werden. Wiederherstellen der BizTalk-Dienste auf eine niedrigere Edition, wird aus, wenn die Sicherung durchgeführt wurde, nicht unterstützt.

    Beispielsweise kann eine Sicherung mit der Basisversion zur Premium Edition wiederhergestellt werden. Eine Sicherung mit der Premium Edition kann nicht auf Standard Edition wiederhergestellt werden.

- Die EDI-Steuerelement Zahlen werden kontinuierlichen Steuerelement Zahlen verwalten gesichert. Wenn nach der letzten Sicherung Nachrichten verarbeitet wurden, kann die dieses Inhaltstyps Sicherungsdatei wiederherstellen Zahlen doppelter Steuerelement verursachen.

- Wenn ein Stapel aktive Nachrichten aufweist, verarbeitet die Stapel **vor** eine Sicherung ausführen. Beim Erstellen einer Sicherung (als erforderlich oder geplanten), werden Nachrichten in Stapeln nie gespeichert. 

    **Wenn eine Sicherung mit aktiven Nachrichten in einem Stapel geöffnet ist, wird diese Nachrichten werden nicht gesichert und können daher gehen verloren.**

- Optional: Im Portal BizTalk, beenden Sie alle Vorgänge Management.


## <a name="create-a-backup"></a>Erstellen Sie eine Sicherungskopie

Eine Sicherung zu einem beliebigen Zeitpunkt geöffnet werden kann und von Ihnen vollständig gesteuert. Dieser Abschnitt listet die Schritte zum Erstellen von Sicherungskopien mithilfe des Azure klassischen Portals einschließlich:

[Klicken Sie auf bei Bedarf Sicherung](#backupnow)

[Planen einer Sicherung](#backupschedule)

#### <a name="a-namebackupnowaon-demand-backup"></a><a name="backupnow"></a>Klicken Sie auf bei Bedarf Sicherung
1. Im Portal Azure klassischen **BizTalk-Dienste**wählen Sie aus, und wählen Sie die gewünschte BizTalk Service sichern.
2. Wählen Sie die Registerkarte **Dashboard** am unteren Rand der Seite **Sichern** .
3. Geben Sie einen Namen der Sicherungsdatei. Geben Sie zum Beispiel *MyBizTalkService*Business Unit*Datum*.
4. Wählen Sie ein Blob-Speicher-Konto aus, und wählen Sie das Prüfzeichen in die Sicherung zu starten.

Sobald die Sicherung abgeschlossen ist, wird ein Container mit der Name der Sicherungskopie, die Sie eingeben im Speicherkonto erstellt. Dieser Container enthält die Sicherung BizTalk Service-Konfiguration.

#### <a name="a-namebackupscheduleaschedule-a-backup"></a><a name="backupschedule"></a>Planen einer Sicherung

1. Klicken Sie im Portal Azure klassischen wählen Sie **BizTalk-Dienste**, wählen Sie den BizTalk Service-Namen, den Sie die Sicherung planen möchten, und wählen Sie dann auf die Registerkarte **Konfigurieren** .
2. Legen Sie die **Sicherung Status** auf **automatisch**. 
3. Wählen Sie das **Konto Speicher** die Sicherung speichern, geben die **Häufigkeit** , um die Sicherungskopien erstellen und wie lange die Sicherungskopien (**Aufbewahrung Tage**) gespeichert:

    ![][AutomaticBU]

    **Notizen**   
    - In **Aufbewahrung Tage**muss der Aufbewahrungszeitraum größer als die Sicherung Häufigkeit.
    - Wählen Sie **mindestens eine Sicherung immer verfügbar**, selbst wenn sie ältere der Aufbewahrungszeitraum ist.
    

4. Wählen Sie **Speichern**aus.


Wenn eine geplante Aufträge ausgeführt wird, wird im Speicherkonto eingegebene ein Containers (um zusätzliche Daten speichern) erstellt. Der Name des Containers heißt *Name-Datum-/ Uhrzeit-BizTalk-Dienst*. 

Wenn das Dashboard BizTalk Service Status **fehlgeschlagen** angezeigt wird:

![Status der letzten geplanten Sicherung][BackupStatus] 

Der Link öffnet die Protokolle Management Services Vorgang, um die Behandlung. Finden Sie unter [BizTalk-Dienste: Problembehandlung bei der Verwendung von Vorgang Protokolle](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Stellen Sie wieder her

Sie können Sicherungskopien vom klassischen Azure-Portal oder die [REST-API für BizTalk Dienst wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=325582)wiederherstellen. Dieser Abschnitt listet die Schritte zum Verwenden des klassischen Portals wiederherstellen.

#### <a name="before-restoring-a-backup"></a>Vor dem Wiederherstellen einer Sicherung

- Neue Überwachung und Archivierung Stores Überwachung können beim Wiederherstellen einer BizTalk Service eingegeben werden.

- EDI-Laufzeit dieselben Daten werden wiederhergestellt. Die Sicherung EDI-Laufzeit speichert die Kontrolle Zahlen. Die wiederhergestellte Steuerelement Zahlen sind nacheinander ab dem Zeitpunkt der Sicherung. Wenn nach der letzten Sicherung Nachrichten verarbeitet wurden, kann die dieses Inhaltstyps Sicherungsdatei wiederherstellen Zahlen doppelter Steuerelement verursachen.

#### <a name="restore-a-backup"></a>Stellen Sie eine Sicherung wieder her.

1. Wählen Sie im Portal Azure klassischen **neu** > **App Services** > **BizTalk-Dienst** > **wiederherzustellen**:

    ![Stellen Sie eine Sicherung wieder her.][Restore]

2. **Zusätzliche URL**wählen Sie das Ordnersymbol, und Erweitern des Azure-Speicher-Kontos, in dem Sicherungskopie der BizTalk Service-Konfiguration gespeichert. Erweitern Sie den Container, und wählen Sie im rechten Bereich das entsprechende Sichern in TXT-Datei. 
<br/><br/>
Klicken Sie auf **Öffnen**.

3. Klicken Sie auf der Seite **BizTalk-Dienst wiederherstellen** Geben Sie einen **Namen der BizTalk-Dienst** , und überprüfen Sie die **Domänen-URL**, die **Edition**und die **Region** für den wiederhergestellten BizTalk Service. **Erstellen Sie eine neue Instanz von SQL-Datenbank** für die Datenbank Verlauf:

    ![][RestoreBizTalkService]

    Wählen Sie den nächsten Pfeil.

4.  Überprüfen Sie den Namen der SQL-Datenbank aus, geben Sie den physischen Server, in die SQL-Datenbank erstellt werden soll, und einen Benutzernamen und Kennwort für diesen Server.


    Wenn Sie die SQL-Datenbank-Edition konfigurieren möchten, wählen Sie Größe und andere Eigenschaften **Konfigurieren erweiterte Database Settings**. 

    Wählen Sie den nächsten Pfeil.

5. Erstellen eines neuen Kontos mit Speicher, oder geben Sie ein vorhandenes Speicherkonto für den BizTalk Service.

7. Wählen Sie das Häkchen neben der Wiederherstellung zu beginnen.

Sobald die Wiederherstellung erfolgreich abgeschlossen wurde, wird ein neuer BizTalk Service Status "gesperrt" auf der Seite BizTalk-Dienste in der klassischen Azure-Portal aufgeführt.



### <a name="a-namepostrestoreaafter-restoring-a-backup"></a><a name="postrestore"></a>Nach dem Wiederherstellen einer Sicherung

Der BizTalk Service wird immer in einem Status " **angehalten** " wiederhergestellt werden. In diesem Zustand können Sie Änderungen Konfiguration vornehmen, bevor die neue Umgebung funktionsfähig, einschließlich ist:

- Wenn Sie BizTalk-Service-Applications mit Azure BizTalk-Dienste SDK erstellt haben, müssen Sie die Access-Steuerelement (ACS) Anmeldeinformationen in diesen Programmen für die Arbeit mit den wiederhergestellten Umgebung aktualisieren zu.

- Sie Wiederherstellen einer BizTalk Service um eine vorhandene BizTalk Service-Umgebung repliziert. In diesem Fall befinden sich im ursprünglichen BizTalk-Portal konfiguriert Vereinbarungen, die einen Datenquelle FTP-Ordner verwenden müssen Sie in der Umgebung neu wiederhergestellte mit einer anderen Quelle FTP-Ordner aktualisieren. Andernfalls können zwei verschiedenen Vereinbarungen versuchen, dieselbe Nachricht herauszuziehen vorhanden sein.

- Wenn Sie wiederhergestellt, damit mehrere BizTalk Service-Umgebungen, stellen Sie sicher, dass Sie die richtige Umgebung in der Visual Studio-Anwendungen, PowerShell-Cmdlets, REST-APIs oder Trading Partner Management OM-APIs Zielgruppen.

- Es empfiehlt sich, automatisierte Sicherungskopien auf die neu wiederhergestellte BizTalk Service-Umgebung konfigurieren.

Um die BizTalk Service im klassischen Azure-Portal zu starten, wählen Sie den wiederhergestellten BizTalk Service aus, und wählen Sie **Lebenslauf** , in der Taskleiste. 



## <a name="what-gets-backed-up"></a>Was gesichert wird

Wenn Sie eine Sicherungskopie erstellt wurde, werden die folgenden Elemente gesichert:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Elemente gesichert</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Konfiguration und Laufzeit</td> 
<td>
<ul>
<li>Partner und Profil-details</li>
<li>Partner Agreements</li>
<li>Benutzerdefinierte Assemblys bereitgestellt</li>
<li>Brücken bereitgestellt</li>
<li>Zertifikate</li>
<li>Sie können bereitgestellt</li>
<li>Pipelines</li>
<li>Vorlagen erstellt und im Portal BizTalk gespeichert</li>
<li>X12 ST01 und GS01 Zuordnungen</li>
<li>Steuerelement Zahlen (EDI)</li>
<li>AS2 Nachricht Mikrofon Werte</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-Dienst</strong></td>
</tr> 
<tr>
<td>SSL-Zertifikat</td> 
<td>
<ul>
<li>SSL-Zertifikat-Daten</li>
<li>SSL Zertifikatkennwort</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk Diensteinstellungen</td> 
<td>
<ul>
<li>Anzahl von Dezimalstellen Einheit</li>
<li>Edition</li>
<li>Version des Produkts</li>
<li>Region/Datacenter</li>
<li>Access Control Service (ACS) Namespace und Schlüssel</li>
<li>Nachverfolgen der Verbindungszeichenfolge für die Datenbank</li>
<li>Archivierung Verbindungszeichenfolge für Speicher-Konto</li>
<li>Überwachen der Verbindungszeichenfolge für Speicher-Konto</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Weitere Elemente</strong></td>
</tr> 
<tr>
<td>Nachverfolgen der Datenbank</td> 
<td>Beim Erstellen der BizTalk Service sind die Angaben zur Überwachung eingegeben haben, einschließlich der Azure SQL-Datenbankserver und den Datenbanknamen Überwachung an. Die Überwachungsdatenbank wird nicht automatisch gesichert.
<br/><br/>
<strong>Wichtige</strong><br/>
Wenn die Überwachungsdatenbank gelöscht wird und der Datenbank muss wiederhergestellt, muss eine vorhandene Sicherungskopie vorhanden sein. Wenn eine Sicherung nicht vorhanden ist, können die Datenbank zum Nachverfolgen und deren Daten nicht wiederhergestellt werden. Erstellen Sie in diesem Fall eine neue Überwachung-Datenbank mit dem gleichen Datenbanknamen aus. Geo-Replikation wird empfohlen.</td>
</tr> 
</table>

## <a name="next"></a>Weiter

Um im klassischen Azure-Portal Azure BizTalk-Dienste zu erstellen, wechseln Sie zu [BizTalk-Dienste: klassischen Provisioning verwenden Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Zum Erstellen von Applications beginnen möchten, wechseln Sie zu [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

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

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 

<properties
    pageTitle="Erstellen der Wiederherstellung Pläne | Microsoft Azure" 
    description="Erstellen Sie Wiederherstellung Pläne Azure Website Wiederherstellung über fehlschlagen und Wiederherstellen von virtuellen Computern und physischen Servern Gruppen." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Erstellen der Wiederherstellung Pläne

Der Dienst Azure Website Wiederherstellung beiträgt zu Ihrer Strategie Business Continuity- und Disaster Wiederherstellung (BCDR) durch Replikation, Failover und Wiederherstellung von virtuellen Computern und physischen Servern orchestriert. Maschinen können Azure oder einem sekundären lokalen Data Center repliziert werden. Für einen schnellen Überblick lesen [Neuigkeiten Azure Website Wiederherstellung?](site-recovery-overview.md).


## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält Informationen zum Erstellen und Anpassen der Wiederherstellung Pläne. 

Wiederherstellung Pläne bestehen aus einer oder mehreren Ziffern oder Buchstaben sortierte Gruppen, die enthalten virtuellen Computern oder physischen Server, denen über zusammen fehlschlagen soll. Wiederherstellung Pläne folgendermaßen vor:

- Definieren von Gruppen von Computern, die über fehl, und beginnen Sie von zusammen.
- Modell Abhängigkeiten zwischen Computern, indem Sie sie in der Gruppe Plan Wiederherstellung gruppieren. Für Beispiel, wenn Sie über fehl, und eine bestimmte Anwendung erfassenden Sie möchten die virtuellen Computer für die Anwendung in derselben Gruppe Wiederherstellung Plan gruppieren möchten.
- Automatisieren und Failover zu erweitern. Sie können einem Test, geplant, oder einer ungeplanten Failover auf einen Wiederherstellungsplan ausführen. Sie können die Wiederherstellung Pläne mit Skripts, Azure Automatisierung und manuelle Aktionen anpassen.

Wiederherstellung Pläne werden auf der **Wiederherstellung Pläne** im Portal Wiederherstellung der Website angezeigt.


Posten Sie Kommentare oder Fragen am Ende dieses Artikels oder im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Bevor Sie beginnen

Beachten Sie Folgendes:

- Ein Wiederherstellungsplan dürfen nicht virtuellen Computern mit einzelner und mehrerer Netzwerkadapter kombiniert werden. Dies ist, da diese virtuellen Computern mischen in einer Azure-Cloud-Dienst nicht zulässig ist.
- Sie können die Wiederherstellung Pläne mit Skripts und manuelle Aktionen erweitern. Beachten Sie Folgendes:
    - Mithilfe der Windows PowerShell Skripts schreiben.
    - Stellen Sie sicher, dass Skripts Try / Catch-Blöcke verwenden, damit die Ausnahmen ordnungsgemäß verarbeitet werden. Es ist eine Ausnahme im Skript es beendet, und die Aufgabe wird als fehlerhaft.  Wenn ein Fehler auftritt, wird keine verbleibenden Teil des Skripts ausgeführt. In diesem Fall beim Ausführen eines ungeplanten Failovers weiterhin der Wiederherstellung planen. In diesem Fall, wenn Sie ein geplantes Failover ausgeführt werden, wird der Wiederherstellungsplan beenden. In diesem Fall korrigieren Sie das Skript, stellen Sie sicher, dass es erwartungsgemäß ausgeführt wird, und führen Sie den Wiederherstellungsplan erneut aus.
    - Der Befehl schreiben-Host funktioniert nicht in einem Wiederherstellung Plan Skript, und das Skript schlägt fehl. Wenn Sie die Ausgabe erstellen möchten, erstellen Sie ein Proxyskript, die wiederum Ihr Hauptfenster Skript ausgeführt wird, und stellen Sie sicher, dass alle Ausgabe mit geleitet wird die >> Befehl.
    - Das Skript Timeout erreicht werden, wenn sie nicht innerhalb von 600 Sekunden zurückgibt.
    - Wenn Sie etwas in STDERR geschrieben ist, wird das Skript klassifiziert werden als fehlerhaft. Diese Informationen werden die Details für das Skript Ausführung angezeigt werden.
    - Beachten Sie, dass, wenn Sie in der Bereitstellung VMM verwenden:

        - Skripts in einem Wiederherstellungsplan im Kontext des Kontos VMM-Dienst ausgeführt. Stellen Sie sicher, dass dieses Konto Leseberechtigungen auf Freigabe auf dem Remotecomputer hat, auf dem sich das Skript befindet, und Testen Sie das Skript Ebene Recht Konto der VMM-Dienst ausgeführt.
        - VMM-Cmdlets sind in einem Windows PowerShell-Modul übermittelt. Das VMM Windows PowerShell-Modul wird bei der Installation der VMM-Konsole installiert. Das Modul VMM in Ihr Skript mit dem folgenden Befehl im Skript geladen werden kann: Import-Module-Namen Virtualmachinemanager. [Weitere Details](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Stellen Sie sicher, dass Sie mindestens eine Bibliotheksserver Ihrer VMM-Bereitstellung haben. Standardmäßig befindet der Bibliothek freigeben Pfad für eine VMM-Server lokal auf dem VMM-Server mit den Namen des Ordners MSCVMMLibrary.
        - Ist Ihre Bibliothek freigeben Pfad remote (oder lokale aber nicht freigegeben MSCVMMLibrary, konfigurieren Sie die Freigabe wie folgt (mit \\libserver2.contoso.com\share\ als Beispiel):
            - Öffnen Sie den Registrierungs-Editor, und navigieren Sie zu HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Website Recovery\Registration.
            -  Bearbeiten Sie den Wert ScriptLibraryPath, und setzen Sie den Wert als \\libserver2.contoso.com\share\. die vollständige vollqualifizierten Domänennamen an. Bereitstellen von Berechtigungen für den freigegebenen Speicherort.
            -  Stellen Sie sicher, dass Sie das Skript mit einem Benutzerkonto zu, die weist dieselben Berechtigungen wie die VMM Dienstkontos testen, um sicherzustellen, dass eigenständig getesteten Skripts auf die gleiche Weise ausführen, dass sie im Wiederherstellung Pläne werden. Legen Sie auf dem Server VMM die Ausführungsrichtlinie umgehen wie folgt aus:
                -  Öffnen Sie die 64-Bit-Windows PowerShell-Konsole erhöhten mithilfe.
                -  Type: **Set-Executionpolicy umgehen**. [Weitere Details](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Erstellen Sie einen Wiederherstellungsplan

Die Möglichkeit, in der Sie einen Wiederherstellungsplan erstellen, hängt von der Website Wiederherstellung Bereitstellung ab.

- **Virtuelle VMware-Computer repliziert und physische Server**– Sie einen Plan erstellen und Hinzufügen von Replikationsgruppen, die virtuellen Computern und physischen Servern zu einem Wiederherstellungsplan enthalten.
- **Replikation-Hyper-V virtuellen Computern (in VMM Wolken)**– Sie erstellen Sie einen Plan und geschützten Hyper-V virtuelle Computer aus einer VMM Cloud zu einem Wiederherstellungsplan hinzufügen.
- **Replikation-Hyper-V virtuelle Computer (nicht in VMM Wolken)**– erstellen Sie einen Plan und Hinzufügen von geschützten Hyper-V virtuellen Computern aus einer Gruppe "Schutz" zu einem Wiederherstellungsplan.
- **SAN-Replikation**– erstellen Sie einen Plan aus, und fügen Sie eine Replikation hinzu, die virtuellen Computern, die dem Wiederherstellungsplan enthält. Sie einer Replikationsgruppe statt bestimmte virtuelle Computer auswählen, da alle virtuellen Computern in einer Replikationsgruppe über zusammen ein Fehler auftreten, müssen (Failover erfolgt zu den Layer Speicher zuerst).


Erstellen eines Wiederherstellungsplans wie folgt aus:

1. Klicken Sie auf die Registerkarte **Wiederherstellung Pläne** > **Planen der Wiederherstellung erstellen**.
Geben Sie einen Namen für den Wiederherstellungsplan und einer Quell- und Zielwebsites. Der Server für die Datenquelle müssen virtuellen Computern, die für Failover und Wiederherstellung aktiviert sind.

    - **Quellentyp**auswählen, ob Sie von VMM VMM repliziert sind > **VMM**und den Quell- und Zielwebsites VMM-Servern. Klicken Sie auf **Hyper-V** , um Wolken anzuzeigen, für die Verwendung von Hyper-V Replica konfiguriert sind. 
    - Wenn Sie VMM SAN select **Quellentyp**mithilfe von VMM repliziert sind > **VMM**und den Quell- und Zielwebsites VMM-Servern. Klicken Sie auf **SAN** um Wolken anzuzeigen, die für die Replikation SAN konfiguriert werden.
    - Wenn Sie VMM Replikation von auf Azure sind **Quellentyp**auswählen > **VMM**.  Wählen Sie die Quelle VMM-Server und **Azure** als Ziel ein.
    - **Quellentyp**auswählen, ob Sie von einer Website Hyper-V repliziert sind > **Hyper-V-Website**. Wählen Sie die Website als Quell- und **Azure **als Ziel ein.
    - Wenn Sie von VMware- oder eine lokale physische Server in Azure repliziert, wählen Sie einen Konfigurationsserver als Quell- und **Azure** als Ziel

2. Wählen Sie unter **Wählen Sie virtuellen Computern** der virtuellen Computern (oder Replikation), die Sie in die entsprechende Planung der Standardgruppe (Gruppe 1) hinzufügen möchten.

## <a name="customize-recovery-plans"></a>Anpassen der Wiederherstellung Pläne

Nachdem Sie hinzugefügt haben, geschützten virtuellen Computern oder Replikationsgruppen Wiederherstellung Plan Standardgruppe und erstellt den Plan, dass Sie ihn anpassen können:

- **Hinzufügen neuer Gruppen**– können Sie zusätzliche Wiederherstellung Plangruppen hinzufügen. Gruppen, die Sie hinzufügen werden in der Reihenfolge nummeriert, in denen Sie hinzufügen. Sie können bis zu sieben Gruppen hinzufügen. Sie können diesen neuen Gruppen weitere Rechner oder Replikationsgruppen hinzufügen. Beachten Sie, dass ein virtuellen Computern oder eine Replikationsgruppe nur in eine Wiederherstellung Plan Gruppe einbezogen werden kann.
- **Hinzufügen eines Skripts **– Sie können die Skripts, die vor oder nach einer Wiederherstellung Planen der Gruppe hinzufügen. Wenn Sie ein Skript hinzufügen, fügt es ein neuer Satz von Aktionen für die Gruppe hinzu. Beispielsweise wird eine Reihe von Pre Schritte für Gruppe 1 mit dem Namen erstellt: Gruppe 1: Schritten vor. Innerhalb dieser Satz werden alle Pre Schritte aufgelistet. Beachten Sie, dass Sie nur ein Skript auf dem primären Standort hinzufügen können, wenn Sie einen VMM Server bereitgestellt haben.
- **Hinzufügen eine manuelle Aktion**– Sie können manuelle Aktionen, die ausgeführt vor oder nach einer Wiederherstellung Plan Gruppe werden hinzufügen. Wenn Sie der Wiederherstellungsplan ausgeführt wird, stoppt an der Stelle, an der Sie die manuelle Aktion eingefügt, und ein Dialogfeld aufgefordert, die angeben, dass die manuelle Aktion abgeschlossen wurde.

## <a name="extend-recovery-plans-with-scripts"></a>Erweitern der Wiederherstellung Pläne mit Skripts

Sie können Ihr Plan für die Wiederherstellungsdatei ein Skript hinzufügen:

- Wenn Sie eine Quellwebsite VMM können Sie ein Skript auf dem Server VMM erstellen und in Ihr Wiederherstellungsplan einzuschließen haben.
- Wenn Sie in Azure repliziert sind, können Sie Azure Automatisierung Runbooks in Ihr Wiederherstellungsplan integrieren

### <a name="create-a-vmm-script"></a>Erstellen Sie ein VMM-Skript


Erstellen Sie das Skript wie folgt ein:

1. Erstellen eines neuen Ordners in der Bibliothek freigeben, beispielsweise \<VMMServerName > \MSSCVMMLibrary\RPScripts. Legen Sie es auf der Quelle und Ziel VMM-Server.
2. Erstellen Sie das Skript (beispielsweise RPScript), und überprüfen Sie, dass es wie erwartet funktioniert.
3. Legen Sie das Skript an der Stelle \<VMMServerName > \MSSCVMMLibrary auf den Quell- und Zielwebsites VMM-Servern.

### <a name="create-an-azure-automation-runbook"></a>Erstellen einer Azure Automatisierung Runbooks

Sie können Ihr Wiederherstellungsplan erweitern, indem Sie eine Azure Automatisierung Runbooks als Teil der Plan ausgeführt. [Weitere Informationen finden](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Hinzufügen von benutzerdefinierten Einstellungen zu einem Wiederherstellungsplan

1. Öffnen Sie den Wiederherstellungsplan, die, den Sie anpassen möchten.
2. Durch Klicken hinzufügen einer virtuellen Computern oder einer neuen Gruppe.
3. Hinzufügen einer oder eine manuelle Aktion klicken Sie auf ein beliebiges Element in der Liste **Schritt** , und klicken Sie dann auf **Skript** oder die **Manuelle Aktion**. Geben Sie an, ob das Skript oder die Aktion, die vor oder hinter dem ausgewählten Element hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten** , um die Position des Skripts nach oben oder unten verschieben.
4. Wenn Sie ein VMM Skript hinzufügen möchten, wählen Sie **Failover auf VMM Skript**, und geben Sie in **Pfad** des relativen Pfads für die Freigabe. In diesem Fall für Beispiel, in dem die Freigabe sich unter befindet \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, geben Sie den Pfad: \RPScripts\RPScript.PS1.
5. Wenn Sie eine Azure Automatisierung ausführen Adressbuch hinzufügen möchten, geben Sie das **Azure Automatisierung-Konto** , in dem sich die Runbooks befindet, und wählen Sie das entsprechende **Azure Runbooks Skript**.
5. Führen Sie ein Failover für den Wiederherstellungsplan, um sicherzustellen, dass das Skript wie erwartet funktioniert.


## <a name="next-steps"></a>Nächste Schritte

Sie können verschiedene Arten von Failovers Wiederherstellung Plan, einschließlich eines Test Failovers zum Überprüfen Ihrer Umgebung und der geplanten oder ungeplanten Failovers ausführen. [Erfahren Sie mehr](site-recovery-failover.md).


 

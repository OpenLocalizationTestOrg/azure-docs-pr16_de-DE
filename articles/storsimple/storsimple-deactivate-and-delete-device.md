<properties 
   pageTitle="Deaktivieren und löschen Sie ein Gerät StorSimple | Microsoft Azure"
   description="Beschreibt, wie Sie StorSimple Gerät aus Dienst entfernen, indem zunächst deaktivieren und dann löschen."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktivieren Sie und löschen Sie ein Gerät StorSimple

## <a name="overview"></a>(Übersicht)

Sie möchten ein StorSimple Gerät außer Betrieb nehmen (z. B., wenn Sie ersetzen oder aktualisieren Ihr Gerät oder wenn Sie nicht mehr StorSimple verwenden). Wenn dies der Fall ist, müssen Sie das Gerät zu deaktivieren, bevor Sie ihn löschen können. Deaktivieren von trennt die Verbindung zwischen dem Gerät und dem entsprechenden StorSimple Manager-Dienst. In diesem Lernprogramm wird erläutert, wie ein StorSimple Gerät aus dem Dienst entfernen, indem Sie zunächst deaktiviert wird, und klicken Sie dann löschen. 

Wenn Sie ein Gerät deaktiviert haben, werden alle Daten, die lokal auf dem Gerät gespeichert wurde nicht mehr zugegriffen werden. Nur die Daten, die zugeordnete Gerät, das in der Cloud gespeichert wurde, können wiederhergestellt werden.  

>[AZURE.WARNING] Deaktivierung ist ein endgültiger Vorgang und kann nicht rückgängig gemacht werden. Eine deaktivierte Geräte kann nicht mit dem Dienst StorSimple Manager registriert sein, es sei denn, sie zuerst von der Factory zurückgesetzt wird. 
>
>Die Factory zurücksetzen Prozess löscht alle Daten, die lokal auf Ihrem Gerät gespeichert wurde. Daher ist es wichtig, dass Sie eine Cloud all Ihrer Daten Momentaufnahme, bevor Sie ein Gerät deaktiviert haben. Dies können Sie alle Daten zu einem späteren Zeitpunkt wiederherstellen.

In diesem Lernprogramm wird erläutert, wie Sie:

- Deaktivieren Sie ein Gerät, und löschen Sie die Daten
- Deaktivieren Sie ein Gerät und behalten die Daten bei

Außerdem erfahren Sie, wie Deaktivierung und löschen kann nur auf ein StorSimple virtuelles Gerät.

>[AZURE.NOTE] Bevor Sie ein StorSimple physische oder virtuelle Gerät deaktiviert haben, stellen Sie sicher, beenden oder Löschen von Clients und Hosts, die auf dem Gerät abhängig sind.

## <a name="deactivate-and-delete-data"></a>Deaktivieren und Löschen von Daten

Wenn Sie das Gerät vollständig löschen interessiert sind, und nicht die Daten auf dem Gerät beibehalten möchten, führen Sie die folgenden Schritte.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Deaktivieren das Gerät, und löschen die Daten  

1. Vor der können ein Gerät, müssen Sie alle die Lautstärke löschen Container (und die Datenträger) mit dem Gerät verbunden sind. Nur, nachdem Sie die zugehörigen Sicherungskopien gelöscht haben, können Sie die Lautstärke Container löschen.

2. Deaktivieren Sie das Gerät wie folgt ein:

    1. Wählen Sie auf der Seite Dienst **Geräte** StorSimple Manager das Gerät, das Sie deaktivieren, und klicken Sie am unteren Rand der Seite, die **Deaktivieren**möchten.

    2. Es wird eine bestätigungsmeldung angezeigt. Klicken Sie auf **Ja,** um den Vorgang fortzusetzen. Startet der Prozess deaktivieren und ein paar Minuten dauern.

3. Nach der Deaktivierung können Sie das Gerät vollständig löschen. Löschen ein Gerät entfernt aus der Liste der mit dem Dienst verbundenen Geräte. Der Dienst können Sie nicht mehr das gelöschte Gerät verwalten. Gehen Sie folgendermaßen vor, um das Gerät zu löschen:

    1. Wählen Sie auf der Seite Dienst **Geräte** StorSimple Manager ein deaktivierte Gerät, das Sie löschen möchten.

    2. Klicken Sie unten auf der Seite die auf **Löschen**.

    3. Sie werden zur Bestätigung aufgefordert werden. Klicken Sie auf **Ja,** um den Vorgang fortzusetzen.

    Für das Gerät zu löschenden einige Minuten dauern.

## <a name="deactivate-and-retain-data"></a>Deaktivieren und Beibehalten der Daten

Wenn Sie löschen das Gerät interessiert sind, aber die Daten beibehalten möchten, führen Sie die folgenden Schritte.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Deaktivieren ein Gerät und die Daten beibehalten 

1. Deaktivieren Sie das Gerät an. Die Lautstärke-Container als auch die Momentaufnahmen des Geräts bleibt.

    1. Wählen Sie auf der Seite Dienst **Geräte** StorSimple Manager das Gerät, das Sie deaktivieren, und klicken Sie am unteren Rand der Seite, die **Deaktivieren**möchten.

    2. Es wird eine bestätigungsmeldung angezeigt. Klicken Sie auf **Ja,** um den Vorgang fortzusetzen. Startet der Prozess deaktivieren und ein paar Minuten dauern.

2. Sie können jetzt über die Lautstärke Container und die zugehörigen Momentaufnahmen fehl. Verfahren finden Sie unter [Failover und Disaster Wiederherstellung für Ihr Gerät StorSimple](storsimple-device-failover-disaster-recovery.md).

3. Nach der Deaktivierung und Failover können Sie das Gerät vollständig löschen. Löschen ein Gerät entfernt aus der Liste der mit dem Dienst verbundenen Geräte. Der Dienst können Sie nicht mehr das gelöschte Gerät verwalten. Führen Sie die folgenden Schritte aus, um das Gerät zu löschen:
 
    1. Wählen Sie auf der Seite Dienst **Geräte** StorSimple Manager ein deaktivierte Gerät, das Sie löschen möchten.

    2. Klicken Sie unten auf der Seite die auf **Löschen**.

    3. Sie werden zur Bestätigung aufgefordert werden. Klicken Sie auf **Ja,** um den Vorgang fortzusetzen.

    Für das Gerät zu löschenden einige Minuten dauern.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktivieren Sie und löschen Sie ein virtuelles Gerät

Bei einem StorSimple virtuelle Gerät freigegeben Deaktivierung des virtuellen Computers. Sie können dann Löschen des virtuellen Computers und den Ressourcen erstellt, wenn sie nach der Bereitstellung wurde. Nachdem das virtuelle Gerät deaktiviert ist, können sie den ursprünglichen Zustand wiederhergestellt werden. 

Deaktivierung Ergebnisse in der folgenden Aktionen durch:

- Das StorSimple virtuelle Gerät wird entfernt.

- Die OSDisk und Daten Datenträger für das StorSimple virtuelle Gerät erstellt werden entfernt.

- Der Dienst gehostet und virtuelle Netzwerk, die während der Bereitstellung erstellt wurden werden beibehalten. Wenn Sie diese Elemente nicht verwenden, sollten Sie diese manuell löschen.

- Cloud Momentaufnahmen erstellte virtuelle Gerät StorSimple aufbewahrt werden.

## <a name="next-steps"></a>Nächste Schritte
- Wechseln Sie zum Wiederherstellen der deaktivierte Geräte auf Standardwerte für das zu [Standardeinstellungen Factory Gerät zurücksetzen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Technische Unterstützung, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md).

- Weitere Informationen zum Verwenden des StorSimple Manager-Diensts finden Sie unter der StorSimple-Manager-Dienst zum Verwalten von Ihrem Geräts StorSimple zu [verwenden](storsimple-manager-service-administration.md). 

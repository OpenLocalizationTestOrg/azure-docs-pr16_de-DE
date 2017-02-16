<properties 
   pageTitle="Verwalten Sie Ihre StorSimple zusätzliche Richtlinien | Microsoft Azure"
   description="Erläutert die Verwendung des StorSimple Manager-Diensts erstellen und Verwalten von manuelle Sicherungskopien, zusätzliche Zeitpläne und Sicherung Aufbewahrungsrichtlinien."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>Verwenden des StorSimple Manager-Diensts können Sie zusätzliche Richtlinien verwalten

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird erläutert, wie die Seite StorSimple Manager **Zusätzliche Richtlinien** verwenden, um zusätzliche Prozesse und Sicherung Aufbewahrungsrichtlinien für Ihre StorSimple Datenmengen zu steuern. Es wird beschrieben, wie eine manuelle Sicherung ausführen wird.

Die Seite **Zusätzliche Richtlinien** können Sie zusätzliche Richtlinien verwalten und lokale planen und Momentaufnahmen cloud. (Zusätzliche Richtlinien werden so konfigurieren Sie zusätzliche Zeitpläne und Sicherung Aufbewahrungsrichtlinien für eine Auflistung von Datenmengen verwendet). Zusätzliche Richtlinien können Sie eine Momentaufnahme der mehrere Datenmengen gleichzeitig zu erstellen. Dies bedeutet, dass die Sicherungskopien erstellt eine Sicherungskopie Richtlinie Absturz konsistente Kopien umgewandelt werden. Diese Seite listet die Sicherung Richtlinien, deren Typen, die zugeordneten Datenmengen, die Anzahl der Sicherungskopien beibehalten und die Möglichkeit, diese Richtlinien aktivieren.

Die Seite **Zusätzliche Richtlinien** können auch die vorhandenen Sicherung Richtlinien durch einen oder mehrere der folgenden Felder filtern:

- **Name der Richtlinie** – den Namen der Richtlinie zugeordnet. Die verschiedenen Typen von Richtlinien umfassen:

   - Geplante Richtlinien, die vom Benutzer explizit erstellt werden.
   - Automatische Richtlinien, die erstellt werden, wenn die Standardeinstellungen für die Sicherung für diese Option Volumen zum Zeitpunkt der Erstellung der Lautstärke aktiviert wurde. Diese Richtlinien werden als VolumeName_Default benannt, wobei Lautstärke Name den Namen des Datenträgers StorSimple vom Benutzer in der klassischen Azure-Portal nicht konfiguriert bezeichnet. Die automatischen Richtlinien zu Fehlern im täglichen Cloud Momentaufnahmen 22:30 Gerät gleichzeitig beginnen.
   - Importierten Richtlinien, die ursprünglich in den StorSimple Snapshot-Manager erstellt wurden. Sie verfügen über eine Kategorie, die den Host StorSimple Snapshot-Manager beschrieben, dem die Richtlinien aus importiert wurden.

- **Datenmengen** – die Datenmengen, die mit der Richtlinie verbunden ist. Alle Datenträger, die eine Sicherung Richtlinie zugeordnet werden zusammengefasst, wenn Sicherungskopien erstellt werden.

- **Letzte erfolgreiche Sicherung** – Datum und Uhrzeit der letzten Sicherung erfolgreich, die mit dieser Richtlinie durchgeführt wurde.

- **Nächste Sicherung** – Datum und Uhrzeit der nächsten geplanten Sicherung, die von dieser Richtlinie eingeleitet werden.

- **Zeitpläne** – die Anzahl der Zeitpläne die Sicherung Richtlinie zugeordnet.

Die häufig verwendete Vorgänge, die Sie von dieser Seite ausführen können sind:

- Fügen Sie eine zusätzliche Richtlinie hinzu 
- Hinzufügen oder Ändern eines Projektplans 
- Löschen einer Sicherung Richtlinie 
- Nehmen Sie eine manuelle Sicherung 
- Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Datenmengen und Zeitpläne 

## <a name="add-a-backup-policy"></a>Fügen Sie eine zusätzliche Richtlinie hinzu

Fügen Sie eine zusätzliche Richtlinie für Ihre Sicherungskopien automatisch planen. Führen Sie die folgenden Schritte aus, im klassischen Azure-Portal eine zusätzliche Richtlinie für Ihr Gerät StorSimple hinzugefügt. Nachdem Sie die Richtlinie hinzugefügt haben, können Sie einen Zeitplan definieren (finden Sie unter [Hinzufügen oder ändern ein Zeitplans](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video verfügbar](./media/storsimple-manage-backup-policies/Video_icon.png) **Video verfügbar**

Wenn Sie ein Video zur Verfügung, die veranschaulicht, wie erstellen Sie einen lokalen oder cloud Sicherung Richtlinie, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Hinzufügen oder Ändern eines Projektplans

Sie können hinzufügen oder ändern ein Zeitplans, das an eine vorhandene Sicherungsdatei Richtlinie auf Ihrem Gerät StorSimple angeschlossen ist. Führen Sie die folgenden Schritte aus, im klassischen Azure-Portal hinzufügen oder Ändern eines Zeitplans.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Löschen einer Sicherung Richtlinie

Führen Sie die folgenden Schritte aus, im klassischen Azure-Portal löschen eine Sicherung Richtlinie auf Ihrem Gerät StorSimple.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Nehmen Sie eine manuelle Sicherung

Führen Sie die folgenden Schritte aus, im klassischen Azure-Portal So erstellen Sie eine Sicherung bei Bedarf (manuelle), für ein einzelnes Volume.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Datenmengen und Zeitpläne

Führen Sie im klassischen Azure-Portal eine benutzerdefinierte Richtlinien zu erstellen, die mehrere Datenmengen und Terminpläne weist die folgenden Schritte aus.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Verwenden des Diensts StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple](storsimple-manager-service-administration.md).

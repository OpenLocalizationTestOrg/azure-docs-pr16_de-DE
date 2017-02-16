<properties 
   pageTitle="Ändern Sie die Kennwörter StorSimple | Microsoft Azure" 
   description="Beschreibt, wie den StorSimple Manager-Dienst verwenden, um Ihre Administratorkennwörtern StorSimple Snapshot-Manager und Gerät zu ändern." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Verwenden Sie den Dienst StorSimple Manager so ändern Sie die Kennwörter StorSimple

## <a name="overview"></a>(Übersicht) 

Das Azure klassische Portal **Konfigurieren** Seite enthält alle Geräteparameter, die Sie auf einem Gerät StorSimple neu konfigurieren können, die von einem Dienst StorSimple Manager verwaltet wird. In diesem Lernprogramm wird beschrieben, wie Sie die Seite **Konfigurieren** auf Ihrem Gerät Administrator- oder StorSimple Snapshot-Manager Kennwort ändern.

## <a name="change-the-device-administrator-password"></a>Ändern Sie das Kennwort des Administrators

Wenn Sie Windows PowerShell-Benutzeroberfläche zum Zugreifen auf des StorSimple Geräts verwenden, müssen Sie ein Gerät Administratorkennwort einzugeben. Wenn das erste StorSimple Gerät mit einem Dienst registriert ist, ist das standardmäßige Kennwort für diese Schnittstelle *Kennwort1*aus. Für die Sicherheit Ihrer Daten müssen Sie dieses Kennwort am Ende der Registrierung ändern. Sie können nicht aus der Registrierungsprozess beenden, ohne das Ändern des Kennworts. Weitere Informationen finden Sie unter [Schritt 3: konfigurieren, und melden Sie sich das Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Das Kennwort, das zuerst über die Windows PowerShell-Schnittstelle während der Registrierung festgelegt wurde kann dann über das klassische Azure-Portal geändert werden. Führen Sie die folgenden Schritte aus, um das Gerät Administratorkennwort ändern.

#### <a name="to-change-the-device-administrator-password"></a>So ändern Sie das Kennwort des Administrators

1. Klicken Sie in der klassischen-Portal auf **Geräte** > **Konfigurieren** für Ihr Gerät.

2. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Gerät Administratorkennwort** . Bereitstellen eines Administratorkennworts, das von 8 auf 15 Zeichen enthält. Das Kennwort muss eine Kombination von mindestens 3 Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen sein.

3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie auf **Speichern** , am unteren Rand der Seite.

Das Gerät Administratorkennwort sollte jetzt aktualisiert werden. Dieses geänderte Kennwort können Sie die Windows PowerShell-Benutzeroberfläche zugreifen.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Ändern des Kennworts StorSimple Snapshot-Manager

StorSimple Snapshot-Manager-Software befindet sich auf Ihrem Windows-Server und können Administratoren verwalten Ihres Geräts StorSimple in Form eines lokalen Sicherungen und Momentaufnahmen cloud.

Wenn Sie ein Gerät in StorSimple Snapshot-Manager konfigurieren, werden Sie aufgefordert, den Gerät IP-Adresse und das Kennwort, das Speichergerät authentifizieren bereitzustellen. Dieses Kennwort wird über die Benutzeroberfläche von Windows PowerShell zuerst konfiguriert. Weitere Informationen finden Sie unter [Schritt 3: konfigurieren, und melden Sie sich das Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Das Kennwort, das zuerst über die Windows PowerShell-Schnittstelle während der Registrierung festgelegt wurde kann dann über das klassische Portal geändert werden. Führen Sie die folgenden Schritte aus, zum Ändern des Kennworts StorSimple Snapshot-Manager.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Zum Ändern des Kennworts StorSimple Snapshot-Manager

1. Klicken Sie in der klassischen-Portal auf **Geräte** > **Konfigurieren** für Ihr Gerät.

2. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **StorSimple Snapshot-Manager** . Geben Sie ein Kennwort für den 14 oder 15 Zeichen ist. Stellen Sie sicher, dass das Kennwort eine Kombination von mindestens 3 Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen enthält.

3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie auf **Speichern** , am unteren Rand der Seite.

Das Kennwort StorSimple Snapshot-Manager sollte jetzt aktualisiert werden.
 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Sicherheit StorSimple](storsimple-security.md).

- Weitere Informationen zum [Ändern der Konfigurations Gerät](storsimple-modify-device-config.md).

- Weitere Informationen zum [Verwenden des Diensts StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple](storsimple-manager-service-administration.md).

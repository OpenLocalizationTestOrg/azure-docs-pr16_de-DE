<properties 
   pageTitle="Bereitstellen den StorSimple Manager-Dienst | Microsoft Azure"
   description="Erläutert, wie Sie erstellen und löschen den Dienst StorSimple Manager im klassischen Azure-Portal und beschreibt, wie der Dienst Registrierungsschlüssel verwalten."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-manager-service"></a>Bereitstellen des StorSimple Manager-Diensts

## <a name="overview"></a>(Übersicht)

Der Dienst StorSimple-Manager in Microsoft Azure ausgeführt wird und auf mehreren Geräten ein StorSimple verbindet. Nachdem Sie den Dienst erstellt haben, können Sie es verwenden, verwalten Sie die Geräte vom klassischen Microsoft Azure-Portal in einem Browser ausgeführt. So können Sie alle Geräte überwachen, die mit der StorSimple Manager-Dienst von einem einzelnen, zentralen Speicherort, damit auch minimieren Verwaltungsaufwand verbunden sind.

Die Startseite StorSimple Manager listet alle StorSimple Manager Dienste, mit denen Sie Ihren StorSimple Speichergeräten verwalten. Die folgende Informationen werden für jeden Dienst StorSimple-Manager auf der Seite StorSimple Manager dargestellt:

- **Name** – den Namen, die mit Ihrem Dienst StorSimple Manager zugewiesen wurde, wenn er erstellt wurde. Der Name des Dienstes kann nicht geändert werden, nachdem der Dienst erstellt wurde.

- **Status** – der Status für den Dienst, der **aktive**, **Erstellen**oder **Online**sein kann.

- **Speicherort** – die geografische Position in dem das Gerät StorSimple bereitgestellt werden soll.

- **Abonnement** – das Abrechnung Abonnement, die mit dem Dienst verknüpft ist.

Sind allgemeinen Aufgaben, die über die Seite StorSimple Manager ausgeführt werden können:

- Erstellen Sie einen Dienst
- Löschen Sie einen Dienst
- Abrufen der Dienst Registrierungsschlüssel
- Der Dienst Registrierungsschlüssel neu zu generieren

In diesem Lernprogramm beschrieben, wie jede dieser Aufgaben ausführen.

## <a name="create-a-service"></a>Erstellen Sie einen Dienst

Verwenden Sie die Option zum **Schnellen Erstellen** einen StorSimple-Manager-Dienst zu erstellen, wenn Sie Ihr Gerät StorSimple bereitstellen möchten. Wenn Sie einen Dienst erstellen, müssen Sie haben:

- Ein Abonnement mit einem Enterprise Agreement
- Ein aktives Microsoft Azure-Speicher-Konto
- Die Rechnungsinformationen, die für die Verwaltung von Access verwendet wird

Sie können auch eine Speicher Standardkonto generieren beim Erstellen des Diensts auswählen.

Ein einzelner Dienst kann mehrere Geräte verwalten. Jedoch kann nicht auf einem Gerät mehrere Dienste erstrecken. Ein großen Unternehmen kann mehrere Dienstinstanzen für die Arbeit mit anderen Abonnements, Organisationen oder sogar Bereitstellung Speicherorte haben. Bitte beachten Sie, dass Sie Instanzen von StorSimple-Manager-Dienst zum Verwalten von StorSimple 8000 Reihe Geräte und StorSimple virtuelle Arrays trennen müssen.

Führen Sie die folgenden Schritte aus, um einen Dienst zu erstellen.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Löschen Sie einen Dienst

Bevor Sie einen Dienst löschen, stellen Sie sicher, dass keine verbundenen Geräte verwendet werden. Wenn der Dienst verwendet wird, deaktivieren Sie die verbundenen Geräte aus. Der Vorgang deaktivieren trennt die Verbindung zwischen dem Gerät und dem Dienst, können aber beibehalten die Gerätedaten in der Cloud. 

[AZURE.IMPORTANT] Nach dem Löschen eines Diensts der Vorgang kann nicht rückgängig gemacht werden. Jedem Gerät die vom Dienst verwendeten wurde müssen werden Factory zurücksetzen, bevor sie mit einem anderen Dienst verwendet werden kann. In diesem Szenario, die lokalen Daten auf dem Gerät als auch die Konfiguration, gehen verloren.

Führen Sie die folgenden Schritte aus, um einen Dienst zu löschen.

### <a name="to-delete-a-service"></a>So löschen Sie einen Dienst

1. Wählen Sie auf der Seite **StorSimple Verwaltungsdienst für** den Dienst, den Sie löschen möchten.

1. Klicken Sie auf am unteren Rand der Seite **Löschen** .

1. Klicken Sie in der bestätigungsbenachrichtigung zur auf **Ja** . Es kann einige Minuten auf den zu löschenden Dienst dauern.

## <a name="get-the-service-registration-key"></a>Abrufen der Dienst Registrierungsschlüssel

Nachdem Sie einen Dienst erfolgreich erstellt haben, müssen Sie Ihr Gerät StorSimple mit dem Dienst zu registrieren. Um Ihre erste StorSimple Gerät zu registrieren, benötigen Sie die Service-Registrierungsschlüssel. Um weitere Geräte mit einem vorhandenen StorSimple Dienst zu registrieren, benötigen Sie sowohl die Registrierungsschlüssel und der Dienst Daten Verschlüsselungsschlüssel (der auf dem ersten Gerät während der Registrierung generiert wird). Weitere Informationen zu den Dienst Datenschlüssel finden Sie unter [StorSimple Sicherheit](storsimple-security.md). Sie können den Zugriff auf **Registrierungsschlüssel** auf der Seite **Dienste** der Registrierungsschlüssel abrufen.

Führen Sie die folgenden Schritte aus, um den Dienst Registrierungsschlüssel abrufen.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Beibehalten der Registrierungsschlüssel Dienst an einem sicheren Ort. Sie benötigen diesen Schlüssel als auch die Dienst Daten Verschlüsselung-Taste, um zusätzliche Geräte für diesen Dienst registrieren. Nach dem Abrufen der Dienst Registrierungsschlüssel, müssen Sie Ihr Gerät über die Windows PowerShell für StorSimple Schnittstelle konfigurieren.

Details zur Verwendung dieser Registrierungsschlüssel finden Sie unter [Schritt 3: konfigurieren, und melden Sie sich das Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough.md#step-2-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Der Dienst Registrierungsschlüssel neu zu generieren

Sie benötigen einen Dienst Registrierungsschlüssel neu zu erstellen, wenn Sie die Taste Drehung ausführen müssen oder wenn Sie die Liste der Dienstadministratoren geändert hat. Wenn Sie den Schlüssel neu erstellen, wird der neue Schlüssel nur für die Registrierung nachfolgende Geräte verwendet. Die Geräte, die bereits registriert wurden sind von diesem Prozess nicht betroffen.

Führen Sie die folgenden Schritte aus, um einen Dienst Registrierungsschlüssel neu zu erstellen.

### <a name="to-regenerate-the-service-registration-key"></a>Der Dienst Registrierungsschlüssel neu erstellen.

1. Klicken Sie auf der Seite **StorSimple Manager-Dienst** auf **Registrierungsschlüssel**.

1. Klicken Sie auf **neu zu generieren**, klicken Sie im Dialogfeld **Dienst-Registrierungsschlüssel** .

1. Eine bestätigungsmeldung wird angezeigt. Klicken Sie auf **OK** , um mit dem erneuten Generieren fortzufahren.

1. Ein neuen Dienst Registrierungsschlüssel wird angezeigt.

1. Kopieren Sie diesen Schlüssel, und speichern Sie sie für die Registrierung von neuen Geräten mit diesem Dienst.

1. Klicken Sie auf das Symbol "Überprüfen" ![Kontrollkästchen-Symbol](./media/storsimple-manage-service/HCS_CheckIcon.png) Um dieses Dialogfeld zu schließen.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den [StorSimple Bereitstellung verarbeiten](storsimple-deployment-walkthrough.md).

- Weitere Informationen zum [Verwalten Ihres StorSimple Speicher-Kontos](storsimple-manage-storage-accounts.md).

- Weitere Informationen zum [Verwenden der StorSimple-Manager-Dienst auf Ihrem Gerät StorSimple verwalten](storsimple-manager-service-administration.md).

 

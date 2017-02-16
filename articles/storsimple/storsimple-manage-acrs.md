<properties 
   pageTitle="Verwalten von Access Steuerelement Einträgen in StorSimple | Microsoft Azure"
   description="Beschreibt, wie Access Steuerelement Einträge (ACRs) um festzustellen, welche Hosts herstellen können, auf einen Datenträger auf dem Gerät StorSimple verwenden."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Mit dem Access-Steuerelement Einträge Verwalten der StorSimple Manager-Dienst

## <a name="overview"></a>(Übersicht)

Access-Steuerelement Einträge (ACRs) können Sie angeben, welche Hosts auf einen Datenträger auf dem Gerät StorSimple eine Verbindung herstellen können. ACRs sind auf einen bestimmten Datenträger festgelegt und die iSCSI-qualifizierte Namen (IQNs) der Hosts enthalten. Wenn ein Host versucht, auf einen Datenträger verbinden, überprüft das Gerät an der ACR die Lautstärke für den Namen der IQN zugeordnet sind, und wenn eine Übereinstimmung vorliegt, klicken Sie dann die Verbindung wird hergestellt. Im Abschnitt Zugriff Steuerelement Datensätze auf der Seite **Konfigurieren** zeigt alle Datensätze Access Steuerelement mit den entsprechenden IQNs der Hosts.

In diesem Lernprogramm werden die folgenden allgemeinen ACR-bezogene Aufgaben erläutert:

- Hinzufügen eines Access-Steuerelement 
- Bearbeiten eines Datensatzes der Access-Steuerelement 
- Löschen eines Datensatzes der Access-Steuerelement 

> [AZURE.IMPORTANT] 
> 
> - Wenn ein Volume ein ACR zuordnen möchten, müssen sicherstellen Sie, dass die Lautstärke von mehr als einem nicht gruppierten Host nicht gleichzeitig zugegriffen werden kann, da dies die Lautstärke beeinträchtigt werden konnte. 
> - Wenn eine ACR von einem Volume löschen, stellen Sie sicher, dass der entsprechende Host nicht die Lautstärke zugreifen ist, da das Löschen einer Unterbrechung Lese-und Schreibzugriff führen kann.

## <a name="add-an-access-control-record"></a>Hinzufügen eines Access-Steuerelement

Sie verwenden die Seite StorSimple Manager **Konfigurieren** , um ACRs hinzuzufügen. Ordnen Sie in der Regel eine ACR mit einem Datenträger.

Führen Sie die folgenden Schritte aus, um eine ACR hinzuzufügen.

#### <a name="to-add-an-access-control-record"></a>Eine Access-Steuerelement-Eintrag hinzufügen

1. Wählen Sie auf der Startseite Dienst den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Geben Sie in tabellarischen Auflistung wird unter **Access Steuerelement Einträge**einen **Namen** für Ihre ACR ein.

3. Bieten Sie den Namen Ihres Windows-Hosts unter **iSCSI Initiatornamen**IQN. Führen Sie folgende Schritte aus, um die IQN Ihre Windows Server-Hosts zu erhalten:

   - Starten Sie den Microsoft iSCSI Initiator auf Ihrem Windows-Host ein.
   - Im Fenster **iSCSI Initiatoreigenschaften** auf der Registerkarte **Konfiguration** wählen Sie aus, und kopieren Sie die Zeichenfolge aus dem Feld **Initiatornamen** .
   - Fügen Sie diese Zeichenfolge in das Feld **iSCSI Initiatornamen** für die ACRs Tabelle im klassischen Azure-Portal an.

4. Klicken Sie auf **Speichern** , um den neu erstellten ACR speichern. Tabellarische Liste wird diese Erweiterung entsprechend aktualisiert werden.

## <a name="edit-an-access-control-record"></a>Bearbeiten eines Datensatzes der Access-Steuerelement

Sie verwenden die Seite **Konfigurieren** im klassischen Azure-Portal an, um ACRs zu bearbeiten. 

> [AZURE.NOTE] Sie können nur diese ACRs ändern, die derzeit nicht verwendet werden. Zum Bearbeiten einer ACR zugeordnet einen Datenträger, der aktuell verwendet wird, müssen Sie zuerst die Lautstärke offline schalten.

Führen Sie die folgenden Schritte aus, um eine ACR zu bearbeiten.

#### <a name="to-edit-an-access-control-record"></a>So bearbeiten Sie einen Access-Steuerelement Datensatz

1. Wählen Sie auf der Startseite Dienst den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Die tabellarische Auflistung der Datensätze Steuerelement Access, zeigen Sie auf der ACR, die Sie ändern möchten.

3. Geben Sie einen neuen Namen und/oder IQN für die ACR an.

4. Klicken Sie auf **Speichern** , um die geänderte ACR speichern. Tabellarische Liste wird aktualisiert, um diese Änderung anzuzeigen.

## <a name="delete-an-access-control-record"></a>Löschen eines Datensatzes der Access-Steuerelement

Sie verwenden die Seite **Konfigurieren** im klassischen Azure-Portal an, um ACRs zu löschen. 

> [AZURE.NOTE] Sie können nur diese ACRs löschen, die derzeit nicht verwendet werden. Zum Löschen einer ACR zugeordnet einen Datenträger, der aktuell verwendet wird, müssen Sie zuerst die Lautstärke offline schalten.

Führen Sie die folgenden Schritte aus, um eine Access-Steuerelement Datensatz zu löschen.

#### <a name="to-delete-an-access-control-record"></a>So löschen Sie ein Eintrag der Access-Steuerelement

1. Wählen Sie auf der Startseite Dienst den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Die tabellarische Auflistung der Access-Steuerelement Datensätze (ACRs), zeigen Sie auf der ACR, die Sie löschen möchten.

3. Ein Löschsymbol (**X**) wird in der extrem rechten Spalte für die ACR angezeigt, die Sie auswählen. Klicken Sie auf das **X** -Symbol, um die ACR löschen.

4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja,** um den Löschvorgang fortzusetzen. Tabellarische Liste wird aktualisiert, um den Löschvorgang zu entsprechen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Verwalten von StorSimple Datenmengen](storsimple-manage-volumes.md).

- Weitere Informationen zum [Verwenden des Diensts StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple](storsimple-manager-service-administration.md).
 

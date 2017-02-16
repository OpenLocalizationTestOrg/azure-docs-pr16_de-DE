<properties
    pageTitle="Erste Schritte mit vorkonfigurierten Lösungen | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie Sie eine Lösung Azure IoT Suite vorkonfiguriert bereitstellen."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Lernprogramm: Erste Schritte mit den vorkonfigurierten Lösungen

## <a name="introduction"></a>Einführung

Azure IoT Suite [vorkonfigurierten Lösungen] [ lnk-preconfigured-solutions] Kombinieren mehrerer IoT Azure-Services, um End-to-End-Lösungen bereitstellen, die allgemeine IoT geschäftliche Szenarios implementieren. Die Lösung für die *remote-Überwachung* vorkonfiguriert her und überwacht Ihren Geräten. Sie können die Lösung zum Analysieren von Daten aus Ihren Geräten Streams und die geschäftliche Ergebnisse zu verbessern, indem Sie Prozesse automatisch in diesen Stream Daten reagieren.

In diesem Lernprogramm erfahren Sie, wie die remote Überwachung vorkonfigurierte Lösung bereitstellen. Es führt Sie auch durch die grundlegenden Features der Überwachung remote-Lösung. Sie können viele dieser Features über die Lösung Dashboard zugreifen, die zusammen mit den vorkonfigurierten Lösung bereitstellt:

![Remote-Überwachung vorkonfiguriert Lösung dashboard][img-dashboard]

Zur Durchführung dieses Lernprogramms benötigen Sie ein aktives Azure-Abonnement.

> [AZURE.NOTE]  Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Ausführliche Informationen finden Sie [Kostenlose Testversion Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Anzeigen der Lösung Dashboards

Das Lösung Dashboard können Sie die bereitgestellte Lösung verwalten. Sie können beispielsweise angezeigt werden, Geräte hinzufügen und Konfigurieren von Regeln.

1.  Wenn die Bereitstellung abgeschlossen ist, und zeigt die Kachel für Ihre vorkonfigurierten Lösung an, **bereit sind**, klicken Sie auf **Starten** , um Ihr remote Überwachung Lösung Portal in einer neuen Registerkarte zu öffnen.

    ![Starten Sie die vorkonfigurierte Lösung][img-launch-solution]

2.  Standardmäßig wird im Portal Lösung der *Lösung Dashboard*. Sie können andere Ansichten mit dem Menü links auswählen.

    ![Remote-Überwachung vorkonfiguriert Lösung dashboard][img-dashboard]

Das Dashboard zeigt die folgenden Informationen:

- Die Karte zeigt die Position der einzelnen Geräte mit der Lösung verbunden. Wenn Sie die Lösung zum ersten Mal starten, gibt es vier simulierte Geräten. Die simulierten Geräten als Azure WebJobs implementiert werden, und die Lösung verwendet die Bing Maps-API, um die Informationen auf der Karte darstellen.
- Im Bereich **Werden Verlauf** zeichnet Temperatur und Feuchtigkeit werden von in der Nähe Echtzeit und zeigt beispielsweise maximale Mindest- und Mittelwert Feuchtigkeit Aggregieren von Daten in einer ausgewählten Gerät.
- Bedienfeld " **Verlauf Erinnerung** " zeigt aktuelle Erinnerung Ereignisse aus, wenn Sie ein Wert werden einen Schwellenwert überschritten hat. Sie können eigene Alarme zusätzlich zu den Beispielen die vorkonfigurierten Lösung erstellte definieren.

## <a name="view-the-device-list"></a>Anzeigen der Geräteliste

Die Geräteliste zeigt alle registrierten Geräte in die Lösung. Sie anzeigen und Bearbeiten von Gerätemetadaten, hinzufügen oder entfernen Geräte, und Befehle an Geräte senden.

1.  Klicken Sie auf **Geräte** in dem Menü links, um die *Geräteliste* für diese Lösung anzuzeigen.

    ![Geräteliste in dashboard][img-devicelist]

2.  Die Geräteliste zeigt, dass es vier simulierten Geräten vom provisioning Prozess erstellt gibt.

3.  Klicken Sie auf einem Gerät in der Geräteliste, um die Gerätedetails anzuzeigen.

    ![Gerätedetails im dashboard][img-devicedetails]

Im Bereich **Device-Details** enthält drei Abschnitte:

- Im Abschnitt **Aktionen** Listet die Aktionen, die Sie auf dem Gerät ausführen können. Wenn Sie das Gerät deaktivieren, ist es nicht mehr zulässig werden senden oder empfangen Befehle. Wenn Sie ein Gerät deaktivieren, können Sie es dann erneut aktivieren. Sie können eine Regel für das Gerät, das eine Erinnerung ausgelöst wird, wenn ein Wert werden einen Schwellenwert überschreitet hinzufügen. Sie können auch einen Befehl an ein Gerät senden. Wenn ein Gerät zuerst eine Verbindung herstellt, erfahren sie der Lösung die Befehle, die, denen Sie zum beantworten kann.
- Im Abschnitt **Eigenschaften Gerät** Listet die Gerätemetadaten auf. Einige dieser Metadaten aus dem Gerät (beispielsweise vom Hersteller) stammt, und einige wird ausgelöst, indem Sie die Lösung (beispielsweise den erstellten Zeit). Sie können die Gerätemetadaten hier bearbeiten.
- Im Abschnitt **Authentifizierung Tasten** Listet die Tasten, die das Gerät verwenden kann, mit der Lösung authentifizieren.

## <a name="send-a-command-to-a-device"></a>Senden eines Befehls an ein Gerät

Des Detailbereichs Gerät zeigt alle Befehle, die ein bestimmtes Gerät unterstützt und ermöglicht Ihnen das Senden an ein Gerät Befehle. Beim ersten Start ein Gerät sendet es Informationen zu den unterstützten Befehlen in der Lösung.

1.  Klicken Sie auf **Befehle** des Detailbereichs Gerät für das ausgewählte Gerät.

    ![Gerät Befehle in dashboard][img-devicecommands]

2.  Wählen Sie aus der Befehlsliste **PingDevice** aus.

3.  Klicken Sie auf **Befehl senden**.

4.  Sie können den Status des Befehls in den Befehlsverlauf anzeigen.

    ![Status des Befehls in dashboard][img-pingcommand]

Die Lösung verfolgt den Status der einzelnen Befehle, die sie sendet. Anfangs ist das Ergebnis **steht noch aus**. Wenn das Gerät meldet, dass es den Befehl ausgeführt hat, wird das Ergebnis in **Success**festgelegt.

## <a name="add-a-new-simulated-device"></a>Hinzufügen eines neuen simulierten Geräts

Wenn Sie die vorkonfigurierte Lösung bereitstellen, wird automatisch die vier Stichproben Geräte, die Sie in der Geräteliste finden Sie unter bereitgestellt. Diese Geräte sind *simulierten Geräten* in einer Azure WebJob ausgeführt. Simulierte Geräten erleichtern so experimentieren Sie mit der vorkonfigurierten Lösung, ohne dass realen, physischen Geräte bereitstellen. Wenn Sie die Verbindung mit der Lösung eines realen Geräts möchten, finden Sie unter [Verbinden mit Ihrem Gerät auf die Remote überwachen vorkonfigurierten Lösung] [ lnk-connect-rm] Lernprogramm.

Die folgenden Schritte gezeigt, wie die Lösung eines simulierten Gerät hinzufügen:

1.  Navigieren Sie zu der Geräteliste zurück.

2.  Klicken Sie auf **+ A-Gerät hinzufügen** in der unteren linken Ecke, ein Gerät hinzuzufügen.

    ![Ein Gerät zum die vorkonfigurierten Lösung hinzufügen][img-adddevice]

3.  Klicken Sie auf die Kachel **Eines simulierten Gerät** auf **Neu hinzufügen** .

    ![Legen Sie die neue Gerätedetails in dashboard][img-addnew]
    
    Zusätzlich zum Erstellen eines neuen simulierten Geräts, können Sie auch ein physisches Gerät hinzufügen, wenn Sie ein **Benutzerdefiniertes Gerät**erstellen. Weitere Informationen zum Herstellen einer Verbindung physische Geräte in die Lösung finden Sie unter [Verbinden mit Ihrem Gerät zur Überwachung vorkonfigurierten Lösung remote IoT Suite][lnk-connect-rm].

4.  Wählen Sie **zulassen, dass ich meinen eigenen Geräte-ID definieren**aus, und geben Sie einen Namen eindeutige Geräte-ID, wie z. B. **mydevice_01**.

5.  Klicken Sie auf **Erstellen**.

    ![Speichern eines neuen Geräts][img-definedevice]

6. Klicken Sie in Schritt 3 von **simulierten Gerät hinzufügen**auf **Fertig** , um die Liste Geräte zurückzukehren.

7. Sie können Ihr Gerät **Ausführen** in der Geräteliste anzeigen.

    ![Neue Ansicht-Gerät Geräteliste][img-runningnew]

8. Sie können auch die simulierten werden von Ihrem neuen Gerät auf dem Dashboard anzeigen:

    ![Ansicht werden von neuen Gerät][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Bearbeiten der Gerätemetadaten

Wenn ein Gerät zuerst zur Lösung eine Verbindung herstellt, sendet er seine Metadaten an die Lösung. Wenn Sie die Gerätemetadaten durch die Lösung Dashboard bearbeiten, sendet die neuen Metadatenwerte an das Gerät und speichert die neuen Werte in der Lösung DocumentDB Datenbank. Weitere Informationen finden Sie unter [Gerät Identität Registrierung und DocumentDB][lnk-devicemetadata].

1.  Navigieren Sie zu der Geräteliste zurück.

2.  Wählen Sie das neue Gerät in der **Liste Geräte**aus, und klicken Sie dann auf **Bearbeiten** , um die **Eigenschaften des Geräts**zu bearbeiten:

    ![Bearbeiten der Gerätemetadaten][img-editdevice]

3. Führen Sie einen Bildlauf nach unten, und nehmen Sie eine Änderung an die Breite und Länge Vales. Klicken Sie dann auf **Speichern von Änderungen an der Registrierung Geräts**.

    ![Bearbeiten der Gerätemetadaten][img-editdevice2]

4. Navigieren Sie zu dem Dashboard zurück, die die Position des Geräts geändert wurde, klicken Sie auf der Karte:

    ![Bearbeiten der Gerätemetadaten][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Fügen Sie eine Regel für das neue Gerät

Es gibt keine Regeln für das neue Gerät, dass Sie gerade hinzugefügt haben. In diesem Abschnitt fügen Sie eine Regel, die eine Erinnerung ausgelöst wird, wenn die Temperatur gemeldet, indem Sie das neue Gerät 47 Grad überschreitet. Bevor Sie beginnen, beachten Sie, dass der Verlauf werden für das neue Gerät auf dem Dashboard zeigt, dass die Gerät Temperatur überschreitet nie 45 Grad.

1.  Navigieren Sie zu der Geräteliste zurück.

2.  Wählen Sie das neue Gerät in der **Liste Geräte**aus, und klicken Sie dann auf **Regel hinzufügen** , um eine Regel für das Gerät hinzuzufügen.

3. Erstellen Sie eine Regel, die **Temperatur** wie das Datenfeld verwendet und wie die Ausgabe **AlarmTemp** verwendet, wenn die Temperatur 47 Grad überschreitet:

    ![Fügen Sie eine Geräteregel][img-adddevicerule]

4. Klicken Sie auf **Speichern und Anzeigen von Regeln** zum Speichern der Änderungen.

5.  Klicken Sie auf **Befehle** des Detailbereichs Gerät für das neue Gerät.

    ![Fügen Sie eine Geräteregel][img-adddevicerule2]

6.  Wählen Sie aus der Befehlsliste **ChangeSetPointTemp** und **SetPointTemp** auf 45 festgelegt. Klicken Sie auf den **Befehl senden**:

    ![Fügen Sie eine Geräteregel][img-adddevicerule3]

7.  Navigieren Sie zu der Lösung Dashboard zurück. Nach einer kurzen Zeit wird einen neuen Eintrag, klicken Sie im **Verlauf der Erinnerung** angezeigt werden, wenn die Temperatur von Ihrem Gerät neue gemeldeten 47-Grad Schwellenwert überschreitet:

    ![Fügen Sie eine Geräteregel][img-adddevicerule4]

8. Überprüfen und alle Regeln auf der Seite **Regeln** des Dashboards bearbeitet werden können:

    ![Liste Geräteregeln][img-rules]

9. Sie können überprüfen und bearbeiten die Aktionen aus, die als Antwort auf eine Regel auf der Seite **Aktionen** des Dashboards durchgeführt werden können:

    ![Listenaktionen-Gerät][img-actions]

> [AZURE.NOTE] Es ist möglich, Aktionen zu definieren, senden eine e-Mail oder SMS als Antwort auf eine Regel oder mit einem Line-of-Business-System über eine [Logik App]integrieren können[lnk-logic-apps]. Weitere Informationen finden Sie unter der [herstellen Logik App zu Ihrer Azure IoT Suite Remote-Überwachung Lösung vorkonfiguriert][lnk-logicapptutorial].

## <a name="other-features"></a>Weitere features

Verwenden das Lösung-Portal an, die, das Sie durchsuchen können, für Geräte mit bestimmten Merkmalen wie eine Zahl Modell:

![Suchen nach einem Gerät][img-search]

Sie können ein Gerät deaktivieren, und nachdem er deaktiviert ist können Sie ihn entfernen:

![Deaktivieren Sie und entfernen Sie ein Gerät][img-disable]

## <a name="behind-the-scenes"></a>Hintergrundinformationen

Wenn Sie eine Lösung vorkonfigurierte bereitstellen, erstellt Verfahren mehrere Ressourcen im Azure-Abonnement, das Sie ausgewählt haben. Sie können diese Ressourcen in der Azure- [Portal]anzeigen[lnk-portal]. Verfahren erstellt eine **Ressourcengruppe** mit einem Namen basierend auf den Namen, die, den Sie für Ihre Lösung vorkonfigurierten auswählen:

![Vorkonfigurierten Lösung Azure-Portal][img-portal]

Sie können die Einstellungen der einzelnen Ressourcen, indem Sie es auswählen in der Liste der Ressourcen in der Ressourcengruppe anzeigen.

Sie können auch den Quellcode für die vorkonfigurierten Lösung anzeigen. Remote Überwachung vorkonfigurierten Lösung Quellcode befindet sich in der [Azure-Iot-Remote-Überwachung] [ lnk-rmgithub] GitHub Repository:

- Ordner " **DeviceAdministration** " enthält den Quellcode für das Dashboard.
- Der **Simulator** -Ordner enthält den Quellcode für die simulierten Gerät.
- Der Ordner **EventProcessor** enthält den Quellcode für die Back-End-Prozess, der zur Verwaltung von der eingehenden werden.

Wenn Sie fertig sind, können Sie die vorkonfigurierte Lösung aus der Azure-Abonnements auf der [azureiotsuite.com] löschen[ lnk-azureiotsuite] Website. Diese Website können Sie einfach alle Ressourcen zu löschen, die bei der Erstellung der vorkonfigurierten Lösung bereitgestellt wurden.

> [AZURE.NOTE] Um sicherzustellen, dass Sie alle Elemente im Zusammenhang mit dem vorkonfigurierten Lösung löschen, löschen Sie ihn auf die [azureiotsuite.com] [ lnk-azureiotsuite] Website und die Ressourcengruppe im Portal nicht einfach löschen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine Lösung arbeiten vorkonfiguriert bereitgestellt haben, können Sie weiterhin erste Schritte mit IoT Suite, indem Sie den folgenden Artikeln:

- [Remote-Überwachung vorkonfiguriert Lösung Exemplarische Vorgehensweise][lnk-rm-walkthrough]
- [Herstellen einer Verbindung die remote Überwachung vorkonfigurierten Lösung mit Ihrem Gerät][lnk-connect-rm]
- [Berechtigungen für die Website azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md

> [AZURE.SELECTOR]
- [C unter Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [Klicken Sie auf Linux C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [Klicken Sie auf Mbed C](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Szenario (Übersicht)

In diesem Szenario erstellen Sie ein Gerät, das die folgenden werden an die entfernte Datenbank [vorkonfigurierten Lösung]für die Überwachung sendet[lnk-what-are-preconfig-solutions]:

- Externe Temperaturen
- Interne Temperatur
- Luftfeuchtigkeit

Zur Vereinfachung der Code auf dem Gerät generiert Beispielwerte, aber wir empfehlen Ihnen, die Stichprobe zu erweitern, indem Sie reale Sensoren auf Ihr Gerät verbinden, und senden real werden.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein aktives Azure-Konto an. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Ausführliche Informationen finden Sie [Kostenlose Testversion Azure][lnk-free-trial].

## <a name="before-you-start"></a>Bevor Sie beginnen

Bevor Sie den Code für Ihr Gerät schreiben, müssen Sie Ihre remote Überwachung vorkonfigurierte Lösung bereitstellen und dann Bereitstellen eines neues benutzerdefiniertes Geräts in die Lösung.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Ihre remote Überwachung vorkonfigurierte Lösung bereitstellen

Das Gerät, das Sie in diesem Lernprogramm erstellen sendet Daten in eine Instanz der [remote-Überwachung] [ lnk-remote-monitoring] vorkonfiguriert Lösung. Wenn Sie die remote Überwachung vorkonfigurierte Lösung bereits in Ihrem Konto Azure bereitgestellt nicht geschehen ist, führen Sie die folgenden Schritte aus:

1. Klicken Sie auf der Seite <https://www.azureiotsuite.com/> auf **+** zum Erstellen einer neuen Lösung.

2. Klicken Sie im Bereich **Remote-Überwachung** , mit Ihrer neue Lösung erstellen auf **auswählen** .

3. Klicken Sie auf der Seite **Erstellen Remote Überwachung Lösung** Geben Sie einen **Namen der Lösung** Ihrer Wahl, wählen Sie die **Region** , die Sie bereitstellen möchten, und wählen Sie aus der Azure-Abonnement verwenden möchten. Klicken Sie dann auf **Lösung erstellen**.

4. Erst nach Abschluss der Bereitstellung.

> [AZURE.WARNING] Die vorkonfigurierten Lösungen verwenden berechenbaren Azure Services. Achten Sie darauf, dass die vorkonfigurierte Lösung aus Ihrem Abonnement entfernen, wenn dies mit einer beliebigen unnötigen Gebühren zu vermeiden. Sie können eine vorkonfigurierte Lösung vollständig aus Ihrem Abonnement entfernen, finden Sie auf der Seite <https://www.azureiotsuite.com/> .

Klicken Sie auf **Starten** , um die Lösung Dashboard in Ihrem Browser zu öffnen, klicken Sie nach Abschluss der Bereitstellung Prozess für die Überwachung remote-Lösung.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Bereitstellung von Ihrem Gerät in die Überwachung remote-Lösung

> [AZURE.NOTE] Wenn Sie bereits ein Gerät in Ihre Lösung bereitgestellt haben, können Sie diesen Schritt überspringen. Sie müssen die Anmeldeinformationen Gerät Informationen beim Erstellen der Clientanwendung.

Für ein Gerät für die Verbindung zu den vorkonfigurierten Lösung muss es sich IoT Hub Verwendung gültiger Anmeldeinformationen identifizieren. Sie können die Anmeldeinformationen Gerät aus dem Dashboard Lösung abrufen. Sie haben die Anmeldeinformationen Gerät in der Clientanwendung später in diesem Lernprogramm einschließen. 

Um ein neues Gerät zu Ihrer remote Überwachung Lösung hinzufügen möchten, führen Sie die folgenden Schritte aus, in dem Dashboard Lösung:

1.  Klicken Sie in der unteren linken Ecke des Dashboards auf **einem Gerät hinzufügen**.

    ![][1]

2.  Klicken Sie im Bereich **Benutzerdefinierte Gerät** klicken Sie auf **neue hinzufügen**.

    ![][2]

3.  **Öffne ich meinen eigenen Geräte-ID definieren**wählen Sie aus, geben Sie eine ID Gerät, beispielsweise **Mydevice**, klicken Sie auf **ID überprüfen** , stellen Sie sicher, dass sich die Namen bereits verwendet nicht, und klicken Sie auf **Erstellen** , um das Gerät bereitstellen.

    ![][3]

5. Notieren Sie sich die Anmeldeinformationen Gerät (Geräte-ID, IoT Hub Hostname und Gerät-Taste), Ihre Clientanwendung benötigt diese Verbindung in die Überwachung remote-Lösung. Klicken Sie dann auf **Fertig**.

    ![][4]

6. Stellen Sie sicher, dass Ihr Gerät im Abschnitt Geräte angezeigt wird. Der Gerätestatus steht **aus** , bis das Gerät eine Verbindung mit der remote Überwachung Solution herstellt.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
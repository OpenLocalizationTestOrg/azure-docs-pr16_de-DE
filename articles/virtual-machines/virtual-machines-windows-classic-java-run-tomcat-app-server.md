<properties
    pageTitle="Auf einem virtuellen Computer Tomcat | Microsoft Azure"
    description="In diesem Lernprogramm erstellte, mit dem Bereitstellungsmodell klassischen Ressourcen verwendet, und zeigt, wie Windows virtuelle Computer erstellen und konfigurieren Sie ihn um Apache Tomcat Anwendungsserver auszuführen."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Ausführen einen Java-Anwendungsserver auf einem Computer mit dem Bereitstellungsmodell klassischen erstellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Mit Azure können Sie einen virtuellen Computer Serverfunktionen bereitstellt. Beispielsweise können eine virtuellen Computern, die auf Windows Azure ausgeführte für die Java-Anwendungsserver, wie z. B. Apache Tomcat hosten konfiguriert werden. Nach Abschluss dieses Handbuch müssen Sie wissen, wie eine virtuellen Computern, die auf Windows Azure ausgeführte erstellen und konfigurieren, um eine Java-Anwendungsserver ausführen.

Lernen Sie Folgendes:

* Zum Erstellen eines virtuellen Computers, das eine Java Development Kit (JDK) bereits installiert wurde.
* Wie bei Ihrem virtuellen Computern remote anmelden.
* Wie Sie einen Java-Anwendungsserver auf des virtuellen Computers zu installieren.
* So erstellen Sie einen Endpunkt für den virtuellen Computer.
* So öffnen Sie einen Port in der Firewall für Ihre Anwendungsserver.

Für die Zwecke dieses Lernprogramms wird ein Apache Tomcat Anwendungsserver auf einem virtuellen Computer installiert. Die abgeschlossene Installation führt zu einer Installation Tomcat, beispielsweise die folgende.

![Virtuellen Computern Apache Tomcat ausgeführt][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Zum Erstellen eines virtuellen Computers

1. Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2. Klicken Sie auf **neu**, klicken Sie auf **zu berechnen**, klicken Sie auf **virtuellen Computern**, und klicken Sie dann auf **Aus Galerie**.
3. Wählen Sie im Dialogfeld **Wählen Sie Bild virtuellen Computern** **JDK 7, Windows Server 2012**.
Beachten Sie, dass **JDK 6 Windows Server 2012** verfügbar ist, wenn Sie ältere Applikationen verfügen, die nicht in JDK 7 ausführen möchten.
4. Klicken Sie auf **Weiter**.
5. Klicken Sie im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Geben Sie einen Namen für den virtuellen Computer an.
    2. Geben Sie die Größe des virtuellen Computers verwenden.
    3. Geben Sie einen Namen für den Administrator im Feld **Benutzername** ein. Denken Sie daran, dieser Name und das Kennwort ein, das Sie als Nächstes eingeben möchten, verwenden Sie diese beim Remote virtuellen Computer anmelden.
    4. Geben Sie ein Kennwort in das Feld **Neues Kennwort ein** , und geben Sie es erneut in das Feld **bestätigen** ein. Dies ist das Kontokennwort Administrator.
    5. Klicken Sie auf **Weiter**.
6. Im nächsten **Konfiguration des virtuellen Computers** Dialogfeld folgendermaßen vor:
    1. **Cloud-Dienst**verwenden Sie die Standardeinstellung **Erstellen einer neuen Cloud-Dienst**aus.
    2. Der Wert für **Cloud-Dienst DNS-Name** muss über cloudapp.net eindeutig sein. Falls erforderlich, ändern Sie diesen Wert, damit die Azure zeigt an, dass er eindeutig ist.
    2. Geben Sie eine Region, Zugehörigkeit Gruppe oder virtuelles Netzwerk. Geben Sie für die Zwecke dieses Lernprogramms einen Bereich, z. B. **Westen US**an.
    2. Wählen Sie für **Speicher-Konto** **Verwenden Sie ein automatisch generiertes Speicherkonto**ein.
    3. Wählen Sie zum **Festlegen der Verfügbarkeit** **(keine)**aus.
    4. Klicken Sie auf **Weiter**.
7. In der endgültigen klicken Sie im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Akzeptieren Sie die standardmäßigen Endpunkt Einträge ein.
    2. Klicken Sie auf **abgeschlossen**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Remote Anmeldung bei Ihrer virtuellen Computern

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **virtuellen Computern**.
3. Klicken Sie auf den Namen des virtuellen Computers, die Sie zum anmelden möchten.
4. Nach dem Start des virtuellen Computers ermöglicht ein Popupmenü am unteren Rand der Seite Verbindungen aus.
5. Klicken Sie auf **Verbinden**.
6. Reagieren Sie auf den Anweisungen, je nach Bedarf Verbindung zum des virtuellen Computers. Dies sollte nach sich ziehen speichern oder öffnen die RDP-Datei, die die Details der Verbindung enthält. Möglicherweise müssen zum Kopieren des Url: Ports als des letzten Teils der ersten Zeile der RDP-Datei, und fügen Sie ihn in einer remote-in-Anwendung.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>So installieren Sie einen Java-Anwendungsserver auf Ihre virtuellen Computern

Können Sie einen Java-Anwendungsserver in Ihrer virtuellen Computern kopieren, oder Sie können einen Java-Anwendungsserver durch ein Installationsprogramm installieren.

Für die im Rahmen dieses Lernprogramms wird Tomcat installiert sein.

1. Wenn Sie sich bei Ihrem virtuellen Computern angemeldet sind, öffnen Sie eine Browsersitzung zu [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Doppelklicken Sie auf den Link für **32-Bit-/ 64-Bit-Windows-Dienst Installer**. Mithilfe dieses Verfahren wird als Windows-Dienst Tomcat installiert.
3. Wenn Sie dazu aufgefordert werden, wählen Sie aus, das Installationsprogramm auszuführen.
4. Führen Sie die Anweisungen zur Installation Tomcat innerhalb des **Apache Tomcat Setup** -Assistenten. Für die Zwecke dieses Lernprogramms ist die Standardwerte zu übernehmen Ordnung. Wenn Sie im Dialogfeld **Abschluss der Apache Tomcat Setup-Assistenten** erreicht haben, können Sie optional **Ausführen Apache Tomcat** Tomcat jetzt beginnen, damit überprüfen. Klicken Sie auf **Fertig stellen** , um den Installationsvorgang Tomcat abzuschließen.

## <a name="to-start-tomcat"></a>So starten Sie Tomcat
Wenn Sie nicht die Tomcat im Dialogfeld **Abschluss der Apache Tomcat Setup-Assistenten** ausführen ausgewählt haben, starten Sie ihn durch Öffnen ein Eingabeaufforderungsfenster auf Ihre virtuellen Computern und **Netz starten Tomcat7**ausgeführt.

Sie sollten jetzt Tomcat ausgeführt, wenn Sie des virtuellen Computers Browser ausführen, und öffnen Sie <http://localhost: 8080>sehen.

Um Tomcat aus externen Computern ausgeführt angezeigt wird, müssen Sie einen Endpunkt erstellen, und öffnen Sie einen Anschluss.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>So erstellen einen Endpunkt für Ihre virtuellen Computern
1. Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2. Klicken Sie auf **virtuellen Computern**.
3. Klicken Sie auf den Namen des virtuellen Computers, die Ihre Java-Anwendungsserver ausgeführt wird.
4. Klicken Sie auf die **Endpunkte**.
5. Klicken Sie auf **Hinzufügen**.
6. Klicken Sie im Dialogfeld **Hinzufügen Endpunkt** sicher **Hinzufügen eigenständigen Endpunkt** ausgewählt ist, und klicken Sie dann auf **Weiter**.
7. Klicken Sie im Dialogfeld **neue Endpunktdetails** :
    1. Geben Sie einen Namen für den Endpunkt; beispielsweise **HttpIn**.
    2. Geben Sie für das Protokoll **TCP** ein.
    3. **80** für den öffentlichen Port angeben.
    4. Geben Sie **8080** für den privaten Port an.
    5. Klicken Sie auf die Schaltfläche " **abgeschlossen** ", um das Dialogfeld zu schließen. Der Endpunkt wird jetzt erstellt werden.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>So öffnen Sie einen Anschluss in die Firewall für Ihre virtuellen Computern
1. Melden Sie sich bei Ihrem virtuellen Computern an.
2. Klicken Sie auf **Windows zu starten**.
3. Klicken Sie auf **Systemsteuerung**.
4. Klicken Sie auf **System und Sicherheit**, klicken Sie auf **Windows-Firewall**, und klicken Sie dann auf **Erweiterte Einstellungen**.
5. Klicken Sie auf **Eingehende Regeln**, und klicken Sie dann auf **Neue Regel**.
 ![Neue eingehende Regel][NewIBRule]
6. Wählen Sie für den **Regeltyp** **Port**aus, und klicken Sie dann auf **Weiter**.
 ![Port für neue eingehende Regel][NewRulePort]
7. Wählen Sie auf dem Bildschirm **Protokoll und Ports** **TCP aus**, geben Sie die **spezifischen lokalen Anschluss** **8080** , und klicken Sie dann auf **Weiter**.
 ![Neue eingehende Regel][NewRuleProtocol]
8. Klicken Sie auf dem Bildschirm **Aktion** wählen Sie **Zulassen die Verbindung**aus, und klicken Sie dann auf **Weiter**.
 ![Neue für eingehende Regelaktion][NewRuleAction]
9. Auf dem Bildschirm **Profil** stellen Sie sicher, dass die **Domäne**, **Private**und **Öffentliche** ausgewählt sind, und klicken Sie dann auf **Weiter**.
 ![Neues Profil für eingehende Regel][NewRuleProfile]
10. Klicken Sie auf dem Bildschirm **Name** Geben Sie einen Namen für die Regel, z. B. **HttpIn** (der Name der Regel wird an den Endpunkt an, jedoch nicht erforderlich), und klicken Sie dann auf **Fertig stellen**.  
 ![Name der neuen eingehende Regel][NewRuleName]

An diesem Punkt Ihre Website Tomcat sollten machen aus einem externen Browser mithilfe einer URL des Formulars * *http://*Ihre\_DNS\_Namen*. cloudapp.net**wobei ** *der\_DNS\_Namen*** der DNS-Namen, die Sie beim Erstellen des virtuellen Computers angegeben ist.

## <a name="application-lifecycle-considerations"></a>Anwendung Lebenszyklus Aspekte
* Erstellen Ihrer eigenen Anwendung Webarchiv (WAR), und fügen Sie es in den Ordner **Webapps** . Beispielsweise erstellen Sie ein grundlegendes Java Service Page (JSP) dynamische Webprojekt und exportieren Sie diese als WAR-Datei, kopieren Sie die WAR in den Apache Tomcat **Webapps** Ordner des virtuellen Computers, führen Sie es in einem Browser.
* Wenn der Tomcat-Dienst installiert ist, ist es Standardeinstellung manuell starten. Sie können sie automatisch starten, indem Sie mit dem Dienste-Snap-in wechseln. Das Services-Snap-in zu starten, indem Sie im **Startmenü von Windows**, **Verwaltung**, und klicken Sie dann **Services**. Doppelklicken Sie auf die **Apache Tomcat** -Dienst, und legen Sie **Starttyp** auf **automatisch**.

    ![Festlegen von einem Dienst für den automatischen start][service_automatic_startup]

    Die Vorteile von Tomcat Start automatisch Probleme ist, dass er gestartet werden kann, wenn der virtuelle Computer neu gestartet wird (beispielsweise nach der Installation der Softwareupdates, die einen Neustart des Computers erforderlich).

## <a name="next-steps"></a>Nächste Schritte
Informationen Sie zu anderen Diensten (z. B. Azure-Speicher, Dienstbus und SQL-Datenbank), die möglicherweise enthalten sein mit einer Java-Anwendung sollen durch die verfügbaren Informationen in der [Java Developer Center](https://azure.microsoft.com/develop/java/)anzeigen.

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png

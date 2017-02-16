<properties
    pageTitle="Einrichten eines virtuellen Computers als IPython Notizbuch Server | Microsoft Azure"
    description="Festlegen von einer Azure virtuellen Computern für die Verwendung in einer Umgebung Wissenschaft mit IPython Server für erweiterte Analytics an."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Einrichten einer Azure-virtuellen Computern als IPython Notizbuch Server für erweiterte analytics

In diesem Thema wird zum Bereitstellen und Konfigurieren einer Azure-virtuellen Computern für erweiterte Analyse, die als Teil einer Wissenschaft-Umgebung verwendet werden können. Die Windows-Computer ist mit Unterstützung von Tools wie z. B. IPython Notizbuch, Azure-Speicher-Explorer, AzCopy sowie andere Dienstprogramme, die für Projekte, die erweiterte Analyse nützlich sind konfiguriert. Azure-Speicher-Explorer und AzCopy, ermöglichen das beispielsweise geeignete Hochladen von Daten in Azure Blob-Speicher von Ihrem lokalen Computer oder von Blob-Speicher auf Ihrem lokalen Computer herunterladen.

## <a name="a-namecreate-vmastep-1-create-a-general-purpose-azure-virtual-machine"></a><a name="create-vm"></a>Schritt 1: Erstellen eines allgemeinen zwecks Azure-virtuellen Computern

Wenn Sie eine Azure-virtuellen Computern bereits und nur von einem Server IPython Notizbuch daran festlegen möchten, können Sie diesen Schritt überspringen und fahren Sie mit [Schritt2: Hinzufügen von außen liegenden Tabellenblättern für IPython Notizbücher zu vorhandenen virtuellen Computer](#add-endpoint).

Bevor Sie beginnen die Vorgehensweise zum Erstellen eines virtuellen Computers auf Azure, müssen Sie die Größe des Computers zu ermitteln, der zum Verarbeiten der Daten für ihr Projekt benötigt wird. Kleiner Rechner weniger Arbeitsspeicher sowie weniger CPUs als größeren Computern, jedoch stehen auch weniger teure. Eine Liste der Computer Dateitypen und Preise finden Sie auf der Seite <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Preise virtuellen Computern</a>

1. Melden Sie sich bei <a href="https://manage.windowsazure.com" target="_blank">Klassischen Azure-Portal</a>, und klicken Sie in der unteren linken Ecke auf **neu** . Ein Fenster wird geöffnet. Wählen Sie **berechnen** -> **virtuellen Computers** -> **FROM-Katalog**.

    ![Arbeitsbereich erstellen][24]

2. Wählen Sie eine der folgenden Bilder:

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials-Benutzeroberfläche (Windows Server 2012 R2)

    Klicken Sie dann auf den Pfeil nach rechts unten rechts auf die nächste Konfigurationsseite zu wechseln.

    ![Arbeitsbereich erstellen][25]

3. Geben Sie einen Namen für den virtuellen Computern, die Sie erstellen möchten, wählen Sie die Größe des Computers (Standard: A3) basierend auf den Umfang der Daten des Computers den Prozess vertraut ist und wie leistungsfähige möchten Sie den Computer (Arbeitsspeichergröße und die Anzahl der Kerne berechnen) sein soll, geben Sie einen Benutzernamen und ein Kennwort für den Computer. Klicken Sie dann auf den Pfeil nach rechts zum Wechseln zur nächsten Konfigurationsseite.

    ![Arbeitsbereich erstellen][26]

4. Wählen Sie das **REGION Zugehörigkeit/Gruppe/virtuellen Netzwerk** , die **Speicher-Konto** , die Sie planen enthält, die für diesen virtuellen Computer verwenden, und wählen Sie dann auf diesem Storage-Konto. Außen liegenden Tabellenblättern am Ende im Feld **ENDPUNKTE** hinzu, indem Sie den Namen des Endpunkts ("IPython" hier) eingeben. Sie können eine beliebige Zeichenfolge als **Namen** für den Endpunkt und eine ganze Zahl zwischen 0 und 65536, die **verfügbar** als **Öffentlicher PORT**auswählen. Der **PRIVATE PORT** muss **9999**werden. Benutzer sollten **vermeiden** öffentliche Ports, die bereits zugewiesen wurden für Internet-Dienste. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Ports für Internet Services</a> bietet eine Liste der Ports, die zugewiesen wurden und sollte vermieden werden.

    ![Arbeitsbereich erstellen][27]

5. Klicken Sie auf das Häkchen zum Starten des virtuellen Computers Prozess bereitgestellt.

    ![Arbeitsbereich erstellen][28]


15-25 Minuten des virtuellen Computers provisioning Prozess dauern. Nach des virtuellen Computers erstellt wurde, sollte der Status des Computers als **ausgeführt**angezeigt.

![Arbeitsbereich erstellen][29]

## <a name="a-nameadd-endpointastep-2-add-an-endpoint-for-ipython-notebooks-to-an-existing-virtual-machine"></a><a name="add-endpoint"></a>Schritt 2: Hinzufügen von außen liegenden Tabellenblättern für IPython Notizbücher zu einer vorhandenen virtuellen Computern

Wenn Sie die virtuellen Computern anhand der Anweisungen in Schritt 1 erstellt haben, klicken Sie dann der Endpunkt für IPython Notizbuch bereits hinzugefügt wurde, und dieser Schritt übersprungen werden kann.

Wenn des virtuellen Computers bereits vorhanden ist, und Sie müssen einen Endpunkt für IPython Notizbuch hinzufügen, die Sie in Schritt 3 anhand der ersten Log in der klassischen Azure-Portal installieren möchten, wählen Sie den virtuellen Computer aus, und fügen Sie den Endpunkt für IPython Notizbuch-Server. Die folgende Abbildung enthält ein Bildschirmabbild des Portals nach der Endpunkt für IPython Notizbuch auf einem Windows-Computer hinzugefügt wurde.

![Arbeitsbereich erstellen][17]

## <a name="a-namerun-commandsastep-3-install-ipython-notebook-and-other-supporting-tools"></a><a name="run-commands"></a>Schritt 3: Installieren Sie IPython Notizbuch und andere unterstützende tools

Nach der Erstellung des virtuellen Computers verwenden Sie (Remotedesktopprotokoll) für die Windows-Computer die Anmeldung bei. Anweisungen finden Sie unter [So melden Sie sich bei einem virtuellen Computern ausführen von Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Öffnen Sie die **Befehlszeile** (**nicht im Powershell-Befehlsfenster**) als **Administrator** aus, und führen Sie den folgenden Befehl aus.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Klicken Sie nach Abschluss die Installation wird automatisch in der Server IPython Notizbuch gestartet der *C:\\Benutzer\\\<Benutzername\>\\Dokumente\\IPython Notizbücher* Directory.

Wenn Sie dazu aufgefordert werden, geben Sie ein Kennwort für das Notizbuch IPython und das Kennwort des der Administrator des Computers ein. Dadurch wird das Notizbuch IPython als Dienst auf dem Computer ausgeführt werden.

## <a name="a-nameaccessastep-4-access-ipython-notebooks-from-a-web-browser"></a><a name="access"></a>Schritt 4: Zugreifen auf IPython Notizbücher mit einem Webbrowser
Öffnen Sie zum Zugreifen auf das Notizbuch IPython Server ein Web Browser und Eingabe *https://&#60;virtual DNS-Computernamen >: & #60; öffentlichen Port-Nummer >* im Textfeld URL. Hier die *& #60; öffentlichen Port-Nummer >* sollten die Port-Nummer, die Sie angegeben haben, wenn der Notizbuch IPython Endpunkt hinzugefügt wurde.

Die *& #60; DNS-Name des virtuellen Computers >* finden Sie in der klassischen Portal Azure. Nachdem Sie die Protokollierung in klassischen-Portal an, klicken Sie auf **virtuellen Computern**, wählen Sie den Computer, die Sie erstellt haben, und wählen Sie dann auf **DASHBOARD**, der DNS-Name wird wie folgt angezeigt:

![Arbeitsbereich erstellen][19]

Warnung angezeigt wird, die _Es liegt ein Problem mit dem Sicherheitszertifikat dieser Website_ (Internet Explorer) oder _die Verbindung ist nicht privat_ (Chrome), werden auftreten, wie in der folgenden Abbildung gezeigt. Klicken Sie auf **Weiter, um diese Website (nicht empfohlen)** (Internet Explorer) oder **Erweitert** und dann * *Weiter zu & #60;* DNS-Namen*> (unsicheren) ** (Chrome), um den Vorgang fortzusetzen. Geben Sie dann das Kennwort ein, die Sie zuvor angegeben haben, um die IPython Notizbücher zuzugreifen.

**InternetExplorer:**
![Arbeitsbereich erstellen][20]

**Chrome:**
![Arbeitsbereich erstellen][21]

Nachdem Sie sich mit dem Notizbuch IPython anmelden, wird ein Verzeichnis *DataScienceSamples* im Browser angezeigt. Dieses Verzeichnis enthält Stichprobe IPython Notizbücher, die von Microsoft, damit Benutzer durchführen Wissenschaft Datentasks freigegeben wurden. Diese Stichprobe IPython Notizbücher werden aus [**Github Repository**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) mit den virtuellen Computern während der Prozess einrichten IPython Notizbuch Server ausgecheckt. Microsoft unterhält und dieses Repository häufig aktualisiert. Benutzer möglicherweise besuchen Sie das Github Repository, um die zuletzt aktualisierten Stichprobe IPython Notizbücher zu erhalten.
![Arbeitsbereich erstellen][18]

## <a name="a-nameuploadastep-5-upload-an-existing-ipython-notebook-from-a-local-machine-to-the-ipython-notebook-server"></a><a name="upload"></a>Schritt 5: Hochladen eines vorhandenen Notizbuchs IPython von einem lokalen Computer auf dem Server IPython Notizbuch

IPython Notizbücher bieten eine einfache Möglichkeit für Benutzer zum Hochladen eines vorhandenen Notizbuchs für IPython auf den lokalen Computern auf dem Server IPython Notizbuch auf den virtuellen Computern. Nachdem Benutzer zum Notizbuch IPython in einem Webbrowser angemeldet haben, klicken Sie auf in **Verzeichnis** , die auf das Notizbuch IPython hochgeladen wird. Wählen Sie dann eine Notizbuch IPython .ipynb Datei zum Hochladen aus dem lokalen Computer im **Datei-Explorer**, und ziehen, und legen sie das Notizbuch IPython Verzeichnis in Ihrem Browser ein. Klicken Sie auf **Hochladen** , um die Datei .ipynb auf dem Server IPython Notizbuch hochzuladen. Andere Benutzer können beginnen Sie ihn in ihrem Webbrowser verwenden.

![Arbeitsbereich erstellen][22]

![Arbeitsbereich erstellen][23]


##<a name="a-nameshutdownashutdown-and-de-allocate-virtual-machine-when-not-in-use"></a><a name="shutdown"></a>War(en) und heben Sie die Zuordnung virtuellen Computern nicht in verwenden

Der Preis von Azure-virtuellen Computern sind als **bezahlen nur für was Sie verwenden**. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden sind während des virtuellen Computers nicht verwenden, muss es in den Zustand **beendet (Deallocated)** nicht in verwenden werden.

> [AZURE.NOTE] Wenn Sie des virtuellen Computers von innerhalb des virtuellen Computers beenden (mit Windows Power Optionen), der virtuellen Computer ist nicht mehr bleibt jedoch zugewiesenen. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden weiterhin, beenden Sie immer virtuellen Computern vom [Klassischen Azure-Portal](http://manage.windowsazure.com/). Sie können auch den virtuellen Computer über Powershell beenden, indem Sie **ShutdownRoleOperation** mit "PostShutdownAction" gleich "StoppedDeallocated" aufrufen.

Beenden und des virtuellen Computers freigeben:

1. Melden Sie sich mit Ihrem Konto [Klassischen Azure-Portal](http://manage.windowsazure.com/) an.  

2. Wählen Sie in der linken Navigationsleiste **virtuellen Computern** aus.

3. Klicken Sie auf den Namen des virtuellen Computers, und wechseln Sie zu der Seite **DASHBOARD** , in der Liste von virtuellen Computern.

4. Klicken Sie am unteren Rand der Seite auf **Beenden**.

![Virtueller Computer war(en)][15]

Des virtuellen Computers werden freigegeben, aber nicht gelöscht. Sie können Ihre virtuellen Computers zu einem beliebigen Zeitpunkt vom klassischen Azure-Portal neu starten.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Ihre Azure-virtuellen Computer verwendet wird: Was nächsten ist?

Ihre virtuellen Computern kann nun in Ihrer Daten Wissenschaft Übungen verwenden. Des virtuellen Computers ist auch für die Verwendung als IPython Notizbuch-Server für die Untersuchung und Bearbeitung der Daten sowie zu anderen Aufgaben in Verbindung mit Azure maschinellen Lern- und Team von Wissenschaft Daten bereit.

Die nächsten Schritte beim Team Wissenschaft Daten werden in den [Pfad Learning](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet und Schritte, die Verschieben von Daten in HDInsight, verarbeiten und dort Vorbereitung Learning aus den Daten mit den Azure Computer vertraut machen (Beispiel) einschließen.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png

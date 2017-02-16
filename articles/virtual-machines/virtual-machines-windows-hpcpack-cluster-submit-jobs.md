<properties
 pageTitle="Übermitteln Aufträge auf ein HPC Pack cluster in Azure | Microsoft Azure"
 description="Informationen Sie zum Einrichten eines lokalen Computers zu einem Cluster HPC Pack in Azure Aufträge senden"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Übermitteln Sie HPC Aufträge von einem lokalen Computer zu einem HPC Pack Cluster in Azure bereitgestellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfigurieren eines lokalen Clientcomputers um Einzelvorgänge zu einem [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Cluster in Azure zu übergeben. In diesem Artikel wird das Einrichten eines lokalen Computers mit Clienttools Position über HTTPS und übermitteln Sie zum Cluster in Azure veranschaulicht. Auf diese Weise können mehrere Cluster Benutzer Aufträge eines cloudbasierten HPC Pack Cluster, aber ohne direkt an den am Knoten virtueller Computer anschließen oder den Zugriff auf ein Abonnement Azure einreichen.

![Senden eines Auftrags mit einem Cluster in Azure][jobsubmit]

## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack am Knoten bereitgestellt auf einer Azure-virtuellen Computer** - Es empfiehlt sich, dass Sie automatisierte Tools, wie etwa einer [Azure Schnellstart Vorlage](https://azure.microsoft.com/documentation/templates/) oder eine [Azure PowerShell-Skript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) verwenden, um die am Knoten und Cluster bereitstellen. Sie benötigen den DNS-Namen des am Knotens und die Anmeldeinformationen eines Administrators Cluster die Schritte in diesem Artikel ausführen.

* **Clientcomputer** - benötigten auf einem Windows oder Windows Server-Clientcomputer, die Clienttools HPC Pack ausgeführt werden kann (siehe [Systemanforderungen](https://technet.microsoft.com/library/dn535781.aspx)). Wenn Sie nur die HPC Pack Web-Portal oder die REST-API verwenden, Aufträge senden möchten, können Sie alle Clientcomputer Ihrer Wahl.

* **HPC Pack Installation Media** -, installieren die HPC Pack-Clienttools, die kostenlose Installationspaket für die neueste Version von HPC Pack (HPC Pack 2012 R2) steht im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Stellen Sie sicher, dass Sie dieselbe Version von HPC Pack herunterladen, die auf dem am Knoten virtuellen Computer installiert ist.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Schritt 1: Installieren Sie und konfigurieren Sie die Web Components auf dem am Knoten

Stellen Sie sicher, dass die HPC Pack Web Components auf dem HPC Pack am Knoten konfiguriert sind, klicken Sie zum Aktivieren einer REST-Schnittstelle Aufträge zum Cluster über HTTPS senden. Wenn sie noch nicht installiert, müssen Sie zuerst installieren Sie die Web Components durch Ausführen der HpcWebComponents.msi Installationsdatei. Klicken Sie dann konfigurieren Sie die Komponenten durch Ausführen des HPC PowerShell-Skripts **Festlegen-HPCWebComponents.ps1**.

Ausführliche Anweisungen finden Sie unter [Microsoft HPC Pack Web Components zu installieren](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Bestimmte Vorlagen Azure Schnellstart für HPC Pack installieren und konfigurieren die Web Components automatisch. Wenn Sie das [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) verwenden, um den Cluster zu erstellen, können Sie optional installieren und konfigurieren die Web Components als Teil der Bereitstellung.

**So installieren Sie die Web components**

1. Verbinden Sie mit dem am Knoten virtueller Computer mit den Anmeldeinformationen eines Administrators Cluster.

2. Führen Sie im Ordner HPC Pack Setup HpcWebComponents.msi auf dem am Knoten ein.

3. Führen Sie die Schritte im Assistenten, um die Web Components zu installieren.

**So konfigurieren Sie die Web components**

1. Starten Sie auf die am Knoten HPC PowerShell als Administrator aus.

2. Um Verzeichnis zum Speicherort der das Skript zu ändern, geben Sie den folgenden Befehl aus:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Zum Konfigurieren der REST-Schnittstelle und zum Starten der HPC-Webdienst, geben Sie den folgenden Befehl aus:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Wenn Sie dazu aufgefordert werden, um ein Zertifikat auszuwählen, wählen Sie das Zertifikat, das den öffentlichen DNS-Namen des am Knotens entspricht. Beispielsweise, wenn Sie den am Knoten virtueller Computer mit dem Bereitstellungsmodell klassischen bereitstellen, der Name des Zertifikats sieht CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden, sieht der Name des Zertifikats CN =&lt;*HeadNodeDnsName*&gt;. &lt; *Region*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Sie wählen dieses Zertifikat später beim Übermitteln Aufträge zum am Knoten von einem lokalen Computer. Nicht aktivieren oder konfigurieren Sie ein Zertifikat, das den Computernamen, der den am Knoten in der Active Directory-Domäne entspricht (z. B. CN =*MyHPCHeadNode.HpcAzure.local*).

5. Um das Webportal Senden des Auftrags konfigurieren möchten, geben Sie den folgenden Befehl aus:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nach Abschluss des Skripts beenden Sie, und starten Sie den HPC Job Scheduler-Dienst, indem Sie die folgenden Befehle eingeben:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Schritt 2: Installieren Sie die Clienttools HPC Pack auf einem lokalen computer

Wenn Sie die HPC Pack-Clienttools auf Ihrem Computer installieren möchten, laden Sie die HPC Pack Setup-Dateien (vollständige Installation) aus dem [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Wenn Sie die Installation zu starten, wählen Sie die Setupoption für die **HPC Pack-Clienttools**.

Wenn die Clienttools HPC Pack Aufträge zum am Knoten virtueller Computer senden verwenden möchten, müssen Sie auch exportieren Sie ein Zertifikat aus dem am Knoten, und installieren es auf dem Clientcomputer. Das Zertifikat muss sein. CER-Format.

**So exportieren Sie das Zertifikat aus dem am Knoten**

1. Fügen Sie auf dem am Knoten das Zertifikate-Snap-in zu einer Microsoft Management Console für den lokalen Computer-Konto hinzu. Schritte zum Snap-in hinzufügen finden Sie unter [Zertifikate-Snap-in einer MMC hinzufügen](https://technet.microsoft.com/library/cc754431.aspx).

2. Erweitern Sie im Strukturbaum **Zertifikate – lokaler Computer** > **Personal**, und klicken Sie dann auf **Zertifikate**.

3. Suchen Sie das Zertifikat, das Sie für die HPC Pack Web Components in konfiguriert [Schritt 1: Installieren und konfigurieren Sie die Web Components auf dem am Knoten](#step-1:-install-and-configure-the-web-components-on-the-head-node) (z. B. CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Mit der rechten Maustaste in des Zertifikats, und klicken Sie auf **Alle Vorgänge** > **Exportieren**.

5. Klicken Sie im Zertifikat Export-Assistenten auf **Weiter**, und stellen Sie sicher, dass **Nein, privaten Schlüssel nicht exportieren** ausgewählt ist.

6. Führen Sie die restlichen Schritte des Assistenten aus, um das Zertifikat in DER-codierte binäre x. 509 exportieren (. CER)-Format.


**So importieren Sie das Zertifikat auf dem Clientcomputer**


1. Kopieren Sie das Zertifikat, das Sie aus dem am Knoten mit einem Ordner auf dem Clientcomputer exportiert.

2. Führen Sie auf dem Clientcomputer certmgr.msc ein.

3. Erweitern Sie die **Zertifikate-Aktueller Benutzer**in Zertifikat Manager > **Trusted Root Zertifizierungsstellen**, mit der rechten Maustaste **Zertifikate**, und klicken Sie dann auf **Alle Vorgänge** > **Importieren**.

4. Das Zertifikat Import-Assistenten auf **Weiter** , und führen Sie die Schritte aus, um das Zertifikat zu importieren, das Sie aus dem am Knoten zum Trusted Root Zertifizierungsstellen Store exportiert.



>[AZURE.TIP] Möglicherweise angezeigt Warnung eines Wertpapiers zurück, da die Zertifizierungsstelle auf dem am Knoten vom Clientcomputer erkannt nicht zur Verfügung. Zu Testzwecken können Sie diese Warnung ignorieren und Abschließen den Import des Zertifikats.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Schritt 3: Ausführen Testaufträge im Cluster

Zum Überprüfen der Konfigurations, versuchen Sie es im Cluster in Azure Aufträge aus dem lokalen Computer ausgeführten. Beispielsweise können Sie mit dem Cluster Aufträge senden HPC Pack Benutzeroberfläche Tools oder Befehlen. Sie können auch eine Web-basierte Portal verwenden, Aufträge senden.


**Befehle, die Übermittlung auf dem Clientcomputer ausführen**


1. Starten Sie auf einem Clientcomputer, auf dem die Clienttools HPC Pack installiert werden, ein Eingabeaufforderungsfenster.

2. Beispiel für den Befehl eingeben. Geben Sie beispielsweise um eine Liste aller Projekte auf dem Cluster ähnlich der folgenden Werte, je nach den vollständigen DNS-Namen des am Knotens Befehl aus:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    oder
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Verwenden Sie den vollständigen DNS-Namen des am Knotens, nicht die IP-Adresse, in der Scheduler-URL ein. Wenn Sie die IP-Adresse angeben, wird ein Fehler ähnlich wie "muss das Serverzertifikat entweder eine gültige Kette des Trust haben oder im Speicher vertrauenswürdiges platziert werden."

3. Wenn Sie dazu aufgefordert werden, geben Sie den Benutzernamen (im Formular &lt;Domänenname&gt;\\&lt;Benutzername&gt;) und das Kennwort für den HPC Cluster-Administrator oder ein anderer Cluster-Benutzer, die Sie konfiguriert. Sie können auch die Anmeldeinformationen lokal für weitere Position Vorgänge zu speichern.

    Eine Liste der Aufträge angezeigt.


**Verwenden von HPC Auftrags-Manager auf dem Clientcomputer**

1. Wenn Sie Anmeldeinformationen für die Domäne für einen Benutzer Cluster beim Übermitteln eines Auftrags zuvor gespeichert haben, können Sie die Anmeldeinformationen in Anmeldeinformationsverwaltung hinzufügen.

    ein. Beginnen Sie in der Systemsteuerung auf dem Clientcomputer Anmeldeinformationsverwaltung.

    b. Klicken Sie auf **Windows-Anmeldeinformationen** > **Hinzufügen eine generische Anmeldeinformationen**.

    c. Geben Sie die Internetadresse (z. B. https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler oder https://&lt;HeadNodeDnsName&gt;.&lt; Region&gt;.cloudapp.azure.com/HpcScheduler), und der Benutzername (&lt;Domänenname&gt;\\&lt;Benutzername&gt;) und das Kennwort für den Cluster-Administrator oder ein anderer Cluster-Benutzer, die Sie konfiguriert.

2. Starten Sie auf dem Clientcomputer HPC Auftrags-Manager.

3. Im Dialogfeld **Kopf-Knoten auswählen** , geben Sie die URL zum am Knoten in Azure (beispielsweise https://&lt;HeadNodeDnsName&gt;. cloudapp.net oder https://&lt;HeadNodeDnsName&gt;.&lt; Region&gt;. cloudapp.azure.com).

    HPC Auftrags-Manager wird geöffnet und zeigt eine Liste der Aufträge auf dem am Knoten.

**Verwenden das Web-Portal auf dem am Knoten ausgeführt**

1. Starten Sie einen Webbrowser auf dem Clientcomputer, und geben Sie eine der folgenden Adressen, je nach den vollständigen DNS-Namen des Knotens am:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    oder
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Geben Sie die Anmeldeinformationen für die Domäne der HPC Cluster-Administrator, klicken Sie im Dialogfeld Sicherheit. (Sie können auch anderen Benutzern Cluster in verschiedenen Rollen hinzufügen. Finden Sie unter [Verwalten von Benutzern Cluster](https://technet.microsoft.com/library/ff919335.aspx).)

    Das Web-Portal wird geöffnet, in der Listenansicht Position.

3. Um eine Stichprobe Auftrag übermitteln, der die Zeichenfolge "Hallo Welt" aus dem Cluster zurückgibt, klicken Sie im linken Navigationsbereich auf **neue Position** .

4. Klicken Sie auf der Seite **Neue Position** unter **von Einreichung Seiten**auf **HelloWorld**. Die Auftrag Absenden der Seite angezeigt wird.

5. Klicken Sie auf **Absenden**. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für die Domäne der HPC Cluster-Administrator. Der Auftrag gesendet wird, und die Auftrags-ID, die auf der Seite **Meine Projekte** angezeigt wird.

6. Zum Anzeigen der Ergebnisse des Projekts, das Sie übermittelt, klicken Sie auf die Auftrags-ID, und klicken Sie dann auf **Aufgaben anzeigen** , um die Ausgabe des Befehls (klicken Sie unter **Ausgabe**) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Sie können auch Aufträge zum Azure Cluster mit [HPC Pack REST-API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)senden.

* Wenn Sie einen Linux-Client-Clusteraufträge senden möchten, finden Sie unter der Stichprobe Python [HPC Pack 2012 R2 SDK und Beispiel-Code](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png

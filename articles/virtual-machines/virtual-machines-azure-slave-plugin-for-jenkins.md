<properties
    pageTitle="Verwendung der Azure-Plug-Ins untergeordnete mit fortlaufender Integration Jenkins | Microsoft Azure"
    description="Beschreibt, wie der Azure-Plug-Ins untergeordnete mit fortlaufender Integration Jenkins verwenden."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Verwenden Sie die Plug-in-Azure untergeordnete mit Jenkins fortlaufende Integration

Können die Azure untergeordnete-Plug-In für Jenkins bereitstellen untergeordnete Knoten auf Azure beim Ausführen von verteilten erstellt.

## <a name="install-the-azure-slave-plug-in"></a>Installieren der Azure untergeordnete-Plug-in

1. Klicken Sie auf dem Dashboard Jenkins **Jenkins verwalten**.

1. Klicken Sie auf der Seite **Jenkins verwalten** auf **Verwalten-Plug-Ins**.

1. Klicken Sie auf der Registerkarte **verfügbar** .

1. Geben Sie im Filterfeld die Liste der verfügbaren Plug-Ins **Azure** einschränken die Liste, um relevante-Plug-ins.

    Wenn Sie sich entscheiden, um einen Bildlauf durch die Liste der verfügbaren Plug-Ins durch, finden Sie folgende Angaben der untergeordnete Azure-Plug-Ins unter dem Abschnitt **Cluster Verwaltungs- und verteilt erstellen** .

1. Aktivieren Sie das Kontrollkästchen **Azure untergeordnete-Plug-in** .

1. Klicken Sie auf **Installieren ohne Neustart** oder **jetzt herunterladen und installieren Sie nach dem Neustart**.

Nachdem Sie nun das plug-in installiert ist, sind die nächsten Schritte mit Ihrem Abonnementprofil Azure-des Plug-Ins konfigurieren und erstellen Sie eine Vorlage, die verwendet werden in den virtuellen Computern für den untergeordnete Knoten erstellen.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurieren der Azure-Plug-Ins untergeordnete mit Ihrem Abonnementprofil

Ein Abonnementprofil, auch als bezeichnet Einstellungen veröffentlichen ist eine XML-Datei, die sichere Anmeldeinformationen und einige zusätzliche Informationen benötigen Sie für die Arbeit mit Azure in Ihrer Entwicklungsumgebung enthält. Zum Konfigurieren der Azure untergeordnete-Plug-Ins müssen Sie folgende Aktionen ausführen:

* Ihr Abonnement-id
* Ein Zertifikat Management für Ihr Abonnement

Diese können in Ihrem [Abonnementprofil]gefunden werden. Es folgt ein Beispiel für ein Abonnementprofil.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nachdem Sie Ihr Abonnementprofil haben, folgendermaßen Sie vor, um die Azure untergeordnete-Plug-Ins zu konfigurieren:

1. Klicken Sie auf dem Dashboard Jenkins **Jenkins verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Führen Sie einen Bildlauf nach unten zum Abschnitt **Cloud** suchen.

1. Klicken Sie auf **Hinzufügen neuer Cloud > Microsoft Azure**.

    ![Cloud-Abschnitt][cloud section]

    Hieraus sehen die Felder, in dem Sie Ihre Abonnementdetails eingeben müssen.

    ![Abonnement-Konfiguration][subscription configuration]

1. Kopieren Sie die Abonnement-Id und Verwaltung Zertifikatwerte aus Ihrem Abonnementprofil, und fügen Sie diese in die entsprechenden Felder.

    Wenn Sie das Abonnement-Id und Verwaltung Zertifikat kopieren, enthalten Sie nicht die Angebote auf, die die Werte zu setzen.

1. Klicken Sie auf, **Überprüfen Sie die Konfiguration**.

1. Wenn die Konfiguration um korrekt zu sein überprüft wird, klicken Sie auf **Speichern**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Einrichten einer Vorlage für virtuelle Computer für die Azure untergeordnete-Plug-in

Vorlage für einen virtuellen Computer definiert den Parameter, die das plug-in verwendet werden, um eine untergeordnete Knoten auf Azure zu erstellen. In den folgenden Schritten erstellen wir eine Vorlage für eine Ubuntu virtuellen Computers.

1. Klicken Sie auf dem Dashboard Jenkins **Jenkins verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Führen Sie einen Bildlauf nach unten zum Abschnitt **Cloud** suchen.

1. Klicken Sie im Abschnitt **Cloud** suchen Sie **Azure-virtuellen Computern-Vorlage hinzufügen**, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen von virtuellen Computer-Vorlage][add vm template]

    Dadurch werden die Felder angezeigt, in dem Sie Details zur Vorlage eingeben, die Sie erstellen.

    ![leere allgemeine Konfiguration][blank general configuration]

1. Geben Sie im Feld **Name** einen Namen der Azure-Cloud-Dienst aus. Wenn der eingegebene Name zu einem vorhandenen Clouddienst verweist, wird der virtuellen Computern in diesem Dienst bereitgestellt werden. Einen neuen erstellen Sie andernfalls Azure.

1. Geben Sie im Feld **Beschreibung** Text, der die Vorlage beschreibt, die Sie erstellen. Dies trifft nur für Ihre Einträge und wird nicht in der Bereitstellung eines virtuellen Computers verwendet.

1. Das Kontrollkästchen **Elementnamen** wird verwendet, um die Vorlage zu identifizieren, die Sie erstellen und anschließend zum Verweisen auf der Vorlage beim Erstellen eines Auftrags Jenkins verwendet. Geben Sie für unsere Zwecke **Linux** in diesem Feld ein.

1. Klicken Sie in der Liste **Region** auf den Bereich, in dem die virtuellen Computern erstellt wird.

1. Klicken Sie in der Liste **Größe des virtuellen Computers** auf die entsprechende Größe.

1. Geben Sie im Feld **Kontoname Speicher** ein Speicherkonto, wo des virtuellen Computers erstellt werden soll. Stellen Sie sicher, dass sie in der gleichen Region als Cloud-Dienst ist, die Sie verwenden möchten. Wenn Sie neuen Speicher erstellt werden soll, können Sie dieses Feld leer lassen.

1. Aufbewahrungsrichtlinien Zeit gibt die Anzahl der Minuten anzugeben, bevor Jenkins einer im Leerlauf untergeordnete löscht an. Beibehalten des Standardwerts 60. Sie können auch auswählen, fahren Sie die untergeordnete anstatt ihn zu löschen, wenn er nicht verwendet wird. Wählen Sie dazu das Kontrollkästchen **Nur war(en) (nicht löschen) nach Aufbewahrung Uhrzeit** ein.

1. Klicken Sie auf die entsprechende Bedingung, in der Liste **Verwendung** bei diesem untergeordnete Knoten verwendet werden soll. Klicken Sie jetzt auf **Nutzen dieser Knoten so weit wie möglich**.

    An diesem Punkt sollte das Formular etwa so aussehen:

    ![Wissensstand Vorlage Allgemein config][checkpoint general template config]

    Im nächsten Schritt wird Details zu dem Bild Betriebssystem bereit, die Ihre untergeordnete in erstellt werden sollen.

1. Im **Bild Familie oder -Id** müssen Sie angeben, welche Systemabbild auf dem virtuellen Computer installiert werden soll. Sie können aus einer Liste von Familien Bild auswählen oder ein benutzerdefiniertes Bild anzugeben.

    Wenn Sie aus einer Liste von Familien Bild auswählen möchten, geben Sie das erste Zeichen des Namens Familie Bild (Groß-/Kleinschreibung). **U** eingeben wird beispielsweise um eine Liste der Ubuntu Server Familien anzuzeigen. Nachdem Sie in der Liste auswählen, wird die neueste Version von, die diesem Systemabbild aus dieser Familie Jenkins verwendet, beim Bereitstellen des virtuellen Computers.

    ![Beispiel für OS Bild-Liste][OS Image list sample]

    Wenn Sie ein benutzerdefiniertes Bild, das Sie stattdessen verwenden möchten haben, geben Sie den Namen des benutzerdefinierten Bilds. Benutzerdefinierten Bildnamen werden in einer Liste nicht angezeigt, Sie haben, um sicherzustellen, dass der Name korrekt eingegeben wurde.

    In diesem Lernprogramm Typ **U** , um eine Liste der Ubuntu Bilder zu öffnen, und klicken Sie dann auf **Ubuntu Server 14.04 LTS**.

1. Klicken Sie in der Liste **Starten Methode** **SSH**auf.

1. Kopieren Sie das Skript unten, und fügen Sie ihn in das Feld **Initialisierung Skript** ein.

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Das Skript Initialisierung wird nach der Erstellung des virtuellen Computers ausgeführt. In diesem Beispiel werden das Skript Java, Git und Ant installiert.

1. Geben Sie Ihre bevorzugten Werte in den Feldern **Benutzername** und **Kennwort** für das Administratorkonto, das auf Ihre virtuellen Computern erstellt wird.

1. Klicken Sie auf **Überprüfen Vorlage** zu überprüfen, ob die angegebene Parameter gültig sind.

1. Klicken Sie auf **Speichern**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Erstellen eines Auftrags für Jenkins, das auf eine untergeordnete Knoten Azure ausgeführt wird.

In diesem Abschnitt werden Sie einen Vorgang Jenkins erstellen, der auf eine untergeordnete Knoten auf Azure ausgeführt werden soll. Sie müssen Ihr eigenes Projekt von GitHub entlang Folgen haben.

1. Klicken Sie auf **Neues Element**, in dem Dashboard Jenkins.

1. Geben Sie einen Namen für den Vorgang, den Sie erstellen.

1. Klicken Sie auf den Projekttyp **Freestyle Projekt**.

1. Klicken Sie auf **Ok**.

1. Wählen Sie die Seite Task-Konfiguration **einschränken, wo dieses Projekt ausgeführt werden kann**.

1. Geben Sie im Feld **Beschriftungsausdruck** **Linux**. Im vorherigen Abschnitt erstellt eine untergeordnete Vorlage, dass wir heißt **Linux**, die wir hier angegeben haben.

1. Klicken Sie im Abschnitt **Erstellen** klicken Sie auf **Add erstellen Schritt** , und wählen Sie **Verwaltungsshell ausführen**.

1. Bearbeiten Sie das folgende Skript, und Ersetzen **(Ihr Name GitHub-Konto)**, **(der Name Ihres Projekts)**und **(Ordner Ihres Projekts)** mit den entsprechenden Werten, und fügen Sie das bearbeitete Skript im Textbereich, der angezeigt wird.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klicken Sie auf **Speichern**.

1. Zeigen Sie im Dashboard Jenkins auf die Aufgabe, die Sie soeben erstellt haben, und klicken Sie auf den Dropdown-Pfeil um Aufgabenoptionen anzuzeigen.

1. Klicken Sie auf **jetzt erstellen**.

Jenkins anschließend erstellen einen untergeordnete Knoten mithilfe der im vorherigen Abschnitt erstellten Vorlage und führen Sie das Skript, die, das Sie im Schritt erstellen für diese Aufgabe angegeben haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/
[Abonnementprofil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png
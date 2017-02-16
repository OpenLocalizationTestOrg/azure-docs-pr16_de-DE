<properties
    pageTitle="Verwendung der Azure-Plug-Ins untergeordnete mit fortlaufender Integration Hudson | Microsoft Azure"
    description="Beschreibt, wie der Azure-Plug-Ins untergeordnete mit fortlaufender Integration Hudson verwenden."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Verwenden Sie die Plug-in-Azure untergeordnete mit Hudson fortlaufende Integration

Das plug-in für Hudson Azure untergeordnete können Sie untergeordnete Knoten auf Azure bereitstellen, beim Ausführen von verteilten erstellt.

## <a name="install-the-azure-slave-plug-in"></a>Installieren der untergeordnete Azure-Plug-in

1. Klicken Sie auf dem Dashboard Hudson **Hudson verwalten**.

1. Klicken Sie auf der Seite **Hudson verwalten** auf **Verwalten-Plug-Ins**.

1. Klicken Sie auf der Registerkarte **verfügbar** .

1. Klicken Sie auf **Suchen** , und geben Sie **Azure** zum Einschränken der Liste, um relevante-Plug-ins.

    Wenn Sie sich entscheiden, um einen Bildlauf durch die Liste der verfügbaren Plug-Ins durch, finden der untergeordnete Azure-Plug-in unter dem Abschnitt **Cluster Verwaltungs- und verteilt erstellen** Sie in der Registerkarte **andere** .

1. Aktivieren Sie das Kontrollkästchen für **Azure untergeordnete Plug-in**.

1. Klicken Sie auf **Installieren**.

1. Starten Sie Hudson neu.

Nachdem Sie nun das plug-in installiert ist, wäre den nächsten Schritten fort, mit Ihrem Abonnementprofil Azure-des Plug-Ins konfigurieren und erstellen Sie eine Vorlage, die verwendet werden soll, in den virtuellen Computer für die untergeordnete Knoten erstellen.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurieren der untergeordnete Azure-Plug-Ins mit Ihrem Abonnementprofil

Ein Abonnementprofil, auch als bezeichnet Einstellungen veröffentlichen ist eine XML-Datei, die sichere Anmeldeinformationen und einige zusätzliche Informationen benötigen Sie für die Arbeit mit Azure in Ihrer Entwicklungsumgebung enthält. Zum Konfigurieren der Azure untergeordnete-Plug-Ins müssen Sie folgende Aktionen ausführen:

* Ihr Abonnement-id
* Ein Zertifikat Management für Ihr Abonnement

Diese können in Ihrem [Abonnementprofil]gefunden werden. Es folgt ein Beispiel für ein Abonnementprofil.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nachdem Sie Ihr Abonnementprofil haben, folgendermaßen Sie vor, um die Azure untergeordnete-Plug-in zu konfigurieren.

1. Klicken Sie auf dem Dashboard Hudson **Hudson verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Führen Sie einen Bildlauf nach unten zum Abschnitt **Cloud** suchen.

1. Klicken Sie auf **Hinzufügen neuer Cloud > Microsoft Azure**.

    ![Hinzufügen von neuen cloud][add new cloud]

    Hieraus sehen die Felder, in dem Sie Ihre Abonnementdetails eingeben müssen.

    ![Profil konfigurieren][configure profile]

1. Kopieren Sie das Abonnement-Id und Verwaltung Zertifikat aus Ihrem Abonnementprofil, und fügen Sie diese in die entsprechenden Felder.

    Wenn Sie das Abonnement-Id und Verwaltung Zertifikat kopieren, beifügen **nicht** die Angebote auf, die die Werte zu setzen.

1. Klicken Sie auf **Konfiguration überprüfen**.

1. Wenn die Konfiguration erfolgreich überprüft wird, klicken Sie auf **Speichern**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Einrichten einer Vorlage für virtuelle Computer für die untergeordnete Azure-Plug-in

Vorlage für einen virtuellen Computer definiert die Parameter, die das plug-in verwendet wird, um eine untergeordnete Knoten auf Azure zu erstellen. In den folgenden Schritten werden wir Vorlage für eine VM Ubuntu erstellen.

1. Klicken Sie auf dem Dashboard Hudson **Hudson verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Führen Sie einen Bildlauf nach unten zum Abschnitt **Cloud** suchen.

1. Klicken Sie innerhalb des Abschnitts **Cloud** suchen Sie **Azure-virtuellen Computern-Vorlage hinzufügen** zu, und klicken Sie auf die Schaltfläche **Hinzufügen** .

    ![Hinzufügen von virtuellen Computer-Vorlage][add vm template]

1. Geben Sie im Feld **Name** einen Cloud-Dienstnamen ein. Wenn der Name, den Sie angeben, um einen vorhandenen Clouddienst verweist, wird der virtuellen Computer in diesen Dienst bereitgestellt werden. Einen neuen erstellen Sie andernfalls Azure.

1. Geben Sie in das Feld **Beschreibung** den Text, der die Vorlage beschreibt, die Sie erstellen. Diese Informationen ist nur für Unterlagen Zwecke und wird nicht in der Bereitstellung eines virtuellen Computers verwendet.

1. Geben Sie im Feld **Etiketten** **Linux**. Diese Beschriftung wird verwendet, um die Vorlage zu identifizieren, die Sie erstellen und anschließend zum Verweisen auf der Vorlage beim Erstellen eines Auftrags Hudson verwendet.

1. Wählen Sie einen Bereich, in der virtuellen Computer erstellt werden soll.

1. Wählen Sie die entsprechenden virtueller Speicher aus.

1. Geben Sie einen Speicher an, in der virtuellen Computer erstellt werden soll. Stellen Sie sicher, dass sie in der gleichen Region als Cloud-Dienst ist, die Sie verwenden möchten. Wenn Sie neuen Speicher erstellt werden soll, können Sie dieses Feld leer lassen.

1. Aufbewahrungsrichtlinien Zeit gibt die Anzahl der Minuten anzugeben, bevor Hudson einer im Leerlauf untergeordnete löscht an. Beibehalten des Standardwerts 60.

1. Wählen Sie die entsprechende Bedingung **Verwendung**bei diesem untergeordnete Knoten verwendet werden soll. Wählen Sie jetzt **Nutzen dieser Knoten so weit wie möglich**aus.

    An diesem Punkt sollte das Formular etwa so aussehen:

    ![Vorlage config][template config]

1. In der **Abbildung Familie oder -Id** müssen Sie angeben, welche Systemabbild Ihrer virtuellen Computers installiert werden soll. Sie können aus einer Liste von Familien Bild auswählen oder ein benutzerdefiniertes Bild anzugeben.

    Wenn Sie aus einer Liste von Familien Bild auswählen möchten, geben Sie das erste Zeichen des Namens Familie Bild (Groß-/Kleinschreibung). **U** eingeben wird beispielsweise um eine Liste der Ubuntu Server Familien anzuzeigen. Nachdem Sie in der Liste auswählen, wird die neueste Version von, die diesem Systemabbild aus dieser Familie Jenkins verwendet, beim Bereitstellen von Ihrem virtuellen Computer.

    ![OS familiäre Liste][OS family list]

    Wenn Sie ein benutzerdefiniertes Bild, das Sie stattdessen verwenden möchten haben, geben Sie den Namen des benutzerdefinierten Bilds. Benutzerdefiniertes Bildnamen werden in einer Liste nicht angezeigt, Sie haben, um sicherzustellen, dass der Name korrekt eingegeben wurde.    

    Geben Sie in diesem Lernprogramm **U** , um eine Liste von Bildern Ubuntu aufzurufen, und wählen Sie **Ubuntu Server 14.04 LTS**ein.

1. Wählen Sie **SSH** **Methode starten**.

1. Kopieren Sie das folgenden Skript, und fügen Sie im Feld **Initialisierung Skript** .

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

    Der **Initialisierung Skript** wird ausgeführt werden, nachdem der virtuellen Computer erstellt wurde. In diesem Beispiel werden das Skript Java, Git und Ant installiert.

1. Geben Sie Ihre bevorzugten Werte in den Feldern **Benutzername** und **Kennwort** für das Administratorkonto, das Ihre virtuellen Computers erstellt wird.

1. Klicken Sie auf die **Vorlage überprüfen** prüfen, ob die angegebene Parameter gültig sind.

1. Klicken Sie auf **Speichern**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Erstellen eines Auftrags für Hudson, das auf eine untergeordnete Knoten Azure ausgeführt wird.

In diesem Abschnitt werden Sie einen Vorgang Hudson erstellen, der auf eine untergeordnete Knoten auf Azure ausgeführt werden soll.

1. Klicken Sie auf dem Dashboard Hudson auf **Neue Position**.

1. Geben Sie einen Namen für das Projekt, das Sie erstellen.

1. Wählen Sie für den Auftrag ein **Erstellen Sie ein Projekt - Schreibweise Software**aus.

1. Klicken Sie auf **OK**.

1. Wählen Sie in der Seite Position Konfiguration **einschränken dieses Projekt Ausführungsort**aus.

1. Wählen Sie **Knoten und Bezeichnung Menü** und wählen Sie **Linux** (wir angegeben diese Beschriftung, wenn die Vorlage virtuellen Computern im vorherigen Abschnitt erstellen).

1. Klicken Sie im Abschnitt **Erstellen** klicken Sie auf **Add erstellen Schritt** , und wählen Sie **Verwaltungsshell ausführen**.

1. Bearbeiten Sie das folgende Skript, **{Ihren Kontonamen Github}**, **{Projektname}**und **{Ordner Ihres Projekts}** mit geeignete Werte ersetzen, und fügen Sie das bearbeitete Skript im Textbereich, der angezeigt wird.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klicken Sie auf **Speichern**.

1. Finden Sie auf dem Dashboard Hudson den Auftrag soeben erstellte und klicken Sie auf das Symbol **Terminplan einer erstellen** .

Hudson anschließend erstellen einen untergeordnete Knoten mithilfe der im vorherigen Abschnitt erstellten Vorlage und führen Sie das Skript, die, das Sie im Schritt erstellen für diese Aufgabe angegeben haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/
[Abonnementprofil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png


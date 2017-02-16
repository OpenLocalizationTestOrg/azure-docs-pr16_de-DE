<properties
    pageTitle="Verwenden die Azure CLI mit Azure-Speicher | Microsoft Azure"
    description="Erfahren Sie, wie die Azure Line Schnittstelle (Azure CLI) verwenden mit Azure-Speicher erstellen und Verwalten von Speicherkonten und Arbeiten mit Azure Blobs und Dateien. Die Azure ist ein Plattform-tool "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Verwenden die Azure CLI mit Azure-Speicher

## <a name="overview"></a>(Übersicht)

Die CLI Azure bietet eine Reihe von open Source Plattform-Befehle für die Arbeit mit Azure-Plattform. Es bietet im Wesentlichen die gleiche Funktionalität in [Azure-Portal](https://portal.azure.com) als auch Rich-Daten-Access-Funktionalität gefunden.

In diesem Handbuch wird so [Azure Line Interface (CLI Azure)](../xplat-cli-install.md) verwenden Sie zum Ausführen einer Vielzahl von Entwicklung und Verwaltungsaufgaben mit Azure-Speicher vorgestellt. Es empfiehlt sich, dass das Herunterladen und installieren oder aktualisieren Sie auf die neueste Azure CLI vor der Verwendung von diesem Leitfaden.

Mit diesem Leitfaden wird davon ausgegangen, dass Sie die grundlegenden Konzepte Azure-Speicher verstehen. Das Handbuch enthält eine Reihe von Skripts, um die Verwendung der Azure CLI mit Azure-Speicher zu veranschaulichen. Achten Sie darauf, dass Sie die Skriptvariablen auf Grundlage der Konfiguration vor dem Ausführen jedes Skript zu aktualisieren.

> [AZURE.NOTE] Das Handbuch enthält die Azure CLI-Befehl und Skript Beispiele für klassische Speicherkonten. Finden Sie unter [Verwendung der CLI Azure für Mac, Linux, und Windows Azure Ressource Verwaltung](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) für Azure CLI-Befehle für Ressourcenmanager Speicherkonten.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Erste Schritte mit Azure-Speicher und die CLI Azure in 5 Minuten

Mit diesem Leitfaden verwendet Ubuntu Beispiele, jedoch andere Plattformen OS auf ähnliche Weise ausführen.

**Neu bei Azure:** Erhalten eines Microsoft Azure-Abonnement und einem Microsoft-Konto mit diesem Abonnement verknüpft ist. Informationen zu Language Pack für Azure-Optionen finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Optionen erwerben](https://azure.microsoft.com/pricing/purchase-options/)und [Mitglied bietet](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network, und BizSpark und anderen Microsoft-Programmen).

Weitere Informationen zu Azure-Abonnements finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nach dem Erstellen eines Microsoft Azure-Abonnement und eines Kontos:**

1. Herunterladen und Installieren der Azure CLI dabei in [der Azure CLI installieren](../xplat-cli-install.md).
2. Sobald die Azure CLI installiert wurde, werden Sie mithilfe des Befehls Azure, aus der Benutzeroberfläche Line (Bash, Terminal, Befehlszeile) Azure CLI-Befehle zugreifen können. Typ `azure` Befehl und Sie sollten die folgende Ausgabe sehen.

    ![Ausgabe des Befehls Azure][Image1]

3. Geben Sie den Befehl-Schnittstelle `azure storage` auf alle Befehle der Azure-Speicher Liste und erhalten einem ersten Eindruck von der Funktionen der CLI Azure ermöglicht. Sie können Befehlsname mit **h -** Parameter eingeben (z. B. `azure storage share create -h`) Befehlssyntax angezeigt.
4. Nun können wir Sie ein einfaches Skript gewähren, das zeigt Azure CLI Basisbefehlen Zugriff auf Azure-Speicher. Das Skript fragt zunächst zwei Variablen für Ihr Speicherkonto und Schlüssel festlegen. Klicken Sie dann das Skript erstellen einen neuen Container in diesem Speicher Neukunde und Hochladen eine vorhandenen Bilddatei (Blob) zum Container. Nachdem das Skript alle Blobs im Container Listen, wird es die Bilddatei in das Zielverzeichnis herunterladen, die auf dem lokalen Computer vorhanden ist.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Öffnen Sie in Ihrem lokalen Computer Ihrem bevorzugten Texteditor (beispielsweise Vim) ein. Geben Sie das obige Skript in den Texteditor ein.

6. Jetzt, müssen Sie die Skriptvariablen auf Grundlage der Konfiguration Einstellungen zu aktualisieren.

    - **< Storage_account_name >** Verwenden Sie den Vornamen in das Skript, oder geben Sie einen neuen Namen für Ihr Speicherkonto. **Wichtige:** Der Name des Speicherkontos muss in Azure eindeutig sein. Es muss auch Kleinbuchstaben, sein!

    - **< Storage_account_key >** Zugriffstaste Ihres Kontos Speicher.

    - **< Container_name >** Verwenden Sie den Vornamen in das Skript, oder geben Sie einen neuen Namen für den Container.

    - **< Image_to_upload >** Geben Sie einen Pfad zu einem Bild auf dem lokalen Computer, z. B.: "~ / images/HelloWorld.png".

    - **< Destination_folder >** Geben Sie einen Pfad zu einem lokalen Verzeichnis aus Azure-Speicher, beispielsweise als heruntergeladene Dateien gespeichert: "~/downloadImages".

7. Nachdem Sie die erforderlichen Variablen in Vim aktualisiert haben, drücken Sie Tastenkombinationen "Esc,:, Wq!" um das Skript zu speichern.

8. Um dieses Skript ausführen, geben Sie den Namen der Skriptdatei einfach in der Bash-Verwaltungskonsole. Nach der dieses Skript ausgeführt wird, müssen Sie einen lokalen Zielordner, der die heruntergeladene Datei enthält. Das folgende Bildschirmabbild zeigt eine Beispiel für die Ausgabe an:

Nachdem das Skript ausgeführt wird, sollten Sie einen lokalen Zielordner verfügen, der die heruntergeladene Datei enthält.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Verwalten von Speicherkonten mit Azure CLI

### <a name="connect-to-your-azure-subscription"></a>Herstellen einer Verbindung Ihres Abonnements Azure mit

Obwohl die meisten Befehle Speicher ohne ein Azure-Abonnement funktionieren, empfehlen wir, Sie über die Befehlszeile Azure eine Verbindung zu Ihrem Abonnement. Folgen Sie den Schritten in [Verbindung mit einer Azure-Abonnement über die Befehlszeile Azure herstellen](../xplat-cli-connect.md), die CLI Azure für die Arbeit mit Ihrem Abonnement um zu konfigurieren.

### <a name="create-a-new-storage-account"></a>Erstellen eines neuen Kontos mit Speicher

Wenn Azure-Speicher verwenden möchten, benötigen Sie ein Speicherkonto. Nachdem Sie Ihren Computer für die Verbindung zu Ihrem Abonnement konfiguriert haben, können Sie ein neues Azure-Speicher-Konto erstellen.

        azure storage account create <account_name>

Der Name Ihres Kontos Speicher muss zwischen 3 und 24 Zeichen lang sein und Zahlen und nur Kleinbuchstaben verwenden.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Festlegen einer Azure-Speicher Standardkonto in Umgebungsvariablen

Sie können mehrere Speicherkonten in Ihr Abonnement haben. Sie können wählen sie eine, und legen Sie es in den Umgebungsvariablen für alle Speicher Befehle in der gleichen Sitzung. So können Sie Azure CLI Speicher-Befehle ohne Angabe des Speicherkontos ausführen und explizit wichtiger.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Eine weitere Möglichkeit, eine Speicher Standardkonto festlegen ist Verbindungszeichenfolge verwenden. Als erstes Abrufen der Verbindungszeichenfolge von Befehl ein:

        azure storage account connectionstring show <account_name>

Klicken Sie dann kopieren Sie die Ausgabe Verbindungszeichenfolge, und legen Sie dafür Umgebungsvariable:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Erstellen und Verwalten von blobs

Azure Blob-Speicher ist ein Dienst zum Speichern von große Mengen von unstrukturierten Daten, wie z. B. Text oder binäre Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure BLOB-Speicherkonzepten vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) und [BLOB-Dienst Konzepte](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Erstellen eines Containers

Jeder Blob in Azure-Speicher muss sich in einem Container. Erstellen Sie eine private Container mit der `azure storage container create` Befehl:

        azure storage container create mycontainer

> [AZURE.NOTE] Es gibt drei anonymen Zugriffsebenen gelesen: **Deaktivieren**, **Blob**und **Container**. Um anonymer Zugriff auf Blobs zu verhindern, legen Sie den Berechtigungsparameter zu **Deaktivieren**. Standardmäßig der neue Container ist privat und nur über den Kontobesitzer zugegriffen werden kann. Um anonyme öffentliche Lesezugriff BLOB-Ressourcen, aber nicht Container Metadaten oder die Liste der Blobs im Container, legen Sie den Berechtigung-Parameter auf **Blob**zu ermöglichen. Um vollständigen öffentlichen Lesezugriff auf BLOB-Ressourcen, Container Metadaten und die Liste der Blobs im Container zu ermöglichen, legen Sie den Parameter über die Berechtigung zum **Container**aus. Weitere Informationen finden Sie unter [anonyme Lesezugriff auf Container und Blobs verwalten](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Azure Blob-Speicher unterstützt blockieren Blobs und Seitenblobs. Weitere Informationen finden Sie unter [Grundlegendes zu blockieren Blobs, Anfügen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Um Blobs in einem Container hochladen möchten, können Sie die `azure storage blob upload`. Standardmäßig uploads dieser Befehl die lokalen Dateien in ein Blob blockieren. Um den Typ für das Blob angeben möchten, können Sie die `--blobtype` Parameter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Herunterladen von Blobs aus einem container

Im folgenden Beispiel wird veranschaulicht, wie Blobs aus einem Container herunterladen.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopieren von blobs

Sie können asynchrone Blobs in oder mehreren Speicherkonten und die Regionen kopieren.

Im folgenden Beispiel wird veranschaulicht, wie Blobs von einem Speicherkonto in ein anderes kopieren. In diesem Beispiel erstellen wir ein Containers Blobs, wo öffentlich sind, anonym zugegriffen werden.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

In diesem Beispiel führt eine asynchrone Kopie an. Sie können den Status der einzelnen Kopiervorgang überwachen, durch Ausführen der `azure storage blob copy show` Vorgang.

Notiz, die die Quelle-URL für den Kopiervorgang bereitgestellt muss entweder öffentlich zugängliche, oder ein Token SAS (gemeinsamen Zugriff Signatur) enthalten.

### <a name="delete-a-blob"></a>Löschen eines BLOBs

Verwenden Sie zum Löschen eines BLOBs den folgenden Befehl:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Erstellen und Verwalten von Dateifreigaben

Azure Dateispeicher bietet freigegebene Speicher für Applikationen mit dem standardmäßigen SMB-Protokoll. Microsoft Azure-virtuellen Computern und Cloud Services sowie lokalen Applications, können die Dateidaten über bereitgestellten Freigaben freigeben. Sie können Dateifreigaben und Dateidaten über die Azure CLI verwalten. Weitere Informationen für den Azure Dateispeicher finden Sie unter [Erste Schritte mit Azure Dateispeicher unter Windows](storage-dotnet-how-to-use-files.md) oder [So Azure Dateispeicher mit Linux verwenden](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

Eine Azure Dateifreigabe ist eine Dateifreigabe SMB in Azure. Alle Verzeichnisse und Dateien müssen in einer Dateifreigabe erstellt werden. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien, die im Rahmen der Kapazität des Speicherkontos speichern. Im folgende Beispiel wird eine Dateifreigabe namens " **EigeneFreigabe**" erstellt.

        azure storage share create myshare

### <a name="create-a-directory"></a>Erstellen Sie ein Verzeichnis

Ein Verzeichnis stellt eine optionale hierarchische Struktur für eine Dateifreigabe Azure. Im folgende Beispiel wird ein Verzeichnis mit dem Namen **MyDir** in die Dateifreigabe erstellt.

        azure storage directory create myshare myDir

Hinweis Dieses Verzeichnispfad kann mehrere Ebenen, *z.*B. enthalten **einer / b**. Jedoch müssen Sie sicherstellen, dass alle übergeordneten Verzeichnisse vorhanden ist. Zum Beispiel für den Pfad **ein / b**, müssen Sie Verzeichnis **ein** zuerst erstellen und dann Verzeichnis **b**erstellen.

### <a name="upload-a-local-file-to-directory"></a>Hochladen einer lokalen Datei auf Verzeichnis

Im folgende Beispiel uploads eine Datei von **~/temp/samplefile.txt** auf **MyDir** Verzeichnis. Bearbeiten Sie den Dateipfad, sodass es auf eine gültige Datei auf dem lokalen Computer zeigt:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Beachten Sie, dass eine Datei in der Freigabe bis zu 1 TB groß sein kann.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Listet die Dateien im Stamm freigeben oder Verzeichnis

Sie können Dateien und Unterverzeichnisse in die Quadratwurzel einer freigeben oder ein Verzeichnis mit dem folgenden Befehl auflisten:

        azure storage file list myshare myDir

Beachten Sie, dass der Name des Verzeichnisses für den Eintrag Vorgang optional ist. Wenn nicht angegeben, wird der Inhalt des Stammverzeichnisses der Freigabe von der Befehl aufgeführt.

### <a name="copy-files"></a>Kopieren Sie die Dateien

Beginnend mit Azure CLI Version 0.9.8, können Sie eine Datei an eine andere Datei, eine Datei zu einer Blob oder ein Blob in eine Datei kopieren. Im folgenden führen wir für diese CLI-Befehlen Kopieren Operationen wie vor. Kopieren eine Datei in das neue Verzeichnis:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

So kopieren einen Blob in ein Dateiverzeichnis ein:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige verwandten Artikeln und Ressourcen mehr über Azure-Speicher.

- [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-Speicher REST-API-Referenz](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png

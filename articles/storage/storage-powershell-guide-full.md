<properties
    pageTitle="Verwenden von Azure PowerShell mit Azure-Speicher | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit der Azure-PowerShell-Cmdlets für Azure-Speicher erstellen und Verwalten von Speicherkonten; Arbeiten Sie mit Blobs, Tabellen, Warteschlangen und Dateien. Konfigurieren und Speicher Analytics Abfragen und gemeinsamen Zugriff Signaturen erstellen."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Verwenden von Azure PowerShell mit Azure-Speicher

## <a name="overview"></a>(Übersicht)

Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Es ist eine Befehlszeile Shell aufgabenbasierten und Skriptingtools Sprache durchführen möchten, besonders für Systemadministration. Mit PowerShell einfach steuern und die Verwaltung Ihrer Azure Dienste und Applications automatisieren. Beispielsweise können Sie die Cmdlets verwenden, die gleichen Aufgaben ausführen möchten, die Sie über das [Azure-Portal](https://portal.azure.com)durchführen können.

In diesem Handbuch wird so der [Azure-Speicher-Cmdlets](https://msdn.microsoft.com/library/azure/mt269418.aspx) verwenden, um eine Vielzahl von Entwicklung und Verwaltungsaufgaben mit Azure-Speicher ausführen vorgestellt.

Mit diesem Leitfaden wird davon ausgegangen, dass Sie die vorherige Erfahrung mit [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/) und [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx)haben. Das Handbuch enthält eine Reihe von Skripts, um die Verwendung der PowerShell mit Azure-Speicher zu veranschaulichen. Sie sollten die Skriptvariablen auf Grundlage der Konfiguration vor dem Ausführen jedes Skript aktualisieren.

Der erste Abschnitt in diesem Handbuch bietet einen schnellen Blick auf Azure-Speicher und PowerShell. Ausführliche Informationen und Anweisungen starten Sie von der [Voraussetzung für die Verwendung von Azure PowerShell mit Azure-Speicher](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Erste Schritte mit Azure-Speicher und PowerShell in 5 Minuten

In diesem Abschnitt werden Azure-Speicher über PowerShell in 5 Minuten zugreifen.

**Neu bei Azure:** Erhalten eines Microsoft Azure-Abonnement und einem Microsoft-Konto mit diesem Abonnement verknüpft ist. Informationen zu Language Pack für Azure-Optionen finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Optionen erwerben](https://azure.microsoft.com/pricing/purchase-options/)und [Mitglied bietet](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network, und BizSpark und anderen Microsoft-Programmen).

Weitere Informationen zu Azure-Abonnements finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nach dem Erstellen eines Microsoft Azure-Abonnement und eines Kontos:**

1.  Herunterladen Sie und installieren Sie der [PowerShell Azure](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start Windows PowerShell Integrated Scripting-Umgebung (ISE): Wechseln Sie zu im **Startmenü** In Ihrem lokalen Computer. Geben Sie **Verwaltung** , und klicken Sie auf, um Sie auszuführen. Klicken Sie im Fenster **Verwaltung** mit der rechten Maustaste in **Windows PowerShell ISE**, klicken Sie auf **als Administrator ausführen**.
3.  Klicken Sie auf **Datei**, klicken Sie in **Windows PowerShell ISE**, > **neu** , um eine neue Skriptdatei zu erstellen.
4.  Nun können wir Sie ein einfaches Skript gewähren, das PowerShell Basisbefehlen Zugriff auf Azure-Speicher zeigt. Das Skript wird zuerst bitten Sie Ihre Azure Anmeldeinformationen für Ihre Azure Hinzufügen der lokalen PowerShell-Umgebung zu berücksichtigen. Klicken Sie dann das Skript Festlegen der Azure-Abonnement und erstellen ein neues Speicherkonto in Azure. Als Nächstes wird das Skript erstellen einen neuen Container in diesem Speicher Neukunde und Hochladen eine vorhandenen Bilddatei (Blob) zum Container. Nachdem das Skript alle Blobs im Container aufgeführt wird, wird ein neues Zielverzeichnis in Ihrem lokalen Computer erstellen und Herunterladen die Bilddatei.
5.  Im folgenden Codeabschnitt, wählen Sie das Skript zwischen den Hinweisen **#beginnen** und **#end**. Drücken Sie STRG + C, um ihn in die Zwischenablage zu kopieren.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  Drücken Sie in **Windows PowerShell ISE**STRG + V, um das Skript zu kopieren. Klicken Sie auf **Datei** > **Speichern**. Geben Sie den Namen der Skriptdatei, wie etwa "Mystoragescript", klicken Sie im Dialogfeld **Speichern unter** . Klicken Sie auf **Speichern**.

6.  Jetzt, müssen Sie die Skriptvariablen auf Grundlage der Konfiguration Einstellungen zu aktualisieren. Sie müssen die Variable **$SubscriptionName** mit Ihrem eigenen Abonnement aktualisieren. Können Sie die anderen Variablen gemäß Angabe im Skript beibehalten oder aktualisieren diese wie gewünscht.

    - **$SubscriptionName:** Sie müssen diese Variable mit den Namen Ihres eigenen Abonnements aktualisieren. Führen Sie einen der folgenden drei Methoden, den Namen Ihres Abonnements gesucht werden soll:

        ein. Klicken Sie auf **Datei**, klicken Sie in **Windows PowerShell ISE**, > **neu** , um eine neue Skriptdatei zu erstellen. Kopieren Sie das folgende Skript in die neue Skriptdatei, und klicken Sie auf **Debuggen** > **Ausführen**. Das folgende Skript wird zuerst bitten Sie Ihre Azure Anmeldeinformationen hinzufügen Ihrer Azure-Konto in der lokalen PowerShell-Umgebung und zeigen Sie dann auf alle Abonnements, die mit der lokalen PowerShell-Sitzung verbunden sind. Notieren Sie sich den Namen des Abonnements, die Sie beim Durchführen dieses Lernprogramms verwenden möchten:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Klicken Sie zum Suchen und kopieren den Namen Ihres Abonnements im [Portal Azure](https://portal.azure.com)Hub im Menü auf der linken Seite auf **Abonnements**. Kopieren Sie den Namen des Abonnements, die Sie während der Ausführung der Skripts in diesem Handbuch verwenden möchten.

        ![Azure-Portal][Image2]

        c. Um auszuwählen, und kopieren den Namen Ihres Abonnements in der [Klassischen Azure-Portal](https://manage.windowsazure.com/), führen Sie einen Bildlauf nach unten, und klicken Sie auf **Einstellungen** , klicken Sie auf der linken Seite des Portals. Klicken Sie auf **Abonnements** , um eine Liste Ihrer Abonnements anzuzeigen. Kopieren Sie den Namen des Abonnements, die Sie bei der Vorführung der angegebenen Skripts in diesem Handbuch verwenden möchten.

        ![Azure klassischen-Portal][Image1]

    - **$StorageAccountName:** Verwenden Sie den Vornamen in das Skript, oder geben Sie einen neuen Namen für Ihr Speicherkonto. **Wichtige:** Der Name des Speicherkontos muss in Azure eindeutig sein. Es muss auch Kleinbuchstaben, sein!

    - **$Location:** Verwenden Sie den angegebenen "Westen USA" in das Skript oder wählen Sie aus anderen Speicherorten Azure wie ostasiatischen US, North Europa, usw.

    - **$ContainerName:** Verwenden Sie den Vornamen in das Skript, oder geben Sie einen neuen Namen für den Container.

    - **$ImageToUpload:** Geben Sie einen Pfad zu einem Bild auf dem lokalen Computer, z. B.: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Geben Sie einen Pfad zu einem lokalen Verzeichnis aus Azure-Speicher, beispielsweise als heruntergeladene Dateien gespeichert: "C:\DownloadImages".

7.  Nach der Aktualisierung der Skriptvariablen in der Datei "mystoragescript.ps1", klicken Sie auf **Datei** > **Speichern**. Klicken Sie dann auf **Debuggen** > **Ausführen** oder drücken Sie **F5** , um das Skript auszuführen.  

Nach der das Skript ausgeführt wird, müssen Sie einen lokalen Zielordner, der die heruntergeladene Datei enthält. Das folgende Bildschirmabbild zeigt eine Beispiel für die Ausgabe an:

![Herunterladen von Blobs][Image3]


> [AZURE.NOTE] Im Abschnitt "Erste Schritte mit Azure-Speicher und PowerShell 5 Minuten" bereitgestellten eine schnelle Einführung in zum Azure PowerShell mit Azure-Speicher verwenden möchten. Ausführliche Informationen und Anweisungen empfehlen wir Ihnen, lesen in den folgenden Abschnitten.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Voraussetzung für die Verwendung von Azure PowerShell mit Azure-Speicher
Sie benötigen eine Azure-Abonnement und -Konto, um die in diesem Handbuch angegebenen PowerShell-Cmdlets ausführen, wie zuvor beschrieben.

Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Informationen zum Installieren und Einrichten von Azure PowerShell finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md). Es empfiehlt sich, dass das Herunterladen und installieren oder Aktualisieren auf das neueste Azure PowerShell-Modul, bevor Sie dieses Handbuch verwenden.

Sie können die Cmdlets in standard Windows PowerShell-Konsole oder der Windows PowerShell Integrated Scripting-Umgebung (ISE) ausführen. Beispielsweise um **Windows PowerShell ISE**zu öffnen, wechseln Sie zu dem Startmenü, geben Sie Verwaltung und klicken Sie auf, um Sie auszuführen. Klicken Sie im Fenster Verwaltung mit der rechten Maustaste in Windows PowerShell ISE, klicken Sie auf als Administrator ausführen.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Zum Verwalten von Speicherkonten in Azure

### <a name="how-to-set-a-default-azure-subscription"></a>So legen Sie einen Standardwert Azure-Abonnement
Zum Verwalten von mithilfe der PowerShell Azure Azure-Speicher müssen Sie Ihre Clientumgebung mit Azure über Azure Active Directory-Authentifizierung oder Zertifikaten basierende Authentifizierung authentifizieren. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) -Lernprogramm. Mit diesem Leitfaden wird die Azure-Active Directory-Authentifizierung verwendet.

1.  Windows PowerShell ISE, geben Sie den folgenden Befehl zum Hinzufügen Ihrer Azure lokale PowerShell-Umgebung zu berücksichtigen:

    `Add-AzureAccount`

2.  Klicken Sie im Fenster "Sign in auf Microsoft Azure" Geben Sie die e-Mail-Adresse und Ihr Kennwort ein, der mit Ihrem Konto verbunden ist. Azure authentifiziert speichert Anmeldeinformationen für die, und klicken Sie dann das Fenster wird geschlossen.

3.  Führen Sie dann den folgenden Befehl zum Anzeigen der Azure-Konten in Ihrem lokalen PowerShell-Umgebung, und stellen Sie sicher, dass Ihr Konto aufgeführt wird:

    `Get-AzureAccount`

4.  Klicken Sie dann führen Sie das folgende Cmdlet aus, um alle Abonnements anzuzeigen, die mit der lokalen PowerShell-Sitzung verbunden sind, und stellen Sie sicher, dass Ihr Abonnement aufgeführt wird:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Führen Sie zum Festlegen einer standardmäßigen Azure-Abonnement das Select-AzureSubscription-Cmdlet aus:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Überprüfen Sie den Namen des Abonnements Standard aus, indem Sie das Cmdlet "Get-AzureSubscription" ausführen:

    `Get-AzureSubscription -Default`

7.  Um alle verfügbaren PowerShell-Cmdlets für Azure-Speicher angezeigt wird, führen Sie Folgendes aus:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>So erstellen Sie ein neues Konto mit Azure-Speicher
Wenn Azure-Speicher verwenden möchten, benötigen Sie ein Speicherkonto. Nachdem Sie Ihren Computer für die Verbindung zu Ihrem Abonnement konfiguriert haben, können Sie ein neues Azure-Speicher-Konto erstellen.

1.  Führen Sie das Cmdlet "Get-AzureLocation", um alle verfügbaren Datacenter Speicherorte finden:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Führen Sie dann das neu AzureStorageAccount Cmdlet zum Erstellen eines neuen Kontos mit Speicher ein. Im folgende Beispiel wird ein neues Speicherkonto im Datencenter "Westen US" erstellt.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Der Name Ihres Kontos Speicher muss innerhalb Azure eindeutig sein und muss Kleinbuchstaben. Benennungskonventionen und Einschränkungen finden Sie unter [Informationen zum Azure Speicherkonten](storage-create-storage-account.md) und [Naming und verweisen auf Container, Blobs, und Metadaten](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Wie Sie eine Azure-Speicher Standardkonto festlegen
Sie können mehrere Speicherkonten in Ihr Abonnement haben. Sie können wählen sie eine und es als Standardkonto Speicher für alle Speicher-Befehle in der gleichen PowerShell-Sitzung festlegen. So können Sie die Azure PowerShell Speicher Befehle ausführen, ohne den Speicherkontext explizit anzugeben.

1.  Wenn kein Speicherplatz Standardkonto für Ihr Abonnement festlegen möchten, können Sie das Cmdlet "Set-AzureSubscription" ausführen.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Führen Sie dann das Cmdlet "Get-AzureSubscription", um zu überprüfen, dass das Speicherkonto Ihr Abonnement Standardkonto zugeordnet ist. Dieser Befehl gibt die Abonnementeigenschaften auf das aktuelle Abonnement einschließlich deren aktuellen Speicherkonto an.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Wie Sie die Liste alle Azure-Speicher-Konten in einem Abonnement
Jede Azure-Abonnement kann bis zu 100 Speicherkonten verfügen. Die aktuellste Informationen Grenzwerte finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).

Führen Sie das folgende Cmdlet aus, um herauszufinden, den Namen und den Status der Speicherkonten in das aktuelle Abonnement:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>So erstellen Sie einen Kontext Azure-Speicher
Kontext Azure-Speicher ist ein Objekt in PowerShell kapseln die Anmeldeinformationen Speicher. Verwenden einen Speicherkontext aus, während alle nachfolgenden Cmdlet ausführen, Sie Ihre Anforderung eine Authentifizierung ohne Angabe des Speicherkontos und die Zugriffstaste explizit können. Sie können einen Speicherkontext auf viele Arten, z. B. mit Speicher Konto Name und Access-Taste, gemeinsamen Zugriff Signatur (SAS) Token, Verbindungszeichenfolge, erstellen oder anonymer. Weitere Informationen finden Sie unter [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Verwenden Sie eine der folgenden drei Methoden zum Erstellen eines Speicher Kontexts aus:

- Führen Sie das Cmdlet " [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) " um herauszufinden, der primären Speicher Zugriffstaste für Ihr Konto Azure-Speicher. Als Nächstes rufen Sie das Cmdlet [Neu-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) aus, um einen Speicherkontext zu erstellen:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Generieren Sie einer freigegebenen Access Signaturtoken für einen Container Azure-Speicher, und verwenden sie einen Speicherkontext erstellt:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Weitere Informationen finden Sie unter [Neu-AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) und [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- In einigen Fällen möchten Sie möglicherweise die Endpunkte geben Sie beim Erstellen eines neuen Speicher Kontexts. Dies kann erforderlich sein, wenn Sie einen benutzerdefinierten Domänennamen für Ihr Speicherkonto, mit dem Blob-Dienst registriert haben oder eine freigegebenen Access-Signatur für den Zugriff auf Speicherressourcen verwenden möchten. Legen Sie die Dienstendpunkte in einer Verbindungszeichenfolge und ihre Verwendung zum Erstellen eines neuen Speicher Kontexts, wie unten dargestellt:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Weitere Informationen zum Konfigurieren einer Verbindungszeichenfolge von Speicher finden Sie unter [Konfigurieren von Verbindungszeichenfolgen](storage-configure-connection-string.md).

Jetzt, da Sie richten Sie Ihren Computer und zum Verwalten von Abonnements und Speicherkonten mithilfe der PowerShell Azure gelernt, wechseln Sie zum nächsten Abschnitt Informationen zum Verwalten von Azure Blobs und BLOB-Momentaufnahmen konvertiert.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Zum Abrufen und erneut generieren Azure-Speicher Tasten

Ein Konto Azure-Speicher im Lieferumfang von zwei Konto-Taste. Das folgende Cmdlet können Sie Ihre Schlüssel abzurufen.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Verwenden Sie das folgende Cmdlet aus, um einen bestimmten Schlüssel abzurufen. Gültige Werte sind primären und sekundären.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Wenn Sie Ihre Schlüssel neu erstellen möchten, verwenden Sie das folgende Cmdlet aus. Gültige Werte für KeyType - sind "Primary" und "Sekundäre"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Zum Verwalten von Azure blobs
Azure Blob-Speicher ist ein Dienst zum Speichern von große Mengen von unstrukturierten Daten, wie z. B. Text oder binäre Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure Blob-Speicherdienst Konzepten vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) und [BLOB-Dienst Konzepte](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>So erstellen Sie einen container
Jeder Blob in Azure-Speicher muss sich in einem Container. Sie können einen privaten Container mithilfe des Cmdlets New-AzureStorageContainer erstellen:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Es gibt drei anonymen Zugriffsebenen gelesen: **Deaktivieren**, **Blob**und **Container**. Um anonymer Zugriff auf Blobs zu verhindern, legen Sie den Berechtigungsparameter zu **Deaktivieren**. Standardmäßig der neue Container ist privat und nur über den Kontobesitzer zugegriffen werden kann. Um anonyme öffentliche Lesezugriff BLOB-Ressourcen, aber nicht Container Metadaten oder die Liste der Blobs im Container, legen Sie den Berechtigung-Parameter auf **Blob**zu ermöglichen. Um vollständigen öffentlichen Lesezugriff auf BLOB-Ressourcen, Container Metadaten und die Liste der Blobs im Container zu ermöglichen, legen Sie den Parameter über die Berechtigung zum **Container**aus. Weitere Informationen finden Sie unter [anonyme Lesezugriff auf Container und Blobs verwalten](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Wie Sie einen Blob in einen Container hochladen.
Azure Blob-Speicher unterstützt blockieren Blobs und Seitenblobs. Weitere Informationen finden Sie unter [Grundlegendes zu blockieren Blobs, Anfügen BLobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Um zu einem Container Blobs in hochladen, können Sie das Cmdlet " [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) " verwenden. Standardmäßig uploads dieser Befehl die lokalen Dateien in ein Blob blockieren. Um den Typ für das Blob angeben möchten, können Sie den BlobType - Parameter verwenden.

Im folgende Beispiel wird das Cmdlet " [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) " um alle Dateien in dem angegebenen Ordner zu gelangen ausgeführt, und übergibt diese an das nächste Cmdlet mit dem Operator Verkaufspipeline. Das Cmdlet " [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) " lädt die lokalen Dateien an Ihre Container:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Zum Herunterladen von Blobs aus einem container
Im folgenden Beispiel wird veranschaulicht, wie Blobs aus einem Container herunterladen. Im Beispiel stellt zuerst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts der Speicher-Konto, wozu auch den Kontonamen für den Speicher und die primäre Zugriffstaste her. Klicken Sie dann im Beispiel einer Blob ruft Verweis ab, verwenden das Cmdlet " [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) ". Als Nächstes verwendet das Beispiel das Cmdlet " [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) " Blobs in den Zielordner lokale herunterladen.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>So Blobs von einem Speichercontainer in eine andere zu kopieren.
Sie können asynchrone Blobs über Speicherkonten und Regionen kopieren. Im folgenden Beispiel wird veranschaulicht, wie Blobs von einem Speichercontainer in ein anderes in zwei verschiedenen Speicherkonten zu kopieren. Im Beispiel zuerst wird Variablen für Quell- und Zielfelder Speicher-Konten, und klicken Sie dann einen Speicherkontext für jedes Konto erstellt. Im nächsten Schritt kopiert das Beispiel Blobs aus der Quellcontainer zum Zielcontainer mithilfe des [Start-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) -Cmdlets. Im Beispiel wird davon ausgegangen, dass die Quell- und Zielfelder Speicher-Konten und Container bereits vorhanden sind.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Beachten Sie, dass in diesem Beispiel wird eine asynchrone Kopie ausführt. Sie können den Status der einzelnen überwachen, indem Sie das Cmdlet " [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) " ausführen.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>So kopieren Sie Blobs von einem zweiten Standort
Sie können aus der zweiten Standort eines Kontos RAS-GRS-fähige Blobs kopieren.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>So löschen Sie einen blob
Zum Löschen eines BLOBs zuerst einen Verweis Blob und rufen Sie dann das Cmdlet entfernen-AzureStorageBlob daran. Im folgende Beispiel werden alle Blobs in einem bestimmten Container gelöscht. Im Beispiel zuerst Variablen für ein Speicherkonto festgelegt, und dann erstellt einen Speicherkontext. Als Nächstes im Beispiel ruft einen Blob-Verweis mithilfe des Cmdlets [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) ab und wird das Cmdlet [Entfernen-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) zum Entfernen von Blobs aus einem Container in Azure-Speicher ausgeführt.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Zum Verwalten von Momentaufnahmen Azure blob
Azure können Sie eine Momentaufnahme der ein Blob erstellen. Eine Momentaufnahme ist eine schreibgeschützte Version eines Blob, der zu einem bestimmten Zeitpunkt durchgeführt wird. Nachdem eine Momentaufnahme erstellt wurde, können sie lesen, kopiert, oder gelöscht, jedoch nicht geändert werden. Momentaufnahmen bieten eine Möglichkeit, ein Blob sichern, wie er bei einem Zeitpunkt angezeigt wird. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>So erstellen Sie einen Blob snapshot
Zum Erstellen einer eines Blob Snapshot zuerst einen Verweis Blob und rufen Sie dann die `ICloudBlob.CreateSnapshot` Methode daran. Im folgenden Beispiel wird zuerst Variablen für ein Speicherkonto festgelegt, und dann erstellt einen Speicherkontext. Als Nächstes im Beispiel ruft einen Blob-Verweis verwenden das Cmdlet " [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) " ab und führt die [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -Methode, um eine Momentaufnahme erstellen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Wie ein Blob Momentaufnahmen aufgelistet.
Sie können beliebig viele Momentaufnahmen beliebig für ein Blob erstellen. Sie können die Momentaufnahmen der Blob zum Nachverfolgen Ihrer aktuellen Momentaufnahmen zugeordnet auflisten. Im folgenden Beispiel wird einen vordefinierten Blob verwendet und das Cmdlet " [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) ", um die Liste der Momentaufnahmen der betreffende Blob-Anrufe.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>So kopieren Sie eine Momentaufnahme der ein blob
Sie können eine Momentaufnahme der Blob Snapshot wiederherstellen kopieren. Ausführliche Informationen und Einschränkungen finden Sie unter [Erstellen einer Momentaufnahme eines Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Im folgenden Beispiel wird zuerst Variablen für ein Speicherkonto festgelegt, und dann erstellt einen Speicherkontext. Als Nächstes definiert das Beispiel die Container und Blob Namensvariablen. Im Beispiel ruft einen Blob-Verweis verwenden das Cmdlet " [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) " ab und führt die [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -Methode, um eine Momentaufnahme zu erstellen. Dann wird das Beispiel mit dem [Start-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) -Cmdlet zum Kopieren des eines Blob mithilfe des ICloudBlob-Objekts für die Quelle Blob Snapshots ausgeführt. Achten Sie darauf, zum Aktualisieren der Variablen basierend auf Ihrer Konfiguration, bevor Sie das Beispiel ausführen. Beachten Sie, dass im folgende Beispiel wird davon ausgegangen, dass die Quelle und Zielcontainer, und die Quelle Blob bereits vorhanden sind.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Jetzt, da Sie zum Verwalten von Azure Blobs und Momentaufnahmen mit PowerShell Azure blob gelernt haben, wechseln Sie zum nächsten Abschnitt erfahren, wie Tabellen, Warteschlangen und Dateien zu verwalten.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Zum Verwalten von Azure Tabellen und Tabelle Einheiten
Azure Tabelle Speicherdienst ist ein Datenspeicher NoSQL, die Sie zum Speichern und große Datenmengen strukturierten, nicht relationalen Abfrage verwenden können. Die wichtigsten Komponenten des Diensts sind Tabellen, Elemente und Eigenschaften. Eine Tabelle ist eine Auflistung von Elementen. Eine Entität ist eine Reihe von Eigenschaften. Jede Entität kann bis zu 252 Eigenschaften verfügen, die alle Name / Wert-Paare sind. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure Tabelle Speicherdienst Konzepten vertraut sind. Ausführliche Informationen finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx) und [Erste Schritte mit Azure Tabellenspeicher mit .NET](storage-dotnet-how-to-use-tables.md).

In den folgenden Abschnitten erfahren Sie zum Verwalten von Azure Tabelle Speicherdienst Azure PowerShell verwenden. Die Szenarios dieser gehören **Erstellen**, **Löschen**und **Abrufen von** **Tabellen**, als auch **Hinzufügen**, **Abfragen**und **Tabelle Elemente löschen**.

### <a name="how-to-create-a-table"></a>So erstellen Sie eine Tabelle
Jeder Tabelle muss sich in einem Konto Azure-Speicher befinden. Im folgenden Beispiel wird veranschaulicht, wie eine Tabelle in Azure-Speicher zu erstellen. Im Beispiel stellt zuerst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts der Speicher-Konto, wozu auch den Kontonamen für den Speicher und die Zugriffstaste her. Als Nächstes wird das Cmdlet [Neu-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) zum Erstellen einer Tabelle in Azure-Speicher verwendet.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Zum Abrufen einer Tabelle
Sie können Abfragen und eine oder alle Tabellen in einem Speicherkonto abzurufen. Im folgenden Beispiel wird veranschaulicht, wie eine bestimmte Tabelle mithilfe des Cmdlets [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Wenn Sie das Cmdlet "Get-AzureStorageTable" ohne Parameter aufrufen, ruft alle Speichertabellen für ein Speicherkonto ab.

### <a name="how-to-delete-a-table"></a>So löschen Sie eine Tabelle
Sie können eine Tabelle von einem Speicherkonto mithilfe des Cmdlets [Entfernen-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) löschen.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Zum Verwalten der Tabelle Einheiten
Azure PowerShell bietet derzeit keine Cmdlets zum Verwalten von Tabelle Einheiten direkt. Zum Ausführen von Vorgängen in der Tabelle Einheiten können Sie die in der [Azure-Speicher-Client-Bibliothek für .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)angegebenen Klassen verwenden.

#### <a name="how-to-add-table-entities"></a>Zum Hinzufügen der Tabelle Einheiten
Wenn eine Entität zu einer Tabelle hinzufügen möchten, erstellen Sie zuerst ein Objekt, Ihre Entitätseigenschaften definiert. Eine Entität kann bis zu 255 Eigenschaften, einschließlich 3 Systemeigenschaften aufweisen: **PartitionKey**, **RowKey**und **Zeitstempel**. Sie sind für das Einfügen und aktualisieren die Werte **PartitionKey** und **RowKey**verantwortlich. Der Server verwaltet den Wert der **Timestamp**, der geändert werden kann. Die **PartitionKey** und **RowKey** identifizieren zusammen eindeutig jede Entität innerhalb einer Tabelle.

-   **PartitionKey**: bestimmt die Partition, die in die Entität gespeichert ist.
-   **RowKey**: die Entität innerhalb der Partition eindeutig identifiziert.

Sie können bis zu 252 benutzerdefinierte Eigenschaften für eine Entität definieren. Weitere Informationen finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Im folgenden Beispiel wird veranschaulicht, wie Personen zu einer Tabelle hinzuzufügen. Im Beispiel wird so rufen Sie die Tabelle der Mitarbeiter und fügen mehrere Personen hinein. Zunächst wird es sich um eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts der Speicher-Konto, die den Kontonamen für den Speicher und die Zugriffstaste enthält. Als Nächstes wird die angegebene Tabelle mithilfe des Cmdlets [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abgerufen. Wenn die Tabelle nicht vorhanden ist, wird das Cmdlet [Neu-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) zum Erstellen einer Tabelle in Azure-Speicher verwendet. Als Nächstes definiert das Beispiel eine benutzerdefinierte Funktion hinzufügen-Entität, um Personen zur Tabelle hinzufügen, indem Sie jede Entität des Partition und Zeile Key angeben. Die hinzufügen-Entität-Funktion ruft das [New-Object](http://technet.microsoft.com/library/hh849885.aspx) -Cmdlet für die [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) Klasse zum Erstellen eines Entitätsobjekts. Später wird im Beispiel die Methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) für dieses Entitätsobjekt zu einer Tabelle hinzuzufügen.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Vorgehensweise zum Abfragen von Tabelle Einheiten
Eine Tabelle abgefragt werden soll, verwenden Sie die [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) -Klasse. Im folgende Beispiel wird davon ausgegangen, dass Sie das Skript in der wie angegeben bereits ausgeführt haben Abschnitt dieses Handbuchs Personen hinzufügen. Im Beispiel stellt zuerst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts Speicher, wozu auch den Kontonamen für den Speicher und die Zugriffstaste her. Als Nächstes versucht, die zuvor erstellte "Mitarbeiter" Tabelle mithilfe des Cmdlets [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen. Aufrufen des Cmdlets [New-Objekt](http://technet.microsoft.com/library/hh849885.aspx) in der Microsoft.WindowsAzure.Storage.Table.TableQuery-Klasse erstellt ein neue Abfrageobjekt. Im Beispiel wird für die Personen, die eine Spalte "ID" aufweisen, deren Wert 1 gemäß Angabe in einer Zeichenfolge Filter ist. Ausführliche Informationen finden Sie unter [Abfragen Tabellen und Einheiten](http://msdn.microsoft.com/library/azure/dd894031.aspx). Wenn Sie diese Abfrage ausführen, liefert alle Personen, die die Filterkriterien entsprechen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Wie beim Löschen von Tabelle Einheiten
Sie können eine Entität mithilfe ihrer Schlüssel Partition und Zeile löschen. Im folgende Beispiel wird davon ausgegangen, dass Sie das Skript in der wie angegeben bereits ausgeführt haben Abschnitt dieses Handbuchs Personen hinzufügen. Im Beispiel stellt zuerst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts Speicher, wozu auch den Kontonamen für den Speicher und die Zugriffstaste her. Als Nächstes versucht, die zuvor erstellte "Mitarbeiter" Tabelle mithilfe des Cmdlets [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen. Wenn die Tabelle vorhanden ist, wird im Beispiel die [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) -Methode, um eine Entität basierend auf ihrer Partition und Zeile Schlüsselwerte abzurufen. Klicken Sie dann übergeben Sie die Entität an die Methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) zu löschen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>So verwalten Sie Azure Warteschlangen und Nachrichten in Warteschlange
Azure Warteschlange-Speicher ist ein Dienst für das Speichern einer großen Anzahl von Nachrichten, die über eine beliebige Stelle in der Welt über authentifizierten Anrufe mit HTTP oder HTTPS zugegriffen werden können. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure Warteschlange-Speicherdienst Konzepten vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure Warteschlange-Speicher mit .NET](storage-dotnet-how-to-use-queues.md).

In diesem Abschnitt wird gezeigt, wie Azure Warteschlange-Speicherdienst mithilfe der PowerShell Azure verwaltet werden. Die Szenarios dieser einschließen **Einfügen** und **Löschen von** Warteschlangennachrichten als auch **Erstellen**, **Löschen**und **Abrufen einer Warteschlange**.

### <a name="how-to-create-a-queue"></a>So erstellen Sie eine Warteschlange
Im folgenden Beispiel wird zunächst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts der Speicher-Konto, die den Kontonamen für den Speicher und die Zugriffstaste enthält. Als Nächstes ruft es [Neu AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) Cmdlets zum Erstellen einer Warteschlange mit dem Namen 'Queuename' aus.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Informationen zu Benennungskonventionen für Azure Warteschlange-Dienst finden Sie unter [Warteschlangen benennen und Metadaten](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Wie Sie eine Warteschlange abrufen
Sie können Abfragen und Abrufen einer bestimmten Warteschlange oder eine Liste aller Warteschlangen in einem Speicherkonto. Im folgenden Beispiel wird veranschaulicht, wie eine angegebene Warteschlange mithilfe des Cmdlets [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) abrufen.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Wenn Sie das Cmdlet " [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) " ohne Parameter aufrufen, ruft sie eine Liste aller Warteschlangen ab.

### <a name="how-to-delete-a-queue"></a>So löschen Sie eine Warteschlange
Rufen Sie das Cmdlet entfernen-AzureStorageQueue zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten. Im folgenden Beispiel wird gezeigt, wie eine angegebene Warteschlange mithilfe des Cmdlets entfernen-AzureStorageQueue löschen.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>So fügen Sie einer Nachricht in einer Warteschlange ein
Zum Einfügen einer Nachricht in eine vorhandene Warteschlange erstellen Sie zuerst eine neue Instanz der Klasse [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Rufen Sie als Nächstes die [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -Methode aus. Eine CloudQueueMessage kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Im folgenden Beispiel wird veranschaulicht, wie eine Warteschlange Nachricht hinzufügen. Im Beispiel stellt zuerst eine Verbindung mit Azure-Speicher unter Verwendung des Kontexts der Speicher-Konto, wozu auch den Kontonamen für den Speicher und die Zugriffstaste her. Danach ruft sie die angegebene Warteschlange, indem Sie das Cmdlet " [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) " ab. Wenn die Warteschlange vorhanden ist, wird das [New-Object](http://technet.microsoft.com/library/hh849885.aspx) -Cmdlet zum Erstellen einer Instanz der Klasse [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) verwendet. Später wird im Beispiel die [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -Methode für dieses Nachrichtenobjekt es zu einer Warteschlange hinzufügen. So sieht Code Ruft eine Warteschlange und fügt die Meldung 'MessageInfo' aus:

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>So heben die Warteschlange am nächsten Nachricht
Code Warteschlange eine Nachricht von einer Warteschlange in zwei Schritten. Wenn Sie die Methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **GetMessage** zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch die Methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) aufrufen. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Zum Verwalten von Azure-Dateifreigaben und Dateien
Azure Dateispeicher bietet freigegebene Speicher für Applikationen mit dem standardmäßigen SMB-Protokoll. Microsoft Azure-virtuellen Computern und Cloud Services können Daten über die Anwendungskomponenten über bereitgestellten Freigaben freizugeben und zu lokalen Applikationen Dateidaten in einer Freigabe über die Datei-Speicher-API oder Azure PowerShell zugreifen können.

Weitere Informationen für den Azure Dateispeicher finden Sie unter [Erste Schritte mit auf Windows Azure-Datei-Speicher](storage-dotnet-how-to-use-files.md) und die [Datei Dienst REST-API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>So legen Sie und die Abfrage Speicher analytics
[Azure-Speicher Analytics](storage-analytics.md) können zum Sammeln von Kennzahlen für Ihren Azure-Speicher-Konten und Log Daten zu Anfragen bei Ihrem Speicherkonto gesendet. Speicher Kennzahlen können Sie die Integrität des Speicher-Konto und Speicher Protokollierung zum Identifizieren und beheben Probleme mit Ihrem Speicherkonto überwachen.
Standardmäßig ist die Speicher Kennzahlen für Ihre Dienstleistungen Speicher nicht aktiviert. Sie können die Überwachung der Azure-Portal oder Windows PowerShell verwenden oder programmgesteuert mithilfe der Speicher-Client-Bibliothek aktivieren. Speicher Protokollierung geschieht serverseitigen und ermöglicht es Ihnen, können Sie Details für erfolgreiche und fehlgeschlagene Besprechungsanfragen in Ihrem Speicherkonto erfassen. Diese Protokolle können Sie Details zu lesen, schreiben und Löschvorgängen anhand von Tabellen, Warteschlangen, und Blobs sowie die Gründe für fehlgeschlagene Anfragen angezeigt.

Informationen zum Aktivieren und Anzeigen von Speicher Kennzahlen Daten mithilfe der PowerShell, finden Sie unter [Speicher Kennzahlen mithilfe der PowerShell aktivieren](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Informationen zum Aktivieren und Speicher Protokollierung Daten mithilfe der PowerShell abrufen, finden Sie unter [Speicher Protokollierung mithilfe der PowerShell aktivieren](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) und [Ihre Speicher Protokollierung Log-Daten zu suchen](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Ausführliche Informationen zur Verwendung von Speicher Kennzahlen und Speicher Protokollierung von Speicher-Problemen finden Sie unter [Überwachung, diagnostizieren, und Problembehandlung bei Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Zum Verwalten von freigegebenen Access Signatur (SAS) und Zugriffsrichtlinie gespeichert
Freigegebene Access Signaturen sind ein wichtiger Bestandteil des Sicherheitsmodells für jede andere Anwendung Azure-Speicher. Sie sind nützlich für die Bereitstellung von eingeschränkter Berechtigungen bei Ihrem Speicherkonto für Clients, die keine der kontoschlüssel. Standardmäßig kann nur der Besitzer des Speicherkontos Blobs, Tabellen und innerhalb dieses Konto zugreifen. Wenn der Dienst oder einer Anwendung diese Ressourcen an andere Clients zur Verfügung stellen werden kann, ohne die Freigabe Ihrer Access-Taste muss, haben Sie drei Optionen aus:

- Festlegen von Berechtigungen eines Containers, um zuzulassen, dass anonyme Lesezugriff auf den Container und deren Blobs. Dies ist für Tabellen oder Warteschlangen nicht zulässig.
- Verwenden einer freigegebenen Access Signatur, dass gewährt eingeschränkte Zugriffsrechte zum Container, Blobs, Queues und Tabellen für einen bestimmten Zeitraum aus.
- Verwenden Sie eine gespeicherte Zugriffsrichtlinie, um eine zusätzliche Sicherheitsstufe Kontrolle über die freigegebenen Access Signaturen für einen Container oder deren Blobs, für eine Warteschlange oder für eine Tabelle zu erhalten. Die gespeicherte Zugriffsrichtlinie können Sie die Startzeit, Ablauf Uhrzeit oder Berechtigungen für eine Signatur zu ändern, oder um ihn dahinter widerrufen ausgestellt wurde.

Eine freigegebene Access-Signatur in einer von zwei Formen sind möglich:

- **Ad-hoc-SAS**: beim Erstellen einer ad-hoc-SAS, die Startzeit, Ablaufzeit und Berechtigungen für die SAS werden auf dem SAS-URI angegeben. Dieses Typs SA-Container Blob auf eine Tabelle erstellt werden kann, oder Warteschlange und es ist nicht revokable.
- **SAS mit gespeicherten Zugriffsrichtlinie**: eine gespeicherte Access-Richtlinie definiert ist, klicken Sie auf eine Ressource Container eines Containers Blob, Tabelle oder Warteschlange - und sie zum Verwalten von Einschränkungen für eine oder mehrere gemeinsamen Zugriff Signaturen verwenden können. Wenn Sie eine gespeicherte Zugriffsrichtlinie einem SAS zuordnen, erbt die SAS Einschränkungen - Startzeit, Ablaufzeit und Berechtigungen – für die gespeicherte Zugriffsrichtlinie definiert ist. Dieser Typ SA-ist revokable.

Weitere Informationen finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) und [anonyme Lesezugriff auf Container und Blobs verwalten](storage-manage-access-to-resources.md).

In den nächsten Abschnitten erfahren Sie, wie Sie einer freigegebenen Access Signatur token und gespeicherten Zugriffsrichtlinie für Azure Tabellen erstellen. Azure PowerShell bereit Container, Blobs und Warteschlangen sowie ähnliche Cmdlets. Führen Sie die Skripts in diesem Abschnitt Herunterladen der [Azure PowerShell Version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) oder höher.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>So erstellen Sie ein Policy-basiertes freigegeben Access Signatur token
Verwenden Sie das Cmdlet "New-AzureStorageTableStoredAccessPolicy" zum Erstellen einer neuen gespeicherten Zugriffsrichtlinie. Rufen Sie dann das Cmdlet [AzureStorageTableSASToken neu](http://msdn.microsoft.com/library/azure/dn806400.aspx) , um ein neues Token der freigegebenen Access Policy-basierte Signatur für eine Tabelle Azure-Speicher zu erstellen.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>So erstellen Sie eine ad-hoc-(nicht revokable) freigegeben Access Signatur token
Verwenden Sie das Cmdlet " [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) ", um ein neues ad-hoc-(nicht revokable) freigegeben Access Signatur Token für eine Tabelle Azure-Speicher zu erstellen:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>So erstellen Sie eine gespeicherte Zugriffsrichtlinie
Verwenden Sie das Cmdlet "New-AzureStorageTableStoredAccessPolicy" zum Erstellen einer neuen gespeicherten Zugriffsrichtlinie für eine Tabelle Azure-Speicher ein:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Zum Aktualisieren einer gespeicherten Zugriffsrichtlinie
Verwenden Sie das Cmdlet "Set-AzureStorageTableStoredAccessPolicy" Aktualisieren eine vorhandenen gespeicherten Zugriffsrichtlinie für eine Tabelle Azure-Speicher ein:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>So löschen Sie eine gespeicherte Access-Richtlinie
Verwenden Sie das Cmdlet entfernen-AzureStorageTableStoredAccessPolicy zum Löschen einer gespeicherten Zugriffsrichtlinie auf einer Tabelle Azure-Speicher:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Verwendung von Azure-Speicher für US-Regierung und Azure China
Eine Azure-Umgebung ist eine unabhängige Bereitstellung von Microsoft Azure, z. B. [Azure Government für US-Regierung](https://azure.microsoft.com/features/gov/), [AzureCloud für globale Azure](https://portal.azure.com)und [AzureChinaCloud für Azure von 21Vianet in China betrieben](http://www.windowsazure.cn/). Sie können neue Azure-Umgebungen für Meile Government und Azure China bereitstellen.

Um Azure-Speicher mit AzureChinaCloud verwenden zu können, müssen Sie einen Speicherkontext zu erstellen, der mit AzureChinaCloud verknüpft ist. Führen Sie die folgenden Schritte aus, um Ihnen den Einstieg:

1.  Führen Sie das Cmdlet " [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) ", um die verfügbaren Azure-Umgebungen finden Sie unter:

    `Get-AzureEnvironment`

2.  Hinzufügen eines Kontos Azure China zu Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Erstellen Sie einen Speicherkontext für ein Konto AzureChinaCloud:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Um Azure-Speicher mit [US Azure Government](https://azure.microsoft.com/features/gov/)verwenden zu können, sollten Sie definieren eine neue Umgebung und erstellen Sie dann einen neuen Speicherkontext mit dieser Umgebung:

1.  Führen Sie das Cmdlet " [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) ", um die verfügbaren Azure-Umgebungen finden Sie unter:

    `Get-AzureEnvironment`

2.  Hinzufügen eines Kontos Azure US-Regierung in Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Erstellen Sie einen Speicherkontext für ein Konto AzureUSGovernment:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Weitere Informationen finden Sie unter:

- [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).
- [Übersicht über die Unterschiede beim Erstellen einer Anwendung China-Diensts](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Nächste Schritte
In diesem Handbuch haben Sie zum Verwalten von Azure-Speicher mit Azure PowerShell erfahren. Hier sind einige verwandten Artikeln und Ressourcen mehr über diese aus.

- [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
- [PowerShell-Cmdlets Azure-Speicher](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShell-Referenz](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next

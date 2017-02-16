<properties
   pageTitle="Erste Schritte mit Lake Datenspeicher | Azure"
   description="Verwenden Sie zum Erstellen eines Kontos Lake Datenspeicher und einfache Operationen Azure PowerShell"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Erste Schritte mit Azure Lake Datenspeicher mithilfe der PowerShell Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Erfahren Sie, wie Sie mithilfe von Azure PowerShell erstellen Sie ein Konto Azure Lake Datenspeicher und einfache Operationen beispielsweise wie Erstellen von Ordnern, hoch- und Herunterladen von Datendateien, löschen Sie Ihr Konto usw.. Weitere Informationen zu Lake Datenspeicher finden Sie unter [Übersicht der dem Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 oder größer**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Authentifizierung

In diesem Artikel wird einfacher Authentifizierung mit Lake Datenspeicher, in dem Sie aufgefordert werden, geben Sie Ihre Kontoanmeldeinformationen ein Azure-verwendet. Die Zugriffsebene Lake Datenspeicher Konto und Dateisystem dann, der der Benutzer die Zugriffsebene unterliegt. Es gibt jedoch andere Ansätze sowie zum Authentifizieren mit Lake Datenspeicher, die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anleitungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure Lake Datenspeicher

1. Öffnen Sie auf dem Desktop ein neues Windows PowerShell-Fenster zu, und geben Sie den folgenden Codeausschnitt zum Melden Sie sich bei Ihrem Konto Azure, legen Sie das Abonnement und den Anbieter Lake Datenspeicher registriert. Wenn Sie aufgefordert werden, melden Sie sich, stellen Sie sicher, dass Sie in einem von Ihrem Admininistrators/Besitzer anmelden:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Ein Konto Azure Lake Datenspeicher ist eine Azure Ressourcengruppe zugeordnet. Erstellen einer Ressourcengruppe Azure zunächst.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Erstellen einer Ressourcengruppe Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Erstellen einer Ressourcengruppe Azure")

2. Erstellen Sie ein Azure Lake Datenspeicher-Konto an. Der Name, den Sie angeben, darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Erstellen eines Kontos Azure dem Datenspeicher] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Erstellen eines Kontos Azure dem Datenspeicher")

3. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe für diesen sollte **erfüllt**werden.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Erstellen Sie in Ihrer Azure Lake Datenspeicher Directory-Strukturen

Sie können die Verzeichnisse unter Ihrem Konto Azure Lake Datenspeicher zum Verwalten und zum Speichern der Daten erstellen.

1. Geben Sie ein Stammverzeichnis an.

        $myrootdir = "/"

2. Erstellen Sie ein neues Verzeichnis namens **Mynewdirectory** unter dem angegebenen Stamm.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Stellen Sie sicher, dass das neue Verzeichnis erfolgreich erstellt wird.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Es sollte eine Ausgabe wie die folgende angezeigt:

    ![Vergewissern Sie sich Verzeichnis] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Vergewissern Sie sich Verzeichnis")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Hochladen von Daten in Ihrer Azure Lake Datenspeicher

Sie können Ihre Daten hochladen, Sees Datenspeicher direkt auf der Stammebene oder ein Verzeichnis, das Sie in das Konto erstellt haben. Die folgenden Codeausschnitte veranschaulichen, wie einige Beispieldaten im Verzeichnis (**Mynewdirectory**) hochladen, die Sie im vorherigen Abschnitt erstellt haben.

Wenn Sie einige Beispieldaten hochladen suchen, können Sie den Ordner **Krankenwagen Daten** aus dem [Azure Daten dem Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)abrufen. Laden Sie die Datei, und speichern Sie es in einem lokalen Verzeichnis auf Ihrem Computer, z. B. C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, herunterladen und Löschen von Daten aus Ihrem Lake Datenspeicher

Wenn Sie eine Datei umbenennen möchten, verwenden Sie den folgenden Befehl aus:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Wenn eine Datei herunterladen möchten, verwenden Sie den folgenden Befehl aus:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Verwenden Sie zum Löschen einer Datei mit dem folgenden Befehl ein:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Wenn Sie dazu aufgefordert werden, geben Sie **Y** , um das Element zu löschen. Wenn Sie mehr als eine Datei zu löschenden verfügen, können Sie alle Pfade durch Kommas getrennte bereitstellen.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Löschen Sie Ihr Konto Azure Lake Datenspeicher

Verwenden Sie den folgenden Befehl aus, um Ihr Konto Lake Datenspeicher löschen.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Wenn Sie dazu aufgefordert werden, geben Sie **Y** , um das Konto zu löschen.


## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)

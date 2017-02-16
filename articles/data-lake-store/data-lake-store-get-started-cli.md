<properties
   pageTitle="Erste Schritte mit Lake Datenspeicher Plattform-Befehl Linie Schnittstelle | Microsoft Azure"
   description="Verwenden Sie zum Erstellen eines Kontos Lake Datenspeicher und einfache Operationen Azure Plattformen Befehlszeile"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Erste Schritte mit Azure Lake Datenspeicher mit Azure Befehlszeile

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informationen Sie zum Verwenden von Azure Befehl Linie Benutzeroberflächen erstellen Sie ein Konto Azure Lake Datenspeicher und einfache Operationen beispielsweise wie Erstellen von Ordnern, hoch- und Herunterladen von Datendateien oder Löschen von Ihrer Firma, usw.. Weitere Informationen zu Lake Datenspeicher finden Sie unter [Übersicht der dem Datenspeicher](data-lake-store-overview.md).

Die CLI Azure ist in Node.js implementiert. Sie können auf einer beliebigen Plattform verwendet werden, Node.js, einschließlich Windows, Mac und Linux unterstützt. Die Azure ist Quelle öffnen. Der Quellcode wird im GitHub am <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>verwaltet. Dieser Artikel behandelt nur die CLI Azure mit Lake Datenspeicher verwenden. Nach einem allgemeinen Leitfaden zum Azure CLI verwenden, finden Sie unter [Verwendung der CLI Azure] [azure-command-line-tools].


##<a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** - finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) Installation und Konfiguration von Informationen. Stellen Sie sicher, dass Sie Ihren Computer nach der Installation der CLI neu starten.

## <a name="authentication"></a>Authentifizierung

In diesem Artikel verwendet Authentifizierung einfacher mit Lake Datenspeicher, in dem Sie als Endbenutzer Benutzer anmelden. Die Zugriffsebene Lake Datenspeicher Konto und Dateisystem dann, der der Benutzer die Zugriffsebene unterliegt. Es gibt jedoch andere Ansätze sowie zum Authentifizieren mit Lake Datenspeicher, die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anleitungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Melden Sie sich Ihr Abonnement Azure

1. Führen Sie die Schritte beschrieben, die in [Verbindung mit einer Azure-Abonnement aus der Azure Line Interface (CLI Azure) herstellen](../xplat-cli-connect.md) , und verbinden Sie Ihr Abonnement mit den `azure login` Methode.

2. Liste der Abonnements, die mit Ihrem Konto verbunden sind die `azure account list` Befehl.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Aus der obigen Ausgabe **Azure-Sub-1** aktiviert ist, und das andere Abonnement ist **Azure-Sub-2**. 

3. Wählen Sie das Abonnement, dem Sie unter arbeiten möchten. Wenn Sie unter dem Abonnement Azure-Sub-2 arbeiten möchten, verwenden Sie die `azure account set` Befehl.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure Lake Datenspeicher

Öffnen Sie ein Eingabeaufforderungsfenster, Shell oder eine terminal Sitzung, und führen Sie die folgenden Befehle.

2. Wechseln Sie zur Azure Ressourcenmanager Modus mit den folgenden Befehl aus:

        azure config mode arm


5. Erstellen einer neuen Ressourcengruppe an. Geben Sie in den folgenden Befehl aus die Parameterwerte, die Sie verwenden möchten.

        azure group create <resourceGroup> <location>

    Wenn Sie der Namen des Orts Leerzeichen enthält, setzen Sie ihn in Anführungszeichen ein. Beispielsweise "ostasiatischen US 2".

5. Erstellen Sie das Konto Lake Datenspeicher.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Erstellen von Ordnern im Datenspeicher Lake

Erstellen von Ordnern unter Ihrem Konto Azure Lake Datenspeicher zu verwalten und Daten speichern. Verwenden Sie den folgenden Befehl aus, um einen Ordner namens "Mynewfolder" im Stammverzeichnis der Lake Datenspeicher zu erstellen.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Beispiel:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Hochladen von Daten in Ihrer Lake Datenspeicher

Sie können Ihre Daten hochladen, Sees Datenspeicher direkt auf der Stammebene oder in einen Ordner, den Sie innerhalb des Kontos erstellt. Die folgenden Codeausschnitte veranschaulichen einige Beispieldaten Hochladen von in den Ordner (**Mynewfolder**), die Sie im vorherigen Abschnitt erstellt haben.

Wenn Sie einige Beispieldaten hochladen suchen, können Sie den Ordner **Krankenwagen Daten** aus dem [Azure Daten dem Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)abrufen. Laden Sie die Datei, und speichern Sie es in einem lokalen Verzeichnis auf Ihrem Computer, z. B. C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Beispiel:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Listendateien in Lake Datenspeicher

Verwenden Sie den folgenden Befehl aus, um die Dateien in einem Konto Lake Datenspeicher aufzulisten.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Beispiel:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Die Ausgabe dieses sollte ähnlich wie der folgende aussehen:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, herunterladen und Löschen von Daten aus Ihrem Lake Datenspeicher

* **Zum Umbenennen einer Datei**, verwenden Sie den folgenden Befehl aus:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Beispiel:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Um eine Datei nicht herunterladen**, verwenden Sie den folgenden Befehl aus. Stellen Sie sicher, dass der Zielpfad, die, den Sie, bereits angeben, vorhanden ist.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Beispiel:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Zum Löschen einer Datei**, verwenden Sie den folgenden Befehl aus:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Beispiel:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Wenn Sie dazu aufgefordert werden, geben Sie **Y** , um das Element zu löschen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Anzeigen der Access Control List für einen Ordner in Lake Datenspeicher

Verwenden Sie den folgenden Befehl aus, um die ACLs für einen Ordner Lake Datenspeicher anzuzeigen. In der aktuellen Version können ACLs nur auf der Stammebene der Datenspeicher Lake festgelegt werden. Ja, wird der der unten angezeigten Pfadparameter immer Root (/) sein.

    azure datalake store permissions show <dataLakeStoreName> <path>

Beispiel:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Löschen Sie Ihr Konto Lake Datenspeicher

Verwenden Sie den folgenden Befehl aus, um ein Konto Lake Datenspeicher löschen.

    azure datalake store account delete <dataLakeStoreAccountName>

Beispiel:

    azure datalake store account delete mynewdatalakestore

Wenn Sie dazu aufgefordert werden, geben Sie **Y** , um das Konto zu löschen.


## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md

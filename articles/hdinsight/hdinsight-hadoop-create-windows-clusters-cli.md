<properties
   pageTitle="Erstellen von Windows-basiertem Hadoop Cluster in mit CLI Azure HDInsight"
    description="Erfahren Sie, wie Sie mithilfe von Azure CLI Cluster für Azure-HDInsight zu erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Erstellen von Windows-basiertem Hadoop Cluster in mit CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie Sie mithilfe von CLI Azure HDInsight-Cluster erstellen. Für die Clustererstellung von anderen Tools und Features klicken Sie auf der Registerkarte wählen Sie am oberen Rand dieser Seite oder finden Sie unter [Methoden zur Erstellung](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voraussetzungen für:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Bevor Sie die Anweisungen in diesem Artikel beginnen, müssen Sie die folgenden verfügen:

- **Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Herstellen einer Verbindung Azure mit

Verwenden Sie den folgenden Befehl mit Azure verbinden:

    azure login

Weitere Informationen zum Authentifizieren über ein Konto geschäftlichen oder schulnotizbücher finden Sie unter [Verbinden zu einem Azure Abonnement über die Befehlszeile Azure](../xplat-cli-connect.md).

Verwenden Sie den folgenden Befehl aus, um in der Cloud-Modus zu schalten:

    azure config mode arm

Um Hilfe zu erhalten, verwenden Sie den Schalter **-h** .  Beispiel:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Cluster erstellen

Sie benötigen einer Azure Ressource Management (Cloud) und ein Azure Blob-Speicher-Konto, bevor Sie einen HDInsight Cluster erstellen können. Um einen Cluster HDInsight erstellen möchten, müssen Sie Folgendes angeben:

- **Ressourcengruppe Azure**: A Daten dem Analytics-Konto muss innerhalb einer Azure Ressourcengruppe erstellt werden. Azure Ressourcenmanager können Sie für die Arbeit mit den Ressourcen in der Anwendung als Gruppe. Sie können bereitstellen, aktualisieren oder Löschen aller Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang.

    Listen Sie die Ressourcengruppen in Ihrem Abonnement:

        azure group list

    So erstellen Sie eine neue Ressourcengruppe

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight Clusternamen**

- **Standort**: eine der zentriert Azure-Daten, die die HDInsight Cluster unterstützt. Eine Liste der unterstützten Speicherorte finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Speicher Standardkonto**: HDInsight verwendet einen Azure BLOB-Speichercontainer als Standard-Dateisystem. Ein Konto Azure-Speicher ist erforderlich, bevor Sie einen HDInsight Cluster erstellen können.

    So erstellen Sie ein neues Konto mit Azure-Speicher

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Das Konto Speicher muss mit HDInsight in Data Center zusammengestellt werden.
    > Kontotyp Speicher nicht möglich ZRS, da ZRS Tabelle nicht unterstützt.

    Informationen zum Erstellen eines Kontos Azure-Speicher mithilfe der Azure-Portal finden Sie unter [erstellen, verwalten oder Löschen eines Kontos Speicher] [Azure-erstellen-Storageaccount].

    Wenn Sie bereits über ein Speicherkonto verfügen jedoch den Kontonamen und kontoschlüssel nicht kennen, können Sie die folgenden Befehle, zum Abrufen der Informationen:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Details erhalten Sie Informationen mithilfe der Azure-Portal finden Sie im Abschnitt "Verwalten Ihr Speicherkonto" [zur Azure-Speicher-Konten](../storage/storage-create-storage-account#manage-your-storage-account).

- **(Optional) Standard Blob Container**: der **Azure Hdinsight Cluster erstellen** Befehl erstellt den Container aus, wenn es nicht vorhanden ist. Wenn Sie den Container im Voraus erstellen auswählen, können Sie den folgenden Befehl aus:

    Azure-Speichercontainer erstellen –-Kontonamen "<Storage Account Name>" – kontoschlüssel <Storage Account Key> [ContainerName]

Nachdem Sie das vorbereitet Speicher-Konto haben, sind Sie bereit sind, einen Cluster zu erstellen:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Verwenden von Konfigurationsdateien Cluster erstellen
In der Regel erstellen einen HDInsight Cluster, Aufträge daran ausführen und löschen Sie dann den Cluster Ausschneiden unten die Kosten. Die Benutzeroberfläche Line bietet Ihnen die Möglichkeit, die Konfigurationen in eine Datei speichern, damit Sie es jedes Mal, wenn Sie einen Cluster erstellen wiederverwenden können.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Beispiel: Erstellen einer Konfigurationsdatei, die eine Skriptaktion zur Ausführung beim Erstellen eines Clusters enthält.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Erstellen Sie mit der Skriptaktion Cluster

Erstellen einer Konfigurationsdatei, die eine Skriptaktion zur Ausführung beim Erstellen eines Clusters enthält.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Erstellen Sie einen Cluster mit einer Skriptaktion

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Allgemeine Skript Aktionsinformationen finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion "Skript" (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Verwenden von Vorlagen des Cloud Cluster erstellen

Sie können CLI Cluster erstellen, indem Sie Cloud-Vorlagen. Finden Sie unter [mit Azure CLI bereitstellen](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Siehe auch

- [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – Informationen zum Arbeiten mit Ihren Cluster HDInsight starten
- [Hadoop übermitteln Aufträge programmgesteuert](hdinsight-submit-hadoop-jobs-programmatically.md) - erfahren Sie, wie programmgesteuert Aufträge mit HDInsight senden
- [Verwalten von Hadoop Cluster unter Verwendung der CLI Azure HDInsight](hdinsight-administer-use-command-line.md)
- [Verwendung der Azure CLI für Mac, Linux und Windows Azure Service-Verwaltung](../virtual-machines-command-line-tools.md)

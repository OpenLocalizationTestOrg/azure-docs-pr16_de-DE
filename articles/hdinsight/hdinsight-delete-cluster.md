<properties
pageTitle="So löschen Sie einen HDInsight Cluster | Azure"
description="Informationen über die verschiedenen Optionen können Sie einen HDInsight Cluster löschen."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="how-to-delete-an-hdinsight-cluster"></a>So löschen Sie einen HDInsight cluster

HDInsight Cluster Abrechnung beginnt, sobald ein Cluster erstellt wird und wird beendet, wenn der Cluster gelöscht wird und pro Minute, habe, ist sodass Sie immer Ihren Cluster löschen soll, wenn es nicht mehr verwendet wird. In diesem Dokument erfahren Sie, wie einen Cluster mit Azure-Portal, Azure PowerShell und Azure CLI gelöscht werden.

> [AZURE.IMPORTANT] Löschen eines HDInsight Clusters wird die Azure-Speicher-Konten zugeordnet Cluster nicht gelöscht. So können Sie beibehalten und Wiederverwenden von Daten durch den Cluster gespeichert.

##<a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich im [Portal Azure](https://portal.azure.com) , und wählen Sie Ihren Cluster HDInsight. Wenn Ihre HDInsight Cluster nicht auf dem Dashboard fixiert ist, können Sie auf der rechten Seite der Navigationsleiste dafür anhand des Namens, mit dem Suchfeld (Lupensymbol), suchen.

    ![Portal suchen](./media/hdinsight-delete-cluster/navbar.png)

2. Nachdem das Blade für den Cluster geöffnet wird, wählen Sie das Symbol __Löschen__ . Wenn Sie dazu aufgefordert werden, wählen Sie __Ja__ Cluster löschen.

    ![Löschen-Symbol](./media/hdinsight-delete-cluster/deletecluster.png)

##<a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie den folgenden Befehl aus eine Aufforderung PowerShell Cluster zu löschen:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Ersetzen Sie __CLUSTERNAME__ mit dem Namen der HDInsight Cluster ein.

##<a name="azure-cli"></a>Azure CLI

Aus einer dazu aufgefordert werden anhand der folgenden um Cluster zu löschen:

    azure hdinsight cluster delete CLUSTERNAME
    
Ersetzen Sie __CLUSTERNAME__ mit dem Namen der HDInsight Cluster ein.

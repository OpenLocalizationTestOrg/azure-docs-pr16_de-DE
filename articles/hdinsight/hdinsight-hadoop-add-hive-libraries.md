<properties
pageTitle="Struktur Bibliotheken hinzufügen, während der Erstellung von HDInsight Cluster | Azure"
description="Erfahren Sie, wie Struktur Bibliotheken (JAR-Dateien), hinzufügen zu einem Cluster HDInsight während der Clustererstellung."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Hinzufügen von Bibliotheken Struktur während der Erstellung von HDInsight cluster

Wenn Sie Bibliotheken, die Sie häufig mit Struktur auf HDInsight verwenden verfügen, enthält dieses Dokument Informationen zur Verwendung einer Aktion Skript die Bibliotheken während der Clustererstellung vorab zu laden. Dadurch wird die Bibliotheken global verfügbar in die Struktur (keine müssen [JAR hinzufügen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) verwenden, um diese zu laden.)

##<a name="how-it-works"></a>So funktioniert es

Beim Erstellen eines Clusters können Sie optional eine Aktion Skript angeben, die ein Skript auf den Clusterknoten ausgeführt wird, während sie erstellt werden. Das Skript in diesem Dokument akzeptiert einen einzigen Parameter, die einen Speicherort WASB die Bibliotheken (gespeichert als JAR-Dateien) enthält, die vorab geladen werden.

Während der Clustererstellung das Skript listet die Dateien, die sie kopiert die `/usr/lib/customhivelibs/` Verzeichnis auf den Kopf und Worker-Knoten fügt dann hinzu, damit die `hive.aux.jars.path` Eigenschaft in der `core-site.xml` Datei. Klicken Sie auf Linux-basierten Cluster wird außerdem aktualisiert, die `hive-env.sh` Datei durch den Speicherort der Dateien.

> [AZURE.NOTE] Verwenden die Skriptaktionen in diesem Artikel bereitgestellt, die Bibliotheken in den folgenden Szenarien:
>
> * __HDInsight Linux-basierten__ - bei Verwendung des __Befehlszeile Struktur__, __WebHCat__ (Templeton) und __HiveServer2__.
> * __Windows-basiertem HDInsight__ - bei Verwendung des __Befehlszeile Struktur__ und __WebHCat__ (Templeton).

##<a name="the-script"></a>Das Skript

__Skript-Speicherort__

Für __Linux-basierten Cluster__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Für __Windows-basiertem Cluster__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Anforderungen__

* Die Skripts müssen auf den __Kopf Knoten__ und die __Worker Knoten__angewendet werden.

* Die Gläser, die Sie installieren möchten, müssen in einem __einzelnen Container__in Azure Blob Storage gespeichert werden. 

* Das mit der Bibliothek von JAR-Dateien __müssen__ Speicher-Konto verknüpft werden zum Cluster HDInsight während der Erstellung. Dies kann auf zwei Arten erfolgen:

    * Werden Sie in einem Container auf das Speicher-Standardkonto für den Cluster.
    
    * Werden Sie in einem Container auf eine verknüpfte Speichercontainer. Beispielsweise können Sie im Portal __Optionale Konfiguration__, __verknüpfte Speicherkonten__ zusätzlichen Speicher hinzufügen verwenden.

* Der Pfad WASB zum Container muss als Parameter für die Aktion Skript angegeben werden. Angenommen, wenn die Gläser in einem Container mit dem Namen __Bibliotheken__ auf einem Speicherkonto mit dem Namen __Mystorage__gespeichert sind, der Parameter wäre __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Dieses Dokument wird davon ausgegangen, dass Sie haben bereits ein Speicherkonto erstellen, BLOB-Container, und sie die Dateien geladen. 
    >
    > Wenn Sie ein Speicherkonto nicht erstellt haben, können Sie über das [Azure-Portal](https://portal.azure.com)ausführen. Sie können ein Programm wie [Azure-Speicher-Explorer](http://storageexplorer.com/) klicken Sie dann einen neuen Container erstellen, in dem Konto und Hochladen von Dateien verwenden.

##<a name="create-a-cluster-using-the-script"></a>Erstellen Sie einen Cluster mit dem Skript

> [AZURE.NOTE] Die folgenden Schritte durch erstellen einen neuen HDInsight Linux-basierten Cluster. Zum Erstellen eines neuen Windows-basierten Clusters __Windows__ beim Erstellen des Clusters als Cluster OS wählen Sie aus, und verwenden Sie das Skript Windows (PowerShell) anstelle des Bash Skripts.
> 
> Azure PowerShell oder HDInsight .NET SDK können Sie auch um einen Cluster verwenden dieses Skripts zu erstellen. Weitere Informationen zur Verwendung dieser Methode finden Sie unter [Anpassen HDInsight Cluster mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Starten Sie einen Cluster anhand der Schritte in [Bereitstellen HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal)bereitgestellt, aber nicht abschließen Sie provisioning.

2. Klicken Sie auf das Blade **Optionale Konfiguration** wählen Sie **Skript-Aktionen**aus, und geben Sie die Informationen ein, wie unten dargestellt:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __WORKER__: Aktivieren Sie diese Option.
    * __ZOOKEEPER__: Dieses Feld leer lassen.
    * __Parameter__: Geben Sie die Adresse WASB zu den Container und Speicher-Konto, das Gläser enthält. Beispielsweise __wasbs://libs@mystorage.blob.core.windows.net/__.

3. Am unteren Rand der **Skript-Aktionen**verwenden Sie die Schaltfläche **auswählen** zum Speichern der Konfiguration aus.

4. Klicken Sie auf das Blade **Optionale Konfiguration** wählen Sie __Verknüpfte Speicher-Konten__ , und wählen Sie den Link zum __Hinzufügen eines Speicher-Taste__ . Wählen Sie das Speicherkonto, das die Gläser enthält, und klicken Sie dann mithilfe der Schaltflächen __Wählen Sie__ Einstellungen speichern und das __Optionale Konfiguration__ Blade zurückzukehren.

5. Verwenden Sie die Schaltfläche **Wählen Sie** am unteren Rand der Blade **Optionale Konfiguration** , die optionale Konfigurationsinformationen zu speichern.

6. Fortsetzen Sie den Cluster bereitgestellt, wie unter [Bereitstellen von HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Nachdem Clustererstellung abgeschlossen ist, Sie werden möglicherweise verwenden Sie die Gläser hinzugefügt durch dieses Skript aus Struktur ohne Verwenden der `ADD JAR` Anweisung.

##<a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt Struktur Bibliotheken in JAR-Dateien zu einem HDInsight Cluster enthalten sind, während der Clustererstellung hinzufügen. Weitere Informationen zum Arbeiten mit Struktur finden Sie unter [Verwendung mit HDInsight Struktur](hdinsight-use-hive.md)

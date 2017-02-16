<properties
pageTitle="Einschränken der HDInsight Zugriff auf Daten mit Access Signaturen freigegeben"
description="Erfahren Sie, wie freigegebene Access Signaturen zum Einschränken des Zugriffs für HDInsight in Azure-Speicher Blobs gespeicherten Daten verwenden."
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
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Verwenden von Azure-Speicher freigegeben Access Signaturen zum Einschränken des Zugriffs auf Daten mit HDInsight

HDInsight verwendet Azure-Speicher Blobs zum Speichern von Daten. Während HDInsight Vollzugriff auf das Blob als Standard-Speicherplatz für den Cluster verwendet haben muss, können Sie die Berechtigungen, um Daten aus anderen Blobs vom Cluster verwendet einschränken. Möglicherweise möchten Sie beispielsweise einiger Daten Schreibschutz versehen. Sie können dazu Access Signaturen freigegeben.

Freigegebene Access Signaturen (SAS) sind ein Feature von Azure-Speicher-Konten, die Sie den Zugriff auf Daten einschränken kann. Bereitstellen von z. B. schreibgeschützten Zugriff auf Daten. In diesem Dokument erfahren Sie, wie SAS Lese- und Liste schreibgeschützten Zugriff auf einen Blob-Container aus HDInsight aktivieren.

##<a name="requirements"></a>Anforderungen

* Ein Azure-Abonnement

* C# oder Python. Beispiel für C#-Code wird als eine Visual Studio-Lösung bereitgestellt.

    * Visual Studio muss Version 2013 oder 2015
    
    * Python muss Version 2.7 oder höher

* Eine Linux-basierten HDInsight cluster oder [Azure PowerShell] [ powershell] – Wenn Sie einen vorhandenen Linux-basierten Cluster verfügen, können Ambari zum Hinzufügen einer freigegebenen Access-Signatur zu Cluster. Wenn dies nicht der Fall ist, können Sie einen neuen Cluster erstellen und Hinzufügen einer Signatur für freigegebene Access während der Clustererstellung Azure PowerShell.

* Die Beispieldateien aus [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Dieses Repository weist die folgenden:

    * Ein Visual Studio-Projekt, das eine Speichercontainer, gespeicherte Richtlinie und SAS zur Verwendung mit HDInsight erstellt werden können
    
    * Ein Python-Skript, das für die Verwendung mit HDInsight einen Speichercontainer, gespeicherte Richtlinie und SAS erstellen können
    
    * Ein Powershellskript, einen neuen HDInsight Cluster erstellen und konfigurieren, um die SAS verwenden kann.

##<a name="shared-access-signatures"></a>Freigegebene Access-Signaturen

Es gibt zwei Arten von Signaturen für freigegebene Access aus:

* Ad-hoc-: Startzeit, Ablaufzeit und Berechtigungen für die SAS werden alle auf dem SAS URI angegebenen (oder impliziten, in den Fall, in dem Startzeit nicht angegeben wird).

* Gespeicherte Access-Richtlinie: eine gespeicherte Zugriffsrichtlinie auf einer Ressource Container - Blob Container, Tabelle, Warteschlange oder Dateifreigabe - definiert ist, und zum Verwalten von Einschränkungen für eine oder mehrere gemeinsamen Zugriff Signaturen verwendet werden können. Wenn Sie eine gespeicherte Zugriffsrichtlinie einem SAS zuordnen, erbt die SAS Einschränkungen - Startzeit, Ablaufzeit und Berechtigungen – für die gespeicherte Zugriffsrichtlinie definiert ist.

Der Unterschied zwischen den beiden Formularen ist wichtig für ein Key Szenario: Sperrung. Eine SAS ist einer URL, sodass alle Personen, die die SAS erhält, unabhängig davon, wer zunächst angefordert verwenden kann. Wenn eine SAS öffentlich veröffentlicht wird, kann es von allen Benutzern in der Welt verwendet werden. Eine SAS, die verteilt ist ist gültig, bis eine der vier Punkte geschieht:

1. Klicken Sie auf die SAS angegebene Zeitspanne Ablauf erreicht ist.

2. Klicken Sie auf die gespeicherte Zugriffsrichtlinie optimiert die SAS angegebene Zeitspanne Ablauf erreicht ist (Wenn eine gespeicherte Zugriffsrichtlinie verwiesen wird, und wenn es sich um eine Ablaufzeit angibt). Dies kann entweder auftreten, da das Intervall abgelaufen ist, oder Sie einem Ablaufdatum in der Vergangenheit, damit eine Methode, um die SAS widerrufen also die gespeicherte Zugriffsrichtlinie geändert haben.

3. Die gespeicherte Zugriffsrichtlinie optimiert die SAS, entfällt, welche ist eine weitere Möglichkeit, die SAS widerrufen. Beachten Sie, dass, wenn Sie die gespeicherten Zugriffsrichtlinie mit demselben Namen neu erstellen, alle vorhandene SAS Token erneut gültige entsprechend den Berechtigungen, die gespeicherten Zugriffsrichtlinie (vorausgesetzt, die nicht den Ablaufzeit auf die SAS verstrichen) zugeordnet. Wenn Sie die SAS widerrufen festlegen möchten, müssen Sie einen anderen Namen verwenden, wenn Sie die Zugriffsrichtlinie mit einem Ablaufdatum in der Zukunft neu erstellen.

4. Der kontoschlüssel, der zum Erstellen der SAS verwendet wurde neu erstellt. Beachten Sie, dass Hiermit werden alle Komponenten der Anwendung, die mit diesem Konto-Key fehlschlägt authentifizieren, bis sie aktualisiert werden, um entweder die anderen gültigen Konto-Taste oder die Taste neu generierten Konto verwenden.

> [AZURE.IMPORTANT] URI eine freigegebenen Access-Signatur zugeordnet ist, die zum Erstellen der Signatur verwendete Konto-Taste und die zugehörigen Speicherorten eine Zugriffsrichtlinie, (falls vorhanden). Wenn keine gespeicherten Zugriffsrichtlinie angegeben ist, ist die einzige Möglichkeit, eine Signatur gemeinsamen Zugriff widerrufen Konto Key ändern. 

Es wird empfohlen, dass Sie immer gespeicherte Access-Richtlinien, damit Sie können Signaturen widerrufen oder das Ablaufdatum nach Bedarf zu verlängern. Die Schritte in diesem Dokument verwenden gespeicherte Access-Richtlinien zum Generieren der SAS.

Weitere Informationen zu Access-Signaturen freigegeben finden Sie unter [Grundlegendes zu SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Erstellen einer gespeicherten Richtlinie und einem SAS generieren

Sie müssen eine gespeicherte Richtlinie aktuell programmgesteuert erstellen. Sie können sowohl c# als auch Python Beispiel zum Erstellen einer gespeicherten Richtlinie und SAS am [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)suchen.

###<a name="create-a-stored-policy-and-sas-using-c"></a>Erstellen Sie eine gespeicherte Richtlinie und SAS mit C\#

1. Öffnen Sie die Lösung in Visual Studio.

2. Im Lösung-Explorer mit der rechten Maustaste auf das Projekt __SASToken__ , und wählen Sie __Eigenschaften__aus.

3. Wählen Sie __Einstellungen__ aus, und fügen Sie die Werte für die folgenden Einträge hinzu:

    * StorageConnectionString: Die Verbindungszeichenfolge für das Speicherkonto, dem Sie eine gespeicherte Richtlinie und SAS für erstellen möchten. Das Format sollten `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` , in dem `myaccount` ist der Name Ihres Kontos Speicher und `mykey` ist der Schlüssel für das Speicherkonto.
    
    * ContainerName: Der Container im Speicherkonto, dem Sie Zugriff einschränken möchten.
    
    * SASPolicyName: Der Name für die gespeicherte Richtlinie verwendet werden soll, die erstellt werden soll.
    
    * FileToUpload: Der Pfad zu einer Datei, die in den Container hochgeladen wird.
    
4. Führen Sie das Projekt an. Ein Console-Fenster wird angezeigt, und Informationen ähnlich wie der folgende wird angezeigt, nachdem die SAS generiert wurde:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Speichern Sie das Token SAS Richtlinie, da Sie dies benötigen, wenn Sie das Speicherkonto mit Ihren Cluster HDInsight verknüpfen. Sie benötigen auch den Kontonamen Speicherplatz und den Containername.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Erstellen Sie eine gespeicherte Richtlinie und SAS Python verwenden

1. Öffnen Sie die Datei SASToken.py, und ändern Sie die folgenden Werte:

    * Richtlinie\_Namen: den Namen für die gespeicherte Richtlinie verwendet werden soll, die erstellt werden soll.
    
    * Speicher\_Konto\_Namen: der Name Ihres Kontos Speicher.
    
    * Speicher\_Konto\_Schlüssel: die Taste für das Speicherkonto.
    
    * Speicher\_Container\_Name: den Container im Speicherkonto, das Sie Zugriff einschränken möchten.
    
    * Beispiel\_Datei\_Pfad: der Pfad zu einer Datei, die in den Container hochgeladen wird

2. Führen Sie das Skript. Es wird das SAS Token ähnlich wie der folgende angezeigt, wenn das Skript abgeschlossen ist:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Speichern Sie das Token SAS Richtlinie, da Sie dies benötigen, wenn Sie das Speicherkonto mit Ihren Cluster HDInsight verknüpfen. Sie benötigen auch den Kontonamen Speicherplatz und den Containername.
    
##<a name="use-the-sas-with-hdinsight"></a>Verwenden Sie die SAS mit HDInsight

Beim Erstellen eines HDInsight Clusters müssen Sie einer primären Speicher-Konto angeben und optional können Sie zusätzlichen Speicherkonten angeben. Beide Methoden zum Hinzufügen von Speicher erfordern Vollzugriff auf die Speicherkonten und Container, die verwendet werden.

Um eine freigegebene Access-Signatur verwenden, den Zugriff auf einen Container beschränken, müssen Sie einen benutzerdefinierten Eintrag an der Konfiguration __Core-Website__ für den Cluster hinzufügen.

* Für __Windows-basiertem__ oder __Linux-basierten__ HDInsight Cluster kann dies während der Clustererstellung mithilfe der PowerShell erfolgen.

* Für __Linux-basierte__ HDInsight Cluster ändern Sie die Konfiguration nach der Clustererstellung Ambari verwenden.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Erstellen Sie einen neuen Cluster, der die SAS verwendet.

Ein Beispiel für das Erstellen eines HDInsight Clusters, die die SAS verwendet befindet sich auf der `CreateCluster` Verzeichnis des Repositorys. Gehen Sie folgendermaßen vor, um es zu verwenden:

1. Öffnen der `CreateCluster\HDInsightSAS.ps1` -Datei in einem Text-Editor, und ändern Sie die folgenden Werte am Anfang des Dokuments.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Ändern Sie beispielsweise `'mycluster'` auf den Namen des zu erstellenden Cluster. Die Werte des SAS sollte die Werte aus den vorherigen Schritten übereinstimmen, beim Erstellen eines Speicher-Konto und SAS Token.
    
    Nachdem Sie die Werte geändert haben, speichern Sie die Datei.

1. Öffnen Sie eine neue Azure PowerShell Aufforderung angezeigt. Wenn Sie nicht mit Azure PowerShell vertraut sind, oder nicht installiert haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][powershell].

2. Aus der entsprechenden Aufforderung für Ihr Abonnement Azure authentifizieren anhand der folgenden:

        Login-AzureRmAccount
    
    Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Konto Ihres Abonnements Azure.
    
    Wenn die Anmeldung mit mehreren Azure-Abonnements verknüpft ist, müssen Sie möglicherweise verwenden `Select-AzureRmSubscription` das Abonnement auswählen, Sie verwenden möchten.

2. Wechseln Sie von dazu aufgefordert werden, in der `CreateCluster` Verzeichnis, die Datei HDInsightSAS.ps1 enthält. Verwenden Sie dann die folgenden zum Ausführen des Skripts
        
        .\HDInsightSAS.ps1
    
    Wie das Skript ausgeführt wird, wird es Ausgabe auf die PowerShell-Eingabeaufforderung melden Sie sich beim Erstellen der Ressource gruppieren und Speicher-Konten. Es werden Sie aufgefordert, geben Sie den HTTP-Benutzer für den Cluster HDInsight einzugeben. Dies ist das Benutzerkonto, das zum Sichern des HTTP/s Zugriff auf den Cluster verwendet.
    
    Wenn Sie einen Linux-basierten Cluster erstellen, werden Sie auch für einer SSH Konto Benutzername und Kennwort aufgefordert. Dies wird Remote anmelden, um den Cluster verwendet.
    
    > [AZURE.IMPORTANT] Wenn für den HTTP/s oder SSH Benutzername und Kennwort aufgefordert werden, müssen Sie ein Kennwort eingeben, die die folgenden Kriterien erfüllt:
    >
    > - Mindestens 10 Zeichen lang muss sein
    > - Muss mindestens eine Ziffer enthalten
    > - Muss mindestens eine nicht alphanumerische Zeichen enthalten
    > - Müssen Sie mindestens eine obere oder Kleinbuchstaben enthalten.


Es wird eine Weile für dieses Skript ausführen, in der Regel ungefähr 15 Minuten dauern. Wenn Sie das Skript ohne Fehler abgeschlossen ist, wurde der Cluster erstellt.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Aktualisieren der SAS mit einem vorhandenen cluster

Wenn Sie einen vorhandenen Linux-basierten Cluster verfügen, können Sie die SAS an der Konfiguration __Core-Website__ hinzufügen, mithilfe der folgenden Schritte aus:

1. Öffnen Sie das Ambari Web-Benutzeroberfläche für Ihren Cluster an. Die Adresse für diese Seite ist https://YOURCLUSTERNAME.azurehdinsight.net. Wenn Sie dazu aufgefordert werden, authentifizieren sich an den Cluster mithilfe der Administratornamen (Admin) und das Kennwort ein, die Sie beim Erstellen des Clusters verwendet.

2. Wählen Sie von der linken Seite der Ambari Web-Benutzeroberfläche __HDFS__ aus, und wählen Sie dann auf die Registerkarte __Konfigurationen__ in der Mitte der Seite.

3. Wählen Sie die Registerkarte __Erweitert__ , und klicken Sie dann einen Bildlauf bis Sie im Abschnitt __benutzerdefinierte Core-Website finden__ .

4. Erweitern Sie im Abschnitt __benutzerdefinierte Core-Website__ und klicken Sie dann einen Bildlauf zum Ende, und wählen Sie den Link __... Eigenschaft hinzufügen__ . Verwenden Sie die folgenden Werte für die Felder __und den __Wert__ __ aus:

    * __Schlüssel__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Wert__: der SAS zurückgegebene c# oder Python-Anwendung, die Sie zuvor ausgeführt haben
    
    Ersetzen Sie __CONTAINERNAME__ durch den Containernamen, die, den Sie mit dem C#- oder SAS-Anwendung verwendet. Ersetzen Sie __STORAGEACCOUNTNAME__ mit dem Speicher Kontonamen, die, den Sie verwendet haben.

5. Klicken Sie auf die Schaltfläche __Hinzufügen__ , um diese Schlüssel und Wert zu speichern, und klicken Sie auf die Schaltfläche __Speichern__ , um die Konfiguration Änderungen zu speichern. Wenn Sie dazu aufgefordert werden, fügen Sie eine Beschreibung der Änderung ("Hinzufügen von SAS-Speicher Access" beispielsweise) hinzu, und klicken Sie dann auf __Speichern__.

    Klicken Sie auf __OK__ , wenn die Änderungen abgeschlossen wurden.

    > [AZURE.IMPORTANT] Dies speichert die Änderungen Konfiguration, aber Sie müssen mehrere Dienste neu starten, damit die Änderung wirksam wird.

6. Wählen Sie in der Ambari Web-Benutzeroberfläche aus der Liste auf der linken Seite __HDFS__ aus, und wählen Sie dann aus der Dropdownliste __Service-Aktionen__ auf der rechten Seite __Neu starten__ . Wenn Sie dazu aufgefordert werden, wählen Sie __Wartungsmodus aktivieren__ und dann __Conform wählen Sie alle neu starten".

    Wiederholen Sie diesen Vorgang für die MapReduce2 und aus Einträge aus der Liste auf der linken Seite der Seite ein.

7. Nachdem Sie diese neu gestartet haben, jedes Element einzeln auswählen, und deaktivieren Sie Wartungsmodus aus den __Dienst Aktionen__ Dropdown-Liste.

##<a name="test-restricted-access"></a>Testen des eingeschränkten Zugriffs

Um sicherzustellen, dass Sie den Zugriff eingeschränkt haben, verwenden Sie die folgenden Methoden:

* Verwenden Sie für __Windows-basiertem__ HDInsight Cluster Remotedesktop Verbindung mit dem Cluster ein. Weitere Informationen finden Sie unter [Connecto mit HDInsight RDP verwenden](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Nachdem die Verbindung hergestellt wurde, verwenden Sie das __Befehlszeile Hadoop__ -Symbol auf dem Desktop, um ein Eingabeaufforderungsfenster zu öffnen.

* Verwenden Sie für __Linux-basierte__ HDInsight Cluster SSH Verbindung zum Cluster ein. Finden Sie eine der folgenden Informationen zum Verwenden von SSH mit Linux-basierten Cluster aus:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Sobald Sie mit dem Cluster verbunden sind, gehen Sie folgendermaßen vor, um sicherzustellen, dass Sie nur gelesen und Liste Elemente auf der SAS-Speicher-Konto können:

1. Geben Sie von dazu aufgefordert werden Folgendes ein. Ersetzen Sie __SASCONTAINER__ durch den Namen des Containers für das SAS-Speicherkonto erstellt. Ersetzen Sie __SASACCOUNTNAME__ durch den Namen des Speicherkontos für die SAS verwendet:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Dadurch wird der Inhalt des Containers, der die Datei umfassen, die hochgeladen wurde Liste Wenn der Container und SAS erstellt wurde.
    
2. Verwenden Sie die folgenden, um sicherzustellen, dass Sie den Inhalt der Datei lesen können. Ersetzen Sie __SASCONTAINER__ und __SASACCOUNTNAME__ wie im vorherigen Schritt. Ersetzen Sie __Dateiname__ mit dem Namen der Datei angezeigt, die in den vorherigen Befehl aus:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Dadurch wird der Inhalt der Datei angegeben werden.
    
3. Laden Sie die Datei im lokalen Dateisystem anhand der folgenden:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Dadurch wird die Datei in einer lokalen Datei __TestFile.txt__herunterladen.

4. Hochladen die lokale Datei in eine neue Datei mit dem Namen __testupload.txt__ auf der SAS-Speicher anhand der folgenden:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Sie erhalten eine Meldung ähnlich wie der folgende aus:
    
        put: java.io.IOException
        
    Dieser Fehler tritt auf, da der Speicherort finden Sie hier + Liste nur ist. Setzen die Daten auf der Standard-Speicherplatz für den Cluster, also beschreibbare anhand der folgenden:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Diesmal, sollte der Vorgang erfolgreich abgeschlossen.
    
##<a name="troubleshooting"></a>Behandlung von Problemen

###<a name="a-task-was-canceled"></a>Eine Aufgabe wurde abgebrochen.

__Symptome__: Wenn einen Cluster mithilfe des PowerShell-Skripts zu erstellen, erhalten Sie möglicherweise die folgende Fehlermeldung angezeigt:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Ursache__: Dieser Fehler kann auftreten, wenn Sie ein Kennwort für den Administrator/HTTP-Benutzer für den Cluster oder (für Linux-basierten Cluster,) verwenden der Benutzer SSH.

__Lösung__: Verwenden Sie ein Kennwort ein, die die folgenden Kriterien erfüllt:

- Mindestens 10 Zeichen lang muss sein
- Muss mindestens eine Ziffer enthalten
- Muss mindestens eine nicht alphanumerische Zeichen enthalten
- Müssen Sie mindestens eine obere oder Kleinbuchstaben enthalten.

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie Ihren Cluster HDInsight beschränkter Zugriff Speicher hinzuzufügende vertraut gemacht haben, Weitere Informationen Sie zum Arbeiten mit Daten im Cluster enthalten sind:

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)

* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md

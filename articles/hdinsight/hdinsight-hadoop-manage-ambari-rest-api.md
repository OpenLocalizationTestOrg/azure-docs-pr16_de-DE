<properties
   pageTitle="Überwachen und Verwalten von HDInsight Cluster mithilfe der Apache Ambari REST-API | Microsoft Azure"
   description="Erfahren Sie, wie Ambari zum Überwachen und Verwalten von HDInsight Linux-basierten Cluster verwenden. In diesem Dokument erfahren Sie, wie die Ambari REST-API enthaltenen HDInsight Cluster verwendet werden."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Verwalten von HDInsight Cluster mithilfe der REST-API Ambari

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von einem Cluster Hadoop können, indem Sie eine einfache Web-Benutzeroberfläche und den REST-API verwenden. Ambari auf HDInsight Linux-basierten Cluster enthalten ist, und wird verwendet, um den Cluster überwachen und Konfiguration vorzunehmen. In diesem Dokument erfahren Sie die Grundlagen der Arbeit mit der Ambari REST-API, indem Sie allgemeine Aufgaben mit cURL.

##<a name="prerequisites"></a>Erforderliche Komponenten

* [Aufrollen](http://curl.haxx.se/): Dabei handelt es sich ein Plattform-Programm, die für die Arbeit mit den restlichen APIs über die Befehlszeile verwendet werden kann. In diesem Dokument wird es verwendet zur Kommunikation mit der Ambari REST-API.
* [Jq](https://stedolan.github.io/jq/): Jq ist ein Plattform-Befehlszeilendienstprogramm für die Arbeit mit JSON-Dokumente. In diesem Dokument wird es verwendet, die aus der Ambari REST-API zurückgegebenen JSON-Dokumente zu analysieren.
* [Azure CLI](../xplat-cli-install.md): ein Kreuz-Plattform Befehlszeilendienstprogramm für die Arbeit mit Azure Services.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a name="a-idwhatisawhat-is-ambari"></a><a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) macht Hadoop Management einfacher können, indem Sie ein einfach zu verwendendes Web-Benutzeroberfläche, die zum Bereitstellen, verwalten und überwachen Hadoop Cluster verwendet werden kann. Entwickler können diese Funktionen in ihre Programme mithilfe der [Ambari REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)integrieren.

Ambari wird standardmäßig mit HDInsight Linux-basierten Cluster bereitgestellt.

##<a name="rest-api"></a>REST-API

Der base-URI für die Ambari REST-API auf HDInsight ist https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, wobei __CLUSTERNAME__ den Namen Ihrer Cluster.

> [AZURE.IMPORTANT] Während der Clustername in den vollqualifizierten Domänennamen (FULLY Qualified Name) Teil der URI (CLUSTERNAME.azurehdinsight.net) Groß-und Kleinschreibung wird, werden die anderen Vorkommen im URI Groß-/Kleinschreibung beachtet. Angenommen, falls Ihre Cluster MeinCluster benannt wird, finden Sie nachstehend gültige URIs:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Die folgenden URIs einen Fehler zurück, da das zweite Vorkommen des Namens nicht die Groß-/Kleinschreibung ist.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Herstellen einer Verbindung mit Ambari auf HDInsight ist HTTPS erforderlich. Wenn die Verbindung zu authentifizieren, müssen Sie verwenden der Administratorkontonamen (die Standardeinstellung ist __Administrator__) und das Kennwort ein, die Sie zur Verfügung gestellt, wenn der Cluster erstellt wurde.

Im folgende Beispiel wird eine GET-Anforderung gegen die REST-API vornehmen cURL verwendet. Ersetzen Sie mit der Administratorkennworts für den Cluster __Kennwort__ ein. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Die Antwort ist ein JSON-Dokument, das beginnt mit ähnlich wie der folgende Informationen an:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Da dies JSON ist, ist es einfacher, einen JSON-Parser verwenden, für die Arbeit mit den Daten. Im folgende Beispiel wird beispielsweise Jq verwendet, um nur Anzeigen der `health_report` Element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Beispiel: Abrufen Sie, den vollqualifizierten Domänennamen des Cluster-Knoten

Bei der Arbeit mit HDInsight müssen Sie möglicherweise den vollqualifizierten Domänennamen (FULLY) ein Knoten wissen. Sie können einfach den vollqualifizierten Domänennamen für die verschiedenen Knoten in den Cluster mithilfe der folgenden abrufen:

* __Kopf Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Worker Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Beachten Sie, dass jedes dieser Beispiele auf dem gleiche Muster folgen:

1. Abfrage eine Komponente, die wir wissen, die auf diesen Knoten ausgeführt wird.
2. Abrufen der `host_name` Elemente, die den vollqualifizierten Domänennamen für diese Knoten enthalten.

Die `host_components` Element aus dem Rückgabetyp Dokument enthält mehrere Elemente. Verwenden von `.host_components[]`, und jedes Element durchlaufen, und ziehen Sie den Wert von bestimmten Pfad wird dann einen Pfad innerhalb des Elements anzugeben. Wenn Sie nur einen Wert, wie z. B. den ersten Eintrag der FQDN, können Elemente als eine Auflistung zurückgeben und wählen Sie dann einen bestimmten Eintrag:

    jq '[.host_components[].HostRoles.host_name][0]'

Dies gibt den ersten vollqualifizierten Domänennamen in der Websitesammlung.

##<a name="example-get-the-default-storage-account-and-container"></a>Beispiel: Abrufen der Speicher Standardkonto und im container

Beim Erstellen eines HDInsight Clusters müssen Sie ein Azure-Speicher-Konto und eines Containers Blob als Standardspeicher für den Cluster verwenden. Ambari können Sie diese Informationen abrufen, nachdem der Cluster erstellt wurde. Schreiben von Daten direkt auf den Container, beispielsweise wenn Sie programmgesteuert möchten.

Vor wird der WASB URI der Cluster Standard-Speicher abrufen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Dies gibt die erste Konfiguration angewendet werden, auf dem Server (`service_config_version=1`,) der diese Informationen enthält. Wenn Sie einen Wert, der nach der Clustererstellung geändert wurde abrufen, müssen Sie die Liste der Konfigurationsversionen und die neuesten eine abzurufen.

Dies ergibt den Wert ähnlich wie im folgenden Beispiel, wobei __CONTAINER__ der standardmäßige Container und __Kontoname__ ist der Azure-Speicher Kontoname:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Sie können diese Informationen dann hochladen oder Herunterladen von Daten aus dem Container mit der [Azure CLI](../xplat-cli-install.md) verwenden.

1. Abrufen der Ressourcengruppe für den Speicher-Konto an. Ersetzen Sie __Kontoname__ mit dem Speicher Kontonamen aus Ambari abgerufen werden:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Dies gibt den Gruppennamen Ressourcen für das Konto aus.
    
    > [AZURE.NOTE] Wenn nichts aus dieser Befehl zurückgegeben wird, müssen Sie die Azure CLI in Azure Ressourcenmanager Modus ändern, und führen den Befehl erneut. Um zur Azure Ressourcenmanager Modus wechseln, verwenden Sie den folgenden Befehl aus:
    >
    > `azure config mode arm`
    
2. Erhalten Sie die Taste für das Speicher-Konto an. Ersetzen Sie die Ressourcengruppe aus dem vorherigen Schritt __GRUPPENNAME__ ein. Ersetzen Sie mit dem Speicher Kontonamen __Kontoname__ :

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    In diesem Beispiel gibt den Primärschlüssel für das Konto an.
    
3. Verwenden Sie den Befehl hochladen, um eine Datei im Container zu speichern:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Ersetzen Sie __Kontoname__ mit dem Speicher Kontonamen ein. Ersetzen Sie __ACCOUNTKEY__ durch die Taste zuvor abgerufen. __DATEIPFAD__ wird den Pfad zu der Datei, die Sie hochladen, solange __BLOBPATH__ den Pfad im Container ist, möchten.

    Beispielsweise, wenn Sie die Datei in HDInsight wasbs://example/data/filename.txt angezeigt werden soll, klicken Sie dann __BLOBPATH__ wäre `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Beispiel: Update Ambari Konfiguration

1. Abrufen der aktuellen Konfigurations der Ambari als "gewünschte Konfiguration" gespeichert:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    In diesem Beispiel gibt eine JSON-Dokument mit der aktuellen Konfiguration (identifiziert durch den Wert der _Kategorie_ ) für die Komponenten im Cluster installiert. Im folgende Beispiel wird ein Ausschnitt aus den Daten aus einem Spark Clustertyp zurückgegeben.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    In dieser Liste, müssen Sie den Namen der Komponente kopieren (z. B. __Spark\_Thrift\_Sparkconf__ und den Wert für die __Kategorie__ .
    
2. Rufen Sie die Konfiguration für die Komponente und eine Kategorie mit dem folgenden Befehl ab. Ersetzen Sie __Spark-Thrift-Sparkconf__ und __ANFÄNGLICHEN__ mit der Komponente und die Kategorie, die die Konfiguration für abgerufen werden sollen.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Curl Ruft das JSON-Dokument, und klicken Sie dann Jq wird zum Ändern der Daten verwendet, um eine Vorlage zu erstellen. Die Vorlage wird dann zum Hinzufügen/Ändern der Konfigurationswerte. Speziell geschieht Folgendes:
    
    * Erstellt einen eindeutigen Wert, enthält die Zeichenfolge "Version" und das Datum, das im __Newtag__gespeichert ist.
    * Erstellt ein Dokument aus, für die neue gewünschte Konfiguration.
    * Ruft den Inhalt der `.items[]` Arrays ab, und fügt es unterhalb des __Desired_config__ -Elements hinzu.
    * Löscht die __Href__, __Version__, und __Config__ Elemente, wie diese Elemente und übermitteln Sie eine neue Konfiguration nicht erforderlich sind.
    * Fügt ein neues Element der __Kategorie__ und dessen Wert auf __Version ###__festgelegt. Der numerische Teil basiert auf dem aktuellen Datum. Jede Konfiguration müssen ein eindeutiges Kennzeichen.
    
    Schließlich werden die Daten in das Dokument __newconfig.json__ gespeichert. Die Struktur des Dokuments sollte ähnlich wie im folgenden Beispiel wird angezeigt:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Öffnen Sie die Werte für __newconfig.json__ -Dokument und ändern/hinzufügen in das __Properties__ -Objekt. Im folgende Beispiel ändert sich den Wert von __"spark.yarn.am.memory"__ von __"1 g"__ in __"3 g"__ und, und fügt ein neues Element für __"spark.kryoserializer.buffer.max"__ mit dem Wert __"256 m"__hinzu.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Speichern Sie die Datei, nachdem Sie alle Änderungen vorgenommen werden.

4. Verwenden Sie den folgenden Befehl, und übermitteln Sie die aktualisierte Ambari-Konfiguration.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Dieser Befehl leitet den Inhalt der Datei __newconfig.json__ auf die Anforderung Curl, die sie zum Cluster als die gewünschte neue Konfiguration sendet. Die Anfrage cURL gibt eine JSON-Dokument. Das Element __VersionTag__ in diesem Dokument sollte die Version, die Sie auch ursprünglich eingereicht übereinstimmen, und das Objekt __Konfigurationen__ wird die geänderte Konfiguration gewünschte enthalten.

###<a name="example-restart-a-service-component"></a>Beispiel: Starten einer Service-Komponente

Zu diesem Zeitpunkt, wenn Sie sich im Web Ambari Benutzeroberfläche ansehen, wird der Dienst Spark darauf hinzuweisen, dass es muss neu gestartet werden, bevor die neue Konfiguration wirksam werden. Gehen Sie folgendermaßen vor, um den Dienst neu starten.

1. Anhand der folgenden um Wartungsmodus für den Dienst Spark zu aktivieren:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Dieser Befehl sendet ein JSON-Dokument auf dem Server (enthalten, der `echo` -Anweisung) wird die Wartungsmodus aktiviert.
    Sie können überprüfen, ob der Dienst jetzt im Wartungsmodus mithilfe der folgenden Anforderung ist:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Dadurch wird der Wert zurückgegeben `"ON"`.

3. Verwenden Sie folgende weiter, um den Dienst zu deaktivieren:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Dieser Befehl gibt eine Antwort ähnlich wie der folgende.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Die `href` dieser URI zurückgegebene Wert ist die interne IP-Adresse des Clusterknotens verwenden. Wenn sie von außerhalb der Cluster verwenden möchten, ersetzen Sie im Bereich '10.0.0.18:8080' mit den vollqualifizierten Domänennamen des Cluster aus. Mit dem folgende Befehl wird beispielsweise den Status der Anfrage abgerufen.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Wenn dieser Wert gibt `"COMPLETED"` und klicken Sie dann die Anforderung abgeschlossen ist.

4. Nach Abschluss der vorherige Anforderung anhand der folgenden den Dienst starten.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Sobald der Dienst neu gestartet wurde, kann die neuen Konfigurations-Einstellungen.

5. Schließlich anhand der folgenden Wartungsmodus deaktivieren.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Nächste Schritte

Umfassende Informationen zu den REST-API finden Sie unter [Ambari API Bezug Version 1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Einige Ambari-Funktionen sind deaktiviert, wie es von HDInsight Cloud-Dienst verwaltet wird; beispielsweise hinzufügen oder Entfernen von Hosts aus dem Cluster oder neue Dienste hinzufügen.

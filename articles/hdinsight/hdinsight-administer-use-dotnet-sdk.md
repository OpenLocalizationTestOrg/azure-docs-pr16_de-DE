<properties
    pageTitle="Verwalten von Hadoop Cluster in HDInsight mit .NET SDK | Microsoft Azure"
    description="Informationen Sie zum Ausführen von Verwaltungsaufgaben für die Hadoop Cluster in HDInsight mit HDInsight .NET SDK."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Verwalten von Hadoop Cluster in HDInsight mithilfe von .NET SDK

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informationen Sie zum Verwalten von HDInsight Cluster mit [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).


**Erforderliche Komponenten**

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).


##<a name="connect-to-azure-hdinsight"></a>Verbinden mit Azure HDInsight

Sie benötigen die folgenden Nuget-Paketen:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

Im folgenden Beispiel wird gezeigt, wie Verbindung zur Azure, bevor Sie unter Ihrem Abonnement Azure HDInsight Cluster verwalten können.

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
            /// <param name="TenantId">The AAD tenant ID</param>
            /// <param name="ClientId">The AAD client ID</param>
            /// <param name="SubscriptionId">The Azure subscription ID</param>
            /// <returns></returns>
            static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                    ClientId, 
                    new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                    PromptBehavior.Always, 
                    UserIdentifier.AnyUser);
                return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
            }
            /// <summary>
            /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
            /// </summary>
            /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
            /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
            /// <param name="authToken">An authentication token for your Azure subscription</param>
            static void EnableHDInsight(TokenCloudCredentials authToken)
            {
                // Create a client for the Resource manager and set the subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register the HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

Eine Aufforderung wird angezeigt werden, wenn Sie dieses Programm ausführen.  Wenn Sie nicht gefragt werden, ob möchten, finden Sie unter [Erstellen nicht interaktiven Authentifizierung .NET HDInsight Applikationen](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

##<a name="create-clusters"></a>Cluster erstellen

Finden Sie unter [Erstellen von Linux-basierten Cluster in HDInsight mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

##<a name="list-clusters"></a>Liste Cluster

Im folgenden Codeausschnitt listet Cluster und einige Eigenschaften:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

##<a name="delete-clusters"></a>Cluster löschen

Verwenden Sie den folgenden Codeausschnitt zu einen Cluster synchron oder asynchronen löschen: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
            
##<a name="scale-clusters"></a>Maßstab Cluster
Cluster Skalierung Feature ermöglicht Ihnen, ändern die Anzahl der Worker-Knoten verwendet, die für einen Cluster, der in Azure HDInsight ausgeführt wird, ohne den Cluster erneut erstellen.

>[AZURE.NOTE] Nur mit HDInsight Version 3.1.3 Cluster oder höher werden unterstützt. Wenn Sie die Version von Ihren Cluster kennen, können Sie auf die Seite Eigenschaften überprüfen.  Finden Sie unter [Liste und anzeigen Cluster](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Die Auswirkung zum Ändern der Anzahl der Datenknoten für jede Art von Cluster von HDInsight unterstützt:

- Hadoop

    Sie können nahtlos die Anzahl der Worker-Knoten in einem Cluster Hadoop erhöhen, die ohne zu beeinträchtigen Ausstehend oder laufenden Aufträge ausgeführt wird. Neue Aufträge können auch gesendet werden, während der Vorgang ausgeführt wird. Fehler bei einer Skalierung sind ordnungsgemäß behandelt, sodass der Cluster funktionsfähig immer offen steht.

    Wenn ein Cluster Hadoop nach unten durch Verringern der Anzahl der Datenknoten skaliert ist, werden einige der Dienste im Cluster neu gestartet. Dadurch alle laufenden und anstehende Aufträge nach Abschluss des Vorgangs Skalierung fehlschlägt. Sie können, jedoch die Aufträge erneut, nachdem der Vorgang abgeschlossen ist.

- HBase

    Sie können nahtlos hinzufügen oder Entfernen von Knoten zum Cluster HBase während der Ausführung. Innerhalb weniger Minuten der Abschluss der Skalierung Operation sind automatisch Landes-/ Server ausgelastet. Sie können jedoch auch manuell die regionalen Server Saldo, nach in der Headnode der Cluster Protokollierung, und führen die folgenden Befehle aus, ein Eingabeaufforderungsfenster:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Storm

    Nahtloses können Sie hinzufügen oder Entfernen von Datenknoten zum Cluster Storm während der Ausführung. Müssen Sie jedoch nach einer erfolgreichen Abschluss des Vorgangs Skalierung, wird der Suchtopologie neu zu verteilen.

    Qualifikationsprofilen kann auf zwei Arten erreicht werden:

    * Storm Web-Benutzeroberfläche
    * Tool line Interface (CLI)

    Lizenzinformationen finden Sie in der [Dokumentation Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) Weitere Details.

    Das Storm Web-Benutzeroberfläche steht im Cluster HDInsight zur Verfügung:

    ![Hdinsight Storm skalieren rebalance](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Hier ist ein Beispiel, wie Sie mithilfe des Befehls CLI um der Suchtopologie Storm neu zu verteilen:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Der folgende Codeausschnitt veranschaulicht, wie einen Cluster synchron oder asynchronen Größe:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    

##<a name="grantrevoke-access"></a>Zugriff gewähren/widerrufen

HDInsight Cluster stehen die folgenden HTTP-Webdienste (alle der folgenden Dienste verwenden REST-Endpunkten):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Standardmäßig werden diese Dienste für den Zugriff erteilt. Sie können widerrufen/den Zugriff gewähren. Um zu widerrufen:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

So gewähren:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


>[AZURE.NOTE] Durch den Zugriff gewähren/aufheben, werden Sie Cluster Benutzername und Kennwort zurücksetzen.

Dies kann auch über das Portal erfolgen. [Verwalten von HDInsight mithilfe der Azure-Portal]finden Sie unter[hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>HTTP-Benutzeranmeldeinformationen aktualisieren

Es ist dasselbe Verfahren, das als [Zugriff gewähren/widerrufen HTTP](#grant/revoke-access). Wenn der Cluster den HTTP-Zugriff gewährt wurde, müssen Sie zuerst widerrufen.  Und gewähren Sie den Zugriff mit neuen HTTP-Benutzeranmeldeinformationen.


##<a name="find-the-default-storage-account"></a>Suchen des Standardkontos-Speicher

Der folgende Codeausschnitt veranschaulicht, wie Name des Standardkontos Speicher und die Verwendung der standardmäßigen Speicher kontoschlüssel für einen Cluster.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


##<a name="submit-jobs"></a>Aufträge senden

**MapReduce Aufträge senden**

Finden Sie unter [Ausführen Hadoop MapReduce Beispiele in HDInsight](hdinsight-hadoop-run-samples-linux.md).

**Struktur Aufträge senden** 

Finden Sie unter [Ausführen Struktur Abfragen mit .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**Schwein Aufträge senden**

Finden Sie unter [Ausführen Schwein Aufträge .NET SDK verwenden](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**Sqoop Aufträge senden**

Finden Sie unter [Verwenden von Sqoop mit HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**Oozie Aufträge senden**

Finden Sie unter [Verwenden von Oozie mit Hadoop definieren und Ausführen eines Workflows in HDInsight](hdinsight-use-oozie-linux-mac.md).

##<a name="upload-data-to-azure-blob-storage"></a>Hochladen von Daten in Azure Blob-Speicher
[Hochladen von Daten mit HDInsight]finden Sie unter[hdinsight-upload-data].


## <a name="see-also"></a>Siehe auch
* [Dokumentation zur HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)
* [Verwalten von HDInsight mithilfe der Azure-Portal][hdinsight-admin-portal]
* [Verwalten einer Line Schnittstelle HDInsight][hdinsight-admin-cli]
* [HDInsight Cluster erstellen][hdinsight-provision]
* [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md



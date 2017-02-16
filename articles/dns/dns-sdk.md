<properties 
   pageTitle="Erstellen von DNS-Zonen und Aufzeichnen von Gruppen in Azure DNS mit dem .NET SDK | Microsoft Azure" 
   description="Zum Erstellen von DNS-Zonen und Datensatz legt in Azure DNS mithilfe von .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Erstellen von DNS-Zonen und Datensätze mit dem .NET SDK

Sie können zum Erstellen, löschen oder Aktualisieren von DNS-Zonen, Datensätze und Datensätze mithilfe von DNS SDK mit .NET DNS Management Bibliothek Vorgänge automatisieren. Ein Visual Studio Gesamtprojekt steht [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Erstellen eines Dienstkontos Hauptbenutzer

In der Regel ist den programmgesteuerten Zugriff auf Azure Ressourcen über eigene Benutzeranmeldeinformationen, statt ein spezielles Konto erteilt. Diese dedizierten Konten werden als ' Hauptbenutzer ' Dienstkonten bezeichnet. Um das Projekt Azure DNS-SDK-Beispiel zu verwenden, müssen Sie zuerst ein Hauptbenutzer Dienstkonto erstellen, und weisen sie die erforderlichen Berechtigungen.

1. Führen Sie die [folgenden Schritte](../resource-group-authenticate-service-principal.md) , um ein Hauptbenutzer Dienstkonto erstellen (Stichprobe Projekt Azure DNS-SDK setzt voraus Authentifizierung).

2. Erstellen eine Ressourcengruppe ([hier wie](../azure-portal/resource-group-portal.md)).

3. Verwenden von Azure RBAC zu den wichtigsten Dienstkontos ' DNS Zone' Teilnehmerberechtigungen der Ressourcengruppe erteilen ([hier wie](../active-directory/role-based-access-control-configure.md).)

4. Wenn das Projekt Azure DNS-SDK-Beispiel verwenden, bearbeiten Sie die Datei "program.cs" wie folgt:
    * Fügen Sie die richtigen Werte für die TenantId, ClientId (auch bekannt als Konto-ID), geheim (Hauptbenutzer Kontokennwort Service) und SubscriptionId wie in Schritt 1 verwendet.
    * Geben Sie die in Schritt 2 ausgewählten Gruppe Ressourcenname aus.
    * Geben Sie einen DNS Zonennamen Ihrer Wahl.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-Pakete und Namespacedeklarationen

Um Azure DNS .NET SDK verwenden zu können, müssen Sie die **Azure DNS Management Bibliothek** NuGet-Paket und andere erforderliche Azure Pakete installieren.
 
1. Öffnen Sie in **Visual Studio**ein Projekt oder ein neues Projekt aus. 

2. Klicken Sie auf **Extras** **>** **NuGet-Paket-Manager** **>** **Lösung... NuGet-Pakete verwalten**. 

3. Klicken Sie auf **Durchsuchen**, und aktivieren Sie das Kontrollkästchen **einschließen Vorabversion** Geben Sie **Microsoft.Azure.Management.Dns** in das Suchfeld ein.

4. Wählen Sie das Paket aus, und klicken Sie auf **Installieren** , um sie zu Ihrem Projekt Visual Studio hinzuzufügen.
 
5. Wiederholen Sie den Vorgang aus, um auch die folgenden Pakete installieren: **Microsoft.Rest.ClientRuntime.Azure.Authentication** und **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Fügen Sie Namespacedeklarationen

Fügen Sie die folgenden Namespacedeklarationen

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Initialisierung des DNS-Verwaltung Clients

Die *DnsManagementClient* enthält die Methoden und Eigenschaften für die Verwaltung von DNS-Zonen und Recordsets erforderlich sind.  Mit dem folgende Code für die wichtigsten Dienstkontos protokolliert und erstellt ein DnsManagementClient-Objekt.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Erstellen oder Aktualisieren einer DNS-zone

Zum Erstellen einer DNS-Zone ist zuerst ein Objekt "Zone" erstellt, um die DNS-Zone Parameter enthalten. Da DNS-Zonen nicht in einen bestimmten Bereich verknüpft sind, wird die Position auf 'global' festgelegt. In diesem Beispiel wird eine [Azure Ressourcenmanager 'Kategorie'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) auch zur Zone hinzugefügt.

Wenn tatsächlich erstellen oder aktualisieren die Zone in Azure DNS, wird das Zone Objekt, enthält die Zone Parameter an die Methode *DnsManagementClient.Zones.CreateOrUpdateAsyc* übergeben.

>[AZURE.NOTE] DnsManagementClient unterstützt drei Modi des Vorgangs: synchroner ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), oder asynchrone mit Zugriff auf die HTTP-Antwort ('CreateOrUpdateWithHttpMessagesAsync').  Sie können eine von drei Modi, je nach Bedarf der Anwendung auswählen.

Azure DNS unterstützt optimistische gleichzeitiger Zugriff, [Etags](dns-getstarted-create-dnszone.md)bezeichnet. In diesem Beispiel angeben "*" für die 'If-keine-Match' erfahren Kopfzeile Azure DNS zum Erstellen einer DNS Zone, wenn Sie noch nicht vorhanden ist.  Der Anruf schlägt fehl, wenn eine Zone mit dem angegebenen Namen in der angegebenen Ressourcengruppe bereits vorhanden ist.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Erstellen von DNS-Datensätze und Datensätze

DNS-Einträge werden als Datensatz Satz verwaltet. Eine Datensatzgruppe ist eine Gruppe von Datensätzen mit demselben Namen und Datensatztyp innerhalb einer Zone an.  Der Datensatz SetName ist relativ zu dem Zonennamen, nicht der vollqualifizierte DNS-Name.

Zum Erstellen oder Aktualisieren eines Datensatzes festlegen möchten, ein "RecordSet" erstellt und an *DnsManagementClient.RecordSets.CreateOrUpdateAsync*übergebene Parameterobjekt. Wie mit DNS-Zonen, es drei Modi des Vorgangs gibt: synchroner ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), oder asynchrone mit Zugriff auf die HTTP-Antwort ('CreateOrUpdateWithHttpMessagesAsync').

Wie bei der DNS-Zonen, gehören Vorgänge Datensätze Unterstützung für die vollständige Parallelität.  In diesem Beispiel da weder 'If-Match' noch 'If-keine-Match' angegeben sind, Datensatzgruppe immer entsteht.  Dieser Anruf überschreibt alle vorhandenen Datensatz mit den gleichen Namen und dem Datensatztyp in diese DNS Zone festlegen.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zonen abrufen und Datensätze

Die Methoden *DnsManagementClient.Zones.Get* und *DnsManagementClient.RecordSets.Get* Abrufen einzelner Zonen und aufzeichnen Sätze. RecordSets identifizierten deren Typ, Name und der Zone und Ressourcen Gruppe, die, der Sie in vorhanden sind. Zonen identifizierten seinen Namen und die Ressourcengruppe aus, die, der Sie in vorhanden sind.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Aktualisieren einer vorhandenen Datensatz festlegen

Klicken Sie dann übermitteln Sie die Änderung, um einen vorhandenen Datensatz Satz von DNS-Einträge aktualisieren, Abrufen der Datensatzgruppe zunächst den Inhalt der Anzahl von Datensätzen aktualisieren.  In diesem Beispiel geben wir das Etag aus dem Satz abgerufenen Datensatz im 'If-Match' Parameter aus. Der Anruf schlägt fehl, wenn ein gleichzeitiger Vorgang den Eintrag festlegen in der Zwischenzeit geändert hat.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Liste Zonen und Datensätze

In der Liste der Zonen verwenden Sie die Methoden *DnsManagementClient.Zones.List...* , die unterstützen, wobei entweder alle Zonen in einer bestimmten Ressourcengruppe oder alle Zonen in einem angegebenen Azure-Abonnement (über Ressourcengruppen.) In der Liste Datensätze *... DnsManagementClient.RecordSets.List* Methoden, mit denen unterstützen entweder Auflisten aller Datensätze in einer bestimmten Zone oder nur diese Datensätze eines bestimmten Typs verwenden.

Beachten Sie bei Zonen auflisten und Datensatz-Datensätze, die sich ergibt paginiert werden können.  Im folgenden Beispiel wird gezeigt, wie die Seiten der Ergebnisse durchlaufen. (Eine künstlich kleine Seitengröße von "2" wird verwendet, um die Seitennavigation erzwingen; in der Praxis sollten für diesen Parameter ausgelassen werden und die Seite Standardgröße verwendet.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Nächste Schritte

Herunterladen von [Azure DNS .NET SDK Stichprobe Project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), wozu auch weitere Beispiele zum Verwenden von Azure DNS .NET SDK, einschließlich Beispiele für andere DNS-Eintragstypen.

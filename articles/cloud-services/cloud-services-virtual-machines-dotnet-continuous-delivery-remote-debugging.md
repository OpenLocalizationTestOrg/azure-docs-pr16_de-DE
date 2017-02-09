<properties
    pageTitle="Aktivieren des remote Debuggen mit fortlaufender Übermittlung | Microsoft Azure"
    description="Erfahren Sie, wie bei Verwendung von der kontinuierlichen Bereitstellung für die Bereitstellung auf Azure remote Debuggen zu aktivieren"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Bei Verwendung von der kontinuierlichen Bereitstellung Azure veröffentlichen remote Debuggen aktivieren

Sie können die remote Debuggen in Azure, für Cloud Services oder virtuellen Computern, wenn Sie [kontinuierlichen Bereitstellung](cloud-services-dotnet-continuous-delivery.md) verwenden, um in Azure veröffentlichen, mit folgenden Schritten aktivieren.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Aktivieren des remote Debuggen für Cloud-Dienste

1. Richten Sie auf dem Build-Agent die ursprüngliche Umgebung für Azure wie [Line erstellen für Azure](http://msdn.microsoft.com/library/hh535755.aspx)umrandet.
2. Da die Laufzeit Debuggen Remoteprozeduraufruf (msvsmon.exe) für das Paket erforderlich ist, installieren Sie die [Remote-Tools für Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (oder die [Remote-Tools für Microsoft Visual Studio 2013 Update 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) , wenn Sie Visual Studio 2013 verwenden). Alternativ können Sie die remote Debugbinärdateien aus einem System kopieren, die Visual Studio installiert wurde.
3. Erstellen Sie ein Zertifikat aus, wie in der [Übersicht über Zertifikate für Azure-Cloud-Dienste](cloud-services-certs-create.md)beschrieben. Behalten Sie der PFX und RDP Fingerabdruck des Zertifikats bei und Hochladen Sie das Zertifikat zum Ziel Cloud-Dienst.
4. Verwenden Sie die folgenden Optionen in die Befehlszeile MSBuild zum Erstellen und Verpacken mit remote zu Debuggen aktiviert. (Ersetzen Sie ist-Pfade auf Ihre System und Project-Dateien für die Elemente Winkel gekennzeichnet).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`Dies ist der Pfad zum Ordner msvsmon.exe in den Remote-Tools für Visual Studio enthält.
    `RemoteDebuggerConnectorVersion`ist die Azure SDK Version in der Cloud-Dienst. Sie sollte auch die mit Visual Studio installierte Version übereinstimmen.

5. Veröffentlichen Sie in der Zielliste Cloud-Service mithilfe der im vorherigen Schritt generierten Paket und .cscfg-Datei ein.
6. Importieren Sie das Zertifikat (PFX-Datei) auf dem Computer, der auf Visual Studio mit Azure SDK für .NET installiert ist. Stellen Sie sicher zu importieren, die `CurrentUser\My` Zertifikat Store, andernfalls Anfügen an den Debugger in Visual Studio schlägt fehl.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Aktivieren des remote Debuggen für virtuellen Computern

1. Erstellen einer Azure-virtuellen Computern an. Finden Sie unter [Erstellen eines virtuellen Computern unter WindowsServer](../virtual-machines/virtual-machines-windows-hero-tutorial.md) oder [Erstellen und Verwalten von Azure-virtuellen Computern in Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Klicken Sie auf die [Azure klassischen Portalseite](http://go.microsoft.com/fwlink/p/?LinkID=269851)zeigen Sie des virtuellen Computers-Dashboard, um die virtuellen Computern **RDP FINGERABDRUCK des Zertifikats**finden Sie unter an Dieser Wert wird verwendet, für die `ServerThumbprint` Wert in der Konfiguration der Erweiterung.
3. Erstellen Sie ein Client-Zertifikat unter [Übersicht über Zertifikate für Azure Cloud Services](cloud-services-certs-create.md) beschrieben (beibehalten der PFX und RDP Fingerabdruck des Zertifikats).
4. Installieren von Azure Powershell (Version 0.7.4 oder höher) unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md)beschrieben.
5. Führen Sie das folgende Skript aus, um die Erweiterung RemoteDebug zu aktivieren. Ersetzen Sie die Pfade und Ihre persönlichen Daten durch ein eigenes, wie Ihre Abonnementname, Dienst- und Fingerabdruck aus.

    >[AZURE.NOTE] Dieses Skript ist für Visual Studio 2015 konfiguriert. Wenn Sie Visual Studio 2013 verwenden, Ändern der `$referenceName` und `$extensionName` mithilfe der folgenden Zuordnungen `RemoteDebugVS2013` (anstelle von `RemoteDebugVS2015`).

    <pre>
   Hinzufügen von AzureAccount

    Wählen Sie-AzureSubscription "Mein Abonnement Microsoft"

    $vm = "mytestvm1" Get-AzureVM - ServiceName-Namen "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    Foreach ($endpoint in $endpoints) {hinzufügen-AzureEndpoint - virtuellen Computer $vm-$endpoint nennen. Name - Protokoll Tcp - PublicPort $endpoint. PublicPort - LokalerAnschluss $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1. * ist" $publicConfiguration = "<PublicConfig>< Connector.Enabled > True < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -Version $version ' - PublicConfiguration $publicConfiguration

    Foreach ($extension in $vm. VIRTUELLER COMPUTER. ResourceExtensionReferences) {Wenn (($extension. Verweisname - Eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - und ($extension. Benennen Sie - Eq $extensionName) "– und ($extension. Version - Eq $version)) {$extension. ResourceExtensionParameterValues [0]. Key = 'config.txt' Seitenumbruch}}

    $vm | Update-AzureVM </pre>

6. Importieren Sie das Zertifikat (PFX-Datei) auf dem Computer, der auf Visual Studio mit Azure SDK für .NET installiert ist.

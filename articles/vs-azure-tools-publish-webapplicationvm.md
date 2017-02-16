<properties
   pageTitle="Veröffentlichen von WebApplicationVM | Microsoft Azure"
   description="Erfahren Sie, wie eine Webanwendung mit einem virtuellen Computer bereitgestellt. Dieses Skript erstellt die erforderlichen Ressourcen in Ihr Abonnement Azure, wenn er nicht vorhanden ist."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Veröffentlichen-WebApplicationVM (Windows PowerShell-Skript)

Bereitstellt eine Webanwendung mit einem virtuellen Computer an. Das Skript erstellt die erforderlichen Ressourcen in Ihr Abonnement Azure, wenn er nicht vorhanden ist.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguration

Der Pfad zu der JSON-Konfigurationsdatei, die die Details der Bereitstellung beschrieben.

|Aliase|keine|
|---|---|
|Erforderlich?|WAHR|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="subscriptionname"></a>SubscriptionName

Der Name des Azure-Abonnements in dem Erstellen des virtuellen Computers werden soll.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|Verwendet das erste Abonnement in der Abonnementdatei|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="webdeploypackage"></a>WebDeployPackage

Der Pfad für das Webbereitstellungspaket virtuellen Computer veröffentlichen. Sie können dieses Paket erstellen, indem Sie mithilfe des Veröffentlichen-Assistenten in Visual Studio. Finden Sie unter [wie: Erstellen einer Web-Bereitstellungspaket in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="allowuntrusted"></a>AllowUntrusted

Ist der Wert true, können Sie die Verwendung von Zertifikaten, die von einer vertrauenswürdigen Stammzertifizierungsstelle signiert werden nicht an.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="vmpassword"></a>VMPassword

Die Anmeldeinformationen für das Konto des virtuellen Computers. Beispiel: - VMPassword @{Name = "Administrator"; Kennwort = "Kennwort"}

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Die Anmeldeinformationen für die in Azure SQL-Datenbank. Beispiel: - DatabaseServerPassword @{Name = "Administrator"; Kennwort = "Kennwort"}

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Wenn der Wert true, Nachrichten Drucken aus dem Skript Ausgabe Streams.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="remarks"></a>Hinweise

So verwenden Sie das Skript zum Erstellen eine vollständige Erklärung Entwicklung und Test-Umgebungen, finden Sie unter [Mithilfe von Windows PowerShell-Skripts zu veröffentlichen, um die Entwicklung und Test-Umgebungen](vs-azure-tools-publishing-using-powershell-scripts.md).

Die JSON-Konfigurationsdatei gibt an, die Details des bereitgestellt werden soll. Sie enthält die Informationen, die Sie beim Erstellen des Projekts, wie der Name, Zugehörigkeit Gruppe, virtuelle Festplattenabbild und Größe des virtuellen Computers angegeben haben. Auch die Endpunkte des virtuellen Computers, die Datenbanken nicht bereitstellen, enthält, sofern vorhanden, und die web-Bereitstellungsparameter. Der folgende Code zeigt eine Beispiel für JSON-Konfigurationsdatei:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Sie können die JSON-Konfigurationsdatei, um ändern, was nach der Bereitstellung ist bearbeiten. Ein virtueller Computer und einen Cloud-Service erforderlich sind, aber im Abschnitt Datenbank ist optional.

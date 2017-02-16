<properties
   pageTitle="(Windows PowerShell-Skript) veröffentlichen-WebApplicationWebSite | Microsoft Azure"
   description="Erfahren Sie, wie ein Webprojekt auf eine Website zu Azure veröffentlichen. Dieses Skript erstellt die erforderlichen Ressourcen in Ihr Abonnement Azure, wenn er nicht vorhanden ist."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Veröffentlichen-WebApplicationWebSite (Windows PowerShell-Skript)

##<a name="syntax"></a>Syntax

Ein Webprojekt veröffentlicht in ein Azure-Website. Das Skript erstellt die erforderlichen Ressourcen in Ihr Abonnement Azure, wenn er nicht vorhanden ist.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguration

Der Pfad zu der JSON-Konfigurationsdatei, die die Details der Bereitstellung beschrieben.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|WAHR|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="subscriptionname"></a>SubscriptionName

Der Name des Azure-Abonnements, die für die Website in erstellen möchten.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="webdeploypackage"></a>WebDeployPackage

Der Pfad für das Webbereitstellungspaket auf der Website veröffentlichen. Sie können dieses Paket erstellen, indem Sie mithilfe des Veröffentlichen-Assistenten in Visual Studio. Weitere Informationen finden Sie unter [Erste Schritte mit Azure-Cloud-Dienste und ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Der Benutzername und Kennwort für die SQL-Datenbank in Azure.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Wenn der Wert true, Nachrichten Drucken aus dem Skript Ausgabe Streams.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Eingaben Verkaufspipeline vorgenommen?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="remarks"></a>Hinweise

So verwenden Sie das Skript zum Erstellen eine vollständige Erklärung Entwicklung und Test-Umgebungen, finden Sie unter [Mithilfe von Windows PowerShell-Skripts zu veröffentlichen, um die Entwicklung und Test-Umgebungen](vs-azure-tools-publishing-using-powershell-scripts.md).

Die JSON-Konfigurationsdatei gibt an, die Details des bereitgestellt werden soll. Sie enthält die Informationen, die Sie angegeben haben, wenn Sie das Projekt, z. B. den Namen und Benutzernamen für die Website erstellt haben. Darüber hinaus die Datenbank bereitstellen, sofern vorhanden. Der folgende Code zeigt eine Beispiel für JSON-Konfigurationsdatei:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Sie können die JSON-Konfigurationsdatei zum Ändern, was bereitgestellt wird bearbeiten. Ein Abschnitt für die WebSite ist erforderlich, aber der Datenbank Abschnitt ist optional.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Veröffentlichen-WebApplicationVM (Windows PowerShell-Skript)](vs-azure-tools-publish-webapplicationvm.md)

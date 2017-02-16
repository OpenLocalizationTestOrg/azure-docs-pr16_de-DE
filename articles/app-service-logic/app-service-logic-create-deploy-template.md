<properties
   pageTitle="Erstellen eine Vorlage für Logik app Bereitstellung | Microsoft Azure"
   description="Erfahren Sie, wie Logik app Bereitstellungsvorlage erstellen und verwenden sie für die Verwaltung von release"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Erstellen einer Vorlage für Logik app Bereitstellung

Nachdem Sie eine app Logik erstellt wurde, empfiehlt es sich, ihn als Ressourcenmanager Azure-Vorlage zu erstellen. Auf diese Weise können Sie ganz einfach bereitstellen die app Logik jeder Umgebung und Ressourcengruppe Sie es müssten. Für eine Einführung in Ressourcenmanager Vorlagen unbedingt den Artikeln auf [Azure Ressourcenmanager Vorlagen erstellen](../resource-group-authoring-templates.md) und [Bereitstellen von Ressourcen mithilfe von Azure Ressourcenmanager Vorlagen](../resource-group-template-deploy.md)Auschecken.

## <a name="logic-app-deployment-template"></a>Vorlage für die Bereitstellung von Logik-app

Eine app Logik besteht aus drei Hauptkomponenten:

* **Logik app Ressource**. Diese Ressource enthält Informationen zu Aspekten wie Preise Plan, den Ort und die Workflowdefinition.
* **Workflowdefinition**. Dies ist, was in Codeansicht angezeigt wird. Er enthält die Definition der Schritte des Ablaufs und wie die-Engine ausgeführt werden soll. Dies ist die `definition` Eigenschaft der Logik app Ressource.
* **Verbindungen**. Hierbei handelt es sich um separate Ressourcen, die Metadaten für alle Verbinder Verbindungen, wie eine Verbindungszeichenfolge und eine Access-Token sicher zu speichern. Sie diese Elemente in einer app Logik in Bezug auf die `parameters` Abschnitt der Logik app Ressource.

Alle diese Stück für vorhandene Logik apps können Sie mithilfe von Tools wie [Azure-Explorers](http://resources.azure.com)anzeigen.

Um eine Vorlage für eine app Logik zur Verwendung mit Ressourcen Gruppe Bereitstellungen zu machen, müssen Sie die Ressourcen definieren und Parametrisieren je nach Bedarf. Beispielsweise, wenn Sie eine Entwicklung, testen und Herstellung Umgebung bereitstellen, werden Sie wahrscheinlich andere Verbindungszeichenfolgen mit einer SQL-Datenbank in jeder Umgebung verwenden möchten. Oder möglicherweise möchten Sie in anderen Abonnements oder Ressourcengruppen bereitstellen.  

## <a name="create-a-logic-app-deployment-template"></a>Erstellen einer Vorlage für Logik app Bereitstellung

Die einfachste Möglichkeit, eine gültige Logik app Bereitstellungsvorlage haben besteht darin, die [Visual Studio-Tools für Logik Apps](./app-service-logic-deploy-from-vs.md)zu verwenden.  Visual Studio-Tools gültige Bereitstellungsvorlage generieren, die über einen Abonnement oder Speicherort verwendet werden kann.

Ein paar andere Tools können Ihnen helfen, wie Sie eine Logik app Bereitstellungsvorlage erstellen. Sie können von hand schreiben d. h., mit den Ressourcen, die bereits angesprochenen um Parameter nach Bedarf zu erstellen. Eine weitere Möglichkeit besteht darin, eine [Logik app Vorlage Ersteller](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-Modul verwenden. Diese Open Source-Modul wertet zuerst die app Logik und alle Verbindungen, dass er verwendet, und klicken Sie dann Vorlagenressourcen mit den notwendigen Parametern für die Bereitstellung generiert. Wenn Sie eine app Logik, die eine Nachricht von einer Warteschlange Azure Service Bus empfängt und fügt Daten in einer SQL Azure-Datenbank verfügen, wird das Tool beispielsweise die gesamte Logik Orchestrierung beibehalten und die SQL und Dienstbus Verbindungszeichenfolgen parametrisieren, so, dass sie bei der Bereitstellung festgelegt werden können.

>[AZURE.NOTE] Verbindungen muss innerhalb der gleichen Ressourcengruppe als die app Logik.

### <a name="install-the-logic-app-template-powershell-module"></a>Das Logik app Vorlage PowerShell-Modul installieren

Die einfachste Möglichkeit zum Installieren des Moduls wird über die [PowerShell-Katalog](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)mithilfe des Befehls `Install-Module -Name LogicAppTemplate`.  

Sie können auch manuell das PowerShell-Modul installieren:

1. Laden Sie die neueste Version des [Logik app Vorlage Ersteller](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Extrahieren Sie den Ordner in Ihrem Ordner der PowerShell-Modul (normalerweise `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Für das Modul für die Arbeit mit alle Mandanten und Abonnement Zugriff token, es empfiehlt sich, dass Sie diese mit dem [ARMClient](https://github.com/projectkudu/ARMClient) Befehlszeilentool verwenden.  ARMClient dieser [Blogbeitrag](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) im Detail erläutert werden.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generieren einer app-Vorlage Logik mithilfe der PowerShell

Nachdem PowerShell installiert ist, können Sie eine Vorlage generieren, mit den folgenden Befehl aus:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`ist die Azure-Abonnement-ID. Diese Zeile zuerst erhält eine Access token über ARMClient, und klicken Sie dann leitet sie bis zu der PowerShell-Skript, und erstellt dann die Vorlage in einer JSON-Datei.

## <a name="add-parameters-to-a-logic-app-template"></a>Hinzufügen von Parametern zu einer Logik app-Vorlage

Nachdem Sie Ihre Logik app-Vorlage erstellt haben, können Sie weiterhin Parameter, die Sie benötigen möglicherweise hinzufügen oder ändern. Angenommen, wenn der Definition Ihrer eine Ressource-ID, um eine Azure-Funktion oder verschachtelte Workflows, die Sie in einer einzigen Bereitstellung bereitstellen möchten enthält, können weitere Ressourcen zur Vorlage hinzufügen und IDs parametrisieren, je nach Bedarf. Dasselbe gilt für alle Verweise auf benutzerdefinierte APIs oder Swagger Endpunkte, die Sie für jede Ressourcengruppe erwarten bereitstellen.

## <a name="deploy-a-logic-app-template"></a>Bereitstellen einer Logik app-Vorlage

Sie können Ihre Vorlage über eine beliebige Anzahl von Tools, einschließlich PowerShell, REST-API, Visual Studio Release Management und die Bereitstellung von Azure-Portal Vorlage bereitstellen. In diesem Artikel zum [Bereitstellen von Ressourcen mithilfe von Azure Ressourcenmanager Vorlagen](../resource-group-template-deploy.md) für Weitere Informationen finden Sie unter. Darüber hinaus wird empfohlen, dass Sie eine [Parameterdatei](../resource-group-template-deploy.md#parameter-file) zum Speichern der Werte für den Parameter zu erstellen.

### <a name="authorize-oauth-connections"></a>OAuth Verbindungen zu autorisieren

Nach der Bereitstellung funktioniert die Logik app-durchgehende mit gültigen Parametern. OAuth Verbindungen werden jedoch weiterhin zum Generieren einer gültigen Zugriffstoken autorisiert werden müssen. Sie können dazu die app Logik im Designer öffnen, und klicken Sie dann autorisieren Verbindungen. Oder, wenn Sie automatisieren möchten, können ein Skript auf jede Verbindung OAuth Zustimmung. Es ist ein Beispielskript auf GitHub unter dem Projekt [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) ein.

## <a name="visual-studio-release-management"></a>Verwaltung von Visual Studio-Version

Ein gängiges Szenario für bereitstellen und Verwalten von einer Umgebung besteht darin, ein Tools wie Visual Studio Release Management mit einer Vorlage für Logik app Bereitstellung verwenden. Visual Studio Team Services enthält einen Vorgang [Azure Ressourcengruppe bereitstellen](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) , können Sie einen beliebigen Build hinzufügen oder lassen Sie wieder los Verkaufspipeline. Benötigen Sie einen [Dienst Hauptbenutzer](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) Genehmigung zum Bereitstellen, und klicken Sie dann können Sie die veröffentlichte Version Definition generieren.

1. Release Management um eine neue Definition zu erstellen, wählen Sie im **leeren** , mit einer leeren Definition zu starten.

    ![Erstellen Sie eine neue, leere definition][1]   

1. Wählen Sie alle Ressourcen, die Sie für diese müssen. Dies ist wahrscheinlich die Logik app Vorlage manuell oder als Teil des Buildprozesses generiert wird.
1. Hinzufügen einer Aufgabe **Azure bereitstellen einer Ressource** an.
1. Mit einem [Dienst Hauptbenutzer](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)konfigurieren, und die Vorlage und Vorlagenparameter Dateien verweisen.
1. Fahren Sie mit der Prozessschritte Release für andere-Umgebung, automatisierten Test oder genehmigende Personen je nach Bedarf zu erstellen.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG

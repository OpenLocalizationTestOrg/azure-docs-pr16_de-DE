<properties
   pageTitle="Erstellen über die Befehlszeile für Azure | Microsoft Azure"
   description="Erstellen über die Befehlszeile für Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Erstellen über die Befehlszeile für Azure

## <a name="overview"></a>(Übersicht)

Sie können ein Paket für Azure-Bereitstellung durch Ausführen von MSBuild über die Befehlszeile erstellen. Sie können konfigurieren und Definieren von Builds für das Debuggen, Staging und Herstellung, neben dem automatischen einige des Prozesses erstellen.


## <a name="microsoft-build-engine-msbuild"></a>Erstellen von Microsoft-Engine (MSBuild)

Mithilfe der Microsoft Build Engine (MSBuild), können Sie eigene Produkte Übung Umgebungen erstellen, in dem Visual Studio nicht installiert ist. MSBuild verwendet ein XML-Format für Project-Dateien, die der extensible und von Microsoft vollständig unterstützt. In diesem Dateiformat können Sie beschreiben, welche Elemente werden müssen für eine oder mehrere Plattformen und Konfigurationen erstellt.

Sie können auch MSBuild über die Befehlszeile ausführen, und dieser Ansatz beschrieben. Durch das Festlegen von Eigenschaften über die Befehlszeile können Sie bestimmte Konfigurationen für ein Projekt erstellen. Auf ähnliche Weise können Sie auch die Ziele definieren, die den MSBuild-Befehl erstellt wird. Weitere Informationen zu Befehlszeilenparameter und MSBuild finden Sie unter [MSBuild Befehlszeile verweisen](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installation

Wie im folgenden beschrieben, müssen Sie Software und Tools auf dem Server erstellen installieren, bevor Sie ein Azure-Paket mithilfe von MSBuild erstellen können:

1. Installieren .NET Framework 4 oder höher, enthält die MSBuild.

1. Installieren Sie die [Azure Authoring-Tools](http://go.microsoft.com/fwlink/?LinkId=394615) (Suchen Sie nach MicrosoftAzureAuthoringTools-x64.msi oder MicrosoftAzureAuthoringTools-x86.msi.

1. Installieren der [Azure-Bibliotheken für .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (Suchen Sie nach MicrosoftAzureLibsForNet-x64.msi oder MicrosoftAzureLibs-x86.msi.

1. Kopieren Sie die Datei Microsoft.WebApplication.targets aus einer Visual Studio-Installation auf einem anderen Computer an.

    Die Datei befindet sich im Verzeichnis c:\Programme\Microsoft Dateien (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (für Visual Studio 2012 V11. 0), und Sie sollten in das gleiche Verzeichnis auf dem Server erstellen kopieren.

1. Installieren Sie die [Azure Tools für Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Suchen Sie nach WindowsAzureTools.vs120.exe zum Erstellen von Projekten mit Visual Studio 2013.

## <a name="msbuild-parameters"></a>MSBuild-Parameter

Die einfachste Methode zum Erstellen eines Pakets ist zum Ausführen von MSBuild mit den `/t:Publish` Option. Dieser Befehl erstellt standardmäßig ein Verzeichnis in Bezug auf den Stammordner für das Projekt, z. B. ProjectDir\bin\Configuration\app.publish\. beim Erstellen eines Projekts auf Azure generieren zwei Dateien, die Paketdatei selbst und der zugehörigen Konfigurationsdatei:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Standardmäßig jedes Azure Projekt umfasst eine, die Dienstkonfiguration Datei für lokale (Debuggen) erstellt und ein anderes für Builds Cloud (Staging oder Fertigung), aber Sie können Dateien hinzufügen oder entfernen Dienstkonfiguration – je nach Bedarf. Wenn Sie ein Paket in Visual Studio erstellen, werden Sie aufgefordert, welche Dienstkonfiguration Datei zusammen mit dem Paket einschließen werden. Wenn Sie ein Paket mithilfe von MSBuild erstellen, ist standardmäßig die lokale Dienstkonfiguration Datei enthalten. Um eine andere Dienstkonfiguration Datei einzufügen, legen Sie die `TargetProfile` Eigenschaft des Befehls MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Wenn Sie eine alternative Directory für das gespeicherte Paket und Konfigurationsdateien verwenden möchten, legen Sie den Pfad mithilfe der `/p:PublishDir=Directory\` Option, einschließlich des nachfolgende umgekehrter Schrägstrich Trennzeichens.

## <a name="deployment"></a>Bereitstellung

Nachdem das Paket erstellt wurde, können Sie es in Azure bereitstellen. Ein, die den Prozess veranschaulicht, finden Sie unter der Azure-Website. Informationen darüber, wie Sie diesen Prozess zu automatisieren finden Sie unter [Kontinuierlichen Bereitstellung von Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

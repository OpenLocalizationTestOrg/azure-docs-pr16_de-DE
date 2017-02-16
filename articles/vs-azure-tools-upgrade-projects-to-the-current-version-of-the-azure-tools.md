<properties
   pageTitle="Vorgehensweise zum Aktualisieren von Projekten in die aktuelle Version der Azure Tools | Microsoft Azure"
   description="Informationen Sie zum upgrade einer Azure-Projekt in Visual Studio auf die aktuelle Version der Azure-tools"
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

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Vorgehensweise zum Aktualisieren von Projekten in die aktuelle Version der Azure-Tools für Visual Studio

## <a name="overview"></a>(Übersicht)

Nach der Installation der aktuellen Version von Azure-Tools (oder einer früheren Version, die neuer als 1.6), lassen Sie alle Projekte, die mit einer Azure-Tools erstellt wurden, bevor 1.6 (November 2011) wird automatisch aktualisiert, sobald sie zu öffnen. Wenn Sie Projekte mithilfe der 1,6 (November 2011) Version dieser Tools erstellt haben und Sie noch die veröffentlichte Version installiert haben, können Sie diese Projekte in der älteren Version öffnen und später entscheiden, ob sie aktualisiert.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Wie Ihres Projekts ändert, wenn Sie es aktualisieren

Wenn Sie ein Projekt wird automatisch aktualisiert, oder Sie angeben, dass Sie sie aktualisieren möchten, Ihr Projekt geändert wird, um die Arbeit mit den aktuellen Versionen von bestimmter Assemblys und einige Eigenschaften auch geändert werden, werden wie in diesem Abschnitt beschrieben. Wenn Ihr Projekt anderen Änderungen mit der neueren Version der Tools kompatibel erforderlich ist, müssen Sie diese Änderungen manuell vornehmen.

- Die Datei web.config für Webrollen und die App für Worker-Rollen werden aktualisiert, um der neueren Version Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll verweisen.

- Die Assemblys Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll und Microsoft.WindowsAzure.ServiceRuntime.dll werden auf der neuen Versionen aktualisiert.

- Profile veröffentlichen, die in der Azure Projektdatei (.ccproj) gespeichert wurden, werden in einer separaten Datei, mit der Erweiterung .azurePubXml, in dem Unterverzeichnis **Veröffentlichen** verschoben.

- Einige Eigenschaften im Profil veröffentlichen werden aktualisiert, um neue und geänderte Funktionen zu unterstützen. **AllowUpgrade** wird durch **DeploymentReplacementMethod** ersetzt, da Sie einen bereitgestellte Cloud-Dienst gleichzeitig oder inkrementell aktualisieren können.

- Die Eigenschaft **UseIISExpressByDefault** wird hinzugefügt und auf falsch festgelegt, damit der Webserver, der für das Debuggen verwendet wird automatisch von Internet Information Services (IIS) zu IIS Express ändern, wird nicht. IIS Express ist der Standard-Webserver für Projekte, die mit den neueren Versionen der Tools erstellt wurden.

- Wenn Azure Zwischenspeichern in einer oder mehreren Rollen Ihres Projekts gehostet wird, werden einige Eigenschaften in der Dienstkonfiguration (.cscfg-Datei) und der Definition des Diensts (.csdef-Datei) geändert, wenn ein Projekt aktualisiert wird. Wenn das Projekt das Azure Zwischenspeichern NuGet-Paket verwendet, wird das Projekt auf die neueste Version des Pakets aktualisiert. Öffnen Sie die Datei web.config, und stellen Sie sicher, dass die Client-Konfiguration während des Upgrades korrekt geführt wurde. Wenn Sie die Bezüge auf Azure Zwischenspeichern Clientassemblys ohne NuGet-Paket hinzugefügt haben, wird nicht diese Assemblys aktualisiert werden. Sie müssen diese Verweise auf die neuen Versionen manuell aktualisieren.

>[AZURE.IMPORTANT] F-Projekten müssen Sie Verweise auf Azure Assemblys manuell aktualisieren, sodass diese die neueren Versionen dieser Assemblys verweisen.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>So aktualisieren Sie ein Projekt Azure auf der aktuellen Version

1. Installieren Sie die aktuelle Version von Azure-Tools in der Installation von Visual Studio, die Sie für die aktualisierten Projekt verwenden möchten, und öffnen Sie das Projekt, das Sie aktualisieren möchten. Wenn das Projekt mit einer Azure-Tools erstellt wurde vor 1.6 freigeben (November 2011), das Projekt automatisch auf die aktuelle Version aktualisiert. Wenn das Projekt erstellt wurde mit der November 2011 lassen und weiterhin die veröffentlichte Version installiert ist, das Projekt wird in dieser Version geöffnet.

1. Öffnen Sie im Explorer-Lösung das Kontextmenü für den Projektknoten, wählen Sie **Eigenschaften**aus, und wählen Sie dann auf die Registerkarte **Anwendung** im Dialogfeld.

    Die Registerkarte **Anwendung** zeigt die Tools-Version, die das Projekt zugeordnet ist. Wenn die aktuelle Version von Azure-Tools angezeigt wird, wurde bereits das Projekt aktualisiert. Wenn Sie eine neuere Version der Tools als was die Registerkarte zeigt installiert haben, wird eine Schaltfläche **Aktualisieren** .

1. Wählen Sie die Schaltfläche **Aktualisieren** ein Projekts auf die aktuelle Version der Tools aktualisieren aus.

1. Erstellen Sie das Projekt, und klicken Sie dann behandeln Sie Fehlern, die aus der API Changes. Informationen darüber, wie Sie den Code für die neue Version ändern können finden Sie in der Dokumentation für bestimmte API.

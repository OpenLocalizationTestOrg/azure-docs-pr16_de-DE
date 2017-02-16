<properties
   pageTitle="Ausführen und Debuggen einen Clouddienst auf einem lokalen Computer mit Emulator Express | Microsoft Azure"
   description="Verwenden von Emulator Express ausführen und Debuggen einen Clouddienst auf einem lokalen Computer"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Verwenden von Emulator Express ausführen und Debuggen einen Clouddienst auf einem lokalen Computer

Mithilfe der Emulator Express können Sie testen und Debuggen von einem Cloud-Dienst ohne Visual Studio als Administrator ausführen. Sie können Ihre Project-Einstellungen Emulator Express oder den vollständigen Emulator, je nach den Anforderungen der Cloud-Dienst verwenden festlegen. Weitere Informationen zu den vollständigen Emulator finden Sie unter [in der Serveremulator eine Azure-Anwendung ausführen](./storage/storage-use-emulator.md). Emulator Express wurde ursprünglich in Azure SDK 2.1, und zum Zeitpunkt Azure SDK 2.3, ist es der Standard-Emulator.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Verwenden von Emulator Express in Visual Studio-IDE

Beim Erstellen eines neuen Projekts in Azure SDK 2.3 oder höher, ist Emulator Express bereits ausgewählt. Folgendermaßen Sie für vorhandene Projekte, die mit einer früheren Version des SDK erstellt wurden vor, um Emulator Express auszuwählen.

### <a name="to-configure-a-project-to-use-emulator-express"></a>So konfigurieren Sie ein Projekt, um Emulator Express verwenden

1. Klicken Sie im Kontextmenü für das Projekt Azure wählen Sie **Eigenschaften**aus, und wählen Sie dann auf die Registerkarte **Web** .

1. Wählen Sie unter **Lokale Development Server**das Optionsfeld mit **IIS Express verwenden** . Emulator Express ist nicht mit IIS Webserver kompatibel.

1. Wählen Sie unter **Emulator**das Optionsfeld **Express verwenden Emulator** aus.

    ![Emulator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Über die Befehlszeile ebenso Emulator Express

Über die Befehlszeile können Sie die express-Version von der Azure-Serveremulator umfasst, csrun.exe, starten, mit der Option /useemulatorexpress.

## <a name="limitations"></a>Einschränkungen

Bevor Sie Emulator Express verwenden, sollten Sie einige Einschränkungen sein:

- Emulator Express ist nicht mit IIS Webserver kompatibel.

- Ihre Cloud-Dienst kann mehrere Rollen enthalten, aber jede Rolle beschränkt eine Instanz ist.

- Sie können keine Portnummern unter 1000 zugreifen. Wenn Sie einen Authentifizierungsanbieter, der einen Port unter 1000 normal verwendet verwenden, müssen Sie diesen Wert auf eine Port-Nummer zu ändern, die über 1000 liegt.

- Einschränkungen, die auf der Azure-Serveremulator anwenden gelten auch für Emulator Express verwendet werden. Angenommen, Sie mehr als 50 Rolleninstanzen pro Bereitstellung nicht möglich. [Führen Sie in der Serveremulator Anwendung Azure](http://go.microsoft.com/fwlink/p/?LinkId=623050) finden Sie unter

## <a name="next-steps"></a>Nächste Schritte

[Für das Debuggen Cloud Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)

<properties 
   pageTitle="Konfigurieren und Verwenden von Speicheremulator mit Visual Studio | Microsoft Azure"
   description="Konfigurieren und Verwenden von Speicheremulator mit Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfigurieren und Verwenden von Speicheremulator mit Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>(Übersicht)
Die Entwicklungsumgebung Azure SDK enthält Speicheremulator, ein Programm, das die Blob, Warteschlange und Tabelle Speicher verfügbaren Dienste in Azure auf Ihrem Computer lokale Entwicklung simuliert. Wenn Sie sind Cloud-Dienst erstellen, die die Dienste Azure-Speicher beschäftigt oder schreiben eine externe Anwendung, die die Speicherdienste Ruft, können Sie Code anhand von Speicheremulator lokal testen. Azure-Tools für Microsoft Visual Studio integrieren Verwaltung von Speicheremulator in Visual Studio. Azure-Tools Initialisierung die Speicher Emulator Datenbank bei der ersten Verwendung, der Emulator Speicherdienst geöffnet wird, wenn Sie ausführen oder von Code in Visual Studio debuggen und stellt schreibgeschützten Zugriff auf die Speicher Emulator Daten über den Azure-Speicher-Explorer.

Ausführliche Informationen zu Speicheremulator, einschließlich systemvoraussetzungen und Hinweise zur benutzerdefinierten Konfiguration finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testen](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Es gibt einige Unterschiede in der Funktionalität Simulation Emulator Speicher und die Dienste Azure-Speicher. In der Dokumentation Azure SDK Informationen über die Unterschiede finden Sie unter [Unterschiede zwischen der Speicheremulator und Azure Storage Services](./storage/storage-use-emulator.md) .

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Konfigurieren einer Verbindungszeichenfolge für Speicheremulator

Um Speicheremulator von Code innerhalb einer Rolle zugreifen zu können, sollten Sie eine Verbindungszeichenfolge zu konfigurieren, der auf Speicheremulator zeigt und später mit einer Firma Azure-Speicher verweisen geändert werden können. Eine Verbindungszeichenfolge ist eine Einstellung für die Konfiguration, die Ihrer Rolle zur Laufzeit für die Verbindung mit einem Speicherkonto gelesen werden kann. Weitere Informationen zum Erstellen der Verbindungszeichenfolgen finden Sie unter [Konfigurieren der Azure-Anwendung](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Sie können einen Verweis auf die Speicher Emulator Konto Code mithilfe der Eigenschaft **DevelopmentStorageAccount** zurückkehren. Dieser Ansatz funktioniert richtig auf, wenn Sie Code Speicheremulator zugreifen möchten, aber wenn Sie eine Anwendung in Azure veröffentlichen möchten, müssen Sie zum Erstellen einer Verbindungszeichenfolge zum Zugreifen auf Ihr Konto Azure-Speicher, und ändern den Code, um die Verbindungszeichenfolge verwendet, bevor Sie ihn um seine Erlaubnis. Wenn Sie zwischen dem Speicher Emulator-Konto und ein Konto Azure-Speicher häufig wechseln, wird eine Verbindungszeichenfolge diesen Vorgang zu vereinfachen.

## <a name="initializing-and-running-the-storage-emulator"></a>Initialisierung und Speicheremulator ausgeführt

Sie können angeben, dass beim Ausführen oder den Dienst in Visual Studio debuggen, Visual Studio automatisch Speicheremulator startet. Öffnen Sie in-Lösung Explorer das Kontextmenü für ein Projekt **Azure** , und wählen Sie **Eigenschaften**aus. Klicken Sie auf der Registerkarte **Entwicklung** in der Liste **Starten Azure Speicheremulator** wählen Sie **Wahr** (Wenn sie bereits nach diesem Wert festgelegt ist).

Beim ersten Ausführen oder den Dienst von Visual Studio Debuggen startet Speicheremulator einen Initialisierungsprozess an. Dieses Verfahren reserviert lokale Ports für Speicheremulator und die Speicher Emulator Datenbank erstellt. Wenn der Vorgang abgeschlossen ist, muss dieses Verfahren nicht erneut ausführen, wenn die Speicher Emulator Datenbank gelöscht wird.

>[AZURE.NOTE] Ab Juni 2012 Release der Azure-Tools, führt Speicheremulator, standardmäßig in SQL Express LocalDB. In früheren Versionen von Azure-Tools, Speicheremulator ausgeführt wird, anhand einer Standardinstanz von SQL Express 2005 oder 2008, die Sie installieren müssen, bevor Sie können die Azure SDK installieren. Sie können auch die Speicheremulator anhand einer benannten Instanz von SQL Express oder eines benannten oder Standardinstanz von Microsoft SQL Server ausführen. Wenn Sie konfigurieren Speicheremulator für eine andere Instanz als die Standardinstanz ausführen müssen, finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testen](./storage/storage-use-emulator.md).

Speicheremulator stellt eine Benutzeroberfläche zum Anzeigen des Status der lokalen Speicherdienste und starten, zu beenden und zurückgesetzt werden. Nachdem der Emulator Speicherdienst gestartet wurde, können Sie-Benutzeroberfläche angezeigt oder beginnen oder beenden Sie den Dienst, indem Sie der rechten Maustaste auf das Symbol im Infobereich für den Microsoft Azure Emulator in der Windows-Taskleiste.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Anzeigen von Speicher Emulator Daten im Server-Explorer

Der Knoten Azure-Speicher im Server-Explorer können Sie Daten anzeigen und Ändern der Einstellungen für Blob und Tabelle Daten in Ihrer Speicherkonten, einschließlich des Speicheremulator. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer](https://msdn.microsoft.com/library/azure/ff683677.aspx) .

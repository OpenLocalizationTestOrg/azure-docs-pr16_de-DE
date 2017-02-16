<properties
    pageTitle="Erste Schritte mit Azure-Speicher in fünf Minuten | Microsoft Azure"
    description="Abheben Sie schnell auf Microsoft Azure Blobs, Tabelle und Warteschlangen mit Azure-Speicher schnelle beginnt, Visual Studio und Azure Speicheremulator. Führen Sie Ihre erste Azure-Speicher Anwendung in fünf Minuten."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Erste Schritte mit Azure-Speicher in fünf Minuten

## <a name="overview"></a>(Übersicht)

Es ist einfach Schritte zur Entwicklung von mit Azure-Speicher. In diesem Lernprogramm erfahren Sie, wie Sie eine Anwendung Azure-Speicher von Sache kommen. Sie verwenden die Azure SDK für .NET enthaltene Schnellstart Vorlagen. Diese Symbolleiste beginnt sofort-und-Los-Code enthalten, der veranschaulicht einige grundlegenden programming Szenarien mit Azure-Speicher.

Wenn Sie weitere Informationen zur Azure-Speicher vor dem Einstieg in den Code, lesen Sie [Nächste Schritte](#next-steps).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, benötigen Sie die folgenden Vorkenntnisse:

1. Zum Kompilieren und erstellen Sie die Anwendung, benötigen Sie eine Version von [Visual Studio](https://www.visualstudio.com/) auf Ihrem Computer installiert ist.

2. Installieren Sie die neueste Version [Azure SDK für .NET](https://azure.microsoft.com/downloads/). Das SDK enthält die Azure Schnellstart Stichprobe Projekte, Azure Speicheremulator und [Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Stellen Sie sicher, dass Sie [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) auf Ihrem Computer installiert haben, wie sie von der Azure Schnellstart Stichprobe Projekte benötigt wird, die wir in diesem Lernprogramm verwenden möchten.

    Wenn Sie nicht sicher sind, welche Version von .NET Framework in Ihrem Computer installiert ist, finden Sie unter [wie: bestimmen, welche .NET Framework-Versionen installiert sind](https://msdn.microsoft.com/vstudio/hh925568.aspx). Oder drücken Sie die Schaltfläche **Start** oder die Windows-Taste, geben Sie **Control Panel**. Klicken Sie dann auf **Programme** > **Programme und Funktionen**, und stellen Sie fest, ob .NET Framework 4.5 unter der installierten Programme aufgeführt wird.

4. Sie benötigen ein Azure-Abonnement und ein Konto Azure-Speicher.

    - Ein Azure-Abonnement können, finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Einkauf Optionen](https://azure.microsoft.com/pricing/purchase-options/)und [Mitglied bietet](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network, und BizSpark und anderen Microsoft-Programmen).
    - Zum Erstellen eines Speicher-Kontos in Azure finden Sie unter [So erstellen Sie ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Führen Sie Ihre erste Azure-Speicher Anwendung für Azure-Speicher in der cloud

Nachdem Sie ein Konto besitzen, können Sie eine einfache Azure-Speicher Anwendung mithilfe einer der Stichprobe Azure schnelle beginnt Projekte in Visual Studio erstellen. In diesem Lernprogramm liegt der Schwerpunkt auf der Stichprobe Projekte Azure Storage: **Azure-Speicher: Blobs**, **Azure-Speicher: Dateien**, **Azure-Speicher: Warteschlangen**, und **Azure-Speicher: Tabellen**:

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **installierte** > **Vorlagen** > **C#-** > **Cloud** > **Schnellstart** > **Data Services**.
    ein. Wählen Sie eine der folgenden Vorlagen: **Azure-Speicher: Blobs**, **Azure-Speicher: Dateien**, **Azure-Speicher: Warteschlangen**, oder **Azure-Speicher: Tabellen**.
    b. Stellen Sie sicher, dass **.NET Framework 4.5** als die Ziel-Framework aktiviert ist.
    - 3.c. Geben Sie einen Namen für Ihr Projekt, und erstellen Sie die neue Visual Studio-Lösung, wie dargestellt:

    ![Azure schnellen Starts][Image1]

Sie möchten den Quellcode zu überprüfen, bevor Sie die Anwendung ausführen. So prüfen Sie den Code, wählen Sie **Explorer Lösung** in Visual Studio im Menü **Ansicht** aus. Klicken Sie dann Doppelklicken auf die Datei Program.cs.

Führen Sie dann die Anwendung Beispiel:

1.  Wählen Sie in Visual Studio **Lösung Explorer** im Menü **Ansicht** aus. Öffnen Sie die App und kommentieren Sie die Verbindungszeichenfolge für Azure Speicheremulator aus:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Entfernen Sie die Kommentarzeichen der Verbindungszeichenfolge für den Azure-Speicher-Dienst, und geben Sie die Speicher Konto Name und Access-Taste in der App:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Rufen Sie Ihre Speicher Konto Zugriffstaste finden Sie unter [verwalten die Zugriffstasten Speicher](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Nach Sie den Namen des Kontos Speicher bereitstellen und Zugriffstaste in der App, klicken Sie auf das Menü **Datei** , klicken Sie auf **Alles speichern** alle Projektdateien speichern.
4.  Klicken Sie im Menü **Erstellen** auf **Lösung erstellen**.
5.  Klicken Sie im Menü **Debuggen** drücken Sie **F11** , um die Lösung Schritt für Schritt ausführen, oder drücken Sie **F5** , um die Lösung auszuführen.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Führen Sie Ihre erste Azure-Speicher Anwendung lokal für Speicheremulator Azure

Der [Speicheremulator Azure](storage-use-emulator.md) bietet eine lokale Umgebung, die Dienste Azure Blob, Warteschlange und Tabelle hinsichtlich der Entwicklung emuliert. Speicheremulator können Speicher testen die Anwendung lokal, ohne eine Azure-Abonnements oder von Speicher-Konto erstellen und ohne alle Kosten anfallen.

Wir eine einfache Azure-Speicher Anwendung mithilfe einer der Stichprobe Azure schnelle beginnt Projekte in Visual Studio erstellen, um ihn zu testen. In diesem Lernprogramm Schwerpunkt auf die **Azure BLOB-Speicher**, **Azure Table Storage**und **Azure Warteschlangenspeicher** Stichprobe Projekte:

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **installierte** > **Vorlagen** > **C#-** > **Cloud** > **Schnellstart** > **Data Services**.
    ein. Wählen Sie eine der folgenden Vorlagen: **Azure-Speicher: Blobs**, **Azure-Speicher: Dateien**, **Azure-Speicher: Warteschlangen**, oder **Azure-Speicher: Tabellen**.
    b. Stellen Sie sicher, dass **.NET Framework 4.5** als die Ziel-Framework aktiviert ist.
    c. Geben Sie einen Namen für Ihr Projekt, und erstellen Sie die neue Visual Studio-Lösung, wie dargestellt:

    ![Azure schnellen Starts][Image1]

4.  Wählen Sie in Visual Studio **Lösung Explorer** im Menü **Ansicht** aus. Öffnen Sie die App und kommentieren Sie die Verbindungszeichenfolge für Ihr Konto Azure-Speicher, wenn Sie bereits hinzugefügt haben. Kommentieren Sie dann die Verbindungszeichenfolge für Azure Speicheremulator an:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Sie möchten den Quellcode zu überprüfen, bevor Sie die Anwendung ausführen. So prüfen Sie den Code, wählen Sie **Explorer Lösung** in Visual Studio im Menü **Ansicht** aus. Klicken Sie dann Doppelklicken auf die Datei Program.cs.

Führen Sie dann die Stichprobe Anwendung in Azure Speicheremulator an:

1.  Drücken Sie die Schaltfläche **Start** oder die Windows-Taste, suchen Sie nach *Microsoft Azure Speicheremulator*, und starten Sie die Anwendung. Wenn der Emulator gestartet wird, sehen Sie ein Symbol und eine Benachrichtigung im Windows Task Ansichtsbereich angezeigt.
2.  Klicken Sie in Visual Studio im Menü **Erstellen** auf **Lösung erstellen** .
3.  Klicken Sie im Menü **Debuggen** drücken Sie **F11** , um die Lösung Schritt für Schritt ausführen, oder drücken Sie Sie **F5** , um die Lösung von a bis z auszuführen.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie hier weitere Informationen zu Azure-Speicher aus:

* [Einführung in Microsoft Azure-Speicher](storage-introduction.md)
* [Erste Schritte mit Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md)
* [Erste Schritte mit Azure Tabellenspeicher mit .NET](storage-dotnet-how-to-use-tables.md)
* [Erste Schritte mit Azure Warteschlangenspeicher mit .NET](storage-dotnet-how-to-use-queues.md)
* [Erste Schritte mit auf Windows Azure-Datei-Speicher](storage-dotnet-how-to-use-files.md)
* [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
* [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png

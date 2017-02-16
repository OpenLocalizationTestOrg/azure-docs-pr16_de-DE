<properties 
    pageTitle="Wie Warteschlange-Speicher von Ruby verwendet | Microsoft Azure" 
    description="Informationen zum Verwenden der Warteschlange Azure Service erstellen und Löschen von Warteschlangen, einfügen, abrufen und Löschen von Nachrichten. Die Beispiele in Ruby geschrieben wurde." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Wie Warteschlange-Speicher von Ruby verwendet.

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mit Microsoft Azure Warteschlange-Speicherdienst ausführen. Die Beispiele werden mithilfe der Ruby Azure-API geschrieben.
Die Szenarios dieser gehören **Einfügen**, **Empfang**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Erstellen Sie eine Anwendung Ruby

Erstellen einer Ruby Anwendung. Anweisungen finden Sie unter [Ruby auf Schienen Webanwendung eine Azure-virtuellen Computers](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendung für Access-Speicher

Wenn Azure-Speicher verwenden möchten, müssen Sie das Ruby Azure-Paket, das eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren herunterladen und verwenden.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden Sie RubyGems, um das Paket zu erhalten.

1. Verwenden Sie eine Line Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster zum Installieren der Gem und Abhängigkeiten "Gem installieren Azure" ein.

### <a name="import-the-package"></a>Das Paket importieren

Verwenden Sie einen Texteditor, fügen Sie den folgenden an den Anfang der Ruby Datei für den Speicher verwenden möchten:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Für die Einrichtung einer Verbindungs Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS_KEY** Informationen erforderlich, um eine Verbindung mit Ihrem Konto Azure-Speicher. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen vor der Verwendung von **Azure::QueueService** mit den folgenden Code angeben:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
Um diese Werte aus einer Classic oder Ressourcenmanager Speicherkonto Azure-Portal zu erhalten:

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie in den Einstellungen Blade auf der rechten Seite auf **Zugriffstasten**.
4. In der Access-Taste Blade, das angezeigt wird, wird die Zugriffstaste 1 und 2-Zugriffstaste angezeigt. Sie können eine der folgenden verwenden. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren. 

Um diese Werte von einem Speicherkonto klassischen im klassischen Azure-Portal zu erhalten:

1. Melden Sie sich der [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie auf **Verwalten ZUGRIFFSTASTEN** am unteren Rand des Navigationsbereichs.
4. Im Popup-Dialogfeld sehen Sie Speicher Kontoname, Zugriffstaste primären und sekundären Zugriffstaste. Zugriffstaste können Sie entweder der primären oder der sekundären. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren.

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen eine Warteschlange

Der folgende Code erstellt ein Objekt **Azure::QueueService** , womit Sie mit Warteschlangen arbeiten kann.

    azure_queue_service = Azure::QueueService.new

Verwenden Sie die **create_queue()** -Methode zum Erstellen einer Warteschlange mit dem angegebenen Namen ein.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Verwenden Sie zum Einfügen einer Nachricht in einer Warteschlange die **create_message()** -Methode zum Erstellen einer neuen Nachricht, und fügen es an die an.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne ihn zu entfernen aus der Warteschlange durch Aufrufen der **Anheften\_messages()** Methode. Standardmäßig **Anheften\_messages()** sieht ein, die bei einer einzelnen Nachricht. Sie können auch angeben, wie viele Nachrichten einsehen möchten.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Gewusst wie: Abrufen der nächsten Nachricht

Sie können eine Nachricht von einer Warteschlange in zwei Schritten entfernen.

1. Wenn Sie sich einwählen **Liste\_messages()**, erhalten Sie die nächste Nachricht in einer Warteschlange standardmäßig. Sie können auch angeben, wie viele Nachrichten Sie erhalten möchten. Die Nachrichten aus zurückgegeben **Liste\_messages()** für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar ist. Sie übergeben die Sichtbarkeit Timeout in Sekunden als Parameter.

2. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch **delete_message()**aufrufen.

Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Ihre Anrufe Code **Löschen\_message()** sofort nach die Nachricht verarbeitet wurde.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern. Im folgenden Code verwendet die **update_message()** -Methode, um eine Nachricht aktualisieren. Die Methode gibt ein Tupels zurück, das enthält ansprechend Bestätigung der Warteschlange Nachricht und eine UTC-Zeitwert, der darstellt, wenn die Nachricht in der Warteschlange sichtbar sein sollen.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Gewusst wie: Zusätzliche Optionen für das Entfernen aus der Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.

1. Sie können eine Reihe von Nachricht erhalten.

2. Sie können festlegen, einen Timeout verlängern oder verkürzen Invisibility zulassen von Ihrem Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit.

Im folgenden code wird mithilfe der **Liste\_messages()** Methode, 15 Nachrichten in einem Anruf zu gelangen. Druckt, und löscht jede Nachricht. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen der Warteschlangenlänge

Sie können eine Schätzung der Anzahl von Nachrichten in der Warteschlange abrufen. Die **abrufen\_Warteschlange\_metadata()** Methode gefragt werden, den Warteschlangendienst, um die Anzahl der ungefähren Nachrichten und Metadaten zur Warteschlange zurückzukehren.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen\_queue()** Methode für das Warteschlangenobjekt.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- Besuchen Sie den [Teamblog Azure-Speicher](http://blogs.msdn.com/b/windowsazurestorage/)
- Besuchen Sie das [Azure SDK für Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) Repository auf GitHub

Einen Vergleich zwischen den Azure Warteschlange Service erläutert in diesem Artikel und Azure Bus Servicewarteschlangen erläuterten im Artikel [So Bus Servicewarteschlangen verwenden](/develop/ruby/how-to-guides/service-bus-queues/) finden Sie unter [Azure Warteschlangen und Service Bus Warteschlangen - verglichen und Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

<properties
    pageTitle="Verwalten von Azure BLOB-Speicher Ressourcen mit Speicher-Explorer (Preview) | Microsoft Azure"
    description="Verwalten von Azure Blob Container und Blobs mit Speicher-Explorer (Preview)"
    services="storage"
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
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Verwalten der Azure BLOB-Speicher Ressourcen mit Speicher-Explorer (Preview)

## <a name="overview"></a>(Übersicht)

[Azure BLOB-Speicher](./storage/storage-dotnet-how-to-use-blobs.md) ist ein Dienst zum Speichern von große Mengen von unstrukturierten Daten, wie z. B. Text oder binäre Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können.
Sie können Daten in die Welt öffentlich verfügbar machen oder zum Speichern von Anwendungsdaten privat Blob-Speicher verwenden. In diesem Artikel erfahren Sie, wie Speicher-Explorer (Preview) für die Arbeit mit Blob-Container und Blobs verwendet werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

- [Herunterladen und Installieren von Speicher-Explorer (Preview)](http://www.storageexplorer.com)
- [Verbinden Sie mit einem Azure-Speicher-Konto oder einen bestimmten Dienst](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Erstellen eines Containers blob

Alle Blobs müssen sich in einem Blob-Container, also einfach eine logische Gruppierung von Blobs befinden. Ein Konto kann eine unbegrenzte Anzahl von Container enthalten, und jeder Container kann eine unbegrenzte Anzahl von Blobs speichern.

Die folgenden Schritte veranschaulichen, wie einen Blob-Container im Speicher-Explorer (Preview) erstellen.

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, in dem Sie den Blob-Container erstellen möchten.
1.  Mit der rechten Maustaste **Blob-Container**und - wählen Sie aus dem Kontextmenü - **Blob-Container erstellen**.

    ![Kontextmenü für Blob-Container erstellen][0]

1.  Ein Textfeld wird unterhalb des Ordners **Blob Container** angezeigt. Geben Sie den Namen für Ihre Blob-Container aus. Finden Sie im Abschnitt [Container Benennungskonventionen](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) für eine Liste von Regeln und Einschränkungen zur Benennung von Blob-Container aus.

    ![Blob-Container Textfeld erstellen][1]

1.  Drücken Sie die **EINGABETASTE** nach Abschluss den Blob-Container erstellen oder **Esc** , um den Vorgang abzubrechen. Nachdem Sie der Container Blob erfolgreich erstellt wurde, wird es unter dem **Blob Container** Ordner für das ausgewählte Speicherkonto angezeigt.

    ![BLOB-Container erstellt][2]

## <a name="view-a-blob-containers-contents"></a>Anzeigen eines Containers Blob-Inhalte

BLOB-Container enthalten Blobs und Ordner (, auch Blobs enthalten können).

Die folgenden Schritte veranschaulichen, wie Sie den Inhalt eines Containers BLOB-Speicher-Explorer (Preview) innerhalb der anzeigen:

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher Kontos den Container Blob mit, dass Sie anzeigen möchten.
1.  Erweitern Sie im Speicher des Kontos **Blob-Container**aus.
1.  Mit der rechten Maustaste in des Blob-Containers, die, den Sie anzeigen möchten, und - wählen Sie aus dem Kontextmenü - **Öffnen Blob-Container-Editor**.
Sie können auch den Container Blob doppelklicken, die, den Sie anzeigen möchten.

    ![Öffnen Blob Container-Editor-Kontextmenüs][19]

1.  Klicken Sie im Hauptfenster Bereich wird der Blob-Container Inhalt angezeigt wird.

    ![BLOB Container-editor][3]

## <a name="delete-a-blob-container"></a>Löschen eines Containers blob

BLOB-Container können einfach erstellt und gelöscht werden, je nach Bedarf. (Wenn So löschen Sie einzelne Blobs anzeigen möchten, finden Sie in der im Abschnitt [Verwaltung Blobs in einem Container Blob](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie zum Löschen eines Containers BLOB-Speicher-Explorer (Preview):

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, die den Container Blob enthält, dass Sie anzeigen möchten.
1.  Erweitern Sie die Speicher-Konto **Blob-Container**aus.
1.  Mit der rechten Maustaste in des Blob-Containers, die, den Sie löschen möchten, und - wählen Sie aus dem Kontextmenü - **Löschen**.
Sie können auch den aktuell ausgewählten Blob Container löschen **Löschen** drücken.

    ![Löschen von Blob Container-Kontextmenü][4]

1.  Wählen Sie **Ja** , um das Bestätigungsdialogfeld.

    ![Löschen von Blob Container Bestätigung][5]

## <a name="copy-a-blob-container"></a>Kopieren eines Containers blob

Speicher-Explorer (Preview) können Sie einen Container Blob in die Zwischenablage zu kopieren, und fügen Sie Blob Container in ein anderes Speicherkonto. (Wenn so kopieren Sie einzelne Blobs anzeigen möchten, finden Sie in Abschnitt [Verwaltung Blobs in einem Container Blob](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie ein Containers BLOB-ein Speicherkonto in eine andere zu kopieren.

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, die den Container Blob enthält, dass Sie kopieren möchten.
1.  Erweitern Sie im Speicher des Kontos **Blob-Container**aus.
1.  Mit der rechten Maustaste in des Blob-Containers, die, den Sie kopieren möchten, und - wählen Sie aus dem Kontextmenü - **Kopie Blob-Container**aus.

    ![Kopieren von Blob Container-Kontextmenü][6]

1.  Mit der rechten Maustaste in des gewünschte "Ziel" Speicherkontos, in dem Sie den Container Blob einfügen möchten, und - wählen Sie aus dem Kontextmenü - **Blob-Container einzufügen**.

    ![Einfügen Blob Container-Kontextmenü][7]

## <a name="get-the-sas-for-a-blob-container"></a>Abrufen der SAS für einen Blob-container

Eine [freigegebene Access-Signatur (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) bietet delegierten Zugriff auf Ihr Speicherkonto Ressourcen.
Dies bedeutet, dass Sie, dass ein Client für einen angegebenen Zeitraum Zeit und mit einem zuvor festgelegten Berechtigungen, Berechtigungen für Objekte in Ihr Speicherkonto eingeschränkt gewähren können, ohne zu Ihrem Konto Zugriffstasten freigeben.

Die folgenden Schritte veranschaulichen, wie eine SAS für einen Container Blob zu erstellen:

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, enthält des Blob-Containers für den Sie eine SAS ermitteln möchten.
1.  Erweitern Sie die Speicher-Konto **Blob-Container**aus.
1.  Mit der rechten Maustaste in des gewünschten Blob-Containers und - wählen Sie aus dem Kontextmenü - **Freigegebene Access-Signatur erhalten**.

    ![Abrufen von SAS-Kontextmenü][8]

1.  Klicken Sie im Dialogfeld **Access-Signatur freigegeben** Geben Sie die Richtlinie, die Anfangs-und Ablaufdatum, die Zeitzone und Zugriffsebenen Sie, die für die Ressource aus.

    ![Abrufen von SAS-Optionen][9]

1.  Wenn Sie angeben der SAS Optionen abgeschlossen haben, wählen Sie **Erstellen**.

1.  Klicken Sie dann wird ein Dialogfeld für das zweites **Freigegeben Access Signatur** angezeigt, die Listen den Blob-Container zusammen mit der URL und dem Formularwerte, die Sie verwenden können, auf die Speicherressource zuzugreifen.
Wählen Sie **Kopie** neben der URL, die Sie in die Zwischenablage kopieren möchten.

    ![Kopieren Sie die SAS-URLs][10]

1.  Wenn Sie fertig sind, wählen Sie auf **Schließen**.

## <a name="manage-access-policies-for-a-blob-container"></a>Verwalten von Richtlinien für einen Blob-container

Die folgenden Schritte veranschaulichen, wie verwalten (hinzufügen und Entfernen von) Richtlinien für einen Blob-Container zugreifen:

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, enthält des Blob-Containers, deren Zugriffsrichtlinien, die Sie verwalten möchten.
1.  Erweitern Sie die Speicher-Konto **Blob-Container**aus.
1.  Wählen Sie den gewünschten Blob-Container und - wählen Sie aus dem Kontextmenü - **Richtlinien verwalten**.

    ![Verwalten von Access Richtlinien-Kontextmenü][11]

1.  Das Dialogfeld **Access-Richtlinien** werden alle Richtlinien für den ausgewählten Blob Container bereits erstellten aufgelistet.

    ![Zugriffsoptionen für die Richtlinie][12]        

1.  Wie folgt vor, je nach Access Richtlinie Management Aufgabe:

    - **Hinzufügen einer neuen Zugriffsrichtlinie** - Select **Hinzufügen**. Nach dem generieren, wird im Dialogfeld **Access-Richtlinien** , die neu hinzugefügte Zugriffsrichtlinie (mit Standardeinstellungen) angezeigt.
    - **Bearbeiten eine Zugriffsrichtlinie** – nehmen Sie alle gewünschten Änderungen vor, und wählen Sie **Speichern**aus.
    - **Entfernen eine Zugriffsrichtlinie** - Select **Entfernen** neben der Richtlinie, die Sie entfernen möchten.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Einrichten der öffentlichen Zugriffsebene für einen Blob-container

Standardmäßig wird jeder Blob-Container auf "Keine öffentliche Zugriff" festgelegt.

Die folgenden Schritte veranschaulichen, wie eine öffentliche Zugriffsebene für einen Blob-Container festlegen.

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, enthält des Blob-Containers, deren Zugriffsrichtlinien, die Sie verwalten möchten.
1.  Erweitern Sie die Speicher-Konto **Blob-Container**aus.
1.  Wählen Sie den gewünschten Blob-Container und - wählen Sie aus dem Kontextmenü - **Einrichten öffentlichen Zugriffsebene**.

    ![Festlegen der Ebene öffentlichen Access-Kontextmenü][13]

1.  Klicken Sie im Dialogfeld **Set Container öffentlichen Access Level** Geben Sie die gewünschte Zugriffsebene.

    ![Festlegen von Optionen für Öffentliche Access-Ebene][14]

1.  Wählen Sie **anwenden**.

## <a name="managing-blobs-in-a-blob-container"></a>Verwalten von Blobs in einem Blob-container

Nachdem Sie einen Container Blob erstellt haben, können Sie einen Blob auf Container Blob hochladen, einen Blob mit Ihrem lokalen Computer herunterladen, öffnen einen Blob auf Ihrem lokalen Computer und vieles mehr.

Die folgenden Schritte veranschaulichen, wie zum Verwalten der Blobs (und Ordnern) innerhalb eines Containers Blob.

1.  Öffnen Sie die Speicher-Explorer (Preview).
1.  Erweitern Sie im linken Bereich des Speicher-Kontos, die den Container Blob enthält, dass Sie verwalten möchten.
1.  Erweitern Sie im Speicher des Kontos **Blob-Container**aus.
1.  Doppelklicken Sie auf die Blob-Container, die, den Sie anzeigen möchten.
1.  Klicken Sie im Hauptfenster Bereich wird der Blob-Container Inhalt angezeigt wird.

    ![Blob-Container anzeigen][3]

1.  Klicken Sie im Hauptfenster Bereich wird der Blob-Container Inhalt angezeigt wird.

1.  Führen Sie die folgenden Schritte aus, abhängig von der Aufgabe, die Sie ausführen möchten:

    - **Hochladen von Dateien zu einem Container blob**

        1.  Wählen Sie auf das Hauptfenster Symbolleiste **Hochladen**und **Hochladen von Dateien** aus dem Dropdownmenü aus.

            ![Hochladen von Dateien im Menü][15]

        1.  Wählen Sie das Auslassungszeichen****(...), klicken Sie auf der rechten Seite des Textfelds **Dateien** wählen Sie die Datei(en) aus, die Sie hochladen möchten, klicken Sie im Dialogfeld **Hochladen von Dateien** .

            ![Hochladen von Dateien Optionen][16]

        1.  Geben Sie den **BLOB-Typ**. [Erste Schritte mit Azure Blob-Speicher mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) Artikel erläutert die Unterschiede zwischen den verschiedenen Typen von Blob.

        1.  Geben Sie optional einen Zielordner, in dem die ausgewählten Dateien hochgeladen werden. Wenn der Zielordner nicht vorhanden ist, wird er erstellt.

        1.  Wählen Sie **Hochladen**aus.

    - **Hochladen eines Ordners zu einer Blob-container**

        1.  Wählen Sie auf das Hauptfenster Symbolleiste **Hochladen**und dann auf **Ordner hochladen** aus dem Dropdownmenü aus.

            ![Menü ' Hochladen ' Ordner][17]

        1.  Wählen Sie das Auslassungszeichen****(...), klicken Sie auf der rechten Seite des Textfelds **Ordner** den Ordner auszuwählen, deren Inhalt Sie hochladen möchten, klicken Sie im Dialogfeld **Ordner hochladen** .

            ![Hochladen von Ordneroptionen][18]

        1.  Geben Sie den **BLOB-Typ**. [Erste Schritte mit Azure Blob-Speicher mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) Artikel erläutert die Unterschiede zwischen den verschiedenen Typen von Blob.

        1.  Geben Sie optional einen Zielordner, in dem Inhalt des ausgewählten Ordners hochgeladen werden. Wenn der Zielordner nicht vorhanden ist, wird er erstellt.

        1.  Wählen Sie **Hochladen**aus.

    - **Einen Blob mit Ihrem lokalen Computer herunterladen**

        1.  Wählen Sie das Blob, die, das Sie herunterladen möchten.

        1.  Wählen Sie im Hauptfenster Bereich Symbolleiste **herunterladen**.

        1.  Geben Sie den Speicherort das Blob heruntergeladen werden soll, und den Namen, die Sie dieser verleihen möchten, klicken Sie im Dialogfeld **geben an, wo das heruntergeladene Blob speichern** .  

        1.  Wählen Sie **Speichern**aus.

    - **Öffnen Sie einen Blob auf Ihrem lokalen computer**

        1.  Wählen Sie das Blob, das Sie öffnen möchten.

        1.  Wählen Sie im Hauptfenster Bereich Symbolleiste **Öffnen**.

        1.  Das Blob werden heruntergeladen und geöffnet, mithilfe der Anwendung, die mit dem Blob des zugrunde liegenden Dateityp verknüpft ist.

    - **Kopieren Sie einen Blob in die Zwischenablage.**

        1.  Wählen Sie das Blob, die, das Sie kopieren möchten.

        1.  Wählen Sie im Hauptfenster Bereich Symbolleiste **Kopieren**.

        1.  Klicken Sie im linken Bereich navigieren Sie zu einem anderen Blob-Container, und doppelklicken Sie darauf, um es anzuzeigen, klicken Sie im Hauptfenster.

        1.  Wählen Sie im Hauptfenster Bereich Symbolleiste **Einfügen** aus, um eine Kopie der Blob erstellen.

    - **Löschen eines BLOBs**

        1.  Wählen Sie das Blob, die, das Sie löschen möchten.

        1.  Wählen Sie im Hauptfenster Bereich Symbolleiste **Löschen**.

        1.  Wählen Sie **Ja** , um das Bestätigungsdialogfeld.

## <a name="next-steps"></a>Nächste Schritte

- Anzeigen der [neuesten Speicher-Explorer (Preview) freigeben, Notizen und Videos bereit](http://www.storageexplorer.com).
- Informationen zum [Erstellen von Applications mit Azure Blobs, Tabellen, Warteschlangen und Dateien](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
<properties
    pageTitle="Erste Schritte mit Speicher-Explorer (Preview) | Microsoft Azure"
    description="Verwalten von Azure-Speicherressourcen mit Speicher-Explorer (Preview)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Erste Schritte mit Speicher-Explorer (Preview)

## <a name="overview"></a>(Übersicht) 

Microsoft Azure-Speicher-Explorer (Preview) ist eine eigenständige Anwendung, die Sie einfach mit Azure-Speicher Daten unter Windows, OS X und Linux arbeiten kann. In diesem Artikel lernen Sie die verschiedenen Möglichkeiten zum Herstellen einer Verbindung mit und Ihren Konten Azure-Speicher verwaltet werden.

![Microsoft Azure-Speicher-Explorer (Preview)][15]

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Herunterladen und Installieren von Speicher-Explorer (Preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinden Sie mit einem Speicher-Konto oder einen bestimmten Dienst

Speicher-Explorer (Preview) bietet eine unzähligen Verwendungsmöglichkeiten zu Speicherkonten herstellen. Herstellen einer Verbindung mit Speicherkonten Ihrer Azure Abonnements, Herstellen einer Verbindung mit Speicherkonten und Dienste, die von anderen Azure-Abonnements freigegeben zugeordnet oder das auch herstellen und Verwalten von lokale Speicherung mit Azure Speicheremulator umfasst:

- [Verbinden einer Azure-Abonnement](#connect-to-an-azure-subscription) - verwalten Speicherressourcen, die für Ihr Abonnement Azure gehören.
- [Arbeiten mit lokale Entwicklung Speicher](#work-with-local-development-storage) – lokale Speicherung mit Azure Speicheremulator verwalten. 
- [Anfügen an externe Speicher](#attach-or-detach-an-external-storage-account) - verwalten Speicherressourcen, die zu einem anderen Azure-Abonnement mit Speicher-Konto Kontonamen und Schlüssel gehören.
- [Anfügen von Speicher-Konto mithilfe SAS](#attach-storage-account-using-sas) - verwalten Speicherressourcen, die zu einem anderen Azure-Abonnement mit einem SAS gehören.
- [Anfügen-Dienst verwenden, SAS](#attach-service-using-sas) - Verwalten einer bestimmten Speicherdienst (Blob Container, Warteschlange oder Tabelle), die zu einem anderen Azure-Abonnement mit einem SAS gehören.

## <a name="connect-to-an-azure-subscription"></a>Verbinden Sie mit einem Azure-Abonnement

> [AZURE.NOTE] Wenn Sie ein Azure-Konto besitzen, können Sie [Sie sich für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Vorteile Ihres Visual Studio Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Wählen Sie im Speicher-Explorer (Preview) **Azure-Konten-Einstellungen**aus. 

    ![Azure Konten-Einstellungen][0]

1. Im linke Bereich werden jetzt alle Microsoft-Konten angezeigt, denen Sie angemeldet haben. Informationen zum Verbinden mit einem anderen Konto wählen Sie **ein Konto hinzufügen**, und führen Sie die Dialogfelder mit einem Microsoft-Konto anmelden, die mit mindestens eine aktive Azure-Abonnement verknüpft ist.

    ![Hinzufügen eines Kontos][1]

1. Sobald Sie erfolgreich mit einem Microsoft-Konto angemeldet, wird im linke Bereich mit der Azure-Abonnements mit diesem Konto verknüpft sind gefüllt werden soll. Wählen Sie die Azure Abonnements, mit denen Sie arbeiten möchten, und wählen Sie dann auf **Übernehmen**. (Markieren **alle Abonnements** schaltet alle oder keines der aufgelisteten Azure-Abonnements auswählen).

    ![Wählen Sie aus Azure-Abonnements][3]

1. Im linke Bereich werden die Speicherkonten die ausgewählten Azure-Abonnements zugeordnet jetzt angezeigt werden.

    ![Ausgewählten Azure-Abonnements][4]

## <a name="work-with-local-development-storage"></a>Arbeiten Sie mit lokale Entwicklung Speicher

Speicher-Explorer (Preview) können Sie für die lokale Speicherung mit Azure Speicheremulator arbeiten. Dadurch können Sie zum Schreiben von Code für, und Testen Speicher ohne zu einem Speicherkonto (da das Speicherkonto durch Speicheremulator Azure emuliert wird) auf Azure bereitgestellt haben.

>[AZURE.NOTE] Azure Speicheremulator wird derzeit nur für Windows unterstützt. 

1. Erweitern Sie im linken Bereich des Speicher-Explorers (Preview), die **(lokale und angefügt** > **Speicherkonten** > Knoten**(Development)** .

    ![Lokale Entwicklung Knoten][21]

1. Wenn Speicheremulator Azure noch nicht installiert haben, werden Sie über eine Infoleiste dazu aufgefordert werden. Wenn die Infoleiste angezeigt wird, wählen Sie **die neueste Version herunterladen**und Installieren des Emulators. 

    ![Herunterladen von Azure Speicheremulator auffordern][22]

1. Nachdem der Emulator installiert wurde, müssen Sie die Möglichkeit zum Erstellen von und Arbeiten mit lokalen Blobs, Queues und Tabellen. Wählen Sie zum Arbeiten mit jeder Kontotyp Speicher finden Sie auf den entsprechenden Link unten:

    - [Verwalten von Azure Blob-Speicher-Ressourcen](./vs-azure-tools-storage-explorer-blobs.md)
    - Verwalten von Azure Datei freigeben Speicherressourcen – *in Kürze verfügbar*
    - Verwalten von Azure Warteschlange Speicherressourcen – *in Kürze verfügbar*
    - Verwalten von Azure Table Storage Ressourcen – *in Kürze verfügbar*

## <a name="attach-or-detach-an-external-storage-account"></a>Fügen Sie an oder trennen Sie einer externen Speicher-Konto

Speicher-Explorer (Preview) bietet die Möglichkeit, die an externe Speicherkonten anfügen, damit Speicherkonten problemlos gemeinsam genutzt werden können. In diesem Abschnitt wird erläutert, wie Anfügen an (und Trennen von) externe Speicherkonten.

### <a name="get-the-storage-account-credentials"></a>Die Anmeldeinformationen des Kontos abrufen

Ein Konto externen Speicherplatz freigeben möchten, muss der Besitzer des Kontos rufen Sie zunächst die Anmeldeinformationen - Kontonamen und Key - für das Konto und anschließendes Freigeben dieser Informationen mit der Person, die dem Konto (externen) anfügen möchte. Beziehen die Anmeldeinformationen des Kontos kann über das Azure-Portal vorgenommen werden, mit folgenden Schritten: 

1.  Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
1.  Wählen Sie **Durchsuchen**.
1.  Wählen Sie **Speicherkonten**.
1.  Wählen Sie in das Blade **Speicherkonten** das gewünschte Speicherkonto ein.
1.  Wählen Sie in den **Einstellungen** Blade für das ausgewählte Speicherkonto **Zugriffstasten**aus.

    ![Tasten Zugriffsoption][5]
    
1.  Kopieren Sie die **Kontonamen Speicher** und die **Taste 1** Werten zur Verwendung in das Blade **Zugriffstasten** beim Anfügen an das Speicherkonto. 

    ![Tastenkombinationen][6]

### <a name="attach-to-an-external-storage-account"></a>Fügen Sie an einem externen Speicher-Konto an
Wenn mit einer Firma externen Speicher anfügen möchten, benötigen Sie die Namen und den Schlüssel des Kontos. Im Abschnitt *erhalten die Anmeldeinformationen des Kontos* wird erläutert, wie diese Werte aus der Azure-Portal zu erhalten. Beachten Sie, dass im Portal kontoschlüssel genannt "key 1" jedoch daher, in der Speicher-Explorer (Preview) für ein Konto Key gefragt werden, wird eingeben (oder einfügen) den Wert "key 1". 
 
1.  Wählen Sie im Speicher-Explorer (Preview) **mit Azure-Speicher verbinden**.

    ![Verbinden Sie mit Azure-Speicher-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung herstellen mit Azure-Speicher** Geben Sie der kontoschlüssel (Wert vom Azure-Portal "key 1 an"), und wählen Sie dann auf **Weiter**.

    ![Herstellen einer Verbindung Azure-Speicher-Dialogfeld mit][24] 

1.  Klicken Sie im Dialogfeld **Anfügen Externspeicher** Geben Sie in das Feld **Kontoname** den Kontonamen Speicher, geben Sie alle gewünschten Einstellungen, und wählen Sie **nächsten** anschließend. 

    ![Externe Speicher Dialogfeld Anfügen][8]

1.  Klicken Sie im Dialogfeld **Verbindung Zusammenfassung** überprüfen Sie die Informationen ein. Wenn Sie etwas ändern möchten, wählen Sie **zurück** , und geben Sie die gewünschten Einstellungen erneut ein. Sobald Sie fertig sind, wählen Sie **Verbinden**.

1.  Nachdem die Verbindung hergestellt wurde, wird das Speicherkonto externe mit dem Text **(externen)** für den Speicher Kontonamen angefügt angezeigt. 

    ![Ergebnis der Herstellen einer Verbindung mit einem externen Speicher-Konto][9]

### <a name="detach-from-an-external-storage-account"></a>Trennen von einem externen Speicher-Konto

1.  Mit der rechten Maustaste in des externen Speicher-Kontos, die zu trennenden und - wählen Sie aus dem Kontextmenü - **Trennen**.

    ![Trennen von Speicheroption][10]

1.  Wenn im Bestätigungsfeld angezeigt wird, wählen Sie **Ja** , die Trennung des externen Speicher-Kontos zu bestätigen.

## <a name="attach-storage-account-using-sas"></a>Anfügen von Speicherkonto mithilfe von SAS

Eine [SAS (Access-Signatur freigegeben)](storage/storage-dotnet-shared-access-signature-part-1.md) bietet der Administrator eines Azure-Abonnements die Möglichkeit, den Zugriff auf ein Speicherkonto vorübergehend gewähren, ohne ihre Anmeldeinformationen ein Azure-Abonnement. 

Um dies zu veranschaulichen, angenommen BenutzerA ist ein Administrator eines Azure-Abonnements und BenutzerA BenutzerB Speicher-Konto für eine begrenzte Zeit mit bestimmten Berechtigungen Zugriff zulassen möchte:

1. UserA generiert eine SAS (aus der Verbindungszeichenfolge für das Speicherkonto) und die gewünschten Berechtigungen für einen bestimmten Zeitraum.
1. UserA teilt die SAS mit der Person, die Zugriff auf die Speicherkonto - UserB, in diesem Beispiel benötigen.  
1. UserB verwendet Speicher-Explorer (Preview) können Sie das Konto, verwenden die bereitgestellten SAS BenutzerA angehören zuordnen. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Holen Sie sich einem SAS für das Konto ein, das Sie freigeben möchten

1.  Im Speicher-Explorer (Preview) mit der rechten Maustaste im Speicher-Kontos, die, das freigegeben werden sollen, und - wählen Sie aus dem Kontextmenü - **Freigegebene Access-Signatur erhalten**.

    ![Abrufen von SAS Kontextmenüoptionen][13]

1. Klicken Sie im Dialogfeld **Access-Signatur freigegeben** Geben Sie den Zeitrahmen und die Berechtigungen, die für das Konto aus, und wählen Sie **Erstellen**.

    ![Abrufen von SAS-Dialogfeld][14]
 
1. Ein Dialogfeld für das zweites **Freigegeben Access Signatur** wird Anzeige der SAS angezeigt. Wählen Sie **Kopie** neben der **Verbindungszeichenfolge** , um ihn in die Zwischenablage zu kopieren. Wählen Sie **Schließen** das Dialogfeld zu schließen.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Fügen Sie an freigegebenen Konto mithilfe der SAS an

1.  Wählen Sie im Speicher-Explorer (Preview) **mit Azure-Speicher verbinden**.

    ![Verbinden Sie mit Azure-Speicher-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung herstellen mit Azure-Speicher** Geben Sie die Verbindungszeichenfolge aus, und wählen Sie dann auf **Weiter**.

    ![Herstellen einer Verbindung Azure-Speicher-Dialogfeld mit][24] 

1.  Klicken Sie im Dialogfeld **Verbindung Zusammenfassung** überprüfen Sie die Informationen ein. Wenn Sie etwas ändern möchten, wählen Sie **zurück** , und geben Sie die gewünschten Einstellungen erneut ein. Sobald Sie fertig sind, wählen Sie **Verbinden**.

1.  Sobald angeschlossen ist, wird das Speicherkonto mit dem Text (SAS) auf den Namen des Kontos eingegebene angefügt angezeigt.

    ![Ergebnis der mit einer Firma mit SAS angefügt][17]

## <a name="attach-service-using-sas"></a>Anfügen mit SAS service

Im Abschnitt [Anfügen Speicherkonto mit SAS](#attach-storage-account-using-sas) veranschaulicht, wie ein Administrator Azure-Abonnement temporären Zugriff auf ein Speicherkonto durch Generieren erteilen (und Freigabe) eine SAS für Speicher-Konto. Auf ähnliche Weise kann ein SAS für einen bestimmten Dienst innerhalb eines Kontos Speicher (Blob Container, Warteschlange oder Tabelle) generiert werden.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generieren einer SAS für den Dienst, den Sie freigeben möchten.

In diesem Zusammenhang kann ein Dienst ein Blob Container, Warteschlange oder Tabelle sein. In den folgenden Abschnitten wird erläutert, wie die SAS für den aufgeführten Dienst generieren:

- [Abrufen der SAS für einen Blob-container](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Holen Sie sich die SAS für eine Dateifreigabe - *in Kürze verfügbar*
- Holen Sie sich die SAS für eine Warteschlange - *in Kürze verfügbar*
- Holen Sie sich die SAS für eine Tabelle – *in Kürze verfügbar*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Fügen Sie an den freigegebenen Kontodienst verwenden die SAS an

1.  Wählen Sie im Speicher-Explorer (Preview) **mit Azure-Speicher verbinden**.

    ![Verbinden Sie mit Azure-Speicher-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** Geben Sie den URI SAS, und wählen Sie dann auf **Weiter**.

    ![Herstellen einer Verbindung Azure-Speicher-Dialogfeld mit][24] 

1.  Klicken Sie im Dialogfeld **Verbindung Zusammenfassung** überprüfen Sie die Informationen ein. Wenn Sie etwas ändern möchten, wählen Sie **zurück** , und geben Sie die gewünschten Einstellungen erneut ein. Sobald Sie fertig sind, wählen Sie **Verbinden**.

1.  Sobald angeschlossen ist, wird der neu angefügte Dienst unter dem Knoten **(Service SAS)** angezeigt.

    ![Ergebnis in einen freigegebenen Service mit SAS Anfügen][20]

## <a name="search-for-storage-accounts"></a>Suchen nach Speicherkonten

Wenn Sie eine lange Liste mit Speicherkonten verfügen, ist eine schnelle Möglichkeit zum Suchen eines bestimmten Speicher-Kontos in das Suchfeld oben im linken Bereich zu verwenden. 

Wie Sie in das Suchfeld eingeben, wird im linke Bereich nur die Speicherkonten, die den zu suchenden Wert entsprechen, die, den Sie bis zu eingegeben haben, angezeigt, die verweisen. Der folgende Screenshot zeigt ein Beispiel, in dem ich für alle Speicherkonten gesucht haben, in denen der Kontonamen Speicher den Text "Tarcher" enthält.

![Speicher Konto suchen][11]
    
Um die Suche zu löschen, wählen Sie die Schaltfläche **X** in das Suchfeld ein.

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Azure Blob-Speicherressourcen mit Speicher-Explorer (Preview)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png

<properties
    pageTitle="Verwenden den Datei-Verbinder in Logik apps | Microsoft Azure-App-Verwaltungsdienst"
    description="So erstellen und konfigurieren Sie die Datei Connector-API app und in einer app Logik in Azure-App-Dienst verwenden"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit der Datei Verbinder, und fügen Sie es zu Ihrer Anwendung Logik
>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus.

Verbinden Sie mit einem Dateisystem hochladen, herunterladen und vieles mehr auf Ihre Dateien auf einem Hostcomputer. Logik apps können auslösen basierend auf einer Vielzahl von Datenquellen und Angebot Verbinder zum Abrufen und Daten zu verarbeiten. Sie können Ihre Geschäftsdaten Workflow und Prozess als Teil dieser Workflow innerhalb einer app Logik den Verbinder Datei hinzufügen. 

Der Verbinder Datei verwendet die Hybrid-Verbindungs-Managers für Hybrid Connectivity im Dateisystem Host.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Erstellen eines Verbinders Datei für Ihre app Logik ##
Wenn Sie den Datei-Connector verwenden, müssen Sie zuerst eine Instanz der Datei Connector-API app erstellen. Dies kann wie folgt erfolgen:

1.  Öffnen Sie die mit der + neu Azure Marketplace Option auf der linken Seite des Portals Azure.
2.  Suchen Sie nach "Verbinder" Datei".
3.  Wählen Sie **Datei-Verbinder (Preview)** in den Suchergebnissen aus.
4.  Wählen Sie die Schaltfläche **Erstellen**
5.  Konfigurieren des Datei Verbinders wie folgt aus:  
![][1]

    - **Name** - gewähren einen Namen für den Datei-Verbinder
    - **Paketeinstellungen**
        - **Stammordner** - den Pfad des Stammordners auf Ihrem Hostcomputer angeben. Z. b. D:\FileConnectorTest
        - **Service Bus Verbindungszeichenfolge** - bieten eine Service Bus Verbindungszeichenfolge. Stellen Sie sicher, dass der Dienst Bus Namespace des Typs Standard ist und nicht für die Verwendung Service Bus Relay dürfen grundlegende.  Service Bus Relay ist Verbindung Hybrid Verbindungs-Managers verwendet.
    - **App-Serviceplan** - wählen Sie aus oder erstellen Sie einen Plan für die App-Dienst
    - **Preise Ebene** - wählen Sie eine Preisgestaltung Stufe für den Verbinder aus.
    - **Ressourcengruppe** - wählen Sie aus oder erstellen Sie eine Stelle, an der der Verbinder enthalten sein soll, Ressourcengruppe
    - **Abonnement** - wählen Sie ein Abonnement dieser Verbinder in erstellt werden soll
    - **Standort** : Wählen Sie die geografische Position, wo möchte Sie den Verbinder, um bereitgestellt werden,

4. Klicken Sie auf erstellen. Erstellt ein neue Datei Verbinder wird

## <a name="configure-hybrid-connection-manager"></a>Konfigurieren von Hybrid Verbindungs-Managers ##
Nachdem die Instanz API App erstellt wurde, navigieren Sie zu deren Dashboard.  Dies kann, indem Sie auf Durchsuchen > API Apps > Wählen Sie Ihre Datei Verbinder API App aus.  Hier muss die Hybrid-Verbindungs-Managers konfiguriert sein.
Weitere Informationen zum Konfigurieren von und Problembehandlung der Hybrid-Verbindungs-Managers finden Sie unter [Verwenden der Hybrid-Verbindungs-Manager].

## <a name="using-the-file-connector-in-your-logic-app"></a>Verwenden den Datei-Verbinder in Ihrer app Logik ##
Nachdem Ihre app API erstellt wurde, können Sie den Verbinder Datei nun als eine Aktion für Ihre app Logik verwenden. Dazu müssen Sie:

1.  Erstellen Sie eine neue Logik app, und wählen Sie die hat den Datei Verbinder derselben Ressourcengruppe. Führen Sie die Schritte zum [Erstellen einer neuen Logik app].

2.  Öffnen Sie "Trigger und Aktionen" innerhalb der erstellten Logik app der Logik apps Designer zu öffnen und Ihre Fluss konfigurieren.

3.  Der Verbinder Datei wird im Abschnitt "API Apps in dieser Ressourcengruppe" im Katalog auf der rechten Seite angezeigt.

4.  Sie können die Datei Connector-API app in Editor abgelegt werden, indem Sie auf den Verbinder"Datei". Datei Verbinder macht eine Trigger und 4 Aktionen verfügbar:  
![][5]

6.  Jeweils davon macht bestimmte Eigenschaften verfügbar. Im Bild unten Listet die Eigenschaften für das Auslösen und Datei Aktion erhalten:  
![][6]

7. Sobald diese konfiguriert sind, kann der Trigger und die Aktion in Ihrem Fluss verwendet werden. Auf ähnliche Weise können auch andere Aktionen konfiguriert werden.

> [AZURE.NOTE] Die Datei auslösen löscht die Datei aus, nachdem sie erfolgreich aus dem Ordner gelesen wurde.

## <a name="file-connector-rest-apis"></a>Datei Verbinder REST-APIs ##
Wenn den Verbinder außerhalb einer app Logik verwenden möchten, können die restlichen APIs verfügbar gemacht werden, indem Sie den Verbinder genutzt werden. Sie können diese API Definitionen mit der Funktion Blättern-Api App > Ansicht -> Datei Verbinder. Klicken Sie jetzt auf den Lens-API-Definition unter dem Abschnitt "Zusammenfassung" alle verfügbaren von diesem Connector APIs anzeigen:  
![][7]

Details der APIs finden Sie unter [Datei Connector-API-Definition].

## <a name="do-more-with-your-connector"></a>Führen Sie den Verbinder
Jetzt, da der Verbinder erstellt wurde, können Sie es mit einem Business Workflow mit einer Logik app hinzufügen. Finden Sie unter [Was Logik apps werden?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Wenn Sie mit Azure Logik apps Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [app Logik versuchen](https://tryappservice.azure.com/?appservice=logic), in dem Sie eine kurzlebige Starter Logik app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

Zeigen Sie die Swagger REST-API-Referenz bei [Verbindern und API Apps Bezug](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung von Statistiken und Steuerelement Sicherheit des Verbinders überprüfen. Finden Sie unter [Verwalten und Überwachen Ihrer integrierten API Apps und Verbinder](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Erstellen einer neuen Logik-app]: app-service-logic-create-a-logic-app.md
[Datei-Connector-API-definition]: https://msdn.microsoft.com/library/dn936296.aspx
[Mithilfe der Hybrid-Verbindungs-Managers]: app-service-logic-hybrid-connection-manager.md

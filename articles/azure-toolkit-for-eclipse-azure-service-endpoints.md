<properties
    pageTitle="Azure-Endpunkte"
    description="Beschreibt die Azure Service-Endpunkts an Einstellungen im Azure-Toolkit für "Ellipse"."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure-Endpunkte #

Azure-Endpunkte bestimmen, ob eine Anwendung auf bereitgestellt und über die globale Azure-Plattform verwaltet, Azure von 21Vianet in China oder ein privates Azure-Plattform betrieben. Im **Endpunkte** Dialogfeld können Sie angeben, welche Dienstendpunkte verwenden möchten. Zum Öffnen Sie im Dialogfeld **Dienstendpunkte** innerhalb "Ellipse", klicken Sie auf **Fenster**, klicken Sie auf **Einstellungen**, erweitern Sie **Azure**und klicken Sie dann auf **Endpunkte**. Festlegen von Feld **Aktive festlegen** bestimmt, welche Endpunkte Azure Service für die Azure Projekte in Ihrer aktuellen Arbeitsbereich verwendet werden.

Die nachstehende Abbildung zeigt das Dialogfeld **Endpunkte** aus.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Die Endpunkte festlegen ##

Führen Sie im Dialogfeld **Dienstendpunkte** eine der folgenden Aktionen aus:

* Verwenden die globale Azure-Plattform aus der Dropdownliste **Aktiv festgelegt** werden soll **windowsazure.com** wählen Sie aus, und klicken Sie auf **OK**.
* Wenn Sie von 21Vianet in China, aus der Dropdownliste **Aktiv festgelegt** , betrieben Azure verwenden möchten wählen Sie **windowsazure.cn (China)** , und klicken Sie auf **OK**.
* Wenn Sie eine private Azure-Plattform verwenden möchten:
    1. Klicken Sie auf **Bearbeiten**.
    2. Ein Dialogfeld geöffnet, die Sie darüber informiert, dass das **Endpunkte** Dialogfeld geschlossen wird, und die Einstellung legt Datei wird geöffnet. Klicken Sie auf **OK**.
    3. Klicken Sie in der Datei preferencesets.xml erstellen Sie ein neues `preferenceset` Element. Erstellen Sie für dieses neue Element, `name`, `blob`, `management`, `portalURL` und `publishsettings` Attribute und Werte für diese entsprechen, die auf Ihre privaten Azure-Plattform hinzufügen. Sie können die Werte für die vorhandene bereitgestellten `preferenceset` Elemente als Vorlagen. **Hinweis**: der Wert, für die `blob` Attribut muss den Text "Blob" in der URL enthalten.
    4. Speichern Sie und schließen Sie preferencesets.xml.
    5. Öffnen Sie im Dialogfeld **Dienstendpunkte** erneut.
    6. Wählen Sie aus der Dropdownliste **Aktiven festlegen** die aktive Menge, die Sie erstellt haben, und klicken Sie auf **OK**.
    7. Nachdem Sie Ihre private Azure-Plattform erstellt haben `preferenceset` Element, ändern Sie die Werte, indem Sie auf die Schaltfläche **Bearbeiten** , klicken Sie im Dialogfeld **Dienste-Endpunkt** zugewiesen. Sie können auch mehrere private Azure-Plattform erstellen `preferenceset` Elemente, falls gewünscht.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

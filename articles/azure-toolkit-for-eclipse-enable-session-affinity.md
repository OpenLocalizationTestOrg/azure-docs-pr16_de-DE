<properties
    pageTitle="Aktivieren Sie die Sitzung Zugehörigkeit mit dem Azure-Toolkit für "Ellipse""
    description="Erfahren Sie, wie Sitzung Zugehörigkeit mit dem Azure-Toolkit für "Ellipse" aktivieren."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Aktivieren Sie die Sitzung Zugehörigkeit #

Innerhalb der Azure-Toolkit für die Ellipse können Sie den HTTP-Sitzung Zugehörigkeit oder "Kurznotizen Sitzungen", für Ihre Rollen aktivieren. Die folgende Abbildung zeigt die **Lastenausgleich** Eigenschaften Dialogfeld verwendet, um die Funktion für die Zugehörigkeit Sitzung aktivieren:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>So aktivieren Sie die Sitzung Zugehörigkeit für Ihre Rolle ##

1. Mit der rechten Maustaste in der Rolle des "Ellipse" Projekt-Explorer, klicken Sie auf **Azure**, und klicken Sie dann auf **Den Lastenausgleich**.
1. Im Dialogfeld **Eigenschaften für WorkerRole1 den Lastenausgleich** :
    1. Aktivieren Sie **HTTP aktivieren Sitzung Zugehörigkeit (Kurznotizen Sitzungen) für diese Rolle.**
    1. **Eingabe Endpunkt verwenden**wählen Sie einen Eingaben Endpunkt an, beispielsweise **http (öffentlich: 80, Private: 8080)**verwenden. Die Anwendung muss diesen Endpunkt als deren HTTP-Endpunkt verwenden. Sie können mehrere Endpunkte für Ihre Rolle aktivieren, aber Sie können nur eine von ihnen zur Unterstützung von Kurznotizen Sitzungen auswählen.
    1. Die Anwendung neu erstellt.

Sobald aktiviert, wenn Sie mehr als eine Instanz der Rolle haben, weiterhin die HTTP-Anfragen von einem bestimmten Client, die von der gleichen Rolleninstanz bearbeitet werden.

Die Ellipse Toolkit ermöglicht dies durch eine spezielle IIS Modul mit dem Namen der Anwendung anfordern Routing (ARR) in jeder der Rolleninstanzen installieren. ARR leitet HTTP-Anfragen die geeignete Rolle Instanz. Das Toolkit konfiguriert den ausgewählten Endpunkt automatisch neu, damit die ARR Software zuerst der eingehende HTTP-Verkehr weitergeleitet wird. Das Toolkit erstellt auch einen neuen internen Endpunkt, die Ihre Java-Server ist so konfiguriert, dass Sie hören möchten. Dies ist der Endpunkt von ARR verwendet, um den HTTP-Verkehr auf die geeignete Rolleninstanz einzusetzen. Auf diese Weise fungiert jede Instanz der Rolle in der Bereitstellung mit mehreren Instanzen als reverse Proxy für alle anderen Instanzen Kurznotizen Sitzungen aktivieren.

## <a name="notes-about-session-affinity"></a>Hinweise zu Sitzung Zugehörigkeit ##

* Sitzung Zugehörigkeit funktioniert nicht in der Serveremulator. Die Einstellungen in der Serveremulator angewendet werden können, ohne zu stören Prozesses erstellen oder Emulator Ausführung zu berechnen, aber das Feature selbst funktioniert nicht innerhalb der Serveremulator.
* Aktivieren die Sitzung Zugehörigkeit führt zu Fehlern im einer Erhöhung Speicherplatz als zusätzlicher Software heruntergeladen und installiert in Ihre Rolleninstanzen beim Starten des Diensts in der Cloud Azure ist von der Bereitstellung in Azure, beanspruchen.
* Die Zeit für jede Rolle Initialisierung dauert länger.
* Ein interner Endpunkt, funktioniert als eine Rerouter Datenverkehr wie bereits erwähnt, wird added.ss sein.

Ein Beispiel für die Sitzungsdaten beibehalten werden soll, wenn Sitzung Zugehörigkeit aktiviert ist finden Sie unter [Verwalten von Sitzung Daten mit Sitzung Zugehörigkeit][].

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

[So Sitzungsdaten mit Sitzung Zugehörigkeit zu verwalten][]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[So Sitzungsdaten mit Sitzung Zugehörigkeit zu verwalten]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<properties
    pageTitle="Microsoft Azure-Portal (Übersicht)"
    description="Erfahren Sie, wie das Microsoft Azure-Portal zu verwenden."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Microsoft Azure-Portal (Übersicht)

Microsoft Azure-Portal ist eine zentrale Stelle, wo Sie bereitstellen und Verwalten von Azure Ressourcen können.  In diesem Lernprogramm wird vertraut machen Sie mit dem Portal und gezeigt, wie Sie einige der folgenden wichtigen Funktionen verwenden:
- Eine **umfassende Marketplace** , mit dem Sie Tausende von Elementen von Microsoft und anderen Anbietern, die gekauft und/oder nach der Bereitstellung durchsuchen.
- Ein **einheitliches und skalierbare durchsuchen-Benutzeroberfläche** , die die Ressourcen finden, die Sie interessieren und verschiedene Management Vorgänge durchführen, erleichtert.
- **Konsistente Management-Seiten** (oder Blades), mit denen Sie Verwalten der Azure-Vielzahl von Diensten über ein konsistentes Verfügbarmachen von Einstellungen, Aktionen, Abrechnung Informationen, Gesundheit Überwachung und Verwendung von Daten und vieles mehr.
- Eine **Persönliche Erfahrung** , mit dem Sie eine angepasste Startbildschirm mit den Informationen angezeigt werden, den immer angezeigt werden sollen, erstellen Sie angemeldet.  Sie können auch eine Management Blade anpassen, die Kacheln enthalten.

 ![Azure Portals Benutzeroberfläche Ausrichtung][UIOrientation]

## <a name="before-you-get-started"></a>Bevor Sie beginnen

Sie benötigen ein gültiges Azure-Abonnement für dieses Lernprogramm durchgehen.  Wenn Sie eine, dann [Melden Sie sich für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) heute besitzen.  Nachdem Sie ein Abonnement besitzen, können Sie das Portal unter [https://portal.azure.com] zugreifen.

## <a name="how-to-create-a-resource"></a>So erstellen Sie eine Ressource

Azure verfügt über eine Marketplace mit Tausende von Elementen, die Sie an einem Ort erstellen können.  Angenommen, Sie einen neuen Windows Server 2012 virtuellen Computer erstellen möchten.  Die + neue Hub ist der Einstiegspunkt in eine Reihe curated bereitgestellten Kategorien aus dem Marketplace.  Jede Kategorie enthält eine kleine Gruppe von bereitgestellten Elemente zusammen mit einem Link zum vollständigen Marketplace, der allen Kategorien und eine Suche anzeigt. Um diesen neuen Windows Server 2012 virtuellen Computer erstellen möchten, führen Sie die folgenden Aktionen aus:  

1.  Windows Server 2012 wird empfohlen, damit Sie ihn aus der Kategorie berechnen auswählen können.  
2.  Füllen Sie einige grundlegenden Eingaben in einem Formular aus.
3.  Klicken Sie auf 'Erstellen', und Ihre virtuellen Computer beginnt mit dem unmittelbar bereitstellen.

Der Benachrichtigungen Hub informiert, wenn sich die Ressource eingerichtet wurde und eine Blade Management wird geöffnet (Sie können immer durchsuchen, um Ressourcen weiter unten).

![Portal Kategorien][PortalCategories]


## <a name="how-to-find-your-resources"></a>So suchen Sie Ihre Ressourcen

Sie können häufig verwendete Ressourcen immer auf Ihre Startboard anheften, aber möglicherweise müssen Sie, um einen Beitrag zu suchen, die Sie häufig zugreifen.  Im unten gezeigten durchsuchen-Hub ist zurecht, um alle Ihre Ressourcen zu gelangen.  Sie können nach Abonnement filtern, wählen Sie/Größenänderungs-Spalten, und navigieren Sie zu der Management-Blades, indem Sie auf einzelne Elemente.

![Navigieren Sie Hub][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Zum Verwalten und Zugriffsrechte für Stellvertretung für eine Ressource

Überwachen Sie aus diesem Blade virtuellen Computers mithilfe von Remotedesktop herstellen können der wichtigsten Kennzahlen, steuern Sie des Zugriffs auf diesem virtuellen Computer rollenbasierte Access (RBAC) verwenden, konfigurieren Sie den virtuellen Computer und führen Sie andere wichtige Verwaltungsaufgaben aus.  Delegieren der Zugriff auf Grundlage der Rolle ist entscheidend, bei verwalten.  Klicken Sie auf [hier](./active-directory/role-based-access-control-configure.md) erfahren Sie mehr über diese. Führen Sie die folgenden Aktionen aus, um Zugriff auf eine Ressource delegieren:

1.  Navigieren Sie zu der Ressource.
2.  Klicken Sie auf 'Alle Einstellungen' im Abschnitt Essentials.
3.  Klicken Sie auf 'Benutzer' in der Einstellungsliste.
4.  Klicken Sie auf 'Hinzufügen' in der Befehlsleiste.
5.  Wählen Sie einen Benutzer und eine Rolle aus.

![Verwalten einer Ressource][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>So passen Sie eine Ressource blade

Azure preconfigures die Blades für Ressourcen, aber die Kacheln auf diese Karten sind ähnliches zu steuern.  Sie können ganz einfach wechseln in anpassen Modus zum Hinzufügen, entfernen, Ändern der Größe oder die Kacheln neu anordnen. Zum Anpassen einer Blades führen Sie die folgenden Aktionen aus:

1.  Navigieren Sie zu der Ressource.
2.  Klicken Sie auf der "..." am oberen Rand der Blade, den Sie anpassen möchten.
3.  Klicken Sie auf 'Webparts hinzufügen'.
4.  Ziehen und Ablegen von Webparts zu starten.  

![Anpassen von Blades][CustomizeBlades]

## <a name="how-to-get-help"></a>So erhalten Sie Hilfe

Wenn Sie ein Problem jemals verfügen, können wir für Sie hier.  Das Portal verfügt über eine Hilfe und Support-Seite, die Sie in die richtige Richtung verweisen kann.  Abhängig von Ihrem [Plan unterstützen](https://azure.microsoft.com/support/plans/)können Sie auch supporttickets direkt im Portal erstellen.  Nach dem Erstellen einer Support-Ticket, können Sie den Lebenszyklus des Tickets aus innerhalb des Portals verwalten. Gelangen Sie in der Hilfe und Unterstützung + Hilfe -> Supportseite durch Navigieren zu durchsuchen.  

![Hilfe und support][HelpSupport]

## <a name="summary"></a>Zusammenfassung

Sehen wir uns bisher Gelernten in diesem Lernprogramm:
- Sie erfahren, wie registrieren, erhalten ein Abonnement, und navigieren Sie zu dem portal
- Sie haben Sie mit dem Portal Benutzeroberfläche ausgerichtet und gelernt erstellen und Durchsuchen von Ressourcen
- So erstellen Sie eine Ressource aus, und navigieren Sie Ressourcen gelernten
- Haben Sie Informationen über die Struktur oder Management Blades und wie Sie verschiedene Arten von Ressourcen konsistent verwalten können
- Sie erfahren, wie Sie zum Anpassen des Portals, um die Informationen zu bringen Sie darauf zu, dass die Vorderseite und zentrieren
- Sie erfahren, wie zum Steuern des Zugriffs auf Ressourcen mit rollenbasierte Access (RBAC)
- So erhalten Sie Hilfe und Support gelernten

Microsoft Azure-Portal vereinfacht grundlegend erstellen und Verwalten von Ihrer Anwendung in der Cloud.  Sehen Sie sich den [Blog Management](https://azure.microsoft.com/blog/topics/management/) wie wir ständig [anhören von Feedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) und dessen Verbesserung sind auf dem neuesten Stand beibehalten.  [Blog des ScottGu](http://weblogs.asp.net/scottgu) handelt es sich um eine andere hervorragende Informationen für alle Azure-Updates suchen.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png

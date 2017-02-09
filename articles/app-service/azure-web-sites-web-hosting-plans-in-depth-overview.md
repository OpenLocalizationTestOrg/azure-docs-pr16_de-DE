<properties
    pageTitle="Azure-App-Service-Pläne genaue Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie App Service für die Arbeit mit Azure-App-Verwaltungsdienst-Pläne und deren Vorteile für Ihre Verwaltungsoption."
    keywords="App Dienst Azure app Dienst skalieren skalierbare, app-Serviceplan app Dienst Kosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure-App-Service-Pläne genaue (Übersicht)#

Eine App Serviceplan stellt eine Reihe von Features und Kapazität, die Sie über mehrere apps freigeben können. Web Apps, Mobile-Apps, Apps-Funktion oder API Apps, in der [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714) alle in einer App-Serviceplan ausgeführt wird. Diese Pläne unterstützen fünf Preisgestaltung Ebenen: *frei*, *freigegeben*, *Basic*, *Standard*und *Premium*. Jede Ebene verfügt über eine eigene Funktionen und Kapazität. In der gleichen Abonnement und geographischen Standort Apps können einen Plan freigeben. Alle apps, die einen Plan gemeinsam nutzen können die Funktionen und Features, die von den Plan des Ebene definiert werden. Alle apps, die einen Plan zugeordnet sind, führen Sie auf den Ressourcen, die der Plan definiert.

Angenommen, wenn Ihr Plan für die Verwendung von zwei "kleiner" Instanzen in der Dienstebene standard-konfiguriert ist, alle apps, die mit diesem Plan verknüpft sind beide Instanzen ausgeführt und haben Zugriff auf die standard-Service-Funktionen Ebene. Plan Instanzen auf denen apps ausführen, sind vollständig verwaltete und hochgradig verfügbar.

In diesem Artikel wird erläutert, die Hauptmerkmale, z. B. Ebene und Skalieren einer App-Serviceplan und wie sie in der Wiedergabe beim Verwalten Ihrer apps stammen.

## <a name="apps-and-app-service-plans"></a>Apps und App-Service-Pläne

Eine app im App-Dienst kann nur eine App Serviceplan zu einem beliebigen Zeitpunkt zugeordnet werden.

Sowohl apps und Pläne sind in einer Ressourcengruppe enthalten. Eine Ressourcengruppe dient als die Begrenzungslinie Lebenszyklus für alle Ressourcen, die es enthalten ist. Ressourcengruppen können Sie alle Teile der Anwendung zusammen verwalten.

Da eine einzelne Ressourcengruppe mehrere App Dienstpläne enthalten kann, können Sie andere apps für unterschiedliche physische Ressourcen zuordnen. Beispielsweise können Sie Ressourcen für Entwickler, testen und Herstellung Umgebungen trennen. Separate Umgebungen für Herstellung und Test-/Probleme können Sie Ressourcen isolieren. Auf diese Weise nimmt laden Tests mit einer neuen Version Ihrer Apps nicht dieselben Ressourcen wie Ihre apps Herstellung in Anspruch der tatsächliche Kunden bedienen.

Wenn Sie mehrere Pläne in einem einzigen Ressourcengruppe verfügen, können Sie auch eine Anwendung definieren, die Ländern / Regionen umfasst. Beispielsweise enthält eine hoch verfügbare app in zwei Bereiche ausgeführt mindestens zwei Pläne, eine für die einzelnen Regionen und eine app mit jeder Plan verknüpft ist. In diesem Fall sind alle Kopien der app klicken Sie dann in einer einzigen Ressourcengruppe enthalten. Probleme mit mehreren Pläne und mehrere apps eine Ressourcengruppe vereinfacht das verwalten, steuern und die Integrität der Anwendung anzeigen.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Erstellen Sie eines App-Serviceplan oder verwenden vorhandenes zu

Wenn Sie eine app erstellen, sollten Sie eine Ressourcengruppe erstellen. Andererseits, wenn die app, die Pfeile zum Erstellen einer Komponente für eine größere Anwendung ist, sollte diese app innerhalb der Ressourcengruppe erstellt werden, der für die Anwendung größere zugeordnet wird.

Ob die neue app einer ganz neuen Anwendung oder einen Teil eine größere ist, können Sie mit einer vorhandenen App Serviceplan zu hosten oder einen neuen erstellen auswählen. Diese Entscheidung ist mehr eine Frage Kapazität und erwarteten.

Wenn diese neue app ist zu viele Ressourcen verwenden und verschiedene Faktoren aus anderen apps Skalierung gehostet in einen vorhandenen Plan haben, wird empfohlen, dass Sie es in einem eigenen Plan isolieren.

Wenn Sie einen Plan erstellen, können Sie einen neuen Satz von Ressourcen für Ihre app reservieren und bessere Kontrolle über die Zuweisung von Ressourcen erhalten, da jeder Plan einen eigenen Satz von Instanzen erhält.

Da Sie apps über Pläne verschieben können, können Sie die Methode ändern, die über die Anwendung vergrößern Ressourcen reserviert werden.

Schließlich, wenn Sie eine app in einem anderen Bereich erstellen möchten, und die Region verfügt nicht über einen vorhandenen Plan, erstellen Sie einen Plan in diesem Bereich werden sollen, Ihre app es hosten.

## <a name="create-an-app-service-plan"></a>Erstellen eines App-Serviceplan

>[AZURE.TIP] Wenn Sie eine App-Service-Umgebung haben können, überprüfen Sie in Ihrer Dokumentation App Dienst Umgebungen hier: [Erstellen einer App Dienst planen in einer App-Service-Umgebung](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Sie können einer leeren App Serviceplan durchsuchen mit der App-Service-Plan oder als Teil der Erstellung der app erstellen.

Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **neu** > **Web + Mobile**, und wählen Sie dann **Web App** oder eine andere Art der App-Service-app.
![Erstellen einer app im Azure-Portal an.][createWebApp]

Dann können Sie den App-Serviceplan für die neue app erstellen oder auswählen.

 ![Erstellen eines App-Serviceplan an.][createASP]

Um eine neue App-Serviceplan zu erstellen, klicken Sie auf **[+] neu erstellen**, geben Sie den Namen der **App-Serviceplan** , und wählen Sie dann auf eine geeignete **Stelle**. Klicken Sie auf die **Preise Ebene**, und wählen Sie einen entsprechenden Preisgestaltung Ebenen für den Dienst. Wählen Sie **Ansicht alle** Weitere Optionen Preisgestaltung, wie z. B. **Free** und **freigegebene**anzeigen. Nachdem Sie die Preise Ebene ausgewählt haben, klicken Sie auf die Schaltfläche **auswählen** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Verschieben einer app zu einer anderen App Serviceplan

Sie können eine app zu einer anderen app-Serviceplan im [Portal Azure](https://portal.azure.com)verschieben. Sie können die apps zwischen Pläne verschieben, solange die Pläne in derselben Ressourcengruppe und geografische Region sind.

Wenn eine app in einen anderen Plan verschieben möchten, wechseln Sie zu der app, die Sie verschieben möchten. Suchen Sie nach der **Änderung App Dienst ausführen möchten**, im Menü **Einstellungen** .

**App-Serviceplan ändern** wird die **App-Serviceplan** Ansichtsauswahl geöffnet. An diesem Punkt können Sie entweder wählen Sie einen vorhandenen Plan aus, oder Erstellen eines neuen Kontos. Nur gültige Pläne (in der gleichen Ressourcengruppe und geographischen Standort) werden angezeigt.

![App-Dienst Plan Ansichtsauswahl.][change]

Jeder Plan verfügt über eine eigene Ebene Preise. Beispielsweise beim Verschieben einer Website aus einer kostenlosen Ebene in einer Standardansicht Ebene, die app jetzt können die Features und Ressourcen der Standard-Stufe.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonen einer app zu einer anderen App Serviceplan
Wenn Sie die app in einen anderen Bereich verschieben möchten, ist eine Alternative app klonen. Klonen, wird eine Kopie der app in einer neuen oder vorhandenen App Serviceplan oder App-Service-Umgebung, die in jeder Region.

 ![Duplizieren einer app.][appclone]

**Klonen App** finden Sie im Menü **Extras** .

Klonen gelten einige Einschränkungen, die Sie bei [Azure App Dienst App Klonen mit Azure-Portal](../app-service-web/app-service-web-app-cloning-portal.md)zu lesen können.

## <a name="scale-an-app-service-plan"></a>Eine App Serviceplan skalieren

Es gibt drei Methoden zum Skalieren eines Plans aus:

- **Ändern des Plans Preise Ebene**. Beispielsweise ein Plan in die grundlegende Ebene in eine Stufe Standard oder Premium konvertiert werden kann, und alle apps, die nun die Plan zugeordnet sind, können die Funktionen, die der neuen Dienst Stufe bietet.
- **Ändern des Plans Instanzgröße**. Beispielsweise kann ein Plan in der grundlegenden Ebene, die kleine Instanzen verwendet geändert werden, um große Instanzen verwenden. Alle apps, die mit diesem Plan verknüpft jetzt sind können mehr Arbeitsspeicher und CPU-Ressourcen, die das größere Instanz bietet.
- **Ändern der Anzahl der Instanzen des Plans**. Beispielsweise kann ein Standard-Plan, der sich auf drei Instanzen skaliert wird auf 10 Instanzen skaliert werden. Ein Plan Premium kann auf 20 Instanzen (unterliegen Verfügbarkeit) skaliert werden. Alle apps, die mit diesem Plan verknüpft jetzt sind können mehr Arbeitsspeicher und CPU-Ressourcen, die die Anzahl der Instanzen größere bietet.

Sie können die Preisgestaltung Ebene und Instanz-Größe ändern, indem Sie auf das Element **Skalieren** unter Einstellungen für die app oder der App-Serviceplan. Änderungen anwenden auf der App-Serviceplan und Einfluss auf alle apps, die es hostet.

 ![Legen Sie die Werte in einer app zu skalieren.][pricingtier]

## <a name="app-service-plan-cleanup"></a>App-Dienst planen zum Aufräumen
**App-Service-Pläne** , die keine apps verknüpft ist, können weiterhin fallen Gebühren, da sie weiterhin die so konfiguriert, dass die App-Dienst Plan Maßstab Eigenschaften berechnen Kapazität reservieren.
Zur Vermeidung von unerwartete Gebühren, wenn die letzte app in einer App-Serviceplan gehostet wird gelöscht wird, wird der resultierende leeren App Serviceplan ebenfalls gelöscht.


## <a name="summary"></a>Zusammenfassung

App-Service-Pläne stehen für eine Reihe von Features und Kapazität, die Sie in Ihrer apps freigeben können. App-Service-Pläne bieten Ihnen die Flexibilität, bestimmte apps für eine Reihe von Ressourcen zuordnen und weiteren Optimierung Ihrer Azure Ressource Nutzung. Diese Methode, wenn Geld auf Ihrer Umgebung testen gespeichert werden sollen können Sie einen Plan über mehrere apps freigeben. Sie können auch für Ihre Umgebung Herstellung liegenden durch über mehrere Regionen und Pläne skalieren.

## <a name="whats-changed"></a>Was hat sich geändert

* Einen Leitfaden zur Änderung von Websites-App-Dienst, finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png

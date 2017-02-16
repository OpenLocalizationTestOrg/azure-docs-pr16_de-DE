<properties
   pageTitle="Hintergrund Aufträge Anleitungen | Microsoft Azure"
   description="Leitfaden zum Hintergrund Vorgänge dieser ausführen, unabhängig von der Benutzeroberfläche."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Hintergrund Aufträge Anleitungen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>(Übersicht)

Viele Arten von Applications erfordern Hintergrundaufgaben, die unabhängig von der Benutzeroberfläche (UI) ausgeführt werden. Beispiele für Stapelverarbeitung, stark Verarbeitungsaufgaben und langer Prozesse wie Workflows. Hintergrundaufträge ausgeführt werden können, ohne Interaktion mit dem Benutzer – die Anwendung startet den Auftrag, und fahren Sie mit interaktive Anfragen von Benutzern bearbeitet werden können. Dies kann dazu beitragen die Last auf der Benutzeroberfläche, die Anwendung minimieren die Verfügbarkeit verbessern und interaktive Reaktionszeiten verringern können.

Wenn eine Anwendung erforderlich ist, um Miniaturansichten von Bildern zu generieren, die von Benutzern hochgeladen werden, kann es beispielsweise Aktion im Hintergrund und die Miniaturansicht Speicher speichern nach Abschluss – ohne dass der Benutzer benötigen warten, für den Vorgang abgeschlossen sein soll. Auf die gleiche Weise kann ein Benutzer eine Bestellung aufgeben einen Hintergrund Workflow initiieren, der die Reihenfolge, verarbeitet die Benutzeroberfläche kann den Benutzer weiterhin Durchsuchen des Web app. Wenn das Hintergrund Projekt abgeschlossen ist, können sie die Bestellungen gespeicherten Daten zu aktualisieren und Senden einer e-Mail an den Benutzer, der die Reihenfolge bestätigt.

Wenn Sie überlegen, ob eine Aufgabe als Hintergrundauftrag implementiert wird, die wichtigsten Kriterien ist, ob der Vorgang ohne Interaktion des Benutzers und ohne die Benutzeroberfläche ausführen kann benötigen warten, für das Projekt abgeschlossen sein soll. Aufgaben, die der Benutzer oder der Benutzeroberfläche, warten sie abgeschlossen sind, erfordern möglicherweise nicht als Hintergrundaufträge geeignet.

## <a name="types-of-background-jobs"></a>Typen von Aufträgen Hintergrund

Hintergrundaufträge beinhalten in der Regel eine oder mehrere der folgenden Typen von Aufträgen:

- CPU-Auslastung Aufträge, wie z. B. mathematische Berechnungen oder strukturelle Modell Datenanalyse.
- Ich/O-Auslastung Aufträge, wie z. B. Ausführen einer Reihe von Speichertransaktionen oder das Indizieren von Dateien.
- Stapelverarbeitungsaufträge, wie z. B. jede Nacht Datenupdates oder geplanten Verarbeitung.
- Langer Workflows, wie z. B. Erfüllung oder Dienste und Systeme bereitgestellt.
- Sensible Daten Verarbeitung, in dem die Aufgabe an einem sicherer Speicherort für die Verarbeitung erfüllt ist. Möglicherweise möchten Sie beispielsweise nicht verarbeiten sensible Daten innerhalb einer Web app. Stattdessen möglicherweise Sie ein Muster wie [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) verwenden, um die Daten in einem Hintergrundprozess isoliert übertragen, die auf geschützten Speicher zugreifen kann.

## <a name="triggers"></a>Trigger

Hintergrundaufträge können auf verschiedene Arten initiiert werden. Sie können in eine der folgenden Kategorien:

- [**Trigger ereignisgesteuerten**](#event-driven-triggers). Die Aufgabe wird als Antwort auf ein Ereignis in der Regel eine Aktion von einem Benutzer oder einen Schritt in einem Workflow gestartet.
- [**Trigger Terminplan leistungsgesteuert**](#schedule-driven-triggers). Der Vorgang ist für einen Zeitplan basierend auf einem Zeitgeber aufgerufen. Dies möglicherweise einen periodischen Zeitplan oder einer einmaligen aufrufen, die für einen späteren Zeitpunkt angegeben ist.

### <a name="event-driven-triggers"></a>Ereignisgesteuerten Trigger

Ereignisgesteuerten Aufrufen verwendet ein Trigger, um den Hintergrund starten. Beispiele für die Verwendung der Trigger ereignisgesteuerten umfassen:

- Die Benutzeroberfläche oder einer anderen Stelle platziert eine Nachricht in einer Warteschlange. Die Nachricht enthält Daten über eine Aktion, die, wie der Benutzer eine Bestellung aufgeben stattgefunden hat. Der Vorgang Hintergrund überwacht diese Warteschlange und erkennt das Eintreffen einer neuen Nachricht. Die Nachricht liest und die Daten darin als Eingabe für den Hintergrundauftrag verwendet.
- Die Benutzeroberfläche oder einer anderen Stelle speichert oder ein Werts in Speicher aktualisiert werden. Der Vorgang Hintergrund überwacht die Speicherung und Änderungen erkennt. Liest die Daten, und es als Eingabe für den Hintergrundauftrag verwendet.
- Die Benutzeroberfläche oder einer anderen Stelle führt eine Anforderung an den Endpunkt, wie etwa einen HTTPS-URI oder eine API, die als Webdienst verfügbar gemacht wird. Die Daten, die erforderlich ist, führen Sie die Hintergrundaufgabe als Teil der Anforderung übergeben. Der Endpunkt oder Web-Dienst Ruft die Hintergrundaufgabe, die die Daten als Eingabe verwendet.

Typische Beispiele für Aufgaben, die zum Aufrufen ereignisgesteuerten eignen einschließen Bild Verarbeitung, Workflows, Informationen zu remote Services senden, e-Mail-Nachrichten senden und neue Benutzer in Clientanwendungen mandantenfähigen bereitgestellt.

### <a name="schedule-driven-triggers"></a>Zeitplan leistungsgesteuert Trigger

Zeitplan leistungsgesteuert verwendet einen Zeitgeber zum Starten des Hintergrundtasks. Beispiele für die Verwendung der Terminplan leistungsgesteuert Trigger umfassen:

- Ein Zeitgeber, der lokal in der Anwendung oder als Teil der Anwendung: das Betriebssystem ausgeführt wird, ruft einen Hintergrund Vorgang in regelmäßigen Abständen.
- Ein Zeitgeber, der in eine andere Anwendung oder eine Timer-Dienst wie Azure-Scheduler ausgeführt wird sendet eine Anforderung an einen API oder Web-Dienst in regelmäßigen Abständen. Der Dienst-API oder Web Ruft die Hintergrundaufgabe.
- Einen separaten Prozess oder einer Anwendung gestartet wird einen Zeitgeber, der bewirkt, dass die Hintergrundaufgabe einmal nach einer bestimmten Zeit Verzögerung oder zu einem bestimmten Zeitpunkt aufgerufen werden.

Typische Beispiele für Aufgaben, die für Terminplan leistungsgesteuert aufrufen geeignet sind: Stapelverarbeitung Routinen (z. B. aktualisieren Zusammenhang Produkte Listen für Benutzer, basierend auf deren zuletzt verwendete Verhalten), eine routinemäßige Datenverarbeitung Aufgaben (z. B. Aktualisieren von Indizes oder direkte kumulierten Ergebnisse), Datenanalyse für tägliche Berichte, Daten Aufbewahrung Aufräumen und Daten Konsistenz überprüft.

Wenn Sie einen Vorgang leistungsgesteuert Terminplan, der als einzelne Instanz ausgeführt werden müssen verwenden, beachten Sie die folgenden Aktionen aus:

- Wenn die berechnen Instanz, für die Ausführung der Scheduler (beispielsweise eine am virtuellen Computer mit Windows geplanten Vorgängen) skaliert wird, haben Sie mehrerer Instanzen von der Scheduler ausgeführt. Diese konnte mehrerer Instanzen des Vorgangs starten.
- Wenn Vorgänge länger als der Zeitraum zwischen Scheduler Ereignissen ausführen zu können, kann der Scheduler einer anderen Instanz des Vorgangs während der Ausführung der vorherigen Folie starten.

## <a name="returning-results"></a>Zurückgeben von Ergebnissen

Hintergrund Einzelvorgänge ausführen asynchrone in einem separaten Prozess oder auch in einem anderen Speicherort, von der Benutzeroberfläche oder den Prozess, der die Hintergrundaufgabe aufgerufen. Idealerweise Hintergrundaufgaben sind "Auslösen und vergessen" Vorgänge und deren Status unter Datenausführungsverhinderung wirkt sich nicht auf der Benutzeroberfläche oder der einen Prozess. Dies bedeutet, dass der Prozess einen Abschluss der Aufgaben nicht warten muss. Daher kann nicht automatisch erkennen, wenn der Vorgang endet.

Wenn Sie einen Hintergrund Vorgang zur Kommunikation mit dem einen Vorgang an den Fortschritt oder Abschluss erforderlich ist, müssen Sie ein Verfahren für diese implementieren. Einige Beispiele für sind:

- Schreiben Sie einen Wert des Status Indikator in Speicher, die für den Vorgang Benutzeroberfläche oder Anrufer zugänglich ist die überwachen und überprüfen diesen Wert bei Bedarf können. Andere Daten, die der Hintergrund Vorgang werden, um den Anrufer zurückgegeben muss können in denselben Speicher platziert werden.
- Richten Sie eine Antwortwarteschlange, der die Benutzeroberfläche oder die Anrufer überwacht. Der Vorgang Hintergrund kann Nachrichten in der Warteschlange, die angeben, Status und Fertigstellung senden. Daten, die der Hintergrund Vorgang werden, um den Anrufer zurückgegeben muss können in die Nachrichten platziert werden. Wenn Sie Azure-Dienstbus verwenden, können Sie die Eigenschaften **ReplyTo** und **CorrelationId** verwenden, diese Funktion implementiert wird. Weitere Informationen finden Sie unter [Korrelationskoeffizienten in Service Bus vermittelte Messaging](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Verfügbar machen Sie eine API oder einen Endpunkt aus dem Hintergrund Vorgang, den die Benutzeroberfläche oder die Anrufer zugreifen können, um Statusinformationen zu erhalten. Daten, die der Hintergrund Vorgang werden, um den Anrufer zurückgegeben muss können in der Antwort enthalten sein.
- Sollen Sie Hintergrund Rückruf an die Benutzeroberfläche oder die Anrufer über eine API Status an vordefinierten Punkten oder Abschluss angeben. Dies möglicherweise über lokal ausgelöste Ereignisse oder über ein Verfahren zum Veröffentlichen und abonnieren. Daten, die der Hintergrund Vorgang werden, um den Anrufer zurückgegeben muss können in der Besprechungsanfrage oder eines Ereignisses Nutzlast aufgenommen werden.

## <a name="hosting-environment"></a>Hostumgebung

Sie können Hintergrundaufgaben hosten, indem Sie eine Reihe von anderen Azure-Plattform Services verwenden:

- [**Azure Web Apps und WebJobs**](#azure-web-apps-and-webjobs). WebJobs können Sie benutzerdefinierte Aufträge basierend auf einen Bereich von verschiedenen Arten von Skripts oder ausführbare Programme innerhalb des Kontexts von einer Web app ausführen.
- [**Web und Worker Azure Cloud Services-Rollen**](#azure-cloud-services-web-and-worker-roles). Sie können Code innerhalb einer Rolle schreiben, die im Hintergrund ausgeführt wird.
- [**Azure-virtuellen Computern**](#azure-virtual-machines). Wenn Sie einen Windows-Dienst haben oder im Windows-Taskplaner verwenden möchten, ist es üblich, Ihre Hintergrundaufgaben innerhalb eines dedizierten virtuellen Computers zu hosten.
- [**Azure Stapel**](./batch/batch-technical-overview.md). Es ist ein Plattformdienst, der in geplant rechenintensiver Arbeit für eine verwaltete Gruppe von virtuellen Computern ausgeführt werden, und kann automatisch skalieren berechnen von Aufträgen Anforderungen Ressourcen.

In den folgenden Abschnitten wird jede dieser Optionen detailliert beschrieben, und einschließen Aspekte, damit Sie die entsprechende Option auswählen können.

## <a name="azure-web-apps-and-webjobs"></a>Azure Web Apps und WebJobs

Azure WebJobs können Sie benutzerdefinierte Aufträge als innerhalb einer Azure Web App Hintergrundaufgaben ausführen. Führen Sie WebJobs innerhalb des Kontexts der Web app als fortlaufender Prozess. WebJobs werden auch Reaktion auf ein auslösendes Ereignis aus Azure Scheduler oder externe Faktoren wie Änderungen an Speicher Blobs und Nachrichten ausgeführt. Aufträge können Schritte und bei Bedarf beendet werden, und fahren Sie ordnungsgemäß. Wenn eine kontinuierlich laufenden WebJob fehlschlägt, wird es automatisch neu gestartet. Wiederholen und Fehler Aktionen werden konfiguriert.

Wenn Sie eine WebJob konfigurieren:

- Wenn Sie den Auftrag eines ereignisgesteuerten Trigger antworten möchten, sollten Sie es als **fortlaufend ausführen**konfigurieren. Das Skript oder Programm wird in den Ordner mit dem Namen Website/Wwwroot/App_data/Aufträge/fortlaufender gespeichert.
- Wenn Sie den Auftrag eines Triggers Terminplan leistungsgesteuert antworten möchten, sollten Sie es als **ausführen, klicken Sie auf einen Zeitplan**konfigurieren. Das Skript oder Programm wird in den Ordner mit dem Namen Website/Wwwroot/App_data/Aufträge/ausgelöst wurde gespeichert.
- Wenn Sie die Option **bei Bedarf ausführen** auswählen, wenn Sie ein Projekt konfigurieren, wird es beim Starten den gleichen Code als die Option **nach einem Zeitplan ausführen** ausgeführt.

Führen Sie Azure WebJobs innerhalb der Sandkastenmodus des Web app. Dies bedeutet, dass sie Zugriff auf Umgebungsvariablen und Freigeben von Informationen, wie z. B. Verbindungszeichenfolgen, für das Web app können. Der Auftrag hat Zugriff auf den eindeutigen Bezeichner des Computers, der den Auftrag ausgeführt wird. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsStorage** stellt Zugriff auf Warteschlangen Azure-Speicher, Blobs und Tabellen für die Anwendungsdaten und Zugriff auf Dienstbus für messaging und Kommunikation. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsDashboard** ermöglicht den Zugriff auf die Protokolldateien Auftrag Aktion.

Azure WebJobs weisen die folgenden Merkmale auf:

- **Sicherheit**: WebJobs durch die Bereitstellung Anmeldeinformationen des Web app geschützt werden.
- **Unterstützte Dateitypen**: Sie können WebJobs mithilfe des Befehls-Skripts (.cmd), Stapelverarbeitungsdateien (bat), PowerShell-Skripts (ps1) definieren, bash Shell-Skripts (.sh), PHP Skripts (PHP), Python Skripts (.py), JavaScript-Code (JS) und ausführbare Programme (.exe, .jar und mehr).
- **Bereitstellung**: Bereitstellen von Skripts und ausführbare Dateien mithilfe des Azure-Portals mithilfe des [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) -add-Ins für Visual Studio oder [Visual Studio 2013 Update 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (Sie erstellen und bereitstellen können mit dieser Option), mit dem [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)oder indem Sie sie direkt an den folgenden Speicherorten kopieren:
  - Ausgelöst wurde für die Ausführung: Website/Wwwroot/App_data/Aufträge/ausgelöst wurde / {job Name}
  - Für kontinuierliche Ausführung: Website/Wwwroot/App_data/Aufträge/fortlaufender / {job Name}
- **Protokollierung**: Console.Out wird als Informationen (markiert) behandelt. Console.Error wird als Fehler behandelt. Sie können die Überwachung und Diagnose Informationen mithilfe des Azure-Portals zugreifen. Sie können die Protokolldateien direkt von der Website herunterladen. Sie sind an folgenden Speicherorten gespeichert:
  - Ausgelöst wurde für die Ausführung: Vfs/Daten/Aufträge/ausgelöst wurde/JobName
  - Für kontinuierliche Ausführung: Vfs/Daten/Aufträge/fortlaufender/JobName
- **Konfiguration**: Sie können mithilfe des Portals, REST-API und PowerShell WebJobs konfigurieren. Eine Konfigurationsdatei mit dem Namen settings.job im gleichen Stammverzeichnis als das Skript Position können Sie die von Konfigurationsinformationen für ein Projekt bereitstellen. Beispiel:
  - {"Stopping_wait_time": 60}
  - {"Is_singleton": WAHR}

### <a name="considerations"></a>Aspekte

- Standardmäßig skalieren WebJobs mit der Web-app. Sie können jedoch Aufträge, die auf einzelnen Instanz ausgeführt werden soll, indem Sie die Eigenschaft **Is_singleton** Konfiguration auf **true**konfigurieren. Einzelne Instanz WebJobs sind nützlich für Aufgaben, die nicht zu skalieren oder gleichzeitiges Vielfachen ausführen sollen, Instanzen, wie etwa neu rekonstruieren, Datenanalyse und ähnliche Aufgaben.
- Um den Einfluss der Aufträge auf die Leistung des Web app zu minimieren, erwägen Sie das Erstellen einer leeren Azure Web App-Instanz für die stark in einem neuen App Serviceplan Host WebJobs, die möglicherweise lange ausgeführt oder einer Ressource.

### <a name="more-information"></a>Weitere Informationen

- Listen [Azure WebJobs empfohlen Ressourcen](./app-service-web/websites-webjobs-resources.md) der vielen nützlichen Ressourcen, Downloads und Beispielen für WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloud Services-Web und Worker Rollen.

Sie können die Hintergrundaufgaben innerhalb einer Webrolle oder in einer separaten Arbeiter Rolle ausführen. Wenn Sie bei der Entscheidung sind, verwenden Sie eine Worker-Rolle, erwägen Sie Skalierbarkeit und Elastizität Anforderungen, Aufgabe Lebensdauer, lassen Sie Trittfrequenz, Sicherheit, Fehlertoleranz, Konflikte, Komplexität und die logische Architektur. Weitere Informationen finden Sie unter [Berechnen Ressource Konsolidierung Muster](http://msdn.microsoft.com/library/dn589778.aspx).

Es gibt mehrere Methoden zum Implementieren Hintergrundaufgaben innerhalb einer Rolle der Cloud Services:

- Erstellen Sie eine Implementierung der Klasse **RoleEntryPoint** in die Rolle aus, und verwenden Sie deren Methoden auszuführende Hintergrundaufgaben. Führen Sie die Vorgänge im Kontext WaIISHost.exe. **GetSetting** -Methode der Klasse **CloudConfigurationManager** können diese um Einstellungen Konfiguration zu laden. Weitere Informationen finden Sie unter [Lebenszyklus (Cloud Services)](#lifecycle-cloud-services).
- Verwenden von Startaufgaben Hintergrundaufgaben ausführen, wenn die Anwendung gestartet wird. Zum erzwingen, dass die Aufgaben weiterhin im Hintergrund ausgeführt, legen Sie die Eigenschaft **TaskType** auf **Hintergrund** (Wenn Sie dies tun, der Anwendung beim Start-Prozess werden angehalten und warten, bis die Aufgabe abgeschlossen). Weitere Informationen finden Sie unter [beim Start von Vorgängen in Azure ausführen](./cloud-services/cloud-services-startup-tasks.md).
- Verwenden Sie das WebJobs SDK Hintergrundaufgaben wie WebJobs implementiert wird, die als Startaufgabe initiiert werden. Weitere Informationen finden Sie unter [Erste Schritte mit der Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Verwenden eines Vorgangs starten, installieren einen Windows-Dienst, der eine oder mehrere Hintergrundaufgaben ausgeführt wird. Sie müssen die Eigenschaft **TaskType** auf **Hintergrund** festlegen, sodass der Dienst im Hintergrund ausgeführt wird. Weitere Informationen finden Sie unter [beim Start von Vorgängen in Azure ausführen](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Ausführen von Hintergrundaufgaben in die Web-Rolle

Der wichtigste Vorteil Hintergrundaufgaben in die Web-Rolle ausgeführt wird, das Speichern in Hostinganbieter Kosten, da es nicht erforderlich ist, um zusätzliche Rollen bereitstellen.

### <a name="running-background-tasks-in-a-worker-role"></a>Ausführen von Hintergrundaufgaben in eine Worker-Rolle

Hintergrundaufgaben in eine Worker-Rolle ausgeführt hat mehrere Vorteile:

- Sie können Sie dieselbe Skalierung separat für jede Art von Rolle verwalten. Beispielsweise müssen Sie gegebenenfalls weitere Instanzen von einer Webrolle zur Unterstützung der aktuellen laden, aber weniger Instanzen von Worker-Rolle, die Hintergrundaufgaben ausgeführt wird. Durch Skalierung Hintergrund Vorgang berechnen Instanzen unabhängig von der Benutzeroberfläche Rollen, können Sie Hostinganbieter Kosten, bei weitgehender zulässigen Leistung reduzieren.
- Die Verarbeitung von Verwaltungsaufwand für Hintergrundaufgaben aus der Webrolle verlagert. Die Web-Rolle, die die Benutzeroberfläche enthält kann reagiert bleiben, und es kann bedeuten, dass weniger Instanzen erforderlich sind, um ein bestimmtes Volume von Anfragen von Benutzern zu unterstützen.
- Sie können Sie Trennung von Bereichen implementieren. Jedes Typs Rolle kann eine bestimmte Folge von klar definierten und den zugehörigen Aufgaben implementieren. Dadurch wird das Entwerfen und Verwalten des Codes vereinfachen, da weniger Interdependenz Code und Funktionalität zwischen jede Rolle vorhanden ist.
- Es kann dazu beitragen vertrauliche Prozesse und Daten isolieren. Beispielsweise müssen Webrollen, die die Benutzeroberfläche implementieren nicht auf Daten zugreifen können, die verwaltet und durch eine Worker-Rolle gesteuert. Dies kann nützlich sein, in das Optimieren der Sicherheit, besonders, wenn Sie ein Muster wie das [Gatekeeper Muster](http://msdn.microsoft.com/library/dn589793.aspx)verwenden.  

### <a name="considerations"></a>Aspekte

Berücksichtigen Sie beim Auswählen wie und wo die folgenden Punkte Hintergrundaufgaben bereitstellen bei Verwendung des Cloud Services Web und Worker Rollen:

- Hostinganbieter Hintergrundaufgaben in einer vorhandenen Webrolle kann die Gesamtkosten der Ausführung einer separaten Arbeiter Rolle nur für diese Aufgaben speichern. Es ist jedoch, die die Leistung und Verfügbarkeit der Anwendung beeinflussen, wenn es Konflikte für Verarbeitung und anderen Ressourcen. Mit einer separaten Arbeiter Rolle Loss die Web-Rolle aus den Einfluss der langer oder Ressource ankommt Hintergrundaufgaben.
- Wenn Sie die Hintergrundaufgaben mithilfe der Klasse **RoleEntryPoint** hosten, können Sie diese einfach zu einer anderen Rolle verschieben. Wenn Sie in einer Web-Klasse erstellen und später entscheiden, dass Sie die Aufgaben in einer Worker-Rolle ausführen müssen, können Sie die Implementierung der **RoleEntryPoint** in der Worker-Rolle verschieben.
- Startaufgaben sollen ein Programm oder ein Skript ausführen. Bereitstellen eines Auftrags Hintergrund als ausführbare Programm ist möglicherweise schwieriger, insbesondere dann, wenn es auch Bereitstellung von abhängigen Assemblys erforderlich ist. Möglicherweise leichter bereitstellen und Verwenden eines Skripts zum Hintergrund zu definieren, wenn Sie beim Startaufgaben verwenden.
- Ausnahmen, die einen Hintergrund Vorgang zum Fehlschlagen verursachen wirken sich verschiedene, je nachdem, wie, dass sie gehostet werden:
  - Wenn Sie den **RoleEntryPoint** Klasse Ansatz verwenden, verursachen eine fehlgeschlagene Aufgabe die Rolle neu starten, damit die Aufgabe wird automatisch neu gestartet. Dies kann die Verfügbarkeit der Anwendung auswirken. Um dies zu verhindern, stellen Sie sicher, dass Sie robuste innerhalb der **RoleEntryPoint** Klasse und die Hintergrundaufgaben Behandlung von Ausnahmen enthalten. Verwenden Sie die Feldfunktion, um Aufgaben, die ein Fehler auftreten, wenn dies geeignet ist, neu zu starten, und die Ausnahme verwenden, um die Rolle nur, wenn Sie innerhalb des Codes ordnungsgemäß des Fehlers wiederherstellen können nicht neu zu starten.
  - Wenn Sie beim Startaufgaben verwenden, sind Sie verantwortlich für die Verwaltung von der Ausführung der Aufgabe wird überprüft, ob es fehlschlägt.
- Verwalten und Überwachen von Startaufgaben wird schwieriger als die Verwendung des **RoleEntryPoint** Klasse Ansatzes. Die Azure WebJobs SDK enthält jedoch ein Dashboard, um WebJobs zu verwalten, die Sie durch Startaufgaben einleiten erleichtern.

### <a name="more-information"></a>Weitere Informationen

- [Berechnet die Konsolidierung Muster Ressource](http://msdn.microsoft.com/library/dn589778.aspx)
- [Erste Schritte mit der Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure-virtuellen Computern

Hintergrundaufgaben implementiert werden könnte, auf eine Weise, die verhindert, dass diese Azure Web Apps oder Cloud Services bereitgestellt wird, oder diese Optionen möglicherweise nicht geeignete. Typische Beispiele sind Windows-Diensten und Drittanbieter-Dienstprogramme und ausführbare Programme. Ein weiteres Beispiel möglicherweise Programme geschrieben für eine Ausführung-Umgebung, die mit der Anwendung unterscheidet. Dies möglicherweise z. B. Unix oder Linux Programm, das Sie von einer Windows oder .NET Anwendung ausführen möchten. Sie können eine Auswahl der Betriebssysteme für eine Azure-virtuellen Computern und Ausführen von Ihrem Dienst oder ausführbare Datei des betreffenden virtuellen Computers.

Damit Sie die Verwendung von virtuellen Computern auswählen können, finden Sie unter [Azure App Services, Cloud-Diensten und virtuellen Computern Vergleich](./app-service-web/choose-web-site-cloud-service-vm.md). Informationen zu den Optionen für virtuelle Maschinen finden Sie unter [Größe von virtuellen Computern und Cloud-Dienst für Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Weitere Informationen zu den Betriebssystemen und vordefinierten Bilder, die für virtuelle Computer verfügbar sind, finden Sie unter [Azure-virtuellen Computern Marketplace](https://azure.microsoft.com/gallery/virtual-machines/).

Um den Hintergrund Vorgang in einem separaten virtuellen Computer zu starten, müssen Sie eine Reihe von Optionen aus:

- Sie können die Aufgabe bei Bedarf direkt aus Ihrer Anwendung ausführen, durch Senden einer Anforderung an einen Endpunkt, den die Aufgabe verfügt. Dies übergibt alle Daten, die die Aufgabe erforderlich sind. Diesen Endpunkt Ruft den Vorgang an.
- Sie können konfigurieren, dass die Aufgabe für einen Zeitplan mithilfe eines Scheduler oder Zeitmessung, das Ihnen gewählte Betriebssystem bieten ausgeführt. Beispielsweise unter Windows können Windows-Taskplaner Sie Skripts und Aufgaben ausführen. Oder, wenn Sie SQL Server auf virtuellen Computers installiert haben, können Sie mithilfe der SQL Server-Agent Skripts und Aufgaben ausführen.
- Azure Scheduler können Sie um durch Hinzufügen einer Nachricht an eine Warteschlange, der die Aufgabe überwacht oder durch Senden einer Anforderung an eine API, die die Aufgabe verfügt über den Vorgang zu starten.

Finden Sie im früheren Abschnitt [Trigger](#triggers) für Weitere Informationen, wie Sie die Hintergrundaufgaben einleiten können.  

### <a name="considerations"></a>Aspekte

Beachten Sie die folgenden Punkte, wenn Sie entscheiden, ob Sie bereitstellen Hintergrundaufgaben in einer Azure-virtuellen Computern sind:

- In einer separaten Azure-virtuellen Computern Hintergrundaufgaben Hostinganbieter sorgt für Flexibilität und präzise steuern Einleitung, Ausführung, Planung und ressourcenzuteilung kann. Sie jedoch wird Laufzeit Kosten erhöhen, wenn eine virtuellen Computern bereitgestellt werden müssen, einfach, um Hintergrundaufgaben ausführen.
- Es gibt keine Möglichkeit, die Aufgaben in der Azure-Portal und keine Funktionen für automatisierte Neustart fehlgeschlagene Tasks – überwachen, obwohl Sie können den grundlegenden Status des virtuellen Computers überwachen und verwalten es mithilfe der [Azure Service-Cmdlets für die Verwaltung](http://msdn.microsoft.com/library/azure/dn495240.aspx). Es gibt jedoch keine Fertigungsanlagen Prozesse und Nachrichtenfäden Datenverarbeitungsknoten steuern. Normalerweise werden mithilfe eines virtuellen Computers benötigen zusätzlichen Aufwand ein Verfahren implementiert wird, die Daten in der Aufgabe Instrumentation und vom Betriebssystem des virtuellen Computers erfasst. Eine Lösung, die entsprechenden möglicherweise ist das [System Center Management Pack für Azure](http://technet.microsoft.com/library/gg276383.aspx)verwenden.
- Es möglicherweise sinnvoll, erstellen Überwachung Prüfpunkte, die über HTTP-Endpunkte verfügbar gemacht werden. Der Code für diese Prüfpunkte konnte integritätsprüfungen ausführen, sammeln Informationen zum Betrieb und Statistiken – oder Fehlerinformationen sortiert werden sollen und an eine Anwendung zurückgeben. Weitere Informationen finden Sie unter [Dienststatus Endpunkt Überwachung Muster](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Weitere Informationen

- Azure [virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/)
- [Azure-virtuellen Computern häufig gestellte Fragen](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Entwurfsaspekte

Es gibt verschiedene grundlegende Faktoren berücksichtigen Sie beim Entwerfen Hintergrundaufgaben. In den folgenden Abschnitten erläutern Partitionierung, Konflikte und koordinieren.

## <a name="partitioning"></a>Partitionierung

Wenn Sie sich entscheiden Hintergrundaufgaben innerhalb einer vorhandenen berechnen Instanz (wie eine Web app, Webrolle, vorhandene Worker-Rolle oder virtuellen Computern) aufnehmen möchten, müssen Sie berücksichtigen, wie diese Qualitätsattribute der berechnen Instanz und den Hintergrund Vorgang selbst auswirken. Diese Faktoren helfen Ihnen zu entscheiden, ob die Aufgaben mit der vorhandenen berechnen Instanz zu installieren, oder trennen sie sich in einer separaten berechnen Instanz:

- **Verfügbarkeit**: Hintergrundaufgaben möglicherweise nicht der gleichen Ebene wie die anderen Teile der Anwendung, die über die Verfügbarkeit von der Benutzeroberfläche und andere Teile, die direkt in die Interaktion mit dem Benutzer beteiligt sind im besonderen sein müssen. Hintergrundaufgaben möglicherweise weitere der Wartezeit, wiederholt Verbindungsfehlern, Vergleich und andere Faktoren die Verfügbarkeit wirkt, da die Vorgänge in der Warteschlange werden können. Es muss jedoch ausreichend Kapazität, um zu verhindern, dass die Sicherung von Besprechungsanfragen, die Warteschlangen blockieren und Einfluss auf die Anwendung als Ganzes.
- **Skalierbarkeit**: Hintergrundaufgaben sind wahrscheinlich einen anderen Skalierbarkeit als die Benutzeroberfläche und die interaktiven Teile der Anwendung benötigen. Möglicherweise die Skalierung der Benutzeroberfläche Spitzen Nachfrage, erfüllen, während ausstehenden Hintergrundaufgaben weniger Zeiten von einer kleineren ausgefüllt werden möglicherweise Nummer der Instanzen berechnen.
- **Stabilität**: Fehler einer berechnen Instanz, die nur Hintergrundaufgaben hostet möglicherweise keine dass schwer Auswirkung auf die Anwendung als Ganzes Wenn die Anforderungen für die folgenden Aufgaben können in der Warteschlange oder zurückgestellt, bis die Aufgabe wieder verfügbar ist. Wenn die berechnen Instanz und/oder Vorgänge innerhalb einer geeigneten Zeitspanne neu gestartet werden können, können Benutzer der Anwendung nicht betroffen.
- **Sicherheit**: Hintergrundaufgaben möglicherweise andere Sicherheitsstandards gelten oder Einschränkungen als die Benutzeroberfläche oder andere Teile der Anwendung. Mithilfe einer separaten berechnen Instanz, können Sie eine andere Sicherheit-Umgebung für die Aufgaben angeben. Muster wie Gatekeeper können Sie auch im Hintergrund berechnen Instanzen von der Benutzeroberfläche isolieren, um die Sicherheit und Trennung zu optimieren.
- **Leistung**: Sie können den Typ der berechnen Instanz für Hintergrund zur ganzen Übereinstimmung die Leistung Anforderungen Aufgaben Aufgaben auswählen. Dies bedeutet möglicherweise mit einer weniger teure berechnen-Option aus, wenn die Aufgaben nicht dieselben Verarbeitungsfunktionen wie der Benutzeroberfläche oder eine größere Instanz, benötigen Wenn sie eine zusätzliche Kapazität und Ressourcen benötigen.
- **Verwaltbarkeit**: Hintergrundaufgaben möglicherweise müssen Sie einen anderen Entwicklung und Bereitstellung Rhythmus aus dem Hauptfenster der Anwendungscode oder der Benutzeroberfläche. Sie auf einem separaten berechnen Instanz bereitzustellen kann Updates und Versioning vereinfachen.
- **Kosten**: Hinzufügen von Instanzen auszuführende Hintergrund Aufgaben erhöht Kosten Hostinganbieter zu berechnen. Das Verhältnis zwischen zusätzliche Kapazität und diese zusätzlichen Kosten sollten Sie sorgfältig.

Weitere Informationen finden Sie unter [Füllzeichen Wahl](http://msdn.microsoft.com/library/dn568104.aspx) und [Nutzer-Muster Mitbewerber auftritt](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Konflikte

Wenn Sie mehrere Instanzen eines Auftrags Hintergrund haben, ist es möglich, dass sie für den Zugriff auf Ressourcen und Dienste, wie z. B. Datenbanken und Speicher Teilen müssen. Diesen gleichzeitige Zugriff kann Ressourcenkonflikte, führen die Konflikte bei der Verfügbarkeit der Dienste und der Integrität der Daten im Speicher verursachen kann. Sie können Ressourcenkonflikte lösen, mithilfe eines pessimistischen Sperren Ansatzes. Dadurch wird verhindert, dass Mitbewerber auftritt Instanzen eines Vorgangs gleichzeitig einen Dienst zugreifen oder diese Daten zu beschädigen.

Ein anderes Verfahren zum Lösen von Konflikten ist Hintergrundaufgaben als einem einzelnen, definieren, damit es immer nur eine Instanz ausgeführt wird. Jedoch ist es dadurch die Zuverlässigkeit und Leistung Vorteile, die eine Konfiguration von mehreren Instanzen bereitstellen kann. Dies ist insbesondere dann, wenn die Benutzeroberfläche ausreichend Arbeit, um mehr als einen Hintergrund Vorgang beschäftigt beibehalten angeben kann.

Es ist entscheidend, stellen Sie sicher, dass die Hintergrundaufgabe automatisch gestartet werden kann, und es ausreichenden Kapazität für Spitzen in Anforderung bewältigen hat. Dazu können Sie eine berechnen Instanz mit ausreichende Ressourcen zuordnen, indem Sie ein Einfügen in die Warteschlange Verfahren, das Speichern von Besprechungsanfragen für eine spätere Ausführung bei Bedarf verkleinert oder mithilfe einer Kombination dieser Techniken.

## <a name="coordination"></a>Koordinierung

Die Hintergrundaufgaben möglicherweise komplexe und erfordern möglicherweise mehrere einzelne Vorgänge, um ein Ergebnis zu erzeugen oder alle Vorschriften zu erfüllen auszuführen. Es ist häufig für diese Szenarien, und teilen Sie die Aufgabe in kleineren einzelne Schritte oder Teilvorgänge, die von mehreren Nutzer ausgeführt werden können. Da die einzelnen Schritte möglicherweise in mehrere Aufträge wieder verwendbaren können mit mehreren Schritten Aufträge effizienter und flexibler sein. Es ist auch einfach, hinzufügen, entfernen oder Ändern der Reihenfolge der Schritte.

Koordinieren von mehreren Vorgängen und Schritte kann schwierig sein, aber es gibt drei allgemeine Muster, die Sie verwenden können, um Ihre Implementierung einer Lösung Leitfaden:

- **Eine Aufgabe in mehreren wieder verwendbaren Schritte zerlegen**. Eine Anwendung möglicherweise eine Vielzahl von Aufgaben mit wechselnder Komplexität auf den Informationen ausführen möchten, die sie verarbeitet erforderlich sein. Ein recht einfach aber feste Ansatz zum Implementieren dieser Anwendungs möglicherweise diese Verarbeitung als monolithische Modul ausführen. Dieser Ansatz ist jedoch wahrscheinlich verringern die Verkaufschancen für den Code Umgestaltung, Optimieren sie oder wiederzuverwenden Teile der gleichen Verarbeitung an anderer Stelle in der Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Pipes und Filter Muster](http://msdn.microsoft.com/library/dn568100.aspx).
- **Verwalten der Ausführung der Schritte für einen Vorgang**. Eine Anwendung möglicherweise Aufgaben ausführen, die mehrere Schritte umfassen (von denen einige möglicherweise Aufrufen remote Services oder Zugriff auf remote-Ressourcen). Die einzelnen Schritte möglicherweise unabhängig voneinander, aber sie werden von der Anwendungslogik, die die Aufgabe implementiert koordiniert. Weitere Informationen finden Sie unter [Scheduler Agent Vorgesetzten Muster](http://msdn.microsoft.com/library/dn589780.aspx).
- **Verwalten der Wiederherstellung für den Vorgang Schritte, die ein Fehler auftreten**. Anwendung müssen um die Arbeit rückgängig zu machen, die durch eine Folge von Schritten durchgeführt werden (wodurch zusammen eine Gelegenheit konsistenten Betrieb definiert), wenn eine oder mehrere Schritte ein Fehler auftreten. Weitere Informationen finden Sie unter [Compensating Transaktion Muster](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Lebenszyklus (Cloud Services)

 Wenn Sie Hintergrund Einzelvorgänge für Cloud Services Applications implementieren, die Rollen Web und Worker mithilfe der Klasse **RoleEntryPoint** verwenden möchten, ist es wichtig, den Lebenszyklus dieser Klasse zu verstehen, um ordnungsgemäß verwendet werden kann.

Web und Worker Rollen wechseln über eine Reihe von Phasen, sobald sie beginnen, ausführen, und beenden. Die **RoleEntryPoint** -Klasse macht eine Reihe von Ereignissen, die darauf hinweisen, wenn Sie diese Phasen auftreten können. Verwenden Sie diese bei der Initialisierung, ausführen, und beenden Ihre benutzerdefinierten Hintergrundaufgaben. Der vollständige Zyklus ist:

- Azure lädt die Rolle Assembly und sucht es nach einer Klasse, die von **RoleEntryPoint**abgeleitet wird.
- Wenn sie diese Klasse findet, ruft sie **RoleEntryPoint.OnStart()**an. Sie überschreiben diese Methode, um Ihre Hintergrundaufgaben Initialisierung.
- Nach Abschluss die **OnStart** -Methode präsentieren Azure Anrufe **Application_Start()** in der Anwendung Globaldatei Wenn diese ist (z. B. Global.asax in einer Webrolle ausführen ASP.NET).
- Azure Anrufe **RoleEntryPoint.Run()** in einem neuen Vordergrundthread, der mit **OnStart()**parallel ausgeführt wird. Sie überschreiben diese Methode, um Ihre Hintergrundaufgaben zu starten.
- Am Ende die Run-Methode ruft Azure **Application_End()** zuerst in der Anwendung Globaldatei, wenn dies vorhanden ist, und klicken Sie dann **RoleEntryPoint.OnStop() Ruft**. Sie überschreiben die **OnStop** -Methode, um Ihre Hintergrundaufgaben beenden, Ressourcen bereinigen, Objekte freigeben, und schließen Verbindungen, bei denen die Aufgaben möglicherweise verwendet haben.
- Der Azure Worker Rolle Host Vorgang angehalten. Die Rolle wird zu diesem Zeitpunkt wiederverwendet werden und wird neu gestartet.

Weitere Details und ein Beispiel für die Methoden der Klasse **RoleEntryPoint** verwenden finden Sie unter [Berechnen Ressource Konsolidierung Muster](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Aspekte

Beachten Sie die folgenden Punkte, wenn Sie beabsichtigen, wie Sie die Hintergrundaufgaben in einer Rolle Web oder Arbeitskollegen ausgeführt werden:

- Die standardmäßige **Ausführen** Methode Implementierung in der Klasse **RoleEntryPoint** enthält einen Anruf an **Thread.Sleep(Timeout.Infinite)** , die die Rolle endlos aktiv bleiben. Wenn Sie die Methode **Run** überschreiben (die in der Regel auszuführende Hintergrundaufgaben erforderlich ist), müssen Sie nicht zulassen von Code, um die Methode zu beenden, wenn Sie die Instanz der Rolle wiederverwenden möchten.
- Eine typische Implementierung der Methode **Ausführen** enthält Code zum Starten der einzelnen die Hintergrundaufgaben und einer Schleife erstellen, die den Status der die Hintergrundaufgaben regelmäßig überprüft. Sie können eine neu starten, die ein Fehler auftreten, oder überwachen für Abbruch, die darauf hinweisen, dass Aufträge durchgeführt wurden.
- Wenn eine Hintergrundaufgabe Ausnahmefehler auslöst, sollten diesen Vorgang wiederverwendet beim ermöglichen, dass alle anderen Hintergrundaufgaben in die Rolle weiter ausgeführt werden. Jedoch, wenn die Ausnahme durch eine Beschädigung von Objekten außerhalb der Aufgabe, beispielsweise freigegebenen Speicher verursacht wird die Ausnahme von Ihrer Klasse **RoleEntryPoint** behandelt werden soll, alle Vorgänge abgebrochen werden soll, und die Methode **Ausführen** dürfen, zu beenden. Azure wird dann die Rolle neu gestartet.
- Verwenden Sie die **OnStop** -Methode anhalten oder Abbrechen Hintergrundaufgaben und Ressourcen bereinigen. Dies möglicherweise umfassen, langer oder mit mehreren Schritten Aufgaben zu beenden. Es ist entscheidend, sollten wie folgt ausgeführt werden kann, um Dateninkonsistenzen zu vermeiden. Eine Instanz der Rolle aus irgendeinem Grund als ein manueller war(en) Tabstopps, muss der Code ausgeführt werden, in der **OnStop** -Methode innerhalb von fünf Minuten, bevor sie zwangsweise beendet wird abgeschlossen sein. Stellen Sie sicher, dass Ihr Code innerhalb dieses Zeitraums abgeschlossen werden kann oder tolerieren kann nicht vollständig ausgeführt.  
- Azure Lastenausgleich startet den Datenverkehr in die Instanz der Rolle Beendigung die Methode **RoleEntryPoint.OnStart** den Wert **true**zu leiten. Daher sollten Sie Sie Ihren Initialization-Code in der **OnStart** -Methode festlegen, damit Rolleninstanzen, die nicht erfolgreich Initialisierung keine Datenverkehr empfangen werden.
- Sie können beim Startaufgaben zusätzlich zu den Methoden der Klasse **RoleEntryPoint** verwenden. Startaufgaben sollten Änderungen an den Einstellungen Initialisierung, die Sie benötigen, um in Azure Lastenausgleich nicht ändern, da diese Aufgaben ausführt, bevor die Rolle alle Anfragen erhält. Weitere Informationen finden Sie unter [beim Start von Vorgängen in Azure ausführen](./cloud-services/cloud-services-startup-tasks.md).
- Ist es ein Fehler in einem Vorgang starten, möglicherweise die Rolle ständig neu starten erzwingen. Dies kann verhindern, dass Sie eine virtuelle IP-Adresse (VIP) Adresse austauschen wieder in eine zuvor bereitgestellte Version durchführen, da die austauschen exklusiven Zugriff auf die Rolle erforderlich ist. Dies kann nicht abgerufen werden, während die Rolle neu gestartet wurde. Um dieses Verhalten zu beheben:
    -  Fügen Sie den folgenden Code an den Anfang der Methoden **OnStart** , und **Führen Sie** in Ihrer Rolle aus:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Fügen Sie die Definition der Einstellung **fixieren** als boolesche Wert ServiceDefinition.csdef und ServiceConfiguration. *.cscfg Dateien für die Rolle aus, und legen Sie es auf* *falsch* *. Wenn die Rolle in einem wiederholten Restart-Modus geht, können Sie die Einstellung für die ändern * *Wahr** Funktion Execution fixieren, und lassen sie Sie mit einer früheren Version ausgetauscht werden.

## <a name="resiliency-considerations"></a>Stabilität Aspekte

Hintergrundaufgaben muss flexibel, um zuverlässigen Services können Sie die Anwendung bereitstellen. Wenn Sie planen und Hintergrundaufgaben entwerfen, beachten Sie die folgenden Punkte:

- Hintergrundaufgaben müssen Rolle oder einen bestimmten Dienst neu gestartet wurde ordnungsgemäß behandelt werden, ohne Daten zu beschädigen oder die Anwendung Inkonsistenzen Viren können. Für Vorgänge langer oder mit mehreren Schritten bietet _zeigende überprüfen_ , indem Sie den Status der Einzelvorgänge beständigen Speicher oder als Nachrichten in einer Warteschlange speichern, wenn dies zutrifft. Sie können beispielsweise beibehalten werden Statusinformationen in einer Nachricht in einer Warteschlange und diese Statusinformationen inkrementell mit den Vorgangsfortschritt aktualisieren, damit die Aufgabe aus dem letzten bekannten guten Prüfpunkt – statt vom Anfang einen Neustart verarbeitet werden kann. Bei der Verwendung von Azure Servicebuswarteschlangen können Sitzungen Nachricht Sie das gleiche Szenario aktivieren. Sitzungen ermöglichen es Ihnen, speichern und den Status der Anwendung Verarbeitung mithilfe der [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) und [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) Methoden abrufen. Weitere Informationen über das Entwerfen von zuverlässigen mit mehreren Schritten Prozesse und Workflows finden Sie unter [Scheduler Agent Vorgesetzten Muster](http://msdn.microsoft.com/library/dn589780.aspx).
- Wenn Sie Web oder Arbeitskollegen Rollen verwenden, um mehrere Hintergrundaufgaben hosten, Entwerfen Sie Ihre Außerkraftsetzung der Methode für fehlgeschlagene oder angehaltene Vorgänge überwachen und starten sie **Ausführen** . Dies ist nicht praktisch, und verwenden Sie eine Worker-Rolle, erzwingen Sie Worker-Rolle zu starten, indem Sie verlassen der Methode **Ausführen** .
- Wenn Sie Warteschlangen verwenden, um die Kommunikation mit Hintergrundaufgaben, die Warteschlangen fungieren können als Puffer Anfragen gespeichert, die den Vorgängen beim Ausführen der Anwendung gesendet werden unter üblichen Auslastung größer ist. Dadurch wird die Aufgaben auf den mit der Benutzeroberfläche in weniger ausgelasteten Zeiträumen neuesten Stand. Dies bedeutet auch, dass die Rolle Wiederverwendung nicht die Benutzeroberfläche blockiert. Weitere Informationen finden Sie unter [Warteschlange-basierten laden Kapazitätsabgleich Muster](http://msdn.microsoft.com/library/dn589783.aspx). Wenn einige Vorgänge wichtiger als andere sind, erwägen Sie das Implementieren der [Priorität Warteschlange Muster](http://msdn.microsoft.com/library/dn589794.aspx) , um sicherzustellen, dass diese Aufgaben vor weniger wichtigen Ereignissen ausgeführt.
- Hintergrundaufgaben, die Nachrichten oder Verarbeiten von Nachrichten eingeleitet werden müssen konzipiert werden Inkonsistenzen, wie etwa Nachrichten, die außerhalb der Reihenfolge empfangen, Nachrichten, die wiederholt einen Fehler (oft _beschädigte Nachrichten_genannt) verursachen und Nachrichten, die mehr als einmal übermittelt werden. Beachten Sie Folgendes aus:
  - Nachrichten, die in einer bestimmten Reihenfolge, wie die Daten basierend auf den vorhandenen Datenwert (z. B. Hinzufügen eines Werts zu einem vorhandenen Wert) ändern verarbeitet werden müssen, möglicherweise nicht in der ursprünglichen Reihenfolge, in der sie gesendet wurden, eintreffen. Möglicherweise auch von anderen Instanzen eines Vorgangs Hintergrund in einer anderen Reihenfolge aufgrund mit wechselnder Auslastung jede Instanz behandelt werden. Nachrichten, die in einer bestimmten Reihenfolge verarbeitet werden müssen sollten einschließen, eine laufende Nummer, Schlüssel oder einige andere Hervorhebung, die Hintergrundaufgaben verwenden können, um sicherzustellen, dass sie in der richtigen Reihenfolge verarbeitet werden. Wenn Sie Azure-Dienstbus verwenden, können Sie Nachricht Sitzungen verwenden, um die Reihenfolge der Übermittlung zu gewährleisten. Es ist jedoch in der Regel effizienter, soweit möglich, um den Vorgang zu entwerfen, sodass die Reihenfolge der Nachrichten nicht wichtig ist.
  - In der Regel wird eine Hintergrundaufgabe Nachrichten in der Warteschlange einsehen die vorübergehend aus einer anderen Nachricht Nutzer Blendet aus. Klicken Sie dann löscht die Nachrichten, nachdem sie erfolgreich verarbeitet wurden. Wenn Sie ein Hintergrund Vorgang schlägt fehl, wenn eine Nachricht zu verarbeiten, wird diese Nachricht in der Warteschlange wieder angezeigt, nachdem das anheften Timeout abläuft. Es wird von einer anderen Instanz des Vorgangs oder während des nächsten Verarbeitung dieser Instanz verarbeitet werden. Wenn die Nachricht im Consumer konsistente einen Fehler verursacht, wird es der Aufgabe, die Warteschlange und schließlich die Anwendung selbst blockiert, wenn die Warteschlange voll ist. Daher ist es entscheidend, erkennen und beschädigte Nachrichten aus der Warteschlange zu entfernen. Wenn Sie Azure-Dienstbus verwenden, können Nachrichten, die einen Fehler automatisch oder manuell in einer zugeordneten unzustellbare Warteschlange verschoben werden.
  - Warteschlangen am _mindestens einmal_ Übermittlung Verfahren garantiert sind, aber sie möglicherweise dieselbe Nachricht mehrmals vorführen. Darüber hinaus, wenn eine Hintergrundaufgabe nach der Verarbeitung einer Nachricht, aber vor dem Löschen aus der Warteschlange fehlschlägt, wird die Nachricht für die Verarbeitung erneut verfügbar. Hintergrundaufgaben sollten Idempotent, d. h., dieselbe Nachricht mehrmals Verarbeitung keinen Fehler oder Inkonsistenzen in der Anwendung Daten verursacht. Einige Vorgänge sind natürlich Idempotent, wie etwa einen gespeicherten Wert auf einen bestimmten neuen Wert festlegen. JOIN-Operationen wie das Hinzufügen eines Werts zu einem vorhandenen gespeicherten Wert ohne zu überprüfen, dass der gespeicherte Wert weiterhin dieselbe als, wenn die Nachricht ursprünglich gesendet wurde werden jedoch Inkonsistenzen verursachen. Azure Servicebuswarteschlangen können konfiguriert werden, um doppelte Nachrichten automatisch zu entfernen.
  - Einige messaging-Systemen, wie z. B. Azure-Speicher Warteschlangen und Azure-Servicebuswarteschlangen unterstützen eine De-queue Count-Eigenschaft, die die Anzahl, wie oft angibt, die eine Nachricht aus der Warteschlange gelesen wurde. Dies kann bei der Behandlung von wiederholter und beschädigte Nachrichten hilfreich sein. Weitere Informationen finden Sie unter [Asynchrone Messaging Einführung](http://msdn.microsoft.com/library/dn589781.aspx) und [Messagingtransport Muster](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Aspekte der Skalierung und Leistung

Hintergrundaufgaben müssen ausreichende Leistung, um sicherzustellen, dass sie nicht die Anwendung zu blockieren, oder Inkonsistenzen aufgrund verzögerte Vorgang, wenn das System Auslastung anbieten. In der Regel ist die Leistung durch die Skalierung der Instanzen berechnen, die die Hintergrundaufgaben hosten verbessert. Berücksichtigen Sie beim werden Planung und Hintergrundaufgaben entwerfen um Skalierbarkeit und Leistung die folgenden Punkte:

- Azure unterstützt automatische Skalierung (Skalierung und wieder in dieselbe Skalierung) auf Grundlage aktuellen Bedarf und laden – oder eine vordefinierte termingerecht für Web Apps, Cloud Services Web und Worker-Rollen und virtuellen Computern gehostet werden Bereitstellungen. Verwenden Sie diese Funktion, um sicherzustellen, dass die Anwendung als Ganzes ausreichende Leistung Funktionen und minimiert die Laufzeit Kosten hat.
- Wobei Hintergrundaufgaben einer anderen Leistungsfähigkeit von anderen Teile einer Cloud Services-Anwendung (beispielsweise der Benutzeroberfläche oder Komponenten wie der Data Access Layer) haben, kann Hostinganbieter die Hintergrundaufgaben zusammen in einer separaten Arbeiter die Benutzeroberfläche und den Hintergrund Vorgang Rollen um unabhängig voneinander zu skalieren zum Verwalten der laden. Wenn mehrere Hintergrundaufgaben erheblich unterschiedliche Performance Funktionen voneinander haben, sollten Sie sie in separaten Arbeiter Rollen unterteilen und dieselbe Skalierung jedes Typs Rolle unabhängig voneinander ein. Beachten Sie aber, dass dies Laufzeit Kosten im Vergleich zum Kombinieren von alle Vorgänge in weniger Rollen erhöhen kann.
- Skalierung einfach die Rollen möglicherweise reicht nicht viel Leistung Auslastung verlieren. Sie müssen möglicherweise auch Speicher Warteschlangen und weitere Ressourcen, die einen einzelnen Punkt der Gesamtkosten skalieren Verarbeitung Kette aus ausgestattet. Erwägen Sie auch andere Einschränkungen, beispielsweise den maximalen Durchsatz der Speicherplatz und andere Dienste, denen auf die Anwendung und die Hintergrundaufgaben aufsetzen.
- Hintergrundaufgaben müssen für die Skalierung konzipiert sein. Beispielsweise werden diese dynamisch die Anzahl der Speicher Warteschlangen verwendet, um das Abhören oder Senden von Nachrichten an die entsprechende Warteschlange erkennen können.
- Standardmäßig skalieren WebJobs mit den zugehörigen Azure Web Apps-Instanz aus. Wenn eine WebJob als nur eine Instanz ausgeführt werden soll, Sie können jedoch erstellen eine Settings.job-Datei, die die JSON-Daten enthält **{"Is_singleton": WAHR}**. Dies erzwingt Azure nur eine Instanz der WebJob ausführen, auch wenn mehrere Instanzen der zugeordneten Web app vorhanden sind. Dies kann eine nützliche Technik für geplante Aufträge sein, die als nur eine Instanz ausgeführt werden müssen.

## <a name="related-patterns"></a>Verwandte Muster

- [Asynchrone Messaging Einführung](http://msdn.microsoft.com/library/dn589781.aspx)
- [Automatische Skalierung Anleitungen](http://msdn.microsoft.com/library/dn589774.aspx)
- [Ermittlung Transaktion Muster](http://msdn.microsoft.com/library/dn589804.aspx)
- [Verschiedenen Nutzer Muster](http://msdn.microsoft.com/library/dn568101.aspx)
- [Partitionierung Anleitungen zu berechnen](http://msdn.microsoft.com/library/dn589773.aspx)
- [Berechnet die Konsolidierung Muster Ressource](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeper Muster](http://msdn.microsoft.com/library/dn589793.aspx)
- [Füllzeichen Wahl Muster](http://msdn.microsoft.com/library/dn568104.aspx)
- [Pipes und Filter Muster](http://msdn.microsoft.com/library/dn568100.aspx)
- [Priorität Warteschlange Muster](http://msdn.microsoft.com/library/dn589794.aspx)
- [Warteschlange-basierten laden Muster Abgleich](http://msdn.microsoft.com/library/dn589783.aspx)
- [Scheduler Agent Vorgesetzten Muster](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Weitere Informationen

- [Anpassungsbereich für Azure Applications mit Worker-Rollen](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Hintergrundaufgaben ausführen](http://msdn.microsoft.com/library/ff803365.aspx)
- [Start-Lebenszyklus Azure-Rolle](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (Blogbeitrag)
- [Azure Cloud Services Rolle Lebenszyklus](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (Video)
- [Erste Schritte mit dem Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure und Service Bus Warteschlangen - verglichen und bereitstehen](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Aktivieren der Diagnose in einen Cloud-Dienst](./cloud-services/cloud-services-dotnet-diagnostics.md)

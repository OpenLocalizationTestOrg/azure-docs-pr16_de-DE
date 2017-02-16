<properties
   pageTitle="Azure App Dienst lokalen Cache Übersicht | Microsoft Azure"
   description="In diesem Artikel beschrieben, wie Sie aktivieren, Ändern der Größe und den Status des Features Azure App Dienst lokalen Cache Abfragen"
   services="app-service"
   documentationCenter="app-service"
   authors="SyntaxC4"
   manager="yochayk"
   editor=""
   tags="optional"
   keywords=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="03/04/2016"
   ms.author="cfowler"/>

# <a name="azure-app-service-local-cache-overview"></a>Azure App Dienst lokalen Cache (Übersicht)

Azure Web app-Inhalten auf Azure Storage gespeichert werden und wird in einer dauerhaften Weise als Inhalt Freigabe von angegeben. Dieser Entwurf ist für die Arbeit mit einer Vielzahl von apps vorgesehen und weist die folgenden Attribute:  

* Der Inhalt über mehrere Instanzen von virtuellen Computern (virtueller Computer) des Web app freigegeben.
* Der Inhalt ist dauerhaften und geändert werden kann, indem Sie Web apps.
* Protokolldateien und diagnostic Datendateien sind unter demselben freigegebenen Ordner Inhalt verfügbar.
* Veröffentlichen neuen Inhalte direkt aktualisiert den Inhalten Ordner. Sie können die denselben Inhalt über die SCM-Website und der laufenden Web app sofort anzeigen (in der Regel einige Technologien wie ASP.NET führen Sie einen Neustart des Web-app auf Ihre Datei Änderungen vorgenommen, die neueste Inhalte werden initiieren).

Während viele Web apps eine oder alle diese Features verwenden möchten, benötigen Sie einige Web apps nur einen leistungsfähige, schreibgeschützt Content-Speicher, den sie aus mit hoher Verfügbarkeit ausgeführt werden können. Diese apps können eine Instanz der von einem bestimmten lokalen Cache virtueller Computer nutzen.

Das Feature Azure App Dienst lokalen Cache bietet eine Web-Rolle-Ansicht Teil Ihres Inhalts. Dieses Inhaltstyps ist ein Cache schreiben aber verwerfen Ihrer Speicher-Inhalte, die Website beim Start asynchrone erstellt wird. Wenn der Cache bereit ist, wird die Website aktiviert, um für den zwischengespeicherten Inhalt auszuführen. Web apps, die auf dem lokalen Cache ausgeführt stehen die folgenden Vorteile:

* Sie sind immun Wartezeiten, die auftreten, wenn sie Inhalte auf Azure-Speicher zugreifen.
* Sie sind immun geplante Upgrades oder einer ungeplanten Ausfallzeiten und andere Ausbluten Azure-Speicher, die auf Servern auftreten, die die Inhalte freigeben dienen.
* Sie verfügen über weniger app Neustarten aufgrund Speicherplatz freigeben Änderungen.

## <a name="how-local-cache-changes-the-behavior-of-app-service"></a>Lokalen Cache ändert das Verhalten der App-Dienst

* Der lokale Cache ist eine Kopie der Ordner /site und /siteextensions des Web app an. Es wird auf die lokale Instanz virtueller Computer beim Start Web-app erstellt. Die Größe des lokalen Cache pro Web app auf 300 MB standardmäßig beschränkt ist, aber Sie können sie bis zu 1 GB erhöhen.
* Der lokale Cache ist Lese-und Schreibzugriff. In irgendeiner Weise werden jedoch verworfen werden, wenn das Web app virtuellen Computern navigiert oder neu gestartet wird. Sie sollten nicht lokalen Cache für apps verwenden, die im Inhalt Store wichtiger Daten zu speichern.
* Web apps können weiterhin Protokolldateien und diagnostic Daten schreiben aktuell wie. Protokolldateien und Daten werden jedoch lokal des virtuellen Computers gespeichert. Dann sind sie regelmäßig an den freigegebenen Inhalt Speicher kopiert. Die Kopie der freigegebenen Inhalt Store ist eine optimistische Initiative – schreiben, die Sicherung Fälligkeitsdatum zu einem Absturz plötzlich, der eine Instanz der virtueller Computer verloren.
* Es gibt eine Änderung in der Ordnerstruktur der Ordner log-Dateien und Daten von Web apps, die lokalen Cache verwenden. Es gibt nun Unterordner im Speicher log-Dateien und Daten, die dem naming Muster "Eindeutiger Bezeichner" + Zeitstempel folgen Ordner. Jeder Unterordner entspricht einer Instanz virtueller Computer, in dem die Web app läuft oder ausgeführt wurde.  
* Für die Veröffentlichung Änderungen an der Web-app durch keines der Veröffentlichung Verfahren werden in den freigegebenen Inhalt Store veröffentlichen. Dies ist systembedingt, da wir der veröffentlichten Inhalt dauerhaften werden soll. Um den lokalen Cache des Web app zu aktualisieren, muss es neu gestartet werden. Scheint dies wie eine übermäßige Schritt? Um den Lebenszyklus nahtlose machen, finden Sie weiter unten in diesem Artikel.
* D:\Home wird auf dem lokalen Cache verweisen. D:\Local weiterhin auf die temporäre bestimmte virtueller Computer-Speicher zeigen.
* Der Standard-Inhaltsansicht des SCM Website weiterhin mit den freigegebenen Inhalt Store werden.

## <a name="enable-local-cache-in-app-service"></a>Aktivieren der lokalen Cache in App-Dienst

Konfigurieren Sie lokalen Cache mithilfe einer Kombination von reservierte app-Einstellungen. Sie können diese Einstellungen für die app mithilfe der folgenden Methoden konfigurieren:

* [Azure-portal](#Configure-Local-Cache-Portal)
* [Azure Ressourcenmanager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Konfigurieren der lokalen Cache mithilfe des Azure-Portals
<a name="Configure-Local-Cache-Portal"></a>

Lokalen Cache auf Basis pro Web app zu aktivieren, verwenden diese Einstellung app:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure Portals app-Einstellungen: lokalen Cache](media/app-service-local-cache/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Konfigurieren der lokalen Cache mithilfe von Azure Ressourcenmanager
<a name="Configure-Local-Cache-ARM"></a>

```
...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],
    "properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-the-size-setting-in-local-cache"></a>Ändern Sie die Einstellung Größe im lokalen Cache

Standardmäßig ist die lokale Cachegröße **300 MB**. Dies umfasst die /site und /siteextensions-Ordner, die aus dem Inhalt Store kopiert werden als auch Protokolle und Daten lokal erstellter Ordner. Um diesen Grenzwert zu erhöhen, verwenden Sie die app-Einstellung `WEBSITE_LOCAL_CACHE_SIZEINMB`. Sie können bis zu **1 GB** (1000 MB) pro Web app vergrößern.

## <a name="best-practices-for-using-app-service-local-cache"></a>Bewährte Methoden für die Verwendung von App-Dienst lokalen Cache

Es empfiehlt sich, dass Sie zusammen mit dem Feature [Staging-Umgebungen](../app-service-web/web-sites-staged-publishing.md) lokalen Cache verwenden.

* Fügen Sie die Einstellung für _Kurznotizen_ app `WEBSITE_LOCAL_CACHE_OPTION` mit dem Wert `Always` auf Ihre **Herstellung** Slot. Wenn Sie verwenden `WEBSITE_LOCAL_CACHE_SIZEINMB`, auch als Kurznotizen Einstellung, um Ihre Herstellung Slot Lizenz.
* Erstellen Sie einen Slot **Staging** und auf Ihre Staging Slot veröffentlichen. Sie festlegen nicht in der Regel den staging Slot mit lokalen Cache aktivieren ein nahtloses Build bereitstellen Test-Lebenszyklus für das staging, wenn Sie die Vorteile von lokalen Cache für die Herstellung Slot abrufen.
*   Testen Sie Ihrer Website anhand Ihrer Staging Slot.  
*   Wenn Sie bereit sind, Ausgeben einer [austauschen Vorgang](../app-service-web/web-sites-staged-publishing.md#to-swap-deployment-slots) zwischen Ihrem Staging und Herstellung Steckplätzen.  
*   Kurznotizen Einstellungen gehören die Namen und um ein Slot Kurznotizen. Wenn der Staging Slot in Betrieb ausgetauscht werden, wird es also die lokalen Cache app-Einstellungen erben. Neu ausgetauscht Herstellung Slot wird mit dem lokalen Cache nach ein paar Minuten ausgeführt und anzulassen wird von als Teil der Slot Aufwärmdauer nach austauschen. Nach Abschluss der Slot austauschen, wird der Herstellung Slot also mit dem lokalen Cache ausgeführt werden.

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Woran erkenne ich, ob meine Web app lokalen Cache gilt?

Wenn Web app einen leistungsfähige, zuverlässigen Content-Speicher benötigt, den Inhalten Store nicht verwendet Schreibvorgangs wichtige Daten zur Laufzeit und weniger als 1 GB in Gesamtgröße ist, ist die Antwort "Ja"! Um die Gesamtgröße der Ordner /site und /siteextensions zu erhalten, können Sie die Website Erweiterung "Azure Web Apps Datenträgerverwendung" verwenden.  

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Woran erkenne ich, ob meine Website bei der Verwendung von lokalen Cache aktiviert wurde?

Wenn Sie das Feature lokalen Cache mit Staging-Umgebungen verwenden, wird der Vorgang austauschen erst lokalen Cache aufgewärmt ist abgeschlossen. Zum Überprüfen, ob Ihre Website lokalen Cache ausgeführt wird, können Sie die Worker Prozess Umgebungsvariable überprüfen `WEBSITE_LOCALCACHE_READY`. Verwenden Sie die Anweisungen auf der Seite [Worker Prozess Umgebungsvariable](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) , um die Worker Prozess Umgebungsvariable für mehrere Instanzen zuzugreifen.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Nur die neue Änderungen veröffentlicht, aber meine Web app anscheinend nicht bitten. Warum?

Wenn Ihre Web app lokalen Cache verwendet, müssen Sie Ihrer Website, um die neuesten Änderungen erhalten neu starten. Keine Änderungen in einer Website für die Herstellung veröffentlichen möchten? Finden Sie die Optionen Slot in den vorherigen Abschnitt zu bewährten Methoden aus.

### <a name="where-are-my-logs"></a>Wo sind Meine Protokolle?

Lokalen Cache führen Sie Ihre Protokolle und Datenordner ein wenig anders aussehen. Die Struktur der Unterordnern bleibt unverändert, jedoch mit dem Unterschied, dass die Unterordner unter einen Unterordner mit dem Format "virtuellen Computer Kennung" + Zeitstempel nestled sind.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Ich habe lokalen Cache aktiviert, aber meine Web-app immer noch ruft neu gestartet. Woran liegt das? Wollten, beigetragen lokalen Cache mit häufig verwendeten app neu gestartet wurde.

Lokaler Cache verhindern, dass Speicher-bezogene Web app neu gestartet wurde. Jedoch konnte die Web-app immer noch neu gestartet wurde während des geplanten Infrastruktur-Upgrades mit dem virtuellen Computer werden. Sollten weniger Neustart des gesamten app, die mit dem lokalen Cache aktiviert auftreten.

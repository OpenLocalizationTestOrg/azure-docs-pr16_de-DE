<properties 
    pageTitle="Konfigurieren der App-Verwaltungsdienst Azure Web apps" 
    description="Konfigurieren eine Web app Azure-App-Dienste" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Konfigurieren der App-Verwaltungsdienst Azure Web apps #

In diesem Thema wird erläutert, wie eine Web app Verwenden des [Azure-Portal]zu konfigurieren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Anwendungseinstellungen

1. Öffnen Sie das Blade für das Web app im [Portal Azure].
2. Klicken Sie auf **Alle Einstellungen**.
3. Klicken Sie auf **ApplicationSettings**.

![Anwendungseinstellungen][configure01]

Das Blade **Anwendungseinstellungen** weist die Einstellungen unter mehrere Kategorien gruppiert.

### <a name="general-settings"></a>Allgemeine Einstellungen

**Framework-Versionen**. Legen Sie diese Optionen aus, wenn Ihre app eine der folgenden Framework verwendet: 

- **.NET Framework**: Legen Sie die .NET Framework-Version. 
- **PHP**: Legen Sie die Version von PHP oder **OFF** zum Deaktivieren von PHP. 
- **Java**: Wählen Sie die Java-Version oder **aus** Java deaktivieren. Verwenden Sie die Option **Web Container** , um zwischen Tomcat und Pier auszuwählen.
- **Python**: Wählen Sie die Version Python oder **aus** Python deaktivieren.

Technischen Gründen deaktiviert Aktivieren von Java für Ihre app die .NET, PHP und Python Optionen aus.

<a name="platform"></a>
**Plattform**. Wählt aus, ob die Web-app in einer 32-Bit- oder 64-Bit-Umgebung ausgeführt wird. Die 64-Bit-Umgebung erfordert Basic oder Standard-Modus. Frei, und führen Sie freigegebene Modi immer in einer 32-Bit-Umgebung.

**Web Sockets**. Legen Sie **auf** das WebSocket-Protokoll aktivieren. Wenn beispielsweise Ihre Web app [ASP.NET SignalR] oder [socket.io]verwendet.

<a name="alwayson"></a>
Klicken Sie **auf immer**. Standardmäßig sind Web apps entfernt, wenn Sie sich für einige Zeitraum im Leerlauf befinden. Dadurch wird das System Ressourcen zu sparen. In Basic oder Standard-Modus können Sie **Immer auf** die app geladen immer beibehalten aktivieren. Wenn Ihre app fortlaufender Webaufträge ausgeführt wird, sollten Sie **Immer auf**aktivieren oder die Web Einzelvorgänge möglicherweise nicht zuverlässig ausgeführt.

**Verwaltete Verkaufspipeline Version**. Setzt den IIS [Verkaufspipeline Modus]. Behalten Sie die auf den integrierten (die Standardeinstellung), wenn Sie eine ältere app verfügen, die eine ältere Version von IIS erforderlich sind.

**Automatische austauschen**. Wenn Sie für ein Slot Bereitstellung automatisch austauschen aktivieren, werden App-Dienst automatisch austauschen des Web app in Betrieb, wenn Sie ein Update zu diesem Slot Pushbenachrichtigungen. Weitere Informationen finden Sie unter [bereitstellen zu staging von Web apps in Azure-App-Verwaltungsdienst Steckplätze] (Web-Websites-eingestufte-publishing.md).

### <a name="debugging"></a>Für das Debuggen

**Remote Debuggen**. Ermöglicht das Debuggen Remoteprozeduraufruf. Wenn aktiviert ist, können Sie den Remotedebugger in Visual Studio verwenden, Verbindung direkt an Ihre Web app. Remote-Debuggen bleibt von 48 Stunden aktiviert. 

### <a name="app-settings"></a>App-Einstellungen

Dieser Abschnitt enthält Name/Wert-Paare, die Sie web app wird beim Start von geladen. 

- Für .NET apps, werden diese Einstellungen in der Konfiguration .NET eingefügt `AppSettings` zur Laufzeit überschreiben vorhandene Einstellungen. 

- Anwendungen von PHP, Python, Java und Knoten können diese Einstellungen als Umgebungsvariablen zur Laufzeit zugreifen. Für jede Einstellung app werden zwei Umgebungsvariablen erstellt. eine mit dem Namen durch den Eintrag der app-Einstellung und einem anderen mit dem Präfix APPSETTING_ angegeben. Beide enthalten denselben Wert.

### <a name="connection-strings"></a>Verbindungszeichenfolgen

Verbindungszeichenfolgen für verknüpfte Ressourcen. 

Für .NET apps, werden diese Verbindungszeichenfolgen in der Konfiguration .NET eingefügt `connectionStrings` Einstellungen zur Laufzeit überschreiben vorhandene Einträge, bei denen die Taste den Datenbanknamen verknüpfte gleich. 

Für Applikationen von PHP, Python, Java und Knoten werden diese Einstellungen als Umgebungsvariablen zur Laufzeit, mit dem Präfix des Verbindungstyps verfügbar. Die Umgebung Variable Präfixe werden wie folgt aus: 

- SQLServer:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL­Datenbank:`SQLAZURECONNSTR_`
- Benutzerdefiniert:`CUSTOMCONNSTR_`

Wenn eine MySql-Verbindungszeichenfolge mit dem Namen wurden beispielsweise `connectionstring1`, würde über die Umgebungsvariable vom zugegriffen werden `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standarddokumente

Das Standarddokument ist die Webseite, die bei der Stamm-URL für eine Website angezeigt wird.  Die erste übereinstimmende Datei in der Liste wird verwendet. 

Web apps möglicherweise Module verwenden Sie, dass es bedienen statischen Inhalt in diesem Fall ist kein Standarddokument als solche, anstatt weiterleiten, basierend auf URL.    

### <a name="handler-mappings"></a>Ereignishandler Zuordnungen

Verwenden Sie in diesem Bereich Hinzufügen benutzerdefinierter Skriptprozessoren, um die Anforderung von bestimmter Dateierweiterungen behandeln. 

- **Erweiterung**. Die Erweiterung wie *.PHP oder handler.fcgi behandelt werden sollen. 
- **Skript Prozessor Pfad**. Der absolute Pfad der Skriptprozessor. Anfragen für Dateien, die die Erweiterung entsprechen werden durch das Skriptprozessor verarbeitet werden. Verwenden Sie den Pfad `D:\home\site\wwwroot` in Ihrer app Stammverzeichnis verweisen.
- **Zusätzliche Argumente**. Optionale Befehlszeilenargumente für das Skriptprozessor 


### <a name="virtual-applications-and-directories"></a>Virtuelle Anwendungen und Verzeichnisse durchsuchen 
 
Geben Sie virtuelle Anwendungen und Verzeichnisse durchsuchen um zu konfigurieren, jedes virtuelle Verzeichnis und der entsprechenden physischen Pfad relativ zum Stammverzeichnis Website ein. Optional können Sie das Kontrollkästchen **Anwendung** virtuelles Verzeichnis als Anwendung kennzeichnen auswählen.


## <a name="enabling-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen

Aktivieren von Diagnoseprotokollen:

1. Klicken Sie in das Blade für Ihre Web-app klicken Sie auf **Alle Einstellungen**.
2. Klicken Sie auf **Diagnoseprotokolle**. 

Optionen zum Schreiben von Diagnoseprotokollen aus einer Webanwendung, die die Protokollierung unterstützt: 

- Die **Anwendung protokolliert**. Schreibt Anwendungsprotokolle in das Dateisystem. Protokollierung für einen Zeitraum von 12 Stunden. 

**Ebene**. Wenn die Anwendung Protokollierung aktiviert ist, gibt diese Option an, dass die Menge der Informationen, die für den (Fehler, Warnung, Informationen oder ausführlich) aufgezeichnet.

**Protokollierung Webserver**. Protokolle werden im erweiterten Log W3C-Dateiformat gespeichert. 

**Ausführliche Fehlermeldungen**. Ausführliche Fehler speichert Nachrichten htm-Dateien. Die Dateien werden unter /LogFiles/DetailedErrors gespeichert. 

**Fehlgeschlagen Anforderung Tracing**. Protokolle Fehler bei Besprechungsanfragen in XML-Dateien Die Dateien werden unter /LogFiles/W3SVC*Funktionen Länge und LÄNGEB*, gespeichert, wobei Funktionen Länge und LÄNGEB einen eindeutigen Bezeichner. Dieser Ordner enthält eine XSL-Datendatei und eine oder mehrere XML-Dateien. Vergewissern Sie sich zum Herunterladen der Datei XSL an, da diese Funktionalität bereitstellt, für die Formatierung und den Inhalt der XML-Dateien zu filtern.

Wenn Sie die Protokolldateien anzeigen zu können, müssen Sie FTP-Anmeldeinformationen, wie folgt erstellen:

1. Klicken Sie in das Blade für Ihre Web-app klicken Sie auf **Alle Einstellungen**.
2. Klicken Sie auf **Bereitstellung Anmeldeinformationen**.
3. Geben Sie einen Benutzernamen und ein Kennwort ein.
4. Klicken Sie auf **Speichern**.

![Legen Sie die Bereitstellung Anmeldeinformationen][configure03]

Der vollständigen FTP-Benutzername ist "App\username" wobei *app* den Namen der Web app. Der Benutzername ist die Web app-Blade unter **Essentials**aufgeführt.  

![FTP-Bereitstellung Anmeldeinformationen][configure02]

## <a name="other-configuration-tasks"></a>Weitere Aufgaben

### <a name="ssl"></a>SSL 

In Basic oder Standard-Modus können Sie SSL-Zertifikate für eine benutzerdefinierte Domäne hochladen. Weitere Informationen finden Sie unter [aktivieren HTTPS für eine Web app]. 

Wenn Sie Ihre hochgeladene Zertifikate anzeigen möchten, klicken Sie auf **Alle Einstellungen** > **benutzerdefinierte Domänen und SSL**.

### <a name="domain-names"></a>Domänennamen

Hinzufügen von benutzerdefinierten Domänennamen für Ihre Web app. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-app in Azure-App-Verwaltungsdienst].

Wenn Sie den Domänennamen Ihrer anzeigen möchten, klicken Sie auf **Alle Einstellungen** > **benutzerdefinierte Domänen und SSL**.

### <a name="deployments"></a>Bereitstellungen

- Einrichten von kontinuierlichen Bereitstellung. Finden Sie unter [Verwenden Git Web Apps in Azure-App-Dienst bereitgestellt](./web-sites-deploy.md).
- Bereitstellung Steckplätze. Finden Sie unter [Staging-Umgebungen für Web Apps in Azure App-Verwaltungsdienst bereitstellen].


Wenn Ihre Bereitstellung Steckplätze anzeigen möchten, klicken Sie auf **Alle Einstellungen** > **Bereitstellung Steckplätze**.

### <a name="monitoring"></a>Für die Überwachung

In Basic oder Standard-Modus können Sie die Verfügbarkeit von HTTP oder HTTPS Endpunkte von bis zu drei Geo verteilt Positionen testen. Ein Überwachung Test schlägt fehl, wenn der HTTP-Antwort-Code ein Fehler (4xx oder 5xx ist) oder die Antwort mehr als 30 Sekunden dauert. Ein Endpunkt gilt zur Verfügung, wenn die Überwachung Tests alle angegebenen Orten erfolgreich ausgeführt werden kann. 

Weitere Informationen finden Sie unter [wie: Überwachen der Web-Endpunkt Status].

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen], in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Verwaltungsdienst]
- [Aktivieren Sie HTTPS für eine app in Azure App-Verwaltungsdienst]
- [Eine Web app im App-Verwaltungsdienst Azure skalieren]
- [Grundlagen von Web Apps in Azure-App-Dienst für die Überwachung]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure-Portal]: https://portal.azure.com/
[Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Verwaltungsdienst]: ./web-sites-custom-domain-name.md
[Implementierung von Web Apps in Azure App-Verwaltungsdienst Staging-Umgebungen]: ./web-sites-staged-publishing.md
[Aktivieren Sie HTTPS für eine app in Azure App-Verwaltungsdienst]: ./web-sites-configure-ssl-certificate.md
[So: Web-Endpunkt Status überwachen]: http://go.microsoft.com/fwLink/?LinkID=279906
[Grundlagen von Web Apps in Azure-App-Dienst für die Überwachung]: ./web-sites-monitor.md
[Verkaufspipeline-Modus]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Eine Web app im App-Verwaltungsdienst Azure skalieren]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Wiederholen Sie den App-Dienst]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

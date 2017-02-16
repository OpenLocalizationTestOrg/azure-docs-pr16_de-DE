# <a name="internet-of-things-security-best-practices"></a>Bewährte Methoden für die Sicherheit von Internet der Dinge

Zum Sichern einer Internet der Dinge (IoT) erfordert Infrastruktur eine strenge Sicherheit in Tiefe Strategie. Diese Strategie erfordert, dass die Daten in der Cloud zu sichern, Schutz der Datenintegrität bei der Übertragung über das Internet öffentlichen und Bereitstellen von Geräten Sie. Jede Ebene erstellt höhere Sicherheitsgarantie in der gesamten Infrastruktur.

## <a name="secure-an-iot-infrastructure"></a>Sichern einer IoT Infrastruktur

Diese Sicherheit in Tiefe Strategie kann entwickelt und mit aktiven Teilnahme von verschiedenen Akteuren verbindet mit Fertigung, Entwicklung und Bereitstellung von IoT Geräte und Infrastruktur ausgeführt werden. Es folgt eine allgemeine Beschreibung der diese Player.  

- **IoT Hardware Hersteller/Integrator**: normalerweise die Hersteller von IoT Hardware bereitgestellt wird Integratoren zusammenstellen Hardware von verschiedenen Herstellern oder Lieferanten bereitstellen Hardware für eine Bereitstellung IoT hergestellt oder von anderen Lieferanten integriert sind.
- **IoT Lösungsentwickler**: die Entwicklung einer Lösung IoT in der Regel durch eine Lösungsentwickler abgeschlossen ist. Dieser Entwickler möglicherweise Teil einer für Team oder auf in dieser Aktivität Systemintegrator (SI). Die IoT Lösungsentwickler kann Entwickeln der verschiedenen Komponenten der Lösung IoT von Grund, Integrieren von verschiedenen einsatzbereite oder Open Source-Komponenten oder vorkonfigurierte Lösungen mit geringfügigen Anpassung übernehmen.
- **IoT Lösung Bereitsteller**: nach ein IoT Lösung entwickelt wird, muss es im Feld bereitgestellt werden. Dies umfasst die Bereitstellung von Hardware, Verbund Geräte und Bereitstellung von Lösungen in Hardware-Geräten oder der Cloud.
- **IoT Lösung Operator**: nach IoT Lösung bereitgestellt wird, dies erforderlich macht langfristiges Vorgänge, Überwachung, Upgrades und Wartung. Dies kann durch eine interne Team erfolgen, die umfasst Informationen Technologiespezialisten, Hardware Vorgänge und Wartung Teams und Domäne Experten für das ordnungsgemäße Funktionieren der gesamten IoT Infrastruktur überwachen.

Die folgenden Abschnitten bewährte Methoden für diese Spieler besser entwickeln, bereitstellen und die Steuerung einer sicheren IoT Infrastruktur.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT Hardware Hersteller/integrator

Im folgenden werden die optimalen Methoden zum IoT Hardwarehersteller und Hardware Integratoren.

- **Bereich Hardware zu Mindestanforderungen**: Hardware-Design sollte enthalten die minimale Features, die für den Vorgang an der Hardware und nichts mehr erforderlich ist. Ein Beispiel ist USB-Ports nur, wenn Sie für die Ausführung des Geräts erforderlich aufnehmen möchten. Diese zusätzlichen Funktionen Öffnen Sie das Gerät für unerwünschte Angriffsmethoden, die vermieden werden sollte.
- **Stellen Nachweis manipulieren Hardware**: Erstellen von Verfahren zur Erkennung physisch manipuliert werden, wie z. B. Öffnen des Deckblatts Gerät oder einen Teil des Geräts entfernen. Diese manipulieren Signale möglicherweise Teil des Datenstreams in der Cloud, welche Operatoren dieser Ereignisse benachrichtigen konnte hochgeladen werden.
- **Erstellen, um sichere Hardware**: Wenn COGS explizit gestattet, zum Erstellen von Sicherheitsfeatures wie sichere und verschlüsselte Speicher oder -Funktionen basierend auf vertrauenswürdige Platform Modul (TPM) zu starten. Diese Funktionen machen Geräte sicherer und die gesamte IoT Infrastruktur zu schützen.
- **Stellen Sie sicher Upgrades**: Firmwareupdates während der Gültigkeitsdauer des Geräts unvermeidbare sind. Geräte mit sicheren Pfade für Upgrades und cryptographic Sicherstellung der Firmwareversionen erstellen, das Gerät während und nach dem Update secure möglicherweise zulassen.

## <a name="iot-solution-developer"></a>IoT Lösungsentwickler

Im folgenden werden die optimalen Methoden zum IoT Lösungsentwickler:

- **Folgen sicherer Software Development Methoden**: Entwicklung von sicherer Software erforderlich Grund auf denken Sicherheit seit Beginn des Projekts ganz nach deren Implementierung, testen und bereitstellen. Die verfügbaren Optionen für Plattformen, Sprachen und Tools werden alle mit dieser Methode beeinflusst. Die Microsoft-Sicherheits-Entwicklungszyklus bietet einen schrittweisen Ansatz zum Erstellen von sicheren Software.
- **Wählen Sie öffnen Source - Software Vorsicht**: Open Source-Software bietet die Möglichkeit, schnell Lösungen entwickeln. Bei der Auswahl öffnen Source-Software haben, erwägen Sie die Aktivität Ebene der Community für jede öffnen Quellkomponente. Eine aktive Community wird sichergestellt, dass Software unterstützt wird und Probleme erkannt und berücksichtigt werden. Alternativ eine undurchsichtige und inaktive Open Source-Software möglicherweise nicht unterstützt und Probleme wahrscheinlich nicht gefunden.
- **Integrieren Vorsicht**: viele Software Sicherheitsfehler bei der Grenze für Bibliotheken und APIs vorhanden. Funktionen, die nicht für die aktuelle Bereitstellung erforderlich sein kann möglicherweise noch über einem Layer API verfügbar sein. Um allgemeine Sicherheit zu gewährleisten, überprüfen Sie alle Schnittstellen der Komponenten für Sicherheitslücken integriert werden.      

## <a name="iot-solution-deployer"></a>IoT Lösung Bereitsteller

Im folgenden finden bewährte Methoden für Benutzer geeignet IoT-Lösung Anrufschemen verfügen:

- **Bereitstellen von Hardware sicher**: IoT Bereitstellungen erfordern möglicherweise Hardware im unsicheren Speicherorten, z. B. in öffentlichen Leerzeichen oder unbeaufsichtigt Gebietsschemas bereitgestellt werden. In diesen Fällen stellen Sie sicher, dass Hardware Bereitstellung unbefugtem ist gestatteten. USB oder andere Ports auf Hardware verfügbar sind, sicher, dass diese sicher gelten. Viele Angriffsmethoden können diese als Felder und Optionen.
- **Schützen Sie Authentifizierung Tasten**: während der Bereitstellung jedem Gerät erfordert Geräte-IDs und die zugehörigen Authentifizierung Tasten durch die Cloud-Dienst generiert. Schützen Sie diese Schlüssel physisch auch nach der Bereitstellung. Alle betroffenen Schlüssel kann von einem bösartiger Gerät um zu ausgeben als ein vorhandenes Gerät verwendet werden.

## <a name="iot-solution-operator"></a>IoT Lösung operator

Im folgenden werden die optimalen Methoden für die Lösung Operatoren IoT:

- **Das System auf dem neuesten Stand zu bleiben**: sicherstellen, dass das Gerätebetriebssysteme und alle Gerätetreiber auf die neuesten Versionen aktualisiert werden. Wenn Sie automatische Updates in Windows-10 (IoT oder anderen SKUs) aktivieren, bleibt Microsoft Stand, ein sicheres Betriebssystem für IoT Geräte bereitstellen. Andere Betriebssysteme (z. B. Linux) planmäßigen sichergestellt auf dem neuesten Stand, diese ebenfalls vor Angriffen geschützt sind.
- **Schutz vor bösartiger Aktivität**: Wenn das Betriebssystem zulässt, installieren Sie die neuesten Funktionen von Viren und Malware auf jedem Gerätebetriebssystem. Damit können Sie die meisten externe Einschränken dieser Risiken. Sie können die meisten modernen Betriebssysteme gegen geeignete Maßnahmen schützen.
- **Audit häufig**: Überwachung IoT Infrastruktur Sicherheits-Probleme ist Schlüssel aus, wenn auf Sicherheitsvorfälle reagiert. Die meisten Betriebssysteme bieten integrierte ereignisprotokollierung, die häufig überprüft werden sollte, um sicherzustellen, dass keine sicherheitsverletzung aufgetreten ist. Audit Informationen kann gesendet werden als separate werden Stream in der Cloud-Service, wo diese analysiert werden.
- **Schützen Sie die Infrastruktur IoT physisch**: die schlechtesten Angriffen gegen IoT Infrastruktur werden physische Zugriff auf Geräte mit gestartet. Eine wichtige Sicherheit Methode ist zum Schutz vor bösartiger verwenden der USB-Ports und anderen physischen Zugriff. Eine Taste, um Unternehmensdaten Verletzung, die aufgetreten sind möglicherweise ist der physische zugreifen, wie z. B. USB-Anschluss verwenden Protokollierung. Windows-10 (IoT und anderen SKUs) ermöglicht erneut ausführliche Protokollierung dieser Ereignisse.
- **Schützen Cloud Anmeldeinformationen**: Cloud Authentifizierungsinformationen für das Konfigurieren und Betrieb einer bereitstellungs IoT verwendet werden möglicherweise die einfachste Möglichkeit zum zugreifen und einem IoT System zu beeinträchtigen. Schützen Sie die Anmeldeinformationen, indem Sie das Kennwort häufig ändern, und nicht zu diese Anmeldeinformationen auf öffentlichen Computern verwenden.

Funktionen, die von anderen IoT Geräte variieren. Bei einigen Geräten möglicherweise Computern allgemeine desktop-Betriebssysteme und einigen Geräten unter den Betriebssystemen sehr leicht. Beschriebenen bewährten Sicherheitsmethoden möglicherweise zuvor auf folgenden Geräten in unterschiedlichen Grad anwendbar. Wenn bereitgestellt, sollte zusätzliche Sicherheit und Bereitstellung bewährte Methoden der Hersteller dieser Geräte folgen.

Bei einigen Geräten älterer Versionen und der eingeschränkten möglicherweise nicht speziell für IoT Bereitstellung entworfen haben. Diese Geräte möglicherweise nicht über die Möglichkeit, verschlüsseln Sie die Daten, mit dem Internet verbinden, oder geben Sie die erweiterte Überwachung. In diesen Fällen können Sie ein Gateway modernes und sicheren Feld Aggregieren von Daten aus älteren Geräten und zum Herstellen einer Verbindung zu diesen Geräten über das Internet erforderliche Sicherheit bereitstellen. Feld Gateways bieten sichere Authentifizierung, Aushandlung von verschlüsselten Sitzungen, Bestätigung der Befehle aus der Cloud und viele weitere Sicherheitsfeatures.

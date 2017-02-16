# <a name="compute"></a>Berechnen

Azure ermöglicht Ihnen das Bereitstellen und Überwachen Ihrer Anwendungscode in einem Microsoft Data Center ausgeführt. Wenn Sie eine Anwendung erstellen und auf Azure ausführen, die Code und Konfiguration nicht trennen heißt eine Azure gehostet Dienst. Gehosteten Dienste sind leicht zu verwalten, nach oben oder unten skalieren, neu zu konfigurieren und mit neuen Versionen von Code der Anwendung zu aktualisieren. Dieser Artikel befasst sich die gehostet Azure Service-Anwendungsmodell.<a id="compare" name="compare"></a>

## <a name="table-of-contentsa-idgoback-namegobacka"></a>Inhaltsverzeichnis<a id="_GoBack" name="_GoBack"></a>

-   [Vorteile der Azure-Anwendung-Modell][]
-   [Gehosteten Dienst grundlegenden Konzepte][]
-   [Gibt gehosteten Dienst][]
-   [Entwerfen der Anwendung für die Skalierung][]
-   [Gehosteten Dienstdefinition und Konfiguration][]
-   [Die Definition Dienstdatei][]
-   [Die Konfiguration Dienstdatei][]
-   [Erstellen und Bereitstellen eines gehosteten Diensts][]
-   [Verweise][]

## <a id="benefits"> </a>Azure-Anwendung Modell Vorteile

Wenn Sie die Anwendung als gehosteten Dienst bereitstellen, Azure erstellt eine oder mehrere virtuellen Computern (virtuelle Computer), die Ihrer Anwendung Code enthalten, und startet die virtuellen Computern auf physischen Computern, die sich in einem der Azure Data Center befinden. Client-Anfragen an Ihrer Anwendung gehosteten das Data Center eingeben, verteilt ein Lastenausgleich diese Anfragen mit den virtuellen Computern gleich aus. Während die Anwendung in Azure gehostet wird, wird es drei wichtige Vorteile:

-   **Hohe Verfügbarkeit.** Hoher Verfügbarkeit bedeutet dies, dass Azure wird sichergestellt, dass Ihrer Anwendung so weit wie möglich ausgeführt wird und kann auf Clientanfragen zu antworten. Wenn Ihre Anwendung beenden (aufgrund Ausnahmefehler, beispielsweise), und klicken Sie dann Azure erkannt, und es automatisch wird starten Sie die Anwendung erneut. Wenn der Computer Ihrer Anwendung Erfahrung einige Arten von Hardware-Fehlern ausgeführt wird, klicken Sie dann Azure auch erkennt dies und automatisch erstellen ein neues virtuellen Computers auf einem anderen arbeiten physischen Computer und führen Sie den Code von dort aus. Hinweis: In der Reihenfolge für die Anwendung auf die erste Ebene von Microsoft-Servicevertrag von 99,95 % verfügbar, müssen Sie mindestens zwei virtuellen Computern ausführen von Ihrer Anwendungscode verfügen. Dadurch wird eine virtueller Computer Clientanfragen zu verarbeiten, während Azure Code eines ausgefallenen virtuellen Computers an einen neuen, gute virtuellen verschoben wird.

-   **Skalierbarkeit.** Azure können Sie einfach und dynamisch ändern der Anzahl von virtuellen Computern ausführen von Ihrer Anwendungscode die tatsächliche Belastung Ihrer Anwendung zu behandeln. So können Sie Ihrer Anwendung, um die Arbeitsbelastung anpassen, die Ihre Kunden daran platzieren werden während der nur für die virtuellen Computern müssen Sie bei Bedarf bezahlen. Wenn Sie die Anzahl der virtuellen Computern ändern möchten, reagiert Azure in Minuten lässt sich die Anzahl der virtuellen Computern ausgeführt so oft wie gewünscht dynamisch ändern.

-   **Verwaltbarkeit.** Da Azure eine Plattform als Geschenk Service (PaaS) ist, wird er die Infrastruktur (Hardware selbst, Elektrizität und Netzwerke) erforderlich, um diesen Computern beibehalten verwaltet. Azure verwaltet auch die Plattform, um ein auf dem neuesten Stand Betriebssystem mit alle die richtigen Patches und Sicherheitsupdates sowie Komponentenupdates, beispielsweise .NET Framework und Internet Information Server aus. Da alle virtuellen Computern Windows Server 2008 ausführen, bietet Azure zusätzliche Features wie diagnostic Überwachung, remote desktop-Support, Firewalls und Konfiguration zum Speichern von Zertifikat. Alle diese Features auf Nein zur Verfügung stehen zusätzliche Kosten. Beim Ausführen der Anwendungs in Azure ist die Windows Server 2008-Betriebssystem (BS) Lizenz tatsächlich enthalten. Da alle die virtuellen Computern Windows Server 2008 ausführen, funktioniert problemlos Wenn Azure ausgeführt zu jeder Code, der für Windows Server 2008 ausgeführt wird.

## <a id="concepts"> </a>Dienst grundlegenden Konzepte gehostet

Bei der Bereitstellung Ihrer Anwendungs als gehosteten Dienst in Azure ausgeführt wird als eine oder mehrere *Rollen.* Eine *Rolle* gemeint, Anwendungsdateien und Konfiguration. Sie können eine oder mehrere Rollen für eine Anwendung, jede mit einem eigenen Satz von Dateien der Anwendung und Konfiguration definieren. Für jede Rolle in Ihrer Anwendung können Sie die Anzahl der virtuellen Computern oder *Rolleninstanzen*angeben, um auszuführen. In der Abbildung unten zeigen zwei einfache Beispiele für die Anwendung als gehosteten Dienst mithilfe von Rollen und Rolleninstanzen erstellt.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Abbildung 1: Eine einzelne Rolle mit drei Instanzen (virtueller Computer), die in einem Azure Data Center ausgeführt

![Bild][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Abbildung 2: Zwei Rollen, jede mit zwei Instanzen (virtuelle Computer), die in einem Azure Data Center ausgeführt

![Bild][1]

Rolleninstanzen Anfragen werden in der Regel Internet Client eingeben das Data Center über was eines *Eingabewerte Endpunkt*aufgerufen wird. Eine einzelne Rolle kann 0 oder mehr von Endpunkten haben. Jeder Endpunkt zeigt an, ein Protokoll (HTTP, HTTPS oder TCP) und einen Anschluss. Im Allgemeinen so konfigurieren Sie eine Rolle aus, um zwei Eingabewerte Endpunkte aufweisen: HTTP hört Port 80 und HTTPS Port 443 abfragt. Die folgende Abbildung zeigt ein Beispiel von zwei verschiedenen Funktionen mit anderen Eingabewerte Endpunkten Client-Anfragen zu leiten.

![Bild][2]

Wenn Sie einen gehosteten Dienst in Azure erstellen, ist es eine öffentlich adressierbare IP-Adresse zugewiesen, mit denen Clients darauf zugreifen können. Beim Erstellen des gehosteten Diensts müssen Sie auch ein Präfix URL auswählen, die an diese Adresse zugeordnet ist. Dieses Präfix muss eindeutig sein, wie Sie das *Präfix*im Wesentlichen reservieren. cloudapp.net URL, so dass kein anderer Benutzer es haben kann. Clients kommunizieren mit Ihrer Rolleninstanzen mithilfe der URL. In der Regel werden nicht verteilen oder Veröffentlichen des Azure *Präfix*. cloudapp.net URL. Stattdessen kaufen einen Namen für die DNS-Einträge aus Ihrer DNS-Registrierungsstelle Wahl und Ihren Namen DNS-Clientanfragen zur Azure URL umleiten konfigurieren. Weitere Informationen hierzu finden Sie unter [einen benutzerdefinierten Domänennamen in Azure konfigurieren][].

## <a id="considerations"> </a>Dienst gibt gehostet

Beim Entwerfen einer Anwendungs auf einer Cloud-Umgebung ausgeführt werden, gibt es verschiedene Aspekte wie Wartezeit, hoher Verfügbarkeit und Skalierbarkeit anzustellen.

Entscheidung, wo den Code der Anwendung zu finden ist ein wichtiges Kriterium, wenn einen verwalteter Service Azure ausgeführt. Es ist üblich zur Bereitstellung von Ihrer Anwendungs auf Data Center, die am nächsten Kunden Wartezeit reduzieren und die optimale Leistung möglich sind.
Jedoch können Sie einem Data Center näher an Ihr Unternehmen oder näher an Ihre Daten auswählen, wenn Sie über einige jurisdictional oder juristischen Fragen zu dem die Daten und die Stelle, an der sie sich befindet. Es gibt sechs Data Center auf der ganzen Welt Hosten Ihrer Anwendungscode werden kann. Die folgende Tabelle zeigt die verfügbaren Speicherorte aus:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Land/Region**

</td>
<td style="width: 200px;">
**Untergeordnete Regionen**

</td>
</tr>
<tr>
<td>
USA

</td>
<td>
Süd Mittel- und North Central

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Nord- und "Westen"

</td>
</tr>
<tr>
<td>
Asien

</td>
<td>
Oder & Osten

</td>
</tr>
</tbody>
</table>
Beim Erstellen eines gehosteten Diensts, wählen Sie eine untergeordnete Region angibt, die die Position in der den Code ausgeführt werden sollen.

Um hohe Verfügbarkeit und Skalierbarkeit zu erreichen, ist es entscheidend, dass die Daten der Anwendung in einem zentralen Repository für mehrere Rolleninstanzen zugänglich gehalten werden. Um dies zu ermöglichen, bietet Azure mehrere Speicheroptionen wie Blobs, Tabellen und SQL-Datenbank. Finden Sie weitere Informationen zu diesen Technologien Artikel [Daten Storage Angebote in Azure][] . Die folgende Abbildung zeigt, wie Lastenausgleich innerhalb des Azure Data Center-Anfragen an andere Rolleninstanzen verteilt Zugriff auf denselben Datenspeicher aufweisen.

![Bild][3]

In der Regel Suche nach Ihrer Anwendungscode und Ihre Daten in derselben Data Center wie folgt niedriger Wartezeit (eine bessere Leistung) ermöglicht an Ihrer Anwendungscode greift auf die Daten. Darüber hinaus sind Sie nicht für die Bandbreite berechnet, wenn Daten innerhalb der gleichen Data Center, um verschoben werden.

## <a id="scale"> </a>Entwurf Ihrer Anwendung für skalieren

Manchmal, sollten Sie eine einzelne Anwendung (wie einer einfachen Website) machen und es in Azure gehostet wird. Aber häufig, Ihrer Anwendung möglicherweise bestehen aus mehreren Rollen, die zusammenarbeiten. Angenommen, in der folgenden Abbildung gibt es zwei Instanzen der Rolle Website, drei Instanzen der Rolle Order Processing und eine Instanz der Rolle des Berichts-Generator. Diese Funktionen werden alle zusammenarbeiten und des Codes für alle Folien in einem Paket und als einzelne Einheit nach Zeitphasen bis zum Azure bereitgestellt werden kann.

![Bild][4]

Der wichtigste Grund einer Anwendung in unterschiedliche Rollen aufteilen jeder Ausführung auf einen eigenen Satz von Rolleninstanzen (d. h., virtuellen Computern) ist die Rollen unabhängig voneinander zu skalieren. Beispielsweise können während Weihnachten viele Kunden erwerben werden Produkte aus Ihrem Unternehmen, damit erhöhen die Anzahl der Rolleninstanzen ausgeführt werden, Ihre Rolle Website als auch die Anzahl der Rolleninstanzen Ihre Rolle Order Processing ausgeführt werden soll. Nach Weihnachten können viele der Produkte zurückgegeben, so dass Sie immer noch viele Instanzen der Website, aber weniger Order Processing Instanzen möglicherweise angezeigt werden. Während der Rest des Jahres benötigen Sie nur ein paar Website und Order Processing Instanzen. In allen diesen benötigen Sie möglicherweise nur eine Instanz von Berichts-Generator. Die Flexibilität von rollenbasierte Bereitstellungen in Azure können Sie problemlos Ihrer Anwendung an Ihre geschäftlichen Bedürfnisse anpassen.

Es ist häufig die Rolle sein, die innerhalb des Diensts gehostete Instanzen kommunizieren. Beispielsweise die Website Rolle akzeptiert Bestellung des Kunden, aber dann die Reihenfolge auf die Order Processing Rolleninstanzen Verarbeitung verlagert. Die beste Methode zum Arbeitsformular übergeben werden, eine Reihe von Rolleninstanzen auf einen weiteren Satz Instanzen ist mit Warteschlange Technologie Azure, hat, der Warteschlange Dienst- oder Bus Servicewarteschlangen. Die Verwendung einer Warteschlange ist ein wichtiger Bestandteil der Geschichte hier. Die Warteschlange kann der gehosteten Dienst seine Rollen unabhängig voneinander so, dass Sie die Arbeitsbelastung gegen die Kosten abwägen skalieren. Wenn die Anzahl der Nachrichten in der Warteschlange über einen Zeitraum vergrößert wird, können Sie die Anzahl der Order Processing Rolleninstanzen einrichten skalieren. Wenn die Anzahl der Nachrichten in der Warteschlange über einen Zeitraum wird verringert, können Sie die Anzahl der Order Processing Rolleninstanzen nach unten skalieren. Auf diese Weise sind Sie nur für die Instanzen erforderlich, um die tatsächliche Arbeitsbelastung behandeln bezahlen.

Die Warteschlange enthält auch Zuverlässigkeit. Beim unten die Anzahl der Order Processing Rolleninstanzen skalieren, beschließt Azure Instanzen beenden aus. Sie könnten zum Beenden einer Instanz, die in der Mitte eine Nachricht Warteschlange verarbeitet wird. Da die Verarbeitung von Nachrichten nicht erfolgreich abgeschlossen wird, wird die Nachricht erneut mit einer anderen Reihenfolge Verarbeitung Rolleninstanz, die Sie aufnimmt und diesen verarbeitet sichtbar. Aufgrund von Warteschlange Nachricht Sichtbarkeit werden Nachrichten garantiert Gelegenheit verarbeitet abrufen. Die Warteschlange fungiert auch als ein Lastenausgleich, indem Sie dessen Nachrichten an alle Rolleninstanzen, die Nachrichten daraus anfordern effektiv verteilt.

Für die Website Rolleninstanzen, können überwachen den Datenverkehr in diese und entscheiden, um die Anzahl zu skalieren nach oben oder nach unten ebenfalls. Die Warteschlange können Sie die Anzahl der Website Rolleninstanzen unabhängig von den Order Processing Rolleninstanzen skalieren. Dies ist sehr leistungsfähige und bietet Ihnen viel Flexibilität. Natürlich Wenn zusätzliche Rollen aus Ihrer Anwendung besteht, könnten Sie hinzufügen zusätzliche Warteschlangen als Kanäle dazwischen und dieselbe Skalierung nutzen und Kosten Vorteile kommunizieren.

## <a id="defandcfg"> </a>Gehostet Dienstdefinition und Konfiguration

Bereitstellen von gehosteten Dienst in Azure erfordert, dass Sie auch eine Definition Dienstdatei und eine Konfiguration Dienstdatei besitzen. Beide Dateien sind XML-Dateien, und ermöglichen Ihnen Bereitstellungsoptionen für Ihre gehosteten Dienst deklarativ angegeben. Die Definition Dienstdatei werden alle Rollen, aus denen Ihre gehosteten Dienst und wie sie kommunizieren. Die Konfiguration Dienstdatei beschreibt die Anzahl der Instanzen für jede Rolle und Einstellungen verwendet, um jede Instanz der Rolle konfigurieren.

## <a id="def"> </a>Der Definition Dienstdatei

Wie kann ich die Definition Service (CSDEF) erwähnt, ist die Datei eine XML-Datei, die die verschiedenen Funktionen beschrieben, die die vollständige Anwendung bilden. Das vollständige Schema für die XML-Datei finden Sie hier: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx] [].
Die Datei CSDEF enthält ein WebRole oder WorkerRole Element für jede Rolle, die Sie in Ihrer Anwendung wünschen. Bereitstellen einer Rolle als Webrolle (mit dem Element WebRole) bedeutet, dass der Code auf eine Instanz der Rolle mit Windows Server 2008 und Internet Information Server (IIS) ausgeführt wird.
Bereitstellen einer Rolle aus, wie eine Worker-Rolle (mit dem Element WorkerRole) bedeutet, dass die Instanz der Rolle Windows Server 2008 daran (IIS nicht installiert werden).

Natürlich können Sie erstellen und Bereitstellen eine Worker-Rolle, die eine andere Methode zur eingehenden Webanfragen Überwachung verwendet (z. B. konnte Code erstellen und Verwenden eines .NET HttpListener). Da die Rolleninstanzen allen Windows Server 2008 ausgeführt werden, kann Code Vorgänge aus, die normalerweise zur Anwendung unter Windows Server verfügbar sind
2008.

Für jede Rolle geben Sie die gewünschte Größe virtueller Computer, die Instanzen dieser Rolle verwendet werden sollen. Die folgende Tabelle zeigt die verschiedenen virtuellen Computer Größen heute verfügbar und die Attribute der einzelnen:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Virtueller Speicher**

</td>
<td>
**CPU**

</td>
<td>
**RAM**

</td>
<td>
**Datenträger**

</td>
<td>
**Höchstwert Netzwerk e/a**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Zusätzliche Small**

</td>
<td>
1 x 1,0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kleine**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Mittel**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 MB

</td>
</tr>
<tr align="left" valign="top">
<td>
**Große**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Sehr groß**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800/s

</td>
</tr>
</tbody>
</table>
Sie unterliegen stündlich für jeden virtuellen Computer Sie als eine Instanz der Rolle verwenden und auch unterliegen für alle Daten, dass Ihre Rolleninstanzen außerhalb des Data Center senden. Sie unterliegen nicht für das Data Center eingeben von Daten. Weitere Informationen finden Sie unter [[Azure Preise]]. Im Allgemeinen ist es sinnvoll, viele kleine Rolleninstanzen im Gegensatz zu wenigen großen Instanzen verwenden, damit die Anwendung mehr flexibel in Bezug auf Fehler ist. Nach allen, die weniger Rolleninstanzen vorhanden sind, weitere verheerende ein Fehler in einer von ihnen besteht darin, eine allgemeine Anwendung. Darüber hinaus müssen wie oben erwähnt, Sie bereitstellen mindestens zwei Instanzen für jede Rolle um 99,95 % Servicelevel zu erhalten, die Microsoft bietet.

Die Dienstdatei Definition (CSDEF) ist ebenfalls in dem Sie viele Attribute für jede Rolle in Ihrer Anwendung angeben möchten. Hier sind einige der hilfreicher Elemente zur Verfügung:

-   **Zertifikate**. Verwenden Sie Zertifikate zum Verschlüsseln von Daten oder, wenn der Webdienst SSL unterstützt. Alle Zertifikate auf Azure hochgeladen werden müssen. Weitere Informationen finden Sie unter [Verwalten von Zertifikaten in Azure][]. Diese Einstellung XML-Installationen zuvor hochgeladen Zertifikate in die Instanz der Rolle des Zertifikats Store, damit sie nach Ihrer Anwendungscode verwendbar sind.

-   **Konfiguration Einstellungsnamen**. Für Werte, die Sie Ihre Anwendung(en) zu lesen, während Sie auf eine Instanz der Rolle ausführen möchten. Der tatsächliche Wert der Konfiguration Einstellungen im Dienstkonfigurationsdatei (CSCFG) festgelegt, der zu einem beliebigen Zeitpunkt aktualisiert werden kann, ohne dass Sie den Code erneut bereitstellen. Tatsächlich können Sie Ihre Anwendungen in an, dass die geänderte Konfigurationswerte erkennen, ohne eine ausführliche codieren.

-   **Eingabemethoden Endpunkte**. Hier geben Sie alle Endpunkte HTTP, HTTPS oder TCP (mit Ports), die Sie nach außen über Ihre *Präfix*verfügbar machen möchten. cloadapp.net URL. Wenn Ihre Rolle Azure bereitgestellt werden, wird automatisch die Firewall auf die Instanz der Rolle konfiguriert werden.

-   **Interne Endpunkte**. Hier geben Sie eine beliebige HTTP oder TCP Endpunkte die gewünschte verfügbar gemacht an andere Rolleninstanzen, die als Teil Ihrer Anwendung bereitgestellt werden. Interne Endpunkte können alle Rolleninstanzen innerhalb der Anwendung miteinander sprechen jedoch nicht möglich auf eine beliebige Rolleninstanzen, die sich außerhalb der Anwendung befinden.

-   **Module importieren**. Diese werden optional hilfreiche Komponenten auf Ihre Rolleninstanzen installieren. Komponenten für die Diagnose Überwachung, Remotedesktop und Azure verbinden (wodurch die Instanz der Rolle zum Zugriff auf lokale Ressourcen über einen sicheren Kanal) vorhanden sind.

-   **Lokaler Speicher**. Dies weist ein Unterverzeichnis auf die Instanz der Rolle für eine Anwendung verwenden. Es wird in den [Daten Storage Angebote in Azure][] Artikel ausführlicher beschrieben.

-   **Startaufgaben**. Beim Start von Vorgängen bieten Ihnen eine Möglichkeit zum Installieren erforderlicher Komponenten auf eine Instanz der Rolle aus, wie es Mal gestartet wird. Die Aufgaben können als Administrator bei Bedarf erhöhten ausgeführt werden.

## <a id="cfg"> </a>Der Konfiguration Dienstdatei

Die Dienstkonfigurationsdatei (CSCFG) ist eine XML-Datei, die Einstellungen zu, die geändert werden kann beschrieben, ohne das erneute Bereitstellung Ihrer Anwendung. Das vollständige Schema für die XML-Datei finden Sie hier: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Die CSCFG-Datei enthält ein Element Rolle für jede Rolle in Ihrer Anwendung. Hier sind einige der Elemente, die Sie in der Datei CSCFG angeben können:

-   **Version des Betriebssystems**. Dieses Attribut ermöglicht Ihnen die Auswahl die Betriebssystem (BS) gewünschten verwendete Version für alle Rolleninstanzen den Anwendungscode ausgeführt wird. Diese OS wird als *Gast OS*bezeichnet, und jede neue Version enthält die neuesten Sicherheitsupdates und Updates verfügbar zum Zeitpunkt der Gast OS freigegeben ist. Wenn Sie den Wert der OsVersion Attribut festgelegt "\*", und klicken Sie dann Azure Gast-BS auf jeder der Rolleninstanzen automatisch aktualisiert, sobald neue Gast-BS-Versionen zur Verfügung stehen. Sie können jedoch von automatischen Updates entscheiden, indem Sie bestimmten Gast Betriebssystemversion auswählen. Beispielsweise das Attribut OsVersion auf einen Wert der Einstellung "WA – Gast-OS-2,8\_201109-01" bewirkt, dass alle Rolleninstanzen zu erhalten, klicken Sie auf diese Webseite sein: [http://msdn.microsoft.com/library/hh560567.aspx][]. Weitere Informationen zu Gast-BS-Versionen finden Sie unter [Verwalten von Upgrades auf Azure Gäste OS].

-   **Instanzen**. Wert des Elements gibt die Anzahl der Rolleninstanzen Ausführen des Codes für eine bestimmte Rolle bereitgestellte gewünschten an. Da Sie eine neue CSCFG-Datei in Azure (ohne erneute Bereitstellung Ihrer Anwendung) hochladen können, ist es im Grunde Einsteiger-bis zum Ändern Sie den Wert für dieses Element und Hochladen einer neuen CSCFG Datei dynamisch vergrößern oder verkleinern die Anzahl der Rolleninstanzen den Anwendungscode ausgeführt wird. So können Sie problemlos Skalieren Ihrer Anwendung, nach oben oder nach unten bis zum erfüllen tatsächlichen Arbeitsbelastung erfordert während auch steuern, wie viel Sie für die Ausführung der Rolleninstanzen belastet.

-   **Konfiguration Festlegen von Werten**. Dieses Element zeigt Werte für Einstellungen (wie in der Datei CSDEF definiert) an. Ihre Rolle kann diese Werte lesen, während er ausgeführt wird. Diese Konfiguration Einstellungen-Werte werden in der Regel für die Verbindungszeichenfolgen mit SQL-Datenbank oder in den Azure-Speicher verwendet, aber sie können für einen bestimmten Zweck gewünschte verwendet werden.

## <a id="hostedservices"> </a>Erstellen und Bereitstellen eines gehosteten Diensts

Erstellen eines gehosteten Diensts erfordert, wechseln Sie zum [Azure-Verwaltungsportal] und einen gehosteten Dienst durch Angabe eines DNS-Präfix bereitzustellen und das Data Center schließlich möchten, Ihre Code aus. Klicken Sie dann in Ihrer Entwicklungsumgebung Ihrer Definition (CSDEF) Dienstdatei erstellen, Erstellen Ihrer Anwendungscode und Paket (PLZ) alle diese Dateien in eine Service-Paketdatei (CSPKG). Sie müssen auch Ihre Konfiguration (CSCFG) Dienstdatei vorbereiten. Zum Bereitstellen von Ihrer Rolle Laden Sie die Dateien CSPKG und CSCFG mit der Azure Service Management-API hoch. Nachdem Azure bereitgestellt wird bereitstellen Rolleninstanzen in Data Center (je nach Konfigurationsdaten), extrahieren Ihrer Anwendungscode aus dem Paket, Kopieren in die Rolleninstanzen und die Instanzen starten. Nun kann Code einsatzbereit.

Die folgende Abbildung zeigt die CSPKG und CSCFG-Dateien, die Sie auf Ihrem Computer zu erstellen. Die Datei CSPKG enthält die Datei CSDEF und den Code für zwei Rollen. Nach dem Hochladen der CSPKG und CSCFG-Dateien mit der Azure Service Management-API, erstellt Azure die Rolleninstanzen in Data Center. In diesem Beispiel angegebene Datei CSCFG, dass Azure drei Instanzen der Rolle erstellen sollten \#1 und zwei Instanzen der Rolle \#2.

![Bild][5]

Weitere Informationen zum Bereitstellen aktualisieren und neu zu konfigurieren Ihrer Rollen, finden Sie unter [Bereitstellen und Aktualisieren von Azure Applications][] Artikel.<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Verweise

-   [Erstellen eines gehosteten Diensts für Azure][]

-   [Verwalten von gehosteten Dienste in Azure][]

-   [Migrieren von Applications to Azure][]

-   [Konfigurieren von Azure-Anwendung][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Geschrieben von Jeffrey Richter (Wintellect)</p>

</div>

  [Vorteile der Azure-Anwendung-Modell]: #benefits
  [Gehosteten Dienst grundlegenden Konzepte]: #concepts
  [Gibt gehosteten Dienst]: #considerations
  [Entwerfen der Anwendung für die Skalierung]: #scale
  [Gehosteten Dienstdefinition und Konfiguration]: #defandcfg
  [Die Definition Dienstdatei]: #def
  [Die Konfiguration Dienstdatei]: #cfg
  [Erstellen und Bereitstellen eines gehosteten Diensts]: #hostedservices
  [Verweise]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Konfigurieren einen benutzerdefinierten Domänennamen in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Daten Storage Angebote in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Verwalten von Zertifikaten in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.Microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.Microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Verwalten von Upgrades auf die Azure Gäste OS]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure-Verwaltungsportal]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Bereitstellen und Aktualisieren von Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Erstellen eines gehosteten Diensts für Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Verwalten von gehosteten Dienste in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migrieren von Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Konfigurieren von Azure-Anwendung]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx

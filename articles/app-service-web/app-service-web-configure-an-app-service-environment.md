<properties
    pageTitle="Wie Sie eine App-Service-Umgebung konfigurieren | Microsoft Azure"
    description="Konfiguration, Verwaltung und Überwachung der App-Service-Umgebungen"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Konfigurieren einer App-Service-Umgebung #

## <a name="overview"></a>(Übersicht) ##

Besteht aus einer Azure App-Service-Umgebung auf hoher Ebene mehrere Hauptkomponenten:

- Berechnen von Ressourcen, die in der App-Service-Umgebung gehostet Dienst ausgeführt werden
- Speicher
- Eine Datenbank
- Ein Classic(V1) oder Ressource Manager(V2) Azure Virtual Network (VNet) 
- Ein Subnetz mit dem App-Service-Umgebung gehostet Dienst darin ausführen

### <a name="compute-resources"></a>Berechnen von Ressourcen

Sie verwenden die berechnen Ressourcen für Ihre vier Ressourcenpools.  Jede App-Service-Umgebung (ASE) verfügt über eine Reihe von front-End und drei mögliche Worker Pools. Sie müssen nicht alle drei Worker Pools – verwenden, wenn Sie möchten, können Sie einfach verwenden eine oder zwei.

Die Hosts in den Ressourcenpools (front-End und Kollegen) sind nicht direkt zu Mandanten zugegriffen werden kann. (Remotedesktopprotokoll) Sie können nicht zu verbinden, deren Bereitstellung ändern oder dienen als Administrator diesen.

Sie können Ressourcen Ressourcenpool Menge und Größe festlegen. In einer ASE müssen Sie vier Größenoptionen, bis P4 P1 beschriftet sind. Details zu diesen Größen und deren Preise finden Sie unter [App Preisen](../app-service/app-service-value-prop-what-is.md).
Die Menge oder die Größe ändern, wird eine Skalierung bezeichnet.  Nur eine Skalierung kann nacheinander ausgeführt werden.

**Vorderseite endet**: der front-End sind die Endpunkte HTTP-/HTTPS für Ihre apps, die in Ihrem ASE gehalten werden. Sie Ausführen nicht in der front-End Auslastung.

- Eine ASE beginnt mit zwei P2s, welche ist für Test-/Auslastung und Low-Level Produktionsarbeitslasten ausreichend. Es wird dringend empfohlen P3s für moderieren für Produktionsarbeitslasten beanspruchen.
- Für moderieren für beanspruchen Produktionsarbeitslasten empfehlen wir aktuell mindestens vier P3s, um sicherzustellen, dass es ausreichend front-End ausgeführt, wenn Sie geplante Wartung erfolgt sind. Geplante Wartungsaktivitäten werden unten eine front-End nacheinander angezeigt. Dies reduziert insgesamt verfügbare Front-End-Kapazität während Wartungsaktivitäten.
- Sie können nicht sofort eine neue Front-End-Instanz hinzufügen. Sie können zwischen 2 und 3 Stunden bereitstellen übernehmen.
- Für die weitere Feinabstimmung der Maßstab, sollten Sie den Prozentsatz der CPU, Arbeitsspeicher Prozentsatz und aktiven Anfragen Kennzahlen für den Front-End-Pool überwachen. Wenn die CPU oder Arbeitsspeicher Prozentsätze über 70 % sind, wenn P3s ausgeführt wird, fügen Sie weitere front-End hinzu. Wenn der Wert des aktiven Anfragen auf 15.000 zu 20.000 Besprechungsanfragen pro front-End Durchschnitt, sollten Sie auch weitere front-End hinzufügen. Das Ziel ist CPU- und Prozentsätzen darunter 70 % beibehalten und aktiven Abfragen aus um unter 15.000 Abfragen pro Vorderseite zu beenden, wenn Sie P3s ausführen.  

**Kollegen**: die Arbeitskräften sind, wo Ihre apps tatsächlich ausgeführt. Beim Skalieren Ihrer App-Service-Pläne, die von Kollegen im zugeordneten Arbeitskollegen Pool verwendet.

- Sie können keine sofort Kollegen hinzufügen. Er kann von 2 auf Bereitstellen von 3 Stunden dauern, unabhängig davon, wie viele hinzugefügt wird.
- Die Größe einer Ressource berechnen für alle Ressourcenpool Skalierung dauert 2 und 3 Stunden pro Domäne aktualisieren. Es gibt 20 Update Domänen in einem ASE ein. Wenn Sie die Größe berechnen von einem Ressourcenpool Worker mit 10 Instanzen skaliert, dauert es konnte zwischen 20 und 30 Stunden in Anspruch.
- Wenn Sie die Größe der berechnen Ressourcen, die in einem Ressourcenpool Worker verwendet werden ändern, werden Sie kalt Starts der apps verursachen, die in diesem Pool Worker ausgeführt werden.

So ändern Sie die Größe berechnen Ressource einem Ressourcenpool Worker, die keine apps ausgeführt wird die schnellste Möglichkeit ist:

- Verkleinern Sie die Anzahl der Instanzen 0 ein. Es dauert etwa 30 Minuten zum Freigeben Ihrer Instanzen.
- Wählen Sie die neue berechnen Größe und Anzahl der Instanzen. Von hier aus dauert es zwischen 2 und 3 Stunden in Anspruch.

Wenn Ihre apps berechnen Ressource vergrößern benötigen, können nicht Sie die vorherige Anleitung nutzen. Statt ändern die Größe der Ressourcenpool Worker, die diese apps gehostet wird, können Sie ein anderes Worker Pool mit Kollegen, der die gewünschte Größe zu füllen und bewegen Sie Ihre apps auf diesem Pool.

- Erstellen Sie die zusätzlichen Instanzen von der Größe der benötigten berechnen in ein anderes Worker Pool aus. Dies wird von 2 auf 3 Stunden dauern.
- Neuzuweisen der App-Service-Pläne, die die apps gehostet werden, die einen größeren an den neu konfigurierten Worker Pool benötigen. Dies ist eine schnelle Vorgang, der weniger als eine Minute abgeschlossen haben soll.  
- Verkleinern des ersten Worker Pools aus, wenn Sie diese nicht verwendeten Instanzen nicht mehr benötigen. Dieser Vorgang dauert etwa 30 Minuten.

**Automatische Skalierung**: eines der Tools, die Sie zum Verwalten Ihrer Ressourcenverbrauch berechnen helfen können die automatische Skalierung ist. Sie können die automatische Skalierung für Front-End- oder Arbeitskollegen Pools. Aktionen Dinge wie erhöhen die Instanzen eines beliebigen Typs Ressourcenpool am Morgen und am Abend zu verringern. Oder vielleicht können Sie Instanzen hinzufügen, wenn die Anzahl der Kollegen, die in einem Ressourcenpool Worker verfügbar sind unter einen bestimmten Schwellenwert fällt.

Wenn Sie Regeln für die automatisch Skalieren um berechnen Ressource Ressourcenpool Kennzahlen festlegen möchten, klicken Sie dann orientieren Sie die Zeit, die Bereitstellung erforderlich sind. Weitere Details zu automatische Skalierung App-Service-Umgebungen, finden Sie unter [Skalieren in einer App-Service-Umgebung konfigurieren][ASEAutoscale].

### <a name="storage"></a>Speicher

Jede ASE wird mit 500 GB Speicher konfiguriert. Hier wird über alle apps in die ASE verwendet. Dieser Speicherplatz ist Bestandteil der ASE und aktuell kann nicht gewechselt werden, um Ihre Speicherplatz zu verwenden. Wenn Sie zur Sicherheit oder virtuelles Netzwerkrouting Anpassungen vornehmen möchten, müssen Sie weiterhin Zugriff auf Azure-Speicher zulassen oder die ASE kann nicht ausgeführt werden.

### <a name="database"></a>Datenbank

Die Datenbank enthält Informationen, die zum Definieren der Umgebung als auch die Details zu den apps, die darin enthaltenen ausgeführt werden. Dies ist ebenfalls ein Teil des Abonnements Azure frei. Es ist nicht für ein Element, das Sie eine direkte Möglichkeit zum Bearbeiten haben. Wenn Sie zur Sicherheit oder virtuelles Netzwerkrouting Anpassungen vornehmen möchten, müssen Sie Zugriff auf SQL Azure – weiterhin zulassen oder die ASE kann nicht ausgeführt werden.

### <a name="network"></a>Netzwerk

Eine, die Sie bei der Erstellung der ASE vorgenommen oder eine, die Sie zuvor festgelegt haben, kann die VNet, die mit Ihrem ASE verwendet wird sein. Wenn Sie während der Erstellung ASE im Subnetz erstellen, wird die ASE in derselben Ressourcengruppe als das virtuelle Netzwerk werden erzwungen. Wenn Sie die von Ihrem ASE verwendet, um sich mit Ihrer VNet unterscheiden Ressourcengruppe benötigen, müssen Sie Ihre ASE mithilfe einer Ressource-Manager Vorlage erstellen.

Es gibt einige Einschränkungen auf das virtuelle Netzwerk, das für eine ASE verwendet wird:
 
- Das virtuelle Netzwerk muss ein Landes-/ virtuelles Netzwerk.
- Es muss ein Subnetz mit mindestens 8 Adressen, wo die ASE bereitgestellt wird.
- Nach einem Subnetz eine ASE hosten verwendet wird, kann der Adresse der Subnetz Zellbereich geändert werden. Aus diesem Grund empfehlen wir, dass das Subnetz mindestens 64 Adressen, um für alle zukünftigen ASE Wachstum enthält.
- Es kann nichts anderes in das Subnetz jedoch die ASE.

Im Gegensatz zu den angebotenen Service, die die ASE, das [virtuelle Netzwerk] enthält[ virtualnetwork] und Subnetz Benutzer gesteuert werden.  Sie können über die virtuelle Netzwerk-Benutzeroberfläche oder PowerShell Netzwerks virtuelle verwalten.  Eine ASE kann in einer Classic oder Ressource-Manager VNet bereitgestellt werden.  Dem Portal und dem API Erfahrung unterscheiden sich geringfügig zwischen Classic und Ressourcenmanager VNets ASE-Benutzeroberfläche ist jedoch identisch.

Die VNet, mit dem eine ASE hosten, kann entweder privaten RFC1918 IP-Adressen oder es kann öffentliche IP-Adressen verwenden.  Wenn Sie einen IP-Bereich verwenden, die nicht über RFC1918 sind möchten (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) müssen Sie erstellen Ihre VNet und Subnetz, von Ihrer ASE vor ASE Creation verwendet werden soll.

Da diese Funktion der Azure-App-Dienst in Ihrem Netzwerk virtuelle platziert, dies bedeutet, dass Ihre apps, die in Ihrem ASE gehostet werden jetzt Ressourcen zugreifen können, die direkt über ExpressRoute oder Website-zu-Standort VPN (VPN) zur Verfügung gestellt werden. Der apps, die innerhalb der App-Service-Umgebung sind ist keine zusätzliche Netzwerkfeatures Zugriff auf Ressourcen das virtuelle Netzwerk zur Verfügung, die Ihre App-Service-Umgebung befindet, erforderlich. Dies bedeutet, dass Sie keinen VNET Integration oder Hybrid-Verbindungen verwenden, um Ressourcen in oder die Verbindung zu Ihrem Netzwerk virtuelle aufrufen müssen. Sie können weiterhin beide diese Funktionen zwar den Zugriff auf Ressourcen in Netzwerken verwenden, die nicht mit Ihrem virtuellen Netzwerk verbunden sind.

Beispielsweise können Sie VNET Integration mit einem virtuellen Netzwerk integriert werden soll, die im Rahmen Ihres Abonnements ist, aber nicht mit dem virtuellen Netzwerk, dem Ihre ASE ist verbunden ist. Weiterhin können Hybrid Verbindungen auf Ressourcen zugreifen Sie auch, die in anderen Netzwerken, wie Sie normalerweise können sind.  

Wenn Sie virtuelle Netzwerks mit einer ExpressRoute VPN konfiguriert haben, sollten Sie einige der Weiterleitung Anforderungen berücksichtigen, die eine ASE weist. Es gibt einige User defined Routing (UDR) Konfigurationen, die nicht mit einer ASE kompatibel sind. Weitere Details zum Ausführen einer ASE in einem virtuellen Netzwerk mit ExpressRoute, finden Sie unter [Ausführen eines App-Service-Umgebung in einem virtuellen Netzwerk mit ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Schützen von eingehenden Datenverkehr

Es gibt zwei Verfahren zum eingehenden Datenverkehr an Ihre ASE steuern.  Können Netzwerk Sicherheit Groups(NSGs) um zu steuern, welche-IP-Adressen Ihrer ASE zugreifen können, wie hier [So steuern Sie eingehenden Datenverkehr in einer App-Service-Umgebung](app-service-app-service-environment-control-inbound-traffic.md) beschrieben, und Sie können auch Ihre ASE mit einem internen laden Balancer(ILB) konfigurieren.  Diese Features können auch zusammen verwendet werden, wenn Sie zum Einschränken des Zugriffs NSGs zu Ihrer ILB ASE verwenden möchten.

Beim Erstellen einer ASE erstellt er eine VIP in Ihrer VNet.  Es gibt zwei Arten für VIP, internen und externen aus.  Beim Erstellen einer ASE mit einer externen VIP sind Ihre apps in Ihrem ASE über eine geroutet IP-Adresse von Internet zugänglich. Wenn Sie Internal Ihrer ASE auswählen werden mit einer ILB konfiguriert werden und werden nicht direkt Internet zugänglich.  Eine ILB ASE erfordert immer noch eine externe VIP aber dient nur für Azure Management und Wartung-Zugriff.  

Während der Erstellung der ILB ASE die Unterdomäne die ILB ASE untersuchten bereitstellen und verwalten Ihre eigenen DNS-Einträge für die Unterdomäne, die Sie angeben, müssen.  Da Sie den Unterdomänennamen festlegen müssen Sie das Zertifikat für HTTPS-Zugriff verwalten.  Nach der Erstellung ASE werden Sie aufgefordert, das Zertifikat bereitzustellen.  Weitere Informationen zum Erstellen und Verwenden einer ILB ASE lesen Sie [verwenden eine interne Lastenausgleich mit einer App-Service-Umgebung]Informationen[ILBASE]. 


## <a name="portal"></a>Portal

Sie können verwalten und überwachen Ihre App-Service-Umgebung mithilfe der Benutzeroberfläche in der Azure-Portal. Wenn Sie eine ASE verfügen, können Sie häufig das App-Service-Symbol auf der Randleiste angezeigt. Dieses Symbol wird verwendet, um die App-Service-Umgebungen Azure-Portal darstellen:

![Symbol für App-Service-Umgebung][1]

Um die Benutzeroberfläche zu öffnen, die alle Ihre App-Service-Umgebungen aufgelistet sind, verwenden Sie das Symbol oder wählen Sie den Doppelpfeil (">" Symbol) am unteren Rand der Randleiste Umgebungen der App-Dienst auswählen. Durch Auswahl der ASEs aufgeführt, öffnen Sie die Benutzeroberfläche, die zum Überwachen und Verwalten des verwendet wird.

![Benutzeroberfläche für die Überwachung und Verwaltung Ihrer App-Service-Umgebung][2]

Dieser erste Blade zeigt einige Eigenschaften der ASE, zusammen mit einem metrischen Diagramm pro Ressourcenpool an. Auch sind einige der Eigenschaften, die im Block **Essentials** angezeigt werden links, die von der Blade werden geöffnet, die zugeordnet ist. Beispielsweise können Sie auswählen, den Namen des **Virtuellen Netzwerks** , um die Benutzeroberfläche, die das virtuelle Netzwerk, das Ihre ASE, in ausgeführt wird zugeordnet zu öffnen. **App-Service-Pläne** und **-Apps** Öffnen von Blades, in denen diese Elemente aufgelistet, die in Ihrem ASE sind.  

### <a name="monitoring"></a>Für die Überwachung

Die Diagramme können Sie eine Vielzahl von Performance-Werte in den einzelnen Ressourcenpool finden Sie unter. Für den Front-End-Pool können Sie die durchschnittliche CPU- und überwachen. Für Worker Pools können Sie die Menge an, die verwendet wird und der Menge, die zur Verfügung überwachen.

Mehrere App-Verwaltungsdienst, Pläne vorgenommen werden können, von den Kollegen in einem Worker Ressourcenpool verwenden. Die Arbeitsbelastung wird nicht auf die gleiche Weise, wie der Front-End-Webservern, also die CPU- und die Verwendung nicht zur Verfügung zu stellen viele beim hilfreiche Informationen. Unbedingt mehr nachverfolgen, wie viele sind verfügbar – und Kollegen, die Sie verwendet haben, insbesondere dann, wenn Sie dieses System für andere Personen verwenden verwalten.  

Können Sie auch alle der Metrik, die nachverfolgt werden können, die in den Diagrammen zum Einrichten von Benachrichtigungen. Einrichten von Benachrichtigungen hier funktioniert genauso wie an anderer Stelle in der App-Dienst. Sie können eine Benachrichtigung Webpart Benutzeroberfläche **Benachrichtigungen** oder Drilldowns in einer beliebigen Metrik Benutzeroberfläche und **Benachrichtigung hinzufügen**auswählen festlegen.

![Kennzahlen UI][3]

Die Metrik, die nur erläutert wurden sind die App-Service-Umgebung Metrik. Es gibt auch Kriterien, die bei der App-Plan Dienstalter verfügbar sind. Dies ist die Überwachung CPU- und viele sinnvoll macht.

In einer ASE sind alle die App Dienstpläne dedizierten App-Service-Pläne. Das heißt, die der einzige apps, die auf den Hosts zugewiesen, dass die App-Serviceplan sind apps in dieser App Serviceplan ausgeführt werden. Damit Ihre App-Serviceplan Details angezeigt wird, zeigen Sie Ihre App-Serviceplan aus jeder der Listen in der Benutzeroberfläche ASE oder **Durchsuchen der App-Verwaltungsdienst-Pläne** (der alle Listen).   

### <a name="settings"></a>Einstellungen

Innerhalb der Blade ASE steht Ihnen der Abschnitt **Einstellungen** , der mehrere wichtige Funktionen enthält:

**Einstellungen** > **Eigenschaften**: das Blade **Einstellungen** automatisch geöffnet wird, wenn Sie Ihre ASE Blade abrufen. Oben befindet sich **Eigenschaften**. Es gibt eine Reihe von Elementen in hier die redundant, was Sie **Essentials**angezeigt werden, aber sehr nützlich wird **Virtuelle IP-Adresse**als auch **Ausgehende IP-Adressen**.

![Einstellungen Blade- und Eigenschaften][4]

**Einstellungen** > **IP-Adressen**: Wenn Sie eine app IP-Secure Sockets Layer (SSL) in Ihrem ASE erstellen, benötigen Sie eine SSL IP-Adresse. Um eine zu erhalten, benötigt der ASE SSL IP Adressen, die sie besitzt, die zugewiesen werden können. Beim Erstellen einer ASE es hat einen SSL IP-Adresse für diesen Zweck, aber Sie können weitere hinzufügen. Vorhanden ist eine Gebühr für zusätzliche IP SSL Adressen, wie in der [App-Verwaltungsdienst Preise] dargestellt[ AppServicePricing] (im Abschnitt auf SSL-Verbindungen). Der zusätzliche Kurs wird IP SSL Preis an.

**Einstellungen** > **Front-End-Pool** / **Worker Pools**: jede diese Ressource Ressourcenpool Karten bietet die Möglichkeit, Informationen nur auf die Ressourcenpool, zusätzlich zum Bereitstellen von Steuerelementen, um die Ressourcenpool vollständig Skalieren finden Sie unter.  

Das base Blade für jede Ressourcenpool enthält ein Diagramm mit Kennzahlen für die Ressourcenpool an. Wie mit den Diagrammen aus dem Blade ASE können wechseln Sie in das Diagramm und Einrichten von Benachrichtigungen wie gewünscht. Festlegen einer Warnung aus dem Blade ASE für einen bestimmten Ressourcenpool ist dasselbe, wie Sie es aus dem Ressourcenpool ausführen. Aus dem Worker Ressourcenpool **Einstellungen** Blade Sie haben Zugriff auf die Datei Apps oder App-Service-Pläne, die in diesem Pool Worker ausgeführt werden.

![Worker Ressourcenpool Einstellungen UI][5]

### <a name="portal-scale-capabilities"></a>Portal Maßstab-Funktionen  

Es gibt drei Maßstab Vorgänge aus:

- Ändern der Anzahl der IP-Adressen in der ASE, die für die Verwendung von SSL IP verfügbar sind.
- Ändern der Größe der Ressource berechnen, die in einem Ressourcenpool verwendet wird.
- Ändern der Anzahl der berechnen Ressourcen, die in einem Ressourcenpool entweder manuell oder über automatische Skalierung verwendet werden.

Es gibt drei Methoden zum Steuern, wie viele Server, die Sie in Ihrem Ressourcenpools aufweisen, im Portal:

- Eine Skalierung aus dem Hauptfenster ASE Blade oben. Sie können mehrere Maßstab Konfiguration der Front-End- und Arbeitskollegen Pools ändern. Sie sind alle in einem Vorgang angewendet.
- Eine manuelle Skalierung aus dem bestimmte Ressource Ressourcenpool **Maßstab** Blade, befindet sich unter **Einstellungen**.
- Automatische Skalierung, die Sie aus dem bestimmte Ressource Ressourcenpool **Maßstab** Blade haben eingerichtet.

Wenn die Skalierung auf das Blade ASE verwenden möchten, ziehen Sie den Schieberegler auf die Menge an, und speichern Sie. Diese Benutzeroberfläche unterstützt auch die Größe ändern.  

![Maßstab UI][6]

Um die manuelle oder automatisch skalieren-Funktionen in einer bestimmten Ressourcenpool verwenden, wechseln Sie zu **Einstellungen** > **Front-End-Pool** / **Arbeitskollegen Pools** je nach Bedarf. Öffnen Sie den Pool, die Sie ändern möchten. Wechseln Sie zu **Einstellungen** > **Skalierung** oder **Einstellungen** > **Skalieren nach oben**. Das Blade **Maßstab,** können Sie Instanz Menge steuern. **Skalieren** ermöglicht Ressourcengröße zu steuern.  

![Skalierungseinstellungen-Benutzeroberfläche][7]

## <a name="fault-tolerance-considerations"></a>Fehlertoleranz-Aspekte

Sie können eine App-Service-Umgebung um bis zu 55 Ressourcen für insgesamt berechnen verwenden konfigurieren. Dieser 55 berechnen Ressourcen können nur 50 zu Host Auslastung verwendet werden. Der Grund dafür hat zwei Seiten. Es ist mindestens 2 berechnen Front-End-Ressourcen.  Bis zu 53 zur Unterstützung der Zuteilung Worker-Pool bleiben. Um Fehlertoleranz bereitstellen zu können, müssen Sie eine Ressource für die zusätzliche berechnen verfügen, die nach den folgenden Regeln zugeordnet ist:

- Jeder Worker Pool benötigt mindestens 1 berechnen von zusätzlichen Ressourcen, die für eine Arbeitsbelastung zugewiesen werden nicht verfügbar ist.
- Wenn die Menge der berechnen von Ressourcen in einem Ressourcenpool Worker über einen bestimmten Wert geht, ist eine andere berechnen Ressource für Fehlertoleranz erforderlich. Dies ist nicht der Fall, in dem Front-End-Pool.

Innerhalb eines einzelnen Worker Ressourcenpool sind die Fehlertoleranz-Anforderungen, die für einen angegebenen Wert von X zu einem Ressourcenpool Worker zugeordnete Ressourcen berechnet:

- X ist zwischen 2 und 20, wie viele Ressourcen verwendbar berechnen, mit denen Sie für Auslastung, X-1.
- X ist zwischen 21 und 40, wie viele Ressourcen verwendbar berechnen, mit denen Sie für Auslastung, X-2.
- Ist X zwischen 41 und 53, wird die Menge des verwendbar berechnen Ressourcen, die Sie für Auslastung verwenden können X-3.

Minimale Platzbedarf weist 2 Front-End-Webservern und 2 Kollegen.  Mit den oben genannten Aussagen klicken Sie dann hier ein paar Beispiele verdeutlichen möchten:  

- Wenn Sie in einem einzelnen Ressourcenpool 30 Worker verfügen, können 28 hiervon Host Auslastung verwendet werden.
- Wenn Sie in einem einzelnen Ressourcenpool 2 Worker verfügen, können 1 Host Auslastung verwendet werden.
- Wenn Sie in einem einzelnen Ressourcenpool 20 Worker verfügen, können 19 Host Auslastung verwendet werden.  
- Wenn Sie in einem einzelnen Ressourcenpool 21 Kollegen haben, kann dann noch nur 19 zu Host Auslastung verwendet werden.  

Der Fehlertoleranz Aspekt ist wichtig, aber Sie weiterhin Beachten Sie, wie Sie über bestimmte Schwellenwerte skalieren müssen. Wenn Sie mehr Kapazität von 20 Instanzen hinzufügen möchten, wechseln Sie dann zu 22 oder höher, da 21 irgendeiner Weitere Eigenschaft hinzufügen nicht. Dasselbe gilt vertraut über 40, wo finde ich die nächste Nummer, die addiert Kapazität 42.  

## <a name="deleting-an-app-service-environment"></a>Löschen einer App-Service-Umgebung ##

Wenn Sie eine App-Service-Umgebung löschen möchten, verwenden Sie die Aktion **Löschen** einfach am oberen Rand der App-Service-Umgebung Blade. Wenn Sie dies tun, werden Sie aufgefordert, geben Sie den Namen Ihrer Umgebung App-Dienst, um zu bestätigen, dass Sie wirklich tun dies möchten. Beachten Sie beim Löschen eines App-Service-Umgebung nicht der gesamte Inhalt darin ebenfalls gelöscht.  

![Löschen einer App-Service-Umgebung UI][9]  

## <a name="getting-started"></a>Erste Schritte

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Erstellen einer App-Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md).

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/

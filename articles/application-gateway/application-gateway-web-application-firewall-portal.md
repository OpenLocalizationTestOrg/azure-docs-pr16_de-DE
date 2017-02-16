<properties
   pageTitle="Erstellen Sie einen Gateway mit Web Anwendung Firewall mit dem Portal | Microsoft Azure"
   description="Erfahren Sie, wie ein Gateway mit Firewall der Web-Anwendung zu erstellen, mit dem portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Erstellen Sie einen Gateway mit Web Anwendung Firewall mithilfe des Portals

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-web-application-firewall-powershell.md)

Die Web-Anwendung Firewall (WAF) in Azure Application Gateway verhindert allgemeine webbasierten Angriffen wie SQL einfügen, websiteübergreifende Skriptingtools Angriffen und übernimmt Sitzung Webanwendungen. Web-Anwendung bietet Schutz vor viele der OWASP Top 10 Web Leitfaden.

Azure Application Gateway ist ein Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind.
Anwendung bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL).
Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Anwendung Gateway (Übersicht)](application-gateway-introduction.md)

## <a name="scenarios"></a>Szenarien

In diesem Artikel gibt es zwei Szenarios:

Im ersten Szenario lernen, wie Sie [Web Anwendung Firewall mit einer vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway).

Im zweiten Szenario erfahren Sie, um [ein Gateway mit Firewall der Web-Anwendung zu erstellen](#create-an-application-gateway-with-web-application-firewall)

![Szenario-Beispiel][scenario]

>[AZURE.NOTE] Zusätzliche Konfiguration des Gateways Anwendung, einschließlich benutzerdefinierter Gesundheit untersucht, Back-End-Ressourcenpool Adressen und weiteren Regeln nach das Anwendungsgateway konfiguriert ist und nicht während der anfänglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Vorbemerkung

Azure Application Gateway benötigt ein eigenen Subnetz. Wenn Sie ein virtuelles Netzwerk erstellen, stellen Sie sicher, dass Sie über genügend Speicherplatz Adresse, um mehrere Subnetze haben lassen. Nachdem Sie einen Gateway zu einem Subnetz bereitstellen, sind nur zusätzliche Anwendungsgateways im Subnetz hinzugefügt werden können.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen von Web Anwendung Firewall zu einer vorhandenen Anwendungsgateway

Dieses Szenario aktualisiert ein Gateway der vorhandenen Web Anwendung Firewall im Modus Prevention unterstützt.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal, wählen Sie eine vorhandene Anwendungsgateway.

![Erstellen von Application Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Konfiguration** , und Aktualisieren von gatewayeinstellungen der Anwendung. Wenn Sie fertig sind, klicken Sie auf **Speichern**

Die Einstellungen ein vorhandenen Anwendung Gateways zur Unterstützung von Web-Anwendung Firewall aktualisiert werden:

- **Schicht** - der ausgewählten Ebene muss **WAF** zur Unterstützung von Web-Anwendung firewall
- **SKU Größe** – diese Einstellung wird die Größe des Gateways Anwendung mit Firewall der Web-Anwendung, die verfügbaren Optionen sind (**Mittel** und **Groß**).
- **Status der Firewall** - diese Einstellung entweder aktiviert oder deaktiviert Web Anwendung Firewall.
- **Firewall-Modus** – diese Einstellung ist wie Web Anwendung Firewall bösartiger Datenverkehr behandelt. **Erkennungsmodus** protokolliert nur die Ereignisse, wobei **Prevention** Modus protokolliert die Ereignisse und stoppt den bösartigen Datenverkehr an.

![grundlegende Blade mit-Einstellungen][2]

>[AZURE.NOTE] Zum Anzeigen der Web-Anwendung Firewallprotokolle Diagnose muss aktiviert sein, und ApplicationGatewayFirewallLog ausgewählt. Eine Anzahl der Instanzen von 1 kann zu Testzwecken ausgewählt werden. Es ist wichtig zu wissen, dass eine beliebige Anzahl der Instanzen unter zwei Instanzen fällt nicht der Vereinbarung zum Servicelevel und können daher nicht empfohlen. Kleine Gateways sind nicht verfügbar, wenn Web Anwendung Firewall verwenden.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen Sie einen Gateway mit Firewall der Web-Anwendung

Dieses Szenario wird:

- Erstellen Sie ein Medium Web Anwendung Firewall Anwendungsgateway mit zwei Instanzen aus.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit reservierte CIDR 10.0.0.0/16 einen Textblock ein.
- Erstellen Sie ein Subnetz bezeichnet Appgatewaysubnet, die 10.0.0.0/28 als deren CIDR-Block verwendet.
- Konfigurieren Sie ein Zertifikat für die SSL-Verschiebung.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal, klicken Sie auf **neu** > **Networking** > **Application Gateway**

![Erstellen von Application Gateway][1-1]

### <a name="step-2"></a>Schritt 2

Füllen Sie als Nächstes grundlegende Informationen zu der Anwendungsgateway ein. Achten Sie darauf, dass **WAF** als die Ebene auswählen. Wenn Sie fertig sind, klicken Sie auf **OK**

Die Informationen für die grundlegenden Einstellungen erforderlich sind:

- **Name** – den Namen für das Anwendungsgateway.
- **Schicht** - die Ebene des Gateways Anwendung, die verfügbaren Optionen sind (**Standard** und **WAF**). Web-Anwendung Firewall ist nur in der WAF Ebene verfügbar.
- **SKU Größe** – diese Einstellung wird die Größe des Gateways Anwendung, die verfügbaren Optionen sind (**Mittel** und **Groß**).
- **Anzahl der Instanzen** - die Anzahl der Instanzen, diesen Wert sollte eine Zahl zwischen **2** und **10**sein.
- **Ressourcengruppe** - Ressourcengruppe zu halten Sie das Anwendungsgateway kann es eine vorhandene Ressourcengruppe oder eine neue sein.
- **Speicherort** – der Bereich für das Anwendungsgateway es am selben Speicherort in der Ressourcengruppe ist. *Die Position ist als das virtuelle Netzwerk wichtig und öffentliche IP-Adresse muss in am selben Speicherort wie das Gateway*.

![grundlegende Blade mit-Einstellungen][2-2]

>[AZURE.NOTE] Eine Anzahl der Instanzen von 1 kann zu Testzwecken ausgewählt werden. Es ist wichtig zu wissen, dass eine beliebige Anzahl der Instanzen unter zwei Instanzen fällt nicht der Vereinbarung zum Servicelevel und können daher nicht empfohlen. Kleine Gateways werden Web Anwendung Firewall Szenarien nicht unterstützt.

### <a name="step-3"></a>Schritt 3

Nachdem die grundlegenden Einstellungen definiert sind, besteht der nächste Schritt definiert das virtuelle Netzwerk verwendet werden. Das virtuelle Netzwerk-Website beinhaltet die Anwendung, der Anwendung Gateways für Lastenausgleich.

Klicken Sie auf **Auswählen eines virtuellen Netzwerks** , um das virtuelle Netzwerk konfigurieren.

![Blade mit Einstellungen für die Anwendungsgateway][3]

### <a name="step-4"></a>Schritt 4

Klicken Sie in das Blade **Virtuelles Netzwerk auswählen** auf **Neu erstellen**

Während der nicht in diesem Szenario erläutert, kann ein vorhandenes virtuelles Netzwerk zu diesem Zeitpunkt ausgewählt werden.  Wenn Sie ein vorhandenes virtuelles Netzwerk verwendet wird, ist es wichtig zu wissen, dass das virtuelle Netzwerk einer leeren Subnetz oder einem Subnetz nur Anwendung Gateway-Ressourcen verwendet werden soll.

![Wählen Sie virtuelles Netzwerk Blade aus.][4]

### <a name="step-5"></a>Schritt 5

Füllen Sie die Informationen in das **Erstellen von virtuellen Netzwerk** Blade ein, wie in der Beschreibung des vorhergehenden [Szenarios](#scenario) beschrieben.

![Erstellen von virtuellen Netzwerk Blade mit eingegebene Informationen][5]

### <a name="step-6"></a>Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, besteht der nächste Schritt die Front-End-IP-Adresse für das Anwendungsgateway definiert. An diesem Punkt ist die Wahl zwischen einer öffentlichen oder eine private IP-Adresse für die Front-End-ein. Die Option abhängig, ob die Anwendung Internet zugänglichen oder internen nur. Dieses Szenario setzt voraus, mithilfe einer öffentlichen IP-Adresse. Zum Auswählen einer privaten IP-Adresse kann die **Private** Schaltfläche geklickt werden. Eine automatisch zugewiesene IP-Adresse ausgewählt ist, oder auf das Kontrollkästchen **Auswählen einer bestimmten privaten IP-Adresse** , um eine manuell einzugeben.

Klicken Sie auf **Auswählen einer öffentlichen IP-Adresse**. Wenn Sie eine vorhandene öffentliche IP-Adresse verfügbar ist zu diesem Zeitpunkt ausgewählt werden können, in diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **neu erstellen**

![Wählen Sie aus öffentlichen IP-Adresse Karte][6]

### <a name="step-7"></a>Schritt 7

Als Nächstes geben Sie der öffentlichen IP-Adresse einen Anzeigenamen ein, und klicken Sie auf **OK**

![Erstellen von öffentlichen IP-Adresse blade][7]

### <a name="step-8"></a>Schritt 8

Als Nächstes einrichten Sie die Zuhörer Konfiguration aus.  Wenn **http** verwendet wird, wird nichts links konfigurieren und **OK** geklickt werden kann. Zur Verwendung von **Https** ist weitere Konfiguration erforderlich.

Wenn Sie **Https**verwenden möchten, ist ein Zertifikat erforderlich. Der private Schlüssel des Zertifikats ist es erforderlich, damit einer PFX-Datei exportieren, der das Zertifikat muss angegeben werden, und das Kennwort für die Datei.

Klicken Sie auf **HTTPS**, klicken Sie auf **das Ordnersymbol neben das Textfeld **Hochladen PFX-Zertifikat** ** .
Navigieren Sie zu der Zertifikatsdatei PFX-Datei in Ihrem Dateisystem. Sobald sie markiert ist, geben Sie dem Zertifikat einen Anzeigenamen ein, und geben Sie das Kennwort für die PFX-Datei.

Klicken Sie auf **OK** , um die Einstellungen für das Gateway-Anwendung zu überprüfen, wenn der Vorgang abgeschlossen.

![Zuhörer Konfigurationsabschnitt Einstellungen blade][8]

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die spezifischen **WAF** -Einstellungen.

- **Status der Firewall** - diese Einstellung schaltet WAF ein oder aus.
- **Firewall-Modus** – diese Einstellung bestimmt, dass die Aktionen WAF bösartiger Datenverkehr annimmt. Wenn **Erkennung** ausgewählt ist, wird nur Datenverkehr protokolliert.  Wenn **Prevention** ausgewählt ist, den Datenverkehr protokolliert und mit einem 403 nicht autorisiert beendet.

![Web-Anwendung Firewall-Einstellungen][9]

### <a name="step-10"></a>Schritt 10

Überprüfen Sie die Seite "Zusammenfassung", und klicken Sie auf **OK**.  Das Anwendungsgateway wird jetzt in der Warteschlange nach oben und erstellt.

### <a name="step-12"></a>Schritt 12

Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie zu im Portal, um die Konfiguration des Gateways Anwendung fortzusetzen.

![Anwendung Gateway Ressourcenansicht][10]

Diesen Schritten erstellen einen einfachen Anwendungsgateway mit Standardeinstellungen für die Zuhörer, Back-End-Ressourcenpool, Back-End-HTTP-Einstellungen und Regeln. Sie können diese Einstellungen entsprechend die Bereitstellung, nachdem die Bereitstellung erfolgreich ist ändern

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Konfigurieren der diagnoseprotokollierung, um die Ereignisse aus, die erkannt oder verhindert mit Web Anwendung Firewall besuchen Sie die [Anwendung Gateway Diagnose](application-gateway-diagnostics.md) protokollieren

Erfahren Sie, wie benutzerdefinierte Gesundheit Prüfpunkte besuchen, [erstellen einen benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-probe-portal.md) erstellen

Informationen Sie zum Konfigurieren SSL Verschiebung und die teure SSL entschlüsseln Deaktivieren von Webservern stehen Ihnen auf [Konfigurieren SSL Auslagern](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
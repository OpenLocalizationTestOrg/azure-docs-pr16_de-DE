<properties
   pageTitle="Erstellen Sie einen Gateway mit dem Portal | Microsoft Azure"
   description="Erfahren Sie, wie ein Gateway mit dem Portal erstellen"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Erstellen Sie einen Gateway mit dem portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassischen PowerShell](application-gateway-create-gateway.md)
- [Azure Ressourcenmanager Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL). Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Anwendung Gateway (Übersicht)](application-gateway-introduction.md)

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie einen Gateway mit dem Azure-Portal zu erstellen.

Dieses Szenario wird:

- Erstellen eines Gateways Medium Anwendung mit zwei Instanzen an.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit reservierte CIDR 10.0.0.0/16 einen Textblock ein.
- Erstellen Sie ein Subnetz bezeichnet Appgatewaysubnet, die 10.0.0.0/28 als deren CIDR-Block verwendet.
- Konfigurieren Sie ein Zertifikat für die SSL-Verschiebung.

![Szenario-Beispiel][scenario]

>[AZURE.IMPORTANT] Zusätzliche Konfiguration des Gateways Anwendung, einschließlich benutzerdefinierter Gesundheit untersucht, Back-End-Ressourcenpool Adressen und weiteren Regeln nach das Anwendungsgateway konfiguriert ist und nicht während der anfänglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Vorbemerkung

Azure Application Gateway benötigt ein eigenen Subnetz. Wenn Sie ein virtuelles Netzwerk erstellen, stellen Sie sicher, dass Sie über genügend Speicherplatz Adresse, um mehrere Subnetze haben lassen. Nachdem Sie einen Gateway zu einem Subnetz bereitstellen, sind nur zusätzliche Anwendungsgateways im Subnetz hinzugefügt werden können.

## <a name="create-the-application-gateway"></a>Erstellen des Gateways Anwendung

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal, klicken Sie auf **neu** > **Networking** > **Application Gateway**

![Erstellen von Application Gateway][1]

### <a name="step-2"></a>Schritt 2

Füllen Sie als Nächstes grundlegende Informationen zu der Anwendungsgateway ein. Wenn Sie fertig sind, klicken Sie auf **OK**

Die Informationen für die grundlegenden Einstellungen erforderlich sind:

- **Name** – den Namen für das Anwendungsgateway.
- **Schicht** - Dies ist die Ebene des Gateways Anwendung. Zwei Ebenen stehen **WAF** und den **Standardversionen**. WAF ermöglicht die Web-Anwendung Firewall-Funktion.
- **SKU Größe** – diese Einstellung wird die Größe des Gateways Anwendung, die verfügbare Optionen sind (**klein**, **Mittel**und **Groß**). *Kleine ist nicht verfügbar, wenn WAF Ebene ausgewählt ist*
- **Anzahl der Instanzen** - die Anzahl der Instanzen, diesen Wert sollte eine Zahl zwischen 2 und 10 sein.
- **Ressourcengruppe** - Ressourcengruppe, halten Sie das Anwendungsgateway kann es eine vorhandene Ressourcengruppe oder eine neue sein.
- **Speicherort** – der Bereich für das Anwendungsgateway es am selben Speicherort in der Ressourcengruppe ist. *Die Position ist als das virtuelle Netzwerk wichtig und öffentliche IP-Adresse muss in am selben Speicherort wie das Gateway*.

![grundlegende Blade mit-Einstellungen][2]

>[AZURE.NOTE] Eine Anzahl der Instanzen von 1 kann zu Testzwecken ausgewählt werden. Es ist wichtig zu wissen, dass eine beliebige Anzahl der Instanzen unter zwei Instanzen fällt nicht der Vereinbarung zum Servicelevel und können daher nicht empfohlen. Kleine Gateways sind für die Entwicklung Test und nicht für die Herstellung Zwecke verwendet werden.

### <a name="step-3"></a>Schritt 3

Nachdem die grundlegenden Einstellungen definiert sind, besteht der nächste Schritt definiert das virtuelle Netzwerk verwendet werden. Das virtuelle Netzwerk-Website beinhaltet die Anwendung, der Anwendung Gateways für Lastenausgleich.

Klicken Sie auf **Auswählen eines virtuellen Netzwerks** , um das virtuelle Netzwerk konfigurieren.

![Blade mit Einstellungen für die Anwendungsgateway][3]

### <a name="step-4"></a>Schritt 4

Klicken Sie in das Blade *Virtuelles Netzwerk auswählen* auf **Neu erstellen**

Während der nicht in diesem Szenario erläutert, kann ein vorhandenes virtuelles Netzwerk zu diesem Zeitpunkt ausgewählt werden.  Wenn Sie ein vorhandenes virtuelles Netzwerk verwendet wird, ist es wichtig zu wissen, dass das virtuelle Netzwerk einer leeren Subnetz oder einem Subnetz nur Anwendung Gateway-Ressourcen verwendet werden soll.

![Wählen Sie virtuelles Netzwerk Blade aus.][4]

### <a name="step-5"></a>Schritt 5

Füllen Sie die Informationen in das **Erstellen von virtuellen Netzwerk** Blade ein, wie in der Beschreibung des vorhergehenden [Szenarios](#scenario) beschrieben.

![Erstellen von virtuellen Netzwerk Blade mit eingegebene Informationen][5]

### <a name="step-6"></a>Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, besteht der nächste Schritt die Front-End-IP-Adresse für das Anwendungsgateway definiert. An diesem Punkt ist die Wahl zwischen einer öffentlichen oder eine private IP-Adresse für die Front-End-ein. Die Option abhängig, ob die Anwendung Internet zugänglichen oder internen nur. Dieses Szenario setzt voraus, mithilfe einer öffentlichen IP-Adresse. Zum Auswählen einer privaten IP-Adresse kann die **Private** Schaltfläche geklickt werden. Eine automatisch zugewiesene IP-Adresse ausgewählt ist, oder auf das Kontrollkästchen **Auswählen einer bestimmten privaten IP-Adresse** , um eine manuell einzugeben.

### <a name="step-7"></a>Schritt 7

Klicken Sie auf **Auswählen einer öffentlichen IP-Adresse**. Wenn Sie eine vorhandene öffentliche IP-Adresse verfügbar ist zu diesem Zeitpunkt ausgewählt werden können, in diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **neu erstellen**

![Wählen Sie aus öffentlichen IP-Adresse Karte][6]

### <a name="step-8"></a>Schritt 8

Als Nächstes geben Sie der öffentlichen IP-Adresse einen Anzeigenamen ein, und klicken Sie auf **OK**

![Erstellen von öffentlichen IP-Adresse blade][7]

### <a name="step-9"></a>Schritt 9

Die letzte Einstellung zu konfigurieren, wenn Sie einen Gateway ist die Zuhörer-Konfiguration.  Wenn **http** verwendet wird, wird nichts links konfigurieren und **OK** geklickt werden kann. Zur Verwendung von **Https** ist weitere Konfiguration erforderlich.

Wenn Sie **Https**verwenden möchten, ist ein Zertifikat erforderlich. Der private Schlüssel des Zertifikats ist es erforderlich, damit eine PFX-Datei Exportieren des Zertifikats und des Kennworts bereitgestellt werden müssen.

### <a name="step-10"></a>Schritt 10

Klicken Sie auf **HTTPS**, klicken Sie auf **das Ordnersymbol neben das Textfeld **Hochladen PFX-Zertifikat** ** .
Navigieren Sie zu der Zertifikatsdatei PFX-Datei in Ihrem Dateisystem. Sobald sie markiert ist, geben Sie dem Zertifikat einen Anzeigenamen ein, und geben Sie das Kennwort für die PFX-Datei.

Klicken Sie auf **OK** , um die Einstellungen für das Gateway-Anwendung zu überprüfen, wenn der Vorgang abgeschlossen.

![Zuhörer Konfigurationsabschnitt Einstellungen blade][9]

### <a name="step-11"></a>Schritt 11

Überprüfen Sie die Seite "Zusammenfassung", und klicken Sie auf **OK**.  Das Anwendungsgateway wird jetzt in der Warteschlange nach oben und erstellt.

### <a name="step-12"></a>Schritt 12

Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie darauf im Portal, um die Konfiguration des Gateways Anwendung fortzusetzen.

![Anwendung Gateway Ressourcenansicht][10]

Diesen Schritten erstellen einen einfachen Anwendungsgateway mit Standardeinstellungen für die Zuhörer, Back-End-Ressourcenpool, Back-End-HTTP-Einstellungen und Regeln. Sie können diese Einstellungen entsprechend die Bereitstellung, nachdem die Bereitstellung erfolgreich ist, ändern.

## <a name="next-steps"></a>Nächste Schritte

Dieses Szenario wird einen Standard-Gateway-Anwendung erstellt. Die nächsten Schritte sind und das Anwendungsgateway zu konfigurieren, indem Sie Ressourcenpool Mitglieder hinzufügen, ändern Einstellungen und Anpassen von Regeln in das Gateway dafür ordnungsgemäß funktioniert.

Erfahren Sie, wie benutzerdefinierte Gesundheit Prüfpunkte besuchen, [erstellen einen benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-probe-portal.md) erstellen

Informationen Sie zum Konfigurieren SSL Verschiebung und die teure SSL entschlüsseln Deaktivieren von Webservern stehen Ihnen auf [Konfigurieren SSL Auslagern](application-gateway-ssl-portal.md)

Erfahren Sie, wie Ihre Clientanwendungen mit einem [Web-Anwendung Firewall](application-gateway-webapplicationfirewall-overview.md) ein Feature des Gateways Anwendung zu schützen.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png

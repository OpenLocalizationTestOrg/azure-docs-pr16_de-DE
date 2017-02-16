<properties
   pageTitle="Konfigurieren ein vorhandenen Anwendung Gateways für das Hosten von mehreren Websites im Portal Azure | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Konfigurieren eines vorhandenen Azure-Anwendung Gateways für das Hosten von mehreren Webanwendungen auf demselben Gateway mit dem Azure-Portal an."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurieren eines vorhandenen Anwendung Gateways für das Hosten von mehreren Webanwendungen

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-multisite-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Mehreren Websitehost, können Sie mehrere Webanwendung auf demselben Anwendungsgateway bereitstellen. Er beruht auf Vorhandensein von Host Kopfzeile in der eingehenden HTTP-Anforderung, um zu bestimmen, welche Zuhörer Datenverkehr empfangen möchten. Die Zuhörer weist dann den Datenverkehr in die entsprechenden Back-End-Pool an, wie in der Regeln Definition des Gateways konfiguriert. In Webanwendungen SSL aktiviert wird die Erweiterung Server Namen Angabe (SNI), wählen Sie die richtige Zuhörer für den Datenverkehr im Web Anwendungsgateway abhängig. Eine häufige Verwendung von mehreren Websitehost besteht darin Saldo Besprechungsanfragen für verschiedene Webdomänen und Pools anderen Back-End-Server zu laden. Mehrere Unterdomänen derselben Stamm Domäne konnte auf ähnliche Weise auch auf dem gleichen Anwendungsgateway gehostet werden.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel werden Anwendungsgateway Datenverkehr für contoso.com und fabrikam.com mit zwei Back-End-Serverpools übermittelt: Contoso Server Ressourcenpool und Fabrikam Server Ressourcenpool. Ähnliche Setup konnte zu Host Unterdomänen wie app.contoso.com und blog.contoso.com verwendet werden.

![aktivierter Szenario][multisite]

## <a name="before-you-begin"></a>Vorbemerkung

Dieses Szenario hinzugefügt ein Gateway der vorhandenen Website mit mehreren Support. Für die Durchführung dieses Szenario muss Szenario eines vorhandenen Anwendung Gateways konfigurieren zur Verfügung. Besuchen Sie [erstellen ein Gateway mit dem Portal](./application-gateway-create-gateway-portal.md) , um weitere Informationen zum Erstellen eines grundlegenden Anwendung Gateways im Portal aus.

## <a name="requirements"></a>Anforderungen

- **Back-End-Server Ressourcenpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder mit dem virtuellen Netzwerksubnetz gehören oder einer öffentlichen IP-Adresse/VIP sollten. FQDN kann auch verwendet werden.
- **Back-End-Pool servereinstellungen:** Jeder Ressourcenpool hat davon ausgehen, dass Port, Protokoll und Cookie-basierten Zugehörigkeit. Diese Einstellungen zu einem Ressourcenpool verknüpft sind, und klicken Sie auf alle Server im Pool angewendet werden.
- **Front-End-Anschluss:** Dieser Port wird öffentlichen, die auf dem Anwendungsgateway geöffnet ist. Datenverkehr Treffer diesen Anschluss, und klicken Sie dann auf einen der Back-End-Server weitergeleitet.
- **Zuhörer:** Die Zuhörer weist einen Front-End-Anschluss, ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung beachtet), und der Zertifikatsname SSL (falls Auslagern Konfigurieren von SSL). Für mehrere Website aktiviert Anwendungsgateways sind auch Hostname und SNI Indikatoren hinzugefügt.
- **Regel:** Die Regel bindet das Zuhörer, des Back-End-Server befinden, und welche Back-End-Server Ressourcenpool an der Datenverkehr weitergeleitet werden soll, wenn ein bestimmtes Zuhörer Ball definiert.
- **Zertifikate:** Jede Zuhörer erfordert ein eindeutige Zertifikat, in diesem Beispiel 2 Listener für mehrere Website erstellt werden. Zwei PFX-Zertifikate und die Kennwörter für sie erstellt werden müssen.

## <a name="create-an-application-gateway"></a>Erstellen Sie einen gateway

Im folgenden werden die Schritte zum Aktualisieren des Gateways Anwendung erforderlich sind:

1. Anlegen Sie Back-End-Pool für jede Website.
2. Erstellen Sie eine neue Zuhörer für jede Website Anwendungsgateway unterstützt wird.
3. Erstellen von Regeln zum Zuordnen der einzelnen Zuhörer mit den entsprechenden Back-End.

## <a name="create-back-end-pools-for-each-site"></a>Back-End-Pool für jede Website anlegen

Ein Ressourcenpool Back-End für jede Website dieser Anwendungsgateway wird Unterstützung erforderlich ist, in diesem Fall 2 erstellt werden, eine für contoso11.com und eine für fabrikam11.com.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einer vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie die **Back-End-Pools** , und klicken Sie auf **Hinzufügen**

![Hinzufügen von Back-End-pools][7]

### <a name="step-2"></a>Schritt 2

Füllen Sie die Informationen für die Back-End-Pool **pool1**, die IP-Adressen oder vollqualifizierte Domänennamen für die Back-End-Servern hinzufügen, und klicken Sie auf **OK**

![Back-End-Pool pool1-Einstellungen][8]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf die Back-End-Pools Blade auf **Hinzufügen** , um eine zusätzliche Back-End-Pool **pool2**, Hinzufügen von IP-Adressen oder vollqualifizierte DOMÄNENNAMEN für die Back-End-Servern hinzufügen, und klicken Sie auf **OK**

![Back-End-Pool pool2-Einstellungen][9]

### <a name="create-listeners-for-each-back-end"></a>Erstellen von Listenern für jeden Back-end

Application Gateway basiert auf HTTP 1.1 Host Überschriften, um mehr als eine Website auf die gleiche öffentliche IP-Adresse und den Port zu hosten. Die grundlegende Zuhörer erstellt im Portal enthält keine dieser Eigenschaft.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf der vorhandenen Anwendungsgateway auf **Abhörer** , und klicken Sie auf **mehrere Website** , um die erste Zuhörer hinzuzufügen.

![Listener Übersicht blade][1]

### <a name="step-2"></a>Schritt 2

Füllen Sie die Informationen für die Zuhörer, In diesem Beispiel SSL-Beendigung konfiguriert ist, erstellen einen neuen Front-End-Anschluss. Hochladen des PFX-Zertifikats für SSL-Beendigung verwendet werden soll. Klicken Sie auf diese Blade im Vergleich zu den standardmäßigen grundlegende Zuhörer Blade der einzige Unterschied ist der Hostname.

![Zuhörer Eigenschaften blade][2]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf **mit mehreren Standorten** und erstellen Sie eine andere Zuhörer, wie im vorherigen Schritt für die zweite Website beschrieben. Vergewissern Sie sich, ein anderes Zertifikat für die zweite Zuhörer verwendet. Klicken Sie auf diese Blade im Vergleich zu den standardmäßigen grundlegende Zuhörer Blade der einzige Unterschied ist der Hostname. Füllen Sie die Informationen für die Zuhörer, und klicken Sie auf **OK**.

![Zuhörer Eigenschaften blade][3]

> [AZURE.NOTE] Erstellung der Listener im Azure-Portal für Anwendungsgateway ist eine zeitintensive Aufgabe, es möglicherweise etwas dauern zwei Listener in diesem Szenario zu erstellen. Nach Abschluss das Listener anzeigen im Portal wie in der folgenden Abbildung gezeigt.

![Zuhörer (Übersicht)][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Erstellen von Regeln zum Listener Back-End-Pools zuordnen

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einer vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie **Regeln** , und wählen Sie das vorhandene Standard Regel **rule1** aus, und klicken Sie auf **Bearbeiten**.

### <a name="step-2"></a>Schritt 2

Füllen Sie das Blade Regeln ein, wie in der folgenden Abbildung gezeigt. Auswählen der ersten Zuhörer und ersten Ressourcenpool, und klicken Sie auf **Speichern** , wenn Sie fertig sind.

![Bearbeiten Sie vorhandenen Regel][6]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf **der grundlegenden Regeln** zum Erstellen der 2. Regel. Füllen Sie das Formular für das zweite Zuhörer und den zweiten Back-End-Ressourcenpool, und klicken Sie auf **OK** , um zu speichern.

![Hinzufügen der grundlegenden Regeln blade][10]

Damit das Konfigurieren eines vorhandenen Anwendung Gateways mit mehreren Standorten Support über das Azure-Portal ist abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Ihre Websites mit [Application Gateway - Web-Anwendung Firewall](application-gateway-webapplicationfirewall-overview.md) schützen

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
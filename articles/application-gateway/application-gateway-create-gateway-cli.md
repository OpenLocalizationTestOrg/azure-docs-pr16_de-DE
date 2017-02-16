<properties
   pageTitle="Erstellen Sie einen Gateway mit Azure CLI in Ressourcenmanager | Microsoft Azure"
   description="Erfahren Sie, wie ein Gateway mithilfe der Azure CLI in Ressourcenmanager erstellen"
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

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Erstellen Sie einen Gateway mithilfe der Azure-CLI

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassischen PowerShell](application-gateway-create-gateway.md)
- [Azure Ressourcenmanager Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist ein Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Anwendungsgateway weist die folgenden Features der Anwendung Übermittlung: HTTP Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit und Secure Sockets Layer (SSL) Verschiebung, benutzerdefinierte Gesundheit Prüfpunkte laden und Unterstützung für mehrere Website.

## <a name="prerequisite-install-the-azure-cli"></a>Voraussetzung: Installieren Sie die Azure CLI

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure Line Benutzeroberfläche für Mac, Linux, und Windows Azure CLI () installieren](../xplat-cli-install.md) und Sie [Melden Sie sich bei Azure](../xplat-cli-connect.md)müssen. 

> [AZURE.NOTE] Wenn Sie ein Azure-Konto besitzen, benötigen Sie diesen. Steigen Sie melden Sie sich für eine [kostenlose Testversion hier](../active-directory/sign-up-organization.md)aus.

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie Sie einen Gateway mit dem Azure-Portal zu erstellen.

Dieses Szenario wird:

- Erstellen eines Gateways Medium Anwendung mit zwei Instanzen an.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit reservierte CIDR 10.0.0.0/16 einen Textblock ein.
- Erstellen Sie ein Subnetz bezeichnet Appgatewaysubnet, die 10.0.0.0/28 als deren CIDR-Block verwendet.
- Konfigurieren Sie ein Zertifikat für die SSL-Verschiebung.

![Szenario-Beispiel][scenario]

>[AZURE.NOTE] Zusätzliche Konfiguration des Gateways Anwendung, einschließlich benutzerdefinierter Gesundheit untersucht, Back-End-Ressourcenpool Adressen und weiteren Regeln nach das Anwendungsgateway konfiguriert ist und nicht während der anfänglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Vorbemerkung

Azure Application Gateway benötigt ein eigenen Subnetz. Wenn Sie ein virtuelles Netzwerk erstellen, stellen Sie sicher, dass Sie über genügend Speicherplatz Adresse, um mehrere Subnetze haben lassen. Nachdem Sie einen Gateway zu einem Subnetz bereitstellen, sind nur zusätzliche Anwendungsgateways im Subnetz hinzugefügt werden können.

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderungsfenster**, und melden Sie sich. 

    azure login

Nachdem Sie das vorstehende Beispiel eingeben, wird ein Code bereitgestellt. Navigieren Sie zu https://aka.ms/devicelogin in einem Browser, um die Anmeldung fortzusetzen.

![CMD mit Gerät login][1]

Geben Sie im Browser den Code, den Sie erhalten haben. Sie sind auf eine Anmeldeseite umgeleitet.

![Browser Code eingeben][2]

Nachdem Sie der Code eingegeben wurde Sie sind angemeldet, schließen Sie den Browser, um auf dem Szenario fortzusetzen.

![erfolgreich angemeldet][3]

## <a name="switch-to-resource-manager-mode"></a>Wechseln Sie zur Ressourcenmanager Modus

    azure config mode arm

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Vor dem Erstellen des Gateways Anwendung, wird eine Ressourcengruppe erstellt, um das Anwendungsgateway enthalten. Die nachstehende Abbildung zeigt den Befehl aus.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Erstellen Sie ein virtuelles Netzwerk

Nachdem die Ressourcengruppe erstellt wurde, wird ein virtuelles Netzwerk für das Anwendungsgateway erstellt.  Im folgenden Beispiel wurde die Adresse Leerzeichen als 10.0.0.0/16 wie in den vorherigen Szenario Notizen definiert sind.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Erstellen Sie ein Subnetz

Nachdem das virtuelle Netzwerk erstellt wurde, wird ein Subnetz für das Anwendungsgateway hinzugefügt.  Wenn Sie die Anwendungsgateway mit einem Web-app im gleichen virtuellen Netzwerk als Anwendungsgateway gehostet verwenden möchten, müssen Sie unbedingt ausreichend Platz für ein anderes Subnetz.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Erstellen des Gateways Anwendung

Sobald die virtuelle Netzwerk und Subnetz erstellt wurden, werden die erforderlichen Komponenten für das Anwendungsgateway abgeschlossen. Darüber hinaus ein Zertifikat einer zuvor exportierten PFX-Datei und das Kennwort für das Zertifikat für die folgenden Schritte erforderlich sind: die IP-Adressen für die Back-End-verwendet werden, die IP-Adressen für Ihre Back-End-Server. Diese Werte können entweder private IP-Adressen in das virtuelle Netzwerk, öffentliche IP-Adressen oder vollqualifizierten Domänennamen für die Back-End-Server sein.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Eine Liste der Parameter, die während der Erstellung, führen Sie den folgenden Befehl bereitgestellt werden können: **Azure Netzwerk Anwendung-Gateway erstellen – Hilfe**.

In diesem Beispiel wird einen Gateway grundlegende Anwendung mit Standardeinstellungen für die Zuhörer, Back-End-Ressourcenpool, Back-End-HTTP-Einstellungen und Regeln erstellt. Außerdem wird SSL Verschiebung konfiguriert. Sie können diese Einstellungen entsprechend die Bereitstellung, nachdem die Bereitstellung erfolgreich ist, ändern.
Wenn Sie bereits über die Webanwendung mit definiert haben die Back-End-Pool in den vorherigen Schritten, nachdem erstellt, den Lastenausgleich beginnt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie benutzerdefinierte Gesundheit Prüfpunkte besuchen, [erstellen einen benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-probe-portal.md) erstellen

Informationen Sie zum Konfigurieren SSL Verschiebung und die teure SSL entschlüsseln Deaktivieren von Webservern stehen Ihnen auf [Konfigurieren SSL Auslagern](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
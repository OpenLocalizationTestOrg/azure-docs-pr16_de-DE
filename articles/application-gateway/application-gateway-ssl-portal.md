<properties
   pageTitle="Konfigurieren Sie einen Gateway für SSL-Verschiebung mithilfe des Portals | Microsoft Azure"
   description="Diese Seite enthält Anweisungen So erstellen Sie einen Gateway mit SSL mithilfe des Portals Auslagern"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurieren Sie einen Gateway für SSL-Verschiebung mithilfe des Portals

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassische PowerShell](application-gateway-ssl.md)

Die Sitzung Secure Sockets Layer (SSL) auf dem Gateway zu vermeiden Sie teure SSL entschlüsseln Vorgänge, um bei der Webfarm erfolgen beenden können Azure Application Gateway konfiguriert sein. SSL-Verschiebung vereinfacht auch das Front-End-Server einrichten und Verwalten von Web-Anwendung.

## <a name="scenario"></a>Szenario

Das folgende Szenario durchläuft konfigurieren SSL auf einer vorhandenen Anwendungsgateway auslagern. Das Szenario setzt voraus, dass Sie die Schritte zum [Erstellen eines Gateways Anwendung](application-gateway-create-gateway-portal.md)bereits befolgt haben.

## <a name="before-you-begin"></a>Vorbemerkung

Zum Konfigurieren von SSL-Verschiebung für ein Gateway ist ein Zertifikat erforderlich. Dieses Zertifikat ist auf dem Anwendungsgateway geladen und zum Verschlüsseln und Entschlüsseln des Datenverkehrs über SSL verwendet. Das Zertifikat muss sich im Format Personal Information Exchange (Pfx) befinden. Mithilfe dieses Dateiformats ermöglicht für den zu exportierenden privaten Schlüssel zum Ausführen der und Verschlüsselung von Datenverkehr vom Anwendungsgateway erforderlich ist.

## <a name="add-an-https-listener"></a>Hinzufügen einer HTTPS Zuhörer

Die Zuhörer HTTPS für den Datenverkehr auf Grundlage der Konfiguration aussieht, und hilft den Datenverkehr an die Back-End-Pools weiterleiten.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal und wählen Sie eine vorhandene Anwendungsgateway

### <a name="step-2"></a>Schritt 2

Klicken Sie auf Abhörer, und klicken Sie auf die Schaltfläche hinzufügen, um eine Zuhörer hinzuzufügen.

![App Gateway Übersicht blade][1]

### <a name="step-3"></a>Schritt 3

Füllen Sie die erforderlichen Informationen für die Zuhörer und Upload das Zertifikat PFX-Datei, wenn Sie fertig sind, klicken Sie auf OK.

**Name** - dieser Wert wird einen Anzeigenamen für die Zuhörer.

**Front-End-IP-Konfiguration** – dieser Wert wird der Front-End-IP-Konfiguration, die für die Zuhörer verwendet wird.

**Front-End (Name/Anschluss)** : einen Anzeigenamen für den Port am front-End der Anwendung Gateways und den tatsächlichen Port verwendet.

**Protokoll** - einer Option, um festzustellen, ob für die front-End Https oder http verwendet wird.

**Urkunde (Name/Kennwort)** – Wenn SSL Verschiebung wird verwendet, ein PFX-Zertifikat ist erforderlich, damit diese Einstellung und einen Anzeigenamen und ein Kennwort erforderlich sind.

![Hinzufügen von Zuhörer blade][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Erstellen einer Regel, und ordnen Sie es an die Zuhörer

Die Zuhörer ist jetzt erstellt wurde. Es ist Zeit zum Erstellen einer Regel, um den Datenverkehr aus dem Zuhörer zu verarbeiten.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf die **Regeln** des Gateways Anwendung, und klicken Sie dann auf Hinzufügen.

![App Gateway Regeln blade][3]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf das Blade **grundlegende Regel hinzufügen** Geben Sie in den Anzeigenamen für die Regel, und wählen Sie die Zuhörer im vorherigen Schritt erstellt haben. Wählen Sie die entsprechenden Back-End-Pool und http-Einstellung aus, und klicken Sie auf **OK**

![HTTPS-Einstellungen-Fenster][4]

Die Einstellungen werden nun mit dem Anwendungsgateway gespeichert. Speichern verarbeiten für diese Einstellungen möglicherweise eine Weile dauern, bevor sie über das Portal oder über PowerShell anzeigen verfügbar sind. Nach dem Speichern des Gateways Anwendung übernimmt die und Verschlüsselung von Datenverkehr. Gesamten Verkehr zwischen dem Anwendungsgateway und den Back-End-Webservern muss über http behandelt werden. Alle Kommunikation an den Client, wenn Sie über Https initiiert wird an den Client verschlüsselt zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

So konfigurieren Sie einen benutzerdefinierten Gesundheit Prüfpunkt mit Azure Application Gateway finden Sie unter [Erstellen eines benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
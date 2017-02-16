<properties
    pageTitle="Benutzerdefinierte Einstellungen für App-Service-Umgebungen"
    description="Benutzerdefinierte Konfigurationen für die App-Service-Umgebungen"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Benutzerdefinierte Konfigurationen für die App-Service-Umgebungen

## <a name="overview"></a>(Übersicht) ##
Da App Dienst Umgebungen an einen einzelnen Kunden isoliert sind, sind bestimmte Konfiguration Einstellungen, die ausschließlich zur App-Service-Umgebungen angewendet werden können. In diesem Artikel Dokumente verschiedene bestimmten Anpassungen, die für die App-Service-Umgebungen verfügbar sind.

Wenn Sie nicht über eine App-Service-Umgebung verfügen, finden Sie unter [Erstellen einer App-Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md).

Sie können Anpassungen der App-Service-Umgebung mithilfe einer Matrix zurück in das neue **ClusterSettings** Attribut speichern. Dieses Attribut befindet sich im Wörterbuch der *HostingEnvironments* Azure Ressourcenmanager Entität "Eigenschaften".

Der folgenden abgekürzten Ressourcenmanager Vorlage Codeausschnitt zeigt das Attribut **ClusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Das Attribut **ClusterSettings** kann in einer Vorlage Ressourcenmanager, aktualisieren Sie die App-Service-Umgebung aufgenommen werden.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Mit dem Azure Ressource Explorer Aktualisieren einer App-Service-Umgebung
Alternativ können Sie die App-Service-Umgebung mit [Azure-Explorers](https://resources.azure.com)aktualisieren.  

1. Ressourcen-Explorer, wechseln Sie auf den Knoten für die App-Service-Umgebung (**Abonnements** > **ResourceGroups** > **Anbieter** > **Micrososft.Web** > **HostingEnvironments**). Klicken Sie dann auf die bestimmte App-Service-Umgebung, die Sie aktualisieren möchten.

2. Klicken Sie im rechten Bereich in der oberen Symbolleiste Interaktive in Ressource Explorer bearbeiten dürfen auf **Schreibgeschützt** .  

3. Klicken Sie auf den blauen **Bearbeiten** Schaltfläche, um die Vorlage Ressourcenmanager bearbeitbar.

4. Führen Sie einen Bildlauf an das Ende im rechten Bereich. Das Attribut **ClusterSettings** ist ganz unten, wo eingegeben oder eigenen Werte aktualisiert werden können.

5. Geben Sie ein (oder kopieren und Einfügen) das Array von Werten für das Attribut **ClusterSettings** die gewünschten.  

6. Klicken Sie auf das grüne **setzen** , die Schaltfläche hat sich oben im rechten Bereich auf commit die Änderung an die App-Service-Umgebung befindet.

Jedoch Einreichen einer Änderung dauert ungefähr 30 Minuten multipliziert die Anzahl der front-End in der App-Dienst Umgebung die vorgenommene Änderung wirksam wird.
Beispielsweise weist eine App-Service-Umgebung vier front-End, dauert etwa zwei Stunden für die Aktualisierung der Konfiguration zum Abschließen es. Während der Variationswebsites Änderung der Konfiguration können keine anderen Anpassungsbereich für Vorgänge oder Konfiguration Änderungsvorgänge in der App-Service-Umgebung erfolgen.

## <a name="disable-tls-10"></a>Deaktivieren Sie TLS 1.0 ##
Eine laufende Frage von Kunden, besonders Kunden PCI Compliance zuständig sind überwacht, wie TLS 1.0 explizit für ihre apps zu deaktivieren.

TLS 1.0 kann über die folgenden **ClusterSettings** Eintrag deaktiviert werden:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>TLS-Verschlüsselung Suite-Reihenfolge ändern ##
Eine weitere Frage von Kunden können sie die Liste der Daten von deren Server ausgehandelt ändern und ist dies kann erreicht werden, indem Sie die **ClusterSettings** ändern, wie unten dargestellt. Die Liste der verfügbaren Verschlüsselungssammlungen [diesem MSDN-Artikel] abgerufen werden kann (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Wenn die falsche Werten für die Ziffernfolge festgelegt sind, die nicht SChannel verstehen, möglicherweise alle TLS-Kommunikation mit Ihrem Server funktionieren nicht mehr. In diesem Fall müssen Sie den Eintrag *FrontEndSSLCipherSuiteOrder* **ClusterSettings** entfernen, und übermitteln Sie die aktualisierte Ressourcenmanager Vorlage, um die Standardeinstellungen für Verschlüsselung Suite wiederherzustellen.  Verwenden Sie dieses Feature mit Vorsicht.

## <a name="get-started"></a>Erste Schritte
Die Websitevorlage der Azure Schnellstart Ressourcenmanager enthält eine Vorlage mit der Basis Definition zum [Erstellen einer App-Service-Umgebung](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->

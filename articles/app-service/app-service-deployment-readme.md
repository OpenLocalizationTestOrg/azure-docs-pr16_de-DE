<properties
    pageTitle="Bereitstellen von Applications to Azure App-Verwaltungsdienst"
    description="Informationen zum Bereitstellen von Applications App Dienst Arbeit"
    keywords="App-Dienst, Azure app Dienst bereitstellen, Bereitstellung"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="dariagrigoriu"/>

# <a name="azure-app-service-deployment-overview"></a>Übersicht über die App-Verwaltungsdienst Azure-Bereitstellung

App-Verwaltungsdienst Azure bietet eine umfangreiche und integrierte Feature zu erstellen und flexible Bereitstellung Workflows unterstützt. App-Bereitstellung kann Optionen, die fortlaufende Integration oder lokale Datenquellen-Steuerelements veröffentlichen, enthalten WebDeploy und FTP nutzen. Die empfohlene Methode für die Bereitstellung der Herstellung-app ist Bereitstellung Slot austauschen. Bereitstellung Steckplätze darstellen Staging und Integration Umgebungen Herstellung apps zugeordnet. Bereitstellung Steckplätze konfiguriert und mit Web-Verkehr für die Überprüfung ausgerichtet werden können, und den Datenverkehr ausgetauscht werden kann, bei Bedarf für die Bereitstellung auf Herstellung ohne nach unten Zeit und automatisierte Aufwärmphase. Die Schritte eines Workflows für die Bereitstellung können einfach über Release Management-Produkte wie etwa Visual Studio lassen Management automatisierte werden. Dies ist nützlich für zusammen mit anderen Lösung Ressourcen (z. B. Datenspeicher) oder Serie und Replikation über mehrere Einheiten der Bereitstellung. 

[AZURE.INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]

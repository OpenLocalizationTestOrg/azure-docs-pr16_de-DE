<properties
    pageTitle="Wie Sie eine Instanz der Azure-API Management-Dienst in mehrere Azure Regionen bereitstellen"
    description="Erfahren Sie, wie eine Azure-API Management Service-Instanz in mehrere Azure Regionen bereitgestellt." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Wie Sie eine Instanz der Azure-API Management-Dienst in mehrere Azure Regionen bereitstellen

Verwaltung von API unterstützt mehrere Region Bereitstellung der API Herausgeber zum Verteilen von einer einzelnen API-Verwaltungsdienst auf eine beliebige Anzahl von bestimmte Azure Dokumentabschnitte kann. Dadurch wird die Anforderung Wartezeit durch angesehen geografischen verteilt API Nutzer sowie eine verbesserte Verfügbarkeit von Diensten, wenn eine Region in den Offlinemodus wechselt verringert. 

Wenn eine API Verwaltungsdienst erstmalig erstellt wird, enthält nur eine [Einheit][] und befindet sich in einem einzigen Azure Bereich, das als die primäre Region bezeichnet wird. Zusätzliche Regionen können einfach über klassische Azure-Portal hinzugefügt werden. API Management Gateway-Server für die Region bereitgestellt wird, und mit dem nächsten Gateway Anruf Datenverkehr weitergeleitet werden. Wenn Sie ein Bereich den Offlinemodus wechselt, wird der Datenverkehr automatisch mit dem nächsten nächstgelegenen Gateway umgeleitet. 

> [AZURE.IMPORTANT] Mehrere Region Bereitstellung ist nur in der **[Premium][]** Ebene verfügbar.

## <a name="add-region"> </a>Bereitstellen eine API Management Service-Instanz neue Region

Um anzufangen, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Navigieren Sie zur Registerkarte **Skalierung** klassischen Azure-Portal für Ihre API Management Service-Instanz. 

![Registerkarte Skalierung][api-management-scale-service]

Um eine neue Region bereitstellen, klicken Sie auf die Dropdownliste unterhalb des primären Bereichs, und wählen Sie einen Bereich aus der Liste aus.

![Region hinzufügen][api-management-add-region]

Nachdem der Bereich ausgewählt ist, wählen Sie die Anzahl der Einheiten für den neuen Bereich aus der Dropdownliste Einheit aus.

![Angeben von Einheiten][api-management-select-units]

Nachdem Sie die gewünschten Regionen und Einheiten konfiguriert haben, klicken Sie auf **Speichern**.

## <a name="remove-region"> </a>Eine API Management Service-Instanz aus einer Region löschen

Navigieren Sie zum Entfernen einer API Management Service-Instanz aus einer Region zur Registerkarte **Skalierung** klassischen Azure-Portal für Ihre API Management Service-Instanz. 

![Registerkarte Skalierung][api-management-scale-service]

Klicken Sie auf das **X** rechts neben die gewünschte Region zu entfernen.  

![Entfernen der region][api-management-remove-region]

Nachdem Sie die gewünschten Regionen entfernt werden, klicken Sie auf **Speichern**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Erste Schritte mit Azure-API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[Einheit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/


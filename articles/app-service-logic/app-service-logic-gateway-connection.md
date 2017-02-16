<properties
   pageTitle="Logik Apps lokal Gateway Datenverbindung | Microsoft Azure"
   description="Informationen zum Erstellen einer Verbindung mit dem Gateway der lokalen Daten aus einer app Logik."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Verbinden Sie mit dem lokalen datenverwaltungsgateway für Logik Apps

Unterstützte Logik apps Verbinder können Sie eine Verbindung mit dem Zugriff auf lokale Daten über die lokale datenverwaltungsgateway konfigurieren.  Die folgenden Schritte führen Sie bis zum Installieren und Konfigurieren des Gateways lokaler Daten mit einer app Logik entwickelt.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Müssen mithilfe einer Arbeit oder Schule e-Mail-Adresse in Azure, Gateways lokaler Daten mit Ihrem Konto (Azure Active Directory-basierten Konto) zugeordnet werden soll
    * Wenn Sie ein Microsoft-Account verwenden (z. B. @outlook.com, @live.com) können Sie zum Erstellen einer Arbeit oder Schule e-Mail-Adresse anhand [der folgenden Schritte](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) Azure-Konto

> [AZURE.WARNING] Es gibt eine Einschränkung aktuell, die lokal Gateway installieren nur abgeschlossen wird, wenn ein Konto verwenden, die mit Power BI registriert wurde.  Registrieren Sie in der Zwischenzeit einem beliebigen Konto mit "Power BI kostenlosen" erfolgreichen Abschluss die Installation.

* Müssen die lokale Daten Gateways [auf einem lokalen Computer installiert](app-service-logic-gateway-install.md)haben.
* Gateways muss nicht haben wurde in Anspruch genommen durch ein anderes Azure lokaler datenverwaltungsgateway ([beanspruchen geschieht durch Erstellen von Schritt 2 unten](#2-create-an-azure-on-premises-data-gateway-resource)): eine Installation kann nur einem Gateway Ressource zugeordnet werden.

## <a name="installing-and-configuring-the-connection"></a>Installieren und Konfigurieren der Verbindung

### <a name="1-install-the-on-premises-data-gateway"></a>1: Installieren Sie lokaler Daten Gateways

Informationen zum Installieren des Gateways lokaler Daten finden Sie [in diesem Artikel](app-service-logic-gateway-install.md).  Das Gateway muss auf einem lokalen Computer installiert sein, bevor Sie die restlichen Schritte fortsetzen können.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. erstellen Sie eine lokale Azure datenverwaltungsgateway Ressource

Nach der Installation müssen Sie Ihr Abonnement Azure lokaler Daten Gateways zuordnen.

1. Melden Sie sich mit dem gleichen Arbeit oder Schule e-Mail-Adresse, die während der Installation des Gateways verwendet wurde Azure
1. Klicken Sie auf die Schaltfläche für **neue** Ressource
1. Suchen Sie und wählen Sie den **lokalen datenverwaltungsgateway**
1. Führen Sie die Informationen, um das Gateway mit Ihrem Konto – einschließlich den jeweiligen **Installation Namen** zuzuordnen.

    ![Lokaler Datenverwaltungsgateway-Verbindung][1]
1. Klicken Sie auf die Schaltfläche **Erstellen** , um die Ressource zu erstellen.

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3 Erstellen Sie 3 eine app-Verbindung Logik im designer

Jetzt, da Ihr Abonnement Azure eine Instanz des Gateways Daten lokal zugeordnet ist, können Sie eine Verbindung mit von innerhalb einer Logik app erstellen.

1. Öffnen Sie eine app Logik, und wählen Sie einen Verbinder, der lokal Connectivity (derzeit SQL Server) unterstützt
1. Aktivieren Sie das Kontrollkästchen für **Verbinden über lokaler datenverwaltungsgateway**

    ![Erstellung von Logik App-Designer Gateway][2]
1. Wählen Sie den **Gateway** zum Herstellen einer Verbindung mit, und führen Sie eine beliebige andere Verbindungsinformationen erforderlich
1. Klicken Sie auf **Erstellen** , um die Verbindung zu erstellen.

Die Verbindung sollte jetzt erfolgreich für die Verwendung in Ihrer app Logik konfiguriert werden.  

## <a name="next-steps"></a>Nächste Schritte
- [Allgemeine Beispiele und Szenarien Logik apps](app-service-logic-examples-and-scenarios.md)
- [Enterprise-Funktionen](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG
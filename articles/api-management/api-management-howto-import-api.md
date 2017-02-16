<properties 
    pageTitle="Grundlegende Konzepte Management-API" 
    description="Informationen Sie zu APIs, Produkten, Rollen, Gruppen und andere Konzepte API Management." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>So importieren Sie die Definition einer API mit Operationen in Azure-API Management

API-Verwaltung neue APIs erstellt werden können, und die Vorgänge manuell hinzugefügt oder die API sowie die Vorgänge in einem Schritt importiert werden.

APIs und ihre Vorgänge können mit den folgenden Formaten importiert werden.

-   WADL
-   Swagger

Dieses Handbuch zeigt wie erstellen Sie eine neue API und Vorgänge in einem Schritt importieren. Informationen zum Manuelles Erstellen einer API und Vorgänge hinzufügen finden Sie unter [Verfahren zum Erstellen von APIs][] und [zum Hinzufügen von Vorgängen zu einer API][].

## <a name="import-api"> </a>Importieren eine API

APIs erstellt und im Publisher-Portal konfiguriert. Wenn Sie das Publisher-Portal zugreifen zu können, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

![Publisher-portal][api-management-management-console]

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , und klicken Sie dann auf **API importieren**.

![Import-API][api-management-import-apis]

Der **Import-API** enthält drei Registerkarten, die die drei Methoden zum Bereitstellen von API-Spezifikation entsprechen.

-   **Aus der Zwischenablage** ermöglicht die API Spezifikation im Feld festgelegten Texts einzufügen.
-   **Aus Datei** ermöglicht es Ihnen, navigieren Sie zu, und wählen Sie die Datei, die die API Spezifikation enthält aus.
-   **Aus der URL** können Sie die URL der Spezifikation für die-API bereitstellen.

![Der-API-Importformat][api-management-import-api-clipboard]

Verwenden Sie nach der API-Spezifikation Optionsfelder auf der rechten Seite, um das Format Specification anzugeben. Die folgenden Dateiformate werden unterstützt.

-   WADL
-   Swagger

Geben Sie anschließend ein **Suffix API-Web-URL**ein. Dies ist die Basis-URL für Ihre API-Verwaltungsdienst angefügt. Die Basis URL häufig alle APIs auf jede Instanz von einer API Verwaltungsdienst gehostet werden. API Management unterscheidet APIs, indem Sie ihre Suffix und daher das Suffix muss für jede API in einer bestimmten API Management Service-Instanz eindeutig sein.

Nachdem alle Werte eingegeben werden, klicken Sie auf **Speichern** , um die API und die zugeordneten Vorgänge erstellen. 

>[AZURE.NOTE] Ein Lernprogramm Import einen einfachen Taschenrechner API im Format Swagger finden Sie unter [Verwalten der erste API in Azure-API Management](api-management-get-started.md).

## <a name="export-api"></a> Exportieren eine API

Neben dem neuen APIs importieren können Sie die Definitionen der APIs von Publisher-Portal exportieren. Klicken Sie hierzu auf **Export-API** über die **Registerkarte Zusammenfassung** Ihrer **API**aus.

![Export-API][api-management-export-api]

APIs können mithilfe von WADL oder Swagger exportiert werden. Wählen Sie das gewünschte Format aus, klicken Sie auf **Speichern**, und wählen Sie den Speicherort, in dem die Datei gespeichert.

![Exportformat-API][api-management-export-api-format]

## <a name="next-steps"> </a>Nächste Schritte

Sobald eine API erstellt wird und die Vorgänge importiert haben, können Sie überprüfen und konfigurieren weiteren Einstellungen, die API zu einem Produkt hinzufügen, und veröffentlichen, sodass es für Entwickler verfügbar ist. Weitere Informationen finden Sie unter den folgenden Handbüchern.

-   [Konfigurieren von Einstellungen API][]
-   [So erstellen und Veröffentlichen eines Produkts][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance

[Zum Hinzufügen von Vorgängen zu einer API]: api-management-howto-add-operations.md
[So erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[So erstellen Sie APIs]: api-management-howto-create-apis.md
[Konfigurieren von Einstellungen API]: api-management-howto-create-apis.md#configure-api-settings

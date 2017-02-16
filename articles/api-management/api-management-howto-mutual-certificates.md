<properties 
    pageTitle="Wie Back-End-Services-Clients Zertifikatauthentifizierung in Azure-API Management gesichert" 
    description="Erfahren Sie, wie Sie mithilfe der Client Zertifikatauthentifizierung in Azure-API Management Back-End-Dienste zu schützen." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Wie Back-End-Services-Clients Zertifikatauthentifizierung in Azure-API Management gesichert

Projektmanagement-API ermöglicht das Zugriff auf die Back-End-Dienst, der eine API gesichert Client-Zertifikate. Dieses Handbuch zeigt zum Verwalten von Zertifikaten im Portal Publisher API und so konfigurieren Sie eine API, um ein Zertifikat verwenden, um die Back-End-Dienst zugreifen.

Informationen zum Verwalten von Zertifikaten mithilfe der API Management REST-API finden Sie unter [Azure API Management REST API Zertifikat Entität][].

## <a name="prerequisites"> </a>Erforderliche Komponenten

In diesem Handbuch wird gezeigt, wie konfigurieren Instanz der API Management-Dienst, um Client Zertifikatauthentifizierung zu verwenden, um die Back-End-Dienst für eine API zuzugreifen. Bevor Sie die Schritte in diesem Thema, sollten Sie Ihre Back-End-Dienst für Client Zertifikatauthentifizierung ([zum Konfigurieren Zertifikatauthentifizierung in Azure-WebSites finden Sie in diesem Artikel][]) konfiguriert haben, und haben Zugriff auf das Zertifikat und das Kennwort für das Zertifikat für das Hochladen in Publisher-API Verwaltungsportal.

## <a name="step1"> </a>Ein Client-Zertifikat hochladen

Um anzufangen, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-API-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Sicherheit** , und klicken Sie auf **Client-Zertifikate**.

![Client-Zertifikate][api-management-security-client-certificates]

Wenn Sie ein neues Zertifikat hochladen möchten, klicken Sie auf **Zertifikat hochladen**.

![Hochladen des Zertifikats][api-management-upload-certificate]

Navigieren Sie zu Ihrem Zertifikat, und geben Sie das Kennwort für das Zertifikat.

>Das Zertifikat muss im **PFX** -Format. Selbstsignierte Zertifikaten zulässig sind.

![Hochladen des Zertifikats][api-management-upload-certificate-form]

Klicken Sie auf **Hochladen** , um das Zertifikat hochladen.

>Das Zertifikatskennwort zu diesem Zeitpunkt überprüft. Wenn sie falsch ist, wird eine Fehlermeldung angezeigt.

![Zertifikat geladen][api-management-certificate-uploaded]

Nachdem das Zertifikat hochgeladen wird, wird er auf der Registerkarte **Client-Zertifikate** angezeigt. Wenn Sie mehrere Zertifikate besitzen, stellen Sie eine Notiz Teils des Betreffs oder die letzten vier Zeichen des Fingerabdrucks, die verwendet werden, um das Zertifikat auszuwählen, beim API zur Verwendung von Zertifikaten, konfigurieren, wie im folgenden Abschnitt [Konfigurieren einer API verwendet ein Client-Zertifikat für Gateway-Authentifizierung][] behandelt.

>Zum Deaktivieren des Zertifikats Kette Überprüfung bei Verwendung, beispielsweise ein selbst signiertes Zertifikat führen Sie die Schritte in diesem FAQ- [Element](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)beschrieben.

## <a name="step1a"> </a>Löschen eines Clientzertifikats

Um ein Zertifikat zu löschen, klicken Sie neben das gewünschte Zertifikat auf **Löschen** .

![Löschen von Zertifikaten][api-management-certificate-delete]

Klicken Sie auf **Ja, löschen Sie ihn** , um zu bestätigen.

![Löschen bestätigen][api-management-confirm-delete]

Wenn das Zertifikat von einer API wird, wird ein Warnung Bildschirm angezeigt. Um das Zertifikat löschen müssen Sie zuerst das Zertifikat aus einem beliebigen APIs entfernen, die konfiguriert werden, um es zu verwenden.

![Löschen bestätigen][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfigurieren eine API um ein Client-Zertifikat für Gateway-Authentifizierung verwenden

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , klicken Sie auf den Namen der gewünschten-API, und klicken Sie auf der Registerkarte **Sicherheit** .

![API-Sicherheit][api-management-api-security]

Wählen Sie aus der Dropdownliste **mit den Anmeldeinformationen** **Client-Zertifikate** aus.

![Client-Zertifikate][api-management-mutual-certificates]

Wählen Sie das gewünschte Zertifikat aus der Dropdownliste **Client-Zertifikat** . Wenn es mehrere Zertifikate gibt können Sie den Betreff oder den letzten vier Zeichen des Fingerabdrucks wie im vorherigen Abschnitt erwähnt, um festzulegen, das richtige Zertifikat anzeigen.

![Wählen Sie Zertifikat][api-management-select-certificate]

Klicken Sie auf **Speichern** , um die Konfiguration Änderung an die API zu speichern.

>Diese Änderung gilt ab sofort, und Anrufe an Datenoperationen für diese API das Zertifikat zum Authentifizieren auf Back-End-Server verwenden soll.

![Speichern der Änderungen API][api-management-save-api]

>Wenn Sie ein Zertifikat für Gateway-Authentifizierung für die Back-End-Dienst einer API angegeben ist, wird ein Teil der Richtlinie für diese API und in den Richtlinien-Editor angezeigt werden können.

![Zertifikatrichtlinie][api-management-certificate-policy]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Methoden zum Ihren Back-End-Dienst, wie z. B. HTTP-grundlegende oder freigegebene Geheimnis Authentifizierung, finden Sie im folgende Video.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance

[Azure-API Management REST-API Zertifikat Entität]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[zum Konfigurieren Zertifikatauthentifizierung in Azure-WebSites finden Sie in diesem Artikel]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Konfigurieren einer API um ein Client-Zertifikat für Gateway-Authentifizierung verwenden]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 

<properties 
    pageTitle="So implementieren Wiederherstellung mit Service Sicherung und Wiederherstellen in Azure-API Management | Microsoft Azure" 
    description="Erfahren Sie, wie Sichern und wiederherstellen, um die Wiederherstellung in Azure-API Management ausführen." 
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

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>So implementieren Wiederherstellung mit Service Sicherung und bei Azure-API Verwaltung wiederherstellen

Veröffentlichen und Verwalten Ihrer APIs über Azure-API Management auswählen, indem nutzen Sie vieler Fehlertoleranz und Infrastrukturfunktionen, die Sie andernfalls hätten entwerfen, implementieren und verwalten. Die Azure-Plattform reduziert Dezimalbruch großen potenzielle Fehler bei der Kosten Dezimalbruch.

Zum Wiederherstellen von Verfügbarkeit sollten Probleme, die Auswirkungen der Region, in dem Verwaltung Ihrer API Dienst ist gehostet, bereit sind, den Dienst in einem anderen Bereich jederzeit wiederherstellen. Je nach Ihren Zielen zur Verfügbarkeit und Wiederherstellung Zeit Ziel sollten Sie reservieren eines Sicherung Diensts in einem oder mehreren Bereichen, und versuchen, ihre Konfiguration und Inhalt synchron mit dem Dienst aktiv verwalten. Der Dienst sichern und Wiederherstellen-Feature stellt die notwendigen Baustein für Ihre Wiederherstellen nach Datenverlusten implementieren.

Dieses Handbuch zeigt zum Authentifizieren Azure Ressourcenmanager Anfragen und Sichern und Wiederherstellen Ihrer API Management Service Instanzen.

>[AZURE.NOTE] Das Verfahren zum Sichern und Wiederherstellen einer API Management Service-Instanz für die Wiederherstellung kann auch für die Replikation-API Management Service Instanzen für Szenarios wie das Staging verwendet werden.
>
>Beachten Sie, dass jede Sicherung nach sieben Tage läuft ab. Wenn Sie versuchen, eine Sicherung wiederherstellen, nachdem die Gültigkeitsdauer 7 Tage abgelaufen ist, tritt die Wiederherstellung mit einer `Cannot restore: backup expired` Nachricht.

## <a name="authenticating-azure-resource-manager-requests"></a>Authentifizierung Azure Ressourcenmanager anfordert.

>[AZURE.IMPORTANT] Die REST-API für Sicherung und Wiederherstellung Azure Ressourcenmanager verwendet und hat einen anderen Authentifizierungsmechanismus als die REST-APIs für die Verwaltung von Ihre-Einheiten API Management. Die Schritte in diesem Abschnitt beschreiben, wie mit Azure Ressourcenmanager Anfragen authentifiziert wird. Weitere Informationen finden Sie unter [Authentifizierung Azure Ressourcenmanager Anfragen](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Die Aufgaben, die Sie auf Ressourcen mithilfe der Ressourcenmanager Azure erledigen müssen mit Azure Active Directory mithilfe der folgenden Schritte authentifiziert werden.

-   Hinzufügen einer Anwendungs zu den Azure-Active Directory-Mandanten.
-   Festlegen von Berechtigungen für die Anwendung, die Sie hinzugefügt haben.
-   Rufen Sie Token für die Authentifizierung von Anfragen zu Azure Ressourcenmanager ab.

Der erste Schritt besteht im Erstellen einer Azure Active Directory-Anwendungs. Melden Sie sich bei der [Klassischen Azure-Portal](http://manage.windowsazure.com/) mithilfe des Abonnements, die Ihre API Management Service-Instanz enthält, und navigieren Sie zur Registerkarte **Applications** für Ihren Standardkalender Azure Active Directory.

>[AZURE.NOTE] Wenn das standardmäßige Azure Active Directory-Verzeichnis nicht bei Ihrem Konto angezeigt wird, wenden Sie sich an den Administrator des Azure-Abonnements, um die erforderlichen Berechtigungen für Ihr Konto gewähren. Informationen zur Suche nach Ihrem standardmäßigen Verzeichnis finden Sie unter [Suchen Ihrer Standard-Verzeichnis](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Erstellen von Azure Active Directory-Anwendung][api-management-add-aad-application]

Klicken Sie auf **Hinzufügen**, **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**, und wählen Sie **Native Client-Anwendung**. Geben Sie einen beschreibenden Namen ein, und klicken Sie auf den Pfeil nach rechts. Geben Sie einen Platzhalter-URL wie `http://resources` für den **URI umleiten**, wie er ist ein erforderliches Feld, aber der Wert wird nicht später verwendet. Klicken Sie auf das Kontrollkästchen, um die Anwendung zu speichern.

Nachdem Sie die Anwendung gespeichert ist, klicken Sie auf **Konfigurieren**, führen Sie einen Bildlauf nach unten bis zum Abschnitt **Berechtigungen für andere Programme** und klicken Sie auf **Anwendung hinzufügen**.

![Hinzufügen von Berechtigungen][api-management-aad-permissions-add]

Wählen Sie **Windows** **Azure Service Management-API** aus, und klicken Sie auf das Kontrollkästchen, um die Anwendung hinzugefügt haben.

![Hinzufügen von Berechtigungen][api-management-aad-permissions]

Klicken Sie neben der neu hinzugefügten **Windows** **Azure Service Management API** Anwendungs **Delegierter Berechtigungen** auf, aktivieren Sie das Kontrollkästchen für das **Access Azure Servicemanagement (Preview)**, und klicken Sie auf **Speichern**.

![Hinzufügen von Berechtigungen][api-management-aad-delegated-permissions]

Vor dem Aufrufen die APIs, die Sicherung generieren oder wiederherstellen, ist es erforderlich, ein Token abzurufen. Im folgende Beispiel wird das [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) Nuget-Paket, das Token abzurufen verwendet.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Ersetzen Sie `{tentand id}`, `{application id}`, und `{redirect uri}` mit den folgenden Anweisungen.

Ersetzen Sie `{tenant id}` die Mandanten-ID, die soeben erstellte Azure Active Directory-Anwendung. Sie können die Id zugreifen, indem Sie auf **Ansicht Endpunkte**.

![Endpunkte][api-management-aad-default-directory]

![Endpunkte][api-management-endpoint]

Ersetzen Sie `{application id}` und `{redirect uri}` die **Client-Id** und die URL aus dem Abschnitt **Uris umleiten** Ihrer Azure Active Directory-Anwendung **Konfigurieren** Registerkarte verwenden. 

![Ressourcen][api-management-aad-resources]

Nachdem Sie die Werte angegeben sind, sollte das Codebeispiel ein Token ähnlich wie im folgenden Beispiel zurück.

![Token][api-management-arm-token]

Legen Sie vor dem Aufrufen die Sicherung und Wiederherstellung Vorgänge, die in den folgenden Abschnitten beschrieben, die Autorisierung Anforderung Kopfzeile für Ihren Anruf REST aus.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Sichern einer API-Verwaltungsdienst
Um eine Management-API Problem die folgenden HTTP Dienstanfrage sichern:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

wobei Folgendes gilt:

* `subscriptionId`-Id des Abonnements mit der Sie versuchen, API-Verwaltungsdienst zu sichern
* `resourceGroupName`-einer Zeichenfolge in Form von 'Api - Standard-{Service-Region}', in dem `service-region` identifiziert den Azure Bereich, in dem die Verwaltung API service Sie, versuchen, Sicherung gehostet wird, z. B.`North-Central-US`
* `serviceName`– den Namen der API Verwaltungsdienst Sie machen eine Sicherungskopie der angegebenen Zeitpunkt des seiner Erstellung
* `api-version`-Ersetzen durch`2014-02-14`

Geben Sie im Textkörper der Anfrage die Ziel Azure-Speicher Kontonamen, Zugriffstaste, Container mit dem Namen Blob und Name der Sicherungskopie aus:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Legen Sie den Wert, der die `Content-Type` Anforderung Kopfzeile zu `application/json`.

Sicherung ist eine umfangreiche Operation, die mehrere Minuten in Anspruch nehmen kann.  Wenn die Anforderung erfolgreich war und die Sicherung wurde initiiert Sie erhalten eine `202 Accepted` Antwort-Code, der mit einer `Location` Kopfzeile.  Stellen Sie "GET-Anfragen an die URL in" die `Location` Kopfzeile, um herauszufinden, der Status des Vorgangs. Während die Sicherung durchgeführt wird erhalten Sie weiterhin einen '202 akzeptiert' Statuscode. Eine Antwortcode der `200 OK` erfolgreichen Abschluss des Vorgangs Sicherung.

**Hinweis**:

- **Container** angegeben haben, in die Anfrage Text **vorhanden sein muss**.
* Während die Sicherung durchgeführt wird Sie **Alle Servicemanagement sollten nicht versuchen** , z. B. SKU aktualisieren oder downgrade, Domäne Namensänderung usw.. 
* Wiederherstellen von einer **Sicherungskopie ist sichergestellt, dass nur für 7 Tage** seit dem Moment erstellen. 
* Zum Erstellen von Analytics verwendete **Verwendungsdaten** Berichte in die Sicherung **wird nicht berücksichtigt** . Verwenden Sie [Azure API Management REST-API][] , um regelmäßig Analytics-Berichte zur sicheren Aufbewahrung abzurufen.
* Die Häufigkeit, mit der Sie Dienst Sicherung, wirkt sich auf die Zielsetzung der Wiederherstellung Punkt. Zum Minimieren empfehlen wir Implementieren von regelmäßigen Sicherungen ebenso wie bei Bedarf von Sicherungskopien nach dem vornehmen von wichtiger Änderungen an Ihrer API Verwaltungsdienst.
* **Änderungen** versucht, die Dienstkonfiguration (z. B. APIs, Richtlinien, Developer Portal Darstellung) während Sicherung befindet sich im Prozess **möglicherweise nicht in die Sicherung einbezogen und daher geht verloren**.

## <a name="step2"> </a>Wiederherstellen einer API-Verwaltungsdienst
Klicken Sie zum Wiederherstellen einer Management-API Dienst aus einer zuvor erstellten Sicherung die folgende HTTP-Anforderung vornehmen:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

wobei Folgendes gilt:

* `subscriptionId`-Id des Abonnements, den Sie eine Sicherung in wiederherstellen API Verwaltungsdienst enthält
* `resourceGroupName`-einer Zeichenfolge in Form von 'Api - Standard-{Service-Region}', in dem `service-region` die Azure Region, der Sie eine Sicherung in wiederherstellen API Verwaltungsdienst gehostet wird, z. B. bezeichnet`North-Central-US`
* `serviceName`– den Namen der Dienst wird wiederhergestellt in Zeitpunkt des seiner Erstellung angegebenen Management-API
* `api-version`-Ersetzen durch`2014-02-14`

Geben Sie im Textkörper der Anfrage Speicherort der Sicherungsdatei, d. h. Kontonamen Azure-Speicher, Zugriffstaste, Container mit dem Namen Blob und Name der Sicherungskopie aus:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Legen Sie den Wert, der die `Content-Type` Anforderung Kopfzeile zu `application/json`.

Wiederherstellen ist eine umfangreiche Operation, die auf 30 oder mehrere Minuten in Anspruch nehmen kann.  Wenn die Anforderung erfolgreich war und des Wiederherstellungsvorgangs initiiert wurde Sie erhalten eine `202 Accepted` Antwort-Code, der mit einer `Location` Kopfzeile.  Stellen Sie "GET-Anfragen an die URL in" die `Location` Kopfzeile, um herauszufinden, der Status des Vorgangs. Während die Wiederherstellung ausgeführt wird, erhalten Sie weiterhin erhalten '202 akzeptiert' Statuscode. Eine Antwortcode der `200 OK` erfolgreichen Abschluss des Wiederherstellungsvorgangs.

>[AZURE.IMPORTANT] **Der SKU** der der Dienst wird wiederhergestellt in **entsprechen muss** die SKU des gesicherten Dienst wird wiederhergestellt.
>
>**Änderungen** vorgenommen während der Wiederherstellung die Dienstkonfiguration (z. B. APIs, Richtlinien, Developer Portal Darstellung) ist in den Fortschritt **überschrieben werden konnte**.

## <a name="next-steps"></a>Nächste Schritte
Schauen Sie sich die folgenden Microsoft Blogs für zwei verschiedene Vorgehensweisen des Prozesses Sicherung und Wiederherstellung.

-   [Repliziert Azure-API Management-Konten](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Vielen Dank an Gisela für ihren Beitrag zu diesem Artikel.
-   [Verwaltung von Azure-API: Sichern und Wiederherstellen von Konfiguration](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Der ausführliche durch Stuart Ansatz entspricht nicht den offiziellen Leitfaden, aber es ist sehr interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure-API Management REST-API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 

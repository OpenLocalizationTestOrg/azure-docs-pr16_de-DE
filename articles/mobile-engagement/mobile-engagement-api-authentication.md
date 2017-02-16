<properties 
    pageTitle="Authentifizieren Sie mit mobilen Engagement REST-APIs"
    description="Beschreibt, wie Sie mit Azure Mobile Engagement REST-APIs authentifizieren" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Authentifizieren Sie mit mobilen Engagement REST-APIs

## <a name="overview"></a>(Übersicht)

Dieses Dokument beschreibt, wie eine gültige AAD Oauth token mit den Mobile Engagement REST-APIs authentifizieren erhältlich. 

Es wird vorausgesetzt, dass Sie über ein gültiges Azure-Abonnement verfügen und eine mithilfe einer der unsere [Entwicklertools Lernprogramme](mobile-engagement-windows-store-dotnet-get-started.md)Engagement Mobile-app erstellt haben.

## <a name="authentication"></a>Authentifizierung

Microsoft Azure Active Directory basiert OAuth Token für die Authentifizierung verwendet wird. 

In der Reihenfolge zu Authentifizierung eine API Besprechungsanfrage muss eine Kopfzeile Autorisierung auf jeder Anforderung also das folgende Format hinzugefügt werden:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory Token enden in 1 Stunde.

Es gibt mehrere Methoden, ein Token abzurufen. Da die APIs im Allgemeinen aus einem Clouddienst aufgerufen werden, einen API-Schlüssel verwenden möchten. Ein API Key in Azure Terminologie heißt ein Kennwort für das Hauptbenutzer. Das folgende Verfahren beschreibt eine Möglichkeit zum manuellen einrichten.

### <a name="one-time-setup-using-script"></a>Einmalige einrichten (mit Skript)

Folgen Sie den Satz von folgenden Anweisungen zum Ausführen der Einrichtung mithilfe eines PowerShell-Skripts, die die minimale Zeit für die Installation benötigt, aber die am häufigsten zulässigen Standardeinstellungen verwendet. Optional können Sie auch die Anweisungen in das [manuelle Einrichten](mobile-engagement-api-authentication-manual.md) für direkt vom Azure-Portal wird, und führen Sie eine genauere Konfiguration. 

1. Erhalten Sie die neueste Version von Azure PowerShell von [hier](http://aka.ms/webpi-azps)aus. Weitere Informationen zu den Anweisungen herunterladen können Sie diesen [Link](../powershell-install-configure.md)finden Sie unter.  

2. Nachdem Azure PowerShell installiert ist, verwenden Sie die folgenden Befehle, um sicherzustellen, dass Sie der **Azure-Modul** installiert haben:

    ein. Stellen Sie sicher, dass das Azure PowerShell-Modul in der Liste der verfügbaren Module verfügbar ist. 
    
        Get-Module –ListAvailable 

    ![Verfügbare Azure Module][1]
        
    b. Wenn Sie nicht das Azure PowerShell-Modul in der vorstehenden Liste finden, müssen Sie Folgendes ausführen:
        
        Import-Module Azure 
        
3. Anmelden zum Azure-Manager aus PowerShell, indem Sie den folgenden Befehl ausführen, und geben Sie Ihren Benutzernamen und Ihr Kennwort für Ihr Azure an: 
        
        Login-AzureRmAccount

4. Wenn Sie mehrere Abonnements haben sollten Sie Folgendes ausführen:

    ein. Abrufen einer Liste aller Ihrer Abonnements, und kopieren Sie die SubscriptionId des Abonnements, die Sie verwenden möchten. Stellen Sie sicher, dass dieses Abonnement ist identisch, die der Mobile-App Engagement hat, die Sie für die Interaktion mit mithilfe der APIs abgelegt werden. 

        Get-AzureRmSubscription

    b. Führen Sie den folgenden Befehl Bereitstellen der SubscriptionId so konfigurieren Sie das Abonnement verwendet werden.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Kopieren Sie den Text für das Skript [Neu-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) auf Ihrem Computer, und speichern Sie es als PowerShell-Cmdlet (z. B. `APIAuth.ps1`), und führen Sie sie `.\APIAuth.ps1`. 
    
6. Das Skript fragt Sie dafür, dass eine Eingabe **' PrincipalName '**. Geben Sie einem geeigneten Namen so, dass Sie zum Erstellen einer Active Directory-Anwendung (z. B. APIAuth) verwenden möchten. 

7. Nach Abschluss des Skripts wird es angezeigt, die folgenden vier Werte, die Sie programmgesteuert mit AD authentifizieren müssen daher Vergewissern Sie sich, um sie zu kopieren. 
        
    **TenantId**, **SubscriptionId**, **p**und **geheim**.

    Verwenden Sie TenantId als `{TENANT_ID}`, p als `{CLIENT_ID}` und als geheimen `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Der standardmäßige Sicherheitsrichtlinie Ausführung eines PowerShell-Skripts blockiert werden möglicherweise. In diesem Fall konfigurieren Sie vorübergehend Ihre Ausführungsrichtlinie zum Zulassen der Ausführung des Skripts mit dem folgenden Befehl aus:

        > Set-ExecutionPolicy RemoteSigned

8. So sieht wie der Satz von PS Cmdlets aussehen würde. 

    ![][3]

9. Aktivieren Sie im Verwaltungsportal Azure, dass eine neue AD-Anwendung mit dem Namen erstellt wurde, die Sie das Skript namens **' PrincipalName '** klicken Sie unter **Anzeigen von Applications meines Unternehmens zugegriffen**zur Verfügung gestellt.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Schritte zum Abrufen von eines gültigen Tokens

1. Rufen Sie die API mit den folgenden Parametern, und stellen Sie sicher, ersetzen Sie den MANDANTEN\_-ID, CLIENT\_-ID und CLIENT\_geheim:

    - **Anfordern von URL** als *https://login.microsoftonline.com/ {MANDANTEN\_ID} / oauth2/Token*
    - **HTTP-Inhaltstyp - Header** als *Anwendung/X-www-form-urlencoded*
    - **Http-anfordern Text** als *erteilen\_Typ = Client\_Anmeldeinformationen & Client_id = {CLIENT\_ID} & Client_secret = {CLIENT\_geheim} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Im folgenden finden ein Beispiel für eine Anforderung:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Hier ist eine Beispiel für Antwort:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    In diesem Beispiel wird im Lieferumfang von URL-Codierung der Parameter Beitrag, `resource` Wert ist tatsächlich `https://management.core.windows.net/`. Achten Sie auch die URL codiert `{CLIENT_SECRET}` wie es Sonderzeichen enthalten kann.

    > [AZURE.NOTE] Zum Testen, können Sie ein HTTP-Client-Tool wie [Fiddler](http://www.telerik.com/fiddler) oder [Chrome Postman Erweiterung](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) verwenden. 

2. Nun jeder Anruf API umfassen Sie den Autorisierung Anfrage-Header:

        Authorization: Bearer {ACCESS_TOKEN}

    Wenn eine 401 Fehlerstatus angezeigt wird, überprüfen Sie den Textbereich der Antwort es möglicherweise informieren Sie über das Token abgelaufen ist. In diesem Fall erhalten Sie ein neues Token.

##<a name="using-the-apis"></a>Verwenden die APIs

Jetzt, da Sie ein gültiges Token haben, sind Sie bereit sind, nehmen Sie die API-Aufrufe.

1. In jeder Anforderung API müssen Sie eine gültige, nicht abgelaufene Token die erhalten Sie im vorherigen Abschnitt übergeben.

2. Sie müssen einige Parameter in die Anfrage URI Einstecken der Ihrer Anwendung identifiziert. Die Anforderung sieht URI wie folgt aus.

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Wenn Sie erhalten die Parameter, klicken auf den Anwendungsnamen, und klicken Sie auf Dashboard, und Sie werden eine Seite wie folgt mit 3 Parametern angezeigt.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** Ihr Ressourcengruppe Namen hinzuzufügende **MobileEngagement** werden, es sei denn, Sie ein neuer erstellt. 

    ![Mobile Engagement API URI-Parameter][2]

>[AZURE.NOTE] <br/>
>1. Ignorieren Sie die API Stamm-Adresse wie folgt für die APIs der vorherigen wurde.<br/>
>2. Wenn Sie die app mit klassischen Azure-Portal erstellt haben, müssen Sie den Namen der Anwendungsressource verwenden, der anderen als den Namen der Anwendung selbst ist. Wenn Sie die app in der Azure-Portal erstellt sollten Sie den Namen der Anwendung verwenden (wird nicht unterschieden zwischen Anwendung Ressourcenname und App-Namen für apps in das neue Portal erstellt).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png




<properties
    pageTitle="Azure Active Directory Anmeldung Aktivität Bericht API Beispiele | Microsoft Azure"
    description="Gewusst wie: Erste Schritte mit der Azure Active Directory Reporting-API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory Anmeldung Aktivität Bericht API Beispiele

Dieses Thema ist Teil einer Sammlung von Themen zu den reporting API Azure-Active Directory.  
Azure AD-Berichte bietet eine API, die Sie auf Anmeldung Aktivitätsdaten mithilfe von Code oder verwandte Tools zugreifen kann.  
Der Umfang dieses Themas ist zu Beispiel-Code für die **Anmeldung Aktivität API**beschleunigen.

Siehe:

- [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) Weitere grundlegende Informationen
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) für Weitere Informationen über die reporting API.

Wenden Sie für Fragen, Probleme oder Feedback sich an [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie in den Beispielen in diesem Thema verwenden können, müssen Sie die [Voraussetzungen für die Berichterstattung API Azure AD-Zugriff auf](active-directory-reporting-api-prerequisites.md)ausführen.  


##<a name="powershell-script"></a>PowerShell-Skript

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Ausführen des Skripts
Sobald Sie stellen das Skript, führen Sie es, und stellen Sie sicher, dass die erwarteten Daten aus der Audit Bericht protokolliert werden zurückgegeben.

Das Skript gibt Ausgabe aus dem Bericht Anmeldung im JSON-Format zurück. Erstellt auch eine `SigninActivities.json` Datei mit der gleichen Ausgabe. Sie können durch ändern das Skript, um Daten aus anderen Berichte und kommentieren, die nicht benötigte Ausgabeformat zurückzukehren experimentieren.



## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie in den Beispielen in diesem Thema anpassen? Schauen Sie sich die [Anmeldung Aktivität Azure Active Directory-API-Referenz](active-directory-reporting-api-sign-in-activity-reference.md). 

- Wenn Sie eine vollständige Übersicht der Verwendung von Azure Active Directory reporting API anzeigen möchten, finden Sie unter [Erste Schritte mit der Azure-Active Directory-API-Berichte](active-directory-reporting-api-getting-started.md).

- Wenn Sie mehr zur Verwendung von Berichten Azure Active Directory erfahren möchten, finden Sie unter der [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  
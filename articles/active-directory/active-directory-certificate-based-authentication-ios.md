<properties 
    pageTitle="Erste Schritte mit basierendes Zertifikatauthentifizierung unter iOS | Microsoft Azure" 
    description="Informationen Sie zum Konfigurieren der basierendes Zertifikatauthentifizierung in Lösungen mit iOS-Geräte" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Erste Schritte mit basierendes Zertifikatauthentifizierung unter iOS - Public Preview-Version

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


In diesem Thema wird gezeigt, wie konfigurieren und basierendes Zertifikatauthentifizierung (CBA) auf einem iOS-Gerät für Benutzer von Mandanten in Office 365 Enterprise, Education und Business-Pläne verwenden. 

CBA können Sie mit einem Clientzertifikat auf einem Android oder iOS-Gerät von Azure Active Directory authentifiziert werden, wenn Ihr Exchange online-Konto zu verbinden: 

- Office mobile-Anwendungen wie Microsoft Outlook und Microsoft Word   
- Exchange ActiveSync (EAS)-clients 

Konfigurieren dieses Feature entfällt die Notwendigkeit eine Kombination Benutzernamen und Ihr Kennwort in bestimmten e-Mail- und Microsoft Office-Programme auf Ihrem mobilen Gerät eingeben. 
 

## <a name="supported-scenarios-and-requirements"></a>Unterstützte Szenarien und Anforderungen  



### <a name="general-requirements"></a>Allgemeine Vorschriften 


In allen Szenarien in diesem Thema werden die folgenden Aufgaben erforderlich:  

- Zugriff auf Zertifikat Autoritäten Client Zertifikate ausgeben.  

- Die Zertifikate Autoritäten müssen in Azure Active Directory konfiguriert sein. Sie können die detaillierten Schritte zum Abschließen der Konfiguration im Abschnitt [Erste Schritte](#getting-started) suchen.  

- Die Stammzertifizierungsstelle und eine mittlere Zertifizierungsstellen müssen in Azure Active Directory konfiguriert sein.  

- Jede Zertifizierungsstelle muss es sich um ein zertifikatsperrung Zertifikatsperrlisten verfügen, das über eine URL gegenüberliegende Internet verwiesen werden kann.  

- Das Client-Zertifikat muss für Client-Authentifizierung ausgestellt werden.  


- Nur Exchange ActiveSync-Clients muss das Client-Zertifikat geroutet-e-Mail-Adresse des Benutzers in Exchange online in den Namen der Tilgungsanteile oder den RFC822 Wert im Feld Betreff Alternative verfügen. Azure-Active Directory ordnet RFC822 den Wert für das Attribut Proxyadresse im Verzeichnis.  



### <a name="office-mobile-applications-support"></a>Office mobile Applikationen-Unterstützung 

| Apps                      | Support      |
| ---                       | ---          |
| Word / Excel / PowerPoint | ![Kontrollkästchen][1]  |
| OneNote                   | ![Kontrollkästchen][1]  |
| OneDrive                  | ![Kontrollkästchen][1]  |
| Outlook                   | ![Kontrollkästchen][1]  |
| Yammer                    | ![Kontrollkästchen][1]  |
| Skype für Unternehmen        | Demnächst  |


### <a name="requirements"></a>Anforderungen  

Die Geräteversion OS-muss iOS 9 und höher 

Ein Server Föderation muss konfiguriert sein.  

Azure Authentifizierung ist für Office-Clientanwendungen unter iOS erforderlich.  

Das Token ADFS muss Azure Active Directory Sperren eines Clientzertifikats die folgenden Ansprüche verfügen:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Die fortlaufende Zahl des Clientzertifikats) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Die Zeichenfolge für den Herausgeber des Clientzertifikats) 

Azure-Active Directory fügt diese Ansprüche auf das Aktualisieren Token hinzu, wenn sie in das ADFS Token (oder andere SAML-Token) verfügbar sind. Bei das Aktualisierung Token überprüft werden muss, wird diese Informationen verwendet, um die Sperrung zu überprüfen. 

Als bewährte Methode sollten Sie die ADFS Fehlerseiten mit folgenden aktualisieren:

- Der Anforderung zum Installieren von der Azure-Authentifizierung unter iOS

- So erhalten Sie ein Benutzerzertifikat Anweisungen. 

Weitere Informationen hierzu finden Sie unter [Anpassen der AD FS-anmelden Seiten](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Unterstützung von Exchange ActiveSync-clients 


Unter iOS 9 oder höher wird der systemeigenen iOS e-Mail-Client unterstützt. Alle anderen Exchange ActiveSync-Anwendungen um festzustellen, ob diese Funktion unterstützt wird, wenden Sie Ihrer Anwendung Entwicklertools.  



## <a name="getting-started"></a>Erste Schritte 


Um anzufangen, müssen Sie die Zertifizierungsstellen Azure Active Directory konfigurieren. Laden Sie für jede Zertifizierungsstelle die folgenden Schritte aus: 

- Den öffentlichen Teil des Zertifikats, *CER* -Format 

- Im Internet gegenüberliegende URLs, wo befinden sich die Zertifikatsperrlisten (Zertifikatsperrlisten)
 

Im folgenden wird das Schema für eine Zertifizierungsstelle aus: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Wenn Sie die Informationen hochladen, können Sie das Modul Azure AD-über Windows PowerShell verwenden.  
Nachstehend sind Beispiele für das Hinzufügen, entfernen oder Ändern von einer Zertifizierungsstelle aus. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Konfigurieren von Ihrem Azure AD-Mandanten für Zertifikat basierend Authentifizierung 

1. Starten Sie Windows PowerShell mit Administratorrechten an. 

2. Installieren Sie das Azure AD-Modul. Sie müssen Version [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) installieren oder höher.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Verbinden Sie mit Ihrem Mandanten Ziel: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Hinzufügen einer neuen Zertifizierungsstelle

1. Festlegen von verschiedenen Eigenschaften der Zertifizierungsstelle und sie Azure Active Directory hinzufügen: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Abrufen der Zertifizierungsstellen an: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Abrufen der Liste Zertifizierungsstellen

Abrufen der Zertifizierungsstellen, die derzeit in Azure Active Directory für Ihren Mandanten gespeichert: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Entfernen von einer Zertifizierungsstelle

1.  Abrufen der Zertifizierungsstellen an: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Das Zertifikat für die Zertifizierungsstelle zu entfernen: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying einer Zertifizierungsstelle 

1.  Abrufen der Zertifizierungsstellen an: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Ändern der Eigenschaften auf der Zertifizierungsstelle: 

        $c[0].AuthorityType=1 

3. Festlegen der **Zertifizierungsstelle**an: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Testen Office mobile-Clientanwendungen  

So testen Sie Zertifikatauthentifizierung auf Ihrem mobilen Office-Anwendung: 

1.  Installieren Sie auf Ihrem Testgerät einer mobilen Office-Anwendungs (z. B. OneDrive) aus dem App Store.

2.  Stellen Sie sicher, dass das Zertifikat auf Ihr Testgerät bereitgestellt wurde. 

3.  Starten Sie die Anwendung. 

4.  Geben Sie Ihren Benutzernamen ein, und wählen Sie dann das Zertifikat, das Sie verwenden möchten. 

Sie sollten erfolgreich angemeldet sein. 





## <a name="testing-exchange-activesync-client-applications"></a>Testen Exchange ActiveSync-Clientanwendungen

Exchange ActiveSync über basierendes Zertifikat-Authentifizierung zugreifen zu können, muss ein EAS-Profil, das Client-Zertifikat enthält Anwendung zur Verfügung. Das EAS-Profil muss die folgende Informationen enthalten:

- Das Zertifikat für die Authentifizierung verwendet werden 

- Der EAS-Endpunkt muss outlook.office365.com, (wie dieses Feature derzeit nur in der Exchange online-Mandanten mit mehreren-Umgebung unterstützt wird)

Ein EAS-Profil kann konfiguriert und auf dem Gerät durch die Nutzung von einer MDM wie Intune oder indem Sie ein Zertifikat manuell in das EAS-Profil auf dem Gerät platziert werden.  

### <a name="testing-eas-client-applications-on-ios"></a>Testen der EAS-Clientanwendungen unter iOS 

So prüfen Sie die Authentifizierung des Zertifikats mit der systemeigenen e-Mail-Anwendung unter iOS 9 oder höher: 

1.  Konfigurieren Sie ein EAS-Profil, das die oben genannten Anforderungen erfüllt. 

2.  Installieren Sie das Profil auf dem iOS-Gerät (entweder mit einer MDM, z. B. Intune oder die Anwendung Apple-Konfiguration)

3.  Nachdem das Profil ordnungsgemäß installiert ist, öffnen Sie die ursprüngliche E-Mail-Anwendung, und stellen Sie sicher, dass e-Mail synchronisiert wird



## <a name="revocation"></a>Sperrung

Um ein Client-Zertifikat widerrufen, Azure Active Directory der Sperrliste (Sperrliste) von den URLs als Teil der Informationen Zertifizierungsstelle hochgeladen abgerufen und zwischengespeichert. Veröffentlichen der letzten Zeitstempel (**Aktuelles Datum** Eigenschaft) in der Sperrliste verwendet, um sicherzustellen, die Sperrliste noch gültig ist. Die Sperrliste referenziert wird regelmäßig, um den Zugriff auf Zertifikate widerrufen, die einen Teil der Liste sind.

Wenn Sie eine weitere instant Sperrung (zum Beispiel, wenn ein Benutzer ein Gerät verliert) erforderlich ist, kann das Autorisierungstoken des Benutzers ungültig. Um die Autorisierungstoken ungültig zu machen, legen Sie das Feld **StsRefreshTokenValidFrom** für diesen bestimmten Benutzer mithilfe der Windows PowerShell aus. Das Feld **StsRefreshTokenValidFrom** für jeden Benutzer müssen zu widerrufen des Zugriffs auf aktualisiert werden.
 
Ist in der Sperrliste, um sicherzustellen, dass die Sperrung weiterhin besteht, müssen Sie das **Effektives Datum** die Sperrliste zu einem Datum nach der Wert von **StsRefreshTokenValidFrom** festgelegt, und vergewissern Sie sich das Zertifikat in Frage.
 
Die folgenden Schritte beschreiben das Verfahren zum Aktualisieren und das Autorisierungstoken durch Festlegen des **StsRefreshTokenValidFrom** Felds ungültig. 

1. Verbinden Sie mit Administrator-Anmeldeberechtigungen zum Dienst MSOL: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Rufen Sie den aktuellen StsRefreshTokensValidFrom-Wert für einen Benutzer: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Konfigurieren Sie einen neuen StsRefreshTokensValidFrom Wert für den aktuellen Zeitstempel der Benutzer gleich aus: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Das Datum, die, das Sie festlegen, muss in der Zukunft liegen. Wenn das Datum nicht in der Zukunft liegt, wird die **StsRefreshTokensValidFrom** -Eigenschaft nicht festgelegt werden. Wenn das Datum in der Zukunft liegt, wird die aktuelle Uhrzeit (nicht das Datum festlegen-MsolUser Befehl erkennbar) **StsRefreshTokensValidFrom** festgelegt. 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
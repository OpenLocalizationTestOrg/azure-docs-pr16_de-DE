<properties
    pageTitle="Azure MFA Cloud im Vergleich mit einer Server | Microsoft Azure"
    description="Wählen Sie die kombinierte Authentifizierung Secutiry Lösung rechts für Sie darin bestehen, welche Uhr i versuchen, secure Fragen und wo meiner Benutzer ansässig sind.  Wählen Sie dann die Cloud, MFA Server- oder AD FS aus."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Wählen Sie die Lösung Azure kombinierte Authentifizierung für Sie aus.

Da es verschiedene Typen von Azure mehrstufige Authentifizierung (MFA) zur Verfügung gibt, müssen wir beantworten einige Fragen zu ermitteln, welche Version das richtige Schema verwendet wird.  Diese Fragen gefunden werden:

-   [Was muss ich versuche, gesichert](#what-am-i-trying-to-secure)
-   [Wo befinden sich die Benutzer](#where-are-the-users-located)
- [Welche Features sind erforderlich?](#what-featured-do-i-need)

Die folgenden Abschnitte enthalten Anleitungen auf jeder der folgenden Antworten zu bestimmen.

## <a name="what-am-i-trying-to-secure"></a>Was muss Versuche ich, gesichert?

Wenn Sie die richtige in zwei Schritten Überprüfung Lösung ermitteln möchten, muss zuerst wir die Frage, welche sind Sie versuchen, mit einer zweiten Methode der Authentifizierung secure beantworten.  Handelt es sich um eine Anwendung, die in Azure ist?  Oder eines Systems RAS?  Durch bestimmen, welche wir schützen möchten, können wir beantworten die Frage, in denen kombinierte Authentifizierung muss aktiviert sein.  


Was möchten Sie secure| Mehrstufige Authentifizierung in der cloud|Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Von Erstanbietern Microsoft-apps|● |● |
SaaS apps im app-Katalog|● |● |
IIS-Applikationen über Azure AD App Proxy veröffentlicht|● |● |
IIS-Anwendungen, die nicht über Azure AD App Proxy veröffentlicht | |● |
Remotezugriff z. B. VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Wo befinden sich die Benutzer

Als Nächstes hindert betrachtet, wo sich unsere Benutzer befinden um die richtige Lösung zu verwenden, in der Cloud oder lokal mit der MFA-Server zu ermitteln.



Benutzerstandort einstellen| Mehrstufige Authentifizierung in der cloud|Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Azure-Active Directory|● | |
Azure AD- und lokalen Active Directory mithilfe des Föderation mit AD FS|● |● |
Azure AD- und lokalen Active Directory mithilfe von DirSync, Azure AD synchronisieren, Azure AD verbinden – ohne Kennwort synchronisieren|● |● |
Azure AD- und lokalen Active Directory mithilfe von DirSync, Azure AD synchronisieren, Azure AD Verbinden - mit Kennwort synchronisieren|● | |
Lokalen Active Directory| |● |

## <a name="what-features-do-i-need"></a>Welche Features sind erforderlich?

Die folgende Tabelle enthält einen Vergleich der Features, die mit kombinierte Authentifizierung in der Cloud und die kombinierte Authentifizierung-Server verfügbar sind.

 | Mehrstufige Authentifizierung in der cloud | Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Mobile-app Benachrichtigung als ein zweites Argument | ● | ● |
Mobile-app Überprüfungscode als zweite Faktor | ● | ●
Anruf als zweite Faktor | ● | ●
Unidirektionale SMS als zweite Faktor | ● | ●
Bidirektionale SMS als zweite Faktor |  | ●
Hardwaretoken als zweite Faktor |  | ●
App Kennwörter für Clients, die nicht MFA unterstützen | ● |  
Administrator Kontrolle über Authentifizierungsmethoden | ● | ●
PIN-Modus |  | ●
Betrug Benachrichtigung | ● | ●
MFA-Berichte | ● | ●
Einmalige umgehen |  | ●
Benutzerdefinierte Grüße für Telefonanrufe | ● | ●
Anpassbare Anrufer-ID für Telefonanrufe | ● | ●
Vertrauenswürdigen IP-Adressen | ● | ●
Denken Sie daran MFA für vertrauenswürdige Geräte  | ● |  
Bedingte access | ● | ●
Cache |  | ●

Jetzt, da wir, ob Sie Cloud kombinierte Authentifizierung oder der MFA Server lokal verwenden ermittelt haben, können wir Schritte zum Einrichten und Verwenden von Azure kombinierte Authentifizierung. **Wählen Sie das Symbol, das Ihrem Szenario darstellt!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>

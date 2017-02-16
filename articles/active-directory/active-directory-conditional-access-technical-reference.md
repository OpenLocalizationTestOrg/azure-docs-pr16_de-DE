
<properties
    pageTitle="Azure Active Directory bedingten Zugriff Technische Referenz | Microsoft Azure"
    description="Mit bedingten Access-Steuerelement überprüft Azure Active Directory die bestimmten Bedingungen, die Sie bei der Authentifizierung des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem Sie diese Bedingung erfüllt sind, wird der Benutzer authentifiziert und Zugriff auf die Anwendung zulässig."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technische Referenz zu Azure Active Directory bedingten Zugriff

## <a name="services-enabled-with-conditional-access"></a>Aktiviert mit bedingten Access Services
Regeln zur bedingten Access werden über verschiedene Arten von Azure AD-Anwendung unterstützt. Diese Liste enthält:

- Partnerverbundkontakte Applications aus dem Katalog der Azure AD-Anwendung
- Kennwort SSO Applications aus dem Katalog der Azure AD-Anwendung
- Mit dem Azure-Anwendungsproxy registriert Applikationen
- Entwickeltes Textzeile Business und mehrere Mandanten Applikationen mit Azure AD registriert
- Visual Studio Online
- Azure Remote-App
-   Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (einschließlich der OneDrive for Business)


## <a name="enable-access-rules"></a>Aktivieren von Access-Regeln

Jede Regel kann aktiviert oder deaktiviert werden, klicken Sie auf eine pro Anwendung Basis. Wenn Regeln **auf** sind sie aktiviert und für Benutzer, die Zugriff auf die Anwendung erzwungen. Wenn sie **aus** sind sie nicht verwendet werden und wirkt sich nicht auf den Benutzer Anmeldevorgang.

## <a name="applying-rules-to-specific-users"></a>Anwenden von Regeln für bestimmte Benutzer
Regeln können auf bestimmte Sätze von Benutzern anhand von Sicherheitsgruppe **Anwenden**festlegen angewendet werden. **Anwenden** können **Alle Benutzer** oder **Gruppen**festgelegt werden. Beim Festlegen auf **Alle Benutzer** die Regeln für alle Benutzer mit Zugriff auf die Anwendung angewendet werden. Die Option **Gruppen** kann bestimmte Sicherheits- und Verteilergruppen ausgewählt werden, werden nur für diesen Gruppen Regeln erzwungen.

Wenn Sie eine Regel bereitstellen zu können, ist es üblich zuerst übernehmen eine begrenzte Benutzer festlegen, die eine Durchführen eines Pilotprojekts Gruppen angehören. Wenn der Vorgang abgeschlossen kann die Regel für **Alle Benutzer**angewendet werden. Dadurch wird die Regel für alle Benutzer in der Organisation erzwungen.

Ausgewählte Gruppen möglicherweise auch von mit der Option **mit Ausnahme der** Richtlinie ausgenommen werden. Auch wenn sie in einer enthaltenen Gruppe angezeigt werden, werden alle Mitglieder dieser Gruppen ausgenommen werden.

## <a name="at-work-networks"></a>"Im Büro" Netzwerke


Bedingte Access-Regeln, die ein Netzwerk "im Büro" verwenden aufsetzen auf vertrauenswürdige IP-Adressbereiche, die in Azure AD konfiguriert wurden oder über die Inanspruchnahme "inside Corpnet" aus dem AD FS verwenden. Gehören die folgenden Regeln:

- Verlangen der kombinierte Authentifizierung nicht am Arbeitsplatz
- Wenn Sie nicht am Arbeitsplatz Zugriff blockieren

Optionen für die Angabe "im Büro" Netzwerke

1. Konfigurieren von vertrauenswürdigen IP-Adressbereiche [kombinierte Authentifizierung Konfigurationsseite](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Bedingte Zugriffsrichtlinie wird die konfigurierten Bereichen auf jedem Authentifizierungsanfrage und token Emission verwenden, um Regeln ausgewertet werden soll. 
2. Konfigurieren der Verwendung von dem inneren Corpnet beanspruchen diese Option mit partnerverbundkontakte Verzeichnisse durchsuchen, die mit AD FS verwendet werden kann. [Erfahren Sie mehr über dem inneren Coronet Ansprüche](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigurieren von öffentlichen IP-Adressbereiche. Klicken Sie auf der Registerkarte konfigurieren können Sie bei Ihrem Verzeichnis öffentliche IP-Adressen festlegen. Bedingte Access diese als "at Work" IP-Adressen verwenden, um auf diese Weise können zusätzliche Bereiche werden konfigurieren, über die Grenze von 50 IP-Adresse, die von der Seite der MFA-Einstellung erzwungen wird.



## <a name="rules-based-on-application-sensitivity"></a>Regeln basierend auf der Anwendung Vertraulichkeit

Regeln werden für jede Anwendung gleicht der Dienste hoher Wert gesichert werden, ohne dass beeinträchtigt Zugriff auf andere Dienste konfiguriert. Klicken Sie auf die Registerkarte **Konfigurieren** der Anwendung können bedingte Access Regeln konfiguriert sein. 

Regeln, die derzeit angeboten:

- **Mehrstufige Authentifizierung erforderlich**
 - Alle Benutzer, denen dieser Richtlinie, um angewendet wird werden zum Authentifizieren über mindestens einmal mehrstufige Authentifizierung erforderlich.
 
- **Verlangen der kombinierte Authentifizierung nicht am Arbeitsplatz**
 - Wenn die Richtlinie angewendet wird, werden alle Benutzer erforderlich, um die kombinierte Authentifizierung mindestens einmal ausgeführt haben, wenn sie den Dienst aus einem nicht Arbeit entfernten Standort zugreifen. Wenn Sie von einer geschäftlichen entfernten Standort verschoben werden, müssen die Benutzer kombinierte Authentifizierung ausführen, wenn Sie auf den Dienst zugreifen.
 
- **Wenn Sie nicht am Arbeitsplatz Zugriff blockieren** 
 - Wenn Benutzer von der Arbeit an einem entfernten Standort verschieben, werden diese blockiert, wenn die Richtlinie "Block Access nicht am Arbeitsplatz" darauf angewendet wird.  Sie können an der Position Arbeit Access erneut zugelassen werden.


## <a name="related-topics"></a>Verwandte Themen

- [Sichern des Zugriffs auf Office 365 und andere apps mit Azure Active Directory verbunden](active-directory-conditional-access.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

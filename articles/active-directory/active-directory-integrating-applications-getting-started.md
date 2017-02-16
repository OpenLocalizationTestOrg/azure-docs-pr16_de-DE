<properties
   pageTitle="Leitfaden für erste Applikationen Azure Active Directory integrieren Schritte |  Microsoft Azure"
   description="Dieser Artikel bietet einen Leitfaden für erste Schritte für die Integration von Azure Active Directory (AD) mit lokalen Anwendungen und Cloud Applications."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Leitfaden für Schritte Azure Active Directory integrieren von Applications erste
## <a name="overview"></a>(Übersicht)
In diesem Thema dient als Wegweiser zu verleihen, für die Integration von Applications mit Azure Active Directory (AD). Jeder der folgenden Abschnitte enthalten eine kurze Übersicht über eine ausführlichere Thema, damit Sie erkennen können, welche Teile von diesem Leitfaden für erste Schritte für Sie relevant sind.  Folgen Sie den Hyperlinks für eingehender zu den einzelnen Themen aus.

## <a name="before-you-begin-take-inventory"></a>Bevor Sie beginnen, Bestandsaufnahme
Bevor Sie in zu integrieren von Applications in Azure AD springen, ist es wichtig zu wissen, wo Sie sich befinden und die Stelle, an der Sie wechseln möchten.  Die folgenden Fragen sollen Ihnen ein Integrationsprojekt Azure AD-Anwendung anzustellen.

### <a name="application-inventory"></a>Anwendung inventory
- Wo befinden sich alle Ihre Programme? Wem diese gehören?
- Welche Art der Authentifizierung benötigen Ihre Programme?
- Wer Zugriff auf die Anwendungsmöglichkeiten benötigt?
- Möchten Sie eine neue Anwendung bereitstellen?
  - Wird erstellen für zu und bereitstellen es in einer Azure berechnen Instanz?
  - Werden Sie eine verwenden, die in der Galerie Azure verfügbar ist?

### <a name="user-and-group-inventory"></a>Benutzer- und inventory
- Wo sich Ihre Benutzerkonten befinden?
 - Lokalen Active Directory
 - Azure AD
 - In einer separaten Anwendungsdatenbank, die Sie besitzen
 - In Clientanwendungen unbestätigter
 - Alle vorstehend genannten
- Welche Berechtigungen und rollenzuweisungen haben einzelner Benutzer derzeit auf? Müssen Sie ihre Access überprüfen oder sind Sie sicher, dass Ihre Benutzer Zugriff und Rolle Zuordnungen jetzt angemessen sind?
- Sind Gruppen bereits in Ihrem lokalen Active Directory eingerichtet?
 - Wie werden die Gruppen angezeigt?
 - Wer sind die Mitglieder der Gruppe?
 - Welche Berechtigungen/rollenzuweisungen haben Gruppen derzeit?
- Müssen Sie vor der Integration von Benutzer/Gruppe-Datenbanken bereinigen?  (Dies ist ein ziemlich wichtige Frage. Datenmüll in Garbage ab).

### <a name="access-management-inventory"></a>Access Management inventory
- Wie verwalten Sie aktuell des Benutzerzugriffs auf Anwendungen? Benötigt die ändern?  Haben Sie andere Methoden zum zugreifen, wie beispielsweise mit [RBAC](role-based-access-control-configure.md) verwalten berücksichtigt?
- Wer auf was zugreifen benötigt?

Vielleicht verfügen Sie nicht über die Antworten auf diese Fragen voraus, aber das ist normal.  Dieses Handbuch hilft Ihnen einige dieser Fragen beantworten und treffen von Entscheidungen auf einige informierten.

## <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Abonnement und eine Azure-Active Directory-Verzeichnisdienst.  Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie für 30 Tage lang kostenlos Azure ausprobieren. [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Anwendungsintegration in Azure AD-
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Suchen nach unbestätigter Cloudanwendungen mit der Cloud-App-Suche
Wie zuvor erwähnt, können Anwendungen, die von Ihrer Organisation bis jetzt verwaltet wurde noch nicht vorhanden sein.  Als Teil der Inventory-Prozess ist es möglich, unbestätigter Cloud zu ermitteln. Finden Sie unter [unbestätigter Cloud-Programme mit der Cloud-App-Suche suchen](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Authentifizierungsarten
Jede Anwendung möglicherweise verschiedene authentifizierungsanforderungen. Mit Azure AD kann Zertifikate bei der Anmeldung bei Programmen, die verwendet werden, die SAML 2.0, WS-Verbund, oder OpenID verbinden Protokolle sowie Kennwort Single Sign On verwenden. Weitere Informationen zur Anwendung finden Sie unter Authentifizierungsarten für die Verwendung mit Azure AD [Verwalten von Zertifikaten für Partnersuche einmaliges Anmelden in Azure Active Directory](active-directory-sso-certs.md) und [Kennwort auf Grundlage einmaliges Anmelden](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Aktivieren von SSO mit Azure AD-App-Proxy
Mit Microsoft Azure AD-Anwendungsproxy können Sie den Zugriff auf Anwendungen befindet sich in Ihrem privaten Netzwerk sicher, überall und auf jedem Gerät bereitstellen. Nachdem Sie einen Anwendung Proxy Verbinder in Ihrer Umgebung installiert haben, können sie problemlos mit Azure AD konfiguriert werden.

### <a name="integrating-applications-with-azure-ad"></a>Integrieren von Applications in Azure AD
Die folgenden Artikeln diskutieren die Verwendungsmöglichkeiten von Applications Azure AD integrieren und bieten einige Richtlinien.

- [Bestimmen der Active Directory verwenden](active-directory-administer.md)
- [Verwenden von Applications im Katalog Azure-Anwendung](active-directory-appssoaccess-whatis.md)
- [Integration von SaaS Applikationen Lernprogramme Liste](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Verwalten des Zugriffs auf Anwendungen
Die folgenden Artikel beschreiben Möglichkeiten, die Sie Zugriff auf Anwendungen verwalten können, nachdem sie mit Azure AD-mit Azure AD-Verbinder und Azure AD-integriert wurden.

- [Verwalten des Zugriffs auf Azure AD-apps](active-directory-managing-access-to-apps.md)
- [Automatisieren mit Azure AD-Verbinder](active-directory-saas-app-provisioning.md)
- [Zuweisen von Benutzern zur Anwendung](active-directory-applications-guiding-developers-assigning-users.md)
- [Zuweisen von Gruppen zur Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)
- [Freigeben von Konten](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrieren von benutzerdefinierten Programmen
Wenn Sie eine neue Anwendung schreiben und Entwickler in durch Nutzung der Funktionen Azure AD unterstützen möchten, finden Sie unter [Guiding Entwickler](active-directory-applications-guiding-developers-for-lob-applications.md).

Wenn Sie Ihre benutzerdefinierte Anwendung in den Katalog der Azure-Anwendung hinzufügen möchten, finden Sie unter ["Zeigen Sie Ihre eigenen app" mit Azure AD Self-Service SAML-Konfiguration](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Siehe auch

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

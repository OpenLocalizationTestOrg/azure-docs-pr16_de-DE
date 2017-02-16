<properties
    pageTitle="Azure-Active Directory häufig gestellte Fragen zu | Microsoft Azure"
    description="Azure Active Directory häufig gestellte Fragen, die Antworten auf Fragen in Verbindung mit dem Zugriff auf Azure und Azure-Active Directory, bietet Kennwort Management und Anwendung-Zugriff."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Azure-Active Directory häufig gestellte Fragen

Azure-Active Directory ist eine umfassende Identität as a Service (IDaaS)-Lösung, die alle Aspekte der Identität, Verwaltung und Sicherheit umfasst.


Weitere Informationen hierzu finden Sie unter [Neuigkeiten Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Zugreifen auf Azure und Azure Active Directory


**F: Warum erhalte ich "keine Abonnements gefunden" beim Versuch, mich Azure AD im Portal Azure klassischen (https://manage.windowsazure.com) zugreifen?**

**A:** Zugreifen auf das Azure klassischen Portal erfordert einzelnen Benutzer Berechtigungen für eine Azure-Abonnement verfügen. Wenn Sie ein bezahltes Office 365 oder Azure AD-, navigieren Sie zu [http://aka.ms/accessAAD](http://aka.ms/accessAAD) für einen Schritt einmalige Aktivierung haben, müssen Sie andernfalls zu einem vollständigen [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/) oder ein kostenpflichtiges Abonnement aktivieren. 

Weitere Informationen hierzu finden Sie unter:

- [Wie Azure-Abonnements Azure Active Directory zugeordnet sind](active-directory-how-subscriptions-associated-directory.md)

- [Verwalten Sie Verzeichnis für Ihr Office 365-Abonnement in Azure](active-directory-manage-o365-subscription.md)

---

**F: Was die Beziehung zwischen Azure AD ist Office 365 und Azure?**

**A:** Azure-Active Directory bietet allgemeine Identität und Access-Funktionen zu allen Microsoft online Services. Ob Sie Office 365, Microsoft Azure, Intune oder andere verwenden, sind Sie bereits eine Azure AD verwenden, um anmelden aktivieren und Management für alle der folgenden Dienste zuzugreifen. 

Tatsächlich werden alle Benutzer, die Sie für den Microsoft Online Services aktiviert haben, als von Benutzerkonten in eine oder mehrere Instanzen von Azure AD-definiert. Sie können diese Konten kostenlos Azure AD-Funktionen wie Anwendung cloudzugriff aktivieren.
 
Darüber hinaus bezahlt Azure AD Services (z. B.: Azure AD-Basic, Premium, EMS, usw.) ergänzen andere Online-Diensten wie Office 365 und Microsoft Azure mit umfassenden Enterprise-Skala Verwaltung und Sicherheit Lösungen.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Erste Schritte mit Azure AD-Hybrid


**F: wie kann ich meinen lokalen Verzeichnis zu Azure AD Verbindung herstellen?**

**A:** Sie können Ihre lokalen Verzeichnis mit Azure AD mit **Azure AD verbinden**herstellen. 

Weitere Informationen hierzu finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).


---

**F: wie kann ich SSO zwischen meinem lokalen Verzeichnis und Meine Programme Cloud einrichten?**

**A:** Sie müssen nur SSO zwischen Ihrem lokalen Verzeichnis und Azure AD-einrichten. Solange Zugriff auf Ihre Cloud Applikationen über Azure AD Laufwerke Dienst automatisch die Benutzer die ordnungsgemäße Authentifizierung über ihre Anmeldeinformationen lokal aus.

Implementierung-SSO aus lokalen kann leicht mit Föderation Lösungen, wie z. B. ADFS oder indem Sie Kennwort Hash synchronisieren erzielt werden. Sie können ganz einfach beide Optionen mithilfe des Assistenten Azure AD verbinden bereitstellen.
  

Weitere Informationen hierzu finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).
  

---

**F: bietet Azure-Active Directory ein Self-service-Portal für Benutzer in meinem Unternehmen?**

**A:** Ja, bietet Azure Active Directory für Benutzer Self-service und den Zugriff auf die Anwendung der [Systemsteuerung Azure AD-Zugriff](http://myapps.microsoft.com) . Wenn Sie ein Office 365-Kunde sind, finden Sie zahlreiche dieselben Funktionen in Office 365-Portal. 

Weitere Informationen finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md). 



---

**F: kann Azure AD ich meinen lokalen Infrastruktur verwalten?**

**A:** Ja, geschieht dies aus. Die Azure AD Premium Edition bietet Ihnen **Gesundheit verbinden**. Azure AD verbinden Gesundheit hilft Ihnen überwachen und Einblick Ihrer lokalen Identitätsinfrastruktur und der Synchronisierungsdienste.  

Weitere Informationen hierzu finden Sie unter [Überwachen Ihrer lokalen Identität Infrastruktur und Synchronisierung Dienste in der Cloud](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Verwaltung der Kennwörter

**F: kann ich mithilfe von Azure AD-Kennwort schreiben-Back ohne Kennwort synchronisieren? (Quickinfos, ich möchte Azure AD SSPR mit Kennwort schreiben-Back verwenden, aber nicht meine Kennwörter in der Cloud? gespeichert werden)**

**A:** Sie müssen nicht für die Synchronisierung Ihrer Kennwörter AD Azure AD-um schreiben-Back aktivieren. In einer verbundenen Umgebung verwendet Azure AD SSO lokalen Verzeichnis zum Authentifizieren des Benutzers. Dieses Szenario erfordert keine lokalen Kennwort zum in Azure AD erfasst werden.

---

**F: dauert wie lange es, für die ein Kennwort wieder AD lokalen geschrieben werden können?**

**A:** Kennwort schreiben-Back arbeitet in Echtzeit. 

Weitere Informationen hierzu finden Sie unter [Erste Schritte mit der Verwaltung der Kennwörter](active-directory-passwords-getting-started.md) 


---

**F: verwenden kann ich Kennwort schreiben-Back durch Kennwörter, die von einem Administrator verwaltet werden?**

**A:** Ja, wenn Sie Kennwort schreiben-Back aktiviert haben, werden die Kennwortvorgänge, die von einem Administrator ausgeführt, wieder in Ihrer lokalen Umgebung geschrieben.  

Weitere Antworten auf Kennwort verwandte Fragen finden Sie unter [Häufig gestellte Fragen zu Kennwort Management](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Zugriff auf die Anwendung


**F: Wo finde ich eine Liste der Anwendungen, die vorab in Azure AD integriert sind und deren Funktionen?**

**A:** Azure AD verfügt über 2600 vorab integrierte Applikationen von Microsoft, Application Service Provider und Partner Alle vordefinierten Spezialisten unterstützen SSO. SSO können Sie Ihre Anmeldeinformationen ein organisationsinterne zu verwenden, um Ihre apps zuzugreifen. Einige der Programme unterstützen auch automatisierte erteilen und entziehen

Eine vollständige Liste der vordefinierten Spezialisten finden Sie unter dem [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**F: Was passiert, wenn die Anwendung, die ich benötige nicht in dem Azure AD-Marketplace ist?**

**A:** Sie können mit Azure AD Premium hinzufügen und konfigurieren Sie alle gewünschten Anwendung. Abhängig von Ihrer Anwendung Funktionen und Ihre Voreinstellungen können Sie SSO und automatisierten Bereitstellung konfigurieren.  

Weitere Informationen hierzu finden Sie unter:

- [Konfigurieren von einmaliges Anmelden auf Anwendungen, die nicht in der Galerie Azure Active Directory sind](active-directory-saas-custom-apps.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Active Directory Azure Applications mithilfe von SCIM](active-directory-scim-provisioning.md) 


---

**F: wie mich Benutzer in Azure Active Directory mithilfe von Applications?**
 
**A:** Azure-Active Directory bietet mehrere Möglichkeiten für Benutzer zum Anzeigen und ihre Anwendungen wie zugreifen:

- Im Bereich der Azure AD-Zugriff

- Das Startprogramm für Office 365-Anwendung

- Direkte melden Sie sich für den Zugriff auf partnerverbundkontakte apps

- Tiefe Links zu partnerverbundkontakte, Kennwort-basierten oder vorhandenen apps

Weitere Informationen finden Sie unter [Bereitstellen von Azure AD integriert Applikationen für Benutzer](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**F: Was sind, dass die verschiedenen Wege Azure Active Directory ermöglicht Authentifizierung und einmaliges Anmelden auf Anwendungen?**
 
**A:** Azure-Active Directory unterstützt viele standardisierte Protokolle für die Authentifizierung und Autorisierung wie SAML 2.0, OpenID verbinden, OAuth 2.0 und WS-Federation. Azure AD unterstützt auch Kennwort Vaulting und automatisierte Funktionen für apps, die nur formularbasierte Authentifizierung unterstützen.  

Weitere Informationen finden Sie unter:

- [Authentifizierungsszenarien Azure AD-](active-directory-authentication-scenarios.md)

- [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Wie einmaliges Anmelden mit Azure Active Directory arbeiten?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**F: hinzufügen kann ich beim Ausführen von lokalen Applikationen?**

**A:** Azure AD-Anwendungsproxy bietet einfachen und sicheren Zugriff auf lokale Webanwendungen, die Sie auswählen. Sie können diesen Anwendungen auf die gleiche Weise zugreifen Ihre apps SaaS in Azure Active Directory den Zugriff auf. Es ist nicht erforderlich für ein VPN oder Ändern der Infrastruktur des Netzwerks ein.  

Weitere Informationen hierzu finden Sie unter [wie sicheren Remotezugriff auf lokale Anwendungen bereitgestellt](active-directory-application-proxy-get-started.md).


--- 

**F: wie erforderlich ich MFA für den Zugriff auf eine bestimmte Anwendung Benutzer?**

**A:** Mit bedingten Azure AD-Zugriff können Sie eine eindeutigen Zugriffsrichtlinie für jede Anwendung zuweisen. Sie können in Ihrer Richtlinie MFA erforderlich, alle Vorkommen, oder wenn der Benutzer nicht mit dem lokalen Netzwerk verbunden sind.  

Weitere Informationen hierzu finden Sie unter [Sichern von Access mit Office 365 oder andere apps mit Azure Active Directory verbunden ist](active-directory-conditional-access.md).


---

**F: Was ist die automatisierte Benutzer bereitgestellt für SaaS Apps?**

**A:** Azure-Active Directory können Sie die Erstellung, Wartung und Entfernen von Benutzeridentitäten in Clientanwendungen für viele beliebte Cloud (SaaS) automatisieren. 

Weitere Informationen finden Sie unter [automatisieren Benutzer bereitgestellt und Deprovisioning SaaS Applications mit Azure Active Directory](active-directory-saas-app-provisioning.md)

---




<properties
    pageTitle="Einzelne anmelden Management für Enterprise-apps in der Vorschau Azure Active Directory | Microsoft Azure"
    description="Erfahren Sie, wie einmaliges für Enterprise-apps mit Azure Active Directory verwaltet"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Vorschau: Verwalten von einmaliges Anmelden für Enterprise-apps im neuen Azure-Portal

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassischen-portal](active-directory-sso-integrate-saas-apps.md)

Dieser Artikel beschreibt, wie Sie die [Azure-Portal](https://portal.azure.com) verwenden, um die einzelne anmelden Einstellungen für Applikationen, insbesondere verwalten, die aus dem [Katalog von Azure Active Directory (Azure AD) Anwendung](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)hinzugefügt wurden. Die Azure AD-Verwaltungsoption für einmaliges Anmelden ist derzeit in public Preview-Version, und in diesem Artikel werden die neuen Features sowie einige temporäre Einschränkungen, die nur während der Preview-Zeitraum angeordnet werden. [Was ist in der Vorschau?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Suchen Ihre apps im neuen Portal

Ab September 2016 können alle Programme, die für einzelne konfiguriert wurden im Verzeichnis, von einem Directory-Administrator mit Hilfe der [Azure-Active Directory-Anwendung Katalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) innerhalb der [Azure klassischen Portal](https://manage.windowsazure.com)anmelden, jetzt angezeigt und verwaltet werden im Azure-Portal.

Diese Applications im Abschnitt **Enterprise Applications** des Portals Azure, finden Sie ein die Link, der in der Liste **Weitere Dienste** im [Portal](https://portal.azure.com)gefunden werden kann. Enterprise-apps sind apps, haben bereitgestellt wurde, die von Benutzern in Ihrer Organisation verwendet werden.

![Enterprise-Applikationen blade][1]

Wählen Sie **Alle Programme** , um eine Liste alle Apps anzuzeigen, die konfiguriert wurden einschließlich apps, die aus dem Katalog hinzugefügt wurde, ein. Auswählen einer app lädt das Blade Ressourcen für die app, wo Berichte angezeigt werden können, für die app und eine Vielzahl von Einstellungen verwaltet werden kann.

Wählen Sie zum Verwalten von Einstellungen für einzelner Zeichen **einmaliges Anmelden**.

![Anwendung Ressource blade][2]


##<a name="single-sign-on-modes"></a>Modi für einzelne Zeichen

Das Blade **einmaliges Anmelden** beginnt mit einem Menü **Modus** , wodurch den einzelnen anmelden Modus konfiguriert sein. Die verfügbaren Optionen umfassen:

* **SAML-basierten, melden Sie sich auf** – diese Option ist verfügbar, wenn die Anwendung vollständigen partnerverbundkontakte einmaliges Anmelden mit Azure Active Directory mithilfe des SAML 2.0-Protokolls unterstützt.

* **Kennwort-basierten, melden Sie sich auf** – diese Option steht zur Verfügung Azure AD Kennwort Formular für diese Anwendung ausfüllen unterstützen.

* **Verknüpfte anmelden** - früher bekannt als ermöglicht "Vorhandene einmaliges Anmelden", diese Option Administratoren des Benutzers Azure Active Directory Access Systemsteuerung oder Office 365-Anwendung Startprogramm für einen Link zu dieser Anwendung versehen.

Weitere Informationen über diese Modi finden Sie unter [wie einmaliges Anmelden mit Azure Active Directory Arbeit](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>SAML-basierten, melden Sie sich auf

Die Option **SAML-basierten, melden Sie sich auf** zeigt eine Blade, die in vier Abschnitte unterteilt ist:

###<a name="domains-and-urls"></a>Domänen und URLs
Dies ist, in der alle Details der Anwendungs Domäne und URLs zu Ihrem Verzeichnis Azure AD-hinzugefügt werden. Während alle optionale Eingaben angezeigt werden können, indem Sie das Kontrollkästchen **Erweiterte URL-Einstellungen anzeigen** auswählen, werden alle Eingaben erforderlich, damit der einzelnen anmelden Arbeit app direkt auf dem Bildschirm angezeigt. Die vollständige Liste der unterstützten Eingaben umfasst:

* **Melden Sie sich auf URL** – Stelle, an der der Benutzer zum Anmelden bei dieser Anwendung wechselt. Wenn die Anwendung ausführen Service Provider initiiert einmaliges auf, dann Wenn ein Benutzer zu dieser URL navigiert so konfiguriert ist, hat der Dienstanbieter die notwendigen Umleitung zu Azure AD zum Authentifizieren und den Benutzer anmelden. Wenn dieses Feld gefüllt wird, wird dann Azure AD diese URL verwendet zum Starten der Anwendung von Office 365 und im Bereich Azure AD-Zugriff. Wenn dieses Feld nicht angegeben wird, und klicken Sie dann Azure AD stattdessen Identitätsanbieter führt-initiiert melden Sie sich auf, wenn die app von Office 365, klicken Sie im Bereich Azure Active Directory Access oder aus Azure AD-gestartet wird einzelner anmelden URL.

* **Bezeichner** – diese URI sollte eindeutig identifizieren Sie die Anwendung für welche einzelnes anmelden konfiguriert ist. Dies ist der Wert, den Azure AD wieder zur Anwendung als Parameter Zielgruppe von SAML-Token sendet, und die Anwendung wird erwartet, um dies zu überprüfen. Dieser Wert wird auch als der Element-ID im SAML Metadaten, die von der Anwendung bereitgestellt.

* **Antwort-URL** - die Antwort-URL ist, an dem die Anwendung das SAML-Token zu erhalten. Dies wird auch als URL Assertion Consumer Service (ACS) bezeichnet. Nachdem Sie diese eingegeben haben, klicken Sie auf Weiter, um zum nächsten Fenster wechseln. Dieser Bildschirm enthält Informationen, was konfiguriert sein, auf der Anwendungsseite zu aktivieren, um ein SAML-Token aus Azure AD-zu übernehmen muss.

* **Bundesstaat Relay** - im Relay Zustand ist ein optionaler Parameter, der der Anwendung mitteilen, wo Sie den Benutzer umleiten nach Abschluss der Authentifizierung helfen können. Normalerweise entspricht der Wert eine gültige URL bei der Anwendung, jedoch einige Applikationen anders dieses Feld verwenden (Siehe des app einmaliges Klicken Sie auf Weitere Einzelheiten). Die Möglichkeit zum Festlegen des Relay Zustands ist ein neues Feature, das mit dem neuen Azure-Portal eindeutig ist.

###<a name="user-attributes"></a>Benutzerattributen
Dies ist die Stelle, an der Administratoren anzeigen und Bearbeiten von den Attributen, die im SAML-Token, die Probleme Azure AD an die Anwendung gesendet werden, dass Benutzer Mal anmelden können.

Für das erste Release Preview ist das nur bearbeitbare Attribut unterstützt das Attribut für die **Benutzer-ID** an. Der Wert der dieses Attribut ist das Feld in Azure AD, die eindeutig für jeden Benutzer in der Anwendung. Angenommen, wenn die app mit "e-Mail-Adresse" als Benutzernamen und eindeutiger Bezeichner bereitgestellt wurde, würde dann der Wert in das Feld "user.mail" in Azure AD festgelegt werden.

Bearbeiten von zusätzlichen Attributen wird in einer Vorschau nachfolgende unterstützt.

###<a name="saml-signing-certificate"></a>Signaturzertifikat SAML
Dieser Abschnitt listet die Details des Zertifikats, das Azure AD die SAML-Token anmelden, die mit der Anwendung ausgestellt werden jedes Mal des Benutzers Authentifizierung verwendet. Sie können die Eigenschaften des aktuellen Zertifikats geprüft werden müssen, einschließlich des Ablaufdatums.

Die Möglichkeit, Rollover das Zertifikat und Verwalten von zusätzlichen Zertifikat Optionen werden in einem nachfolgenden Preview-Version unterstützt werden. Beachten Sie, dass vollständige Verwaltung von Zertifikaten im [Azure klassischen Portal](active-directory-sso-certs.md)weiterhin ausgeführt werden kann.

###<a name="application-configuration"></a>Anwendungskonfiguration
Der letzte Abschnitt enthält die Dokumentation und/oder Steuerelemente zum Konfigurieren der Anwendung mit Azure Active Directory als Identitätsanbieter erforderlich.

Klicken Sie im Menü der **Anwendung konfigurieren** Flyout bietet neue präzise, eingebettete Anweisungen zum Konfigurieren der Anwendungs. Dies ist ein weiteres neues Feature in das neue Azure-Portal eindeutig.

> [AZURE.NOTE] Ein vollständiges Beispiel der eingebetteten Dokumentation finden Sie unter die Anwendung Salesforce.com. Dokumentation für weitere apps ist während der Vorschau fortlaufend hinzugefügt werden.

![Eingebettete Dokumente][3]

##<a name="password-based-sign-on"></a>Kennwort-basierten, melden Sie sich auf
Wenn für die Anwendung unterstützt, konfiguriert indem Sie das Kennwort-basierten SSO-Modus auswählen und **Speichern** sofort so, kann von Kennwörtern basierende SSO. Weitere Informationen zum Bereitstellen von Kennwörtern basierende SSO, finden Sie unter [wie einmaliges Anmelden mit Azure Active Directory Arbeit](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Kennwort-basierten, melden Sie sich auf][4]


##<a name="linked-sign-on"></a>Klicken Sie auf Verknüpfte anmelden
Wenn für die Anwendung nicht unterstützt, kann die Auswahl des verknüpften SSO-Modus Sie die URL eingeben, der in umgeleitet werden, wenn Benutzer diese App klicken Sie auf Systemsteuerung Azure Active Directory Access oder Office 365 werden soll. Weitere Informationen zu verknüpften SSO (früher als "vorhandene SSO" bezeichnet), finden Sie unter [wie einmaliges Anmelden mit Azure Active Directory Arbeit](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Verknüpfte für einmaliges Anmelden][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG

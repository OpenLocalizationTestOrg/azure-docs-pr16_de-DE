<properties 
    pageTitle="Einmaliges Anmelden auf Anwendungen, die nicht in der Galerie Azure Active Directory sind konfigurieren | Microsoft Azure" 
    description="Erfahren Sie, wie mit Self-Service verbinden Azure Active Directory mithilfe von SAML und Kennwörtern basierende SSO-apps" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Konfigurieren von einmaliges Anmelden auf Anwendungen, die nicht in der Galerie Azure Active Directory sind

In diesem Artikel wird über ein Feature, mit dem Administratoren für einmaliges Anmelden auf Anwendungen, die nicht vorhanden ist, in der Azure-Active Directory-app-Katalog *ohne Schreiben von Code*konfigurieren kann. Dieses Feature wurde veröffentlicht von technical Preview unter 18. November 2015 und in [Azure Active Directory Premium](active-directory-editions.md)enthalten ist. Wenn Sie stattdessen Entwicklertools Anleitungen zum Integrieren von benutzerdefinierten apps mit Azure AD über Code suchen, finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).

Der Azure-Active Directory-Anwendung-Katalog bietet eine Auflistung von Applications, die bekanntermaßen ein Formulars für einmaliges Anmelden mit Azure Active Directory, unterstützen, wie in [diesem Artikel](active-directory-appssoaccess-whatis.md)beschrieben. Wenn Sie (als eine IT-Spezialisten oder Systemintegrator in Ihrer Organisation) die Anwendung gefunden haben, eine Verbindung herstellen möchten, Sie können erste Schritte mit folgen Sie die schrittweisen Anweisungen im Azure-Verwaltungsportal präsentiert einmaliges Anmelden aktivieren.

Kunden mit [Azure Active Directory Premium](active-directory-editions.md) Lizenzen erhalten auch diese zusätzlichen Funktionen:

* Self-service-Integration jeder Anwendung, die die SAML 2.0-Identitätsanbieter (SP initiiert oder IdP initiiert) unterstützt
* Self-service-Integration für jede Web-Anwendung, die eine HTML-basierte Anmeldeseite über [SSO Kennwörtern basierende](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) enthält
* Self-service-Verbindung, verwenden das Protokoll SCIM für Benutzer provisioning ([Alle hier beschriebenen](active-directory-scim-provisioning.md))
* Möglichkeit zum Hinzufügen von Links auf das [Startprogramm für ein Office 365-app](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder der [Azure AD-Access Systemsteuerung](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) zu einer Anwendung

Dies kann nicht nur SaaS-Anwendungen, die Sie verwenden, aber haben nicht noch wurde auf-umgeben in der Galerie Azure AD-, sondern Webanwendungen von Drittanbietern, die Ihrer Organisation auf Servern bereitgestellt hat, die Sie, entweder in der Cloud oder eine lokale steuern umfassen.

Diese Funktionen, auch bekannt als *app-Integration Vorlagen*bieten standardisierten Verbindungspunkte für apps, die Unterstützung von SAML, SCIM oder formularbasierte Authentifizierung, und flexible Optionen und Einstellungen für die Kompatibilität mit eine umfassende Reihe von Programmen enthalten. 

##<a name="adding-an-unlisted-application"></a>Hinzufügen einer aufgelisteten Anwendungs

Um eine Anwendung mithilfe einer Vorlage für Integration-app eine Verbindung herzustellen, melden Sie sich bei der Azure Verwaltungsportal mit Ihrer Azure Active Directory-Administratorkonto, und navigieren Sie zu der **Active Directory > [Verzeichnis] > Applications** Abschnitt, und wählen Sie **Hinzufügen**, und klicken Sie dann **eine Anwendung aus dem Katalog hinzufügen**. 

![][1]

In der app-Katalog können Sie mit der Kategorie **Benutzerdefiniert** angezeigt, auf der linken Seite, oder markieren den Link zum **Hinzufügen einer aufgelistete Anwendung** , der in den Suchergebnissen angezeigt wird, wenn die gewünschte app nicht gefunden wurde aufgelistete app hinzufügen. Nachdem Sie einen Namen für die Anwendung eingegeben haben, können Sie die einzelnen Optionen anmelden und das Verhalten konfigurieren. 

**Tipp**: Verwenden Sie die Suchfunktion als bewährte Methode, überprüfen, um festzustellen, ob die Anwendung bereits in der Galerie vorhanden ist. Wenn die app gefunden wird und deren Beschreibung "einmaliges Anmelden", und klicken Sie dann auf die Anwendung erwähnungen wird bereits partnerverbundkontakte einmaliges Anmelden unterstützt. 

![][2]

Hinzufügen einer Anwendung auf diese Weise bietet ähnelt dem nach dem Vorkommen für integrierte Applikationen zur Verfügung. Um zu beginnen, wählen Sie **Konfigurieren einmaliges Anmelden**. Auf dem nächste Bildschirm bietet die folgenden drei Optionen zum Konfigurieren der einzelner Zeichen, die in den folgenden Abschnitten beschrieben werden.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD einmaliges Anmelden

Wählen Sie diese Option zum Konfigurieren der SAML-basierten Authentifizierung für die Anwendung. Dies ist erforderlich, dass die Anwendung Unterstützung SAML 2.0, und Sie Informationen zum Verwenden der SAML-Funktionen der Anwendung, bevor Sie fortfahren gesammelt werden sollen. Nachdem Sie auf **Weiter**, werden Sie aufgefordert, geben Sie drei verschiedene URLs, die Endpunkte SAML für die Anwendung entspricht. 

![][4]
 
Dies sind:

* **Melden Sie sich auf URL (SP initiiert nur)** –, in der Benutzer zum Anmelden bei dieser Anwendung wechselt. Wenn die Anwendung ausführen Service Provider initiiert einmaliges auf, dann Wenn ein Benutzer zu dieser URL navigiert so konfiguriert ist, kann der Dienstanbieter die notwendigen Umleitung zu Azure AD zum Authentifizieren, und melden Sie sich bei der Benutzer. Wenn dieses Feld gefüllt wird, wird dann Azure AD diese URL verwendet zum Starten der Anwendung von Office 365 und im Bereich Azure AD-Zugriff. Dieses Feld ist Flags, und Azure AD führt stattdessen Identitätsanbieter-initiiert melden Sie sich auf, wenn die app von Office 365, klicken Sie im Bereich Azure Active Directory Access oder aus Azure AD-gestartet wird einzelner anmelden URL (auf der Registerkarte Dashboard copiable).

* **Herausgeber URL** - den Herausgeber, die URL die Anwendung, für welche einmaliges eindeutig auf identifizieren sollte wird konfiguriert. Dies ist der Wert, den Azure AD wieder zur Anwendung als Parameter **Zielgruppe** von SAML-Token sendet, und die Anwendung wird erwartet, um dies zu überprüfen. Dieser Wert wird auch als **Entität-ID** in SAML Metadaten, die von der Anwendung bereitgestellt. Überprüfen der Anwendung SAML-Dokumentation Details auf was es handelt sich Entität ID oder Zielgruppe Wert ist. Es folgt ein Beispiel für die Zielgruppe URL wie im SAML-Token an die Anwendung zurückgegeben wird:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Antwort-URL** - die Antwort-URL ist, an dem die Anwendung das SAML-Token zu erhalten. Dies wird auch als **Assertion Consumer Service (ACS) URL**bezeichnet. Überprüfen der Anwendung SAML-Dokumentation Details auf was deren token SAML-Antworten oder ACS-URL ist.
 Nachdem Sie diese eingegeben haben, klicken Sie auf **Weiter** , um zu zum nächsten Fenster wechseln. Dieser Bildschirm enthält Informationen, was konfiguriert sein, auf der Anwendungsseite zu aktivieren, um ein SAML-Token aus Azure AD-zu übernehmen muss. 

![][5]

Welche Werte erforderlich sind wird variieren je nach der Anwendung, daher sollten Sie die Dokumentation der Anwendung SAML Details. **Anmelden** und **Abmelden** Dienst-URL, die beide in dieselbe IP-Adresse, also den SAML-Endpunkt Request-Verarbeitung für Ihre Instanz von Azure AD aufgelöst werden. Die URL des Herausgebers ist der Wert, der als die "Herausgeber" innerhalb der SAML-Token ausgestellt für die Anwendung angezeigt wird. 

Nach Ihrer Anwendung konfiguriert wurde, klicken Sie auf " **Weiter** ", und klicken Sie dann die **abgeschlossen** , um das Dialogfeld zu schließen. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Zuweisen von Benutzern und Gruppen an Ihrer Anwendung SAML 

Nachdem Sie die Anwendung für die Verwendung von Azure AD als ein SAML-basierten Identitätsanbieter konfiguriert wurde, ist es fast bereit, um zu testen. Als ein Steuerelement Sicherheit wird Azure AD nicht Emission ein Token, sodass sie sich in der Anwendung, es sei denn, sie Azure AD-Zugriff gewährt wurde. Benutzer können Zugriff gewährt werden, direkt oder durch eine Gruppe, der deren Mitglied Sie sind. 

Um einen Benutzer oder eine Gruppe an Ihrer Anwendung zuweisen möchten, klicken Sie auf die Schaltfläche **Benutzer zuweisen** . Wählen Sie aus dem Benutzer oder der Gruppe, die Sie zuweisen möchten, und wählen Sie dann auf die Schaltfläche **zuweisen** . 

![][6]

Zuweisen eines Benutzers Azure AD zu einer auszustellen für den Benutzer als auch bewirken, dass eine Kachel für diese Anwendung in Access-Bereich des Benutzers angezeigt werden können. Die Kachel der Anwendung wird auch in das Startprogramm für Office 365-Anwendung angezeigt, wenn der Benutzer Office 365 verwendet wird. 

Sie können ein Logo Kachel der Anwendung verwenden die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** , für die Anwendung hochladen. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>Anpassen der Ansprüche im SAML-Token ausgestellt 

Wenn ein Benutzer mit der Anwendung authentifiziert, wird Azure AD Emission ein SAML-Token bei der app, die Informationen (oder Ansprüche) enthält über den Benutzer, die sie identifiziert. Standardmäßig umfasst dies Benutzername, e-Mail-Adresse, Vornamen und Nachnamen des Benutzers ein. 

Sie können anzeigen oder Bearbeiten der Ansprüche im SAML-Token mit der Anwendung klicken Sie auf der Registerkarte **Attribute** gesendet. 

![][7]

Es gibt zwei mögliche Gründe, warum Sie möglicherweise die Ansprüche im SAML-Token ausgestellt bearbeiten müssen: Anwendung heraufgeladen werden in eine andere Gruppe von anfordern URIs erfordern geschrieben wurden oder anfordern Werte auf eine Weise, die die NameIdentifier erforderlich sind • Ihre Anwendung bereitgestellt wurde beanspruchen zu einem anderen Wert als den Benutzernamen (Alias Benutzerprinzipalnamen) in Azure Active Directory gespeichert werden. 

Informationen zum Hinzufügen und Bearbeiten von Ansprüchen für diese Szenarios schauen Sie sich in diesem [Artikel Ansprüche anpassen](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testen der SAML-Anwendung 

Nachdem der SAML-URLs und das Zertifikat in Azure AD- und in der Anwendung konfiguriert wurden, Benutzern oder Gruppen zugewiesen wurden, die die Anwendung in Azure, und die Ansprüche überprüft und bei Bedarf bearbeitet wurden, wird des Benutzers bereit sind, melden Sie sich bei der Anwendung. 

So testen Sie anmelden einfach Azure AD Zugriff auf Systemsteuerung am https://myapps.microsoft.com mit einem Benutzerkonto, die Sie mit der Anwendung zugewiesen ist, und klicken Sie dann auf die Kachel der Anwendung zum Deaktivieren der einzelnen anmelden Prozess starten eines. Alternativ können Sie direkt auf die anmelden-URL für die Anwendung und Anmeldung von dort aus durchsuchen. 

Finden Sie für das Debuggen Tipps im [Artikel zum Debuggen SAML-basierten einmaliges Anmelden auf Anwendungen](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Kennwort einmaliges Anmelden

Wählen Sie diese Option, um das [Kennwort-basierten einmaliges Anmelden](active-directory-appssoaccess-whatis.md) für eine Webanwendung konfigurieren, die eine HTML-Anmeldeseite enthält. Kennwort-basierten SSO auch bezeichnet als Kennwort vaulting, können Sie zum Verwalten des Benutzerzugriffs und Kennwörter Webanwendungen, die Identität Föderation nicht unterstützen. Es empfiehlt sich auch für Szenarien, in denen mehrere Benutzer ein einzelnes Konto, beispielsweise in sozialen Medien-app-Konten Ihrer Organisation freigeben müssen. 

Nachdem Sie auf **Weiter**, werden Sie aufgefordert, geben Sie die URL der Anwendung webbasierten-Anmeldeseite. Beachten Sie, dass dies der Seite sein muss, die die Eingabefelder Benutzername und Kennwort enthält. Nachdem eingegeben haben, beginnt Azure AD ein Prozesses zum Analysieren der Anmeldeseite für einen Eingabemethoden Benutzernamen und ein Kennwort einzugeben. Wenn der Vorgang nicht erfolgreich ist, führt Sie dann es durch eine alternative Installation einer Browsererweiterung (erfordert Internet Explorer, Chrome oder Firefox), mit der Sie die Felder manuell zu erfassen kann.

Nachdem die Anmeldeseite aufgezeichnet wird, können Benutzer und Gruppen zugewiesen werden, und Anmeldeinformationen Richtlinien können festgelegt werden, genau wie normale [Kennwort SSO-apps](active-directory-appssoaccess-whatis.md).

Hinweis: Sie können ein Logo Kachel der Anwendung verwenden die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** , für die Anwendung hochladen. 

##<a name="existing-single-sign-on"></a>Vorhandene einmaliges Anmelden

Wählen Sie diese Option, um einen Link zur Anwendung Ihrer Organisation Azure Active Directory Access Systemsteuerung oder Office 365-Portal hinzufügen. Dies können Sie benutzerdefinierte Web apps Links hinzufügen, die aktuell Azure Active Directory Federation Services (oder einem anderen Dienst Föderation) verwenden statt Azure AD für die Authentifizierung. Oder Sie können Tiefe Links hinzufügen, um bestimmte SharePoint-Seiten oder andere Webseiten, die nur auf Access-Bereiche des Benutzers angezeigt werden soll. 

Nachdem Sie auf **Weiter**, werden Sie aufgefordert, geben Sie die URL der Anwendung, mit der eine Verknüpfung. Nachdem abgeschlossen ist, können Benutzer und Gruppen mit der Anwendung, zugewiesen werden, die wodurch die Anwendung angezeigt werden in der [Office 365-app Startprogramm für das](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder die [Access-Systemsteuerung Azure AD-](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) für diesen Benutzer.

Hinweis: Sie können ein Logo Kachel der Anwendung verwenden die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** , für die Anwendung hochladen.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Wie Sie in der SAML-Token für integrierte Apps ausgestellt Ansprüche anpassen](active-directory-saml-claims-customization.md)
- [Behandeln von SAML-basierten einmaliges Anmelden](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png

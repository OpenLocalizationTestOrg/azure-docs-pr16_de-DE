<properties
    pageTitle="Anpassen von Ansprüchen im SAML-Token für integrierte apps in Azure Active Directory ausgestellt | Microsoft Azure"
    description="Erfahren Sie, wie benutzerdefinierte Claims im SAML-Token für integrierte apps in Azure Active Directory ausgestellt"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="asmalser"/>

#<a name="customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>Ausgestellt in der SAML-Token für integrierte apps in Azure Active Directory Ansprüche anpassen

Heute unterstützt Azure Active Directory Tausende von Applications vorab integrierte im Azure AD-Anwendung Katalog, einschließlich über 150, die einmaliges Anmelden mit dem SAML 2.0-Protokoll zu unterstützen. Wenn Benutzerauthentifizierung zur Anwendung bis Azure Active Directory mithilfe von SAML sendet Azure AD ein Token mit der Anwendung (über eine Umleitung HTTP 302), die die Anwendung dann überprüft und verwendet, um die Anmeldung des Benutzers in einen Benutzernamen und Kennwort auffordern, sondern ein. Diese SAML-Token enthält Informationen über den Benutzer als "Ansprüche" bezeichnet.

In der Identität sprechen, ein "Anspruch" Informationen, die als Identitätsanbieter besagt eines Benutzers in das Token, das sie für diesen Benutzer ausgeben. In einem [SAML-Token](http://en.wikipedia.org/wiki/SAML_2.0)diese Daten in der Regel in der SAML-Attribut-Anweisung enthalten ist, und einmalige Nr. des Benutzers wird in der Regel in den Betreff SAML dargestellt.

Standardmäßig wird Azure AD ein SAML-Token an Ihrer Anwendung Emission, die eine NameIdentifier anfordern, mit dem Wert der Benutzername des Benutzers in Azure AD-enthält (dieser Wert gibt den Benutzer eindeutig). Das SAML-Token enthält auch zusätzliche Ansprüche, enthält die e-Mail-Adresse des Benutzers, Vornamen und Nachnamen.

Zum Anzeigen oder Bearbeiten der Ansprüche im SAML-Token zur Anwendung ausgestellt, öffnen Sie den Eintrag der Anwendung im Azure-Verwaltungsportal zu, und wählen Sie auf der Registerkarte **Attribute** unterhalb der Anwendung.

![][1]

Es gibt zwei mögliche Gründe, warum Sie möglicherweise die Ansprüche im SAML-Token ausgestellt bearbeiten müssen: Anwendung heraufgeladen werden in eine andere Gruppe von anfordern URIs erfordern geschrieben wurden oder anfordern Werte • Ihre Anwendung bereitgestellt wurde, auf eine Weise, die die NameIdentifier erfordert beanspruchen zu einem anderen Wert als den Benutzernamen (Alias Benutzerprinzipalnamen) in Azure Active Directory gespeichert werden. 

Sie können eine der Standardwerte anfordern, indem Sie das Symbol mit Bleistift aussieht, das auf der rechten Seite angezeigt wird, wenn Sie die Maus über eine der Zeilen in der Tabelle der SAML-token Attribute auswählen bearbeiten. Sie können Ansprüche (ohne NameIdentifier) verwenden das **X** -Symbol entfernen, und fügen Sie neue Ansprüche über die Schaltfläche **Hinzufügen Benutzerattribut** hinzu.

##<a name="editing-the-nameidentifier-claim"></a>Bearbeiten der NameIdentifier anfordern

Um das Problem zu lösen, die Anwendung wurde, bereitgestellt, verwenden einen anderen Benutzernamen, und klicken Sie auf das Bleistiftsymbol neben der NameIdentifier anfordern. Dies bietet ein Dialogfeld mit mehreren verschiedenen Optionen aus:

![][2]

Wählen Sie im Menü **Attributwert** **user.mail** zum Festlegen des NameIdentifier Anspruchs-e-Mail-Adresse des Benutzers im Verzeichnis werden, oder wählen **user.onpremisessamaccountname** , auf den Namen des Benutzers SAM Konto festzulegen, die lokal Azure Active Directory synchronisiert wurde. 

Sie können auch die spezielle ExtractMailPrefix()-Funktion verwenden, so entfernen Sie entweder die e-Mail-Adresse oder den Benutzerprinzipalnamen resultierender nur der erste Teil des Benutzers benennen über übergebenen Suffix der Domäne (z. B. "Joesmith" statt joesmith@contoso.com).

![][3]

##<a name="adding-claims"></a>Hinzufügen von Ansprüchen

Beim Hinzufügen einer neuen anfordern, können Sie angeben, das Attributname (müssen Sie grundsätzlich nicht so folgen Sie einem Muster URI gemäß der SAML-Spezifikation) sowie festlegen, die der Wert für alle Benutzer Attribut, d. h. im Verzeichnis gespeichert.

![][4]

Angenommen, Sie möchten die Abteilung, die der Benutzer gehört in ihrer Organisation als Anspruch senden (z. B. Sales), und klicken Sie dann Sie eingeben können, welchen Wert beanspruchen erwartet wird, indem Sie die Anwendung, und wählen Sie dann **user.department** als Wert.

Wenn für einen bestimmten Benutzer kein Wert für ein ausgewähltes Attribut gespeichert ist, werden dann Anspruch mit nicht ausgestellt im Token.

**Hinweis:** Die **user.onpremisesecurityidentifier** und **user.onpremisesamaccountname** werden nur unterstützt, bei der Synchronisierung von Benutzerdaten aus mit lokalen Active Directory mit dem [Tool Azure AD verbinden](active-directory-aadconnect.md).

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Konfigurieren von einmaliges Anmelden auf Anwendungen, die nicht in der Galerie Azure Active Directory sind](active-directory-saas-custom-apps.md)
- [Behandeln von SAML-basierten einmaliges Anmelden](active-directory-saml-debugging.md)
    
<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/claimscustomization1.png
[2]: ./media/active-directory-saml-claims-customization/claimscustomization2.png
[3]: ./media/active-directory-saml-claims-customization/claimscustomization3.png
[4]: ./media/active-directory-saml-claims-customization/claimscustomization4.png

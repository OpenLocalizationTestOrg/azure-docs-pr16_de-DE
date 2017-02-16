<properties
    pageTitle="Kontobereitstellung Benachrichtigungen | Microsoft Azure"
    description="Erfahren Sie, wie Sie sicherstellen, dass Sie von Problemen im Zusammenhang mit Benutzer bereitgestellt, die Ihre Aufmerksamkeit erfordern benachrichtigt werden, indem kontobereitstellung Benachrichtigungen aktivieren."
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
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Bereitstellung von Benachrichtigungen zu berücksichtigen

Für Benutzer bereitgestellt werden, können Sie die Verfahren zum Verwalten von Benutzern in Drittanbieter SaaS Clientanwendungen automatisieren. <br>
Während Sie sich einen automatisierten Prozess handelt, ist die Interaktion mit dieser Vorgang von Zeit zu Zeit erforderlich. <br>
Dies ist beispielsweise der Fall, wenn das Kennwort für das Konto ein, dass Sie konfiguriert haben, um den Austausch von Daten mit einem dritten party SaaS Anwendung abgelaufen ist. 

Durch das Aktivieren von Benachrichtigungen für kontobereitstellung, können Sie sicherstellen, dass Sie von Problemen im Zusammenhang mit Benutzer bereitgestellt, die Ihre Aufmerksamkeit erfordern benachrichtigt werden.

Aktivieren oder Deaktivieren von Benachrichtigungen als Teil Ihrer Benutzer bereitgestellt Konfiguration für einen dritten SaaS Anwendung kontobereitstellung.

![Benutzer bereitgestellt][1] 



Kontobereitstellung Benachrichtigungen aktivieren, aktivieren Sie das zugehörige Kontrollkästchen auf **der Bestätigungsseite für das Dialogfeld** , und geben Sie den e-Mail-Alias des Empfängers.

![Bereitstellung von Benachrichtigungen zu berücksichtigen][2]
 


Sie können eine Verteilerliste als Empfänger eingeben. Es ist jedoch zu beachten, dass die e-Mail-Benachrichtigung Links zu Berichten enthält, die nur von den Administratoren Azure AD-zugegriffen werden kann.

Wenn Sie kontobereitstellung Benachrichtigungen aktiviert haben, erhalten Sie e-Mails zu schwerwiegenden Problemen, die sich auf Benutzer bereitgestellt werden. Um eine e-Mail zu vermeiden, erhalten Sie nur eine e-Mail-Benachrichtigung pro Tag für jede Anwendung SaaS jedoch, die, denen für die e-Mail-Benachrichtigung aktiviert ist.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Automatisieren von Benutzer Bereitstellung/aufheben zu SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Anpassen von Attribut Zuordnungen für Benutzer bereitgestellt](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsdefinition Filter für Benutzer bereitgestellt](active-directory-saas-scoping-filters.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Active Directory Azure Applications mithilfe von SCIM](active-directory-scim-provisioning.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
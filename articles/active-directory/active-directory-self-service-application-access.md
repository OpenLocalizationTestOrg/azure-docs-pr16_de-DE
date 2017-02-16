<properties
    pageTitle="Self-service-Anwendungszugriff und delegierte Verwaltung mit Azure Active Directory | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Sie Zugriff auf die Self-service-Anwendung und delegierte Verwaltung mit Azure Active Directory aktivieren."
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
    ms.date="02/09/2016"
    ms.author="asmalser"/>

#<a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Self-service-Anwendungszugriff und delegierte Verwaltung mit Azure Active Directory

Aktivieren von Self-service-Funktionen für Endbenutzer ist ein gängiges Szenario für Enterprise IT. Viele Benutzer, zahlreiche Applikationen und die Person, die in Access erteilen Entscheidungen treffen best-informed ist möglicherweise nicht die Directory-Administrator. Häufig ist die beste Person entscheiden können, wer eine Anwendung zugreifen kann ein Teamleiter oder ein anderer delegierten Administrator. Aber am Ende des Tages, es ist für den Benutzer, der die app verwendet, und der Benutzer weiß, was sie müssen ihre Arbeit machen können.

Self-service-Anwendungszugriff ist ein Feature von [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) , mit denen Directory-Administratoren können:

* Aktivieren Sie Benutzern Zugriff auf Applikationen mithilfe einer Kachel "Erhalten weitere Applications" im [Access-Systemsteuerung Azure AD-](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) anfordern
* Festlegen, welche Benutzer Applikationen Zugriff auf anfordern können
* Festlegen Sie, ob eine Genehmigung für den Zugriff auf eine Anwendung sich selbst zuordnen können Benutzer erforderlich ist
* Festlegen, wer sollte die Anfragen genehmigen und Verwalten des Zugriffs für jede Anwendung

Heute wird diese Funktion für alle vordefinierten integrierte und benutzerdefinierte apps unterstützt, partnerverbundkontakte oder Kennwort basierenden einmaliges Anmelden im [Katalog der Azure-Active Directory-Anwendung](https://azure.microsoft.com/marketplace/active-directory/all/), einschließlich apps wie Vertrieb, Dropbox, Google Apps und zu unterstützen.
Dieser Artikel beschreibt, wie Sie:

* Konfigurieren von Self-service-Anwendungszugriff für Endbenutzer einschließlich Konfigurieren eines optionalen Genehmigungsworkflows 
* Delegieren der Verwaltung von Access für bestimmte Applikationen an die am besten geeigneten Personen in Ihrer Organisation und Teilnahmeberechtigungen an, mit der Access-Systemsteuerung Azure AD-zugriffsanforderungen genehmigen, weisen Sie Zugriff direkt für ausgewählte Benutzer oder (optional) festlegen von Anmeldeinformationen für den Zugriff auf die Anwendung beim Kennwort-basierten einmaliges Anmelden konfiguriert ist


##<a name="configuring-self-service-application-access"></a>Konfigurieren des Anwendungszugriffs für Self-service

Zum Aktivieren des Anwendungszugriffs für Self-service-und die Anwendungsmöglichkeiten hinzugefügt werden können oder angefordert von Ihre Endbenutzer den folgenden Anweisungen konfiguriert.

**1:** Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com/)werden soll.

**2:**  Klicken Sie im Abschnitt **Active Directory** wählen Sie Ihr Verzeichnis aus, und wählen Sie dann die Registerkarte **Applications** . 

**3:** Wählen Sie die Schaltfläche **Hinzufügen** , und verwenden Sie die Option Katalog auswählen und Hinzufügen einer Anwendungs.

**4:** Nachdem Sie Ihre app hinzugefügt wurde, erhalten Sie die Seite "app Schnellstart". Klicken Sie auf **Konfigurieren einmaliges Anmelden**, wählen Sie den gewünschten einzelnen anmelden Modus aus, und speichern Sie die Konfiguration. 

**5:** Wählen Sie dann die Registerkarte **Konfigurieren** . Um Benutzern Zugriff auf diese app aus dem Azure AD-Access Bedienfeld anfordern zu aktivieren, legen Sie **die Self-service-Anwendungszugriff zulassen** auf **Ja**.

![][1]

**6:** Um einen Genehmigungsworkflow für zugriffsanforderungen optional konfigurieren zu können, legen Sie **Anfordern der Genehmigung vor dem Gewähren des Zugriffs** auf **Ja**. Eine oder mehrere genehmigende Personen können dann mithilfe der Schaltfläche **genehmigende Personen** ausgewählt werden.

Ein Genehmiger kann jeder Benutzer in der Organisation mit einem Azure AD-Konto, und könnte für Konto bereitgestellt, Lizenzierung verantwortlich ist, oder einer beliebigen anderen Geschäftsprozess Ihrer Organisation erfordert, bevor Sie gewähren des Zugriffs auf eine app. Der Genehmiger könnte auch den Besitzer der Gruppe von einer oder mehreren freigegebenen Kontogruppen und kann der Benutzer auf einen der diese Gruppen, damit diese Zugriff über ein freigegebenes Konto gewähren zuweisen. 

Wenn keine Genehmigung erforderlich ist, erhalten Benutzer sofort die Anwendung ihren Azure AD-Access Systemsteuerung hinzugefügt. Dies geeignet, wenn die Anwendung für die [Automatische Benutzer bereitgestellt](active-directory-saas-app-provisioning.md)wurde, oder ["Benutzer verwaltete" Kennwort SSO-Modus](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) der Benutzer bereits verfügt über ein Benutzerkonto und das Kennwort kennt eingerichtet wurde.

**7:** Wenn die Anwendung für die Verwendung von Kennwörtern basierende einmaliges Anmelden, und klicken Sie dann eine Option umschaltbaren des Genehmigers Festlegen der SSO-Anmeldeinformationen für jeden Benutzer konfiguriert wurde, ist ebenfalls verfügbar. Finden Sie im folgenden Abschnitt Stellvertretung Access Management für Weitere Informationen.

**8:** Schließlich zeigt die **Gruppe Self-Assigned Benutzer** den Namen der Gruppe, die zum Speichern der Benutzer, die Zugriff auf die Anwendung zugewiesen oder erteilt wurden verwendet werden soll. Der Access-Genehmiger machen Sie der Besitzer dieser Gruppe. Wenn Sie der Gruppennamen dargestellt nicht vorhanden ist, wird es automatisch erstellt. Optional kann den Namen der Gruppe auf den Namen einer vorhandenen Gruppe festgelegt werden.

**9:** Klicken Sie auf am unteren Rand des Bildschirms **Speichern** , um die Konfiguration zu speichern. Jetzt werden Benutzern Zugriff auf diese app aus dem Bereich Access anfordern können.

**10:** Um den Endbenutzer versuchen, melden Sie sich in der Organisation Azure AD-Access Systemsteuerung am https://myapps.microsoft.com, vorzugsweise mit einem anderen Konto, das ein Genehmiger app nicht zur Verfügung. 

**11:** Klicken Sie auf die Kachel **Weitere Programme suchen** , klicken Sie auf die Registerkarte **Applications** . Dadurch werden einen Katalog von die Komponenten, die für den Anwendungszugriff von Self-service-im Verzeichnis, die Möglichkeit zum Suchen und Filtern nach app Kategorie auf der linken Seite aktiviert wurden. 

**12:** Indem Sie auf eine app startet den Anforderungsprozess. Wenn kein Genehmigungsprozess erforderlich ist, wird dann die Anwendung werden unmittelbar unter der Registerkarte **Applications** nach einer kurzen Bestätigung hinzugefügt. Wenn Genehmigung erforderlich ist, und klicken Sie dann Sie ein Dialogfeld sehen, die dieses angibt, und eine e-Mail-Nachricht werden an die genehmigenden Personen zu senden. (Schnelle Notiz: Sie müssen in der Systemsteuerung Access als ein nicht-Genehmiger angemeldet sein, um dieses Verfahren Anforderung finden Sie unter).

**13:** Die e-Mail weist den Genehmiger melden Sie sich bei der Access-Systemsteuerung Azure AD- und die Anforderung genehmigen. Nachdem die Anforderung genehmigt ist (und alle Inhalten Prozessen, die Sie definieren durch den Genehmiger ausgeführt wurde), sieht der Benutzer dann die Anwendung, klicken Sie auf die Registerkarte **Applications** angezeigt werden, können sie sich wieder anmelden.

##<a name="delegated-application-access-management"></a>Delegierte Access anwendungsverwaltung

Ein Anwendung Access Genehmiger kann jeder Benutzer in Ihrer Organisation sein, die die am besten geeigneten Person zu genehmigen oder Verweigern des Zugriffs auf die jeweilige Anwendung ist. Diesen Benutzer könnte verantwortlich Konto bereitgestellt, Lizenzierung, oder einer beliebigen anderen Geschäftsprozess Ihrer Organisation, die vor dem Gewähren des Zugriffs auf eine app erforderlich ist.
 
Beim Konfigurieren des Anwendungszugriffs für Self-service-oben beschrieben, wird alle zugeordnete Anwendung genehmigende Personen **Verwalten von Applications** Kachel zusätzliche im Bereich Azure AD-Access angezeigt, die ihnen die Anwendungsmöglichkeiten anzeigt, die sie für die Access-Administrator sind. Durch Klicken auf eine app zeigt einen Bildschirm mit mehreren Optionen.

![][2]

###<a name="approve-requests"></a>Genehmigen von Besprechungsanfragen

Die Kachel **Anfragen genehmigen** genehmigende Personen ausstehende Genehmigungen erfolgt die app-spezifischen sehen kann, und leitet auf der Registerkarte Genehmigungen erfolgt, in dem die Anforderungen bestätigt oder abgelehnt werden kann. Beachten Sie, dass die genehmigenden Person auch automatisierte e-Mails empfängt, sobald eine Anforderung, die erstellt wird weist, was zu tun ist.

###<a name="add-users"></a>Hinzufügen von Benutzern

Die Kachel **Benutzer hinzufügen** kann genehmigende Personen direkt ausgewählten Benutzern Zugriff auf die Anwendung gewähren möchten. Beim Klicken auf diese Kachel, sieht die genehmigenden Person an, dass ein Dialogfeld zum Anzeigen und für Benutzer in ihrem Verzeichnis suchen kann. Hinzufügen eines Benutzerergebnisse in der Anwendung, die in diese Benutzers Azure AD-Access Bereiche oder Office 365 angezeigt wird. Wenn Sie alle manuellen Benutzer bereitgestellt Prozess ist bei der app erforderlich, bevor der Benutzer anmelden kann, sollte der Genehmiger dieses Verfahren ausführen, vor dem Zuweisen des Zugriffs klicken.  

###<a name="manage-users"></a>Verwalten von Benutzern

Die Kachel **Benutzer verwalten** kann genehmigende Personen direkt aktualisieren oder entfernen, welche Benutzer auf die Anwendung zugreifen können. 

###<a name="configure-password-sso-credentials-if-applicable"></a>Kennwort SSO-Anmeldeinformationen zu konfigurieren (sofern zutreffend)

Die Kachel " **Konfigurieren** " wird nur angezeigt, wenn die Anwendung so, vom IT-Administrator konfiguriert wurde auf Grundlage von Kennwörtern für einmaliges verwenden, und der Administrator der genehmigenden Person die Möglichkeit erteilt, das Kennwort SSO-Anmeldeinformationen festlegen, wie zuvor beschrieben. Bei Auswahl dieser Option wird der Genehmiger mehrere Optionen für wie die Anmeldeinformationen weitergegeben werden, Benutzer zugewiesen angezeigt:

![][3]

* **Benutzer melden sich ihre eigenen Kennwörter** – In diesem Modus zugeordneten Benutzer wissen, was ihre Benutzernamen und Kennwörter für die Anwendung sind, und werden aufgefordert, diese in ihrer ersten Anmeldung mit der Anwendung eingeben. Dies das Kennwort SSO Groß-/Kleinschreibung entspricht, in dem der [Benutzer Anmeldeinformationen verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

* **Benutzer werden automatisch in separaten Konten verwenden, die ich verwalten signiert** – In diesem Modus zugeordneten Benutzer nicht zur Eingabe oder wissen ihre app-spezifische Anmeldeinformationen beim Anmelden bei der Anwendung erforderlich. In diesem Fall legt der Genehmiger die Anmeldeinformationen für jeden Benutzer nach dem Zuweisen des Zugriffs mit der Kachel **Benutzer hinzufügen** . Wenn der Benutzer über die Anwendung in ihren Access Systemsteuerung oder Office 365 klickt, werden diese automatisch angemeldet sein mit den Anmeldeinformationen festlegen, indem Sie den Genehmiger. Dies das Kennwort SSO Groß-/Kleinschreibung entspricht, in dem die [Anmeldeinformationen für Administratoren verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

* Der **Benutzer automatisch angemeldet sind, sich mit einem einzelnen Konto, die ich verwalten** : Dies ist eine spezielle Groß-/Kleinschreibung und eignet sich, wenn alle zugeordneten Benutzer müssen mit einem einzelnen freigegebenen Konto Zugriff erteilt werden. Der am häufigsten verwendeten Anwendungsfall-hierfür ist soziale Medien Applications, wobei eine Organisation verfügt über ein einzelnes "Unternehmenskonto" und mehrere Benutzer müssen dem Konto Aktualisierungen vorzunehmen. Dies entspricht auch das Kennwort SSO Fall, in dem die [Anmeldeinformationen für Administratoren verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Allerdings werden nach Auswahl dieser Option der Genehmiger aufgefordert, geben Sie den Benutzernamen und das Kennwort für die einzelnen freigegebenen Konto. Nachdem abgeschlossen ist, werden alle zugeordneten Benutzer über dieses Konto durch Klicken auf die Anwendung in ihren Azure AD-Access Bereiche oder Office 365 angemeldet sein.

##<a name="additional-resources"></a>Zusätzliche Ressourcen
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG

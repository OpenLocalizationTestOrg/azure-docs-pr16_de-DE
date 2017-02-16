<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit Facebook am Arbeitsplatz | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und Facebook am Arbeitsplatz konfigurieren."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Lernprogramm: Azure-Active Directory-Integration mit Facebook am Arbeitsplatz

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie Facebook bei der Arbeit mit Azure Active Directory (Azure AD) integrieren.

Integration von Facebook bei der Arbeit mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, die bei der Arbeit mit Facebook zugreifen 
- Sie können automatisch Konto für Benutzer bereitgestellt, die mit Facebook am Arbeitsplatz der Zugriff gewährt wurde
- Sie können Ihre Benutzer automatisch angemeldet-mit Facebook am Arbeitsplatz (einmaliges Anmelden) mit ihren Azure AD-Konten auf erste aktivieren.
- Sie können Ihre Konten an einem zentralen Ort verwalten. 

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit Sternchen CS, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Bei der Arbeit mit für einmaliges Anmelden bei Facebook aktiviert

Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Hinzufügen von Facebook am Arbeitsplatz aus dem Katalog
Um die Integration von Facebook bei der Arbeit in Azure AD konfigurieren zu können, müssen Sie Facebook am Arbeitsplatz aus dem Katalog zu Ihrer Liste der verwalteten SaaS apps hinzufügen.

**Wenn Facebook am Arbeitsplatz aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **Facebook am Arbeitsplatz**.

7. Wählen Sie im Ergebnisbereich aus **Facebook im Büro**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden wie Benutzer Authentifizierung mit Facebook bei der Arbeit mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden können.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit Facebook am Arbeitsplatz konfigurieren:**

1.  Nach dem Hinzufügen eines Facebook bei der Arbeit im klassischen Azure-Portal, klicken Sie auf **Konfigurieren einmaliges Anmelden**.

2.  Geben Sie im Bildschirm **Konfigurieren der App-URL** die URL ein, in dem Benutzer in Ihrer Facebook bei der Arbeit Anwendung melden. Dies ist Ihre Facebook unter Arbeit Mandanten URL (Beispiel: https://example.facebook.com/). Sobald Sie fertig sind, klicken Sie auf **Weiter**.

3.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrer Facebook am Firmenwebsite Arbeit als Administrator.

4. Folgen Sie den Anweisungen unter dem folgenden URL zum Konfigurieren von Facebook bei der Arbeit mit Azure AD als Identitätsanbieter: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Nachdem der fertigen, kehren Sie zu dem Browserfenster mit dem klassischen Azure-Portal zurück, klicken Sie auf das Kontrollkästchen, um zu bestätigen, dass Sie den Vorgang abgeschlossen haben, und klicken Sie dann klicken Sie auf **Weiter** und **abgeschlossen**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automatisch Bereitstellung von Benutzern mit Facebook am Arbeitsplatz

Azure AD unterstützt der Möglichkeit, automatisch synchronisieren die Details der zugeordneten Benutzer mit Facebook bei der Arbeit an. Diese automatische Synchronisierung ermöglicht bei der Arbeit, die Daten zu erhalten, die es für Benutzerberechtigungen für den Zugriff im voraus, die bei der erstmaligen Anmeldung muss, bei Facebook. Es Vorschriften heben auch Benutzer aus Facebook am Arbeitsplatz, wenn in Azure Active Directory Access gesperrt wurde.

Automatische Bereitstellung kann eingerichtet werden, indem Sie auf im Azure-Portal klassischen Fenster **Konfigurieren Konto bereitgestellt** .

Weitere Details zum Konfigurieren der automatischen Bereitstellung finden Sie unter [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Zuweisen von Benutzern zu Facebook am Arbeitsplatz

Für bereitgestellte AAD Benutzer am Arbeitsplatz Facebook auf ihre Access-Systemsteuerung sehen können müssen sie Access in der klassischen Azure-Portal zugewiesen werden.

**So weisen Benutzer zu Facebook am Arbeitsplatz:**

1.  Klicken Sie auf der Startseite für Facebook bei der Arbeit im klassischen Azure-Portal auf **Konten zuweisen**.

2.  Wählen Sie im Menü **Anzeigen** , ob Sie verwenden möchten, weisen einen Benutzer oder eine Gruppe zu Facebook am Arbeitsplatz, und klicken Sie auf die Schaltfläche mit dem Häkchen.

3.  Wählen Sie in der Ergebnisliste dem Benutzer oder der Gruppe, der Sie Facebook am Arbeitsplatz zuweisen möchten.

4.  Klicken Sie in der Fußzeile der Seite auf die Schaltfläche **zuweisen** .


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png





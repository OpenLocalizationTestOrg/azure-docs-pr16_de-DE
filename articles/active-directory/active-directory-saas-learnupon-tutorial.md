<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Novatus | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und LearnUpon konfigurieren."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Lernprogramm: Azure-Active Directory-Integration in LearnUpon

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie LearnUpon mit Azure Active Directory (Azure AD) integrieren.  
Integration von LearnUpon mit Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, die auf LearnUpon zugreifen
- Sie können Ihre Benutzer automatisch auf LearnUpon (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – der Azure Active Directory klassischen verwalten. 


Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit LearnUpon, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine LearnUpon einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von LearnUpon aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-learnupon-from-the-gallery"></a>Hinzufügen von LearnUpon aus dem Katalog
Um die Integration der LearnUpon in Azure AD zu konfigurieren, müssen Sie LearnUpon zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um LearnUpon aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **LearnUpon**ein.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. Wählen Sie im Ergebnisbereich **LearnUpon aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit LearnUpon basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück LearnUpon an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LearnUpon eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in LearnUpon zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit LearnUpon testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer LearnUpon Benutzer testen](#creating-a-learnupon-test-user)** : ein Gegenstück von Britta Simon in LearnUpon haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung LearnUpon.



**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit LearnUpon konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite **LearnUpon** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der LearnUpon auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus:.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    ein. Geben Sie im Textfeld **URL Antworten** der Assertion Consumer Service-URL dem folgenden Muster:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Klicken Sie auf **Weiter**. 


4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei LearnUpon** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer. Wir benötigen diese Zertifikat und Metadaten URLs (Entität-ID, melden Sie sich In URL SSO und melden Sie sich ab URL) SSO auf LearnUpon Seite einrichten.

    b. Klicken Sie auf **Weiter**.




1. Öffnen Sie eine andere Instanz des Browsers und melden Sie sich mit einem Administratorkonto in LearnUpon ein. 

1. Klicken Sie auf der Registerkarte **Einstellungen** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. Klicken Sie auf **Single Sign On - SAML**, und klicken Sie dann auf **Allgemeine Einstellungen** zum Konfigurieren der SAML-Einstellungen.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. Führen Sie im Abschnitt **Allgemeine Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    ein. Wählen Sie **aktiviert**aus.

    b. Wählen Sie als **Version** **2.0**aus.

    c. Als **Überspringen Bedingungen**wählen Sie **Nein**aus.

    d. In das Textfeld **Parameter Namen SAML Token Posten** enthält der Namen der Anfrage Beitrag-Parameter der SAML-Consumer-URL vorstehende angegebenen Typ die SAML-Assertion werden überprüft und authentifiziert – beispielsweise **SAMLResponse**.

    e. Geben Sie den Wert, der angibt, wo in der SAML-Assertion die Benutzer-ID (e-Mail-Adresse) – beispielsweise befindet sich in das Textfeld **Bezeichner Namensformat** **Urn: Oasis: Namen: Tc: SAML:1.1:nameid-Format: EmailAddress**.

    f. Geben Sie den Wert, der angibt, wo die Benutzer an gesendet werden, wenn diese auf Ihrer hochgeladene Symbol von Azure klassischen Portal Anmeldebildschirm Ihres klicken Sie auf ein, in das Textfeld **Anbieter Speicherort zu identifizieren** .

    g.in im Portal Azure klassischen, kopieren Sie die **Einzelnen Sign-Out Dienst-URL**, und fügen Sie ihn in der **URL Abmelden** Textbos.

    h. Klicken Sie auf **Verwalten Finger gedruckt wird**, und Laden Sie die Finger Drucken Ihrer heruntergeladene Zertifikat. 


1. Klicken Sie auf **User Settings**, und gehen Sie folgendermaßen vor:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    ein. Geben Sie im Textfeld **Vorname Bezeichner formatieren** den Wert, das angibt, wo in die SAML-Assertion der Benutzer Vorname – beispielsweise befindet: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ Vorname**.

    b. Geben Sie in das Textfeld **Letzten Namensformat Bezeichner enthält** den Wert, das angibt, wo in die SAML-Assertion der Nachname Benutzer - beispielsweise befindet: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ Nachnamen**.


6. Im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-learnupon-test-user"></a>Erstellen eines Testbenutzers LearnUpon

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in LearnUpon bezeichnet. LearnUpon unterstützt in-Time-Bereitstellung, welche ist standardmäßig aktiviert.

Keine für Sie in diesem Abschnitt Aktionselement ist vorhanden. Bei dem Versuch, LearnUpon zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt werden. [Konfigurieren von Azure AD einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell zu erstellen müssen, müssen Sie die LearnUpon Supportteam.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf LearnUpon erteilen.

![Benutzer zuweisen][200] 

**Um Britta Simon LearnUpon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **LearnUpon**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel LearnUpon im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung LearnUpon angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png

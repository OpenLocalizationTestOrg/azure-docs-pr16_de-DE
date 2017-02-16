<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in nämlich | Microsoft Azure"
    description="Informationen zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und nämlich."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Lernprogramm: Azure-Active Directory-Integration in nämlich

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie mit Azure Active Directory (Azure AD) nämlich integriert werden soll.

Integration von nämlich mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD, nämlich die Zugriff auf weist steuern 
- Sie können die Benutzer automatisch angemeldet für den Zugriff auf nämlich abrufen aktivieren (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration in d. h., Sie benötigen die folgenden Elemente:

- Ein Azure AD-Abonnement
- Nämlich einmalige Anmeldung aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen, und zwar aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-namely-from-the-gallery"></a>Hinzufügen, und zwar aus dem Katalog
Zum Konfigurieren der Integration von nämlich in Azure AD müssen Sie zu Ihrer Liste der apps für verwaltete SaaS nämlich aus dem Katalog hinzuzufügen.

**Wenn nämlich aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld ein **und zwar**ein.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Wählen Sie im Ergebnisbereich aus, **und zwar**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt wird veranschaulicht, wie konfigurieren und Azure AD-einmaliges Anmelden mit zu testen, und zwar auf Grundlage eines Testbenutzers "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück nämlich die ein Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in nämlich eingerichtet werden.

Dieser Link Beziehung wird hergestellt, nämlich den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in zuweisen.
 
Zum Konfigurieren und Testen Azure AD einmaliges Anmelden mit nämlich Ihnen die folgenden Bausteine ausführen müssen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer nämlich testen Benutzer](#creating-a-namely-test-user)** – ein Gegenstück von Britta Simon haben in nämlich, verknüpft ist in der Azure AD-Darstellung Ihrer.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung nämlich. 




**Azure AD-einmaliges Anmelden mit nämlich so konfigurieren Sie die folgenden Schritte aus:**

1. Im Azure klassischen-Portal auf der **nämlich** Integrationsseite Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer auf nämlich bei** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.
 
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus:.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von Ihren Benutzern verwendet wird, nämlich Ihrer Anwendung bei auf (z. B.: *https://fabrikam.Namely.com/*).

    b. Klicken Sie auf **Weiter**.
 
 
4. Führen Sie auf der Seite **einmaliges Anmelden bei nämlich konfigurieren** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


1. Melden Sie sich in einem anderen Browserfenster, um zu Ihrer nämlich Firmenwebsite als Administrator.

1. Klicken Sie oben auf der Symbolleiste auf **Firma**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Klicken Sie auf der Registerkarte **Einstellungen** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Klicken Sie auf **SAML**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Führen Sie auf der Seite **SAML-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    ein. Klicken Sie auf **SAML aktivieren**. 

    b. Im Azure klassischen-Portal auf der Seite **einmaliges Anmelden bei nämlich konfigurieren** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und klicken Sie dann in das Textfeld **Identität Anbieter DDO Url** einzufügen. 

    c. Öffnen des heruntergeladenen Zertifikats in Editor, kopieren Sie den Inhalt aus, und fügen Sie ihn in das Textfeld **Identität Anbieter Zertifikat** .    

    d. Klicken Sie auf **Speichern**.


6. Im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

![Erstellen von Azure AD-Benutzer][20]


**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-namely-test-user"></a>Erstellen einer nämlich testen Benutzer

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers, nämlich Britta Simon in aufgerufen.

**Führen Sie zum Erstellen eines Benutzers aufgerufen, nämlich Britta Simon in die folgenden Schritte aus:**

1. Melden Sie sich für den Zugriff auf Ihre nämlich Firmenwebsite als Administrator.

1. Klicken Sie oben auf der Symbolleiste auf **Personen**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Klicken Sie auf der Registerkarte **Verzeichnis** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Klicken Sie auf **neue Person hinzufügen**.



1. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Neue Person hinzufügen** :

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.

    b. Geben Sie im Textfeld **Nachname** **Simon**aus.

    c. Geben Sie in das Textfeld **E-Mail** Brittas e-Mail-Adresse in der klassischen Azure-Portal aus.

    d. Klicken Sie auf **Speichern**.





### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf nämlich erteilen.

![Benutzer zuweisen][200] 

**So weisen Sie Britta Simon nämlich die folgenden Schritte ausführen:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **nämlich**ein.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.

Beim Klicken auf die Kachel und zwar im Bereich Access, Sie sollten erhalten automatisch angemeldet für den Zugriff auf Ihre nämlich Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png







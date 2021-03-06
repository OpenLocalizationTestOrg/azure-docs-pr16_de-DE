<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Novatus | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und 10.000 ft Pläne."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a>Lernprogramm: Azure-Active Directory-Integration in 10.000 ft-Pläne

Ziel dieses Lernprogramms ist zu veranschaulichen 10.000 ft Pläne mit Azure Active Directory (Azure AD) integriert werden soll.  
Integrieren von 10.000 ft Pläne in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf 10.000 ft Pläne zugreifen kann
- Sie können Ihre Benutzer automatisch auf 10.000 ft Pläne (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit 10.000 ft Pläne, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- 10.000 ft Pläne einmalige Anmeldung aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von 10.000 ft aus dem Katalog Pläne
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-10000ft-plans-from-the-gallery"></a>Hinzufügen von 10.000 ft aus dem Katalog Pläne
Zum Konfigurieren der Integration von 10.000 ft Pläne in Azure AD müssen Sie 10.000 ft Pläne zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um 10.000 ft Pläne aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **10.000 ft-Pläne**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_01.png)

7. Wählen Sie im Ergebnisbereich **10.000 ft-Pläne**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit 10.000 ft Pläne basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück 10.000 ft Pläne an einen Benutzer in Azure AD ist. Kurzum, einen Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in 10.000 ft Pläne muss eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in 10.000 ft-Plänen zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit 10.000 ft Pläne testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer 10.000 ft Pläne Benutzer testen](#creating-a-10000ft-plans-test-user)** : ein Gegenstück von Britta Simon in zertifizieren haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer 10.000 ft Pläne Anwendung.



**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit 10.000 ft Pläne konfigurieren:**

1. Im Azure klassischen-Portal auf **10.000 ft Pläne** Anwendung Integrationsseite klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer 10.000 ft Pläne auf bei** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_03.png) 

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_04.png) 


    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** `https://app.10000ft.com`.

    b. Geben Sie in das Textfeld **Bezeichner** `https://app.10000ft.com/saml/metadata`.

    > [AZURE.NOTE] Der Wert für **Bezeichner** unterscheidet sich, wenn Sie eine benutzerdefinierte Domäne verfügen. Wenn Sie Hilfe benötigen, wenden Sie sich an Ihr [Supportteam für 10.000 ft-Pläne](mailto:support@10000ft.com).  

    c. Klicken Sie auf **Weiter**


4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei 10.000 ft-Pläne** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um SSO für die Anwendung konfigurierten zu gelangen, wenden Sie sich an Ihr [Supportteam für 10.000 ft Pläne](mailto:support@10000ft.com), fügen Sie das heruntergeladene Zertifikat an, und geben sie die URL des Herausgebers, die URL der SAML-SSO und melden Sie sich ab URL.

6. Im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie zum Erstellen eines Testbenutzers VORFÜHREN SECURE in Azure AD die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   


### <a name="creating-a-10000ft-plans-test-user"></a>Erstellen einer 10.000 ft testen Pläne Benutzer

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in 10.000 ft Pläne bezeichnet. 10.000 ft Pläne unterstützt in-Time-Bereitstellung, welche ist standardmäßig aktiviert.

Keine für Sie in diesem Abschnitt Aktionselement ist vorhanden. Bei dem Versuch, 10.000 ft Pläne zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt werden. 

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell zu erstellen müssen, müssen Sie die zertifizieren Supportteam.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf 10.000 ft Pläne erteilen.

![Benutzer zuweisen][200] 

**Um 10.000 ft Pläne Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **10.000 ft Pläne**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10000ftplans_50.png) 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die 10.000 ft Pläne Kachel im Bereich Access klicken, Sie sollten automatisch angemeldet-anwendungsspezifische Pläne 10.000 ft auf abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_205.png

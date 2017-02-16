<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in "Schieferleiste" erfahren | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und erfahren Sie, "Schieferleiste"."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Lernprogramm: Azure-Active Directory-Integration in "Schieferleiste" erfahren

In diesem Lernprogramm erfahren Sie, wie "Schieferleiste" Informationen mit Azure Active Directory (Azure AD) integriert werden soll.

Integration von mit Azure AD "Schieferleiste" erfahren Sie, bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, die auf "Schieferleiste" Informationen zugreifen
- Sie können mit ihren Azure AD-Konten Ihrer Benutzer automatisch angemeldet-"Schieferleiste" (einmaliges Anmelden) Informationen in erste aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit "Schieferleiste" erfahren Sie, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Erfahren Sie, Cloud-Plattform "Schieferleiste" einmalige Anmeldung aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
In diesem Lernprogramm testen Sie Azure AD-einmaliges Anmelden in einer testumgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von Informationen aus dem Katalog "Schieferleiste"
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-blackboard-learn-from-the-gallery"></a>Hinzufügen von Informationen aus dem Katalog "Schieferleiste"
Zum Konfigurieren der Integration von "Schieferleiste" Informationen in Azure AD müssen Sie zu Ihrer Liste der verwalteten SaaS apps "Schieferleiste" Informationen aus dem Katalog hinzuzufügen.

**Wenn "Schieferleiste" Informationen aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **"Schieferleiste" zu erfahren**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_01.png)

7. Wählen Sie im Ergebnisbereich **"Schieferleiste" erfahren Sie,**und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD-einmaliges Anmelden mit "Schieferleiste" erfahren Sie, basierend auf einer Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück "Schieferleiste" Informationen für einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in "Schieferleiste" erfahren eingerichtet werden.

Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in "Schieferleiste" erfahren zuweisen.

Zum Konfigurieren und Testen Azure AD-einmaliges Anmelden mit "Schieferleiste" erfahren Sie, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer "Schieferleiste" erfahren Sie, testen Sie Benutzer](#creating-a-blackboard-learn-test-user)** : ein Gegenstück von Britta Simon in "Schieferleiste" erfahren haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD-einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren in der Anwendung "Schieferleiste" erfahren einmaliges Anmelden.

"Schieferleiste" lernen Sie, wie Anwendung erwartet die SAML-Assertionen in einem bestimmten Format an. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute der Registerkarte **"Atrribute"** der Anwendung verwalten. Das folgende Bildschirmabbild zeigt ein Beispiel dafür. 

![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_51.png) 

**So konfigurieren Sie Azure AD-einmaliges Anmelden mit "Schieferleiste" erfahren Sie, die folgenden Schritte aus:**


1. Klicken Sie im Azure klassischen Portal **Erfahren "Schieferleiste"** Anwendung Integration in die Seite, das Menü im oberen Bereich auf **Attribute**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_80.png) 


1. Klicken Sie im Dialogfeld **token SAML-Attribute** für jede Zeile angezeigt, in der Tabelle unten, gehen Sie folgendermaßen vor: Wir haben Userprincipalname als eindeutige Benutzer-Attribut hier zuordnen, aber Sie können auf den entsprechenden Wert, der den Benutzer in der Organisation eindeutig unterscheiden zuordnen und Maps "Schieferleiste" Username-Feld erfahren.

  	| Attributname | Attributwert |
  	| --- | --- |    
  	| urn:oid:1.3.6.1.4.1.5923.1.1.1.6  | User.userPrincipalName |
   
 
    ein. Klicken Sie auf **Benutzerattribut hinzufügen** , um das Dialogfeld **Benutzer Attribure hinzufügen** zu öffnen.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_81.png) 


    b. Geben Sie in das Textfeld **Attrubute Name** das Attribut Bankname für diese Zeile ein.

    c. Aus der Liste **Attributwert** Selsect das Attributwert für diese Zeile angezeigt.

    d. Klicken Sie auf **abgeschlossen**.  

2. Im Portal klassischen auf der **"Schieferleiste" erfahren** Integrationsseite Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .
     
    ![Konfigurieren Sie einmaliges Anmelden][6] 

3. Klicken Sie auf der Seite **Wie möchten Sie Benutzer sich "Schieferleiste" Informationen auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_03.png) 

4. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_04.png) 

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung an Ihrer Informationen "Schieferleiste"-Anwendung unter Verwendung des folgenden Musters verwendet: **https://\<Namen Preise Unternehmen\>.blackboard.com/**
    
    b. Klicken Sie auf **Weiter**
 
5. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei "Schieferleiste" erfahren Sie,** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_05.png)

    ein. Klicken Sie auf **Herunterladen von Metadaten**aus, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


6. Um SSO für die Anwendung konfigurierten zu gelangen, wenden Sie sich an Supportteam "Schieferleiste" erfahren Sie, und teilen Sie sie mit der folgenden:

    • Die heruntergeladene Metadaten


7. Im Portal klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
In diesem Abschnitt erstellen Sie einen Testbenutzer im klassischen Portal Britta Simon bezeichnet.


![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:  ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** -Dialogfeld führen Sie die folgenden Schritte aus: ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_07.png) 

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-blackboard-learn-test-user"></a>Erstellen eines Benutzers mit Test "Schieferleiste" erfahren

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in "Schieferleiste" zu erfahren. 

"Schieferleiste" lernen Sie, wie Anwendung unterstützt nur in der Zeit Benutzer bereitgestellt. Stellen Sie sicher, dass Sie die Ansprüche konfiguriert haben, wie im Abschnitt ** [Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-sign-on) beschrieben.**

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

In diesem Abschnitt aktivieren Sie Britta Simon Azure einmaliges Anmelden verwenden, indem Sie ihre Access "Schieferleiste" Informationen erteilen.

![Benutzer zuweisen][200] 

**Zuweisen von Britta Simon zu "Schieferleiste" erfahren Sie, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **"Schieferleiste" Informationen**ein.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_50.png) 

3. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

5. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD-einzelne anmelden Konfiguration mit der Access-Systemsteuerung.

"Schieferleiste" lernen Sie, wie die Anwendung Unterstützung beim Klicken auf die Kachel "Schieferleiste" finden Sie im Bereich Access, sollten Sie automatisch angemeldet einmaliges Anmelden an Ihrer Anwendung "Schieferleiste" Informationen erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_205.png

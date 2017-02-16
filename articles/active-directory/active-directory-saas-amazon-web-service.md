<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit Amazon Web Service (AWS) | Microsoft Azure"
    description="Erfahren Sie, wie Amazon Web Services (AWS) mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Lernprogramm: Azure-Active Directory-Integration mit Amazon Web Service (AWS)

Ziel dieses Lernprogramms ist zu veranschaulichen Amazon Web Service (AWS) mit Azure Active Directory (Azure AD) integriert werden soll.  
Integrieren von Amazon Web Service (AWS) in Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, die auf der Amazon Web Service (AWS) zugreifen 
- Sie können Ihre Benutzer automatisch auf Amazon Web Service (AWS) (einmaliges Anmelden) angemeldete erhalten mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren der Azure AD-Integration mit Amazon Web Service (AWS), benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine Amazon Web Service (AWS) einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wichtigsten Bausteine:

1. Hinzufügen von Amazon Web Service (AWS) aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Service (AWS) aus dem Katalog
Um die Integration von Amazon Web Service (AWS) in Azure AD konfigurieren zu können, müssen Sie Amazon Web Service (AWS) aus dem Katalog zu Ihrer Liste der verwalteten SaaS apps hinzufügen.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Um Amazon Web Service (AWS) aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1] 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** . 
   
    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite. 
   
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**. 
   
    ![Applikationen][4]

6. Geben Sie in das Suchfeld **Amazon Web Service (AWS)**ein.
   
    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **Amazon Web Service (AWS)**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Applikationen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt wird veranschaulicht, wie konfigurieren und Testen Azure AD-einmaliges Anmelden mit Amazon Web Service (AWS) basierend auf einer Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück in Amazon Web Service (AWS), um einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Service (AWS) hergestellt werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in Amazon Web Service (AWS) zuweisen.
 
Zum Konfigurieren und Testen Azure AD-einmaliges Anmelden mit Amazon Web Service (AWS), müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelnen einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Amazon Web Service (AWS) Benutzer testen](#creating-a-halogen-software-test-user)** : ein Gegenstück von Britta Simon in Amazon Web Service (AWS) haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurieren von Azure AD einzelnes einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung Amazon Web Service (AWS).  
Ihrer Anwendung Amazon Web Service (AWS) erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erfordert. Das folgende Bildschirmabbild zeigt ein Beispiel dafür.


![Konfigurieren Sie einmaliges Anmelden][27]

**Führen Sie die folgenden Schritte aus, Azure AD-einmaliges Anmelden mit Amazon Web Service (AWS) um zu konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite Anwendung Integration **Amazon Web Service (AWS)** klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][7]

2. Klicken Sie auf der Seite **Wie möchten Sie sich auf der Amazon Web Service (AWS), klicken Sie auf Benutzer** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden][8]

3. Klicken Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** auf Weiter. 

    ![Konfigurieren der App-Einstellungen][9]
 
4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Amazon Web Service (AWS)** klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden][10]

5. In einem anderen Browserfenster melden Sie sich für den Zugriff auf Ihre Firmenwebsite Amazon Web Service (AWS) als Administrator.

6. Klicken Sie auf **Start Console**.

    ![Konfigurieren Sie einmaliges Anmelden][11]

7. Klicken Sie auf **Identität und Access-Verwaltung**. 

    ![Konfigurieren Sie einmaliges Anmelden][12]

8. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**. 

    ![Konfigurieren Sie einmaliges Anmelden][13]

9. Klicken Sie auf der Dialogseite **Anbieter konfigurieren** führen Sie die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][14]

     ein. Wählen Sie als **Typ Anbieter** **SAML**aus.

     b. Geben Sie im Textfeld **Name des Anbieters** einen Anbieternamen (z. B.: *WAAD*).

     c. Um die heruntergeladene Metadatendatei hochladen möchten, klicken Sie auf **Datei auswählen**.

     d. Klicken Sie auf **nächsten Schritt fort**.


10. Klicken Sie auf der Seite **Informationen zum Überprüfen Anbieter** Dialogfeld auf **Erstellen**. 

    ![Konfigurieren Sie einmaliges Anmelden][15]

11. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Neue Rolle erstellen**. 

    ![Konfigurieren Sie einmaliges Anmelden][16]

12. Klicken Sie im Dialogfeld **Rollennamen definieren** führen Sie die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][17]

    ein. Geben Sie einen Rollennamen im Textfeld **Name der Rolle** (z. B.: *Testbenutzer*).

    b. Klicken Sie auf **nächsten Schritt fort**.

13. Klicken Sie im Dialogfeld **Typ der Rolle auswählen** führen Sie die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][18]

    ein. Wählen Sie die **Rolle für Identität Anbieter zugreifen**.

    b. Klicken Sie im Abschnitt **Gewähren des Web einmaliges Anmelden (WebSSO) Zugriff auf SAML-Anbieter** auf **auswählen**.


14. Klicken Sie im Dialogfeld **Vertrauen herzustellen** führen Sie die folgenden Schritte aus:  

    ![Konfigurieren Sie einmaliges Anmelden][19]
     
     ein. Wählen Sie als SAML-Anbieter den SAML-Anbieter Previousley erstellt haben (z. B.: *WAAD*) 

     b. Klicken Sie auf **nächsten Schritt fort**.


15. Klicken Sie im Dialogfeld **Überprüfen Rolle zu vertrauen** klicken Sie auf **Nächsten Schritt fort**. 

    ![Konfigurieren Sie einmaliges Anmelden][32]


16. Klicken Sie im Dialogfeld **Anfügen Richtlinie** klicken Sie auf **Nächsten Schritt fort**.  

    ![Konfigurieren Sie einmaliges Anmelden][33]


17. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Überprüfen** :   

    ![Konfigurieren Sie einmaliges Anmelden][34]

     ein. Kopieren Sie den Wert für die **Rolle ARN** .

     b. Kopieren Sie den Wert für **Vertrauenswürdige Einheiten** ARN.

     c. Klicken Sie auf **Rolle erstellen**. 

18. Klicken Sie im Portal Azure klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.

    ![Was ist Azure AD-Verbindung herstellen][20]

19. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen** , um das Dialogfeld **Konfigurieren einmaligen Anmeldens** schließen.

    ![Was ist Azure AD-Verbindung herstellen][22]


20. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen. 

    ![Konfigurieren Sie einmaliges Anmelden][21]

21. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Konfigurieren Sie einmaliges Anmelden][23]

22. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld Benutzerattribut hinzufügen. 

    ![Konfigurieren Sie einmaliges Anmelden][24] 

    ein. Geben Sie in das Textfeld **Name Attribut** **https://aws.amazon.com/SAML/Attributes/Role**ein.

    b. Geben Sie in das Textfeld **Wert Attribut** **[der Rolle ARN Wert], [der Wert vertrauenswürdige Entität ARN]**.

    >[AZURE.TIP] Dies sind die Werte, die Sie im Dialogfeld überprüfen kopiert haben, wenn Sie Ihre Rolle erstellt haben. 

    c. Klicken Sie auf **abgeschlossen** , um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.

23. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Konfigurieren Sie einmaliges Anmelden][23]


24. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld Benutzerattribut hinzufügen. 

    ![Konfigurieren Sie einmaliges Anmelden][25]


     ein. Geben Sie in das Textfeld **Name Attribut** **https://aws.amazon.com/SAML/Attributes/RoleSessionName**ein.

     b. Geben Sie im Textfeld **Attributwert** ein, oder wählen Sie **user.userprincipalname** aus der Dropdownliste aus.
     
    ![Konfigurieren Sie einmaliges Anmelden][35]
    

     c. Klicken Sie auf **abgeschlossen** , um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.


25. Klicken Sie auf **Änderungen anzuwenden**. 

    ![Konfigurieren Sie einmaliges Anmelden][26]





### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen

Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.
  2. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.
  3. Klicken Sie auf Weiter.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In der **Nachname** Txtbox, Typ **Simon**.
  
    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
  
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.
  
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Erstellen eines Testbenutzers Amazon Web Service (AWS)

Das Ziel der in diesem Abschnitt wird einen Benutzer mit dem Namen Britta Simon in Amazon Web Service (AWS) erstellt.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in Amazon Web Service (AWS) zu erstellen:

1. Melden Sie sich als Administrator in Ihrer Firmenwebsite **Amazon Web Service (AWS)** an.

2. Klicken Sie auf das Symbol **Start Console** . 

    ![Konfigurieren Sie einmaliges Anmelden][11]

3. Klicken Sie auf Identität und Management zugreifen. 

    ![Konfigurieren Sie einmaliges Anmelden][28]

4. Klicken Sie im Dashboard klicken Sie auf Benutzer, und klicken Sie dann auf neuen Benutzer erstellen. 

    ![Konfigurieren Sie einmaliges Anmelden][29]

5. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld Benutzer erstellen: 

    ![Konfigurieren Sie einmaliges Anmelden][30]

     ein. Geben Sie in die Textfelder ein, **Geben Sie den Namen** Simons Brita Benutzernamen (Userprincipalname) in Azure AD ein.

     b. Klicken Sie auf **Erstellen**.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie die Berechtigung keinen Zugriff auf Amazon Web Service (AWS).

![Benutzer zuweisen][31]

**Um Britta Simon CloudPassage zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][26]

2. Wählen Sie in der Liste Applikationen **Amazon Web Service (AWS)**ein.

    ![Benutzer zuweisen][27]

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][25]

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][29]

### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel Amazon Web Service (AWS) im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung Amazon Web Service (AWS) angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png























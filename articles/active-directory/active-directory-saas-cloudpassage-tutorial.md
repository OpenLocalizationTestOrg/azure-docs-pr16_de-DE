<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in CloudPassage | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und CloudPassage konfigurieren."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Lernprogramm: Azure-Active Directory-Integration in CloudPassage

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie CloudPassage mit Azure Active Directory (Azure AD) integrieren.  
Integration von CloudPassage mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, die auf CloudPassage zugreifen 
- Sie können Ihre Benutzer automatisch auf CloudPassage (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – Azure Active Directory verwalten. 


Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit CloudPassage, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine CloudPassage einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Azure AD-testumgebung besitzen, können Sie ein kostenloses Testabonnement der einen Monat Azure abrufen [können](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von CloudPassage aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-cloudpassage-from-the-gallery"></a>Hinzufügen von CloudPassage aus dem Katalog
Um die Integration der CloudPassage in Azure AD zu konfigurieren, müssen Sie CloudPassage zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

### <a name="to-add-cloudpassage-from-the-gallery-perform-the-following-steps"></a>Um CloudPassage aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 
 
    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **CloudPassage**ein.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **CloudPassage aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Applikationen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit CloudPassage basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück CloudPassage an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in CloudPassage eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in CloudPassage zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit CloudPassage testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelnen einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer CloudPassage Benutzer testen](#creating-a-halogen-software-test-user)** : ein Gegenstück von Britta Simon in CloudPassage haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure AD-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung CloudPassage.  
Die CloudPassage-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das Hinzufügen von benutzerdefinierten Attribut Zuordnungen zu der SAML-token Attribute Konfiguration erfordert. Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Konfigurieren Sie einmaliges Anmelden][21]

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit CloudPassage konfigurieren:**

1. Klassische Azure AD-Portal auf der Seite **CloudPassage** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][7]

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der CloudPassage auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden][8]

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus: 

    ![Konfigurieren der App-Einstellungen][9]
 
    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL Ihrer Benutzer melden Sie sich für den Zugriff auf Ihre app CloudPassage untersuchten (z. B.: *https://portal.cloudpassage.com/saml/init/accountid*). 

    b. Geben Sie im Textfeld **URL Antworten** Ihrer AssertionConsumerService-URL (z. B.: *https://portal.cloudpassage.com/saml/consume/accountid*). Sie können einen Wert für dieses Attribut abrufen, **SSO-Setup-Dokumentation** im Abschnitt **Einstellungen für einzelne melden Sie sich** von Ihrem CloudPassage Portal klicken.  
    ![Konfigurieren Sie einmaliges Anmelden][10]

    C. Klicken Sie auf **Weiter**.



4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei CloudPassage** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer. 

    ![Konfigurieren Sie einmaliges Anmelden][11]

5. In einem anderen Browserfenster melden Sie sich für den Zugriff auf Ihre CloudPassage Firmenwebsite als Administrator.

6. Klicken Sie im Menü oben klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Websiteverwaltung**. 

    ![Konfigurieren Sie einmaliges Anmelden][12]

7. Klicken Sie auf der Registerkarte **Einstellungen Authentifizierung** . 

    ![Konfigurieren Sie einmaliges Anmelden][13]


8. Führen Sie im Abschnitt **Einstellungen für einzelne Zeichen** die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][14]


    ein. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei CloudPassage** kopieren Sie den Wert für die **URL des Herausgebers** , und klicken Sie dann in das Textfeld **SAML Herausgeber URL** einzufügen.

    b. Kopieren Sie des Werts **Service Provider (SP) initiiert Endpunkt** im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei CloudPassage** , und fügen Sie ihn in das Textfeld **SAML-Endpunkt-URL** .

    c. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei CloudPassage** kopieren Sie den Wert für die **Abmeldung URL** , und fügen Sie ihn in das Textfeld **Startseite Abmelden** .

    d. Erstellen einer **Base-64** -codierte Datei aus Ihrem heruntergeladene Zertifikat. 
          
    >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

    e. Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **x 509-Zertifikat** .

    f. Klicken Sie auf **Speichern**.


9. Klicken Sie im Portal Azure AD-klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**. 

    ![Konfigurieren Sie einmaliges Anmelden][15]


10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**. 

    ![Konfigurieren Sie einmaliges Anmelden][16]



11. Klicken Sie in der NY oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen. 

    ![Konfigurieren Sie einmaliges Anmelden][17]

12. Führen Sie die folgenden Schritte aus, um die erforderlichen Attribute, für jede Zeile in der folgenden Tabelle hinzuzufügen: 

  	| Attributname | Attributwert |
  	| --- | --- |
  	| Vorname | User.GivenName |
  	| Nachname | User.Surname |
  	| E-Mail | User.Mail |

 

    ein. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Konfigurieren Sie einmaliges Anmelden][18]

    b. Geben Sie das Attribut Bankname für diese Zeile und als **Attribure Wert**, wählen Sie den Attributwert für diese Zeile angezeigt, in das Textfeld **Name Attribut** . 

    ![Konfigurieren Sie einmaliges Anmelden][19]
     
    c. Klicken Sie auf **abgeschlossen**.


13. Klicken Sie unten auf der Symbolleiste auf **Änderungen übernehmen**. 

    ![Konfigurieren Sie einmaliges Anmelden][20]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen

Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.
  
    c. Klicken Sie auf Weiter.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  
  
    b. In das letzte Textfeld **Name** , Typ, **Simon**.
  
    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.
  
    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
  
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
 
8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.
  
    b. Klicken Sie auf **abgeschlossen**.   


  
 
### <a name="creating-a-cloudpassage-test-user"></a>Erstellen eines Testbenutzers CloudPassage

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in CloudPassage bezeichnet.

#### <a name="to-create-a-user-called-britta-simon-in-cloudpassage-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in CloudPassage zu erstellen:

1.  Melden Sie sich für den Zugriff auf Ihre **CloudPassage** Firmenwebsite als Administrator. 

2.  Klicken Sie in der Symbolleiste oben auf **Einstellungen**, und klicken Sie dann auf **Websiteverwaltung**. 

    ![Erstellen eines Testbenutzers CloudPassage][22] 

3.  Klicken Sie auf der Registerkarte **Benutzer** , und klicken Sie dann auf **Neuen Benutzer hinzufügen**. 

    ![Erstellen eines Testbenutzers CloudPassage][23]
    
4.  Führen Sie im Abschnitt **Hinzufügen neuer Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Testbenutzers CloudPassage][24]

    ein. Geben Sie im Textfeld **Vorname** Britta aus.

    b. Geben Sie im Textfeld **Nachname** Simon aus.

    c. Geben Sie in das Textfeld für den **Benutzernamen** , das **E-Mail** -Textfeld und das Textfeld **E-Mail erneut** Brittas Benutzernamens in Azure AD ein.

    d. Wählen Sie als **Dateityp Access** **Rand Portal Zugriff aktivieren**aus.

    e. Klicken Sie auf **Hinzufügen**.










### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf CloudPassage erteilen aktivieren.

![Benutzer zuweisen][30]

**Um Britta Simon CloudPassage zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][26]

2. Wählen Sie in der Liste Applications **CloudPassage**.

    ![Benutzer zuweisen][27]

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][25]

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][29]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel CloudPassage im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung CloudPassage angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png






















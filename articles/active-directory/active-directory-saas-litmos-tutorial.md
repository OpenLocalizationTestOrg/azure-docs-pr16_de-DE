<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Litmos | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und Litmos konfigurieren."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Lernprogramm: Azure-Active Directory-Integration in Litmos

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie Litmos mit Azure Active Directory (Azure AD) integrieren.  
Integration von Litmos mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, die auf Litmos zugreifen 
- Sie können Ihre Benutzer automatisch auf Litmos (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – Azure Active Directory verwalten. 

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit Litmos, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine Litmos einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wichtigsten Bausteine:

1. Hinzufügen von Litmos aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-litmos-from-the-gallery"></a>Hinzufügen von Litmos aus dem Katalog
Um die Integration der Litmos in Azure AD zu konfigurieren, müssen Sie Litmos zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um Litmos aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Litmos**ein.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **Litmos aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Applikationen][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit Litmos basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück Litmos an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Litmos eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in Litmos zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit Litmos testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Litmos Benutzer testen](#creating-a-halogen-software-test-user)** : ein Gegenstück von Britta Simon in Litmos haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure AD-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung Litmos.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

Als Teil der Konfiguration müssen Sie die **Token SAML-Attribute** für eine Anwendung Litmos anpassen.  

![Azure AD einmaliges Anmelden][17] 

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit Litmos konfigurieren:**

1. Klassische Azure AD-Portal auf der Seite **Litmos** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Litmos auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.
 
    ![Azure AD einmaliges Anmelden][7] 


1. Melden Sie sich auf der Website Ihres Unternehmens Litmos (z. B.: *https://azureapptest.litmos.com/account/Login*) als Administrator.

    ![Azure AD einmaliges Anmelden][21] 


1. Klicken Sie in der Navigationsleiste auf der linken Seite auf **Konten**.

    ![Azure AD einmaliges Anmelden][22] 


1. Klicken Sie auf der Registerkarte **Integration** .

    ![Azure AD einmaliges Anmelden][23] 


1. Klicken Sie auf der Registerkarte **Integration** führen Sie einen Bildlauf nach unten bis zum **3rd Party Integration**, und klicken Sie dann auf der Registerkarte **SAML 2.0** .

    ![Azure AD einmaliges Anmelden][24] 

1. Kopieren Sie den Wert unter **wird der SAML-Endoiint für Litmos:**.

    ![Azure AD einmaliges Anmelden][26] 


3. Führen Sie im Azure klassischen-Portal auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][8] 
 
    ein. Geben Sie die URL, die die Benutzer zum Anmelden an Ihrer Anwendung Litmos in das Textfeld **Bezeichner** (z. B.: *https://azureapptest.litmos.com/account/Login*).
     
    b. Fügen Sie im Textfeld **URL Antworten** des Werts, den Sie aus der Anwendung Litmos im vorherigen Schritt kopiert haben.

    c. Klicken Sie auf **Weiter**.
 
4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Litmos** führen Sie die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][2] 

    ein. Klicken Sie auf Zertifikat herunterladen, und speichern Sie die Datei auf Ihrem Computer.


1. Führen Sie in Ihrer Anwendung **Litmos** die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][25] 

    ein. Klicken Sie auf **SAML aktivieren**.

    b. Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

    >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    c. Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **SAML x 509-Zertifikat** .

    d. Klicken Sie auf **Änderungen speichern**.


6. Klicken Sie im Portal Azure AD-klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]


20. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen. 

    ![Konfigurieren Sie einmaliges Anmelden][12]


24. Klicken Sie im Dialogfeld **Attribut für Benutzer hinzufügen** führen Sie die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][14]

  	| Attributname | Attributwert |
  	| ---            | ---             |
  	| E-Mail          | User.Mail       |
  	| Vorname      | User.GivenName  |
  	| Nachname       | User.Surname    |

    Führen Sie für jede Datenzeile von in der Tabelle oben die folgenden Schritte aus:
   
    ein. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Konfigurieren Sie einmaliges Anmelden][15]


    ein. Geben Sie in das Textfeld **Name Attribut** das **Attributnamen** für diese Zeile angezeigt.

    b. Wählen Sie den **Wert Attribut** für diese Zeile angezeigt.

    c. Klicken Sie auf **abgeschlossen** , um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.


25. Klicken Sie auf **Änderungen anzuwenden**. 

    ![Konfigurieren Sie einmaliges Anmelden][16]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Portal Azure Clasic**, klicken Sie auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als **Typ des Benutzers** **neuen Benutzer in Ihrer Organisation**ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Erstellen eines Testbenutzers Litmos

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in Litmos bezeichnet.  
Die Anwendung Litmos unterstützt Just-in-Time bereitgestellt. Dies bedeutet, wird ein Benutzerkonto automatisch bei Bedarf bei dem Versuch, den Zugriff auf die Anwendung, die mit der Option Access erstellt.

**Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in Litmos zu erstellen:**


1. Melden Sie sich auf der Website Ihres Unternehmens Litmos (z. B.: *https://azureapptest.litmos.com/account/Login*) als Administrator.

    ![Azure AD einmaliges Anmelden][21] 


1. Klicken Sie in der Navigationsleiste auf der linken Seite auf **Konten**.

    ![Azure AD einmaliges Anmelden][22] 


1. Klicken Sie auf der Registerkarte **Integration** .

    ![Azure AD einmaliges Anmelden][23] 


1. Klicken Sie auf der Registerkarte **Integration** führen Sie einen Bildlauf nach unten bis zum **3rd Party Integration**, und klicken Sie dann auf der Registerkarte **SAML 2.0** .

    ![Azure AD einmaliges Anmelden][24] 

1. Wählen Sie **Autogenerate Benutzer:**.

    ![Azure AD einmaliges Anmelden][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf Litmos erteilen.

![Benutzer zuweisen][200] 

**Um Britta Simon Litmos zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **Litmos**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel Litmos im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung Litmos angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png






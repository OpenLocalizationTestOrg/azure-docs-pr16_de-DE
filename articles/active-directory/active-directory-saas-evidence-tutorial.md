<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Evidence.com | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und Evidence.com konfigurieren."
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
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Lernprogramm: Azure-Active Directory-Integration in Evidence.com

Ziel dieses Lernprogramms ist zum Einrichten einmaliges Anmelden zwischen Azure Active Directory (AAD) und Evidence.com veranschaulichen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:
    
* Ein gültiges Microsoft Azure-Abonnement
* Aktiviert eine Evidence.com Abonnement für einmaliges Anmelden (e-Mail earlyaccess@evidence.com Wenn SAML-basierten für einmaliges Anmelden nicht aktiviert ist)

Am Ende dieses Lernprogramms, werden können einzelne melden Sie sich bei der Anwendung, die Verwendung der Access-Systemsteuerung AAD AAD Benutzer, die Sie Zugriff Evidence.com zugewiesen haben.

## <a name="add-evidencecom-to-your-directory"></a>Hinzufügen von Evidence.com zum Verzeichnis

In diesem Abschnitt wird beschrieben, wie Evidence.com als eine integrierte Anwendung in Azure Active Directory hinzugefügt werden.

**So aktivieren Sie die Integration der Anwendung nach Hinweisen auf:**

1.  Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)auf der linken Navigationsbereich auf **Active Directory**.

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

4.  Um den Katalog der Anwendung zu öffnen, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **eine Anwendung aus dem Katalog hinzufügen**.

5.  Geben Sie im Suchfeld **Evidence.com**ein.

6.  Wählen Sie im Ergebnisbereich **Evidence.com aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.


## <a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

In diesem Abschnitt werden, wie Benutzer authentifizieren Evidence.com mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden können.

**Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:**

1.  Nach dem Hinzufügen Evidence.com im klassischen Azure-Portal an, klicken Sie auf **Konfigurieren einmaliges Anmelden**. 
 
2.  Klicken Sie auf dem nächsten Bildschirm **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

3.  Geben Sie im Bildschirm Konfigurieren der App-URL die URL ein, in dem Benutzer in Ihrem Mandanten-URL Evidence.com melden (Beispiel: https://yourtenant.evidence.com, und klicken Sie dann auf **Weiter**. 

4.  Klicken Sie auf den Link **Zum Herunterladen** , und speichern Sie es auf einem lokalen Laufwerk. Dieses Zertifikat und die URLs Metadaten (Entität-ID, melden Sie sich In URL SSO und melden Sie sich ab URL) werden zum Einrichten von SSO auf Evidence.com Website verwendet werden. 

5.  In einem separaten Webbrowserfenster, melden Sie sich Ihre Evidence.com Mandanten als Administrator und navigieren Sie zur **Registerkarte**
      
6.  Klicken Sie auf die **Stelle für einmaliges Anmelden**
 
7.  Wählen Sie **SAML Single Sign Grundlage**
 
8.  Kopieren Sie die **URL des Herausgebers**, **Single Sign On** und **Einzelnen Abmelden** Werte in der klassischen Azure-Portal und in die entsprechenden Felder in Evidence.com dargestellt.

9.  Öffnen des Zertifikats heruntergeladen haben, klicken Sie in Schritt 4 mit einem Texteditor wie Notepad.exe, und kopieren und Einfügen des Inhalts in das Feld **Sicherheitszertifikat** . 

10. Speichern Sie die Konfiguration in Evidence.com.
 
11. Aktivieren Sie im Portal Azure klassischen **bestätigen, die Sie einmaliges Anmelden wie oben beschrieben konfiguriert haben**. Wenn diese Option aktiviert, aktivieren Sie das aktuelle Zertifikat für diese Anwendung einbinden Arbeit beginnen.
 
12. Klicken Sie auf der Bestätigungsseite für einzelne anmelden auf **abgeschlossen**.  


## <a name="creating-an-evidencecom-test-user"></a>Erstellen eines Benutzers mit Evidence.com testen

Für Azure AD-Benutzer anmelden können müssen sie für den Zugriff innerhalb der Evidence.com Anwendung bereitgestellt werden. In diesem Abschnitt beschrieben, wie Sie Benutzerkonten innerhalb Evidence.com Azure AD-erstellen

**Bereitstellen eines Benutzerkontos in Evidence.com:**

1.  In einem Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Evidence.com als Administrator.

2.  Navigieren Sie zur **Registerkarte** .

3.  Klicken Sie auf **Benutzer hinzufügen**.

4.  Klicken Sie auf die Schaltfläche **Hinzufügen** .

5.  Die **E-Mail-Adresse** des hinzugefügten Benutzers muss der Benutzername, der Benutzer in Azure AD übereinstimmen, die Sie Zugriff gewähren möchten. Wenn Sie der Benutzernamen und e-Mail-Adresse nicht den gleichen Wert in Ihrer Organisation sind, können Sie mithilfe der **Evidence.com > Attribute > einmaliges Anmelden** Abschnitt des klassischen Azure Portals so ändern Sie die Nameidenitifer an Evidence.com gesendet werden, werden die e-Mail-Adresse.


## <a name="assigning-users-to-evidencecom"></a>Zuweisen von Benutzern zu Evidence.com

Für Benutzer von bereitgestellte AAD Evidence.com auf ihre Access-Systemsteuerung sehen können müssen sie Access in der klassischen Azure-Portal zugewiesen werden.

**So weisen Benutzer zu Evidence.com:**

1.  Klicken Sie auf der Seite Schnellstart für Evidence.com im klassischen Azure-Portal auf **Evidence.com Benutzern zuweisen**.
 
2.  Wählen Sie im Menü **Anzeigen** , ob Sie verwenden möchten, weisen Sie einen Benutzer oder eine Gruppe zu Evidence.com, und klicken Sie auf die Schaltfläche mit dem Häkchen.
 
3.  Wählen Sie in der Liste der **Benutzer** die Benutzer in der Gruppe ein, die Sie Evidence.com zuweisen möchten.
 
4.  Klicken Sie in der Fußzeile der Seite auf die Schaltfläche **zuweisen** .


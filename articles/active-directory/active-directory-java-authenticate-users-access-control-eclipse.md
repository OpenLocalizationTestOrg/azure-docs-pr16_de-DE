<properties
    pageTitle="So verwenden Sie die Steuerung des Benutzerzugriffs (Java) | Microsoft Azure"
    description="Informationen Sie zum Entwickeln und Verwenden von Access Control mit Java in Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>So Web Benutzerauthentifizierung mit Azure Access Control Service "Ellipse" verwenden

Dieses Handbuch zeigt Ihnen, wie die Azure Access Control Service (ACS) innerhalb der Azure-Toolkit für "Ellipse" verwendet werden soll. Weitere Informationen zu ACS finden Sie unter Abschnitt [Weitere Schritte](#next_steps) .

> [AZURE.NOTE]
> Den Azure Access Services Steuerelement Filter handelt es sich um eine Community Technology Preview. Als Vorabversion der Software ist es formell von Microsoft nicht unterstützt.

## <a name="what-is-acs"></a>Was ist ACS?

Die meisten Entwickler sind nicht Identität Experten und im Allgemeinen nicht Zeit entwickeln und-Autorisierungsmechanismen für ihre Anwendungen und Dienste aufwenden möchten. ACS ist ein Azure-Dienst, der eine einfache Möglichkeit bietet der Authentifizierung von Benutzern, die von Webanwendungen und Dienstleistungen Faktor komplexe Authentifizierungslogik in Ihren Code ohne Zugriff auf die entsprechenden.

Die folgenden Features sind in ACS verfügbar:

-   Integration in Windows-Identität Foundation (WIF).
-   Unterstützung für gängige Web Identitätsanbieter (IP-Adressen) einschließlich Windows Live ID, Google, Yahoo! und Facebook.
-   Unterstützung für Active Directory Federation Services (AD FS) 2.0.
-   Ein Open Data Protocol (OData)-basierten Verwaltungsdienst, die programmgesteuerten Zugriff auf ACS-Einstellungen ermöglicht.
-   Ein Verwaltungsportal, die Administratorzugriff auf die ACS-Einstellungen ermöglicht.

Weitere Informationen zu ACS finden Sie unter [Access Steuerelement Service 2.0][].

## <a name="concepts"></a>Konzepte

Azure ACS basiert auf die Hauptbenutzer anspruchsbasierte Identität - eine einheitliche Vorgehensweise Authentifizierungsmechanismen für lokale-Anwendungen oder in der Cloud erstellen. Anspruchsbasierte Identität bietet relativem für Anwendungen und Dienste, die Identitätsinformationen benötigten Benutzer in ihrer Organisation, die in anderen Organisationen und im Internet zu erhalten.

Wenn Sie die folgenden Aufgaben abgeschlossen haben, sollten Sie die folgenden Konzepte verstehen:

**Client** - kommt im Kontext dieses schrittweise Anleitung, einen Browser, der versucht, den Zugriff auf Ihre Web-Anwendung

**Relying Party (RP) Anwendung** – ein RP-Anwendung ist eine Website oder einen bestimmten Dienst, die mit dem Konfigurieren Authentifizierung, um eine externe Zertifizierungsstelle beauftragt. In Identität Jargon sagen wir, dass die RP dieser Zertifizierungsstelle vertrauen. Mit diesem Leitfaden wird erläutert, wie die Trust ACS konfiguriert.

**Token** - ein Token ist eine Sammlung von Sicherheitsdaten, die in der Regel nach der erfolgreichen Authentifizierung eines Benutzers ausgestellt wurde. Es enthält eine Reihe von *Ansprüchen*, Attribute den authentifizierten Benutzer. Ein Anspruch kann stehen für den Namen des Benutzers, ein Bezeichner für eine Rolle ein Benutzer eines Benutzers Alter und usw. gehört. Ein Token ist normalerweise digital signiert, d. h., können Sie immer wieder in deren Herausgeber bestellt werden, und deren Inhalt kann nicht manipuliert werden. Ein Benutzer erhält Zugriff auf eine Anwendung RP durch ein gültiges Token ausgestellt von einer Zertifizierungsstelle, die der RP-Anwendung vertraut präsentieren.

**Identität Anbieter (IP)** - eine IP ist eine Zertifizierungsstelle, die authentifiziert Benutzeridentitäten und Sicherheitstokens Probleme. Das eigentliche ausgeben Token wird zwar einen besonderen Dienst aufgerufen Security Token Service (STS) implementiert werden. Typische Beispiele für IP-Adressen sind Windows Live ID, Facebook, Business-Benutzers Repositorys (wie Active Directory) und So weiter.
Wenn ACS eine IP-Adresse vertrauen konfiguriert ist, wird das System akzeptieren und Token ausgestellt von dieser IP-Adresse überprüfen. ACS vertrauen kann mehrere IP-Adressen gleichzeitig, d. h., wenn eine Anwendung ACS vertraut, Sie sofort Ihrer Anwendung und alle authentifizierten Benutzer aus allen IP-Adressen anbieten können, dass ACS vertraut in Ihrem Auftrag.

**Föderation Anbieter (FP)** - IP-Adressen kennen die Benutzer mit ihren Anmeldeinformationen zu authentifizieren und Emission Ansprüche, was sie über diese wissen. Eine Föderation Anbieter (FP) ist eine andere Art von Zertifizierungsstelle: anstatt direkt Authentifizieren von Benutzern, sie fungiert als Vermittler und Makler Authentifizierung zwischen einem RP und eine oder mehrere IP-Adressen. Sowohl IP-Adressen und f/s Emission Sicherheitstokens, daher beide Sicherheit Token Services (STS) verwenden. ACS ist eine FP.

**ACS-Regel-Engine** – die Logik verwendet, um eingehende Token von vertrauenswürdigen IP-Adressen, die Token auffällt von der RP genutzt werden transformieren kodifiziert wird in Form eines einfachen Ansprüche Transformationsregeln. ACS verfügt über eine Regel-Engine, die sorgt dafür, dass Sie für Ihre RP angegeben Transformationslogik anwenden.

**Namespace des ACS** - einen Namespace ist einem auf oberster Ebene Teil ACS, die Sie zum Organisieren Ihrer Einstellungen verwenden. Ein Namespace enthält eine Liste der IP-Adressen Sie vertrauen, RP Programme, die durch dienen soll, die Regeln, dass Sie die Regel erwarten-engine zum Verarbeiten von eingehender Tokens mit usw.. Ein Namespace macht verschiedenen Endpunkten, die zum Abrufen der ACS Ausführen ihrer Funktion durch die Anwendung und der Entwickler verwendet werden.

Die folgende Abbildung zeigt die Funktionsweise der ACS-Authentifizierung mit einer Web-Anwendung:

![ACS Datenflussdiagramm][acs_flow]

1.  Der Client (in diesem Fall einen Browser) fordert eine Seite aus der RP.
2.  Da die Anfrage noch nicht authentifiziert, die RP leitet den Benutzer zu der Zertifizierungsstelle, die ihr vertraut, d. h. ACS. Das ACS bietet den Benutzer die Wahl der IP-Adressen, die für diese RP angegeben wurden. Der Benutzer wählt die entsprechende IP-Adresse.
3.  Der Client wechselt zu der Seite für die IP-Adresse Authentifizierung, und der Aufforderung, melden Sie sich bei.
4.  Nachdem der Client (beispielsweise die Identität eingegebene Anmeldeinformationen) authentifiziert ist, stellt die IP-Adresse ein Sicherheitstoken an.
5.  Nachdem ein Sicherheitstoken ausgegeben wird, die IP-Adresse leitet den Client zum ACS und der Client sendet das Sicherheitstoken ausgestellt ACS durch die IP-Adresse.
6.  ACS überprüft, ob das Sicherheitstoken ausgestellt von Eingaben die Identität Ansprüche in dieses Token in der ACS-Engine Regeln, die Ausgabe Identitätsansprüche berechnet und Probleme ein neues Sicherheitstoken, das diese Ausgabe Ansprüche enthält die IP-Adresse.
7.  ACS leitet den Client der RP. Der Client sendet das neue Sicherheitstoken ausgestellt von ACS der RP. Die RP überprüft die Signatur auf dem Sicherheitstoken ausgestellt von ACS, überprüft der Ansprüche in diesem Token und gibt die Seite, die ursprünglich angefordert wurde.

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie die folgenden Aufgaben ausführen zu können, benötigen Sie Folgendes:

- Eine Java Developer Kit (JDK), V 1.6 oder höher.
- Ellipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden. 
- Eine Verteilung einer Java-basierte Webserver oder Anwendungsserver, wie z. B. Apache Tomcat, GlassFish, JBoss-Anwendungsserver oder Pier.
- ein Azure-Abonnement, die von <http://www.microsoft.com/windowsazure/offers/>erworben werden kann.
- Lassen Sie das Azure Toolkit für Ellipse, April 2014 oder höher. Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für "Ellipse"](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- Ein x. 509-Zertifikat zur Verwendung mit Ihrer Anwendung. Sie benötigen dieses Zertifikat in öffentlichen Zertifikat (CER) und Personal Information Exchange (. PFX)-Format. (Optionen für dieses Zertifikat erstellen werden später in diesem Lernprogramm beschrieben).
- Vertrautheit mit den Azure-Serveremulator und der Bereitstellung Techniken erläutert am [Erstellen einer Hallo Welt-Anwendung für Azure in "Ellipse"](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Erstellen eines ACS-Namespace

Um die Verwendung von Access Control Service (ACS) in Azure beginnen, müssen Sie einen Namespace ACS erstellen. Der Namespace stellt einen eindeutigen Bereich zum Adressieren ACS-Ressourcen in Ihrer Anwendung.

1. Melden Sie sich bei der [Azure-Verwaltungsportal][].
2. Klicken Sie auf **Active Directory**. 
3. Zum Erstellen eines neuen Access Control Namespaces klicken Sie auf **neu**, klicken Sie auf **App-Dienste**, klicken Sie auf **Access-Steuerelement**, und klicken Sie auf der **Symbolleiste erstellen**. 
4. Geben Sie einen Namen für den Namespace. Azure stellt sicher, dass der Name eindeutig ist.
5. Wählen Sie den Bereich, in dem der Namespace verwendet wird. Optimale Leistung zu erzielen verwenden Sie den Bereich, in dem Sie die Anwendung bereitstellen.
6. Wenn Sie mehr als ein Abonnement besitzen, wählen Sie das Abonnement, das Sie für den ACS-Namespace verwenden möchten.
7. Klicken Sie auf **Erstellen**.

Azure erstellt und aktiviert den Namespace. Warten Sie, bis der Status des neuen Namespaces **aktiv** , bevor Sie fortfahren ist. 

## <a name="add-identity-providers"></a>Hinzufügen von Identitätsanbieter

In dieser Aufgabe fügen Sie die IP-Adressen für die Authentifizierung mit Ihrer Anwendung RP verwenden. Aus Gründen der Vorführung dieser Aufgabe gezeigt, wie Windows Live als eine IP-Adresse hinzufügen, aber Sie konnte aufgeführt, die im Verwaltungsportal ACS-IP-Adressen verwenden.


1.  Im [Verwaltungsportal Azure][]- **Active Directory**klicken Sie auf, wählen Sie einen Access Control Namespace, und klicken Sie dann auf **Verwalten**. Der ACS-Verwaltungsportal wird geöffnet.
2.  Klicken Sie in den linken Navigationsbereich des Portals Management ACS- **Identitätsanbieter**auf.
3.  Windows Live ID ist standardmäßig aktiviert und kann nicht gelöscht werden. Für dieses Lernprogramms wird nur Windows Live ID verwendet. Dieser Bildschirm ist jedoch die Stelle, an der Sie andere IP-Adressen, hinzufügen können, indem Sie auf die Schaltfläche **Hinzufügen** .

Windows Live ID ist jetzt als eine IP-Adresse für den ACS-Namespace aktiviert. Als Nächstes geben Sie Ihre Java-Web-Anwendung (zu einem späteren Zeitpunkt erstellt werden) als eine RP.

## <a name="add-a-relying-party-application"></a>Hinzufügen einer sich verlassen Partei Anwendung

In dieser Aufgabe Konfigurieren Sie ACS, um Ihre Java-Web-Anwendung als gültige RP Anwendung erkannt.

1.  Klicken Sie auf der ACS-Verwaltungsportal auf **Relying Party Applications**.
2.  Klicken Sie auf der Seite **Verlassen Party Applications** auf **Hinzufügen**.
3.  Führen Sie auf der Seite **Verlassen Partei Anwendung hinzufügen** folgende Schritte aus:
    1.  Geben Sie im Feld **Name**den Namen der die RP aus. Geben Sie im Rahmen dieses Lernprogramms **Azure Web App**ein.
    2.  Wählen Sie im **Modus** **Geben Sie die Einstellungen manuell**.
    3.  Geben Sie im **Bereich**des gilt das Sicherheitstoken ausgestellt von ACS-URIS aus. Geben Sie für diese Aufgabe **http://localhost: 8080 /**.
        ![Partei Bereich für die Verwendung in Serveremulator verlassen][relying_party_realm_emulator]
    4.  Geben Sie unter **Zurückzukehren URL** die URL an die ACS das Sicherheitstoken zurückgibt. Geben Sie für diese Aufgabe **Http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying Party URL für die Verwendung in Serveremulator zurückzugeben.][relying_party_return_url_emulator]
    5.  Akzeptieren Sie die Standardwerte in den Rest der Felder ein.

4.  Klicken Sie auf **Speichern**.

Sie haben nun erfolgreich konfiguriert Ihrer Java-Web-Anwendung, wenn sie in der Azure-Serveremulator ausgeführt wird (bei http://localhost: 8080 /) eine RP in Ihrem ACS-Namespace sein. Erstellen Sie dann die Regeln, die ACS verwendet, um die RP Ansprüche zu verarbeiten.

## <a name="create-rules"></a>Erstellen von Regeln

In dieser Aufgabe definieren Sie Regeln, die steuern, wie Ansprüche aus IP-Adressen Ihrer RP übergeben werden. In diesem Leitfaden wird ACS, um die Eingabewerte anfordern Typen und Werte direkt in die Ausgabe Token zu kopieren, ohne filtern oder ändern sie einfach konfiguriert.

1.  Klicken Sie auf der Seite Verwaltungsportal ACS-Hauptfenster auf **Regelgruppen**.
2.  Klicken Sie auf der Seite **Gruppen Regel** auf **Regel-Standardgruppe für Azure Web App**.
3.  Klicken Sie auf der Seite **Regel Gruppe bearbeiten** auf **generieren**.
4.  Klicken Sie auf die **Regeln generieren: Standard Regel Gruppe Azure Web App** Seite, stellen Sie sicher, Windows Live ID aktiviert ist, und klicken Sie auf **generieren**.    
5.  Klicken Sie auf der Seite **Regel Gruppe bearbeiten** auf **Speichern**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Hochladen eines Zertifikats, die dem ACS-namespace

In dieser Aufgabe, die Sie Hochladen einer. PFX-Zertifikat, das sich token Anfragen der ACS-Namespace erstellte verwendet werden.

1.  Klicken Sie auf der Seite Verwaltungsportal ACS-Hauptfenster auf **Zertifikate und Schlüssel**.
2.  Klicken Sie auf der Seite **Zertifikate und Schlüssel** auf **Hinzufügen** über **Token Signieren**.
3.  Auf der Seite **Hinzufügen Token Signaturzertifikat oder Schlüssel** :
    1. Klicken Sie im Abschnitt **zur Verwendung für** auf **Partei Anwendung verlassen** , und wählen Sie **Azure Web App** (die Sie zuvor als den Namen der Anwendung sich verlassen Partei festgelegt).
    2. Wählen Sie im Abschnitt **Typ** **X 509-Zertifikat**aus.
    3. Klicken Sie im Abschnitt **Zertifikat** klicken Sie auf die Schaltfläche Durchsuchen, und navigieren Sie zu der x. 509-Zertifikatsdatei, die Sie verwenden möchten. Dies kann ich ein. PFX-Datei. Wählen Sie die Datei, klicken Sie auf **Öffnen**, und geben Sie dann im Textfeld **Kennwort** das Kennwort des Zertifikats. Beachten Sie, dass zu Testzwecken Sie ein selbst-signed-Zertifikat verwenden können. Um ein selbst signiertes Zertifikat erstellen möchten, verwenden Sie die Schaltfläche ' **neu** ' im Dialogfeld **ACS-Filter-Bibliothek** (siehe unten), oder verwenden Sie das **encutil.exe** -Programm von der [Website von Project][] des Azure Starter Kit für Java.
    4. Stellen Sie sicher, dass **Primärem** aktiviert ist. Ihrer Seite **Hinzufügen Token Signaturzertifikat oder Schlüssel** sollte etwa wie folgt aussehen.
        ![Hinzufügen von Token Signaturzertifikat][add_token_signing_cert]
    5. Klicken Sie auf **Speichern** , um Ihre Einstellungen speichern und schließen Sie die Seite **Token hinzufügen Signaturzertifikat oder -Taste** .

Als Nächstes überprüfen Sie die Informationen auf der Seite für die Integration der Anwendung, und kopieren Sie den URI, die Sie zum Konfigurieren einer Java-Web-Anwendung ACS verwenden müssen.

## <a name="review-the-application-integration-page"></a>Überprüfen Sie die Anwendungsintegration-Seite

Sie können alle Informationen und den Code erforderlich sind, um Ihre Java Webanwendung (die RP) für die Arbeit mit ACS auf der Seite Anwendungsintegration der ACS-Verwaltungsportal konfigurieren suchen. Sie benötigen diese Informationen, wenn Ihre Java-Web-Anwendung für partnerverbundkontakte Authentifizierung konfigurieren.

1.  Klicken Sie auf der ACS-Verwaltungsportal auf **Integration der Anwendung**.  
2.  Klicken Sie auf der Seite **Anwendungsintegration** auf **Login Seiten**.
3.  Klicken Sie auf der Seite **Login Seite Integration** auf **Azure Web App**.

In der **Login Seite Integration: Azure Web App** Seite, auf die URL in aufgeführten **Option 1: Link zu einer Seite Login ACS gehostete** in Ihre Java-Web-Anwendung verwendet werden. Sie benötigen diesen Wert, wenn Sie die Bibliothek Azure Access Steuerelement Services Filter an Ihrer Anwendung Java hinzufügen.

## <a name="create-a-java-web-application"></a>Erstellen einer Java-Webanwendung
1. In "Ellipse" auf das Menü klicken Sie auf **Datei**, klicken Sie auf **neu**und klicken Sie dann auf **Dynamische Web-Projekt**. (Wenn Sie nicht, **Dynamische Web-Projekt** als Projekt verfügbar aufgeführt sehen, nachdem Sie auf **Datei**, **neu**, dann gehen Sie folgendermaßen vor: Klicken Sie auf **Datei**, klicken Sie auf **neu**, klicken Sie auf **Projekt**, **Web**zu erweitern, klicken Sie auf **Dynamische Web-Projekt**, und klicken Sie auf **Weiter**.) Benennen Sie das Projekt **MyACSHelloWorld**Zwecken dieses Lernprogramms. (Stellen Sie sicher verwenden Sie diesen Namen, die nachfolgenden Schritte in diesem Lernprogramm erwartet der WAR-Datei MyACSHelloWorld genannt werden). Ihres Bildschirms werden ähnlich wie die folgende angezeigt:

    ![Erstellen eines Projekts Hallo Welt für ACS-exampple][create_acs_hello_world]

    Klicken Sie auf **Fertig stellen**.
2. Erweitern Sie innerhalb des "Ellipse" Projekt-Explorer-Ansicht **MyACSHelloWorld**aus. Mit der rechten Maustaste **Inhaltsordner**, klicken Sie auf **neu**, und klicken Sie dann auf **JSP-Datei**.
3. Benennen Sie die Datei **index.jsp**, klicken Sie im Dialogfeld **Neue JSP-Datei** . Behalten Sie den übergeordneten Ordner als MyACSHelloWorld/Inhaltsordner, wie im folgenden dargestellt:

    ![Hinzufügen einer Datei JSP ACS-Beispiel][add_jsp_file_acs]

    Klicken Sie auf **Weiter**.

4. Klicken Sie im Dialogfeld **JSP-Vorlage auswählen** wählen Sie **Neue JSP-Datei (html)** , und klicken Sie auf **Fertig stellen**.
5. Wenn die Datei index.jsp in "Ellipse" geöffnet wird, in den anzuzeigenden Text hinzufügen **ACS-Welt!** innerhalb des aktuellen `<body>` Element. Der aktualisierten `<body>` Inhalt sollte wie folgt angezeigt:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Speichern Sie index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Hinzufügen der Filters ACS-Bibliothek an Ihrer Anwendung

1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **MyACSHelloWorld**, klicken Sie auf **Pfad erstellen**und klicken Sie dann auf **Erstellen Pfad konfigurieren**.
2. Klicken Sie auf der Registerkarte **Bibliotheken** , klicken Sie im Dialogfeld **Java erstellen Pfad** .
3. Klicken Sie auf die **Bibliothek hinzufügen**.
4. Klicken Sie auf **Azure Access Steuerelement Services Filter (nach MS öffnen Tech)** , und klicken Sie dann auf **Weiter**. Das Dialogfeld **Azure Access Steuerelement Services Filter** wird angezeigt.  (Möglicherweise müssen Sie im Feld **Ort** einen anderen Pfad, je nachdem, wo Sie die Ellipse, installiert und die Versionsnummer konnte unterschiedlich, je nach Softwareupdates sein).

    ![Hinzufügen von Filtern ACS-Bibliothek][add_acs_filter_lib]

5. Mithilfe eines Browsers geöffnet, auf der Seite **Login Seite Integration** Verwaltungsportal, kopieren Sie die URL, die aufgeführt, die den **Option 1: Link zu einer Seite Login ACS gehostete** Feld, und fügen Sie ihn in das Feld **ACS-Authentifizierung Endpunkt** Rand des Dialogfelds "Ellipse".
6. Verwenden einen Browser zur Seite **Verlassen Partei Anwendung bearbeiten** des Verwaltungsportal geöffnet, kopieren Sie die URL in das Feld " **Bereich** " aufgelistet sind, und fügen Sie ihn in das Feld **Verlassen Partei Bereich** des Dialogfelds "Ellipse".
7. Innerhalb der im Dialogfeld "Ellipse" im Abschnitt **Sicherheit** , wenn Sie ein vorhandenes Zertifikat verwenden möchten, klicken Sie auf **Durchsuchen**, navigieren Sie zu dem Zertifikat zu verwenden, wählen Sie ihn aus, und klicken Sie auf **Öffnen**. Oder, wenn Sie ein neues Zertifikat erstellen, klicken Sie auf **neu** , um das Dialogfeld **Neues Zertifikat** anzeigen möchten, geben Sie dann das Kennwort, Name der CER-Datei, und Namen der PFX-Datei für das neue Zertifikat.
8. Aktivieren Sie **einbetten das Zertifikat in die WAR-Datei**ein. Das Zertifikat auf diese Weise einbetten schließt ihn ein in der Bereitstellung ohne dass Sie sie als Komponente manuell hinzufügen. (Wenn stattdessen Sie Ihr Zertifikat extern aus der WAR-Datei speichern müssen, Sie konnte das Zertifikat als Rolle Komponente hinzufügen und deaktivieren Sie **einbetten das Zertifikat in die WAR-Datei**.)
9. [Optional] Beibehalten **erfordern HTTPS-Verbindungen** aktiviert. Wenn Sie diese Option festgelegt haben, müssen Sie die Anwendung, die mit dem HTTPS-Protokoll zugreifen. Wenn Sie keine HTTPS-Verbindungen erfordern möchten, deaktivieren Sie diese Option.
10. Bereitstellung der Serveremulator werden Ihre Einstellungen **Azure ACS-Filter** ähnlich wie der folgende aussehen.

    ![Bereitstellung der Serveremulator Azure ACS-filtereinstellungen][add_acs_filter_lib_emulator]

11. Klicken Sie auf **Fertig stellen**.
12. Klicken Sie auf **Ja** , wenn für ein Dialogfeld mit dem Hinweis, dass eine Datei web.xml erstellt wird angezeigt.
13. Klicken Sie auf **OK** , um das Dialogfeld **Java erstellen Pfad** zu schließen.

## <a name="deploy-to-the-compute-emulator"></a>Bereitstellen Sie für die Serveremulator

1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **MyACSHelloWorld**, klicken Sie auf **Azure**und **Verpacken für Azure**klicken Sie dann auf.
2. Geben Sie **MyAzureACSProject** **Projektnamen ein**und klicken Sie auf **Weiter**.
3. Wählen Sie eine JDK und Anwendungsserver. (Diese Schritte sind in diesem Lernprogramm [Erstellen einer Hallo Welt-Anwendung für Azure in "Ellipse"](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ausführlich behandelt).
4. Klicken Sie auf **Fertig stellen**.
5. Klicken Sie auf die Schaltfläche **in Azure Emulator ausführen** .
6. Nachdem Sie in der Serveremulator Ihrer Java-Web-Anwendung gestartet wird, schließen Sie alle Instanzen Ihres Browsers, (, damit der ACS-Login Test die alle aktuellen Browsersitzungen nicht stören).
7. Führen Sie die Anwendung durch Öffnen <http://localhost:> 8080/MyACSHelloWorld/in Ihrem Browser ( <> oder/Https://localhost:8080/MyACSHelloWorld/Wenn Sie die **Verwendung von HTTPS-Verbindungen**überprüft). Sie werden aufgefordert, für eine Windows Live ID anmelden, und Sie sollten getroffen werden, um die Rückgabe-URL für Ihre sich verlassen Partei Anwendung angegeben.
99.  Wenn Sie die Anzeige Ihrer Anwendungs abgeschlossen haben, klicken Sie auf die Schaltfläche **Azure Emulator zurücksetzen** .

## <a name="deploy-to-azure"></a>Bereitstellen für Azure

Um Azure bereitstellen, müssen Sie sich zu verlassen Partei Bereichsnamen ändern und Rückgabe-URL für den ACS-Namespace.

1. Ändern Sie innerhalb der Azure-Verwaltungsportal, auf der Seite **Bearbeiten verlassen Partei Anwendung** **Bereich** um die URL Ihrer Website bereitgestellt werden. Ersetzen Sie **Beispiel** mit den DNS-Namen, die, den Sie für die Bereitstellung angegeben haben.

    ![Partei Bereich für die Verwendung in der Herstellung verlassen][relying_party_realm_production]

2. **Rückgabe-URL** die URL Ihrer Anwendung benutzerspezifisch ändern. Ersetzen Sie **Beispiel** mit den DNS-Namen, die, den Sie für die Bereitstellung angegeben haben.

    ![Rückgabe Partei-URL für die Verwendung in der Herstellung verlassen][relying_party_return_url_production]

3. Klicken Sie auf **Speichern** , um Ihre aktualisierte beantworten Partei Bereich zu speichern und URL-Änderungen zurückzukehren.
4. Halten Sie die Seite **Login Seite Integration** in Ihrem Browser öffnen, müssen Sie es bald kopieren.
5. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **MyACSHelloWorld**, klicken Sie auf **Pfad erstellen**und klicken Sie dann auf **Erstellen Pfad konfigurieren**.
6. Klicken Sie auf der Registerkarte **Bibliotheken** , klicken Sie auf **Azure Access Steuerelement Services Filter**, und klicken Sie dann auf **Bearbeiten**.
7. Mithilfe eines Browsers geöffnet, auf der Seite **Login Seite Integration** Verwaltungsportal, kopieren Sie die URL, die aufgeführt, die den **Option 1: Link zu einer Seite Login ACS gehostete** Feld, und fügen Sie ihn in das Feld **ACS-Authentifizierung Endpunkt** Rand des Dialogfelds "Ellipse".
8. Verwenden einen Browser zur Seite **Verlassen Partei Anwendung bearbeiten** des Verwaltungsportal geöffnet, kopieren Sie die URL in das Feld " **Bereich** " aufgelistet sind, und fügen Sie ihn in das Feld **Verlassen Partei Bereich** des Dialogfelds "Ellipse".
9. Innerhalb der im Dialogfeld "Ellipse" im Abschnitt **Sicherheit** , wenn Sie ein vorhandenes Zertifikat verwenden möchten, klicken Sie auf **Durchsuchen**, navigieren Sie zu dem Zertifikat zu verwenden, wählen Sie ihn aus, und klicken Sie auf **Öffnen**. Oder, wenn Sie ein neues Zertifikat erstellen, klicken Sie auf **neu** , um das Dialogfeld **Neues Zertifikat** anzeigen möchten, geben Sie dann das Kennwort, Name der CER-Datei, und Namen der PFX-Datei für das neue Zertifikat.
10. Beibehalten **einbetten das Zertifikat in die WAR-Datei** ausgecheckt ist, vorausgesetzt, das Zertifikat in die WAR-Datei einbetten möchten.
11. [Optional] Beibehalten **erfordern HTTPS-Verbindungen** aktiviert. Wenn Sie diese Option festgelegt haben, müssen Sie die Anwendung, die mit dem HTTPS-Protokoll zugreifen. Wenn Sie keine HTTPS-Verbindungen erfordern möchten, deaktivieren Sie diese Option.
12. Für eine Bereitstellung in Azure werden Ihre Einstellungen Azure ACS-Filter ähnlich wie der folgende aussehen.

    ![Azure Filter ACS-Einstellungen für die Herstellung Bereitstellung][add_acs_filter_lib_production]

13. Klicken Sie auf **Fertig stellen** , um das Dialogfeld **Bibliothek bearbeiten** zu schließen.
14. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für MyACSHelloWorld** zu schließen.
15. Klicken Sie in "Ellipse" auf die Schaltfläche **Veröffentlichen in Azure Cloud** . Reagieren Sie auf den Anweisungen, ähnlich wie im Abschnitt **zur Bereitstellung Ihrer Anwendungs auf Azure** des Themas [Erstellen einer Hallo Welt-Anwendung für Azure in Ellipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) fertig. 

Nach Ihrer Webanwendung bereitgestellt wurde, schließen Sie alle geöffneten Browsersitzungen, führen Sie die Webanwendung, und Sie werden aufgefordert, sich mit Windows Live ID-Anmeldeinformationen, gefolgt von an die Rückgabe-URL der Anwendung sich verlassen Partei gesendet werden.

Wenn Sie fertig sind mit Ihrer Anwendung ACS Hallo Welt, denken Sie daran, löschen die Bereitstellung (Sie können Informationen zum Löschen einer bereitstellungs im Thema [Erstellen einer Hallo Welt-Anwendung für Azure in "Ellipse"](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).


## <a name="a-namenextstepsanext-steps"></a><a name="next_steps"></a>Nächste Schritte

Eine Prüfung der der Security Assertion Markup Language (SAML) zurückgegebene ACS an Ihrer Anwendung finden Sie unter [wie der Azure-Dienst zurückgegebene SAML angezeigt][]. Zum weiteren Untersuchen Sie der ACS-Funktionalität und um komplexere Szenarien experimentieren möchten, finden Sie unter [Access Steuerelement Service 2.0][].

In diesem Beispiel wird auch die Option " **einbetten" das Zertifikat in die WAR-Datei** "verwendet. Diese Option vereinfacht das Zertifikat bereitstellen. Wenn Sie stattdessen Signaturzertifikats getrennt von Ihrem WAR-Datei speichern möchten, können Sie das folgende Verfahren:

1. Geben Sie in dem Abschnitt **Sicherheit** Rand des Dialogfelds **Azure Access Steuerelement Services Filter** **${Env. JAVA_HOME}/MyCert.cer** , und deaktivieren Sie **einbetten das Zertifikat in die WAR-Datei**. (Passen Sie mycert.cer aus, wenn des Namens der Zertifikatsdatei abweicht.) Klicken Sie auf **Fertig stellen** , um das Dialogfeld zu schließen.
2. Kopieren Sie das Zertifikat als Komponente in der Bereitstellung: In "Ellipse" des Projekt-Explorer **MyAzureACSProject**erweitern, mit der rechten Maustaste **WorkerRole1**, klicken Sie auf **Eigenschaften**, erweitern Sie **Azure Rolle**und klicken Sie auf **Komponenten**.
3. Klicken Sie auf **Hinzufügen**.
4. Innerhalb des Dialogfelds **Komponente hinzufügen** :
    1. Im Abschnitt **Importieren** :
        1. Verwenden Sie die Schaltfläche **Datei** , um das Zertifikat zu navigieren, die Sie verwenden möchten. 
        2. Wählen Sie für **Methode** **Kopieren**aus.
    2. Klicken Sie auf das Textfeld **Als Namen**und akzeptieren Sie den Namen.
    3. Im Abschnitt **Bereitstellen** :
        1. Wählen Sie für **Methode** **Kopieren**aus.
        2. Geben Sie für **Verzeichnis** **"% JAVA_HOME"**ein.
    4. Ihre **Komponente hinzufügen** Dialogfeld sollte ähnlich wie der folgende aussehen.

        ![Fügen Sie die Zertifikatkomponente][add_cert_component]

    5. Klicken Sie auf **OK**.

An diesem Punkt sollte das Zertifikat in der Bereitstellung enthalten sein. Beachten Sie, dass unabhängig davon, ob Sie das Zertifikat in die WAR-Datei einbetten oder als eine Komponente der Bereitstellung hinzufügen, müssen Sie das Zertifikat, die dem Namespace wie im Abschnitt [Hochladen ein Zertifikats, die dem ACS-Namespace][] beschrieben hochladen.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Hochladen eines Zertifikats, die dem ACS-namespace]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Project-website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Zum Anzeigen der Azure-Dienst zurückgegebene SAML]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 

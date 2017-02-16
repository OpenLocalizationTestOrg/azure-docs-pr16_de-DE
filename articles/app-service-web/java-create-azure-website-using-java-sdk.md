<properties 
    pageTitle="Erstellen Sie eine Web-App in Azure-App-Verwaltungsdienst mit dem Azure-SDK für Java" 
    description="Informationen Sie zum Erstellen einer Web App auf App-Verwaltungsdienst Azure programmgesteuert mit dem Azure-SDK für Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Erstellen Sie eine Web-App in Azure-App-Verwaltungsdienst mit dem Azure-SDK für Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>(Übersicht)

Diese exemplarische Vorgehensweise wird gezeigt, wie Erstellen einer Azure SDK für Java-Anwendung, die ein Web-App in [Azure-App-Verwaltungsdienst][]erstellt, und dann eine Anwendung darauf bereitstellen. Es besteht aus zwei Teilen:

- Teil 1 wird veranschaulicht, wie eine Java-Anwendung zu erstellen, die eine Web app erstellt wird.
- Teil 2 wird veranschaulicht, wie einen einfachen JSP "Hallo Welt" erstellen Anwendung, und klicken Sie dann verwenden einer FTP-Client für die Bereitstellung von Code auf App-Dienst.


## <a name="prerequisites"></a>Erforderliche Komponenten

### <a name="software-installations"></a>Software-Installationen

Der AzureWebDemo Anwendung-Code in diesem Artikel wurde geschrieben mit Azure Java SDK 0.7.0, über das [Web Platform Installer][] (WebPI) installiert werden kann. Darüber hinaus stellen Sie sicher, dass die neueste Version des [Azure-Toolkit für "Ellipse"][]verwenden. Nachdem Sie das SDK installiert haben, aktualisieren Sie die Abhängigkeiten im Projekt "Ellipse", indem Sie **Index aktualisieren** in **Maven Repositorys**ausgeführt und dann fügen Sie die neueste Version von jedes Paket im Fenster **Abhängigkeiten** erneut hinzu. Sie können die Version der installierten Software in "Ellipse" überprüfen, indem Sie auf **Hilfe > Installation Details**; Sie benötigen mindestens die folgenden Versionen:

- Verpacken für Microsoft Azure-Bibliotheken für Java 0.7.0.20150309
- Ellipse IDE für Java EE-Entwickler 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Erstellen und Konfigurieren der Azure Cloudressourcen

Bevor Sie damit beginnen, müssen Sie über ein aktives Azure-Abonnement verfügen und richten Sie einen Standardwert Active Directory (AD) auf Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Erstellen Sie eine lokales Active Directory (AD) in Azure

Wenn Sie nicht bereits eine Active Directory (AD) für Ihr Azure-Abonnement verfügen, melden Sie sich bei der [Azure klassischen Portal][] mit Ihrem Microsoft-Konto. Wenn Sie mehrere Abonnements verfügen, klicken Sie auf **Abonnements** , und wählen Sie das Standardverzeichnis für das Abonnement für dieses Projekt verwendet werden soll. Klicken Sie dann auf **Übernehmen** für zum Wechseln der Ansicht Abonnement.

1. Wählen Sie im Menü auf der linken Seite aus **Active Directory** . **Klicken Sie auf Neu > Directory > Erstellen Sie benutzerdefinierte**.

2. Wählen Sie im **Verzeichnis hinzufügen** **Neues Verzeichnis erstellen**.

3. Geben Sie im Feld **Name**einen Namen ein.

4. Geben Sie unter **Domäne**einen Domänennamen ein. Dies ist eine grundlegende Domänennamen, der mit Ihrem Verzeichnis standardmäßig enthalten ist. Es hat das Format `<domain_name>.onmicrosoft.com`. Sie können einen Namen zu, basierend auf den Namen des Verzeichnisses oder einen anderen Domänennamen ein, die Sie besitzen. Später können Sie einen anderen Domänennamen hinzufügen, die Ihre Organisation bereits verwendet.

5. Wählen Sie im **Land oder Region**Ihr Gebietsschema aus.

Weitere Informationen zu AD finden Sie unter [Was ist ein Verzeichnis Azure AD-][]?


### <a name="create-a-management-certificate-for-azure"></a>Erstellen Sie ein Zertifikat Management für Azure

Java SDK Azure verwendet Management Zertifikate Authentifizierung mit Azure-Abonnements. Hierbei handelt es sich um x. 509 v3-Zertifikate, die Sie verwenden, um eine Clientanwendung authentifizieren, die die Management-API wird verwendet, um im Namen des Abonnements sind zum Verwalten von Abonnements Ressourcen dienen.

Der Code in diesem Verfahren wird ein selbst signiertes Zertifikat Authentifizierung mit Azure verwendet. Für dieses Verfahren müssen Sie ein Zertifikat erstellen und [Azure klassischen Portal][] im Voraus hochladen. Dies umfasst die folgenden Schritte aus:

- Generieren einer PFX-Datei, die Ihre Client-Zertifikat darstellt, und es lokal speichern.
- Erstellen Sie ein Zertifikat Management (CER-Datei) aus der PFX-Datei ein.
- Hochladen der CER-Datei zu Ihrem Azure-Abonnement.
- Konvertieren von der PFX-Datei in JKS, da Java dieses Format wird verwendet, um die Verwendung von Zertifikaten authentifizieren.
- Schreiben Sie den Code der Anwendung Authentifizierung, bezieht sich auf die lokale JKS-Datei ein.

Wenn Sie dieses Verfahren abschließen, das Zertifikat CER in Ihrem Abonnement Azure gespeichert sind, und das Zertifikat JKS auf dem lokalen Laufwerk gespeichert sind. Weitere Informationen zum Projektmanagement Zertifikate finden Sie unter [Erstellen und ein Zertifikat Management für Azure hochladen][].


#### <a name="create-a-certificate"></a>Erstellen eines Zertifikats

Klicken Sie zum Erstellen eines eigenen selbstsignierten Zertifikats öffnen Sie einen Befehl Console auf Ihr Betriebssystem, und führen Sie die folgenden Befehle.

> **Hinweis:**  Der Computer, auf dem Sie diesen Befehl ausführen, müssen JDK installiert. Darüber hinaus hängt der Pfad für die Keytool den Speicherort, in dem Sie das JDK installiert haben. Weitere Informationen finden Sie in der Java-Onlinedokumenten [Schlüssel und Zertifikat-Verwaltungstool (Keytool)][] .

So erstellen Sie die PFX-Datei

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

So erstellen Sie die CER-Datei

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

wobei Folgendes gilt:

- `<java-install-dir>`ist der Pfad zu dem Verzeichnis, in dem Sie Java installiert haben.
- `<keystore-id>`ist der Schlüsselspeicher Eintrag Bezeichner (zum Beispiel `AzureRemoteAccess`).
- `<cert-store-dir>`ist der Pfad zu dem Verzeichnis, in dem Zertifikate gespeichert werden soll (beispielsweise `C:/Certificates`).
- `<cert-file-name>`ist der Name der Zertifikatsdatei (zum Beispiel `AzureWebDemoCert`).
- `<password>`ist das Kennwort ein, das Sie zum Schutz des Zertifikats auswählen. Sie müssen mindestens 6 Zeichen lang sein. Obwohl dies nicht empfohlen wird, können Sie kein Kennwort eingeben.
- `<dname>`ist der x. 500 Distinguished Name Alias zugeordnet werden soll, und wie die Felder Herausgeber und Betreff der selbst signiertes Zertifikat verwendet wird.

Weitere Informationen finden Sie unter [Erstellen und ein Zertifikat Management für Azure hochladen][].


#### <a name="upload-the-certificate"></a>Das Zertifikat hochladen

Wenn Sie ein selbst signiertes Zertifikat in Azure hochladen, wechseln Sie zu der Seite **Einstellungen** im Portal klassischen und dann klicken Sie auf die Registerkarte **Verwaltungszertifikate** . Klicken Sie auf **Hochladen** , am unteren Rand der Seite, und navigieren Sie zum Speicherort der CER-Datei, die Sie erstellt haben.


#### <a name="convert-the-pfx-file-into-jks"></a>Konvertieren von der PFX-Datei in JKS

In der Windows-Befehlszeile (als Administrator ausgeführt), cd in das Verzeichnis, die die Zertifikate enthält, und führen den folgenden Befehl aus, wo `<java-install-dir>` Verzeichnis, in dem Sie Java auf Ihrem Computer installiert, ist:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Wenn Sie dazu aufgefordert werden, geben Sie das Ziel Keystore Kennwort ein; Dies ist das Kennwort für die Datei JKS wird.

2. Geben Sie bei Aufforderung das Kennwort des Quelle Keystore; Dies ist das Kennwort, das Sie für die PFX-Datei angegeben haben.

Die beiden Kennwörter müssen nicht identisch sein. Obwohl dies nicht empfohlen wird, können Sie kein Kennwort eingeben.


## <a name="build-a-web-app-creation-application"></a>Erstellen einer Web App Erstellung Anwendungs

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Erstellen Sie die Ellipse Arbeitsbereich und Maven Projekt

In diesem Abschnitt erstellen Sie einen Arbeitsbereich und ein Maven Projekt für das Web app Creation-Anwendung, mit dem Namen AzureWebDemo.

1. Erstellen eines neuen Projekts von Maven an. Klicken Sie auf **Datei > Neu > Maven Project**. Wählen Sie in **Maven Projekt** **Erstellen eines einfaches Projekts** und **Arbeitsbereich-Standardspeicherort verwenden**.

2. Geben Sie auf der zweiten Seite des **Neuen Maven Projekts**Folgendes ein:

    - Gruppen-ID:`com.<username>.azure.webdemo`
    - Element-ID: AzureWebDemo
    - Version: 0.0.1-SNAPSHOT
    - Verpacken: Jar
    - Name: AzureWebDemo

    Klicken Sie auf **Fertig stellen**.

3. Öffnen Sie das neue Projekt pom.xml Datei im Projekt-Explorer. Wählen Sie die Registerkarte **Abhängigkeiten** aus. Da es sich um ein neues Projekt handelt, werden keine Pakete noch aufgeführt.

4. Öffnen Sie die Ansicht Maven Repositorys. **Klicken Sie auf Fenster > Anzeigen > andere > Maven > Maven Repositorys** , und klicken Sie auf **OK**. Die Ansicht **Maven Repositorys** wird am unteren Rand der IDE angezeigt.

5. **Globale Repositorys**zu öffnen, mit der rechten Maustaste in der **zentralen** Repository gespeichert, und wählen Sie **Index neu erstellen**.

    ![][1]
    
    Dieser Schritt kann einige Minuten, je nach der Geschwindigkeit der Verbindung nehmen. Wenn der Index neu erstellt wird, sollten Sie die Microsoft Azure-Paketen in der **zentralen** Repository Maven sehen.

6. **Abhängigkeiten**klicken Sie auf **Hinzufügen**. Geben Sie in der **Gruppe der EINGABETASTE ID...** `azure-management`. Wählen Sie die Pakete für die Verwaltung von Basis und App Dienst Web Apps:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Hinweis:** Wenn Sie die Abhängigkeiten nach einer neuen Version Version aktualisieren, müssen Sie jede der Abhängigkeiten in dieser Liste wieder hinzufügen.
    > Nachdem Sie klicken Sie auf **Hinzufügen** , und wählen Sie jede einzelne Abhängigkeit, wird es mit der neuen Versionsnummer in der Liste der **Abhängigkeiten** angezeigt.

Klicken Sie auf **OK**. Die Azure Pakete werden dann in der Liste der **Abhängigkeiten** angezeigt.


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java-Code für eine Web App erstellen, indem Sie das Azure SDK schreiben

Schreiben Sie als Nächstes den Code, der in Azure Java SDK zum Erstellen des App-Dienst Web app-APIs Anrufe.

1. Erstellen Sie eine Java-Klasse, um den Haupteintrag Punkt-Code enthalten. Klicken Sie im Projekt-Explorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Neu > Class**.

2. Nennen Sie die Klasse in **Neue Java-Klasse**, `WebCreator` , und aktivieren Sie das Kontrollkästchen **Öffentliche statische void Main** . Die Auswahlmöglichkeiten sollte wie folgt aussehen:

    ![][2]

3. Klicken Sie auf **Fertig stellen**. Die Datei WebCreator.java wird im Projekt-Explorer angezeigt.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Aufrufen der Azure-API zum Erstellen einer App-Dienst Web App


#### <a name="add-necessary-imports"></a>Hinzufügen der erforderlichen Importe

Fügen Sie die folgenden Importe in WebCreator.java; Diese Importe ermöglichen den Zugriff auf Klassen in den Bibliotheken Management für die Verarbeitung von Azure-APIs:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definieren der Haupteintrag Point-Klasse

Da der Zweck der Anwendung AzureWebDemo im Erstellen einer App-Dienst Web App besteht, nennen Sie die wichtigste Klasse für diese Anwendung `WebAppCreator`. Diese Klasse stellt den Haupteintrag Punkt-Code, der die Azure Service Management-API zum Erstellen des Web app ruft an.

Fügen Sie die folgenden Parameterdefinitionen für das Web app und Webspace hinzu. Sie müssen Ihre eigenen Azure-Abonnement-ID und das Zertifikat Informationen bereitzustellen.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

wobei Folgendes gilt:

- `<subscription-id>`ist die Azure-Abonnement-ID, in dem Sie die Ressource erstellen möchten.
- `<certificate-store-path>`ist der Pfad und Dateinamen für die Datei JKS in Ihrem lokalen Zertifikat Store Verzeichnis. Beispielsweise `C:/Certificates/CertificateName.jks` für Linux und `C:\Certificates\CertificateName.jks` für Windows.
- `<certificate-password>`ist das Kennwort, die, das Sie angegeben haben, wenn Sie Ihr JKS Zertifikat erstellt haben.
- `webAppName`einen beliebigen Namen kann sein, die, den Sie auswählen. Dieses Verfahren wird der Name verwendet `WebDemoWebApp`. Der vollständigen Domänennamen ist die `webAppName` mit den `domainName` angefügt, in diesem Fall die vollständige Domäne ist `webdemowebapp.azurewebsites.net`.
- `domainName`sollte angegeben werden, wie oben gezeigt.
- `webSpaceName`einer der Werte in der Klasse [WebSpaceNames][] definiert sollte sein.
- `appServicePlanName`sollte angegeben werden, wie oben gezeigt.

> **Hinweis:** Jedes Mal, führen Sie diese Anwendung, müssen Sie den Wert ändern `webAppName` und `appServicePlanName` (oder löschen Sie das Web-app auf das Portal Azure) bevor Sie die Anwendung erneut ausführen. Andernfalls tritt Ausführung, da die gleiche Ressource Azure bereits vorhanden ist.


#### <a name="define-the-web-creation-method"></a>Definieren Sie die Web-Creation-Methode

Als Nächstes definieren Sie eine Methode, um das Web app erstellen. Diese Methode, `createWebApp`, gibt die Parameter der Web app und die Webspace an. Außerdem erstellt und konfiguriert den App-Dienst Web Apps Management-Client, der durch das Objekt [WebSiteManagementClient][] definiert ist. Der Management-Client, ist das Erstellen von Web Apps. Darüber Rest-Webdiensten, mit die Applikationen Web apps (Ausführen von Vorgängen wie erstellen, aktualisieren und löschen), indem Sie die Servicemanagement API verwalten können.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Der Code den HTTP-Status der Antwort erfolgreich war oder nicht ausgegeben wird, und bei einem erfolgreichen Abschluss ausgegeben wird der Name des erstellten Web app.


#### <a name="define-the-main-method"></a>Definieren der Main()-Methode

Stellen Sie den Code der main()-Methode, der createWebApp() zum Erstellen des Web app-Anrufe.

Rufen Sie abschließend `createWebApp` von `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Führen Sie die Anwendung, und überprüfen, ob Web app erstellt

Um zu überprüfen, dass die Anwendung ausgeführt wird, klicken Sie auf **Ausführen > Ausführen**. Wenn die Anwendung Ausführung abgeschlossen ist, sollten Sie die folgende Ausgabe in der Konsole "Ellipse" sehen:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Melden Sie sich bei dem klassischen Azure-Portal an, und klicken Sie auf **Web Apps**. Das neue Web app sollte innerhalb weniger Minuten in der Web Apps-Liste angezeigt werden.


## <a name="deploying-an-application-to-the-web-app"></a>Bereitstellen einer Anwendung zu Web App

Nachdem Sie AzureWebDemo ausführen und das neue Web app erstellt haben, melden Sie sich bei der klassischen Portal, klicken Sie auf **Web Apps**, und wählen Sie in der **Web Apps** -Liste **WebDemoWebApp** aus. Klicken Sie in der Web-app-Dashboard-Seite, auf **Durchsuchen** (oder klicken Sie auf die URL `webdemowebapp.azurewebsites.net`), um ihn zu. Da noch keine Inhalte in der Web-app veröffentlicht wurde, wird Sie eine Seite leere Platzhalter angezeigt.

Als Nächstes werden Sie eine Anwendung "Hallo Welt" erstellen und zur Web-app bereitstellen.


### <a name="create-a-jsp-hello-world-application"></a>Erstellen Sie eine Anwendung JSP Hallo Welt

#### <a name="create-the-application"></a>Erstellen Sie die Anwendung

Um so eine Anwendung im Web bereitstellen zu veranschaulichen, zeigt das folgende Verfahren zum Erstellen einer einfachen "Hallo Welt" Java-Anwendungs, und klicken Sie auf die App-Online, die Ihrer Anwendung erstellt hochladen können.

1. Klicken Sie auf **Datei > Neu > dynamische Web Project**. Nennen Sie es `JSPHello`. Sie müssen keine Einstellungen in diesem Dialogfeld ändern. Klicken Sie auf **Fertig stellen**.

    ![][3]

2. Klicken Sie im Projekt-Explorer, erweitern Sie das Projekt **JSPHello** , mit der rechten Maustaste **Inhaltsordner**und dann auf **Neu > JSP-Datei**. Klicken Sie im Dialogfeld neue JSP-Datei benennen Sie die neue Datei `index.jsp`. Klicken Sie auf **Weiter**.

3. Klicken Sie im Dialogfeld **JSP-Vorlage auswählen** wählen Sie **Neue JSP-Datei (html)** , und klicken Sie auf **Fertig stellen**.

4. In index.jsp, fügen Sie den folgenden Code in die `<head>` und `<body>` Kategorisieren von Abschnitten:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Führen Sie die Anwendung Hallo Welt im localhost

Bevor Sie diese Anwendung ausführen, müssen Sie ein paar Eigenschaften konfigurieren.

1. Mit der rechten Maustaste in des Projekts **JSPHello** , und wählen Sie **Eigenschaften**aus.

2. Im Dialogfeld **Eigenschaften** : Wählen Sie **Java erstellen Pfad**, wählen Sie die Registerkarte **Reihenfolge und Exportieren** , aktivieren Sie **JRE System Library**und dann klicken Sie auf **nach oben** Verschieben an den Anfang der Liste.

    ![][4]

3. Auch im Dialogfeld **Eigenschaften** : Wählen Sie **Ziel Laufzeiten** aus, und klicken Sie auf **neu**.

4. Klicken Sie im Dialogfeld **Neue Server Runtime-Umgebung** wählen Sie einen Server z. B. **Apache Tomcat 7.0** , und klicken Sie auf **Weiter**. Legen Sie im Dialogfeld **Tomcat Server** **Namen** auf `Apache Tomcat v7.0`, und legen Sie **Tomcat Installationsverzeichnis** zu dem Verzeichnis, in dem Sie die zu verwendende Tomcat-Server-Version installiert.

    ![][5]

    Klicken Sie auf **Fertig stellen**.

5. Sie zurückkehren, klicken Sie dann zu der Seite **Als Ziel Laufzeiten** im Dialogfeld **Eigenschaften** . Wählen Sie **Apache Tomcat 7.0**, und klicken Sie auf **OK**.

    ![][6]

6. Klicken Sie auf **Ausführen**, klicken Sie im Menü "Ellipse" **Ausführen** . Wählen Sie im Dialogfeld **Ausführen als** **Server ausgeführt**. Wählen Sie im Dialogfeld **Ausführen auf Server** **Tomcat 7.0 Server**aus:

    ![][7]

    Klicken Sie auf **Fertig stellen**.

7. Bei der Ausführung der Anwendungs, sollten Sie finden Sie unter der **JSPHello** -Seite, die in einem Fenster Localhost in "Ellipse" angezeigt werden (`http://localhost:8080/JSPHello/`), die folgende Meldung angezeigt:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Exportieren Sie die Anwendung als eine WAR

Exportieren Sie die Web-Projektdateien als eine Web-Archivdatei (WAR), damit es in der Web-app bereitstellen zu können. Die folgenden Web Project-Dateien befinden sich in den Ordner Inhaltsordner:

    META-INF
    WEB-INF
    index.jsp

1. Mit der rechten Maustaste in des Ordners Inhaltsordner und dann auf **Exportieren**.

2. Klicken Sie im Dialogfeld **Wählen Sie exportieren** auf **Web > WAR** Datei und dann auf **Weiter**.

3. Klicken Sie im Dialogfeld **WAR exportieren** wählen Sie das Src-Verzeichnis im aktuellen Projekt und fügen Sie den Namen der am Ende der WAR-Datei ein. Beispiel:

    `<project-path>/JSPHello/src/JSPHello.war`

Weitere Informationen zum Bereitstellen von WAR-Dateien finden Sie unter [Hinzufügen einer Java-Anwendung in Azure App Dienst Web Apps](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Bereitstellen der Hallo Welt Anwendung mithilfe von FTP

Wählen Sie aus einer Drittanbieter-FTP-Client So veröffentlichen Sie die Anwendung. Dieses Verfahren werden zwei Optionen zur Verfügung: der Kudu-Konsole integriert Azure; und FileZilla, eines beliebten Tools mit einer Benutzeroberfläche geeignete, grafische.

> **Hinweis:** Das Azure-Toolkit für "Ellipse" Bereitstellung Speicherkonten und Cloud Services unterstützt, aber unterstützt derzeit nicht Bereitstellung bei Web apps. Sie können auf Speicherkonten und Cloud-Diensten mit einer Azure-Bereitstellungsprojekt aus, wie bei der [Erstellung einer Hallo Welt Anwendung für Azure in "Ellipse"](http://msdn.microsoft.com/library/azure/hh690944.aspx)beschrieben, aber nicht bei Web apps bereitstellen. Verwenden Sie andere Methoden wie FTP oder GitHub Dateien auf Web app übertragen.

> **Hinweis:** Wir empfehlen nicht über FTP über die Windows-Befehlszeile (die Befehlszeile FTP.EXE-Programm, das mit Windows geliefert wird). Möglicher Fehler entwickelt über Firewalls häufig FTP-Clients, die aktive FTP-Verbindung, z. B. FTP.EXE, verwenden. Aktives FTP gibt eine interne LAN-basierten Adresse, die ein FTP-Server meist fehl Verbindung an.

Weitere Informationen zur Bereitstellung zu einer App-Dienst Web app mit FTP finden Sie unter den folgenden Themen:

- [Bereitstellen einer FTP-Programm verwenden](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Einrichten von Anmeldeinformationen für Bereitstellung

Stellen Sie sicher, dass Sie die Anwendung **AzureWebDemo** zum Erstellen einer Web app ausgeführt haben. Sie können Dateien an diesem Speicherort übertragen.

1. Melden Sie sich bei der klassischen Portal, und klicken Sie auf **Web Apps**. Stellen Sie sicher, dass **WebDemoWebApp** in der Liste der Web apps angezeigt wird, und stellen Sie sicher, dass er ausgeführt wird. Klicken Sie auf **WebDemoWebApp** , um deren **Dashboard** -Seite zu öffnen.

2. Klicken Sie auf der Seite **Dashboard** unter **Schnellen Blick**, klicken Sie auf **Ihre Anmeldeinformationen für die Bereitstellung einrichten** (Wenn bereits Bereitstellung Anmeldeinformationen liest **Zurücksetzen Ihrer Anmeldeinformationen für die Bereitstellung**).

    Bereitstellung Anmeldeinformationen stehen im Zusammenhang mit einem Microsoft-Konto. Sie müssen einen Benutzernamen und Ihr Kennwort ein, mit denen Sie mithilfe von Git und FTP bereitstellen, angeben. Diese Anmeldeinformationen können für die Bereitstellung auf eine beliebige Web app in allen Azure-Abonnements, die mit Ihrem Microsoft-Konto verknüpft ist. Bereitstellen Sie Git und FTP-Bereitstellung Anmeldeinformationen in das Dialogfeld, und zeichnen Sie den Benutzernamen und das Kennwort für eine zukünftige Verwendung.


#### <a name="get-ftp-connection-information"></a>Erhalten von Informationen zur FTP-Verbindung

Verwendung von FTP Anwendungsdateien in den neu erstellten Web app bereitstellen, müssen Sie Verbindungsinformationen zu erhalten. Es gibt zwei Methoden, um die Verbindungsinformationen zu erhalten. Eine Möglichkeit besteht darin, besuchen Sie **die Web-app Dashboardseite** ; umgekehrt ist zum Herunterladen im Web app Profil veröffentlichen. Das Veröffentlichungsprofil ist eine XML-Datei, die Informationen, wie etwa FTP-Host Name und die Anmeldeinformationen für Ihre Web apps in Azure-App-Verwaltungsdienst enthält. Dieser Benutzername und Kennwort können für die Bereitstellung auf eine beliebige Web app in alle Abonnements Azure-Konto, nicht nur diesen Termin zugeordnet.

Um FTP-Verbindungsinformationen aus der Web-app vorher in der [Azure-Portal][]zu erhalten:

1. Klicken Sie unter **Essentials**suchen Sie und kopieren Sie der **FTP-Hostname**. Dies ist eine ähnliche URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Klicken Sie unter **Essentials**suchen und **FTP-Bereitstellung/Benutzername**kopieren. Hat dies der Form *Webappname\deployment-Username*; beispielsweise `WebDemoWebApp\deployer77`.

So erhalten Sie Informationen zur FTP-Verbindung aus dem Veröffentlichungsprofil:

1. Klicken Sie in der Web-app-Blade auf **Get Profil veröffentlichen**. Dadurch wird eine PUBLISHSETTINGS-Datei auf die lokale Festplatte herunterladen.

2. Öffnen Sie die PUBLISHSETTINGS-Datei in einer XML-Editor oder Text-Editor, und Suchen nach dem `<publishProfile>` Element mit `publishMethod="FTP"`. Es sollte wie folgt aussehen:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Beachten Sie, dass des Web-app `publishProfile` Einstellungen Karte an den Einstellungen FileZilla Website-Manager wie folgt:

- `publishUrl`entspricht dem **FTP-Hostnamen ein**, der Wert, den Sie in das Feld **Host**festlegen.
- `publishMethod="FTP"`bedeutet, dass Sie **Protokoll** **FTP - File Transfer Protocol**und **Verschlüsselung** mit **einfarbigen FTP**festgelegt.
- `userName`und `userPWD` sind die Tasten für den ist-Benutzernamen und das Kennwortwerte, die Sie angegeben haben, wenn Sie die Anmeldeinformationen für die Bereitstellung zurücksetzen. `userName`entspricht dem **Bereitstellung / FTP-Benutzer**. Sie eine Zuordnung zu **Benutzername** und **Kennwort** in FileZilla.
- `ftpPassiveMode="True"`bedeutet, dass die FTP-Site passiven FTP-Übertragung verwendet. Wählen Sie auf der Registerkarte **Einstellungen übertragen** **passiven** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurieren der Web-App, um eine Java-Anwendung zu hosten.

Bevor Sie die Anwendung veröffentlichen, müssen Sie ein Paar Konfiguration Einstellungen zu ändern, sodass die Web app eine Java-Anwendung hosten kann.

1. Im Portal klassischen wechseln Sie zu der Web-app- **Dashboard** -Seite, und klicken Sie auf **Konfigurieren**. Geben Sie auf der Seite **Konfigurieren** die folgenden Einstellungen aus.

2. In **Java-Version** wird standardmäßig **Off**verwendet; Wählen Sie der Java-Version Ihrer Anwendung Ziele; beispielsweise 1.7.0_51. Nachdem Sie dies tun, außerdem sicherstellen Sie, dass **Webcontainer** auf eine Version von Tomcat Server festgelegt ist.

3. In **Dokumenten Standard**-index.jsp hinzufügen und nach Zeitphasen bis zum Anfang der Liste zu verschieben. (Die Standarddatei von Web apps ist hostingstart.html.)

4. Klicken Sie auf **Speichern**.


#### <a name="publish-your-application-using-kudu"></a>Veröffentlichen Sie die Anwendung mit Kudu

Eine Methode, um die Anwendung veröffentlichen besteht darin, die Kudu Debuggen Konsole integriert Azure verwenden. Kudu wird der bekanntermaßen unveränderliche und mit App-Dienst Web Apps und Tomcat Server konsistent sein. Sie öffnen die Verwaltungskonsole für die Web-app unter einer URL im folgenden Format:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Für dieses Verfahren befindet sich die Kudu Console unter dem folgenden URL; Navigieren Sie zu folgendem Speicherort:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Wählen Sie im oberen Menü **Debuggen Console > CMD**.

3. Navigieren Sie zu der Console-Befehlszeile `/site/wwwroot` (oder klicken Sie auf `site`, dann `wwwroot` in der Verzeichnisansicht am oberen Rand der Seite):

    `cd /site/wwwroot`

4. Nachdem Sie **Java-Version**angegeben haben, sollte Tomcat Server ein Verzeichnis Webapps erstellen. Navigieren Sie in der Zeile Console-Befehl zum Webapps Verzeichnis:

    `mkdir webapps`

    `cd webapps`

5. Ziehen Sie aus JSPHello.war `<project-path>/JSPHello/src/` und legen Sie sie in die Kudu Directory Ansicht unter `/site/wwwroot/webapps`. Ziehen Sie es nicht in den Bereich "Hier ziehen, um Sie hochzuladen und zip" fest, da es Tomcat Entzippen wird.

  ![][8]

Erste JSPHello.war Dokumentbereichs allein im Bereich Directory:

  ![][9]

Tomcat Server wird die WAR-Datei in einem entpackt JSPHello Verzeichnis entzippen Sie in kurzer Zeit (wahrscheinlich kleiner als 5 Minuten). Klicken Sie auf das Stammverzeichnis, um festzustellen, ob index.jsp entpackt haben und es kopiert wurde. In diesem Fall navigieren Sie wieder zum Webapps Verzeichnis zu sehen, ob entpackte JSPHello Verzeichnis erstellt wurde. Wenn Sie diese Elemente nicht angezeigt werden, warten Sie, und wiederholen.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Veröffentlichen Sie die Anwendung mithilfe von FileZilla (optional)

Ein weiteres Tool, die, das Sie verwenden können, um die Anwendung veröffentlichen, ist FileZilla, beliebte Drittanbieter-FTP-Client für eine geeignete, grafisch-Benutzeroberfläche an. Sie können herunterladen und Installieren von FileZilla aus [http://filezilla-project.org/](http://filezilla-project.org/) , wenn Sie noch nicht es verfügen. Weitere Informationen zum Verwenden des Clients finden Sie unter der [Dokumentation FileZilla](https://wiki.filezilla-project.org/Documentation) und diesen Blog-Eintrag auf [FTP-Clients - Teil 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Klicken Sie im FileZilla, auf **Datei > Website-Manager**.
2. Klicken Sie im Dialogfeld **Website-Manager** auf **Neue Website**. **Wählen Sie Posten** Sie aufgefordert werden, geben Sie einen Namen wird eine neue, leere FTP-Website angezeigt. Für dieses Verfahren nennen Sie es `AzureWebDemo-FTP`.

    Geben Sie auf der Registerkarte **Allgemein** die folgenden Einstellungen aus:
    - **Host:** Geben Sie den **FTP-Hostnamen ein** , den Sie aus dem Dashboard kopiert haben.
    - **Port:** (Lassen Sie dieses Feld leer ist, wie dies eine Übertragung Passiv ist und der Server den Port bestimmt zu verwenden.)
    - **Protokoll:** FTP-File Transfer Protocol
    - **Verschlüsselung:** Einfarbigen FTP verwenden
    - **Typ Anmeldeinformationen:** Normal
    - **Benutzer:** Geben Sie die Bereitstellung / FTP-Benutzer, den Sie in dem Dashboard kopiert haben. Dies ist die vollständige FTP-Benutzername, die im Formular *Webappname\username*hat.
    - **Kennwort:** Geben Sie das Kennwort, das Sie angegeben haben, wenn Sie die Bereitstellung Anmeldeinformationen festlegen.

    Wählen Sie auf der Registerkarte **Einstellungen übertragen** **passiven**ein.

3. Klicken Sie auf **Verbinden**. Wenn erfolgreich, FileZillas Console angezeigt werden eine `Status: Connected` Nachricht und Problem einer `LIST` Befehl, um den Inhalt des Verzeichnisses aufzulisten.

4. Wählen Sie im Bereich **lokale** Website Source-Verzeichnis, in dem sich die Datei JSPHello.war befindet; der Pfad werden ähnlich wie der folgende aus:

    `<project-path>/JSPHello/src/`

5. Wählen Sie im Bereich **Remote** -Website den Zielordner aus. Sie werden die WAR-Datei zum Bereitstellen der `webapps` Verzeichnis, unter der Web-app aus. Navigieren Sie zu `/site/wwwroot`, mit der rechten Maustaste auf `wwwroot`, und aktivieren Sie **Erstellen**. Benennen Sie das Verzeichnis `webapps` , und geben Sie dieses Verzeichnis.

6. Übertragen von JSPHello.war zu `/site/wwwroot/webapps`. Wählen Sie in der Liste der **lokalen** Datei JSPHello.war aus, mit der rechten Maustaste darauf, und wählen Sie **Hochladen**aus. Auftreten er angezeigt in `/site/wwwroot/webapps`.

7. Nach dem Kopieren der JSPHello.war in das Verzeichnis Webapps Tomcat-Server automatisch entpackt (Entzippen) die Dateien in die WAR-Datei. Obwohl Tomcat Server Entpacken beinahe sofort beginnt, es kann eine lange dauern Anzeigedauer (oftmals Stunden) für die Dateien im FTP-Client angezeigt werden.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Führen Sie die Hallo Welt Anwendung auf das Web App

1. Nachdem Sie die WAR-Datei hochgeladen haben, und überprüft, dass Tomcat Server ein entpackt erstellt hat `JSPHello` durchsuchen, um im Verzeichnis `http://webdemowebapp.azurewebsites.net/JSPHello` zum Ausführen der Anwendung.

    > **Hinweis:** Wenn Sie vom klassischen-Portal auf **Durchsuchen** klicken, wird möglicherweise eine wie die Standard-Webseite, sagen "Diese Java-basierte Web-Anwendung erfolgreich erstellt wurde." Möglicherweise müssen die Webseite zu aktualisieren, um die Anwendungsausgabe anstelle der Standard-Webseite anzeigen zu können.

2. Wenn die Anwendung ausgeführt wird, sollten Sie eine Webseite mit der folgenden Ausgabe sehen:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Azure Ressourcen bereinigen

Diese Prozedur erstellt eine App-Dienst Web app an. Solange sie vorhanden ist, werden Sie für die Ressource berechnet. Es sei denn, Sie weiterhin verwenden möchten, das Web app zum Testen oder Entwicklung, sollten Sie erwägen, beenden oder gelöscht. Eine Web-app, die beendet wurde entstehen immer noch eine kleine Gebühr, jedoch können Sie es zu einem beliebigen Zeitpunkt starten. Löschen einer Web app löscht alle Daten, die Sie damit hochgeladen haben.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure App-Verwaltungsdienst]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure-Toolkit für "Ellipse"]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klassischen-portal]: https://manage.windowsazure.com
[Was ist ein Verzeichnis Azure AD-]: http://technet.microsoft.com/library/jj573650.aspx
[Erstellen Sie und Hochladen Sie einer Management Zertifikat für Azure]: ../cloud-services/cloud-services-certs-create.md
[Schlüssel und Zertifikat-Verwaltungstool (Keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure-Portal]: https://portal.azure.com

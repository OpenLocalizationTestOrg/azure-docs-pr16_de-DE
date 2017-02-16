<properties
    pageTitle="Erstellen und Bereitstellen einer Java-API-app in Azure-App-Verwaltungsdienst"
    description="Informationen Sie zum Erstellen einer Java-API-app-Pakets und auf App-Verwaltungsdienst Azure bereitgestellt."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Erstellen und Bereitstellen einer Java-API-app in Azure-App-Verwaltungsdienst

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

In diesem Lernprogramm erfahren So erstellen eine Java-Anwendung, und klicken Sie auf Azure App Service-API Apps bereitgestellt [Git]verwenden. Die Anweisungen in diesem Lernprogramm können auf einem beliebigen Betriebssystem folgen, die zur Ausführung von Java ist. Der Code in diesem Lernprogramm wird mithilfe von [Maven]erstellt. [JAX-RS] wird verwendet, um die Rest-Diensts erstellen, und wird auf der Grundlage der Metadatenspezifikation [Swagger] mit dem [Editor Swagger]generiert.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Java-Entwickler Kit 8] \(oder höher)
1. [Maven] auf Ihrem Entwicklungscomputer installiert
1. [Git] auf Ihrem Entwicklungscomputer installiert
1. Ein kostenpflichtiges oder [kostenlose Testversion] [Microsoft Azure] -Abonnement
1. Eine HTTP-Test-Anwendung wie [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Die API mit Swagger.IO scaffold

Mit dem Editor für swagger.io online können Sie Swagger JSON oder YAML Code, die die Struktur Ihrer API darstellt eingeben. Nachdem Sie die API-Oberfläche entworfen haben, können Sie den Code für eine Vielzahl von Plattformen und Framework exportieren. Im nächsten Abschnitt wird der scaffolded Code simulierten Funktionen einbeziehen angepasst werden. 

In dieser Demo beginnt mit einer Swagger JSON-Text, die Sie in den swagger.io-Editor eingefügt werden soll, die dann zum Generieren von Code JAX-RS verwendet, einen Endpunkt REST-API zugreifen verwendet wird. Klicken Sie dann bearbeiten den scaffolded Code um simulierten Daten zurückgeben Sie simulieren eines REST-API über eine Dauerhaftigkeitsmechanismus Daten erstellt.  

1. Kopieren Sie den folgenden Swagger JSON-Code in die Zwischenablage:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Navigieren Sie zu der [Online Swagger Editor]. Klicken Sie einmal vorhanden, auf das Menüelement **Einfügen JSON-Datei >** .

    ![JSON-Menüelement einfügen][paste-json]

1. Fügen Sie die Kontakte Liste API Swagger JSON Sie zuvor kopiert haben. 

    ![JSON-Code in Swagger einfügen][pasted-swagger]

1. Anzeigen der Dokumentationsseiten und API Zusammenfassung im Editor gerendert. 

    ![Ansicht Swagger generiert Dokumente][view-swagger-generated-docs]

1. Wählen Sie die Menüoption **generieren Server-> JAX-RS** zu scaffold des serverseitigen Codes, die Sie später bearbeiten können, um simulierten Implementierung hinzufügen. 

    ![Menüelement Code generieren][generate-code-menu-item]

    Nachdem der Code generiert wird, erhalten Sie eine ZIP-Datei herunterladen bereitgestellt werden. Diese Datei enthält den Code, der vom Code-Generator Swagger Gerüstbau und alle zugeordneten Skripts erstellen. Entzippen Sie die gesamte Bibliothek in ein Verzeichnis auf Ihrer Entwicklungsarbeitsstation. 

## <a name="edit-the-code-to-add-api-implementation"></a>Bearbeiten Sie den Code zum Hinzufügen von API-Implementierung

In diesem Abschnitt werden Sie durch den benutzerdefinierten Code der Swagger generierte Code serverseitige Implementierung ersetzen. Der neue Code wird eine ArrayList des Kontakts Personen an den einen Client zurück. 

1. Öffnen Sie die Modelldatei *Contact.java* , die sich im Ordner *Src/Generation/Java/EA/Swagger/Modell* befindet, mit [Visual Studio-Code] oder Ihrem bevorzugten Texteditor ein. 

    ![Kontakt Modelldatei öffnen][open-contact-model-file]

1. Fügen Sie den folgenden Konstruktor der Klasse **Kontakt** ein. 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Öffnen Sie die *ContactsApiServiceImpl.java* Implementierung Dienstdatei, die mit [Visual Studio-Code] oder Ihrem bevorzugten Texteditor im Ordner *Src/primär/Java/EA/Swagger/api/Impl* befindet.

    ![Geöffneten Kontakt Code Dienstdatei][open-contact-service-code-file]

1. Überschreiben Sie den Code in der Datei mit dieser neuen Code eine simulierte Implementierung den Code hinzufügen. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in den Stammordner Ihrer Anwendung Verzeichnis.

1. Führen Sie den folgenden Befehl aus Maven den Code erstellen und Ausführen den Pier app-Server lokal verwenden. 

        mvn package jetty:run

1. Sie sollten finden Sie im Befehlsfenster darauf hinzuweisen, dass Pier Code Port 8080 gestartet hat. 

    ![Geöffneten Kontakt Code Dienstdatei][run-jetty-war]

1. Verwenden Sie zum Erstellen einer Anforderung API Methode am http://localhost: 8080/api/Kontakte "Get aller Kontakte" [Postman] ein.

    ![Rufen Sie die Kontakte API][calling-contacts-api]

1. Verwenden Sie [Postman] , um eine Anforderung an die Methode API "get bestimmten Kontakt" http://localhost: 8080/api/Kontakte/2 am vornehmen.

    ![Rufen Sie die Kontakte API][calling-specific-contact-api]

1. Erstellen Sie abschließend Java WAR (Webarchiv)-Datei, indem Sie den folgenden Befehl aus Maven in Ihrer Konsole ausführen. 

        mvn package war:war

1. Nachdem die WAR-Datei erstellt wurde, wird es in **den Zielordner** platziert werden. Navigieren Sie in **den Zielordner** , und benennen Sie die WAR-Datei in **ROOT.war**. (Stellen Sie sicher, dass die Groß-/Kleinschreibung dieses Format entspricht).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Führen Sie schließlich die folgenden Befehle aus dem Stammordner Ihrer Anwendung zum Erstellen von Ordnern **Bereitstellen** zu verwenden, um die WAR-Datei in Azure bereitstellen. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Veröffentlichen Sie die Ausgabe in Azure-App-Verwaltungsdienst

In diesem Abschnitt erhalten Sie Informationen zum Erstellen einer neuen API App Azure-Portal mit dieser App API für das Hosten von Applications Java vorbereiten und neu erstellten WAR-Datei zu Azure-App-Verwaltungsdienst zum Ausführen Ihrer neuen API App bereitstellen. 

1. Erstellen Sie eine neue API app im [Azure-Portal]durch Klicken auf das Menüelement **Neu -> Web + Mobile-app API >** , Eingabe der app zu erhalten, und klicken Sie dann auf **Erstellen**.

    ![Erstellen einer neuen API-App][create-api-app]

1. Nachdem Sie Ihre app API erstellt wurde, öffnen Sie Ihre app **Einstellungen** Blade, und klicken Sie dann auf das Menüelement **ApplicationSettings** . Wählen Sie die neuesten Versionen der Java anhand der verfügbaren Optionen, und wählen Sie den neuesten Tomcat im **Webcontainer** , und klicken Sie dann auf **Speichern**.

    ![Einrichten von Java in das Blade API-App][set-up-java]

1. Klicken Sie auf das Menüelement der **Bereitstellung Anmeldeinformationen** Einstellungen, und geben Sie einen Benutzernamen und das Kennwort ein, das Sie für die Veröffentlichung von Dateien in Ihrer App API verwenden möchten. 

    ![Legen Sie die Bereitstellung Anmeldeinformationen][deployment-credentials]

1. Klicken Sie auf das Menüelement der **Bereitstellung Quelle** -Einstellungen. Einmal vorhanden ist, klicken Sie auf die Schaltfläche **Quelle auswählen** , wählen Sie die Option **Lokales Git Repository** , und klicken Sie dann auf **OK**. Dadurch wird ein Git Repository ausgeführt in Azure, der eine Zuordnung API-App erstellt. Jedes Mal, wenn Sie Code in den *master* Zweig des Repositorys Git, commit wird in Ihrer live laufenden API App-Instanz von Code veröffentlicht werden. 

    ![Richten Sie eine neue lokale Git repository][select-git-repo]

1. Kopieren Sie das neue Git Repository-URL in der Zwischenablage. Speichern Sie eine wie er in Kürze wichtige angezeigt wird. 

    ![Richten Sie ein neues Git Repository für Ihre app][copy-git-repo-url]

1. Git zu online Repository und drücken Sie die WAR-Datei. Navigieren Sie zu diesem Zweck in Ordner **Bereitstellen** , die, den Sie zuvor erstellt haben, damit Sie den Code nach Zeitphasen bis zum Repository in Ihrem App-Dienst ausgeführt Commit ausführen können. Nachdem Sie das Konsolenfenster und in den Ordner navigiert, in der Ordner Webapps befindet, auch Befehle die folgenden Git starten den Prozess, und Deaktivieren einer bereitstellungs auslösen. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Nachdem Sie die Anfrage **Pushbenachrichtigungen** registrieren, werden Sie das Kennwort aufgefordert, die Sie für die Bereitstellung Anmeldeinformationen zuvor erstellt haben. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, sollten Sie Ihr Portal anzeigt, dass das Update bereitgestellt wurde sehen.

1. Wenn Sie erneut Postman verwenden, um die App-API neu bereitgestellt in Azure-App-Verwaltungsdienst ausgeführt haben, sehen Sie sich, dass das Verhalten konsistent ist und jetzt ist es Kontaktdaten zurückgeben, wie erwartet, und verwenden einfache Code Änderungen an der Swagger.io Gerüstbau Java-Code. 

    ![Verwenden der Java Kontakte REST-API in Azure live][postman-calling-azure-contacts]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel Sie konnten mit einer Swagger JSON-Datei zu starten, und einige Gerüstbau Java-Code für Ihren Kunden aus dem Swagger.io-Editor. Einfachen Änderungen und eine Git Bereitstellen von dort Prozess in einer funktionsübergreifendes API-app in Java geschriebenen Probleme geführt haben. Im nächsten Lernprogramm gezeigt so [nutzen API apps von JavaScript-Clients unter Verwendung des CORS][App Service API CORS]. Höher Lernprogramme der Reihe anzeigen wie Authentifizierung und Autorisierung implementiert wird.

Um dieses Beispiel zu erstellen, können Sie weitere Informationen zur im [Speicher SDK für Java] die JSON-Blobs beibehalten werden. Oder Sie können das [Dokument DB Java SDK] , um die Kontaktdaten in Azure Dokument DB zu speichern. 

Weitere Informationen zur Verwendung von Java in Azure finden Sie im [Java Developer Center].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portal]: https://portal.azure.com/
[Dokument DB Java SDK]: ../documentdb/documentdb-java-application.md
[kostenlose Testversion]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java-Entwicklercenter]: /develop/java/
[Java-Entwickler Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[JAX-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger-Editor]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[SDK für Java-Speicher]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger-Editor]: http://editor.swagger.io/
[Visual Studio-Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png

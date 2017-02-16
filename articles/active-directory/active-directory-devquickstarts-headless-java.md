<properties
    pageTitle="Erste Schritte Azure AD-Java | Microsoft Azure"
    description="So erstellen Sie eine Java Befehlszeile app, die Benutzer eine API Zugriff auf anmeldet."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Verwenden eine API mit Azure AD-Zugriff auf Java Befehlszeile App

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD erleichtert problemlos und einfach Ihre Web-app Identitätsmanagement Auslagern Bereitstellen von einzelnen anmelden und Abmelden mit nur wenigen Codezeilen.  Java Web Apps lässt sich dies realisieren mithilfe von Microsoft Implementierung von der Community leistungsgesteuert ADAL4J.

  Verwenden Sie hier ADAL4J an:
- Melden Sie den Benutzer in der app Azure AD-als Identitätsanbieter aus.
- Einige Informationen zu den Benutzer anzeigen.
- Melden Sie den Benutzer bei der app abmelden.

Um dies zu tun, müssen Sie:

1. Registrieren Sie sich eine Anwendung mit Azure AD
2. Richten Sie Ihre app aus, zu die Bibliothek ADAL4J verwenden.
3. Verwenden Sie die Bibliothek ADAL4J anmelden und Abmelde Anfragen an Azure AD ausgeben.
4. Drucken Sie Daten über den Benutzer aus.

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Sie benötigen auch einem Azure AD-Mandanten in der Anwendung registriert.  Wenn Sie eine bereits, besitzen [erfahren, wie Sie eins zu erhalten](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. Anwendung mit Azure AD registrieren
Wenn Ihre app für die Benutzerauthentifizierung aktivieren möchten, müssen Sie zuerst eine neue Anwendung in Ihrem Mandanten zu registrieren.

- Melden Sie sich bei der Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie den Mandanten, in dem die Anwendung registriert werden soll.
- Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie auf der unteren Einzug hinzufügen.
- Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    - Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    - Die **Anmeldung URL** ist die Basis der app-URL.  Das Gerüst der Standardwert liegt `http://localhost:8080/adal4jsample/`.
    - Der **App-ID-URI** ist ein eindeutiger Bezeichner für eine Anwendung.  Ist die Verwendung der Messe `https://<tenant-domain>/<app-name>`, z. B.`http://localhost:8080/adal4jsample/`
- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte konfigurieren.

Klicken Sie einmal im Portal für Ihre app erstellen Sie eine **Anwendung geheim** für eine Anwendung, und kopieren sie nach unten.  Sie benötigen diese in Kürze.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. so eingerichtet, dass Ihre app ADAL4J Bibliotheks- und erforderliche Komponenten mit Maven verwenden
Konfigurieren Sie hier ADAL4J, um die OpenID verbinden Authentication-Protokoll verwenden.  ADAL4J wird zum Anmelden und Abmelde Anfragen Emission, die Sitzung des Benutzers zu verwalten und erhalten Informationen über den Benutzer, unter anderem verwendet werden.

-   Im Stammverzeichnis des Projekts, öffnen/erstellen `pom.xml` , und suchen Sie die `// TODO: provide dependencies for Maven` und Ersetzen durch die folgende:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3 die Java PublicClient-Datei erstellen

Wie bereits dargelegt, werden wir die Graph-API verwenden, um Daten zu einem Benutzer erhalten. Für diese einfache uns sein sollten wir Erstellen einer Datei für die Darstellung einer **Directory-Objekt** , und eine einzelne Datei, um den **Benutzer** darzustellen, damit das Serienmuster OO Java verwendet werden kann.

1. Erstellen Sie eine Datei namens `DirectoryObject.java` die grundlegende Daten zu einem beliebigen DirectoryObject gespeichert (Sie können gerne dies für alle anderen Graph-Abfragen später verwenden Sie möglicherweise ausführen) verwendet wird. Sie können Ausschneiden/dies von unten einfügen:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Kompilieren Sie und führen Sie das Beispiel

Ändern Ihrer Stammverzeichnisses zurück, und führen Sie den folgenden Befehl zum Erstellen des Beispiels, das Sie gerade haben zusammen mit `maven`. Hiermit wird verwendet die `pom.xml` Datei, die Sie für Abhängigkeiten geschrieben haben.

`$ mvn package`

Sollte, haben Sie jetzt eine `adal4jsample.war` Datei Ihrer `/targets` Directory. Möglicherweise besuchen Sie die URL und bereitstellen, die in Ihrem Container Tomcat 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Es ist sehr einfach zu einer WAR mit den neuesten Tomcat-Servern bereitstellen. Navigieren Sie einfach zu `http://localhost:8080/manager/` und folgen den Anweisungen zum Hochladen von Ihrem '' adal4jsample.war' Datei. Es werden Autodeploy für Sie mit den richtigen Endpunkt.

##<a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie verfügen nun über arbeiten Java-Anwendung, die die Möglichkeit, die Benutzer auf sichere Weise authentifizieren weist Aufrufen von Web-APIs OAuth 2.0 verwenden und Abrufen grundlegender Informationen über den Benutzer.  Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird.

Als Referenz können die vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP hier bereitgestellt wird](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), oder Sie es GitHub zu duplizieren:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```


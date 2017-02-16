<properties 
    pageTitle="Java Azure SDK herunterladen" 
    description="Erfahren Sie, wie dem Azure SDK für Java, mit der Azure-Toolkit für Ellipse für Maven Projekte und grundlegende Installationsschritte vorgesehenen Beispielcode herunterladen." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="download-the-azure-sdk-for-java"></a>Java Azure SDK herunterladen

Dieser Artikel enthält Anweisungen zum Herunterladen und Installieren der Azure-Bibliotheken für Java.

**Hinweis:** Die Bibliotheken Azure für Java sind unter [Apache-Lizenz, Version 2.0]verteilt[license].

## <a name="azure-libraries-for-java---manual-download"></a>Für Java - manueller Azure Bibliotheken herunterladen

Klicken Sie auf <http://go.microsoft.com/fwlink/?LinkId=690320> , um eine ZIP-Datei herunterladen, die allen Bibliotheken und alle Abhängigkeiten enthält, um die Bibliotheken Azure für Java manuell zu installieren.

Nachdem Sie die Zip-Datei auf Ihren Computer heruntergeladen haben, extrahieren Sie den Inhalt aus, und verwenden Sie eine der folgenden Optionen JAR-Dateien zum Projekt hinzufügen:

* Importieren von JAR-Dateien in Ihrem Projekt Java in "Ellipse".

* Konfigurieren Sie den **Pfad erstellen** für ein Projekt Java in "Ellipse", um den Pfad der JAR-Dateien enthalten.

Ausführliche Informationen zum Einrichten von Build Pfade in "Ellipse" finden Sie im Artikel [Erstellen Java-Pfad] auf der Website "Ellipse".

**Hinweis:** Siehe license.txt und ThirdPartyNotices.txt Datendatei innerhalb der ZIP-Lizenz und andere Informationen.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure Bibliotheken für Java - mit Maven erstellen

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Schritt 1: Einrichten des Projekts Maven für Build verwendet werden soll.

So erstellen Maven Projekte in Ellipse die Azure-Bibliotheken für Java verwenden, folgen die Anweisungen in das [Erste Schritte mit Azure Management Bibliotheken für Java] [ maven-getting-started] Artikel. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Schritt 2: Konfigurieren der Einstellungen für Maven mit der erforderlichen Abhängigkeiten

Nachdem Sie Ihr Projekt für die Verwendung von Maven für Build konfiguriert wurde, können Sie Hinzufügen der die erforderlichen Abhängigkeiten zu Ihrer pom.xml-Datei mit Syntax, wie im folgenden Beispiel wird. Beachten Sie, dass Sie nicht jede Abhängigkeit, die aufgelistet ist im folgenden Beispiel hinzufügen müssen. Sie müssen nur die spezifischen Abhängigkeiten hinzufügen, die Ihr Projekt erfordert.

> [AZURE.NOTE] Innerhalb jeder `<version>` Element im im folgenden Beispiel, ersetzen die Platzhalter für "n.n.n" in diesem Beispiel mit gültigen Versionsnummern, die aus dem [Azure Bibliotheken Repository auf Maven]abgerufen werden können.

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Installieren des Azure-Toolkits für "Ellipse"

Dieser Abschnitt enthält grundlegende Anweisungen für die Installation von der Azure-Toolkit für "Ellipse"; Wenn Sie ausführliche Anweisungen finden Sie unter [Installieren der Azure-Toolkit für "Ellipse"].

### <a name="prerequisites"></a>Erforderliche Komponenten

1. Windows Operating Betriebssysteme im Artikel [Was ist neu in der Azure-Toolkit für "Ellipse"] aufgeführt.
1. Macintosh oder Linux Operating Systeme im Artikel [Was ist neu in der Azure-Toolkit für "Ellipse"] aufgeführt.
1. Ellipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.

### <a name="basic-installation-steps"></a>Grundlegende Installationsschritte

1. Wählen Sie in "Ellipse" über das Menü **Hilfe** **Neue Software installieren**.
1. Geben Sie die Website Speicherort <http://dl.microsoft.com/eclipse> ein und drücken Sie die **EINGABETASTE**.
1. Wählen Sie die Elemente installiert werden, und klicken Sie auf **Fertig stellen**.

Das Azure-Toolkit für "Ellipse" wird die neueste Version des SDK Azure verwendet. Dies kann mithilfe der Web Platform Installer (WebPI) am <http://go.microsoft.com/fwlink/?LinkID=252838>heruntergeladen werden. Wenn jedoch nicht installiert ist, beim Erstellen Ihrer ersten Azure-Bereitstellungsprojekts, das Azure-Toolkit für "Ellipse" werden automatisch die entsprechende Version des Azure SDK installieren.

## <a name="see-also"></a>Siehe auch

[Azure-Toolkit für "Ellipse"]

[Installieren des Azure-Toolkits für "Ellipse"] 

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Bibliotheken Repository auf Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java-Generator-Pfad]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Was ist neu in der Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=690333

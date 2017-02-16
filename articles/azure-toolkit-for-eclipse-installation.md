<properties
    pageTitle="Installieren des Azure-Toolkits für "Ellipse" | Microsoft Azure"
    description="Informationen Sie zum Installieren des Azure-Toolkit für "Ellipse"."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installieren des Azure-Toolkits für "Ellipse"

Der Azure-Toolkit für "Ellipse" bietet Vorlagen und Funktionen, die ermöglichen es Ihnen leicht erstellen, entwickeln, testen und Bereitstellen von Azure Applications mithilfe der Entwicklungsumgebung "Ellipse". Das Azure-Toolkit für "Ellipse" ist eine Open Source-Projekt, dessen Quellcode unter die MIT-Lizenz über die Projektwebsite auf GitHub unter dem folgenden URL verfügbar ist:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Die folgenden Schritte gezeigt, wie das Azure-Toolkit für "Ellipse" installieren.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>So installieren Sie das Azure-Toolkit für "Ellipse"

1. Starten Sie "Ellipse" ein.

1. Wenn "Ellipse" geöffnet wird, klicken Sie auf das Menü **Hilfe** , und klicken Sie dann auf **Neue Software installieren**, wie in der folgenden Abbildung gezeigt.

    ![Installieren des Azure-Toolkits für "Ellipse"][01]

1. Geben Sie im Dialogfeld **Software zur Verfügung** , in das Textfeld **Arbeiten mit** **http://dl.microsoft.com/eclipse** und dann die **EINGABETASTE** , ein.

1. Klicken Sie im Bereich **Namen** aktivieren Sie **Azure-Toolkit für "Ellipse"**, und deaktivieren Sie **alle Websites während aktualisieren Kontakt genügt erforderlichen Software installieren**. Ihres Bildschirms sollte ähnlich wie die folgende angezeigt:

    ![Installieren des Azure-Toolkits für "Ellipse"][02]

1. Wenn Sie das **Azure-Toolkit für "Ellipse"**erweitern, sehen Sie die folgenden Elemente:

    * **Anwendung Einsichten-Plug-In für Java**: Diese Komponente ermöglicht Ihnen die Verwendung des Azure werden Protokollierung und Analysis Services für Ihre Applikationen und Server-Instanzen.
    * **Azure Access Steuerelement Services Filter**: Diese Komponente bietet Unterstützung für die Benutzerauthentifizierung mit Azure ACS-Anwendung, Aktivieren der einzelne Szenarien anmelden und das externalisieren Identität Logik aus der Anwendung.
    * **Allgemeine Azure-Plug-Ins**: Diese Komponente bietet die gemeinsamen Funktionen, die von anderen Toolkit-Komponenten erforderlich.
    * **Azure-Explorer für "Ellipse"**: Diese Komponente bietet die gemeinsamen Funktionen, die von anderen Toolkit-Komponenten erforderlich.
    * **Azure-Plug-In für die Ellipse mit Java**: Diese Komponente bietet Unterstützung für die Entwicklung von Projekten, die Ihnen helfen, erstellen, testen und Bereitstellen von Applications Java, für die Microsoft Azure Cloud "Ellipse" und über die Befehlszeile.
    * **Azure Web Apps-Plug-Ins mit Java**: Diese Komponente bietet Unterstützung für die Bereitstellung von Webanwendungen mit Microsoft Azure Web App Containern Java.
    * **Microsoft JDBC Treiber 4.2 für SQL Server**: Diese Komponente bietet JDBC API für SQL Server und Microsoft Azure SQL-Datenbank für Java-Plattform Enterprise Edition 8.
    * **Verpacken für Apache Qpid Client-Bibliotheken für JMS**: Diese Komponente bietet die JMS-Client-Komponente aus dem Projekt Apache Qpid Ihrer Anwendung AMQP messaging in Microsoft Azure verwenden zu aktivieren.
    * **Verpacken für Microsoft Azure Bibliotheken für Java**: Diese Komponente stellt APIs für den Zugriff auf Microsoft Azure-Dienste, wie z. B. Speicher, Dienstbus, -Dienstlaufzeit.

1. Klicken Sie auf **Weiter**. (Wenn ungewöhnliche Verzögerung auftritt, wenn das Toolkit installieren, stellen Sie sicher, dass **alle Websites während aktualisieren Kontakt genügt jeweils erforderlichen Software installieren** nicht aktiviert ist.)

1. Klicken Sie im Dialogfeld **Installieren Details** auf **Weiter**.

    ![Überprüfen Sie die Installationsdetails][03]

1. Überprüfen Sie im Dialogfeld **Lizenzen überprüfen** die Konditionen, der im-Lizenzvertrag. Wenn Sie den Lizenzvertrag zustimmen, klicken Sie auf **den Lizenzvertrag annehmen** , und klicken Sie dann auf **Fertig stellen**. (Die verbleibenden Schritte wird davon ausgegangen, dass Sie den Lizenzvertrag zustimmen. Wenn Sie nicht im Lizenzvertrag zustimmen, beenden Sie den Installationsvorgang.)

    ![Überprüfen von Lizenzen][04]

    Ellipse wird herunterladen und installieren Sie die erforderliche Pakete.

    ![Verlauf der Installation][05]

1. Wenn Sie dazu aufgefordert werden, neu zu starten "Ellipse", um die Installation abzuschließen, klicken Sie auf **Ja**.

    ![Starten Sie erneut auffordern][06]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu den Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für "Ellipse"]
  - *Installieren des Azure-Toolkits für "Ellipse" (in diesem Artikel)*
  - [Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]
  - [Was ist neu in der Azure-Toolkit für "Ellipse"]
- [Azure-Toolkit für IntelliJ]
  - [Installieren des Azure-Toolkits für IntelliJ]
  - [Erstellen einer Hallo Welt Web App für Azure in IntelliJ]
  - [Was ist neu in der Azure-Toolkit für IntelliJ]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für "Ellipse"]: ./azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer Hallo Welt Web App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure-Toolkits für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Was ist neu in der Azure-Toolkit für "Ellipse"]: ./azure-toolkit-for-eclipse-whats-new.md
[Was ist neu in der Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java-Entwicklercenter]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png


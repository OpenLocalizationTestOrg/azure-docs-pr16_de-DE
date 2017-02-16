<properties
    pageTitle="Was ist neu in der Azure-Toolkit für IntelliJ | Microsoft Azure"
    description="Lernen Sie die neuesten Funktionen im Azure-Toolkit für IntelliJ aus."
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
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Was ist neu in der Azure-Toolkit für IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure-Toolkit IntelliJ Versionen

Dieser Artikel enthält Informationen zu den verschiedenen Versionen und die neuesten Updates für das Azure-Toolkit für IntelliJ.

> [AZURE.NOTE] Es gibt auch ein Azure-Toolkit für die Ellipse IDE. Weitere Informationen finden Sie unter [Azure-Toolkit für "Ellipse"].

### <a name="august-26-2016"></a>26 August 2016

Das Azure-Toolkit für IntelliJ - August 2016 Version umfasst die folgenden Erweiterungen:

* **Benutzerdefinierte JDK Verteilung**. Das Azure-Toolkit für IntelliJ unterstützt jetzt angeben und eine zufällige JDK-Version auf Ihre Azure WebApp Container bereitstellen:
  - Zusätzlich zu den JDKs von Azure bereitgestellt werden können Sie auch über eine große Auswahl an Zulu OpenJDK-Versionen zur Verfügung gestellt Azure von Azul Systems auswählen.
  - Sie können auch eigene JDK Verteilung angeben, wenn Sie es bei Ihrem Speicherkonto als ZIP-Datei hochladen.
* **Erweiterte Funktionen für die Azure-Explorer-Ansicht**:
  - Unterstützung für virtuellen Computern Management mit Azure des neuen Ressourcenmanager Modell: Liste, erstellen und Löschen von Ressourcen-Manager-basierten virtuellen Computern ohne die IDE verlassen zu müssen.
  - Unterstützung für Speicher-Konto Blob Management mithilfe des Azure Ressourcenmanager, die vorhandene Funktionalität für die Verwaltung von Speicherkonten "klassische" ergänzen.
* **Microsoft JDBC-Treiber für SQLServer 6.0**. Dieses Update enthält den aktuellen JDBC-Treiber für Microsoft SQL Server (6.0), wird jetzt als Bibliothek enthalten, die Sie einfach Ihre Projekte Java hinzufügen können, damit auch die ältere Version ersetzen.

### <a name="june-29-2016"></a>29 Juni 2016

Das Azure-Toolkit für IntelliJ - Juni 2016 Version umfasst die folgenden Erweiterungen:

* **Java 8 Anforderung**. Das Azure-Toolkit für IntelliJ ist jetzt Java 8 erforderlich, auch wenn diese Anforderung gilt nur für das Toolkit - Anwendung können weiterhin alle Java-Versionen verwenden, die von Azure unterstützt werden.
* **Unterstützung für die neuesten Java JDKs**. Die neuesten Versionen der Java-JDKs werden jetzt vom Azure Toolkit für IntelliJ unterstützt.
* **Unterstützung für Azure SDK v2.9.1**. Die neueste Version des SDK Azure ist jetzt der minimalen Voraussetzung für das Azure-Toolkit für IntelliJ.
* **Integrierte Beispiele**. Das Azure-Toolkit für IntelliJ features jetzt mehrere Stichprobe Applikationen Entwickler Einstieg helfen.
* **HDInsight Tool Integration**. Der Azure HDInsight Tools sind jetzt im Lieferumfang der Azure-Toolkit für IntelliJ enthalten. Weitere Informationen finden Sie unter [HDInsight Tools-Plug-In für IntelliJ].
* **Remote Debuggen von Java Web Apps**. Das Azure-Toolkit für IntelliJ unterstützt jetzt remote Debuggen von Java Web apps auf App-Verwaltungsdienst Azure.

### <a name="april-12-2016"></a>12 April 2016

Das Azure-Toolkit für IntelliJ - April 2016 Version umfasst die folgenden Erweiterungen:

* **Unterstützung für Azure SDK v2.9.0**. Die neueste Version des SDK Azure ist jetzt der minimalen Voraussetzung für das Azure-Toolkit für IntelliJ.
* **Verschiedene Nutzbarkeit, Verbesserung der Reaktionszeiten und Leistung im Zusammenhang mit Azure Web App-Support**. Eine Reihe von Leistung Optimierungen in Azure Ergebnis wie das Toolkit kommuniziert in eine Benutzeroberfläche mehr reagiert.
* **Möglichkeit zum Löschen eines vorhandenen Webanwendung Containers in Azure aus innerhalb IntelliJ**. Mit dem Azure-Toolkit für IntelliJ können jetzt mit einen vorhandenen Azure Web Container gelöscht, ohne IntelliJ verlassen zu müssen.

## <a name="see-also"></a>Siehe auch ##

Weitere Informationen zu den Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für "Ellipse"]
  - [Installieren des Azure-Toolkits für "Ellipse"]
  - [Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]
  - [Was ist neu in der Azure-Toolkit für "Ellipse"]
- [Azure-Toolkit für IntelliJ]
  - [Installieren des Azure-Toolkits für IntelliJ]
  - [Erstellen einer Hallo Welt Web App für Azure in IntelliJ]
  - *Was ist neu in der Azure-Toolkit für IntelliJ (in diesem Artikel)*

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für "Ellipse"]: ./azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer Hallo Welt Web App für Azure in "Ellipse"]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer Hallo Welt Web App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für "Ellipse"]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure-Toolkits für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Was ist neu in der Azure-Toolkit für "Ellipse"]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight Tools-Plug-In für IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md

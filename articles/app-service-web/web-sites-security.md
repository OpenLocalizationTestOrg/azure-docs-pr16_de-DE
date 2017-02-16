<properties
    pageTitle="Sichern einer app im App-Verwaltungsdienst Azure"
    description="Erfahren Sie, wie ein Web-app, mobile-app Back-End- oder API-app im App-Verwaltungsdienst Azure gesichert."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Sichern einer app im App-Verwaltungsdienst Azure

In diesem Artikel können Sie die Schritte zum Sichern von Ihrem Web app, mobile-app Back-End- oder API-app im App-Verwaltungsdienst Azure. 

Sicherheit in Azure-App-Verwaltungsdienst besteht aus zwei Ebenen: 

- **Infrastruktur und Plattform Sicherheit** – vertrauen Sie Azure, um die Dienste, können Sie Elemente in der Cloud sicher tatsächlich ausführen müssen.
- **Application Security** – Sie müssen die app ähneln sicher gestalten. Dies umfasst, wie Sie mit Azure Active Directory integrieren, wie Sie Zertifikate verwalten und wie Sie sicherstellen, dass Sie für andere Dienste sicher kommunizieren können. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktur und Plattform-Sicherheit
Da App-Dienst Azure virtuellen Computern, Speicher, Netzwerk Verbindungen, Web Framework, Verwaltung und Features für die Integration und vieles mehr, verwaltet ist aktiv gesichert und abgesichert strenge Compliance durchläuft und überprüft, um sicherzustellen, dass permanenten:

- Ihre App-Dienst Apps aus dem Internet und die anderen Kunden Azure Ressourcen isoliert.
- Kommunikation der Kennwörter (z. B. Verbindungszeichenfolgen) zwischen der app-App-Dienst und andere Azure Ressourcen (z. B. SQL-Datenbank) in einer Ressourcengruppe bleibt in Azure und Grenzen von Netzwerken nicht überschreiten. Kennwörter sind immer verschlüsselt.
- Alle Kommunikation zwischen Ihrem App-Service-app und externer Ressourcen, wie PowerShell-Verwaltung, line Interface, Azure SDKs, REST-APIs und Hybrid-Verbindungen ordnungsgemäß verschlüsselt sind.
- 24-Stunden-Bedrohung Management Loss App-Service-Ressourcen vor Malware, verteilt DOS (DDoS), Man-in-the-Mitte (MITM) und ausgefeilte. 

Weitere Informationen zur Sicherheit von Infrastruktur und Plattform in Azure finden Sie unter [Azure Trust Center](/support/trust-center/security/).

#### <a name="application-security"></a>Anwendung Sicherheit

Während der Azure ist verantwortlich für die Infrastruktur sowie Plattform integriert, die die Anwendung ausgeführt wird, klicken Sie auf Sichern, ist es Ihre Aufgabe zur Sicherung Ihrer Anwendung selbst. Kurzum, müssen Sie entwickeln, bereitstellen und Verwalten Ihrer Anwendungscode und Inhalt auf sichere Weise. Ohne diese möglich Ihrer Anwendungscode oder Inhalte weiterhin Angriffen wie:

- SQL einfügen
- Missbrauch Sitzung
- Cross-Site scripting
- MITM Anwendung Ebene
- Anwendung Ebene DDoS

Eine vollständige Erläuterung Sicherheit Aspekten für Applikationen webbasierten ist Gegenstand dieses Dokuments. Als Ausgangspunkt für eine weitere Anleitung zum Sichern der Anwendung finden Sie in der [Geöffneten Web Anwendung Sicherheit Project (OWASP)](https://www.owasp.org/index.php/Main_Page), insbesondere den [Top 10 Project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), der die aktuelle Top 10 wichtige Web Anwendung Sicherheitsfehler aufgelistet sind, durch OWASP Mitglieder bestimmt wird.

## <a name="perform-penetration-testing-on-your-app"></a>Führen Sie Ihre App Durchdringungstests

Eine der einfachsten Methoden, um noch mit Testen auf Sicherheitslücken auf Ihre App-Dienst app gestartet besteht darin, verwenden die [Integration mit Folie Sicherheit](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) Scannen für Ihre app ein-Klick-Sicherheitsrisiko ausführen. Sie können die Testergebnisse in einem leicht verständliche Bericht anzeigen, und erfahren, wie Sie jede Sicherheitsrisiko mit einer schrittweisen Anleitung zu beheben.

Wenn Sie es vorziehen, eine eigene Penetration Tests durchführen oder anderen Scanner Suite oder Anbieter verwenden möchten, müssen Sie folgen [Azure Penetration Tests Genehmigungsvorgang starten](https://security-forms.azure.com/penetration-testing/terms) und vorherige Zustimmung Durchführung der gewünschten Penetration Tests zu erhalten.

##<a name="a-namehttpsa-secure-communication-with-customers"></a><a name="https"></a>Sichere Kommunikation mit Kunden

Wenn Sie verwenden die ** \*. azurewebsites.net** Domänennamen für Ihre App-Service-app erstellt sofort können HTTPS, wie Sie ein SSL-Zertifikat für alle vorgesehen ist ** \*. azurewebsites.net** Domänennamen. Wenn Ihre Website einen [benutzerdefinierten Domänennamen](web-sites-custom-domain-name.md)verwendet, können Sie ein SSL-Zertifikat für die benutzerdefinierte Domäne [HTTPS aktivieren](web-sites-configure-ssl-certificate.md) hochladen.

Aktivieren von [HTTPS](https://en.wikipedia.org/wiki/HTTPS) kann Schutz vor MITM Angriffen bei der Kommunikation zwischen der app und ihre Benutzer.

## <a name="secure-data-tier"></a>Secure Datenebene

App-Dienst integriert hochgradig SQL-Datenbank so, dass alle Verbindungszeichenfolgen generell verschlüsselt sind und nur des virtuellen Computers, der die app *und* ausgeführt wird entschlüsselt werden, nur, wenn die app ausgeführt wird. Darüber hinaus enthält Azure SQL-Datenbank viele Sicherheitsfeatures, um Ihnen dabei helfen Ihrer Anwendungsdaten aus einem Internet Risiken, einschließlich [Verschlüsselung bei Rest](https://msdn.microsoft.com/library/dn948096.aspx), [Immer verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamische Daten Masking](../sql-database/sql-database-dynamic-data-masking-get-started.md)und [Erkennung](../sql-database/sql-database-threat-detection-get-started.md)zu schützen. Wenn Sie vertrauliche Daten oder Vorschriften haben, finden Sie weitere Informationen zum Sichern Ihrer Daten [Sichern Ihrer SQL-Datenbank](../sql-database/sql-database-security.md) .

Wenn Sie einen Drittanbieter-Anbieter, wie z. B. ClearDB, verwenden, sollten Sie mit der Anbieter Dokumentation direkt über bewährte Methoden für die Sicherheit sprechen.  

##<a name="a-namedevelopa-secure-development-and-deployment"></a><a name="develop"></a>Sichere Entwicklung und Bereitstellung

### <a name="publishing-profiles-and-publish-settings"></a>Für die Veröffentlichung Profile und veröffentlichungseinstellungen

Bei der Entwicklung von Applications, Verwaltungsaufgaben ausführen oder zum Automatisieren von Aufgaben, die mit Dienstprogrammen wie **Visual Studio**, **Web Matrix**, **Azure PowerShell** oder der **Azure Line Interface (CLI Azure)**, können Sie eine Datei *veröffentlichungseinstellungen* oder ein *Profil wird veröffentlicht*. Beide Dateitypen Sie mit Azure authentifizieren und gesichert werden sollte, um unbefugten Zugriff zu verhindern.

* Enthält eine Datei **veröffentlichungseinstellungen**

    * Ihre Azure-Abonnement-ID

    * Ein Management-Zertifikat, das Sie Verwaltungsaufgaben für Ihr Abonnement *ohne einen Kontonamen oder ein Kennwort angeben*kann.

* Eine Datei **für die Veröffentlichung Profil** enthält

    * Informationen für die Veröffentlichung zu Ihrer Anwendung

Wenn Sie ein Programm, die eine Einstellungsdatei veröffentlichen oder veröffentlichen Profildatei verwendet verwenden, importieren Sie die Datei, die Einstellungen veröffentlichen oder das Profil in das Programm, und klicken Sie dann **Löschen** Sie die Datei enthält. Wenn Sie die Datei, z. B. am Projekt arbeiten freigeben beibehalten müssen speichern Sie es an einem sicheren Ort, zum Beispiel eine *verschlüsselte* mit eingeschränkten Berechtigungen.

Darüber hinaus sollten Sie sicherstellen, dass die importierten Anmeldeinformationen geschützt sind. Beispielsweise **Azure PowerShell** und die **Azure Line Interface (CLI Azure)** speichern importierten Informationen in Ihrem **home-Verzeichnis** (*~* Linux oder OS X Systeme und */users/yourusername* auf Windows-Systemen.) Für zusätzliche Sicherheit zu gewährleisten können Sie zum **Verschlüsseln der** diese Orte mit Verschlüsselungstools für Ihr Betriebssystem den.

### <a name="configuration-settings-and-connection-strings"></a>Konfiguration von Einstellungen und Verbindungszeichenfolgen
Es ist üblich Verbindungszeichenfolgen, Authentifizierungsinformationen und andere vertrauliche Informationen in Dateien gespeichert. Leider können diese Dateien auf Ihrer Website offen gelegt oder in einer öffentlichen Repository, diese Informationen offenlegen eingecheckt werden. Bei einer einfache Suche auf [GitHub](https://github.com), kann sich Konfigurationsdateien mit zugänglicher vertrauliche Informationen in den öffentlichen Repositorys beispielsweise für eine Steigerung.

Die bewährte Methode ist diese Informationen aus Ihrem app Konfigurationsdateien beibehalten. App-Dienst können Sie Informationen zur Konfiguration als Teil der Runtime-Umgebung als **app-Einstellungen** und **Verbindungszeichenfolgen**zu speichern. Die Werte werden an Ihrer Anwendung zur Laufzeit durch *Umgebungsvariablen* für die meisten programming Sprachen verfügbar gemacht. Für .NET Applications werden diese Werte in der Konfiguration .NET zur Laufzeit eingefügt. Abgesehen von den folgenden Situationen bleibt diese Einstellungen Konfiguration verschlüsselt es sei denn, Sie anzeigen oder mit dem [Azure-Portal](https://portal.azure.com) oder Dienstprogramme, z. B. PowerShell oder der Azure CLI zu konfigurieren. 

Speichern von Informationen zur Konfiguration in der App-Dienst ermöglicht es des app Administrator, vertrauliche Informationen für die Herstellung apps zu sperren. Entwickler können eine separate Reihe von Konfigurationen für app-Entwicklung und die Einstellungen können automatisch ersetzt werden, indem Sie die Einstellungen im App-Dienst konfiguriert. Nicht auch die Entwickler müssen das Geheimnis so konfiguriert, dass für die Herstellung app wissen. Weitere Informationen zum Konfigurieren der Einstellungen für die app und Verbindungszeichenfolgen in der App-Dienst finden Sie unter [Konfigurieren von Web apps](web-sites-configure.md).

### <a name="ftps"></a>FTPS

App-Verwaltungsdienst Azure bietet secure FTP-Zugriff auf das Dateisystem für Ihre app durch **FTPS**. So können Sie den Code der Anwendung auf Web app als auch Diagnose sicheren Zugriff auf Protokolle. Es wird empfohlen, dass Sie immer FTPS anstelle von FTP verwenden. 

Der Link FTPS für Ihre app finden Sie die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie **Alle durchsuchen**.
3. Wählen Sie aus dem Blade **Durchsuchen** **App-Dienste**.
4. Wählen Sie die gewünschte app aus dem **App-Dienste** Blade.
5. Wählen Sie aus der app-Blade **Alle Einstellungen**aus.
6. Wählen Sie **Eigenschaften**aus dem Blade **Einstellungen** aus.
7. Klicken Sie auf das Blade **Einstellungen** werden die FTP und FTPS Links bereitgestellt. 

Weitere Informationen zum FTPS finden Sie unter [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Sicherheit der Azure-Plattform Informationen für die Berichterstattung eines **Wertpapiers Vorfall oder Missbrauch**oder Microsoft zu informieren, dass Sie Ihre Website **Durchdringungstests** aktiv sein werden finden Sie im Sicherheitsabschnitt "des [Microsoft Azure-Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Weitere Informationen zu **web.config** oder **applicationhost.config** Dateien in der App-Service-apps finden Sie unter [Konfigurationsoptionen in der App-Verwaltungsdienst Azure Web apps aufgehoben](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Informationen zu Protokollierungsinformationen für App-Service-apps, Erkennen von Angriffen hilfreich sein können, finden Sie unter [Diagnoseprotokoll aktivieren](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine app kurzlebige Starter sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert

* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

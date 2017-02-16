#<a name="azure-security-guidance"></a>Sicherheit von Azure-Leitfaden

##<a name="abstract"></a>Abstrakte

Bei der Entwicklung von Applications für Azure sind Identität und Access die primäre Sicherheitsgründen wird empfohlen, die Sie benötigen, zu berücksichtigen.
In diesem Thema wird erläutert, die wichtigsten Sicherheitsaspekte im Zusammenhang mit Identität und Access in der Cloud und wie Sie Ihre Cloud-Programme am besten schützen können.

##<a name="overview"></a>(Übersicht)

Die Anwendung Sicherheit ergibt sich aus der Oberfläche. Bezieht sich auf die weitere Oberfläche, dass die Anwendung, desto größere die Sicherheit macht. Beispielsweise macht eine Anwendung, die als Prozess unbeaufsichtigten Stapel ausgeführt wird kleiner als einer öffentlich zugänglichen Website im Hinblick auf Sicherheit.

Wenn Sie in der Cloud zu verschieben, dass Sie erhalten eine bestimmte ein sicheres Gefühl zu Infrastruktur und Netzwerke, da diese Daten verwaltet werden, zentriert mit Methoden für die Sicherheit, Tools, Technologie. Andererseits, stellt die Cloud systembedingt Weitere Oberfläche für eine Anwendung, die potenziell von Angreifern genutzt werden kann. Dies liegt daran viele Cloud-Technologien und Dienste als Endpunkte im Vergleich zu im Speicher Komponenten verfügbar gemacht werden. Azure-Speicher, Dienstbus, SQL-Datenbank (vormals SQL Azure) und viele andere Dienste sind über die Verbindung über ihre Endpunkte zugänglich.

In Cloud Applikationen ordnet an weitere Zuständigkeit auf den Entwickler entwerfen, entwickeln und deren Cloud Applikationen hohe Sicherheitsstandards um Angreifer abzuwehren verwalten.
Erwägen Sie das folgende Diagramm (von J.D Die Meier [Azure Sicherheit Notizen PDF-Datei](http://blogs.msdn.com/b/jmeier/archive/2010/08/03/now-available-azure-security-notes-pdf.aspx)): Beachten Sie, wie das Webpart Infrastruktur vom Cloudanbieter – in diesem Fall durch Azure – mehr Sicherheit Arbeit verlassen, um die Anwendungsentwickler erfüllt werden kann:

![Sichern der Anwendungs][01]

Die gute Nachricht ist, dass alle die Sicherheit Entwicklungsmethoden, Prinzipien und Techniken, die Sie bereits weiterhin kennen angewendet werden, wenn Sie zur Entwicklung von Applications Cloud. Berücksichtigen Sie die folgenden wichtigen Bereiche, die berücksichtigt werden müssen:

-   Alle Eingaben für Typ, Länge, Bereichs- und Zeichenfolge Mustern Injektionsangriffe vermeiden überprüft wird, und alle Daten, die Ihre app wieder wiedergegeben wird ordnungsgemäß bereinigt.
-   Vertrauliche Daten sollten geschützt werden, statisch sind und bei der Übertragung um Informationsfreigabe und Risiken Manipulation Daten zu verringern.
-   Überwachung und Protokollierung müssen ordnungsgemäß implementiert werden, um nicht Ablehnung Einschränken dieser Risiken.
-   Authentifizierung und Autorisierung sollte mit bewährten Verfahren der Plattform zu verhindern, dass Identität spoofing und Erhöhung von Berechtigungen Risiken implementiert werden.

Eine vollständige Liste der Risiken, Angriffen, Sicherheitsrisiken und Gegenmaßnahmen verweisen zu Muster & Methoden [Spickzettel Blatt: Web Anwendung Sicherheit Frame](http://msdn.microsoft.com/library/ff649461.aspx) und [Sicherheit Anleitungen für Applikationen Index](http://msdn.microsoft.com/library/ff650760.aspx).

In der Cloud unterscheiden Authentifizierung und Zugriffssteuerungsmechanismen sehr denen für lokale Anwendungen verfügbar. Es gibt viele weitere Anmelde- und Access-Optionen für Cloudanwendungen, die führen können Verwirrung und daher fehlerhaften Implementierungen angeboten. Weitere Verwirrung ist eingeführt werden beim definieren eine Cloud-app ist. Beispielsweise könnten die Anwendung bereitgestellt werden, in der Cloud noch deren Authentifizierungsmethode möglicherweise durch Active Directory bereitgestellt werden. Auf anderen Hand, könnte die Anwendung bereitgestellten lokalen jedoch mit Authentifizierung Verfahren in der Cloud (z. B., indem Sie Azure Active Directory Access Control (früher bekannt als Steuerelement-Dienst oder ACS)).

##<a name="threats-vulnerabilities-and-attacks"></a>Risiken und Schwachstellen Angriffen

Ein darstellen ist ein potenzieller schlechten Ergebnis, das Sie beispielsweise die Offenlegung vertrauliche Informationen oder einen Dienst zu einem Ausfall vermeiden möchten.
Es ist üblich, zu Risiken klassifizieren mithilfe des Akronyms "Schritt":

-   **S**Poofing oder Identitätsdiebstahl
-   **T**Ampering mit Daten
-   **R**epudiation (Nichtanerkennung) von Aktionen
-   **Ich**Nformation Offenlegung
-   **D**Enial des Diensts
-   **E**Levation von Berechtigungen

Schwachstellen sind Fehler, die wir, als Entwickler, in den Code vorstellen, die Anwendung von Angreifern gefährlichen vornehmen. Beispielsweise ermöglicht vertrauliche Daten in unverschlüsselter Form sendende ein Informationen Offenlegung Risiko nach einer Ermittlung von Angriffen Datenverkehr.

Angriffen sind die Nutzung dieser Sicherheitsrisiken schädlich zur Anwendung. Zum Beispiel ist eine Cross Site Scripting oder Cross-Site Scripting, Angriffen, die unsanitized Ausgabe nutzt. Klicken Sie auf das Netzwerk zum Erfassen von Anmeldeinformationen in Klartext gesendet wird ein weiteres Beispiel abhören. Ein Identität Spoofing Risiko realisiert werden können diesen Angriffen führen. Ausreicht einfach erwägen Risiken, Sicherheitslücken und Angriffen als Probleme.
Berücksichtigen Sie die folgenden Diagramme als Balcony Ansicht der fehlerhafte Aktionen im Zusammenhang mit einer Web-Anwendung (über J.D in Azure bereitgestellt
Meier die [Sicherheit von Azure Notizen PDF-Datei](http://blogs.msdn.com/b/jmeier/archive/2010/08/03/now-available-azure-security-notes-pdf.aspx)):

![Risiken Sicherheitslücken und Angriffen][02]

Haben Sie, als Entwickler Kontrolle über die Sicherheitslücken. Die weniger Sicherheitsrisiken, die Sie einführen, die weniger Optionen, die für Angreifer ausnutzen beibehalten werden.

Identität und Access verknüpften Schwachstellen verlassen, die Sie alle Risiken im Modell Schrittweite zu öffnen. Beispielsweise kann eine nicht ordnungsgemäß implementiert Authentifizierungsmethode mit einer einer Identität gefälschten und als ein Ergebnis, Offenlegung von Informationen, Daten manipuliert werden, erhöhten Vorgänge oder sogar völlig Sie den Dienst beenden führen. Beachten Sie die folgenden Fragen, die möglicherweise zeigen Sie auf möglichen Sicherheitslücken in der Cloud-app Identität und Zugriff auf Implementierung aus:

-   Sind Sie Anmeldeinformationen löschen über die Verbindung mit Ihren Azure Services senden?
-   Speichern Sie Anmeldeinformationen Azure Services in löschen?
-   Führen Sie Ihre Anmeldeinformationen Azure Services sichere Kennwortrichtlinien folgen?
-   Verlassen Sie auf Azure zur Überprüfung der Anmeldeinformationen im Vergleich zu benutzerdefinierten Überprüfung mithilfe?
-   Schränken Sie Azure Services Authentifizierung Sitzungen oder token Lebensdauer in einem angemessenen Zeitfenster?
-   Überprüfen Sie die Berechtigungen für jede Azure Einstiegspunkt der app verteilten Cloud?
-   Zwar Ihre Autorisierung Verfahren sicher ohne vertraulichen Informationen verfügbar zu machen oder ohne unbegrenzte Zugriff fehl?

##<a name="countermeasures"></a>Gegenmaßnahmen

Die beste Gegenmaßnahme vor Angriffen mithilfe der Identität und Access Verfahren der Plattform ist nicht eigene implementiert. Beachten Sie die folgenden herausragender Identität und Access-Technologien aus:

**Identität Windows Foundation (WIF).** Für WIF ist ein .NET Runtime-Bibliothek mit .NET Framework 4.5 (steht auch als separate Download für .NET 3.5/4.0) enthalten. WIF unterstützt Aufgaben für den Umgang mit Protokollen wie WS-Federation und WS-Trust und Token wie Security Assertion Markup Language (SAML) Behandeln von, damit Sie nicht sehr komplexe Sicherheits-Code in Ihrer Anwendung zu schreiben. Die folgenden Ressourcen finden Sie ausführliche Informationen zu WIF:

-   [Windows-Identität Foundation 4.5 Beispiele](http://code.msdn.microsoft.com/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=wif&f%5B1%5D.Type=Topic&f%5B1%5D.Value=claims-based%20authentication) in MSDN Code Gallery.
-   [Windows-Identität Foundation 4.5 Tools für Visual Studio 11 Beta](http://visualstudiogallery.msdn.microsoft.com/e21bf653-dfe1-4d81-b3d3-795cb104066e) in MSDN Code Gallery.
-   [Windows Identität Foundation-Laufzeit (.Net 3.5/4.0)](http://www.microsoft.com/download/details.aspx?id=17331) auf MSDN.
-   [Beispiele für Windows Identität Foundation 3.5/4.0 und Visual Studio 2008/2010-Vorlagen](http://www.microsoft.com/download/details.aspx?displaylang=en&id=4451) auf MSDN.

**Azure AD-Access-Steuerelement (früher bekannt als ACS)**. Azure Active Directory Access Control ist ein Clouddienst, bietet Security Token Service (STS) und ermöglicht Föderation mit anderen Identitätsanbieter (IdPs) wie corporate Active Directory oder Internet IdPs wie Windows Live ID / Microsoft Account, Facebook, Gmail, Yahoo! und Open-ID 2.0 Identitätsanbieter. Die folgenden Ressourcen finden Sie ausführliche Informationen zu Azure AD-Access-Steuerelement:

-   [Access Control Service 2.0](http://msdn.microsoft.com/library/gg429786.aspx) 
-   [Szenarien und Lösungen mithilfe von ACS](http://msdn.microsoft.com/library/gg185920.aspx)
-   [So ACS des](http://msdn.microsoft.com/library/windowsazure/gg185939.aspx)
-   [Einen Leitfaden zum anspruchsbasierte Identität und Access-Steuerelement](http://msdn.microsoft.com/library/ff423674.aspx)
-   [Identität Entwicklertools Schulungskit](http://www.microsoft.com/download/details.aspx?id=14347)
-   [MSDN gehostete Identität Entwicklertools Schulungskurs](http://msdn.microsoft.com/IdentityTrainingCourse)

**Active Directory Federation Services (AD FS).** Active Directory Federation Services (AD FS) 2.0 bietet Unterstützung für Ansprüche unterstützende Identität Lösungen, die Windows-Server? betreffen? und Active Directory-Technologie. AD FS 2.0 unterstützt WS-Trust, WS-Verbund und SAML-Protokolle. Die folgenden Ressourcen finden Sie ausführliche Informationen zu AD FS:

-   [AD FS 2.0 Inhalte Karte](http://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)
-   [SSO Webdesign][Web SSO Design]
-   [Partnerverbundkontakte SSO Webdesign][Federated Web SSO Design]

**Azure freigegeben Access Signaturen.** Freigegebene Access Signaturen können Sie Zugriff auf eine Ressource Blob oder Container optimieren. Die folgenden Ressourcen finden Sie ausführliche Informationen zu Access-Signaturen freigegeben.

-   [Verwalten des Zugriffs auf Blobs und Container](http://msdn.microsoft.com/library/ee393343.aspx)
-   [Neues Speicherfeature: Gemeinsamen Zugriff Signaturen](http://blog.smarx.com/posts/new-storage-feature-signed-access-signatures)
-   [Freigegebene Access Signaturen sind einfach heutzutage](http://blog.smarx.com/posts/shared-access-signatures-are-easy-these-days)

##<a name="scenarios-map"></a>Szenarien Karte

In diesem Abschnitt erhält einen kurzen Überblick die Schlüsselszenarien in diesem Thema behandelt.
Verwenden Sie es als Karte, um die richtige Identität Lösung für eine Anwendung zu ermitteln.

-   **ASP.NET Web Form-Anwendung mit Partnerverbundkontakte Authentifizierung.** In diesem Szenario Sie Steuerung des Zugriffs auf Ihre ASP.NET Web Forms-app verwenden entweder Internetidentität wie Live ID / Microsoft Account oder corporate Identität in Windows Server Active Directory verwaltet.
-   **WCF (SOAP)-Dienst mit Partnerverbundkontakte Authentifizierung.** In diesem Szenario steuern Sie den Zugriff mit Ihrem Dienstidentitäten von Azure Active Directory Access Control verwaltet mit WCF (SOAP)-Dienst.
-   **WCF (SOAP)-Dienst mit Partnerverbundkontakte Authentifizierung Identitäten in Active Directory.** In diesem Szenario können Sie Zugriff auf Ihre WCF (SOAP) Webdienst mithilfe von corporate Windows Server Active Directory verwaltet Identitäten aus.
-   **WCF (REST)-Dienst mit Partnerverbundkontakte Authentifizierung.** In diesem Szenario steuern Sie den Zugriff mit Ihrem Dienstidentitäten von Azure Active Directory Access Control verwaltet mit WCF (REST)-Dienst.
-   **WCF (REST)-Dienst mit Live ID / Microsoft-Konto, Facebook, Gmail, Yahoo!, öffnen Sie die ID an.** In diesem Szenario Sie Steuerung des Zugriffs auf Ihre WCF (REST) Dienst Internetidentität wie Live ID verwenden / Microsoft-Konto.
-   **ASP.NET Web App zum REST WCF-Diensts mithilfe des freigegebenen SWT Token.** In diesem Szenario stehen verteilten Anwendung mit front-End ASP.NET Web app und untergeordnete REST-Dienst und des Endbenutzers Kontext durch physische Ebenen Datenfluss möchten.
-   **Rollenbasierte Steuerelement (RBAC) Autorisierung In Ansprüche unterstützende Anwendungen und Dienste.** In diesem Szenario möchten Autorisierungslogik in Ihrer app basierend auf Rollen implementieren.
-   **Anspruchsbasierte Autorisierung In Ansprüche unterstützende Anwendungen und Dienste.** In diesem Szenario möchten Autorisierungslogik in Ihrer app auf der Grundlage komplexer Autorisierungsregeln implementieren.
-   **Azure-Speicher Dienstidentität und Access Szenarien.** In diesem Szenario müssen Sie Zugriff auf Azure-Speicher Blobs und Container sicher freigeben.
-   **SQL Azure-Datenbank Identität und Access Szenarien.** SQL-Datenbank unterstützt nur die SQL Server-Authentifizierung. Windows-Authentifizierung (integrierte Sicherheit) wird nicht unterstützt. Benutzer müssen Anmeldeinformationen (Benutzername und Kennwort) jedes Mal angeben, die beim Verbinden mit SQL-Datenbank.
-   **Azure Service Bus Identität und Access Szenarien.** In diesem Szenario müssen Sie Azure Servicebuswarteschlangen sicheren Zugriff auf.
-   **In-Memory-Cache Identität und Access Szenarien.** In diesem Szenario müssen Sie sicheren Zugriff auf Daten, die von in-Memory-Cache verwaltet werden.
-   **Azure Marketplace-Identität und Access Szenarien.** In diesem Szenario müssen Sie Azure Marketplace Datasets sicheren Zugriff auf.

##<a name="azure-identity-and-access-scenarios"></a>Azure Identität und Access-Szenarien

In diesem Abschnitt werden häufige Identität und Access Szenarien Anwendungsarchitekturen von anderen an. Verwenden Sie die Karte Szenarien für eine schnelle Ausrichtung.

###<a name="aspnet-web-form-application-with-federated-authentication"></a>ASP.NET Web Form-Anwendung mit Partnerverbundkontakte Authentifizierung

In diesem Szenario der Anwendung Architektur möglicherweise die Webanwendung Azure oder lokal bereitgestellt werden. Die Anwendung erfordert, dass Benutzer durch entweder corporate Active Directory oder Internet Identitätsanbieter authentifiziert werden.

Verwenden Sie zum Lösen dieses Szenarios Azure AD-Access-Steuerelement, und Windows Identität Foundation.

![Azure Active Directory Access-Steuerelement][03]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Meine erste Ansprüche unterstützende ASP.NET-Anwendung mit ACS erstellen](http://msdn.microsoft.com/library/gg429779.aspx)
-   [Gewusst wie: Host Login in Ihrer Anwendung ASP.NET Web Seiten](http://msdn.microsoft.com/library/gg185926.aspx)
-   [Gewusst wie: Implementieren Autorisierung in einer Ansprüche unterstützende ASP.NET-Anwendung mit WIF und ACS-Ansprüche](http://msdn.microsoft.com/library/gg185907.aspx)    
-   [Gewusst wie: Implementieren Access Control (RBAC) in eine Ansprüche unterstützende ASP.NET-Anwendung mit WIF und ACS rollenbasierte](http://msdn.microsoft.com/library/gg185914.aspx)
-   [Gewusst wie: Konfigurieren von Vertrauensstellung zwischen ACS und ASP.NET Webanwendungen mithilfe von x. 509-Zertifikaten](http://msdn.microsoft.com/library/gg185947.aspx)
-   [: Code ASP.NET einfach Beispielformulare](http://msdn.microsoft.com/library/gg185938.aspx)

###<a name="wcf-soap-service-with-service-identity"></a>WCF (SOAP)-Dienst mit Service Identität

In diesem Szenario der Anwendung Architektur kann der Dienst WCF (SOAP) Azure oder lokal bereitgestellt werden. Der Dienst wird als eine untergeordnete Dienst von einer Webanwendung oder auch von einem anderen Webdienst zugegriffen. Sie müssen zum Steuern des Zugriffs mit bestimmten Anwendungsidentität. Stellen Sie sich diese im Hinblick auf den Typ des app-Pool-Konto, das Sie in IIS verwendet: zwar die Technologie abweicht, die Ansätze sind vergleichbar, da die Service-Anwendung Umfang Konto im Vergleich zu Ende Benutzerkonto zugegriffen wird.

Verwenden Sie das Feature Dienstidentität im Azure AD-Access-Steuerelement aus.
Dies ist vergleichbar mit dem IIS-app Ressourcenpool Konto von Ihnen verwendete wurden, wenn Sie Ihre apps für Windows Server und IIS bereitstellen. Konfigurieren der Steuerung des Azure AD-Zugriffs auf Problem SAML-Token, das von WIF bei der Dienst WCF (SOAP) behandelt werden.

![WCF (SOAP)-Dienst][04]


Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Hinzufügen von Dienstidentitäten mit einem x 509-Zertifikat, das Kennwort und symmetrischen Schlüssel](http://msdn.microsoft.com/library/gg185924.aspx)
-   [Gewusst wie: Authentifizieren mit einem WCF-Webdienst durch ACS geschützt ist ein Client-Zertifikat](http://msdn.microsoft.com/library/hh289316.aspx)
-   [Gewusst wie: Authentifizierung mit Benutzername und Kennwort zu einem WCF-Webdienst durch ACS geschützt ist](http://msdn.microsoft.com/library/gg185954.aspx)
-   [Beispiel: WCF Zertifikatauthentifizierung](http://msdn.microsoft.com/library/gg185952.aspx)
-   [Beispiel: WCF Username-Authentifizierung](http://msdn.microsoft.com/library/gg185927.aspx)

###<a name="wcf-soap-service-with-federated-authentication-identities-in-active-directory"></a>WCF (SOAP)-Dienst mit Partnerverbundkontakte Authentifizierung Identitäten In Active Directory.

In diesem Szenario der Anwendung Architektur kann der Dienst WCF (SOAP) Azure oder lokal bereitgestellt werden. Sie müssen Steuern des Zugriffs auf ihn mithilfe einer Identität, die durch eine corporate Windows Server Active Directory (AD) verwaltet wird.

Verwenden Azure AD-Access-Steuerelement für die Föderation mit Windows Server AD FS konfiguriert. In diesem Fall müssen Sie nicht mit Azure Active Directory Access Control Service Identität zu konfigurieren. Der Agent, der Zugriff auf den Dienst WCF (SOAP) stellt von Anmeldeinformationen in AD FS und nach der erfolgreichen Authentifizierung wird das Token von AD FS ausgestellt. Das Token ist dann an Azure Active Directory Access Control gesendet und wieder in der Agent erneut ausgegeben. Der Agent verwendet das Token übermitteln Sie die Anforderung an den Dienst WCF (SOAP).

![WCF (SOAP)-Dienst mit AD][05]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Hinzufügen von Dienstidentitäten mit einem x 509-Zertifikat, das Kennwort und symmetrischen Schlüssel](http://msdn.microsoft.com/library/gg185924.aspx)
-   [Gewusst wie: Konfigurieren von AD FS 2.0 als Identitätsanbieter](http://msdn.microsoft.com/library/gg185961.aspx)
-   [Gewusst wie: Verwenden von Verwaltungsdienst AD FS 2.0 als Identitätsanbieter Enterprise konfigurieren](http://msdn.microsoft.com/library/gg185905.aspx)
-   [Beispiel: WCF Partnersuche Authentifizierung mit AD FS 2.0](http://msdn.microsoft.com/library/hh127796.aspx)

###<a name="wcf-rest-service-with-service-identities"></a>WCF (REST)-Dienst mit Service Identitäten

In diesem Szenario kann der Dienst WCF (REST) Azure oder lokal bereitgestellt werden. Der Dienst wird als eine untergeordnete Dienst von einer Webanwendung oder einem anderen Webdienst zugegriffen. Steuern des Zugriffs auf sie verwenden eine anwendungsspezifische Identität Reaktionszeiten davon in Bezug auf die Art des app-Pool-Kontos, die Sie in IIS - verwendet, obwohl die Technologie unterscheidet, müssen Sie die Ansätze sind vergleichbar, da die Service-Anwendung Umfang Konto im Vergleich zu Ende Benutzerkonto zugegriffen wird.

Verwenden Sie das Feature Dienstidentität im Azure AD Access-Steuerelement aus.
Konfigurieren Sie Azure AD-Access-Steuerelement, um einfache Web Token (SWT) Token ausgeben. Behandeln Sie das SWT Token auf der Seite der REST-Dienst können Sie einen benutzerdefinierten token Ereignishandler implementieren und stecken Sie es in der Verkaufspipeline WIF oder kann "manuell" ohne die WIF Infrastruktur analysieren.

Erwägen Sie das folgende Diagramm (WIF ist optional):

![REST-Dienst][06]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Konfigurieren von Vertrauensstellung zwischen ACS und WCF-Diensts mithilfe des symmetrische Schlüssel](http://msdn.microsoft.com/library/gg185958.aspx)
-   [Gewusst wie: Authentifizieren zu einem REST WCF-Webdienst auf Azure mit ACS bereitgestellt](http://msdn.microsoft.com/library/hh289317.aspx)
-   [Beispiel: ASP.NET Webdienst](http://msdn.microsoft.com/library/gg983271.aspx)
-   [Beispiel: Windows Phone 7-Anwendung](http://msdn.microsoft.com/library/gg983271.aspx)
-   [REST WCF mit SWT Token ausgestellt von Azure Access Control Service (ACS)](http://code.msdn.microsoft.com/REST-WCF-With-SWT-Token-123d93c0)

###<a name="wcf-rest-service-with-live-id--microsoft-account-facebook-google-yahoo-open-id"></a>WCF (REST)-Dienst mit Live ID / Microsoft-Konto, Facebook, Gmail, Yahoo!, öffnen Sie die ID

In diesem Szenario kann der Dienst WCF (REST) Azure oder lokal bereitgestellt werden. Sie müssen zum Steuern des Zugriffs mithilfe einer öffentlichen Internetidentität, z. B. Live-ID / Microsoft Account oder Facebook.

Verwenden Sie die Steuerung des Azure AD-Zugriffs auf Problem SWT Token. Behandeln Sie das SWT Token auf der Seite der REST-Dienst können Sie implementieren einen benutzerdefinierten token Ereignishandler und stecken Sie es in eine WIF Verkaufspipeline oder kann "manuell" ohne die WIF Infrastruktur analysieren.

Es ist wichtig, beachten Sie, dass dieses Szenario implementieren, muss die Anwendung Webbrowser-Steuerelement zu verwenden, um die Anmeldeinformationen des Endbenutzers sammeln. Auf diese Weise nicht geeignet für Szenarien, in denen der REST-Dienst aus einer ASP.NET Web app zugegriffen wird. Es ist jedoch nur für Szenarios, in denen der REST-Dienst von Clientanwendung des Benutzers, wie etwa einer app für Windows Phone 7 oder einem Rich-Desktopclient zugegriffen wird, geeignet. Der wichtige Grund für das Webbrowser-Steuerelement abholen ist, dass Internet Identitäten nicht systembedingt aktive Profilszenarien (Web Services-Szenario) unterstützen. Internet Identitäten unterstützen hauptsächlich passiven Profilszenarien (Web apps), die auf Browser leitet Aufsetzen: Hier kommt Webbrowser-Steuerelements praktischen.

Erwägen Sie das folgende Diagramm (WIF ist optional, und somit nicht gezeigt):

![Für WIF ist optional][07]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Authentifizieren zu einem REST WCF-Webdienst auf Azure mit ACS bereitgestellt](http://msdn.microsoft.com/library/hh289317.aspx)
-   [Gewusst wie: Konfigurieren von Google als Identitätsanbieter](http://msdn.microsoft.com/library/gg185976.aspx)
-   [Gewusst wie: Konfigurieren von Facebook als Identitätsanbieter](http://msdn.microsoft.com/library/gg185919.aspx)
-   [So Konfigurieren Sie Yahoo! als Identitätsanbieter](http://msdn.microsoft.com/library/gg185977.aspx)
-  [Beispiel: Windows Phone 7-Anwendung](http://msdn.microsoft.com/library/gg983271.aspx)
-   [REST WCF mit SWT Token ausgestellt von Azure Access Control Service (ACS)](http://code.msdn.microsoft.com/REST-WCF-With-SWT-Token-123d93c0)


###<a name="aspnet-web-app-to-rest-wcf-service-using-shared-swt-token"></a>ASP.NET Web App zum REST WCF-Diensts mithilfe des freigegebenen SWT Token

In diesem Szenario haben Sie eine verteilte Anwendung mit einer Front-End-ASP.NET Web app und eine untergeordnete REST-Dienst und den Endbenutzer Kontext über mehrere physische Ebenen beibehalten möchten. Dies ist manchmal erforderlich, wenn Autorisierungslogik implementieren oder Protokollierung auf den Endbenutzer Identität in der untergeordnete REST-Dienst basiert.

Steuerung des Azure AD-Zugriffs auf Problem SWT Token zu konfigurieren. Das SWT Token ist der Front-End-ASP.NET Web app ausgestellt und klicken Sie dann mit der untergeordnete REST-Dienst freigegeben. In diesem Fall ist nur eine Identitätsempfänger Azure Active Directory Access Control Berechtigungen konfiguriert sind. Es gibt jedoch mehrere Vorsichtsmaßnahmen berücksichtigt werden:

-   Da WIF nicht mit einen SWT token Ereignishandler im Paket bietet müssen Sie implementieren einen benutzerdefinierten token Ereignishandler, mit der ASP.NET Web-app verwendet werden soll. Sie sollten Aufgaben verlassen, die WIF bedeutet das WS-Verbund-Protokoll unterstützt, das auf Browser leitet im Vergleich zu implementieren selbst basiert.
-   Beim Implementieren eines benutzerdefinierten token SWT-Ereignishandler, stellen Sie sicher, dass das bootstrap Token erfüllt werden kann, um sicherzustellen, dass es beibehalten wird.
    Andernfalls können Sie nicht mehr freigeben, und senden es an der untergeordnete REST-Dienst.
-   Sie müssen nicht WIF für einen REST-Dienst verwenden. Stattdessen können Sie die token "manuell" verarbeiten, da es nicht erforderlich ist, in diesem Fall leitet behandeln.

![ASP.NET-Webseite][08]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Konfigurieren von Google als Identitätsanbieter](http://msdn.microsoft.com/library/gg185976.aspx)
-   [Gewusst wie: Konfigurieren von Facebook als Identitätsanbieter](http://msdn.microsoft.com/library/gg185919.aspx)
-   [So Konfigurieren Sie Yahoo! als Identitätsanbieter](http://msdn.microsoft.com/library/gg185977.aspx)
-   [ASP.NET Web App zum REST WCF-Dienst Delegation mit freigegebenen SWT Token](http://code.msdn.microsoft.com/ASPNET-Web-App-To-REST-WCF-b2b95f82)

###<a name="role-based-access-control-rbac-in-claims-aware-applications-and-services"></a>Rollenbasierte Access Control (RBAC) In Ansprüche unterstützende Anwendungen und Dienste

In diesem Szenario müssen Sie Autorisierung in der Web-Anwendung oder einen bestimmten Dienst basierend auf Benutzerrollen implementieren: Benutzer mit erforderlichen Rollen Zugriff und die erforderliche Rollen besitzen verweigert werden. Einfach gesagt, muss der Anwendung einfache Frage - ist der Benutzer in Rolle X?

Es gibt mehrere Methoden zum Beheben dieses Szenario aus. Sie können Azure Active Directory Access Control, WIF Ansprüche Authentication Manager, SamlSecurityTokenRequirement Zuordnung oder Kunden Rollen-Manager.

WIF wird in allen Fällen verwendet. WIF unterstützt die IPrincipal.IsInRole("MyRole")-Methode. In den meisten Fällen ist die Taste, um sicherzustellen, dass Rolle Typ Antrag mit URI des http://schemas.microsoft.com/ws/2008/06/identity/claims/role im Token vorhanden ist, sodass WIF erfolgreich Rollenmitgliedschaft überprüfen kann, wenn die IsInRole-Methode aufgerufen wird.

**Steuerelement Azure AD-Zugriff**. In dieser Implementierung des Steuerelements Azure AD-Zugriff wird Ansprüche Transformation Regel-Engine verwendet. Unter Verwendung der Ansprüche Transformation Regel-Engine Regeln können Sie alle eingehenden Anspruch in einer Rolle Typ anfordern transformieren so, dass IsInRole Methode Anruf erfolgreicher, wenn das Token der Anwendungs Treffer oder ein Dienst WIF kann diese Rolle anfordern, um sicherzustellen, analysieren befindet.

![][09]

**WIF ClaimsAuthenticationManager**. Zeigen Sie diese Implementierung verwenden ClaimsAuthenticationManager als WIFs Erweiterbarkeit auf. Mit diesem Ansatz transformieren Sie alle eingehenden beliebiger Ansprüche in einen Typ Rolle anfordern, bei der Anwendung. Die Komplexität der Transformation wird durch den Code, die Sie schreiben, nur eingeschränkt.

![][10]

**SamlSecurityTokenRequriement Zuordnung**. In dieser Implementierung verwenden Sie die Konfiguration SamlSecurityTokenRequirement in web.config mitteilen WIF die beanspruchen, dass Typen anders, als wären sie Rolle beanspruchen Typen ein. Wenn das Token enthält, eine Gruppe Typs anfordern wird, können Sie es beispielsweise in Anspruch zu Rolle zuordnen. Sie können nur eine einfache Zuordnung mit diesem Ansatz erzielen.

![][11]

**Benutzerdefinierte RoleManager.** In dieser Implementierung können Sie benutzerdefinierte RoleManger implementieren. WIF wird verwendet, um die eingehenden Ansprüche bei der Implementierung von benutzerdefinierter RoleManager Schnittstellenmethoden, wie etwa GetAllRoles() zu prüfen.

![][12]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Implementieren Access Control (RBAC) in eine Ansprüche unterstützende ASP.NET-Anwendung mit WIF und ACS rollenbasierte](http://msdn.microsoft.com/library/gg185914.aspx)
-   [Gewusst wie: Implementieren Token Transformationslogik von Regeln](http://msdn.microsoft.com/library/gg185955.aspx)
-   [Autorisierung mit RoleManager für Ansprüche bewusst (WIF) ASP.NET Webanwendungen](http://blogs.msdn.com/b/alikl/archive/2010/11/18/authorization-with-rolemanager-for-claims-aware-wif-asp-net-web-applications.aspx)
-   Codebeispiel: Verwenden von Ansprüchen In IsInRole in [Windows Identität Foundation SDK](http://www.microsoft.com/downloads/details.aspx?FamilyID=c148b2df-c7af-46bb-9162-2c9422208504)

###<a name="claims-based-authorization-in-claims-aware-applications-and-services"></a>Anspruchsbasierte Autorisierung In Ansprüche unterstützende Anwendungen und Dienste

In diesem Szenario müssen Sie komplexe Autorisierungslogik in Ihrer Website implementieren ist Anwendung oder Dienst und die IsInRole()-Methode nicht für Ihren Anforderungen Autorisierung. Wenn Ihre Autorisierungsansatz Rollen wird von sollten Sie implementieren rollenbasierte Access-Steuerelements im vorherigen Abschnitt beschrieben.

Verwenden Sie ClaimsAuthorizationManager als den Erweiterbarkeitspunkt WIF ein.
ClaimsAuthorizationManager ermöglicht externer Zugriff Kontrollkästchen Anrufe, damit Ihre Anwendungscode sieht übersichtlicher und mehr als beim Zugriff Prüfungen in den Code der Anwendung implementiert gewartet werden kann.

![][13]

Finden Sie in den folgenden Ressourcen dieses Szenario implementiert wird:

-   [Gewusst wie: Implementieren Token Transformationslogik von Regeln](http://msdn.microsoft.com/library/gg185955.aspx)
-   [Gewusst wie: Implementieren Autorisierung in einer Ansprüche unterstützende ASP.NET-Anwendung mit WIF und ACS-Ansprüche](http://msdn.microsoft.com/library/gg185907.aspx)
-   Beispiel: Anspruchsbasierte Autorisierung in [Windows Identität Foundation SDK](http://www.microsoft.com/downloads/details.aspx?FamilyID=c148b2df-c7af-46bb-9162-2c9422208504)


##<a name="azure-storage-service-identity-and-access-scenarios"></a>Azure-Speicher Dienstidentität und Access-Szenarien

In diesem Szenario müssen Sie Zugriff auf Azure-Speicher Blobs und Container sicher freigeben.

Verwenden von freigegebenen Access Signaturen. Verwenden Sie für den Zugriff auf Ihr Speicher-Service-Konto aus Ihrer eigenen Anwendung des gemeinsam verwendeten Hashs, der über Azure-Portal verfügbar ist, wenn Sie konfigurieren und Verwalten Ihrer Speicher Dienstkonten. Wenn Sie möchten, damit eine Person anderen Personen Zugriff auf die Blobs und Container in Ihrem Dienstkonto Speicher freigegeben Access Signaturen URL des verwenden.

Achten Sie insbesondere auf sichere Verwaltung der Informationen zur Vermeidung von seiner Öffnung; Achten Sie auch besonders die Gültigkeit der Signaturen Zugriff freigegeben.

![][14]

Schlagen Sie in den folgenden Ressourcen zum Beheben dieses Szenario

-   [Verwalten des Zugriffs auf Blobs und Container](http://msdn.microsoft.com/library/ee393343.aspx)
-   [Neues Speicherfeature: Gemeinsamen Zugriff Signaturen](http://blog.smarx.com/posts/new-storage-feature-signed-access-signatures)
-   [Freigegebene Access Signaturen sind einfach heutzutage](http://blog.smarx.com/posts/shared-access-signatures-are-easy-these-days)


## <a name="azure-sql-database-identity-and-access-scenarios"></a>SQL Azure-Datenbank Identität und Access-Szenarien


SQL-Datenbank unterstützt nur SQL Server-Authentifizierung. Windows-Authentifizierung (integrierte Sicherheit) wird nicht unterstützt. Benutzer müssen Anmeldeinformationen (Benutzername und Kennwort) jedes Mal angeben, die sie mit einer SQL-Datenbank herstellen. Achten Sie besonders beim Verwalten von Ihren Benutzernamen und Ihr Kennwort ein, um Informationsfreigabe zu vermeiden.


![][15]


Um dieses Szenario zu lösen, finden Sie in den folgenden Hilfethema:<br/>
[Azure SQL-Datenbank-Entwicklung: Hilfethemen](http://msdn.microsoft.com/library/azure/ee621787.aspx)


Oder finden Sie in einem der zugehörigen viele untergeordnete Themen, von denen einige:


- [So: Herstellen einer Verbindung mithilfe von SQL-Datenbank Sqlcmd mit](http://msdn.microsoft.com/library/azure/ee336280.aspx)
- [Beispiel: Wiederholen von Logik zum Herstellen einer Verbindung mit Azure SQL-Datenbank mit ADO.NET](http://msdn.microsoft.com/library/azure/ee336243.aspx)
- [So: Herstellen einer Verbindung mithilfe von PHP SQL-Datenbank mit](http://msdn.microsoft.com/library/azure/ff394110.aspx)
- [So: Herstellen einer Verbindung mit JDBC SQL-Datenbank mit](http://msdn.microsoft.com/library/azure/gg715284.aspx)


Oder verweisen auf:<br/>
[Richtlinien für SQL Azure-Datenbank Sicherheit und Einschränkungen](http://msdn.microsoft.com/library/azure/ff394108.aspx#authentication)


##<a name="azure-service-bus-identity-and-access-scenarios"></a>Azure Service Bus Identität und Access-Szenarien

Die Dienstbus und Azure Active Directory Access Control haben eine besondere Beziehung, da jeder Dienstbus Dienstnamespace mit einem übereinstimmenden Access Control Service Namespace mit demselben Namen, mit dem Suffix gepaart ist "-Sb". Der Grund für diese spezielle Beziehung ist in der Anzeige von Dienstbus und Access Control deren gemeinsamen Vertrauensstellung und die zugehörigen cryptographic Schlüssel verwalten. Schlagen Sie in den Ressourcen unten für weitere Details.

![][16]

Finden Sie in den folgenden Ressourcen zum Beheben dieses Szenario:

-   [Sichern von Dienstbus mit ACS](http://channel9.msdn.com/posts/Securing-Service-Bus-with-ACS) (Video)
-   [Sichern von Dienstbus mit ACS](https://skydrive.live.com/view.aspx?cid=123CCD2A7AB10107&resid=123CCD2A7AB10107%211849) (Folien)
-   [Service Bus Authentifizierung und Autorisierung über die Access-Dienstleistung](http://msdn.microsoft.com/library/hh403962.aspx)

##<a name="in-memory-cache-identity-and-access-scenarios"></a>In-Memory-Cache Identität und Access-Szenarien

In-Memory-Cache (ehemals Azure Cache) beruht auf Azure AD-Access-Steuerelement für die Authentifizierung. Verfügbar bis Verwaltungsportal freigegebenen Schlüssel verwendet. Verwenden Sie die Tasten in Code oder Konfiguration Dateien ein, wenn den Cache zugreifen. Achten Sie darauf, speichern die Tasten sicher weitere Offenlegung von Informationen zu vermeiden.

![][17]


Finden Sie in den folgenden Ressourcen zum Beheben dieses Szenario:

-   [How to: Cache-Client programmgesteuert zum Zwischenspeichern von Azure konfigurieren](http://msdn.microsoft.com/library/windowsazure/gg618003.aspx)
-   [So: Konfigurieren eines mithilfe der Konfigurationsdatei der Anwendung zum Zwischenspeichern von Azure-Cache-Clients](http://msdn.microsoft.com/library/windowsazure/gg278346.aspx)
-   [Azure Service Bus und Zwischenspeichern Beispiele](http://msdn.microsoft.com/library/ee706741.aspx) (Beispiele Abschnitt Zwischenspeichern)

##<a name="azure-marketplace-identity-and-access-scenarios"></a>Azure Marketplace-Identität und Access-Szenarien

Jeder Zugriff auf ein Dataset Azure Marketplace muss, ob kostenlose oder kostenpflichtiges, den Benutzer authentifizieren, bevor der Zugriff gewährt wird. Wenn Sie eine Anwendung der Authentifizierungsprozess erstellen, müssen in Ihrem Code enthalten sein. Berücksichtigen Sie die folgenden allgemeinen Szenarien:

###<a name="i-access-my-dataset"></a>Kann ich zugreifen Meine Dataset

In diesem Szenario erstellen Sie eine Anwendung, die in Ihrem Abonnement Marketplace Datasets nutzt. Sie können die Benutzer in der Anwendung.
Die Anwendung sein kann entweder auf Azure bereitgestellt lokalen oder Marketplace.

Verwenden des freigegebenen Schlüssels, der über Ihr Abonnement Marketplace verfügbar ist. Sie erhalten den freigegebenen Schlüssel über das Marketplace-Portal.

![][18]

Finden Sie in den folgenden Ressourcen zum Beheben dieses Szenario:

-   [Verwenden Sie in Ihrer App Store HTTP Basic-Authentifizierung](http://msdn.microsoft.com/library/gg193417.aspx)

###<a name="users-access-my-datasets"></a>Benutzern Zugriff auf Meine Datasets

In diesem Szenario erstellen Sie eine Anwendung, die Benutzer Ihre Dataset zugreifen können. Die Anwendung bereitgestellt werden kann, auf Azure lokalen oder Marketplace.

Verwenden Sie zum Beheben dieses Szenario OAuth Delegation aus. Benutzer werden aufgefordert, ihren Live-ID angeben / Microsoft Account Anmeldeinformationen, und klicken Sie dann diese werden durch das Verfahren Zustimmung übernommen.

![][19]

Finden Sie in den folgenden Ressourcen zum Beheben dieses Szenario:

-   [Beispiel für OAuth Web-Client](http://go.microsoft.com/fwlink/?LinkId=219162)
-   [OAuth Rich Client-Beispiel](http://go.microsoft.com/fwlink/?LinkId=219163)

###<a name="application-access-marketplace-api"></a>Anwendung Access Marketplace-API

In diesem Szenario erstellen Sie eine Anwendung, die die Marketplace-API greift auf. Der Marketplace-API erfordert eine Authentifizierung, damit Anrufe erfolgreich zufriedenzustellen. Die Anwendung wird in Azure Marketplace bereitgestellt.

![][20]

Wenden Sie sich an den Marketplace-Kit für die Details der Implementierung Authentifizierung veröffentlichen.

Finden Sie in den folgenden Ressourcen zum Beheben dieses Szenario:

-   [Herunterladen der App Kit veröffentlichen](http://go.microsoft.com/fwlink/?LinkId=221323)
-   [Einführung in Azure Marketplace für Applikationen](https://datamarket.azure.com/)

##<a name="security-knobs"></a>Sicherheit Regler

In diesem Abschnitt werden die Sicherheit Regler für Windows Identität Foundation und Azure Active Directory Access Control. Verwenden Sie es als Checkliste für die grundlegende Sicherheit für diese Technologien beim Entwerfen und Bereitstellen der Anwendung.

###<a name="windows-identity-foundation"></a>Windows-Identität Foundation

Im folgenden sind die wichtigsten Sicherheit Regler von WIF. Die folgenden Informationen finden Sie eine Zusammenfassung von [WIF Entwurfsaspekte](http://msdn.microsoft.com/library/ee517298.aspx) und [Windows Identität Foundation (WIF) Sicherheit for ASP.NET Web Applications - Angriffen und Gegenmaßnahmen](http://blogs.msdn.com/b/alikl/archive/2010/12/02/windows-identity-foundation-wif-security-for-asp-net-web-applications-threats-amp-countermeasures.aspx) .

-   **IssuerNameRegistry**. Gibt die vertrauenswürdigen Security Token Service (STS) an. Stellen Sie sicher, dass nur vertrauenswürdigen STS aufgelistet werden.
-   **CookieHandler RequireSsl = "true"**. Gibt an, ob die Sitzungscookies über das SSL-Protokoll übertragen.
-   **des WsFederation s = "true"**. Gibt an, ob die Föderation Protokoll Kommunikation mit Identitätsanbieter über SSL-Protokoll durchgeführt.
-   **TokenReplayDetection aktiviert = "true"**. Gibt an, ob token Wiedergabe Erkennung Feature aktiviert ist. Achten Sie darauf, dass dieses Feature Zugehörigkeit Server erstellt, wie sie lokale Kopien von verwendeten Token verwaltet werden.
-   **AudienceUris**. Gibt die Zielgruppe des das Token an. Wenn die Anwendung ein Token empfängt, die für Ihre app nicht bestimmt wurde, wird es von WIF abgelehnt werden.
-   **RequestValidation** und **HttpRuntime RequestValidationType**.
    ASP.NET Überprüfung Feature aktiviert/deaktiviert. Finden Sie unter Anleitungen, wie umrandet [Windows Identität Foundation (WIF): A potenziell gefährlicher Request.Form Wert wurde nicht erkannt vom Client](http://social.technet.microsoft.com/wiki/contents/articles/1725.windows-identity-foundation-wif-a-potentially-dangerous-request-form-value-was-detected-from-the-client-wresult-t-requestsecurityto.aspx)

###<a name="azure-ad-access-control"></a>Azure AD-Access Control

Berücksichtigen Sie die folgenden Sicherheit Regler in Azure Active Directory Access Control Bereitstellung. Die folgenden Informationen finden Sie eine Zusammenfassung von [Richtlinien für ACS-Sicherheit](http://msdn.microsoft.com/library/gg185962.aspx) und [Zertifikate und Richtlinien für die Verwaltung von Tasten](http://msdn.microsoft.com/library/hh204521.aspx).

-   **STS Token Ablauf**. Verwenden Sie Azure Active Directory Access Control Verwaltungsportal anspruchsvollen token Ablauf festlegen.
-   **Datenüberprüfung beim Verwenden des Features URL zurück**. Azure Active Directory Access Steuerelement Fehler URL-Funktion erfordert anonymen Zugriff auf die app-Seite, wo es Fehlermeldungen sendet. Nehmen Sie alle Daten, die auf dieser Seite als gefährlicher aus nicht vertrauenswürdigen Quelle stammen an.
-   **Verschlüsseln von Token für vertrauliche Szenarien**. Um das Risiko von Informationen zu verringern Offenlegung in Form von das Token Verschlüsselung in Betracht ziehen der Tokens.
-   **Verschlüsseln von Cookies RSA bei der Bereitstellung für Azure verwenden**.
    WIF werden Cookies standardmäßig mithilfe von DPAPI verschlüsselt. Server-Zugehörigkeit erstellt und dazu führen, dass Ausnahmen beim Webfarm und Azure-Umgebungen bereitgestellt. Verwenden Sie stattdessen RSA in Webfarm und Azure Szenarien.
-   **Bei der Anmeldung Zertifikate token**. Erneuern Sie Tokensignaturzertifikate regelmäßig zur Vermeidung von DOS-Dienst an. Azure AD-Access Control meldet alle Sicherheitstokens, die es Probleme. X. 509-Zertifikate werden zum Signieren, wenn Sie eine Anwendung erstellen, der SAML-Token ausgestellt von ACS verwendet. Wenn signierenden Zertifikate ablaufen erhalten Sie Fehler bei dem Versuch, ein Token anfordern.
-   **Token Schlüssel signieren**. Erneuern Sie token signierenden Schlüssel regelmäßig, um DOS-Dienst zu vermeiden. Azure AD-Access Control meldet alle Sicherheitstokens, die es Probleme. 256-Bit-symmetrische signierenden Schlüssel werden verwendet, wenn Sie eine Anwendung erstellen, der SWT Token ausgestellt von ACS. Nach dem Ablauf signierenden Schlüssel erhalten Sie Fehler bei dem Versuch, ein Token anfordern.
-   **Verschlüsselungszertifikate token**. Erneuern Sie token Verschlüsselungszertifikate regelmäßig zur Vermeidung von DOS-Dienst an. Token Verschlüsselung ist erforderlich, wenn die Anwendung eines sich verlassen Anbieters ein Webdienst-Nachweis der Besitz Token über das Protokoll WS-Trust in anderen Fällen token-Verschlüsselung mit optional ist. Wenn Sie Verschlüsselungszertifikate ablaufen erhalten Sie Fehler bei dem Versuch, ein Token anfordern.
-   **Token Zertifikate entschlüsseln**. Erneuern Sie token entschlüsseln Zertifikate regelmäßig zur Vermeidung von DOS-Dienst an. Azure Active Directory Access Control können verschlüsselte Token von WS-Verbund Identitätsanbieter (z. B. AD FS 2.0) übernehmen. Ein x 509-Zertifikat in Azure Active Directory Access Control gehostet wird zum Entschlüsseln verwendet.
    Wenn entschlüsseln Zertifikate ablaufen erhalten Sie Fehler bei dem Versuch, ein Token anfordern.
-   **Dienst Identitätsanmeldeinformationen**. Erneuern Sie Dienstidentität Anmeldeinformationen regelmäßig zur Vermeidung von DOS-Dienst an. Dienstidentitäten verwenden Anmeldeinformationen, die konfiguriert werden global für Ihren Azure Active Directory Access Control Namespace, mit denen Applikationen oder Clients direkt mit Azure Active Directory Access Control authentifizieren und ein Token zu erhalten. Es gibt drei Arten von Anmeldeinformationen, die Azure Active Directory Access Control Service Identität zugeordnet werden kann: symmetrischen Schlüssel, das Kennwort und x. 509-Zertifikat. Starten Sie Ausnahme empfangen, wenn die Anmeldeinformationen abgelaufen sind.
-   **Anmeldeinformationen Steuerelement-Verwaltungsdienst Azure AD-Zugriff**. Erneuern Sie Management Service Anmeldeinformationen regelmäßig zur Vermeidung von DOS-Dienst an. Der Azure Active Directory Access Steuerelement-Verwaltungsdienst ist eine wichtige Komponente, die Sie programmgesteuert verwalten und Konfigurieren von Einstellungen für Ihren Azure Active Directory Access Control Namespace ermöglicht. Es gibt drei Arten von Anmeldeinformationen, denen die Verwaltung Dienstkontos zugeordnet werden kann. Dies sind symmetrischen Schlüssel, das Kennwort und eine x 509-Zertifikat. Starten Sie Ausnahme empfangen, wenn die Anmeldeinformationen abgelaufen sind.
-   **WS-Verbund Identitätsanbieter signieren und Verschlüsselungszertifikate**. Abfrage für die Gültigkeit des Zertifikats WS-Verbund-Identitätsanbieter DOS-Dienst zu vermeiden. WS-Verbund Identität Anbieter Zertifikat ist über seine Metadaten verfügbar.
    Beim Konfigurieren der Identitätsanbieter WS-Verbund, wie z. B. AD FS wird das WS-Verbund Signaturzertifikat über WS-Verbund Metadaten verfügbar über die URL oder als Datei konfiguriert. Nach der WS-Verbund Identitätsanbieter konfiguriert mithilfe von Azure Active Directory Access Control-Verwaltungsdienst es für seine Validness Zertifikate Abfragen. Beim Ablauf des Zertifikats wird gestartet Ausnahmen empfangen.

##<a name="shared-hosting-using-azure-websites"></a>Gemeinsames Hosting mithilfe Azure-Websites

Alle Szenarien und Lösungen, die in diesem Artikel beschrieben sind gültig, wenn die Anwendung auf Websites Azure gehostet wird.

##<a name="azure-virtual-machines"></a>Azure-virtuellen Computern

Alle Szenarien und Lösungen, die in diesem Artikel beschrieben sind gültig, wenn die Anwendung auf Azure virtuellen Computern gehostet wird.

##<a name="resources"></a>Ressourcen

-   [Identität Entwicklertools Schulungskit](http://go.microsoft.com/fwlink/?LinkId=214555)
-   [MSDN gehostete Identität Entwicklertools Schulungskurs](http://go.microsoft.com/fwlink/?LinkId=214561)
-   [Einen Leitfaden zum anspruchsbasierte Identität und Access-Steuerelement](http://go.microsoft.com/fwlink/?LinkId=214562)
-   [Access Control Service](http://msdn.microsoft.com/library/windowsazure/gg429786.aspx)
-   [So ACS des](http://msdn.microsoft.com/library/windowsazure/gg185939.aspx)
-   [Secure Azure Rolle ASP.NET Web Webanwendung mit Access Service Version 2.0 steuern](http://social.technet.microsoft.com/wiki/contents/articles/2590.aspx)
-   [Azure AD-Access Steuerelement Service (ACS) Academy Videos](http://social.technet.microsoft.com/wiki/contents/articles/2777.aspx)
-   [Microsoft-Sicherheits-Entwicklungszyklus](http://www.microsoft.com/security/sdl/default.aspx)
-   [SDL Threat Modeling Tool 3.1.8](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2955)
-   [Sicherheit und Datenschutz-Blogs](http://www.microsoft.com/about/twc/en/us/blogs.aspx)
-   [Antwort Sicherheitscenter](http://www.microsoft.com/security/msrc/default.aspx)
-   [Security Intelligence Report](http://www.microsoft.com/security/sir/)
-   [Sicherheits-Entwicklungszyklus](http://www.microsoft.com/security/sdl/default.aspx)
-   [Security Developer Center (MSDN)](http://msdn.microsoft.com/security/)


[01]:./media/SecurityRX/01_SecuringTheApplication.gif
[02]:./media/SecurityRX/02_ThreatsVulnerabilitiesandAttacks.gif
[03]:./media/SecurityRX/03_WindowsAzureADAccesscontrol.gif
[04]:./media/SecurityRX/04_WCF(SOAP)Service.gif
[05]:./media/SecurityRX/05_AzureADAccessControl.gif
[06]:./media/SecurityRX/06_RESTService.gif
[07]:./media/SecurityRX/07_WIFisOptional.gif
[08]:./media/SecurityRX/08_ASPNETWebApptoREST.gif
[09]:./media/SecurityRX/09_RBAC.gif
[10]:./media/SecurityRX/10_WIFClaimsAuthenticationManager.gif
[11]:./media/SecurityRX/11_SecurityTokenRequriementmapping.gif
[12]:./media/SecurityRX/12_CustomRoleManager.gif
[13]:./media/SecurityRX/13_ClaimsAuthorizationManager.gif
[14]:./media/SecurityRX/14_WindowsAzurestorage.gif
[15]:./media/SecurityRX/15_SQLAzureIdentityandAccessScenarios.gif
[16]:./media/SecurityRX/16_WindowsAzureServiceBusIdentity.gif
[17]:./media/SecurityRX/17_WindowsAzureCacheIdentity.gif
[18]:./media/SecurityRX/18_IAccessMyDataset.gif
[19]:./media/SecurityRX/19_UsersAccessMyDatasets.gif
[20]:./media/SecurityRX/20_ApplicationAccessMarketplaceAPI.gif

[Web SSO Design]: http://technet.microsoft.com/library/dd807033(WS.10).aspx
[Federated Web SSO Design]: http://technet.microsoft.com/library/dd807050(WS.10).aspx

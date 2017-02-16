<properties 
    pageTitle="Die Funktionalität des Betriebssystems Azure-App-Verwaltungsdienst" 
    description="Erfahren Sie mehr über die verfügbaren Web apps, mobile-app Downloadzeit und API-apps auf App-Verwaltungsdienst Azure OS-Funktionen" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Die Funktionalität des Betriebssystems Azure-App-Verwaltungsdienst #

In diesem Artikel werden die gemeinsame geplante: das Betriebssystem von Funktionen, die für alle apps auf [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714)ausgeführt verfügbar ist. Diese Funktionalität umfasst die Datei, Netzwerk, und Zugriff auf die Registrierung und Diagnose Protokolle und Ereignisse. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>App-Dienst Plan Ebenen

App-Dienst wird Kunden apps in einer Hostinganbieter mit mehreren Mandanten-Umgebung. Apps, die in den **Free** und **freigegebene** Ebenen bereitgestellt führen Sie im Arbeitsprozesse auf freigegebenen virtuellen Computern während apps, die in der **Standard-** und **Premium** Ebenen bereitgestellt auf speziell für die apps, die einen einzelnen Kunden zugeordnet dedizierte virtuelle Computer ausgeführt werden.

Da die App-Dienst Skalierung problemlos zwischen verschiedenen Ebenen unterstützt, bleibt die Sicherheitskonfiguration für App-Dienst apps erzwungen gleich aus. Dadurch wird sichergestellt, dass apps plötzlich, nicht unerwartetes, weiß nicht abweichendem, wenn die App-Serviceplan auf verschiedenen Ebenen in ein anderes wechselt.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Entwicklung Framework

App-Dienst Preisgestaltung Ebenen steuern, wie viele berechnen Ressourcen (CPU, Festplattenspeicher, Arbeitsspeicher und Netzwerk Ausgang) apps zur Verfügung. Die große Bandbreite von Framework verfügbar sind, um apps bleibt jedoch gleich, unabhängig von der Skalierung Ebenen.

App-Dienst unterstützt, eine Vielzahl von Entwicklung Framework, in ASP.NET, klassischen ASP, node.js, einschließlich PHP und Python - des als Erweiterungen innerhalb von IIS ausgeführt wird. Um zu vereinfachen und Konfiguration der Sicherheit normalisieren, ausführen apps App-Dienst in der Regel die verschiedenen Entwicklung Framework mit Standardeinstellungen. Ein Verfahren zum Konfigurieren von apps hätte API-Oberfläche und Funktionalität für jedes einzelne Entwicklungsframework anpassen. App-Dienst bietet einen eher generischen Ansatz stattdessen durch eine gemeinsame Grundlinie der Funktionalität des Betriebssystems unabhängig davon, eine app Entwicklungsframework aktivieren.

In den folgenden Abschnitten werden die allgemeinen Arten von Funktionalität des Betriebssystems App Dienst apps zur Verfügung.

<a id="FileAccess"></a>
##<a name="file-access"></a>Zugriff auf die Datei

Verschiedene Laufwerke befinden sich innerhalb der App-Dienst, einschließlich der lokalen und Netzwerklaufwerke.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Lokale Laufwerke

Kurz gesagt ist App einen Dienst auf der Infrastruktur Azure PaaS (Plattform als Dienst) ausgeführt. Daher sind die lokalen Laufwerke, die "mit einem virtuellen Computer angefügt sind" den gleichen Laufwerktypen verfügbar alle Worker-Rolle in Azure ausgeführt. Dies umfasst eine Betriebssystem-Laufwerk (Laufwerk D:\), eine Anwendung-Laufwerk, Azure-Paket Cspkg Dateien ausschließlich von App-Dienst verwendet wird (und Kunden Sehkraft) enthält, und ein "Benutzer"-Laufwerk (das Laufwerk C:\),, dessen Größe je nach der Größe der virtuellen Computer variiert.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Vernetzten Laufwerken (QuickInfos UNC teilt)

Eine eindeutige Aspekte des App-Dienst, der app-Bereitstellung und Wartung recht einfach ist ist, dass alle Inhalte von Benutzern auf eine Reihe von UNC Freigaben gespeichert ist. Dieses Modell ordnet sehr gut die allgemeine Muster Inhalt Speicher verwendeten lokalen Web-hosting-Umgebungen, die mehrere Lastenausgleich Server verfügen. 

Innerhalb der App-Verwaltungsdienst, es gibt Zahl UNC Freigaben in jedem Datacenter erstellt. Rang der Benutzerinhalt für alle Kunden in jedem Datacenter wird zugewiesen auf jede UNC freigeben. Darüber hinaus haben alle den Dateiinhalt für einen einzelnen Kunden Abonnement immer auf dem gleichen UNC platziert wird. 

Hinweis: aufgrund wie von Cloud-Dienste, eine Freigabe UNC Host für bestimmte virtuellen Computers ändern sich im Laufe der Zeit. Es ist sichergestellt, dass UNC Freigaben von anderen virtuellen Computern bereitgestellt werden, wie sie während der normalen Cloud Vorgänge nach oben oder unten geschaltet werden. Daher sollten apps hartcodierte Annahmen nie stellen Sie, dass die Computerinformationen in einem Dateipfad UNC unveränderliche über einen Zeitraum bleibt. Sie sollten verwenden Sie stattdessen die geeignete *faux* absoluten Pfad **D:\home\site** , die App-Dienst bereitgestellt. Dieser faux absolute Pfad bietet eine portable app-und-Benutzer-unabhängige Methode zum Verweisen auf einen eigenen app. Mithilfe von **D:\home\site**kann eine freigegebene Dateien von app zu app übertragen, ohne einen neuen absoluten Pfad für jede Übertragung konfigurieren zu müssen.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Typen von Dateizugriff gewährt einer app

Abonnement des Kunden hat eine Verzeichnisstruktur reservierte auf einer bestimmten UNC Freigabe in einem Data Center. Ein Kunde möglicherweise mehrere apps erstellt in einem bestimmten Data Center, damit alle Verzeichnisse, gehören zu einem einzelnen Kundenabonnement auf der gleichen UNC erstellt werden, die freigeben. Die Freigabe kann Verzeichnisse wie für Inhalt, Fehler und Diagnoseprotokolle und früheren Versionen von der app erstellte Datenquellen-Steuerelements enthalten. Wie erwartet, sind für Lese- und Schreibzugriff zur Laufzeit von des app Anwendungscode eines Kunden app Verzeichnisse verfügbar.

Klicken Sie auf den lokalen Laufwerken angefügter des virtuellen Computers, die eine app ausgeführt wird, reserviert App-Dienst einen Ausschnitt Speicherplatz auf dem Laufwerk C:\ für die app-spezifische temporäre lokale Speicherung. Zwar eine app vollständigen Lese-und Schreibzugriff auf ihren eigenen temporären lokalen Speicher aufweist, nicht zur Verfügung, Speichers wirklich soll direkt vom Code Anwendung verwendet werden. Lieber, ist die Absicht Speicherung temporärer Dateien für IIS und Web-Anwendung Framework bereitstellen. App-Dienst beschränkt auch die temporäre lokale Speicherplatz für jede app, um zu verhindern, dass einzelne apps übermäßige Mengen von lokalen Dateispeicher Verarbeitung verfügbar.

Zwei Beispiele für wie App-Dienst temporäre lokale Speicherung verwendet werden Verzeichnis für temporäre Dateien von ASP.NET und Verzeichnis für IIS komprimierte Dateien. Das ASP.NET-Kompilierungssystem verwendet Verzeichnis "Temporäre ASP.NET-Dateien" als eine temporäre Kompilierung Cachespeicherort. IIS verwendet das Verzeichnis "IIS temporäre komprimierte Dateien" zum Speichern von komprimierten Antwort ein. Beide der folgenden Arten von Datei Verwendung (ebenso wie andere) werden in den temporären lokalen Speicher pro-app im App-Dienst erneut zugeordnet. Diese neu zuordnen, ist sichergestellt, dass die Funktionalität weiterhin wie erwartet.

Jede app im App-Dienst wird als eine zufällige eindeutige geringen Arbeitsprozessidentität namens "Anwendungspoolidentität", hier beschriebenen weiteren ausgeführt: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Anwendungscode wird diese Identität für grundlegende schreibgeschützten Zugriff auf das Betriebssystem vorbehalten (das Laufwerk D:\) verwendet. Dies bedeutet Anwendungscode Liste Allgemeine Directory-Strukturen und allgemeine Dateien auf Laufwerk des Betriebssystems lesen kann. Zwar dies möglicherweise etwas breiter Ebene von Access, die gleichen Verzeichnisse und Dateien zugegriffen werden, wenn Sie die Bereitstellung werden scheinen eine Worker-Rolle in einer Azure Service gehostet, und Lesen Sie den Laufwerksinhalt. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Dateizugriff über mehrere Instanzen

Start Verzeichnis enthält eine app Inhalten und Anwendungscode darauf schreiben kann. Wenn eine app für mehrere Instanzen ausgeführt wird, ist das home-Verzeichnis für alle Instanzen freigegeben, damit alle Instanzen finden Sie unter demselben Verzeichnis. Ja, wenn eine app hochgeladene Dateien im Stammverzeichnis speichert, werden beispielsweise diese Dateien sofort für alle Instanzen zur Verfügung. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Netzwerkzugriff
Anwendungscode können auf TCP/IP, und UDP-basierte Protokolle ausgehende vornehmen Netzwerk Verbindungen mit Internet zugänglich Endpunkte, die externe Diensten verfügbar machen. Diese dieselben Protokolle können Apps mit Webdiensten innerhalb von Azure & #151; z. B. durch Herstellen einer HTTPS-Verbindung zu SQL-Datenbank herstellen.

Es gibt auch eine eingeschränkte Funktionalität für eine lokale Loopback-Verbindung herstellen, und die lokale Loopback Sockets warten app-apps. Dieses Feature ist hauptsächlich zum Aktivieren von apps, die als Teil ihrer Funktionalität lokale Loopback Sockets Abfragen vorhanden. Beachten Sie, dass jede app eine Loopbackverbindung "Privat" sieht; eine lokale Loopback Sockets eingesetzten app "B" kann nicht App "A" hören möchten.

Named Pipes werden auch als eine Methode der Kommunikation zwischen Prozessen (IPC) zwischen verschiedenen Prozessen unterstützt, die gemeinsam eine app ausgeführt werden. Beispielsweise beruht das Modul IIS FastCGI auf named Pipes, um die einzelnen Prozesse zu koordinieren, die Ausführen von PHP-Seiten.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Ausführung von Code, Prozesse und Arbeitsspeicher
Wie bereits zuvor erwähnt, führen Sie apps innerhalb geringen Arbeitsprozesse eine zufällige Anwendung Ressourcenpool Identität verwenden. Anwendungscode hat Zugriff auf den Speicherbereich Worker-Prozess sowie alle untergeordneten Prozessen, die erzeugt werden möglicherweise durch CGI-Prozesse oder einer anderen Anwendung zugeordnet. Jedoch zugreifen nicht eine app Speicher oder Daten von einer anderen app, auch wenn es auf dem gleichen virtuellen Computer ist.

Apps können Skripts oder Seiten mit unterstützte Web Development Framework geschrieben ausgeführt werden. App-Dienst konfigurieren keine Einstellungen Web Framework mehr eingeschränkten Modi. Führen Sie beispielsweise ASP.NET apps auf App-Dienst ausgeführt in "" vertrauenswürdigem im Gegensatz zu einem mehr eingeschränkten Trust-Modus. COM-Komponenten (aber nicht Out-of-Prozess COM-Komponenten) wie ADO (ActiveX Data Objects), die auf dem Windows-Betriebssystem standardmäßig registriert sind, können Web Framework, einschließlich klassischen ASP und ASP.NET, anrufen.

Apps können Sie erzeugen und beliebigen Code ausführen. Ist es eine App Aufgaben wie dem Erstellen eines Prozesses eine Befehls-Shell oder Ausführen eines PowerShell-Skripts zulässig. Obwohl beliebigen Code und Prozessen aus einer app erzeugt werden können, sind jedoch ausführbare Programme und Skripts auf Berechtigungen von der übergeordneten Anwendung Ressourcenpool noch beschränkt. Beispielsweise kann eine app erzeugen eine ausführbare Datei, die einen ausgehenden HTTP-Anruf macht, aber kann nicht gleich ausführbare versuchen, die IP-Adresse eines virtuellen Computers von deren NIC aufzuheben Tätigen eines Anrufs ausgehenden Netzwerk geringen Code darf, aber bei dem Versuch, deren Einstellungen im Netzwerk auf einem virtuellen Computer neu zu konfigurieren sind Administratorrechte erforderlich.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnose Protokolle und Ereignisse
Protokollinformationen ist Daten, die einige apps versuchen, für den Zugriff auf einen weiteren Satz. Welche Arten von Protokollinformationen Code im App-Dienst zur Verfügung diagnostic enthält und Protokollinformationen generiert werden, indem Sie eine app, die auch bei der app einfach zugänglich ist. 

Beispielsweise stehen W3C HTTP-Protokolle, die von einer aktiven app generiert entweder auf ein Verzeichnis Log in den Speicherort der Netzwerkfreigabe erstellt für die app oder im BLOB-Speicher zur Verfügung, wenn ein Kunde W3C-Protokollierung Speicher eingerichtet hat. Die zweite Option ermöglicht große Mengen von Protokolle, ohne das Risiko von bis zu der Datei von Speichergrenzwerten einer Netzwerkfreigabe zugeordnet erfasst werden.

In einer ähnlichen Vein, in Echtzeit Diagnoseinformationen aus .NET apps kann auch protokolliert werden Optionen aus, um die Spur Informationen auf der app-Netzwerkfreigabe, schreiben mithilfe der .NET Tracing und Diagnose Infrastruktur, oder Sie können auch an einem Speicherort der Blob-Speicher.

Bereiche Diagnose Protokollierung und Spur, die nicht zur Verfügung stehen, um apps sind Windows ETW Events und allgemeine Windows-Ereignisprotokollen (z. B. System, Anwendung und Sicherheit Ereignisprotokollen). Da ETW Spur Informationen potenziell sichtbaren Computer organisationsweite (mit der rechten Seite ACLs) werden kann, sind Lese- und Schreibzugriff auf ETW-Ereignisse blockiert. Entwickler möglicherweise Beachten Sie, dass API zum Lesen und Schreiben von ETW Ereignisse und allgemeine Windows-Ereignisprotokollen zu arbeiten, aber dies liegt daran App Service "Anrufe abfangen ist" ruft, damit sie angezeigt werden, erfolgreich durchgeführt werden kann. In der Praxis hat der Anwendungscode keinen Zugriff auf diese Ereignisdaten.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Zugriff auf die Registrierung
Apps haben schreibgeschützten Zugriff zu viele (jedoch nicht alle) der Registrierung des virtuellen Computers auf ausführen. Methode bedeutet dies, dass die Registrierungsschlüssel, die schreibgeschützten Zugriff auf den lokalen Benutzergruppe zulassen von apps zugegriffen werden. Ein Bereich, der Registrierung, die für entweder Lese- oder Schreibzugriff derzeit nicht unterstützt wird, ist der HKEY\_aktuellen\_Struktur Benutzers.

Schreibzugriff auf die Registrierung ist blockiert, einschließlich Zugriff auf alle Registrierungsschlüssel pro Benutzer. Im Hinblick auf die app Schreibzugriff auf die Registrierung sollten nie zuverlässig in der Azure-Umgebung da apps können (führen) auf verschiedenen virtuellen Computern migriert. Der nur beständige beschreibbare Speicher, der auf einer App abhängig sein kann ist die pro-app Verzeichnis des Websiteinhalts-Struktur, die auf die App-Dienst UNC Freigaben gespeichert. 

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 

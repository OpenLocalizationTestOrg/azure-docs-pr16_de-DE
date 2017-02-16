<properties
    pageTitle="Bewährte Methoden für die App-Verwaltungsdienst Azure"
    description="Erfahren Sie, best Practices und Problembehandlung bei Azure-App-Verwaltungsdienst."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Bewährte Methoden für die App-Verwaltungsdienst Azure

In diesem Artikel sind bewährte Methoden für die Verwendung von [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714)zusammengefasst. 

## <a name="a-namecolocationacolocation"></a><a name="colocation"></a>Colocation
Die Effekte können Folgendes enthalten, wenn Azure Ressourcen Verfassen einer Lösung wie eine Web app und einer Datenbank in unterschiedlichen Regionen ansässig sind:

*  Höhere Wartezeit Kommunikation zwischen Ressourcen
*  Monetäre Gebühren für ausgehende Daten übertragen Cross Region wie auf der [Seite mit Azure Preisgestaltung](https://azure.microsoft.com/pricing/details/data-transfers)erwähnt.

In der gleichen Region Colocation empfiehlt sich für Azure Ressourcen Verfassen einer Lösung wie eine Web app und einer Datenbank oder Speicher-Konto verwendet, um Inhalte oder Daten aufzunehmen. Beim Erstellen von Ressourcen, die Sie sicherstellen, sollte sind sie in der gleichen Azure Region, es sei denn, Sie verfügen über bestimmte Business oder Grund dafür nicht zu entwerfen. Sie können eine App-Service-app durch Nutzung der [App-Dienst Klonen Feature](app-service-web-app-cloning-portal.md) derzeit verfügbar für planen zu Premium App-Service-apps auf den Bereich Ihrer Datenbank verschieben.   

## <a name="a-namememoryresourcesawhen-apps-consume-more-memory-than-expected"></a><a name="memoryresources"></a>Wenn mehr Speicher als erwartet nutzen apps
Wenn Sie feststellen eine app wird mehr Speicher als erwartet wie über die Überwachung angegeben belegt oder Dienst Empfehlungen erwägen Sie das [Feature App Dienst Automatische Korrektur](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Eine der Optionen für die automatische Korrektur Feature sehr benutzerdefinierte Aktionen basierend auf ein Speichergrenzwert. Aktionen umfassen das Spektrum von e-Mail-Benachrichtigungen zu Untersuchung über Arbeitsspeicher Abbild an Ort und Stelle Reduzierung durch Wiederverwendung Worker-Prozess ein. Automatische Korrektur kann über web.config und über eine benutzerfreundliche Benutzeroberfläche wie in diesem Blogbeitrag für die [App Service Support Website Erweiterung](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)am beschrieben konfiguriert werden.   

## <a name="a-namecpuresourcesawhen-apps-consume-more-cpu-than-expected"></a><a name="CPUresources"></a>Wenn weitere CPU Zeit als erwartet benötigt nutzen apps
Wenn Sie feststellen, dass eine app verbraucht Weitere CPU als erwartet oder auftritt CPU-Spitzen wiederholt, wie über die Überwachung angegeben oder Dienst Empfehlungen erwägen Sie das Skalieren oder Skalierung der App-Serviceplan. Die Anwendung ist Statefull, skalieren nur die Option, während ist eine Anwendung statusfreie, Skalierung Out Ihnen mehr Flexibilität und höhere Maßstab potenzieller vermitteln. 

Weitere Informationen zu "Statefull" im Vergleich mit einer "stateless" Applikationen können Sie in diesem Video wird: [Planen der Anwendung einer skalierbare End-to-End-mit mehreren Ebenen auf Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Weitere Informationen zu App skalieren und automatische Skalierung Dienstoptionen finden: [Skalieren ein Web-App in Azure-App-Dienst](web-sites-scale.md).  

## <a name="a-namesocketresourcesawhen-socket-resources-are-exhausted"></a><a name="socketresources"></a>Wenn Sockets Ressourcen erreicht werden
Ein häufiger Grund für erschöpfen ausgehenden TCP-Verbindungen ist die Verwendung von Client-Bibliotheken, die nicht TCP-Verbindungen wiederverwenden implementiert werden und im Falle einer höheren Ebene Protokoll wie HTTP - beibehalten aktiv nicht genutzt werden. Aufforderung zur Überarbeitung der Dokumentation jedes der Bibliotheken optimiert die apps in Ihrem App-Dienst um sicherzustellen, dass sie konfiguriert oder im Code für die effiziente Wiederverwendung von ausgehende Verbindungen zugegriffen werden. Führen Sie auch die Bibliothek Dokumentation Anleitung für die ordnungsgemäße Erstellung und Freigabe oder zum Aufräumen, um zu vermeiden, Verluste im Verbindungen aus. Während die Untersuchungen solche Client-Bibliotheken ausführen möglicherweise Fortschritt Einfluss durch Skalierung auf mehrere Instanzen verringert werden.  

## <a name="a-nameappbackupawhen-your-app-backup-starts-failing"></a><a name="appbackup"></a>Wenn die app gestartet wird weiß nicht sichern
Die beiden wichtigsten Gründe, warum app Sicherung fehlschlägt sind: Ungültige Speicher Einstellungen und ungültige Datenbank-Konfiguration. Diese Fehler erfolgen normalerweise Vorliegens Änderungen an Speicher oder Datenbank Ressourcen oder Änderungen für diese Ressourcen (z. B. Anmeldeinformationen für die Datenbank, in der zusätzliche Einstellungen ausgewählt aktualisiert) zugreifen. Sicherungskopien in der Regel nach einem Zeitplan ausgeführt und erfordern den Zugriff auf Speicher (zur Ausgabe von die gesicherten von Dateien) und Datenbanken (zum Kopieren, und lesen Inhalt, in die Sicherung aufgenommen werden sollen). Das Ergebnis des weiß nicht, um eine der folgenden Ressourcen zuzugreifen wäre konsistente Sicherung Fehler auf. 

Nach einem Ausfall Sicherung, Aufforderung zur Überarbeitung zuletzt Ergebnisse, um zu verstehen, welche Art des Fehlers geschieht. Wenn der fehlgeschlagene Zugriff auf Speicher überprüfen und Aktualisieren der Speicher-Einstellungen in der Sicherungskopie der Konfiguration verwendet werden. Wenn Access Datenbankfehler überprüfen Sie und aktualisieren Sie Ihre Verbindungszeichenfolgen als Teil der Einstellungen für die app; Passen Sie dann die Sicherung Konfiguration aus, um die erforderlichen Datenbanken ordnungsgemäß einschließen aktualisieren. Weitere Informationen zu app Sicherung finden Sie in der Dokumentation [einer Web app im App-Verwaltungsdienst Azure sichern](web-sites-backup.md) .

## <a name="a-namenodejsawhen-new-nodejs-apps-are-deployed-to-azure-app-service"></a><a name="nodejs"></a>Wenn neue Node.js apps Azure-App-Dienst bereitgestellt werden.
Azure App Standard Dienstkonfiguration für Node.js apps soll am besten Anforderungen am häufigsten verwendeten apps. Wenn die Konfiguration für Ihre app Node.js individuellen optimieren zum Verbessern der Leistung oder Ressource: Einsatz CPU/Arbeitsspeicher/Netzwerk-Ressourcen optimieren nutzbringend möchten, lesen Sie konnte unseren best Practices und Schritte zur Problembehandlung. Dokumentation wird beschrieben, die Einstellungen Iisnode möglicherweise für Ihre app Node.js konfigurieren müssen, werden die verschiedenen Szenarios oder Probleme, dass Ihre app gegenüberliegende werden kann, und zeigt, wie diese Probleme behoben werden: [Best Practices und zur Problembehandlung Leitfaden für Knoten Applikationen Azure-App-Dienst](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   



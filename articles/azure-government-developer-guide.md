<properties 
    pageTitle="Leitfaden für Azure Government Entwickler" 
    description="Dies stellt einen Vergleich der Features und Hinweise zur Entwicklung von Applications für Azure Government" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Microsoft Azure Government Developer Guide 

<p> Microsoft Azure Government ist eine physisch und Netzwerk isoliert Instanz von Microsoft Azure.  Vorliegende Entwickler Handbuch enthält Details über die Unterschiede dieser Anwendungsentwickler und Administratoren müssten interagieren und Arbeiten mit diese separaten Bereiche von Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>In diesem Thema


+ [(Übersicht)](#Overview)
+ [Leitfaden für Entwickler](#Guidance)
+ [In Microsoft Azure Government zurzeit verfügbare Funktionen](#Features)
+ [Endpunkt-Zuordnung](#Endpoint)
+ [Nächste Schritte](#next)


## <a name="a-nameoverviewaoverview"></a><a name="Overview"></a>(Übersicht)

Microsoft Azure Government ist eine separate Instanz des Diensts Microsoft Azure adressieren die Anforderungen Sicherheit und Kompatibilität US-Bundesbehörden, staatliche oder lokalen Behörden und deren Lösungsanbieter. Azure Government bietet physische Isolation von US-Regierung Bereitstellungen Netzwerk und überwachtes US-Personal bietet. 

Microsoft bietet eine Reihe von Tools zum Erstellen und Bereitstellen von Cloud eine Anwendung globale Microsoft Azure Dienst ("globale") und Microsoft Azure Government-Dienste von Microsoft.

Beim Erstellen und Bereitstellen von Applications den Entwicklern Azure Government Services im Gegensatz zu dem globalen Dienst, die wichtigsten Unterschiede der beiden Dienste wissen müssen.  Speziell um einrichten und Konfigurieren ihrer Umgebung programming, Konfigurieren von Endpunkten, Schreiben von Applications, und sie als Azure Government-Dienste bereitzustellen.

Die Informationen in diesem Dokument werden die Unterschiede zusammengefasst und ergänzen die Informationen, die auf der Website [Azure Government](http://www.azure.com/gov "Azure Government") und [Technische Bibliothek für Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") auf MSDN verfügbar. Offizielle Daten möglicherweise auch zur Verfügung, in vielen anderen Speicherorten wie die [Microsoft Azure-Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure-Trust Center" /), [Azure Dokumentation Center](https://azure.microsoft.com/documentation/) und in [Azure Blogs] (https://azure.microsoft.com/blog/ "Azure Blogs" /). 

Dieses Inhaltstyps richtet sich Partner und Entwickler, die auf Microsoft Azure Government bereitstellen.



## <a name="a-nameguidanceaguidance-for-developers"></a><a name="Guidance"></a>Leitfaden für Entwickler
Da die meisten technische Inhalte, die derzeit verfügbar ist angenommen wird, dass Applikationen für den globalen Dienst und nicht für Microsoft Azure Government entwickelt wurden, ist es wichtig für Sie sicherstellen, dass Entwickler wichtigsten Unterschiede für Applikationen entwickelt, um in Azure Government gehostet werden kennen.

- Zunächst Services und Funktionsunterschiede vorhanden sind, dies bedeutet, dass bestimmte Features, die in bestimmten Regionen mit dem globalen Dienst sind möglicherweise nicht verfügbar in Azure Government.

- Zweites, für die Features, die in Azure Government zur Verfügung stehen, Konfigurationsunterschiede bestehen aus dem globalen Dienst.  Daher sollten Sie überprüfen der Stichprobe Code, Konfigurationen und Schritte aus, um sicherzustellen, dass Sie erstellen und innerhalb der Umgebung Azure Government Cloud Services ausgeführt werden.


## <a name="a-namefeaturesa-features-currently-available-in-microsoft-azure-government"></a><a name="Features"></a>In Microsoft Azure Government zurzeit verfügbare Funktionen
Azure Government weist die folgenden Dienste zur Verfügung derzeit in sowohl die uns GOV IOWA uns GOV VIRGINIA Regionen:

- Virtuellen Computern
- Cloud-Dienste
- Speicher
- Active Directory
- Scheduler
- Virtuelle Netzwerke
- SQL-Datenbank
- Azure-Dateien
- Media-Dienste
- Datenverkehr-Manager
- Dienstbus
- StorSimple
- Redis Cache
- Azure Sicherung
- Automatisierung
- ExpressRoute
- usw.

Andere Dienste zur Verfügung, und weitere Dienste permanenten hinzugefügt werden sollen.  Die aktuellsten Liste der Dienste finden Sie auf der [Seite Regionen](https://azure.microsoft.com/regions/#services) die einzelnen Regionen zur Verfügung und ihre Dienste hervorgehoben wird.  

Aktuell sind uns GOV Iowa und uns GOV Virginia die Data Centers Azure Government unterstützen.  Lizenzinformationen finden Sie die Seite Regionen über aktuelle Data Center und Dienste zur Verfügung.

## <a name="a-nameendpointaendpoint-mapping"></a><a name="Endpoint"></a>Endpunkt-Zuordnung

Verwenden Sie in der folgenden Tabelle können Sie sich bei der Zuordnung von öffentlichen Microsoft Azure und SQL-Datenbank Endpunkte zu bestimmten Endpunkten Azure Government führen.


Diensttyp|Azure öffentlichen|Azure Government
---|---|---
Verwaltungsportal|Manage.windowsazure.com|Manage.windowsazure.US
Allgemeine|*. Windows|*. usgovcloudapi.net
Core|*. von Core.Windows.NET befinden.|*. core.usgovcloudapi.net
Berechnen|*. cloudapp.net|*. usgovcloudapp.net
BLOB-Speicher|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Warteschlangenspeicher|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Table Storage|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Servicemanagement|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-Datenbank|*. database.windows.net|*. database.usgovcloudapi.net
Lastenausgleich Cloud-Endpunkt|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* Cloud-Authentifizierung über Azure AD-finden Sie [Authentifizieren Azure Ressourcenmanager Besprechungsanfragen](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte

Wenn Sie weitere und zur Azure Government learning interessiert sind nutzen Sie einige der folgenden Links.

- **[Registrieren Sie sich für eine Testversion](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Beim Abrufen und den Zugriff auf Azure Government](http://azure.com/gov)**

- **[Azure Regierung (Übersicht)](/azure-government-overview)**

- **[Azure Government-Blog](http://blogs.msdn.com/b/azuregov/)**

- **[Azure Compliance](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md

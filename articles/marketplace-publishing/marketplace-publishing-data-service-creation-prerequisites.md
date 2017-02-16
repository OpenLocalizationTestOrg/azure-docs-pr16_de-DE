<properties
   pageTitle="Technische erforderlichen Komponenten zum Erstellen einer Data Service von Marketplace | Microsoft Azure"
   description="Grundlegendes zu den Anforderungen für das Erstellen einer Data Service zum Bereitstellen und verkaufen auf Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technische erforderlichen Komponenten zum Erstellen einer Data Service anbieten von Azure Marketplace

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Data Service Herausgeber. Neue Dataservices wird nicht für Auflistung genehmigt abrufen.** Wenn Sie eine SaaS Business-Anwendung haben Sie auf Elemente verwenden veröffentlichen möchten Sie weitere Informationen finden Sie [hier](https://appsource.microsoft.com/partners). Oder wenn Sie eine IaaS Applikationen Entwicklertools Dienst auf Azure Marketplace, die Sie veröffentlichen möchten weitere Informationen finden Sie [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Lesen Sie den Prozess sorgfältig vor Beginn und zu verstehen Sie, wo und warum die einzelnen Schritte durchgeführt werden. So weit wie möglich, Sie sollten Vorbereiten der Firmeninformationen und anderen Daten, erforderlichen Tools herunterladen und/oder technische Komponenten vor Beginn des Erstellungsprozess Angebot erstellen.

Sie sollten Sie Folgendes bereit, bevor Sie beginnen die haben:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Entscheidung, auf welche Technologie verwendet wird, um Ihr Angebot Data Service veröffentlichen

Ein Herausgeber kann entscheiden, zwischen mehreren Technologien beim Veröffentlichen von Data Service in Azure Marketplace. Das Hauptfenster Technologien, die unterstützt werden unter beschrieben. Unabhängig davon verbraucht welche Technologie zum Veröffentlichen der Data Service verwendet wird, der Endbenutzer die Daten durch den **OData-feed** , Azure Marketplace-Dienst bereitgestellt werden. Vollständige Informationen zu OData-Dienst finden Sie auf [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure-Datenbank

Probleme Dataset in SQL Azure bereit ist Zuständigkeit des Herausgebers. Sie müssen Azure abonnieren, Bereitstellung geeignete Größe der Datenbank und Ihrer Daten in SQL Azure DB hochladen. Publisher ist auch verantwortlich seiner/Ihrer Daten immer auf dem neuesten Stand zu bleiben. Weitere Informationen zum Abonnieren von Azure Services finden Sie auf [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Beim Verschieben von Daten in SQL Azure können die Azure Marketplace Tabellen und Sichten verfügbar machen. Herausgeber kann angeben, welche Tabellen/Ansichten und Spalten für den Endbenutzer verfügbar gemacht werden. Weitere kann auch der Inhalte Anbieter angeben, welche Spalten durch den Endbenutzer abgefragt werden können und welche nur in den Nutzdaten zurückgegeben werden. Dadurch wird eine hohe Flexibilität darüber, welche Daten in der Datenbank verfügbar gemacht werden sollen. Spalten, die abgefragt werden können, müssen Sie durch eine oder mehrere Datenbank Indizes unterstützt werden.

## <a name="rest-based-web-service"></a>REST-basierten Webdienst

Protokoll unterstützt: **nur HTTPS**

Vorhandene REST-basierten Services können über dem Azure Marketplace bereitgestellt werden. Da das Dataset immer für den Endbenutzer als einen OData-Feed verfügbar gemacht wird, basiert die Anforderungen Azure Marketplace-Dienst werden sollen, ordnen Sie den Dienst mit einem OData Dienst an. Zum Ausführen, damit der REST Endpunkte basierte muss alle Parameter als HTTP-Parameter verfügbar machen.

Die Nutzlast muss sich in einem Formular befinden, die in einer ATOM-Antwort zugeordnet werden kann. Daher enthalten die Antwort von der Dienste muss werden in XML-Format und kann nur ein wiederholtes Element, das Nutzlast Werte (wie Datensatzgruppe) enthält. Der Azure Marketplace-Dienst wird den sich wiederholenden Knoten den Eintragsknoten ATOM und die Nutzlast Werte in der Eigenschaftsknoten innerhalb der Eintragsknoten zugeordnet werden.

Autorisierungsinformationen (z. B. API Key, Authentifizierung token, usw.) muss als HTTP-Parameter oder in der Kopfzeile HTTP (Schlüsselwert Paare) – auch Standardauthentifizierung unterstützt wird. Ein gültiger Key bereitgestellt werden muss, und alle Anfragen bis Azure Marketplace werden durch die Taste gemacht wird. Benutzer für die Überwachung und Abrechnung geschieht bei den Layer Azure Marketplace.

Fehler vom Dienst zurückgegebenen in HTTP Statuscodes zugeordnet werden müssen. Für den Fall, dass der Dienst eine XML, die den Fehler enthält, die, den diese abgelegt werden vom Azure Marketplace-Dienst zur HTTP Statuscodes zugeordnet werden zurückgibt.

## <a name="soap-based-web-services"></a>SOAP-basierte Webdienste

Protokoll: **nur HTTPS**

Die Anforderungen sind die gleichen wie die übrigen Bereich Service basierend auf. Der einzige Unterschied ist, dass die Parameter auch in eine XML-Textteil bereitgestellt werden können, der mit jeder Anforderung über Azure Marketplace des Herausgebers Dienst gebucht wird. Dies bedeutet, dass HTTP-Parameter, die der Benutzer bei der Front-End-bietet in XML-Elemente eines XML-Dokuments übersetzt werden, die mit der Anforderung an den Inhalt Anbieter Webdienst gebucht wird.

## <a name="odata-based-web-services"></a>Basis OData-Webdiensten

Protokoll: **nur HTTPS**

Daten können als OData-Dienst zu Azure Marketplace verfügbar gemacht werden. Das System wird gezeigt, die den Dienst über übergeben und ersetzt den Stamm des Diensts mit dem Stamm Azure Marketplace-Dienst – um sicherzustellen, dass alle nachfolgende Anrufen über Azure Marketplace wechseln.

OData-Dienste müssen nicht nur für eine Datenbank in die Back-End-wechseln. OData unterstützt alle Arten von Speicher oder die Geschäftslogik zu den Dienst versorgen.


## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie die erforderlichen Komponenten überprüft und die erforderlichen Vorgänge abgeschlossen, können Sie bei der Erstellung Ihr Angebots Data Service wie des [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md)ausführlich nach vorne verschieben.

Oder, wenn Sie den gesamten Vorgang und den entsprechenden Artikeln für die Veröffentlichung Phasen überprüfen möchten, lesen Sie den Artikel [Erste Schritte: So veröffentlichen ein Angebots zu Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md



#<a name="metadata-for-azure-technical-articles"></a>Metadaten für Azure technische Artikel

Alle Azure technische Artikel enthalten zwei Metadaten Abschnitte - einen Eigenschaftenabschnitt und einen Abschnitt Kategorien. Im Eigenschaftenabschnitt kann einige Website Automatisierung und SEO von Inhalten, während Sie im Abschnitt Kategorien viele interne Inhalt Berichte kann. Beide Abschnitte sind erforderlich.

- [Die Syntax]
- [Verwendung]
- [Attribute und Werte für die im Eigenschaftenabschnitt]
- [Attribute und Werte für den Abschnitt Kategorien]

##<a name="syntax"></a>Syntax

Im Eigenschaftenabschnitt verwendet die folgende Syntax:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

Im Abschnitt Kategorien verwendet die folgende Syntax:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Verwendung

- Den Elementnamen und die Namen von Attributen Groß-/Kleinschreibung.
- Die <properties> Abschnitt muss die erste Zeile Ihrer Datei sein.
- Lassen Sie eine Leerzeile nach jedem Metadatenabschnitt und vor der Seitentitel, um sicherzustellen, dass der Seitentitel während des Veröffentlichungsvorgangs ordnungsgemäß in HTML konvertiert wird.

## <a name="attributes-and-values-for-the-properties-section"></a>Attribute und Werte für den Eigenschaftenabschnitt

![](./media/article-metadata/checkmark-small.png)**Seitentitel**: erforderlich; Wichtig SEO. Der Text für dieses Attribut wird auf der Browserregisterkarte auch als Titel in ein Suchergebnis. Verwenden Sie 55-60-Zeichen, einschließlich Leerzeichen und einschließlich den Websitebezeichner *| Microsoft Azure* (als eingegeben: Pipe Leerzeichen Microsoft Azure space).  Die Seitentitel sollten die H1 abweicht.

![](./media/article-metadata/checkmark-small.png)**Beschreibung**: erforderlich; wichtig für SEO (Relevanz) und Website. Die Beschreibung sollten mindestens 125 Zeichen lange auf 155 Zeichen, die maximale, einschließlich Leerzeichen. Beschreiben Sie den Zweck Teil Ihres Inhalts aus, damit Kunden wissen, ob diese aus einer Liste mit Suchergebnissen auswählen müssen. Der Wert ist:

- Dieser Text möglicherweise als Beschreibung oder abstrakte Absatz in den Suchergebnissen auf Google angezeigt.
- Dieser Text wird in [den Ergebnissen der Artikel Index](https://azure.microsoft.com/documentation/articles/)angezeigt.

![](./media/article-metadata/checkmark-small.png)**Services**: Artikel, die Umgang mit einem Dienst erforderlich. Dieser Wert wird ofter als "Servce Slug" bezeichnet. Listen Sie alle verfügbaren Dienste, die durch Kommas getrennt ein. Der erste Dienst, den Sie die Liste wird das Laufwerk der Breadcrumb Navigation für die Seite und der linken Navigationsleiste, die mit der Seite angezeigt wird.

In den Artikeln, die sowohl einen Wert Services und einen DocumentationCenter Wert angeben, wird der Wert Services der Breadcrumb versorgen. Weitere Werte, die Sie Liste als Kategorien im veröffentlichten Artikel angezeigt wird. Werte:

- Active directory
- aktiv-Directory-b2c
- aktiv-Directory-ds
- App-service\api
- Projektmanagement-API
- App-Verwaltungsdienst
- App-servic\mobile
- App-service\web
- App-service\logic
- Gateway-Anwendung
- Einsichten-Anwendung
- Automatisierung
- Azure-portal
- Azure Ressourcenmanager
- Azure-Stapel
- Sicherung
- Stapelverarbeitung
- Empfohlene
- BizTalk-Dienste
- Cache
- CDN
- Cloud-Dienste
- Daten-Katalog
- Factory-Daten
- Daten-Sees-analytics
- Lake Datenspeicher
- Devtest-Übung
- DNS-Einträge
- documentdb
- expressroute
- Ereignis-hubs
- hdinsight
- IOT-hub
- Key-Tresor
- Lastenausgleich
- Computer-Schulung
- Marketplace
- Media-Dienste
- Mobile-engagement
- Mobile-Dienste
- Multi-factor-Authentifizierung
- Benachrichtigung-hubs
- Betrieb Einsichten
- Vorgänge-Management-suite
- powerapps
- Wiederherstellung-manager
- Redis-cache
- RemoteApp
- Verwaltung von Informationsrechten
- Scheduler
- Suchen
- Sicherheitscenter
- Dienstbus
- Fabric-Dienst
- Website-Wiederherstellung
- SQL-Datenbank
- SQL-Data Warehouse-
- SQL-Berichte
- Speicher
- Speichern
- storsimple
- Stream-analytics
- Datenverkehr-manager
- virtuellen Computern
- virtuelles Netzwerk
- Visual Studio-online
- VPN-gateway
- Websites

![](./media/article-metadata/checkmark-small.png)**DocumentationCenter**: optimale Entwickler reduzierte Artikel über eine Developer Center bereitgestellte erforderlich. Geben Sie an einzelnen Developer Center Sprache, die im Artikel gilt. Der Wert, den Sie Liste wird der Breadcrumb Navigation für die Seite zu führen. In den Artikeln, die sowohl einen Wert Services und einen DocumentationCenter Wert angeben, wird der Wert Services der Breadcrumb versorgen. Werte:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **Ruby**
- **Mobile**: nicht mehr unterstützt. Mit bestimmten mobilen Plattform ersetzen.
- **Ios**: Verifing dieser neuen Wert
- **Android**: überprüfen den neuen Wert
- **Windows**: überprüfen den neuen Wert
- **Xamarin**: überprüfen den neuen Wert

![](./media/article-metadata/checkmark-small.png)**Autoren**: erforderlich, nur einen Wert. Liste des GitHub-Kontos für den primären Autor oder Artikel SME an. Dieses Attribut Laufwerke der lauten nach Zeile im veröffentlichten Artikel. Listen Sie nur eine trotz der Pluralform Namen für das Attribut aus.

![](./media/article-metadata/checkmark-small.png)**Manager**: erforderlich, wenn Sie ein Microsoft-Mitwirkender sind. Liste des e-Mail-Alias des Content publishing-Managers für den Technologiebereich. Wenn Sie einen Mitwirkenden Community sind, nehmen Sie das Attribut jedoch bleiben Sie leer, damit wir es ausfüllen können.

![](./media/article-metadata/checkmark-small.png)**Editor**: nicht verwendet. Verwenden sie nicht für andere Zwecke.

![](./media/article-metadata/checkmark-small.png)**Kategorien**: Optional. Gehören Sie nur, wenn Sie eine Verknüpfung unterhalb der Breadcrumb Artikel, um die Artikelseite Index (http://azure.microsoft.com/documentation/articles/) aktivieren möchten, auf eine prefiltered Liste von Artikeln, die einer der genehmigten Werte entsprechen. Bieten eine Möglichkeit, um Inhalte zu gruppieren, wenn der Inhalt gruppieren nicht spezielle-Dienst werden diese Werte auffällt. Diese Tags können auch Beschriftungen, die den Stapel Technologie gibt an, die, dem der Artikel bezieht sich auf, bereitstellen. Dieser Wert **wird nicht** unterstützt, Freiform-Tags oder Hashtags; Klicken Sie auf der Website müssen die Kategorien aktiviert sein. Sie können mehrere Kategorien Werte durch Kommas getrennt ein Artikel angeben. Die genehmigten Werte sind:

  - Architektur
  - Azure Ressourcenmanager
  - Azure Servicemanagement
  - Abrechnung
  - MySQL

![](./media/article-metadata/checkmark-small.png)**Schlüsselwörter**: Optional. Für die Verwendung durch nur SEO-Champs. Separate Begriffe durch Kommas. **Wenden Sie Ihre SEO Champ vor dem ändern oder löschen Sie den Inhalt in diesem Artikel, die diese Ausdrücke enthält.** Dieses Attribut Einträge Schlüsselwörter, die Seo innerhalb wurde und ist nachverfolgen, um die Suche Rang zu verbessern. Die Schlüsselwörter rendern nicht im veröffentlichten HTML. Überprüfung ist dieses Attribut nicht erforderlich.

## <a name="attributes-and-values-for-the-tags-section"></a>Attribute und Werte für den Abschnitt Kategorien

![](./media/article-metadata/checkmark-small.png)**ms.Service**: erforderlich. Gibt die Azure Service, Tool oder Features, die im Artikel gilt. Ein Wert pro Seite.

 Wenn auf eine Seite auf mehrere Dienste angewendet wird, wählen Sie den Dienst, die am häufigsten direkt zugehöriges; ein Artikel aus, der eine app gehostet Websites Dienstbus Funktionen vorzuführen verwendet sollte, der **Dienstbus** Wert, statt **Websites**verfügen. Wenn eine Seite gleichmäßig auf mehrere Dienste angewendet wird, wählen Sie **mehrere**aus. Wenn eine Seite nicht für alle Dienste gilt (seltene ist das), wählen Sie **NV**aus.

 - **Active directory**
 - **aktiv-Directory-b2c**
 - **aktiv-Directory-ds**
 - **Projektmanagement-API**
 - **App-Verwaltungsdienst**: gilt nur für allgemeine interessanten App-Diensts
 - **App-Service-api**
 - **App-Service-Logik**
 - **App-Service-mobile**
 - **App-Service-web**
 - **Einsichten-Anwendung**
 - **Gateway-Anwendung**
 - **Automatisierung**
 - **Azure Ressourcenmanager**
 - **Azure-Sicherheit**
 - **Azure-Stapel**
 - **Sicherung**
 - **Stapelverarbeitung**
 - **Empfohlene**
 - **BizTalk-Dienste**
 - **Abrechnung**
 - **Cache**
 - **CDN**
 - **Cloud-Dienste**
 - **Daten-Katalog**
 - **Lake Datenspeicher**
 - **Daten-Sees-analytics**
 - **Devtest-Übung**
 - **expressroute**
 - **hdinsight**
 - **Internet-von-Elementen**
 - **IOT-hub**
 - **Key-Tresor**
 - **Computer-Schulung**
 - **Marketplace**: Artikel zu Azure Marketplace
 - **Media-Dienste**
 - **Mobile-engagement**
 - **Mobile-Dienste**
 - **Multi-factor-Authentifizierung**
 - **mehrere**: die Seite gilt für mehrere Dienste gleichmäßig
 - **NV**: die Seite gilt nicht für alle Dienste (seltene)
 - **Benachrichtigung-hubs**
 - **Betrieb Einsichten**
 - **powerapps**
 - **Wiederherstellung-manager**
 - **Redis-cache**
 - **RemoteApp**
 - **Verwaltung von Informationsrechten**
 - **Scheduler**
 - **Sicherheitscenter**
 - **Dienstbus**
 - **Fabric-Dienst**
 - **Website-Wiederherstellung**: früher Wiederherstellung Services
 - **SQL-Datenbank**
 - **SQL-Data Warehouse-**
 - **SQL-Berichte**
 - **Speicher**
 - **Speichern**: Artikel zu verfügbar bis den Azure-Speicher
 - **storsimple**
 - **Datenverkehr-manager**
 - **virtuellen Computern**
 - **virtuelles Netzwerk**
 - **Visual Studio-online**
 - **VPN-gateway**
 - **Websites**

![](./media/article-metadata/checkmark-small.png)**ms.DevLang**: erforderlich. Gibt die Programmiersprache, der im Artikel gilt. Einzelner Wert pro Seite.

 Wenn auf zwei programming Sprachen gleichmäßig eine Seite angewendet wird, wählen Sie **mehrere**aus. Wenn eine Seite hauptsächlich konzeptionelle und seinen Inhalt im allgemeinen Programmierung mehrsprachigen anwendbar ist, wählen Sie **mehrere**aus. Wenn eine Seite nicht sich an Entwickler richtet und die Programmierung Sprachversionen nicht relevant ist, wählen Sie **NV**aus. Verwenden Sie **Rest-api** REST-API Bezug Themen beziehen.

 - **cpp**
 - **dotnet**
 - **Java**
 - **JavaScript**
 - **mehreren**: Seite programming mehrsprachigen gleichmäßig gilt.
 - **NV**: die Seite ist nicht Zielgruppe Entwickler und ist nicht spezifisch für alle Sprachen Programmierung.
 - **nodejs**
 - **Ziel-c**
 - **PHP**
 - **Python**
 - **Rest-api**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**ms.Topic**: erforderlich. Geben Sie Besonderheiten des Themas. Die meisten neue Seiten Mitwirkenden erstellte werden Artikel oder Verweis.

 - **Artikel**: Thema, Lernprogramm, der Leistungsmerkmale oder anderen nicht-Referenz-Artikel

 - **für eine Marketingkampagne-Seite**: nur Azure.com.  Eine Seite, die speziell als eine Startseite für externe massensendungen zu ermitteln, und ist nicht Bestandteil des primären Standorts i a  Sollte für Dokumentation Artikeln oder reguläre Zielseiten Dokument nicht verwendet werden.  Beispiele: azure.microsoft.com/develop/net/aspnet/; Azure.Microsoft.com/Develop/Mobile/IOS/

 - **Entwickler-Center-Homepage**: nur Azure.com.  Ein Entwickler Homepage, z. B. zentrieren/entwickeln-Netz /

 - **Erste Schritte Artikel**: Zuweisen zu Artikeln, die im Abschnitt Erste Schritte oder Übersicht im linken Navigationsbereich für einen Dienst bereitgestellte sind.

 - **Hero-Artikel**: ein "Hero" Lernprogramm, das soll eine Einführung in einem Dienst bereitstellen oder bereitstellen, die Besucher schnell mithilfe des Diensts gestartet wird und Laufwerke kostenlosen Testversion Sportteam und MSDN-Vorgänge. Weisen Sie diesen Wert nur zu Artikeln, die am oberen Rand die Startseite Dokumentation des Diensts bereitgestellte sind.

 - **Homepage**: Homepage der obersten Ebene Dokumentation. Wir haben nur zwei: azure.microsoft.com/documentation/ und msdn.microsoft.com/library/azure/

 - **Index-Seite**: zweiter Ebene Zielseiten für die Programmierung von Sprachen, Dienste und Features. Diese, die auf Azure.com und der Bibliothek verteilt werden und werden als Felder und Optionen für spezifischere, Suchbegriffs Informationen verwendet. Beispiele: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **Infographic-Seite**: nur Azure.com. Eine Seite, die eine durchsuchbar Infographic oder Poster, z. B. http://azure.microsoft.com/documentation/infographics/windows-azure/ features

 - **Referenz**: ein API Bezug Seite (einschließlich REST-API) oder PowerShell-Cmdlet Bezug

 - **Service-Homepage**: nur Azure.com.  Eine Dokument Service-Homepage, z. B. /documentation/services/virtual-machines /

 - **Website-Abschnitt-Homepage**: nur Azure.com. Eine "Homepage" für einen bestimmten Typ von Inhalt auf azure.com Beispiele: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **Video-Seite**: nur Azure.com.  Eine Seite, die ein Video, z. B. http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/ features

![](./media/article-metadata/checkmark-small.png)**ms.tgt_pltfrm**: erforderlich. Gibt die Zielplattform, z. B. Windows, Linux, Windows Phone, iOS, Android oder besondere Cache Plattformen. Ein Wert pro Seite. Dieser Wert wird **NV** für die meisten Themen außer Mobile und virtuellen Computern sein.

 - **Cache-in-Rolle**
 - **Cache-Vielfache**
 - **Cache-redis**
 - **Cache-Dienst**
 - **Cache freigegeben**
 - **Befehlszeilenversion Befehlszeile-Benutzeroberfläche**
 - **Ibiza**: Inhalte, die im Portal Ibiza verwenden. Verwenden Sie diese Option nur in Fällen, wo das Feature erwähnt wird über sowohl im Portal Ibiza und der aktuellen Portal verfügbar ist.
 - **Mobile-Android**: nur sofort Azure.com
 - **Mobile-HTML-**: nur sofort Azure.com
 - **Mobile Ios**: nur sofort Azure.com
 - **Mobile-Kindle**: nur sofort Azure.com
 - **Mobile-Vielfache**
 - **Mobile-Nokia-X**: nur sofort Azure.com
 - **Mobile-Phonegap**: nur sofort Azure.com
 - **Mobile-Sencha**: nur sofort Azure.com
 - **Windows Mobile**: nur sofort; Azure.com Windows-Dienst
 - **Windows-Mobiltelefon**
 - **Mobile-Windows-store**
 - **Mobile-Xamarin**: Azure.com nur sofort; Xamarin alle Plattformen
 - **Mobile-Xamarin-Android**: nur sofort Azure.com
 - **Mobile-Xamarin-Ios**: nur sofort Azure.com
 - **mehrere**: die Seite bezieht sich auf mehrere Plattformen gleichmäßig
 - **NV**: ein Spaltenbezeichner Plattform gilt nicht für diese Seite
 - **PowerShell**
 - **virtueller Computer-linux**
 - **virtueller Computer-Vielfache**
 - **Windows-virtueller Computer**
 - **Sharepoint-Windows-virtueller Computer**
 - **virtueller Computer-Windows-Sql-server**
 - **im Vergleich mit einer--überfordert**: die überfordert im Vergleich mit einer Seitengruppe bezeichnet. Tag 1/12/14 hinzugefügt.
 - **im Vergleich mit einer-was-Vorkommnissen**: bezeichnet die Seite im Vergleich mit einer Erste Schritte den Vorkommnissen. Tag 1/12/14 hinzugefügt.

![](./media/article-metadata/checkmark-small.png)**ms.Workload**: erforderlich. Gibt die Azure Arbeitsbelastung, der auf die Seite angewendet wird. Ein Wert nur pro Artikel.

**Aktualisieren von 8/6/15** Der Wert ms.workload wird durch eine Xls, nicht den Wert in der Datei .md zugeordnet. Der Wert ms.workload ist für Überprüfung noch erforderlich, bis das Feature aktualisiert werden kann. Die Arbeit nun geplant wurde.  Verwenden Sie **"NV"** jetzt als Wert.

![](./media/article-metadata/checkmark-small.png)**ms.Date**: erforderlich. Gibt das Datum, die, das im Artikel letzte überprüft wurde, für Relevanz, Genauigkeit, richtige Screenshots und Links arbeiten. Geben Sie das Datum im Format mm/tt/jjjj ein. Dieses Datum wird auch als Datum der letzten Aktualisierung im veröffentlichten Artikel angezeigt.

![](./media/article-metadata/checkmark-small.png)**ms.Author**: erforderlich. Gibt an, die im Thema zugeordnet wer. Interne Berichte (z. B. Aktualität) verwenden Sie diesen Wert in die richtigen wer dem Artikel zugeordnet werden soll. Um mehrere Werte angeben sollten Sie diese durch Semikolons trennen. Entweder Microsoft Aliases oder vollständige e-Mail-Adressen sind zulässig. Die Länge kann nicht mehr als 200 Zeichen enthalten.


###<a name="contributors-guide-links"></a>Mitwirkenden Leitfaden für Links

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)


<!--Anchors-->
[Die Syntax]: #syntax
[Verwendung]: #usage
[Attribute und Werte für die im Eigenschaftenabschnitt]: #attributes-and-values-for-the-properties-section
[Attribute und Werte für den Abschnitt Kategorien]: #attributes-and-values-for-the-tags-section

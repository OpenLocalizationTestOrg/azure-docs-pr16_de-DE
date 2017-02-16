<properties
    pageTitle="So Daten Profildatenquellen"
    description="Gewusst wie-Artikel hervorheben wie Tabelle und Spalte Ebene Daten Profile beim Registrieren von Datenquellen in Azure Datenkatalog einbezogen werden sollen, und wie Sie Daten Profile zu verwenden, um Datenquellen zu verstehen."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Daten von Datenquellen-Profil

## <a name="introduction"></a>Einführung

**Microsoft Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient. Kurzum, geht **Azure Datenkatalog** beiträgt ermitteln, verstehen und Verwenden von Datenquellen und Hilfe Organisationen, um größeren Nutzen aus ihrer vorhandenen Daten zu ziehen. Wenn Sie eine Datenquelle mit **Azure Datenkatalog**registriert ist, seine Metadaten kopiert und vom Dienst indiziert ist, aber die Geschichte wird nicht es beenden.

Die Funktion **Daten Profil erstellen** **Azure Datenkatalog** untersucht die Daten in Ihrem Katalog unterstützten Datenquellen und sammelt Statistiken und Informationen zu diesen Daten. Es ist einfach ein Profil Ihrer Datenbestände aufnehmen möchten. Wenn Sie eine Anlage Daten registrieren, wählen Sie in der Quelle-Registration-Tool **Datenprofil einschließen** .

## <a name="what-is-data-profiling"></a>Was ist ein Profil Daten erstellen

Ein Profil Daten erstellen untersucht die Daten in der Datenquelle, die gerade registriert und sammelt Statistiken und Informationen zu diesen Daten. Während die Quelle Datenermittlung können diese Statistiken ob geeignet deren geschäftliches Problem zu lösen Daten helfen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Die folgenden Datenquellen unterstützen Daten Profil zu erstellen:

- SQL Server (einschließlich Azure SQL-Datenbank und Azure SQL-Data Warehouse) Tabellen und Ansichten
- Oracle-Tabellen und Ansichten
- Teradata-Tabellen und Ansichten
- Struktur von Tabellen

Einschließlich Daten Profile beim Registrieren Datenbestände Benutzern hilft beantworten Fragen zu Datenquellen, einschließlich:

-   Können sie werden verwendet, mein geschäftliches Problem zu lösen?
-   Entspricht die Daten bestimmten Standards oder Mustern?
-   Was sind einige der Abweichung der Datenquelle?
-   Was sind die möglichen Probleme diese Daten in meiner Anwendung zu integrieren?

> [AZURE.NOTE] Sie können auch Dokumentation hinzufügen, um eine Anlage zu beschreiben, wie Daten in eine Anwendung integriert werden konnte. Informationen Sie [zum Dokument Datenquellen](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Wie ein Datenprofil aufnehmen möchten, bei der Registrierung einer Datenquelle

Es ist einfach ein Profil der Datenquelle aufnehmen möchten. Wenn Sie eine Datenquelle im Bereich **Objekte registriert werden** von Tools für die Datenquelle Registrierung registrieren, wählen Sie die **Datenprofil einschließen**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Weitere Informationen zum Registrieren von Datenquellen finden Sie unter [Datenquellen registrieren](data-catalog-how-to-register.md) und [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtern auf Datenbestände, die Daten Profile enthalten
Um Datenbestände ermitteln, die ein Datenprofil enthalten, können Sie einbeziehen `has:tableDataProfiles` oder `has:columnsDataProfiles` als eine der Suchbegriffe.

> [AZURE.NOTE] Auswählen von **Datenprofil einschließen** in der Quelle-Registration-Tool enthält Tabelle und Spalte Ebene Profilinformationen. Der Katalog-API von Daten können jedoch Daten auszuwählen, die Sie mit nur einem Satz von Profilinformationen enthalten registriert sein.

## <a name="viewing-data-profile-information"></a>Profilinformationen Daten anzeigen

Nachdem Sie eine geeignete Datenquelle mit einem Profil gefunden haben, können Sie die Daten Profildetails anzeigen. Klicken Sie zum Anzeigen der Daten Profils wählen Sie eine Anlage Daten aus, und wählen Sie **Datenprofil** im Portal Datenkatalog-Fenster.

![](media\data-catalog-data-profile\data-catalog-view.png)

Ein Datenprofil in **Azure Datenkatalog** zeigt Tabelle und Spalte Profil Informationen, darunter:

### <a name="object-data-profile"></a>Objekt Datenprofil

-   Anzahl von Zeilen
-   Tabellengröße
-   Wenn das Objekt zuletzt aktualisiert wurde

### <a name="column-data-profile"></a>Spalte Datenprofil

- Spaltendatentyp
- Anzahl eindeutiger Werte
- Anzahl von Zeilen mit NULL-Werten
- Minimum, Maximum, Mittelwert und Standardabweichung für Spaltenwerte

## <a name="summary"></a>Zusammenfassung
Ein Profil erstellen Daten enthält Statistiken und Informationen zur registrierten Datenbestände zu festzustellen, ob der Business Probleme zu lösen Daten geeignet. Zusammen mit Stapels und Datenquellen dokumentieren, können Daten Profile Benutzern ein besseres Verständnis für Ihre Daten gewähren.


## <a name="see-also"></a>Siehe auch
-   [Zum Registrieren von Datenquellen](data-catalog-how-to-register.md)
-   [Erste Schritte mit Azure-Datenkatalog](data-catalog-get-started.md)

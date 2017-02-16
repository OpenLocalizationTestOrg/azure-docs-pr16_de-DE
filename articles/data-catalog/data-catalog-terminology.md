<properties
   pageTitle="Azure Datenkatalog Terminologie | Microsoft Azure"
   description="Dieser Artikel enthält eine Einführung in grundlegende Konzepte und Begriffe in Azure Datenkatalog Dokumentation verwendet werden."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure Datenkatalog Terminologie

## <a name="catalog"></a>Katalog

Die Datenkatalog Azure ist kein cloudbasierten Metadatenrepository welche Daten Quellen und Daten Posten registriert werden können. Der Katalog fungiert als zentralen Speicherort für das strukturelle Metadaten aus Datenquellen extrahiert und beschreibende Metadaten von Benutzern hinzugefügt wurden.

## <a name="data-source"></a>Datenquelle

Eine Datenquelle ist ein System oder Container, der Datenbestände verwaltet werden. Beispiele für SQL Server-Datenbanken, Oracle-Datenbanken, SQL Server Analysis Services-Datenbanken (tabellarischen oder mehrdimensionalen) und SQL Server Reporting Services-Servers.

## <a name="data-asset"></a>Daten-Objekt

Datenbestände befinden sich Objekte innerhalb von Datenquellen, die mit dem Katalog registriert werden können. Beispiele für SQL Server-Tabellen und Ansichten, Oracle-Tabellen und Ansichten, SQL Server Analysis Services Measures, Dimensionen und KPIs und SQL Server Reporting Services-Berichte.

## <a name="data-asset-location"></a>Speicherort der Daten-Objekt

Der Katalog speichert die Position eines Datenquelle oder Daten Anlage, die Verbindung zu der Quelle mit einer Clientanwendung verwendet werden können. Das Format und die Details des Speicherorts variieren basierend auf den Datenquellentyp. Beispielsweise kann eine SQL Server-Tabelle identifiziert werden deren vier Webparts Namen – Servername, Datenbankname, Name des Schemas, Objektname – während einer SQL Server Reporting Services-Bericht über den URL identifiziert werden kann.

## <a name="structural-metadata"></a>Strukturelle Metadaten

Strukturelle Metadaten sind die Metadaten extrahiert aus einer Datenquelle, die die Struktur eines Anlageguts Daten beschreibt. Dies umfasst die Position Posten, deren Objektname und Typ und zusätzliche Typ-spezifische Merkmale. Beispielsweise enthält die strukturelle Metadaten für Tabellen und Ansichten, die Namen und Datentypen für Spalten des Objekts.

## <a name="descriptive-metadata"></a>Beschreibende Metadaten

Beschreibenden Metadaten sind Metadaten, die den Zweck oder die Absicht eines Anlageguts Daten beschreibt. In der Regel beschreibende Metadaten von Katalog-Benutzern mit dem Datenkatalog Azure-Portal hinzugefügt wird, aber sie können auch aus der Datenquelle extrahiert werden, während der Registrierung. Beispielsweise wird Tools für die Registrierung Azure Datenkatalog Beschreibungen aus der Beschreibung-Eigenschaft in SQL Server Analysis Services und SQL Server Reporting Services und aus der [Ms_description erweiterte Eigenschaft](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-Datenbanken extrahieren, wenn diese Eigenschaften mit Werten aufgefüllt wurden.

## <a name="request-access"></a>Anfordern des Zugriffs

Eine Daten-Anlage beschreibende Metadaten kann Informationen zum Zugriff auf das Objekt mit Daten oder Datenquelle anfordern einbeziehen. Diese Informationen umfassen eine oder mehrere der folgenden Optionen und den Speicherort der Anlage wird angezeigt.

- Die e-Mail-Adresse des Benutzers oder Teams verantwortlich gewähren des Zugriffs auf die Datenquelle.
- Die URL des dokumentierten Prozesses, die Benutzern Zugriff auf die Datenquelle beachten müssen.
- Die URL für eine Identität und Access-Verwaltungstool (wie etwa Microsoft Identität Manager), die den Zugriff auf die Datenquelle verwendet werden kann.
- Ein-Text-Eintrag, der beschreibt, wie Benutzer auf die Datenquelle zugreifen können.

## <a name="preview"></a>Vorschau

Eine Vorschau Datenkatalog Azure ist eine Momentaufnahme der bis zu 20 Datensätze, die extrahiert aus einer Datenquelle während der Registrierung und im Katalog mit den Daten Anlage Metadaten gespeichert werden können. Die Vorschau helfen Benutzern, die eine Anlage Daten zu entdecken dessen Funktion und Zweck besser zu verstehen. Beispieldaten angezeigt kann also wertvoller als nur die Spaltennamen und Datentypen angezeigt werden.
Vorschau werden nur für Tabellen und Sichten unterstützt und müssen explizit ausgewählt werden vom Benutzer während der Registrierung.

## <a name="data-profile"></a>Datenprofil

Ein Datenprofil in Azure Datenkatalog ist eine Momentaufnahme der Tabelle- und Spalte Ebene Metadaten zu einer erfassten Daten Anlage, die extrahiert aus einer Datenquelle während der Registrierung und im Katalog mit den Daten Anlage Metadaten gespeichert werden kann. Das Datenprofil helfen Benutzern, die eine Anlage Daten zu entdecken dessen Funktion und Zweck besser zu verstehen. Ähnlich wie Vorschauen, müssen Daten Profile explizit vom Benutzer während der Registrierung ausgewählt werden.

> [AZURE.NOTE] Ein Datenprofil extrahieren kann ein teurer Vorgang für große Tabellen und Sichten sein, und die erforderliche Zeit zum Registrieren einer Datenquelle möglicherweise beträchtlich erhöhen.

## <a name="user-perspective"></a>Benutzerperspektive

In Azure Datenkatalog kann jeder Benutzer beschreibende Metadaten für eine Anlage erfassten Daten bieten. Jeder Benutzer verfügt über eine distinct Perspektive auf die Daten und deren Verwendung. Beispielsweise können Sie der für einen Server zuständige Administrator die Details der Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL) oder zusätzliche Windows abgeben; eine Data Steward vorsehen, dass Links zu Dokumentation für Unternehmen unterstützt das Daten verarbeitet; und Analysten möglicherweise stellen Sie eine Beschreibung in den Konditionen, die die für andere Analysten relevant sind, und was kann Spieler für diesen Benutzer müssen ermitteln und die Daten zu verstehen.

Jeder der folgenden Perspektiven sind grundsätzlich hilfreich sein, und mit Azure Datenkatalog kann jeder Benutzer geben Sie die Informationen, die daran sinnvoll ist, während alle Benutzer diese Informationen verwenden können, um die Daten und seine Verwendung zu verstehen.

## <a name="expert"></a>Experten

Rat ist ein Benutzer, als hätten Sie eine laufenden "Experte" Perspektive für eine Anlage Daten identifiziert wurde. Alle Benutzer kann sich selbst oder ein anderer Benutzer als Experte für eine Anlage hinzufügen. Als Experte aufgeführt wird, hat dies nicht irgendeine besondere zusätzlichen Berechtigungen in Azure Datenkatalog; Sie können die Benutzer diese Perspektiven leicht zu finden, die am ehesten hilfreich sein, wenn Sie eine Anlage des beschreibende Metadaten zu überprüfen.

## <a name="owner"></a>Besitzer

Es ist ein Besitzer ein Benutzer mit zusätzlichen Berechtigungen für die Verwaltung von einer Anlage Daten in Azure Datenkatalog. Benutzer können Besitzrechte registrierten Datenbestände und Besitzer können anderen Benutzern als gemeinsame Besitzer hinzufügen. Weitere Informationen finden Sie unter [Verwalten von Datenbestände](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Besitz und Verwaltung stehen nur in der Standard Edition von Azure Datenkatalog.

## <a name="registration"></a>Registrierung

Die Registrierung ist die Act extrahieren Daten Anlage Metadaten aus einer Datenquelle und kopieren es mit dem Datenkatalog Azure-Dienst an. Klicken Sie dann können Datenbestände, die registriert wurden kommentiert und ermittelt werden.

## <a name="see-also"></a>Siehe auch

- [Was ist Datenkatalog Azure?](data-catalog-what-is-data-catalog.md) – In diesem Artikel bietet einen Überblick über den Datenkatalog Azure-Dienst, der Wert, den sie bietet und die Szenarien, die es unterstützt.

- [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md) – in diesem Artikel enthält eine End-to-End-Lernprogramm, die Sie wird gezeigt, wie Azure Datenkatalog für Datenermittlung-Quelle verwendet werden soll.  

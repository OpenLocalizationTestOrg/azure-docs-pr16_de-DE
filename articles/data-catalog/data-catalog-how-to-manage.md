<properties
   pageTitle="So verwalten Sie Datenbestände | Microsoft Azure"
   description="Gewusst wie-Artikel zum Steuern der Sichtbarkeit und Besitz Datenbestände Hervorhebung registriert in Azure Datenkatalog."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>So verwalten Sie Datenbestände

## <a name="introduction"></a>Einführung

**Azure Datenkatalog** bietet Funktionen für die Quelle Datenermittlung, zulassen, dass Benutzer leicht ermitteln und verstehen die benötigten Datenanalysen und treffen von Entscheidungen auf Datenquellen. Diese Discovery-Funktionen stellen die größte Wirkung, wenn alle Benutzer suchen können und breiten Spektrums der verfügbaren Datenquellen verstehen. Mit diesem Hintergrund das Standardverhalten der Datenkatalog ist für alle registrierten Datenquellen für – sichtbar und durch sichtbar sein – alle Benutzer Katalog.

Datenkatalog wird nicht Benutzern Zugriff auf die Daten selbst ermöglicht. Zugreifen auf Daten wird durch den Besitzer der Datenquelle gesteuert. Datenkatalog ermöglicht Benutzern zum Ermitteln von Datenquellen und die Metadaten, die im Zusammenhang mit der Quellen registriert im Katalog anzuzeigen.

Es gibt möglicherweise Situationen, in dem Datenquellen nur bestimmten Benutzern oder Gruppen bestimmte Mitglieder sichtbar sein soll. Für diese Szenarios Datenkatalog ermöglicht Benutzern registrierten Datenbestände innerhalb des Katalogs des Besitzes und dann Steuerelement die Sichtbarkeit der Vermögenswerte sie besitzen.

> [AZURE.NOTE] Die in diesem Artikel beschriebene Funktionen stehen nur in der Standard Edition von Azure Datenkatalog. Die kostenlose Edition bietet keine Funktionen für Besitz und Einschränken der Daten Anlage Sichtbarkeit.

## <a name="managing-ownership-of-data-assets"></a>Verwalten von Besitz Datenbestände
Standardmäßig werden in Datenkatalog registriert Datenbestände ohne Besitzer; Jeder Benutzer mit der Berechtigung zum Zugreifen auf des Katalogs kann-Besprechung diese Ressourcen und ermitteln. Benutzer können ohne Besitzer Datenbestände Besitzrechte und können die Sichtbarkeit der Anlagen, die sie besitzen beschränken.

Wenn ein Objekt Daten in Datenkatalog gehört, nur Benutzer, die berechtigt, indem Sie der Besitzer die Anlage ermitteln und seine Metadaten anzeigen können, und nur die Besitzer können die Anlage aus dem Katalog löschen.

> [AZURE.NOTE] Besitz in Datenkatalog wirkt sich nur auf die Metadaten, die im Katalog gespeichert. Sie werden keine Berechtigungen für die zugrunde liegende Datenquelle erteilt wird.

### <a name="taking-ownership"></a>Übernehmen der Besitzrechte
Benutzer können Datenbestände Besitzrechte, indem Sie die Option "Besitzrechte" im Portal Datenkatalog auswählen. Keine besonderen Berechtigungen sind erforderlich, eines Wirtschaftsguts Daten ohne Besitzer des Besitzes; Jeder Benutzer kann Besitzrechte einer Anlage ohne Besitzer Daten.

### <a name="adding-owners-and-co-owners"></a>Hinzufügen von Besitzer und gemeinsame
Wenn bereits ein Objekt Daten gehört, können einfach Benutzer den Besitz – sie müssen als gemeinsame Besitzer eines vorhandenen Besitzers hinzugefügt werden. Alle Besitzer kann zusätzliche Benutzer oder Sicherheitsgruppen als gemeinsame Besitzer hinzufügen.

> [AZURE.NOTE] Es ist eine bewährte Methode, die mindestens zwei Personen als Besitzer für alle Daten im Besitz Objekt verfügen.

### <a name="removing-owners"></a>Entfernen von Besitzer
Ebenso wie eine Anlage Besitzer gemeinsame Besitzer hinzufügen kann, kann alle verantwortlichen eine gemeinsame Besitzer entfernen.

Wenn ein Objekt Besitzer sich selbst als Besitzer entfernt, kann er die Anlage nicht mehr verwalten. Wenn ein Objekt Besitzer sich selbst als Besitzer entfernt, und es keine anderen gemeinsame Besitzer gibt, wird die Anlage in einer ohne Besitzer Zustand zurückgesetzt.

## <a name="visibility"></a>Sichtbarkeit
Daten Anlage Besitzer können die Sichtbarkeit der Datenbestände steuern, die sie besitzen. Zum Einschränken der Sichtbarkeit von Standard – kann, können alle Datenkatalog Benutzer ermitteln und anzeigen die Anlage Daten – Verantwortlichen für die Ressource der für die Sichtbarkeit von "Jeder" auf "Besitzer und diese Benutzer" in den Eigenschaften für die Anlage umschalten. Besitzer können Sie bestimmte Benutzer und Sicherheitsgruppen hinzufügen.

> [AZURE.NOTE] Wann immer möglich, sollte der Anlage Besitz und Sichtbarkeit Berechtigungen für Sicherheitsgruppen und nicht für einzelne Benutzer zugewiesen werden.

## <a name="catalog-administrators"></a>Katalogadministratoren
Datenkatalog Administratoren sind implizit gemeinsame Besitzer für alle Anlagen im Katalog aus. Anlage Besitzer nicht Sichtbarkeit von Administratoren Katalog entfernen, und Administratoren können Besitz und Sichtbarkeit für alle Datenbestände im Katalog verwalten.

## <a name="summary"></a>Zusammenfassung
Des Datenkatalog Crowdsourcing Modell auf Metadaten und Daten Anlage Discovery kann alle Katalog Benutzer eigene Notizen hinzufügen und anzeigen. Die Standard Edition von Datenkatalog bietet Funktionen für Besitz und Verwaltung der Sichtbarkeit und die Verwendung von bestimmter Datenbestände einschränken.

<properties
    pageTitle="Upgrade von DirSync und Azure AD synchronisieren | Microsoft Azure"
    description="Beschreibt, wie Aktualisieren von DirSync und Azure-Active Directory-Synchronisierung auf Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Aktualisieren von Windows Azure-Active Directory-Synchronisierung ("DirSync") und Azure-Active Directory-Synchronisierung ("Azure AD synchronisieren")
Azure AD verbinden ist die beste Methode zum Verbinden von Ihrem lokalen Verzeichnis mit Azure AD und Office 365. Dies ist eine großartige Zeit, auf Azure AD-Verbinden von Windows Azure Active Directory-Synchronisierung (DirSync) oder Azure AD synchronisieren zu aktualisieren, wie diese Tools jetzt veraltet sind und Ende des Supports auf 13 April 2017 gelangen.

Die zwei Identität Synchronisierung Tools, die nicht mehr unterstützt werden, wurden für einzelne Gesamtstruktur Kunden (DirSync) angeboten und für mit mehreren Gesamtstrukturen und andere erweiterte Kunden (Azure-Active Directory-Synchronisierung). Diese ältere Tools ersetzt wurden mit einer einzigen Lösung, die für alle Szenarien zur Verfügung: Azure AD verbinden. Neue Funktionalität, verbesserten Features und Unterstützung für neue Szenarien vergleichbar. Weiterhin Ihre Identität lokaler Daten Azure AD-synchronisieren können und Office 365, es wird dringend empfohlen, auf Azure AD verbinden zu aktualisieren.

Die letzte Version von DirSync in Juli 2014 freigegeben wurde, und die letzte Version der Azure-Active Directory-Synchronisierung Mai 2015 veröffentlicht wurde.

## <a name="what-is-azure-ad-connect"></a>Was ist Azure AD-Verbindung herstellen
Verbinden von Azure AD ist der Nachfolger DirSync und Azure AD synchronisieren. Alle Szenarien kombiniert diese beiden unterstützt. Sie können weitere Informationen zu erhalten, bei der [Integration Ihrer lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Veraltete Objekte Terminplan

Datum | Kommentar
 --- | ---
13 April 2016 | Windows Azure Active Directory-Synchronisierung ("DirSync") und Microsoft Azure Active Directory-Synchronisierung ("Azure AD synchronisieren") werden als veraltet angekündigt.
13 April 2017 | Unterstützung endet. Kunden werden nicht mehr eine Supportanfrage ohne Upgrade auf Azure AD verbinden zuerst öffnen.

## <a name="how-to-transition-to-azure-ad-connect"></a>So Übergang zu Azure AD-verbinden
Wenn Sie DirSync ausgeführt werden, es gibt zwei Möglichkeiten, die Sie aktualisieren können: In-situ-Upgrades und parallele Bereitstellung. Ein in-situ-Upgrade für die meisten Kunden empfohlen wird und Sie über eine zuletzt verwendete verfügen Betriebssystem und kleiner als 50.000 Objekte. In anderen Fällen empfiehlt es eine parallele Bereitstellung führen, wo Ihre Dirsync-Konfiguration auf einen neuen Server ausgeführt Azure AD verbinden verschoben.

Wenn Sie Azure AD Synchronisieren eines in-situ-Upgrades empfohlen wird verwenden. Wenn Sie möchten, ist es möglich, installieren einen neuen Azure AD verbinden Server parallel, und führen Sie eine Migration Einsatz von Ihrem Server Azure AD synchronisieren zu Azure AD-verbinden.

Lösung | Szenario
----- | -----
[Upgrade von DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Wenn Sie einen vorhandenen Dirsync-Server bereits ausgeführt haben.</li>
[Upgrade von Azure AD-Synchronisierung](active-directory-aadconnect-upgrade-previous-version.md)| <li>Wenn Sie von Azure AD synchronisieren verschieben.</li>

Wenn Sie so Azure AD Verbinden eines in-situ-Upgrades von DirSync Aufgaben anzeigen möchten, klicken Sie dann finden Sie in diesem Video Channel 9:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>Häufig gestellte Fragen
**F: Ich habe eine e-Mail-Benachrichtigung erhalten, aus der Azure-Team und/oder eine Nachricht aus dem Office 365-Nachrichtencenter, aber verwende ich verbinden.**  
Die Benachrichtigung wurde für eine Build-Nummer 1.0 Azure AD-Verbinden mit Kunden auch gesendet. \*.0 (mit einer Version vor-1.1). Microsoft empfiehlt Kunden mit Azure AD verbinden Versionen auf dem laufenden. Mit 1.1, Funktion für die [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) vorgenommen werden wird, installiert wirklich einfach immer eine neuere Version von Azure AD verbinden aufweisen.

**F: wird DirSync/Azure AD-Synchronisierung beenden auf 13 April 2017 arbeiten?**  
Nein. Wenn diese nicht mehr zur Kommunikation mit Azure AD möglich werden der Termin wird zu einem späteren Zeitpunkt vorgestellt. Sie werden finden diese Informationen in diesem Thema, wenn verfügbar.

**F: können welche Dirsync-Versionen, die ich aus aktualisieren?**  
Es wird unterstützt, ein Upgrade von einer beliebigen Dirsync-Version derzeit verwendet wird.

**F: was der Azure AD-Verbinder für FIM/MIM?**  
Der Azure AD-Verbinder für FIM/MIM wurde **nicht** als veraltet markiert. Es ist bei **Feature fixieren**. keine neuer Funktionen hinzugefügt, und es keine Updates empfängt. Microsoft empfiehlt Kunden, die sie zum Planen, um es in Azure AD verbinden zu verschieben. Es wird dringend empfohlen, alle neuen Bereitstellungen arbeiten zu starten. Dieser Connector wird nicht mehr unterstützt in der Zukunft vorgestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

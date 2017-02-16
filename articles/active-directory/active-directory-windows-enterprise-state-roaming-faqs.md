<properties
    pageTitle="Und häufig gestellte Fragen zu roaming | Microsoft Azure"
    description="Bietet Antworten auf einige Fragen IT-Administratoren zu Einstellungen, und Synchronisieren der app-Daten möglicherweise."
    services="active-directory"
    keywords="Enterprise besagen roaming Einstellungen Windows Cloud, häufig gestellte Fragen zu Enterprise Zustand roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>Einstellungen und roaming häufig gestellte Fragen zu Daten

In diesem Thema werden einige Fragen, die IT-Administratoren zu Einstellungen, und Synchronisieren der app-Daten möglicherweise beantwortet.

## <a name="what-data-roams"></a>Welche Daten wechselt?
**Windows-Einstellungen**: der PC-Einstellungen, die in der Windows-Betriebssystem integriert sind. Im Allgemeinen Hierbei handelt es sich um Einstellungen, die Ihrem PC personalisieren und enthalten die folgenden allgemeinen Kategorien:

- *Design*, wozu auch Features wie desktop Design und die Taskleiste Einstellungen.
- *Internet Explorer-Einstellungen*geöffnet zuletzt einschließlich Registerkarten und Favoriten.
- *Browsereinstellungen Kante*, beispielsweise "Favoriten" und Liste lesen.
- *Kennwörter*, einschließlich Internetkennwörter und Wi-Fi-Profile.
- *Spracheinstellungen*, wozu auch die Einstellungen für den unterschiedlichen Tastaturlayouts, Systemsprache, Datum und Uhrzeit und vieles mehr.
- *Steigern der Access-Features*, wie z. B. Design mit hohem Kontrast, Sprachausgabe und Bildschirmlupe.
- *Andere Windows-Einstellungen*, wie etwa Eingabeaufforderungsfenster Einstellungen und Anwendungsliste.


**Anwendungsdaten**: Universal Windows-apps können Einstellungsdaten in einem roaming Ordner schreiben und Daten, die in diesem Ordner geschrieben werden automatisch synchronisiert werden. Es liegt an der einzelnen app-Entwickler zum Entwerfen einer app zu dieser Funktion nutzen. Weitere ausführliche Informationen zum Entwickeln von einem Universal Windows app, die verwendet roaming, finden Sie unter die [Appdata Speicher API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) und den [Windows 8 Appdata roaming Entwicklertools Blog](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Welches Konto für Einstellungen synchronisieren verwendet wird?
In Windows 8 und Windows 8.1 verwendet Einstellungen synchronisieren immer Consumer Microsoft-Konten. Enterprise-Benutzer haben die Möglichkeit, ein Microsoft-Konto mit ihrem Active Directory-Domäne-Konto den Zugriff auf Einstellungen synchronisieren verbinden. In Windows-10 verbunden dies Microsoft-Konto, dass die Funktionalität mit einer primären/sekundären Konto Framework ersetzt wird.

Das primäre Konto wird als bei Windows anmelden verwendete Konto definiert. Dies kann ein Microsoft-Konto, ein Konto Azure Active Directory (Azure AD), einer lokalen Active Directory-Konto oder ein lokales Konto sein. Zusätzlich zu den primären Konto können Windows 10 Benutzer Geräts ein oder mehrere sekundäre Cloud-Konten hinzufügen. Zweites Konto ist in der Regel ein Microsoft-Konto, ein Azure AD-Konto oder einem anderen Konto wie Gmail oder Facebook. Diese sekundären Konten bieten Zugriff auf Weitere Dienste wie einmaliges Anmelden und im Windows Store, aber sie können keine einschalten Einstellungen synchronisieren.

In Windows-10 nur das primäre Konto für das Gerät für Einstellungen synchronisieren verwendet werden kann (finden Sie unter [wie aktualisiere ich von Microsoft-Konto Einstellungen synchronisieren in Windows 8 auf Azure AD Einstellungen synchronisieren in Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Daten gemischt nie zwischen den verschiedenen Benutzerkonten auf dem Gerät. Es gibt zwei Regeln für die Einstellungen synchronisieren:

- Windows-Einstellungen werden immer mit dem primären Konto Roaming.
- App-Daten werden mit dem Konto verwendet, um die app erfassen gekennzeichnet. Nur apps markiert, durch das primäre Konto synchronisiert. App Besitz tagging wird bestimmt, wenn eine app Windows Store oder über die Verwaltung mobiler Geräte (MDM) Seite geladen wird.

Wenn eine app Besitzer identifiziert werden kann, wird es mit dem primären Konto Roaming. Wenn ein Gerät von Windows 8 oder Windows 8.1 für Windows 10 aktualisiert wurde, werden die Datei apps als von der Microsoft-Konto übernommen markiert sein. Dies ist, da die meisten Benutzer von apps im Windows Store erwerben, und es keine Windows Store-Unterstützung für Azure AD-Konten vor Windows 10 wurde. Wenn eine app über eine Lizenz offline installiert ist, wird die app mit dem primären Konto auf dem Gerät markiert sein.

>[AZURE.NOTE]  
> Windows-10-Geräten, die im Besitz eines Enterprise sind und mit Azure AD verbunden sind, können nicht mehr ihren Microsoft-Konten zu einem Domänenkonto herzustellen. Die Möglichkeit, ein Microsoft-Konto an einer Domäne verbinden und des Benutzers Daten synchronisieren mit dem Microsoft-Konto (d. h., das Microsoft-Konto über die verbundenen Microsoft-Konto und Active Directory-Funktionalität roaming) wird von Windows-10-Geräten entfernt, die in einer verbundenen Active Directory oder Azure AD-Umgebung verbunden sind.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Wie kann ich Aktualisieren von Microsoft-Konto Einstellungen synchronisieren in Windows 8 zu Azure AD Einstellungen synchronisieren in Windows 10?
Wenn Sie zu der Active Directory-Domäne unter Windows 8 oder Windows 8.1 mit einem verbundenen Microsoft-Konto verknüpft sind, werden Sie über Ihr Microsoft-Konto Einstellungen synchronisieren. Nach einem Upgrade auf Windows 10, werden Sie weiterhin benutzereinstellungen über Microsoft-Konto synchronisieren, solange Sie eine Domäne-Benutzer sind und Active Directory-Domäne mit Azure AD wird keine Verbindung hergestellt.

Wenn die lokalen Active Directory-Domäne herstellen von Verbindungen mit Azure AD, Ihr Gerät versucht, Synchronisieren mit den verbundenen Einstellungen Azure AD-Konto. Wenn der Administrator Azure AD-nicht Enterprise Zustand Roaming, Ihrer verbundenen aktiviert wird Azure AD-Konto wird die Synchronisierung Einstellungen. Wenn Sie ein Windows-10-Benutzer sind und Sie melden Sie sich mit einer Identität Azure AD-, starten Sie Windows-Einstellungen synchronisieren, sobald der Administrator Einstellungen Synchronisieren über Azure AD-ermöglicht.

Wenn Sie alle persönlichen Daten auf Ihrem Gerät corporate gespeichert haben, sollten beachten Sie, dass Windows-Betriebssystem und Anwendungsdaten beginnen soll mit Azure Active Directory synchronisiert. Dies weist die folgenden folgen:

- Ihre persönlichen Einstellungen der Microsoft-Konto werden wandert abgesehen von den Einstellungen auf Ihre Arbeit oder Schule Azure AD-Konten. Dies ist, da der Microsoft-Konto und Azure AD-Einstellungen synchronisieren separate Konten jetzt verwenden.
- Persönliche Daten wie Kennwörter Wi-Fi, Web Anmeldeinformationen und Internet Explorer-Favoriten, die zuvor über verbundene Microsoft-Konto synchronisiert wurden werden über Azure Active Directory synchronisiert werden.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Wie lassen sich Microsoft-Konto und Azure AD Enterprise Zustand Roaming Interoperability arbeiten?
In den November 2015 oder höhere Versionen von Windows 10 wird Enterprise Zustand Roaming nur ein einzelnes Benutzerkonto nacheinander unterstützt. Wenn Sie melden Sie sich bei Windows mithilfe einer Arbeit oder Schule Azure AD-Konto, werden alle Daten über Azure AD synchronisieren. Wenn Sie Windows mit einem persönlichen Microsoft-Konto anmelden, werden alle Daten über die Microsoft-Konto synchronisiert. Universeller Appdata wird Roaming nur das primäre Anmeldung-Konto auf dem Gerät verwenden, und von überall aus zugänglich nur, wenn das primäre Konto des app Lizenz Besitz ist. Universeller Appdata von den apps, die im Besitz eines sekundären Konten wird nicht synchronisiert werden.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Führen Sie Einstellungen für Azure AD-Konten aus mehreren Mandanten synchronisieren?
Wenn mehrere Azure AD aus anderen Konten Azure AD-Mandanten werden auf dem gleichen Gerät verwenden, müssen Sie der Registrierung des Geräts zur Kommunikation mit Azure Verwaltung von Informationsrechten (RMS Azure) für jede Azure AD-Mandanten aktualisieren.  

1. Suchen Sie die GUID für jede Azure AD-Mandanten. Öffnen Sie das Azure klassische Portal, und wählen Sie eine Azure AD-Mandanten. Die GUID für den Mandanten ist in der URL in der Adressleiste des Browsers. Beispiel: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Nachdem Sie die GUID haben, müssen Sie den Registrierungsschlüssel hinzufügen **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<Mandanten-ID-GUID >**.
Erstellen Sie einen neuen mit mehreren Zeichenfolgenwert (REG-MULTI-SZ) mit dem Namen **AllowedRMSServerUrls**aus dem Schlüssel **Mandanten-ID-GUID** . Geben Sie für die Daten die zur Lizenzierung Verteilung-Punkt-URLs der der Azure Mandanten, die das Gerät greift auf ein.
3. Finden Sie die URLs der zur Lizenzierung, indem Sie das Cmdlet " **Get-AadrmConfiguration** " ausführen. Wenn die Werte für die **LicensingIntranetDistributionPointUrl** und **LicensingExtranetDistributionPointUrl** anders ab, werden beide Werte angeben. Wenn die Werte gleich sind, geben Sie den Wert nur einmal an.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Was sind die roaming Einstellungsoptionen für vorhandene Windows-desktopanwendungen?
Roaming funktioniert nur für Universal Windows-apps. Es gibt zwei Optionen für die Aktivierung auf einer vorhandenen Windows-desktop-Anwendung roaming zur Verfügung:

- Die [Desktop-Bridge](http://aka.ms/desktopbridge) hilft Ihnen, Ihre vorhandene Windows-desktop-apps auf Universal Windows-Plattform zu bringen. Hier werden minimalen Code Änderungen benötigt Azure AD-app Daten roaming nutzen. Die Desktop-Brücke bietet Ihrer apps mit einer app-ID, die app-Daten für vorhandene desktop-apps roaming aktivieren erforderlich ist.
- [Benutzer Erfahrung Virtualisierung (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) unterstützt Sie beim Erstellen einer benutzerdefinierten Einstellungen Vorlage für vorhandene Windows-desktop-apps und roaming für Win32-apps zu aktivieren. Diese Option erfordert keine der app-Entwickler Code der app zu ändern. UE-V beträgt lokalen Active Directory-roaming für Kunden, die Microsoft Desktop Optimization Pack erworben haben.

Administratoren können konfigurieren UE-V, um die Windows-desktop-app Daten Roaming durch Ändern der roaming von Windows-Betriebssystem und universeller app durch [Gruppieren von UE-V Richtlinien](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), einschließlich:

- Windows-Einstellungen Gruppenrichtlinien Roaming
- Windows-Apps Gruppenrichtlinien nicht synchronisieren
- Internet Explorer im Abschnitt Applications roaming

Microsoft möglicherweise in der Zukunft Methoden UE-V tief in Windows integriert und erweitern UE-V, um die Einstellungen in der Cloud Azure AD-Roaming ermitteln.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Kann ich synchronisierten Einstellungen und Daten lokal speichern?
Enterprise Zustand Roaming, werden alle synchronisierte Daten in der Cloud Azure gespeichert. UE-V bietet eine lokale roaming Lösung.

## <a name="who-owns-the-data-thats-being-roamed"></a>Wer besitzt die Daten, die Server gespeichert werden?
Der eigenen Unternehmen ein Roaming Daten über Enterprise Zustand Roaming. Daten werden in einem Rechenzentrum Azure gespeichert. Alle Benutzerdaten ist verschlüsselt, während der Übertragung sowohl statisch sind in der Cloud Azure RMS verwenden. Dies ist eine Verbesserung im Vergleich zu Microsoft-Konto-basierten Einstellungen synchronisieren, das nur bestimmte vertraulichen Daten, z. B. Benutzeranmeldeinformationen verschlüsselt, bevor sie das Gerät verlässt.

Microsoft sieht sich verpflichtet Kundendaten zu schützen. Eine Enterprise Benutzerdaten Einstellungen werden automatisch von Azure RMS verschlüsselt, bevor sie ein Gerät Windows 10 verlässt, sodass kein anderer Benutzer diese Daten lesen kann. Wenn Ihre Organisation ein kostenpflichtiges Abonnement für RMS Azure hat, können Sie andere Azure RMS-Features, wie z. B. nachverfolgen verwenden Dokumente widerrufen, automatisch schützen von e-Mails, die vertraulichen Informationen enthalten und Verwalten Ihrer eigenen Tasten (die "bringen Ihren eigenen Schlüssel" Lösung, auch bekannt als BYOK). Weitere Informationen zu diesen Funktionen und Funktionsweise von Windows Azure-RMS finden Sie unter [Was ist die Verwaltung von Informationsrechten Azure](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kann ich die Synchronisierung für eine bestimmte app oder Einstellung verwalten?
In Windows-10 gibt es keine MDM oder Gruppenrichtlinien-Einstellung, um für eine einzelne Anwendung roaming deaktivieren. Mandantenadministratoren Appdata synchronisieren für alle apps auf einem verwalteten Gerät deaktivieren können, aber kein feiner Steuerelement auf einer Ebene pro-app oder innerhalb der app vorhanden ist.

## <a name="how-can-i-enable-or-disable-roaming"></a>Wie kann ich aktivieren oder Deaktivieren von roaming?
Wechseln Sie in den **Einstellungen** -app zu **Konten** > **synchronisieren Sie Ihre Einstellungen**. Auf dieser Seite können Sie sehen, welches Konto verwendet wird, um die Einstellungen Roaming, und Sie aktivieren oder deaktivieren Sie einzelne Gruppen von Einstellungen auf dem Server gespeichert werden können.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Was ist Microsoft Empfehlungen für die Aktivierung in Windows 10 roaming?
Microsoft hat ein paar andere Einstellungen roaming Lösungen, einschließlich Roaming von Benutzerprofilen, UE-V und Enterprise Zustand Roaming.  Microsoft betrachtet es als Verpflichtung einer Investition in Enterprise besagen Roaming in zukünftigen Versionen von Windows. Wenn Ihre Organisation mit Verschieben von Daten in der Cloud nicht bereit oder vertraut ist, empfehlen wir, dass Sie UE-V als Ihre primäre roaming Technologie verwenden. Wenn Ihre Organisation roaming-Unterstützung für vorhandene Windows-desktopanwendungen erfordert, aber möchten sofort richtig loslegen in die Cloud verschoben wird, wird empfohlen, dass Sie Enterprise Zustand Roaming und UE-V verwenden. Obwohl UE-V und Enterprise Zustand Roaming Technologien sehr ähnlich sind, sind sie nicht gegenseitig. Diese ergänzen einander, um sicherzustellen, dass Ihre Organisation die roaming Dienste bereitstellt, die Ihre Benutzer benötigen.  

Bei Verwendung von Enterprise Zustand Roaming und UE-V gelten die folgenden Regeln:

- Enterprise Zustand Roaming ist der primären roaming Agent auf dem Gerät. UE-V wird verwendet, um die "Win32-Lücke" ergänzen
- UE-V für Windows-Einstellungen und moderne UWP app-Daten roaming sollte deaktiviert werden, wenn mithilfe der Gruppe UE-V Richtlinien. Diese sind bereits durch Enterprise Zustand Roaming abgedeckt.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Wie unterstützt Enterprise Zustand Roaming virtual desktop Infrastructure (VDI)?
Enterprise Zustand Roaming wird auf Windows-10-Client SKUs, aber nicht an den Server SKUs unterstützt. Wenn ein virtueller Computer-Client auf einem Computer Hypervisor gehostet wird, und Sie Remote melden Sie sich bei der virtuellen Computern, werden Ihre Daten zugreifen. Wenn mehrere Benutzer, das Betriebssystem freigeben und Benutzer Remote melden Sie sich bei einem Server für eine vollständige desktop Erfahrung, roaming funktioniert möglicherweise nicht. Im zweite Sitzung-basierten Szenario wird formal nicht unterstützt.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Was passiert bei meiner Organisation Azure RMS EC-oder nach der Verwendung von roaming?
Wenn Ihre Organisation bereits verwendet roaming in Windows 10 mit der Azure RMS Beschränkung Einsatz kostenloses Abonnement erwerben ein kostenpflichtiges Abonnement für RMS Azure hat keinen Einfluss auf die Funktionalität des Features roaming und werden keine Änderungen Konfiguration von Ihrem IT-Administrator benötigt.

## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie versuchen, melden Sie sich mit Ihrem Windows-Gerät mit einer Smartcard oder virtuelle Smartcard, Beenden der Einstellungen synchronisieren arbeiten. Zukünftige Updates in Windows 10 möglicherweise dieses Problem beheben.
- Sie benötigen das Juli kumulative Update für Windows-10 (Build 10586.494 oder höher) für Internet Explorer-Favoriten synchronisieren, wenn Sie arbeiten möchten.
- Daten, die mit Windows Schutz von Informationen geschützt ist werden durch Enterprise Zustand Roaming nicht synchronisieren. Darüber hinaus treten Computer, auf denen Windows Schutz von Informationen aktiviert nicht Design synchronisieren. 
- Unter bestimmten Umständen können Enterprise Zustand Roaming fehl, um Daten zu synchronisieren, wenn Azure kombinierte Authentifizierung konfiguriert ist.
    - Wenn Ihr Gerät Azure Active Directory-Portal [Mehrstufige Authentifizierung](multi-factor-authentication.md) erforderlich ist so konfiguriert ist, wird Sie möglicherweise Einstellungen bei der Anmeldung bei einem Windows-10-Gerät mit einem Kennwort zu synchronisieren. Diese Art der Konfiguration kombinierte Authentifizierung soll eine Azure Administratorkonto schützen. Benutzer Administrator möglicherweise noch melden Sie sich an ihren Windows-10-Geräten mit ihren [Microsoft Passport für die Arbeit](active-directory-azureadjoin-passport.md) PIN oder Dateipfad kombinierte Authentifizierung beim Zugriff auf andere Azure Dienste wie Office 365 zu synchronisieren.
    - Synchronisieren kann fehl, wenn der Administrator die bedingte Zugriffsrichtlinie Active Directory Federation Services mehrstufige Authentifizierung konfiguriert und das Zugriffstoken auf dem Gerät läuft ab.  Stellen Sie sicher, dass anmelden und Abmelden die PIN [Microsoft Passport für die Arbeit](active-directory-azureadjoin-passport.md) mit oder kombinierte Authentifizierung beim Zugriff auf andere Azure Dienste wie Office 365 abzuschließen.

- Ist ein Computer Domänenverbund automatische Registrierung Azure Active Directory-Geräten, kann es synchronisieren Fail kommen, wenn der Computer für längere Zeit externen ist und Domänenauthentifizierung kann nicht abgeschlossen werden. Um dieses Problem zu beheben, schließen Sie den Computer mit einem Unternehmensnetzwerk, sodass die Synchronisierung fortsetzen kann.


## <a name="related-topics"></a>Verwandte Themen
- [Enterprise-Zustand roaming (Übersicht)](active-directory-windows-enterprise-state-roaming-overview.md)
- [Aktivieren Sie Enterprise-Status in Azure Active Directory roaming](active-directory-windows-enterprise-state-roaming-enable.md)
- [Für Gruppenrichtlinien und MDM Einstellungen für Einstellungen synchronisieren](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows-10 roaming Einstellungen Bezug](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

<properties
    pageTitle="Gruppieren von Richtlinien-und MDM | Microsoft Azure"
    description="Informationen zum Gruppenrichtlinien und Mobilgerät Management (MDM)-Einstellungen, die auf Geräten im Unternehmen im Besitz verwendet werden soll. Diese Richtlinien werden auf dem Gerät des Benutzers gesamte angewendet."
    services="active-directory"
    keywords="Was sind Gruppe Richtlinie und MDM-Einstellungen für Enterprise Zustand Roaming, Enterprise Zustand Roaming, Windows Cloud"
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

# <a name="group-policy-and-mdm-settings"></a>Gruppe Richtlinien-und MDM

Verwenden Sie diese Gruppenrichtlinien und Einstellungen für mobilen Gerät Management (MDM) nur auf Geräten im Unternehmen im Besitz, da diese Richtlinien auf dem Gerät des Benutzers gesamte angewendet werden. Anwenden einer Richtlinie MDM zum Synchronisieren von Einstellungen für eine persönliche deaktivieren, werden im Besitz eines Benutzers Gerät die Verwendung dieses Geräts beeinträchtigen. Darüber hinaus sein andere Benutzerkonten auf dem Gerät auch die Richtlinie betroffen.

Unternehmen, die für persönliche (nicht verwalteten) Geräte roaming verwalten möchten können Azure-Portal zu aktivieren oder Deaktivieren von roaming anstelle der Verwendung von Gruppenrichtlinien oder MDM
Die folgenden Tabellen beschreiben die Richtlinieneinstellungen zur Verfügung.

## <a name="mdm-settings"></a>MDM-Einstellungen
Die Richtlinieneinstellungen MDM gelten für Windows 10 und 10 unter Windows Mobile.  Unterstützung von 10 unter Windows Mobile ist nur für Microsoft-Konto basierend roaming über OneDrive-Konto des Benutzers vorhanden.  Lizenzinformationen finden Sie im Abschnitt "Geräte und Endpunkte" Details zum Synchronisieren von Azure AD-basierten welche Geräte unterstützt werden.

| Namen                               | Beschreibung                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsoft-Konto Verbindung zulassen | Ermöglicht Benutzern die Authentifizierung mit einem Microsoft-Konto auf dem Gerät |
| Zulassen von synchronisieren Meine Einstellungen             | Ermöglicht Benutzern das Windows-Einstellungen und app-Daten Roaming; Durch Deaktivieren dieser Richtlinie wird synchronisieren als auch Sicherungskopien auf mobilen Geräten deaktivieren                  |

## <a name="group-policy-settings"></a>Gruppenrichtlinieneinstellungen
Die Gruppenrichtlinien beziehen sich auf Windows-10-Geräten, die Active Directory-Domäne hinzugefügt werden. Die Tabelle enthält auch legacy Einstellungen, die zum Verwalten von synchronisierungseinstellungen angezeigt, aber nicht funktionieren für Enterprise Zustand Roaming für Windows 10, die mit 'Kann nicht in der Beschreibung verwenden' genannt sind.

| Namen                                | Beschreibung |
|-------------------------------------|-------------|
| Konten: Blockieren Microsoft-Konten  |Diese Einstellung wird verhindert, dass Benutzer neue Microsoft-Konten auf diesem Computer hinzufügen|
| Kann ich nicht synchronisieren                         |Verhindert, dass Benutzer, um die Windows-Einstellungen und app-Daten Roaming|
| Kann ich nicht synchronisieren personalisieren             |Deaktiviert die Synchronisierung der Gruppe "Designs"|
| Kann ich Browsereinstellungen nicht synchronisieren        |Deaktiviert die Synchronisierung der Gruppe Internet Explorer|
| Kann ich Kennwörter nicht synchronisieren               |Deaktiviert die Synchronisierung Kennwörter Gruppe|
| Kann ich andere Windows-Einstellungen nicht synchronisieren  |Deaktiviert die Synchronisierung der anderen Windows-Einstellungen-Gruppe|
| Desktop für eine Personalisierungswebsite kann nicht synchronisieren |Verwenden Sie nicht. hat keine Auswirkung|
| Führen Sie getaktete Verbindungen nicht synchronisieren  |Klicken Sie auf roaming deaktiviert getaktete Verbindungen, wie etwa Mobile 3 G|
| Kann ich apps nicht synchronisieren                    |Verwenden Sie nicht. hat keine Auswirkung|
|Kann ich Einstellungen für die app nicht synchronisieren             |Deaktiviert roaming der app-Daten|
|Kann ich Start-Einstellungen nicht synchronisieren           |Verwenden Sie nicht. hat keine Auswirkung|


## <a name="related-topics"></a>Verwandte Themen
- [Enterprise Zustand Roaming (Übersicht)](active-directory-windows-enterprise-state-roaming-overview.md)
- [Aktivieren Sie Enterprise-Status in Azure Active Directory roaming](active-directory-windows-enterprise-state-roaming-enable.md)
- [Einstellungen und roaming häufig gestellte Fragen zu Daten](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows-10 roaming Einstellungen Bezug](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

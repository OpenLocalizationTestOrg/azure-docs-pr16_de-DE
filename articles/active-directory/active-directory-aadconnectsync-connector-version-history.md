<properties
   pageTitle="Verbinder Release Versionsverlauf | Microsoft Azure"
   description="Dieses Thema enthält alle Versionen der Connectors für Forefront Identität-Manager (FIM) und Microsoft Identität-Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Verbinder Release Versionsverlauf
Der Verbinder für Forefront Identität-Manager (FIM) und Microsoft Identität-Manager (MIM) werden häufig aktualisiert.

>[AZURE.NOTE]
In diesem Thema wird nur FIM und MIM. Diese Connectors werden auf Azure AD Verbinden nicht unterstützt.

In diesem Thema Liste aller Versionen der Verbinder, die freigegeben wurden.

Links zu verwandten Themen:

- [Laden Sie die neuesten Verbinder](http://go.microsoft.com/fwlink/?LinkId=717495)
- Dokumentation zur [Allgemeinen LDAP-Connector](active-directory-aadconnectsync-connector-genericldap.md)
- Dokumentation zur [Allgemeinen SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md)
- Dokumentation zur [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245)
- Dokumentation zur [PowerShell-Connector](active-directory-aadconnectsync-connector-powershell.md)
- Dokumentation zur [Lotus Domino Verbinder](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Veröffentlicht: 2016 März

**Neue Verbinder**  
Erste Version des [Generische SQL-Verbinder](active-directory-aadconnectsync-connector-genericsql.md).

**Neue Features:**

- Allgemeine LDAP-Connector:
    - Unterstützung für Delta Import mit Isode hinzugefügt.
- Web Services Connector:
    - Aktualisiert die CsEntryChangeResult Aktivitäten und SetImportErrorCode Aktivitäten dürfen Objekt abgleichen Fehler wieder in die Synchronisierung-Engine zurückgegeben werden soll.
    - Aktualisiert die SAP6 und SAP6User-Vorlagen, um die neuen Objekt abgleichen Fehler Funktionen verwenden.
- Lotus Domino Verbinder:
    - Für den Export benötigen Sie eine Certifier pro Adressbuch. Dasselbe Kennwort für alle Zertifizierer können jetzt um die Verwaltung zu erleichtern.

**Feste Probleme:**

- Allgemeine LDAP-Connector:
    - Einige Bezug Attribute wurden für IBM Tivoli DS nicht richtig erkannt.
    - Für Open LDAP ein Delta Import wurden die Leerzeichen am Anfang und Ende von Zeichenfolgen abgeschnitten.
    - Für Novell und NetIQ umbenannt ein exportieren, die ein Objekt zwischen Organisationseinheiten/Containern und gleichzeitig verschoben des Objekts fehlgeschlagen ist.
- Web Services Connector:
    - Wenn der Webdienst mehrere Endpunkte für die gleiche Bindung vorkam, dann der Verbinder nicht richtig diese Endpunkte ermitteln.
- Lotus Domino Verbinder:
    - Das Attribut FullName zu einer Datenbank e-Mail in ein Export funktioniert nicht.
    - Eine exportieren, die sowohl hinzugefügt und Mitglied aus einer Gruppe entfernt exportiert nur die Mitglieder hinzugefügt.
    - Wenn ein Notes-Dokument ungültige (das Attribut IsValid auf False gesetzt), und klicken Sie dann auf der Verbinder ist fehlerhaft ist.

## <a name="older-releases"></a>Ältere Versionen
Bevor Sie März 2016 wurden die Verbinder als Supportthemen zur Verfügung.

**Allgemeine LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, September 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 März
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, Januar 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, September 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 März

**Webdienste**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, September 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, September 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, September 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 März
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, August 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, Februar 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, Oktober 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, August 2013

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD verbinden synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

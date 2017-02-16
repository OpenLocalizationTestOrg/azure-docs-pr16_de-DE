<properties
   pageTitle="Azure-Active Directory Wartezeiten Reporting | Microsoft Azure"
   description="Wie lange dauert für Berichte Ereignisse in Ihrem Azure-Active Directory angezeigt."
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Azure-Active Directory-Bericht Wartezeiten

*Diese Dokumentation ist Teil des [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Bericht                                                  | Minimalwert  | Mittelwert    | Maximum
------------------------------------------------------- | -------- | ---------- | ----------
**Berichte zur Sicherheit**                                    |          |            |
Unregelmäßiges Aktivität                              | 2 Stunden  | 4 Stunden    | 8 Stunden
Melden Sie sich-ins aus unbekannten Quellen                           | 2 Stunden  | 4 Stunden    | 8 Stunden
Melden Sie sich-ins nach mehreren Fehlern                        | 2 Stunden  | 4 Stunden    | 8 Stunden
Melden Sie sich-ins aus mehreren geografischen Standorten                      | 2 Stunden  | 4 Stunden    | 8 Stunden
Melden Sie sich-ins von IP-Adressen mit verdächtigen Aktivität     | 2 Stunden  | 4 Stunden    | 8 Stunden
Melden Sie sich-ins von möglicherweise infizierten Geräte                 | 2 Stunden  | 4 Stunden    | 8 Stunden
Benutzer mit abweichenden Aktivität                   | 2 Stunden  | 4 Stunden    | 8 Stunden
Benutzer mit verlorene Anmeldeinformationen                           | 2 Stunden  | 4 Stunden    | 8 Stunden
**Anwendung-Berichte**                                 |          |            |
Berücksichtigen Sie die Aktivität bereitgestellt                           | 2 Stunden  | 4 Stunden    | 8 Stunden
Fehler bei der Bereitstellung zu berücksichtigen                             | 2 Stunden  | 4 Stunden    | 8 Stunden
Anwendungsverwendung                                       | 2 Stunden  | 4 Stunden    | 8 Stunden
Kennwort Rollover status                                | 2 Stunden  | 4 Stunden    | 8 Stunden
**Audit und Berichte**                            |          |            |
Überwachungsbericht                                            | 1 minute | 15 Minuten | 30 Minuten
Zurücksetzen des Kennworts Aktivität (Azure AD)                      | 2 Stunden  | 4 Stunden    | 8 Stunden
Zurücksetzen des Kennworts Aktivität (Identität Manager)              | 2 Stunden  | 4 Stunden    | 8 Stunden
Zurücksetzen des Kennworts Registrierungsaktivität (Azure AD-)         | 2 Stunden  | 4 Stunden    | 8 Stunden
Zurücksetzen des Kennworts Registrierungsaktivität (Identität Manager) | 2 Stunden  | 4 Stunden    | 8 Stunden
Self-service Gruppen Aktivität (Azure AD-)                 | 2 Stunden  | 4 Stunden    | 8 Stunden
Self-service Gruppen Aktivität (Identität Manager)         | 2 Stunden  | 4 Stunden    | 8 Stunden
**RMS-Berichte**                                         |          |            |
Am aktivsten RMS-Benutzer                                   | 2 Stunden  | 4 Stunden    | 8 Stunden
RMS Verwendung                                               | 2 Stunden  | 4 Stunden    | 8 Stunden
Verwendung der RMS-Gerät                                        | 2 Stunden  | 4 Stunden    | 8 Stunden
Verwendung der aktiviert RMS-Anwendung                           | 2 Stunden  | 4 Stunden    | 8 Stunden
**Privat Berichten in der Vorschau**                             |          |            |
Aktivität für alle Benutzer anmelden                               | 2 Stunden  | 4 Stunden    | 8 Stunden

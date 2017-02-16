<properties
    pageTitle="Azure AD-verbinden: Synchronisieren Dienstinstanzen | Microsoft Azure"
    description="Diese Seite Dokumente für Azure AD-Instanzen Besonderheiten."
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

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD-verbinden: Besonderheiten Instanzen
Azure AD verbinden wird am häufigsten verwendet, mit der World Wide-Instanz von Azure AD- und Office 365. Aber auch andere Instanzen vorhanden sind, und Sie verfügen über unterschiedliche Anforderungen URLs oder andere spezielle Aspekte.

## <a name="microsoft-cloud-germany"></a>Microsoft-Cloud-Deutschland
Die [Microsoft-Cloud-Deutschland](http://www.microsoft.de/cloud-deutschland) ist einer unabhängigen Cloud von Treuhänder Deutsch Daten betrieben.

Diese Cloud gibt es zurzeit in der Vorschau. Viele der Szenarios, die sich selbst, indem Sie wie gewohnt können Domänen überprüfen, durch den Operator vorgenommen werden. Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter für Weitere Informationen zur Teilnahme an der Vorschau an.

So öffnen Sie in dem Proxyserver URLs |
--- |
\*. microsoftonline.de |
\*. Windows |
+ Zertifikatsperrlisten |

Bei der Anmeldung bei Ihrem Verzeichnis Azure AD-müssen Sie ein Konto in der Domäne onmicrosoft.de verwenden.

Features, die aktuell nicht in der Microsoft-Cloud-Deutschland präsentieren:

- Azure AD verbinden Integrität ist nicht verfügbar.
- Automatische Updates ist nicht verfügbar.
- Kennwort abgeschlossenen writebackvorgängen ist nicht verfügbar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government cloud
Der [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/) ist eine Cloud für US-Regierung.

Diese Cloud wurde von früheren Versionen von DirSync unterstützt wurde. Aus Build 1.1.180 von Azure AD verbinden nächsten wird der zweiten Generation der Cloud unterstützt. In der zweiten Generation ist nur US-basierend Endpunkte verwenden und eine andere Liste der URLs in dem Proxyserver geöffnet haben.

So öffnen Sie in dem Proxyserver URLs |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Zertifikatsperrlisten |

Verbinden von Azure AD werden nicht automatisch erkennen können, dass Ihr Azure AD-Verzeichnis in der Cloud Government befindet. Stattdessen müssen Sie die folgenden Aktionen aus, bei der Installation von Azure AD verbinden.

1. Starten Sie die Installation Azure AD verbinden.
2. Sobald die die erste Seite angezeigt, in dem Sie überfüllt um den Endbenutzer-Lizenzvertrag anzunehmen, nicht fortgesetzt werden, und lassen Sie den Assistenten zum Installieren ausgeführt.
3. Starten Sie Regedit ein, und Ändern des Registrierungsschlüssels `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` auf den Wert `2`.
4. Wechseln Sie wieder zum Verbinden von Azure AD-Installation-Assistenten, akzeptieren den Endbenutzer-Lizenzvertrag und fortzufahren. Während der Installation sicherzustellen Sie, verwenden Sie den Pfad der **benutzerdefinierten Konfiguration** Installation (und nicht Express Installation). Klicken Sie dann fortzusetzen Sie die Installation wie gewohnt.

Features, die aktuell nicht in der Cloud Microsoft Azure Government präsentieren:

- Azure AD verbinden Integrität ist nicht verfügbar.
- Automatische Updates ist nicht verfügbar.
- Kennwort abgeschlossenen writebackvorgängen ist nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

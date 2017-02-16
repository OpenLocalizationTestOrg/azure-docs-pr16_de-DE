<properties
   pageTitle="Konfigurieren von Sicherheitshinweisen | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren von Sicherheitshinweisen für Azure berechtigten Identität-Erweiterung."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Konfigurieren von Sicherheitshinweisen bei Azure AD berechtigten Identität Verwaltung

## <a name="security-alerts"></a>Von Sicherheitshinweisen
Azure berechtigten Identität Management (PIM) generiert Benachrichtigungen, wenn es verdächtige oder unsicheren Aktivitäten in Ihrer Umgebung. Wenn eine Warnung ausgegeben wird, zeigt es auf dem Dashboard PIM nach oben. Wählen Sie die Benachrichtigung, um einen Bericht anzuzeigen, der Listet die Benutzer oder Rollen aus, die die Warnung ausgelöst.

![PIM Dashboard von Sicherheitshinweisen - screenshot][1]



| Benachrichtigen | Auslösen | Empfehlungen |
| ----- | ------- | -------------- |
| **Außerhalb PIM werden Rollen zugewiesen wird** | Ein Administrator wurde eine Rolle außerhalb der Benutzeroberfläche PIM dauerhaft zugewiesen. | Überprüfen Sie die neue Rolle Zuordnung an. Da permanente Administratoren nur von anderen Diensten zuweisen können, ändern Sie ihn in eine Zuordnung berechtigt, falls erforderlich. |
| **Rollen werden häufig auch aktiviert wird** | Es wurden zu viele Reaktivierungsverhältnis der gleichen Rolle innerhalb der in den Einstellungen hierfür vorgesehene Zeit. | Wenden Sie sich an den Benutzer aus, um anzuzeigen, warum sie die Rolle des so oft aktiviert haben. Möglicherweise ist der Frist zu kurz für, damit ihre Aufgaben ausführen, oder vielleicht sie Skripts verwenden, um automatisch eine Rolle zu aktivieren. |
| **Rollen erforderlich nicht für die Aktivierung mehrstufige Authentifizierung.** | Es gibt Rollen ohne MFA in den Einstellungen aktiviert. | Wir MFA für die am häufigsten hohen Berechtigungen Rollen erforderlich, jedoch können dringend empfohlen, dass Sie für die Aktivierung aller Rollen MFA aktivieren. |
| **Administratoren werden nicht ihren berechtigten Rollen verwenden.** | Es gibt berechtigte Administratoren, die ihre Rollen zuletzt nicht aktiviert haben. | Starten Sie eine Access-überprüfen Ermittlung der Benutzer, die Access nicht mehr benötigen. |
| **Es gibt zu viele globale Administratoren** | Es gibt mehr globale Administratoren als empfohlen. | Wenn Sie eine hohe Anzahl von globalen Administratoren verfügen, ist es wahrscheinlich, dass Benutzer weitere Berechtigungen müssen, sondern abrufen. Verschieben von Benutzern, die weniger berechtigten Rollen, oder stellen sie einige berechtigt statt der Rolle des dauerhaft zugewiesen. |

## <a name="configure-security-alert-settings"></a>Konfigurieren der Sicherheit Benachrichtigungseinstellungen

Sie können einige der von Sicherheitshinweisen in PIM für die Arbeit mit Ihrer Umgebung und Sicherheitsziele anpassen. Wie folgt vor, um das Blade Einstellungen zu erreichen:

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) aus, und wählen Sie die Kachel **Azure AD berechtigten Identitätsmanagement** aus dem Dashboard.
2. Wählen Sie **Berechtigungen Rollen verwaltet** > **Einstellungen** > **Benachrichtigungen Einstellungen**.

    ![Navigieren Sie zu der Sicherheitseinstellungen für Benachrichtigungen][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Benachrichtigung "Rollen sind auch häufig aktiv"

Diese Warnung wird ausgelöst, wenn ein Benutzer die Rolle des gleiche Stufe mehrmals innerhalb eines bestimmten Zeitraums aktiviert. Sie können sowohl den Zeitraum und die Anzahl der Vorgänge konfigurieren.

- **Aktivierung Erneuerung Zeitrahmen**: Geben Sie in Tagen, Stunden, Minuten, und die zweite des Zeitraums, die Sie zum Nachverfolgen von verdächtigen Erneuerung verwenden möchten.

- **Anzahl der Aktivierung Erneuerung**: Geben Sie die Anzahl der Vorgänge, 2 und 100, die Sie der Benachrichtigung innerhalb des Zeitrahmens betrachten Sie ausgewählt haben. Sie können diese Einstellung, indem Sie den Schieberegler verschieben oder eine Zahl in das Textfeld eingeben ändern.


### <a name="there-are-too-many-global-administrators-alert"></a>Benachrichtigung "Befinden sich zu viele globale Administratoren"

Diese Warnung wird PIM auslöst, wenn zwei verschiedene Kriterien erfüllt sind, und Sie können beide zu konfigurieren. Zuerst müssen Sie einen bestimmten Schwellenwert von globalen Administratoren erreicht haben. Zweites, muss ein bestimmten Prozentsatz von Ihrem total rollenzuweisungen globale Administratoren. Wenn Sie nur eine der folgenden Maße entsprechen, wird die Warnung nicht angezeigt.  

- **Minimale Anzahl von globalen Administratoren**: Geben Sie die Anzahl von globalen Administratoren, 2 und 100, ein unsicheren Betrags zu berücksichtigen.

- **Prozentsatz der globalen Administratoren**: Geben Sie den Prozentsatz der Administratoren, die globale Administratoren sind, von 0 % und 100 %, die in Ihrer Umgebung unsicher ist.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"ihre berechtigten Rollen Administratoren werden nicht mithilfe von" Benachrichtigung

Diese Warnung wird ausgelöst, wenn ein Benutzer einen bestimmten Zeitraum wechselt, ohne zu eine Rolle zu aktivieren.

- **Anzahl der Tage**: Geben Sie die Anzahl von Tagen zwischen 0 und 100, die ein Benutzer wechseln kann, ohne zu eine Rolle zu aktivieren.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png

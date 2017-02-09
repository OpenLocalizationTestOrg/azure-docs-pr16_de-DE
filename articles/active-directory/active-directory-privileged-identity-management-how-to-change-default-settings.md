<properties
   pageTitle="Zum Verwalten von Einstellungen für die Aktivierung von Rolle | Microsoft Azure"
   description="Informationen Sie zum Ändern der Standardeinstellungen für berechtigte Identitäten mit der Erweiterung Azure-Active Directory Berechtigungen der Identität-Verwaltung."
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
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Zum Verwalten der Rolle Aktivierungs-Einstellungen bei Azure AD berechtigten Identität Verwaltung

Ein Administrator Rolle Stufe kann Azure AD berechtigten Identitätsmanagement (PIM) in ihrer Organisation, einschließlich des Änderns der Benutzeroberfläche für ein Benutzer, der eine Zuordnung berechtigt Rolle aktivieren ist anpassen.

## <a name="manage-the-role-activation-settings"></a>Verwalten Sie die Rolle Aktivierungs-Einstellungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) aus, und wählen Sie die **Azure AD berechtigten Identitätsmanagement** app aus dem Dashboard.
2. Wählen Sie **Verwalten von Berechtigungen Rollen** > **Einstellungen** > **Berechtigten Rollen**.
3. Wählen Sie der Rolle aus, dessen Einstellungen Sie verwalten möchten.

Auf der Einstellungsseite für die einzelnen Rollen gibt es eine Reihe von, die Sie konfigurieren können. Diese Einstellung wirkt sich nur auf Benutzer, die berechtigt und nicht permanent Administratoren sind.

**Vorgänge**: die Zeit in Stunden, die eine Rolle aktiv bleibt, bevor diese abgelaufen ist. Dies kann zwischen 1 und 72 Stunden sein.

**Benachrichtigungen**: Sie können entscheiden, ob das System sendet e-Mails an Administratoren zu bestätigen, dass sie eine Rolle aktiviert haben. Dies kann für das Erkennen von unbefugten oder unzulässigen Vorgänge hilfreich sein.

**Vorfall/Anforderungsticket**: Sie können auswählen, ob er erfordern berechtigte Administratoren eine Ticketnummer beim Aktivieren von ihrer Rolle einbezogen werden sollen. Dies kann hilfreich sein, wenn Sie Rolle Access Überwachungen durchführen.

**Mehrstufige Authentifizierung**: Sie können auswählen, ob er festlegen, dass Benutzer ihre Identität mit MFA überprüfen, bevor Sie ihre Rollen aktiviert werden kann. Sie müssen lediglich überprüfen dies einmal pro Sitzung, nicht jedes Mal, wenn eine Rolle zu aktivieren. Es gibt zwei Tipps zu beachten, wenn Sie MFA aktivieren:

- Benutzer, die Microsoft-Konten für ihre e-Mail-Adressen haben (normalerweise @outlook.com, aber nicht immer) kann nicht für Azure MFA registrieren. Wenn Sie Benutzer mit einem Microsoft-Konto Rollen zuweisen möchten, sollten Sie permanente Administratoren machen oder MFA für diese Funktion deaktivieren.

- Sie können keine MFA für hohen Berechtigungen Rollen für Azure AD deaktivieren und Office 365. Dies ist ein Sicherheitsfeature, da diese Rollen sorgfältig geschützt werden sollen:  

    - Anwendungsadministrator
    - Proxy-Serveradministrator Anwendung
    - Abrechnung-administrator  
    - Compliance-administrator  
    - CRM Dienstadministrator
    - Kunden LockBox Access genehmigenden Person
    - Verzeichnis Autor  
    - Exchange-administrator  
    - Globaler administrator
    - Dienstadministrator Intune
    - Postfachadministrator  
    - Partner tier1-support  
    - Partner tier2-support  
    - Rolle Stufe administrator   
    - Sicherheitsadministrator  
    - SharePoint-administrator  
    - Skype für Business-administrator  
    - Benutzer Konto-administrator  

Weitere Informationen zur Verwendung von MFA mit PIM finden Sie unter [So MFA erforderlich](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

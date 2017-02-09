<properties
   pageTitle="So verwenden Sie das Überwachungsprotokoll | Microsoft Azure"
   description="Erfahren Sie, wie das Überwachungsprotokoll in der Azure berechtigten Identität-Erweiterung verwenden."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-use-the-audit-log-in-azure-ad-privileged-identity-management"></a>Verwenden Sie das Überwachungsprotokoll in Azure AD berechtigten Identitätsmanagement

Überwachungsprotokoll berechtigten Identität Management (PIM) können um alle Benutzer Zuordnungen und Vorgänge innerhalb eines bestimmten Zeitraums anzuzeigen. Wenn Sie die vollständige Audit-Historie der Aktivitäten in Ihrem Mandanten, einschließlich Administrator, Endbenutzer und Synchronisierungsaktivität, finden Sie unter möchten können die [Azure Active Directory Access und Verwendungsberichten.](active-directory-view-access-usage-reports.md)

## <a name="navigate-to-the-audit-log"></a>Navigieren Sie zu der Überwachungsprotokoll
Wählen Sie aus dem Dashboard [Azure-Portal](https://portal.azure.com) **Azure AD berechtigten Identitätsmanagement** app aus. Hier Zugriff auf das Überwachungsprotokoll durch Klicken auf **Berechtigte Rollen verwalten** > im Dashboard PIM**Verlauf überwachen** .

## <a name="the-audit-log-graph"></a>Das Audit Log-Diagramm
Sie können das Überwachungsprotokoll verwenden, um die gesamte Vorgänge, max Vorgänge pro Tag und Mittelwert Vorgänge pro Tag in einem Liniendiagramm anzuzeigen.  Sie können auch die Daten nach Rolle filtern, ist es mehr als eine Rolle im Audit protokolliert.

Verwenden Sie die **Uhrzeit**, **Aktion**und **Rolle** Schaltflächen, um das Protokoll zu sortieren.

## <a name="the-audit-log-list"></a>Der Audit Log-Liste
Die Spalten in der Liste der Audit Log sind:

- **Requestor** - der Benutzer, der Rolle Aktivierung oder Änderung angefordert.  Ist der Wert "Azure System", aktivieren Sie das Azure Überwachungsprotokoll für Weitere Informationen.
- **Benutzer** : den Benutzer, der aktivierende oder zu einer Rolle zugewiesen wird.
- **Rolle** – die Rolle zugewiesen oder vom Benutzer aktiviert.
- **Aktion** - die Aktionen aus, indem Sie das jeweilige. Dies kann Zuordnung, Unassignment, Aktivierung oder Deaktivierung umfassen.
- **Zeit** – Wenn die Aktion ausgeführt wurde.
- **Schlussfolgerungen** – Wenn Sie Text in das Feld Grund bei der Aktivierung eingegeben wurde, wird es hier angezeigt.
- **Ablauf** - nur relevant, für die Aktivierung von Rollen.

## <a name="filter-the-audit-log"></a>Filtern des Überwachungsprotokolls

Sie können die Informationen filtern, die in das Überwachungsprotokoll wird durch Klicken auf die Schaltfläche **Filter** .  Das **Update Diagramm Parameter Blade** wird angezeigt.

Nachdem Sie den Filter festlegen möchten, klicken Sie auf **Aktualisieren** , um die Daten in das Protokoll zu filtern.  Wenn die Daten nicht sofort angezeigt werden, aktualisieren Sie die Seite aus.


### <a name="change-the-date-range"></a>Ändern des Datumsbereichs
Verwenden Sie die Schaltflächen **heute**, **Letzte Woche**, **Letzten Monats**oder **Benutzerdefiniert** , so ändern Sie den Zeitbereich des Überwachungsprotokoll.

Wenn Sie auf die Schaltfläche **benutzerdefinierte** auswählen, werden Sie ein Datumsfeld **aus** und ein Datumsfeld **zum** angeben ein Datumsbereichs für das Protokoll angegeben sein.  Sie können entweder die Datumsangaben im Format MM/TT/JJJJ eingeben oder klicken Sie auf **das Kalendersymbol** und wählen Sie das Datum aus einem Kalender aus.

### <a name="change-the-roles-included-in-the-log"></a>Ändern Sie die Rollen in der Log enthalten

Aktivieren Sie oder deaktivieren Sie das Kontrollkästchen **Rolle** neben jeder Rolle einbeziehen oder ausschließen ihn aus dem Protokoll.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

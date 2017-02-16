<properties
   pageTitle="So führen Sie eine Access-überprüfen | Microsoft Azure"
   description="Nachdem Sie eine Access-Überprüfen bei Azure AD berechtigten Identität Verwaltung begonnen haben, erfahren Sie, wie erledigt haben und die Ergebnisse anzeigen"
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

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>So führen Sie eine Access Durchgang Azure AD berechtigten Identitätsmanagement


Rolle Stufe Administratoren können Berechtigungen Zugriff einmal eine [Sicherheit überprüfen gestartet wurde](active-directory-privileged-identity-management-how-to-start-security-review.md)überprüfen. Azure AD-Berechtigungen Identitätsmanagement (PIM) wird automatisch eine Aufforderung Benutzer, um zu überprüfen, deren Zugriff e-Mail-Nachricht senden. Wenn ein Benutzer eine e-Mail-Nachricht nicht erzielen, können Sie diese in [So führen Sie eine Bewertung der Sicherheit](active-directory-privileged-identity-management-how-to-perform-security-review.md)die Anweisungen senden.

Nachdem der Sicherheit überprüfen abgelaufen ist, oder alle Benutzer ihre Self überprüfen abgeschlossen haben, führen Sie die Schritte in diesem Artikel zum Verwalten der überprüfen und die Ergebnisse anzuzeigen.

## <a name="manage-security-reviews"></a>Verwalten der Sicherheit Prüfungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und wählen Sie die Anwendung **Azure AD berechtigten Identitätsmanagement** auf dem Dashboard.
2. Wählen Sie im Abschnitt **Zugriff prüft** des Dashboards an.
3. Wählen Sie die Access-Bewertung, die Sie verwalten möchten.

Klicken Sie auf die Access-Bewertung Detail Blade gibt es eine Zahl Optionen für die Verwaltung von dieser überprüfen.

![PIM-Schaltflächen überprüfen Access - screenshot][1]

### <a name="remind"></a>Erinnern

Wenn eine Access-überprüfen festgelegt ist, damit die Benutzer sich selbst überprüfen, sendet die Schaltfläche **erinnern** eine Benachrichtigung. 

### <a name="stop"></a>Beenden

Alle Access-Prüfungen haben ein Enddatum, aber Sie können die Schaltfläche **Beenden** Sie um früh fertig zu stellen. Wenn alle Benutzer bis zu diesem Zeitpunkt überprüft wurde nicht geschehen ist, können diese können nicht, nachdem Sie die Bewertung beenden. Sie können nicht nach er beendet wurde, wird eine Überprüfung neu starten.

### <a name="apply"></a>Anwenden

Nach Abschluss einer Access-überprüfen, da Sie das Enddatum erreicht oder es manuell beendet implementiert entweder die Schaltfläche **Übernehmen** das Ergebnis der Überprüfung. Wenn bei der Überprüfung des Benutzers der Zugriff verweigert wurde, ist dies der Schritt darin, die ihre rollenzuweisung entfernt werden.  

### <a name="export"></a>Exportieren

Wenn Sie die Ergebnisse der Prüfung Sicherheit manuell anwenden möchten, können Sie die Bewertung exportieren. Die Schaltfläche " **Exportieren** " beginnt eine CSV-Datei herunterladen. Sie können die Ergebnisse in Excel oder andere Programme, die CSV-Dateien öffnen verwalten.

### <a name="delete"></a>Löschen

Wenn Sie nicht die Bewertung weiteren interessiert sind, löschen Sie sie. Die Schaltfläche **Löschen** entfernt die Bewertung aus der PIM-Anwendung.

> [AZURE.IMPORTANT] Sie werden nicht wird eine Warnung angezeigt, bevor Löschung stattfindet, also werden Sie sicher, dass die Überprüfung gelöscht werden soll.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png

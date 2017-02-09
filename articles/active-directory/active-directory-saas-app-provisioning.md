<properties
    pageTitle="Automatisierte SaaS App Benutzer bereitgestellt, die in Azure AD | Microsoft Azure"
    description="Einführung in die Nutzung Azure AD automatisch bereitstellen, heben bereitstellen, und aktualisieren Benutzerkonten kontinuierlich über mehrere Drittanbieter-SaaS Applications."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisieren von Benutzer Bereitstellung und Entfernung SaaS Applications mit Azure-Active Directory

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Was ist die automatisierte Benutzer bereitgestellt für SaaS Apps?

Azure Active Directory (Azure AD) können Sie die Erstellung, Wartung und Entfernen von Benutzeridentitäten in Clientanwendungen Cloud ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)), z. B. Dropbox, Vertrieb, ServiceNow und mehr zu automatisieren.

**Nachfolgend finden Sie einige Beispiele für was dieses Feature Ihnen ermöglicht, gehen Sie wie folgt aus:**

- Erstellen Sie Automatisches neue Konten in der richtigen SaaS apps für neue Benutzer beim Verknüpfen von Ihrem Teams.
- Deaktivieren Sie automatisch Konten aus SaaS apps auf, wenn Personen zwangsläufig das Team verlassen.
- Stellen Sie sicher, dass die Identitäten in Ihrer SaaS apps auf dem neuesten Stand ausgehend von Änderungen im Verzeichnis gehalten werden.
- Bereitstellung von nicht-Benutzer-Objekte, z. B. Gruppen, SaaS-apps, die diese unterstützen.

**Automatisches Benutzer bereitgestellt enthält darüber hinaus folgende Funktionen:**

- Die Möglichkeit, die vorhandene Identitäten zwischen Azure AD- und SaaS apps.
- Anpassungsoptionen zur Azure AD-Hilfe passt die aktuellen Konfigurationen der SaaS apps, die Ihrer Organisation aktuell verwendet wird.
- Optional: e-Mail-Benachrichtigungen für die Bereitstellung von Fehlern.
- Berichterstattung und Aktivität Protokolle für die Überwachung und Problembehandlung helfen.

##<a name="why-use-automated-provisioning"></a>Warum sollten automatisierte bereitgestellt werden?

Einige häufige Gründe für die Verwendung dieses Features gehören:

- Um die Kosten, Effizienz und personenbezogenen Fehler manuelle Bereitstellung Prozesse zugeordnet zu vermeiden.
- So sichern Sie Ihre Organisation sofort Benutzeridentität von Key SaaS apps entfernen, wenn die Organisation verlassen.
- Um eine Massen Anzahl Benutzer einfach in einer bestimmten SaaS-Anwendung zu importieren.
- Nutzen Sie den Vorteil, Ihre Bereitstellung Lösung, die sich die gleichen app-Richtlinien ausführen, die Sie für Azure AD einmaliges Anmelden definiert.

##<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie häufig schreibt Azure AD Verzeichnisses ändert sich in der app SaaS?**

Azure AD überprüft alle fünf bis zehn Minuten für Änderungen aus. Wenn mehrere Fehler (wie in der Groß-/Kleinschreibung von ungültige Administrator-Anmeldeberechtigungen), die app SaaS zurückgibt, dann verlangsamt Azure AD allmählich deren Häufigkeit zu auf einmal täglich, bis der Fehler korrigiert werden.

**Wie lange dauert es meiner Benutzer bereitstellen?**

Ändert beinahe sofort geschehen, wenn Sie die meisten Ihres Verzeichnisses bereitstellen möchten, klicken Sie dann das hängt jedoch die Anzahl der Benutzer und Gruppen, die Sie besitzen. Kleine Verzeichnisse nur ein paar Minuten dauern, mittlerer Größe Verzeichnisse können einige Minuten dauern und sehr große Verzeichnissen können mehrere Stunden dauern.

**Wie kann ich den Status des aktuellen provisioning Auftrags nachverfolgen?**

Sie können das Konto Provisioning-Bericht unter den Abschnitt Berichte von Ihrem Verzeichnis überprüfen. Eine andere Möglichkeit ist dann, besuchen Sie die Registerkarte Dashboard für die SaaS-Anwendung, die Sie zum bereitgestellt werden, und klicken Sie im Abschnitt "Integration Status" am unteren Rand der Seite suchen.

**Woher weiß ich, wenn Benutzer nicht ordnungsgemäß bereitgestellt erhalten?**

Am Ende der Bereitstellung Konfiguration ist es-Assistent eine Option zum Abonnieren von e-Mail-Benachrichtigungen für die Bereitstellung von Fehlern. Sie können auch den Provisioning Fehler Bericht anzeigen, welche Benutzer bereitgestellt werden konnte überprüfen und warum.

**Kann Azure AD Änderungen aus der app SaaS wieder in das Verzeichnis schreiben?**

Für die meisten SaaS-apps ist die Bereitstellung nur ausgehenden, was bedeutet, dass Benutzer aus dem Verzeichnis mit der Anwendung geschrieben werden und nicht Änderungen aus der Anwendung wieder zu dem Verzeichnis geschrieben werden. Für [Arbeitstag](https://msdn.microsoft.com/library/azure/dn762434.aspx)ist jedoch provisioning nur eingehende, d. h., die Benutzer werden in dem Verzeichnis aus Arbeitstag importiert, und ebenso Änderungen im Verzeichnis nicht erhalten geschrieben wieder in Arbeitstag.

**Wie kann ich Feedback an das Entwicklungsteam übermitteln?**

Wenden Sie sich an uns durch das [Azure Active Directory Feedback-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Wie funktioniert der automatisierte Bereitstellung?

Azure AD Vorschriften, die den Benutzer SaaS apps durch Herstellen einer Verbindung zur Bereitstellung von den Lieferanten Anwendung bereitgestellte Endpunkte. Diese Endpunkte ermöglichen Azure AD programmgesteuert erstellen, aktualisieren und Entfernen von Benutzern. Es folgt eine kurze Übersicht über die verschiedenen Schritte, die Azure AD Zeitspanne für den Automatisierung der Bereitstellung von.

1. Wenn Sie die Bereitstellung für eine Anwendung zum ersten Mal aktivieren, werden die folgenden Aktionen ausgeführt:
 - Azure AD versucht, alle vorhandenen Benutzer in der app SaaS mit ihren entsprechenden Identitäten im Verzeichnis entsprechen. Wenn ein Benutzer zugeordnet wird, sind sie *nicht* automatisch für einmaliges Anmelden aktiviert. In der Reihenfolge für einen Benutzer mit der Anwendung zugreifen können müssen sie explizit bei der app in Azure AD direkt oder per Gruppenmitgliedschaft zugewiesen werden.
 - Wenn Sie bereits angegeben haben, welche Benutzer mit der Anwendung zugewiesen werden soll, und Azure Werbeanzeige nicht vorhandene Konten für diesen Benutzer suchen, Bereitstellen Azure AD Konten für sie in der Anwendung.
2. Sobald die ursprüngliche Synchronisierung abgeschlossen wurde, wie zuvor beschrieben wird Azure AD alle 10 Minuten für folgenden Änderungen überprüfen:
 - Wenn Sie neue Benutzer mit der Anwendung zugewiesen wurden (direkt oder durch Gruppenmitgliedschaft), dann wird es sich um ein neues Konto in der app SaaS nach der Bereitstellung.
 - Des Benutzerzugriffs wurde entfernt, und ihr Konto in der app SaaS wird als deaktiviert markiert (Benutzer werden nie vollständig gelöscht, die verhindert, dass Datenverlust im Falle einer fehlerhaften Konfiguration).
 - Wenn ein Benutzer mit der Anwendung zuletzt zugewiesen wurde, und sie bereits ein Konto in der app SaaS hatten, diesem Konto wird als aktiviert gekennzeichnet, und bestimmte Benutzereigenschaften möglicherweise aktualisiert werden, wenn diese veraltet im Vergleich zu Verzeichnis sind.
 - Wenn die Informationen eines Benutzers (z. B. Telefonnummer, Bürostandort usw.) im Verzeichnis geändert wurde, werden diese Informationen auch in der Anwendung SaaS aktualisiert werden.

Weitere Informationen wie die Attribute zwischen Azure AD zugeordnet sind und Ihre SaaS-app, finden Sie im Artikel auf [Attribut Zuordnungen anpassen](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Liste der Apps, die automatisierte Benutzer bereitgestellt unterstützen

Klicken Sie auf einer app zu einer Lernprogramm zum Konfigurieren der automatisierten Bereitstellung dafür finden Sie unter:

- [Feld](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Damit übereinstimmen](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox für Unternehmen](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google-Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Vertrieb](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Vertrieb Sandkastenmodus](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Arbeitstag](http://go.microsoft.com/fwlink/?LinkId=690250) (eingehende bereitgestellt)

Damit eine Anwendung zur Unterstützung von Automatisches Benutzer bereitgestellt muss zuerst die erforderlichen Endpunkte darüber, mit die für externe Programme, um die Erstellung, Wartung und Entfernen von Benutzern zu automatisieren können. Daher sind nicht alle SaaS-apps mit diesem Feature kompatibel. Apps, die dies unterstützen, dem Entwicklungsteam Azure AD-werden dann ein provisioning Verbinders an diesen apps zu erstellen, und diese Arbeit wird durch den Anforderungen der aktuellen oder potenziellen Kunden Priorität zugewiesen.

Um dem Azure AD-Entwicklungsteam zum Anfordern provisioning Unterstützung für zusätzliche Applikationen wenden Sie sich an, senden Sie eine Nachricht über das [Azure Active Directory Feedback-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Anpassen von Attribut Zuordnungen für Benutzer bereitgestellt](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsdefinition Filter für Benutzer bereitgestellt](active-directory-saas-scoping-filters.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory Applications mithilfe von SCIM](active-directory-scim-provisioning.md)
- [Bereitstellung von Benachrichtigungen zu berücksichtigen](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)

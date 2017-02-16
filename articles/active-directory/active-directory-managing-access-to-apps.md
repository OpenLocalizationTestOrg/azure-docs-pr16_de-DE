<properties
  pageTitle="Verwalten des Zugriffs auf apps Azure AD-|  Microsoft Azure"
  description="Beschreibt, wie Azure Active Directory es Organisationen ermöglicht, die apps angeben, die jedem Benutzer zugreifen kann."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Verwalten des Zugriffs auf apps

Laufenden Access Management, Verwendung Auswertung und reporting weiterhin eine Herausforderung sein, nachdem Sie eine app in Ihrer Organisation Identität System integriert ist. In vielen Fällen müssen IT-Administratoren oder Helpdesk eine laufende aktive Rolle in Verwalten des Zugriffs auf Ihre apps zu übernehmen. In manchen Fällen erfolgt die Zuordnung von einem allgemeinen oder Abteilung IT-Team. Häufig die Zuordnung Entscheidung soll an Entscheidungsträger, dass deren Genehmigung vor IT ist delegiert werden die Zuordnung.  Andere Unternehmen investieren in Integration in einem vorhandenen automatisierten Identität und Access-System, wie rollenbasierte Access Control (RBAC) oder Attribut-basierte Access Control (ABAC). Die Integration und die Regel Entwicklung meist spezialisierte und teure werden. Überwachung oder Berichterstattung entweder Managementansatz ist eine eigene separate, Kosten- und komplexe Investition.

## <a name="how-does-azure-active-directory-help"></a>Wie hilft Azure Active Directory?

 Azure AD unterstützt die Verwaltung von umfangreichen Zugriff für Applikationen konfigurierten, UFI-Organisationen, die richtigen Richtlinien von automatischen, Attribut-basierte Zuordnung (ABAC oder RBAC Szenarien) bis hin, über die Delegierung und einschließlich Administrator Management einfach zu erzielen. Mit Azure AD können einfach erreichen komplexe Richtlinien mehrere Management-Modelle für eine einzelne Anwendung kombinieren und erneut können sogar Management Regeln für Webanwendungen mit der gleichen Zielgruppen.

 - [Hinzufügen von neuen oder vorhandenen Applikationen](active-directory-sso-integrate-saas-apps.md)


 Die Anwendung Zuordnung Azure AD-Schwerpunkt auf zwei primäre Zuordnung Modi:

- **Einzelne Zuordnung** Ein IT-Administrator mit Directory globaler Administrator-Berechtigungen kann wählen Sie einzelne Benutzerkonten und gewähren sie Zugriff auf die Anwendung.
- **Gruppen basierende Zuordnung (nur Azure AD bezahlt)** Ein IT-Administrator mit Directory globaler Administrator-Berechtigungen kann eine Gruppe mit der Anwendung zuweisen. Bestimmten Benutzern den Zugriff ergibt, ob sie Mitglied der Gruppe gleichzeitig, die dem Versuch werden, auf die Anwendung zugreifen. Kurzum, kann ein Administrator effektiv eine Regel für die Zuweisung besagt "enthält alle aktuellen Mitglieder der Gruppe zugeordneten Zugriff auf die Anwendung" erstellen. Verwenden diese Option für die Zuordnung, können Administratoren von jedem Azure AD-Gruppenverwaltungsoptionen, einschließlich [Attribut-basierte dynamische Gruppen](active-directory-accessmanagement-manage-groups.md), Gruppen im externen System (beispielsweise lokale Active Directory oder Arbeitstag) oder Administrator verwalteten oder selbst-service-verwalteten Gruppen profitieren. Eine einzelne Gruppe kann einfach zugewiesen werden mehrere apps, sicherstellen, dass die Regeln für die Zuordnung von Applications mit Zuordnung Zugehörigkeit freigegeben werden können die Gesamtkosten für die Verwaltungskomplexität verringern. Bitte beachten Sie die geschachtelte Gruppe für die Zuordnung Gruppe basierend auf Anwendungen zu diesem Zeitpunkt Mitgliedschaften werden nicht unterstützt.

Verwenden diese Zuordnung zwei Modi, können Administratoren alle wünschenswert Zuordnung Managementansatz erzielen.

Mit Azure AD ist Verwendung und Zuordnung Berichte vollständig integriert, und Administratoren einfach Zuordnung Zustand, Zuordnung Fehler und sogar Verwendung Berichten.

## <a name="complex-application-assignment-with-azure-ad"></a>Komplexe Anwendung Zuordnung mit Azure AD-

Erwägen Sie eine Anwendung wie Vertrieb. In vielen Organisationen wird Vertrieb hauptsächlich von der Marketing- und Organisationen verwendet. Häufig haben marketing Teammitglieder hohe Berechtigungen Zugriff auf Vertrieb, während sales Teammitglieder beschränkten Zugriff haben. In vielen Fällen eine umfassende Population der Information Worker haben eingeschränkten Zugriff auf die Anwendung. Ausnahmen für diese Regeln erschweren Belange. Es ist oft die Exekutivagentur der Marketing- oder Führung Teams zu einem Benutzer den Zugriff gewähren, oder ändern ihre Rollen unabhängig von dieser generischen Regeln.

Bei Azure AD können Anwendungen wie Vertrieb vorkonfiguriertes für einmaliges Anmelden (SSO) und automatisierten Bereitstellung sein. Nachdem Sie die Anwendung so konfiguriert ist, kann ein Administrator die einmalige Aktion zu erstellen, und weisen die entsprechenden Gruppen übernehmen. In diesem Beispiel könnte ein Administrator die folgenden Aufgaben ausführen:

- [Dynamische Gruppen](active-directory-accessmanagement-manage-groups.md) kann definiert werden, um alle Mitglieder der Marketing- und Teams mithilfe von Attributen wie Abteilung oder Rolle automatisch darstellen:

    - Die Rolle "marketing" Vertrieb würde alle Mitglieder der marketing Gruppen zugewiesen werden

    - Alle Mitglieder von Vertrieb, die die Rolle "sales" Vertrieb Gruppen zugewiesen werden sollte. Eine weitere Verfeinerung konnte mehrere Gruppen zu verwenden, die regionalen sales Teams verschiedenen Vertrieb Rollen zugewiesen sind darstellen.

- Klicken Sie zum Aktivieren des Ausnahmeverfahren konnte eine Self-service-Gruppe für jede Rolle erstellt werden. Beispielsweise kann die Gruppe "Vertrieb marketing Ausnahme" als Gruppe Self-service erstellt werden. Vertrieb marketing Rolle kann die Gruppe zugewiesen werden, und das marketing Führung Team Besitzer vorgenommen werden kann. Marketing Führung Teammitglieder konnte hinzufügen oder Entfernen von Benutzern, festlegen eine Richtlinie Verknüpfung oder sogar genehmigen oder verweigern einzelner Benutzer Anfragen zur Teilnahme an. Dies wird durch eine entsprechende Informationen Worker-Benutzeroberfläche unterstützt, die keine spezielle Schulung für Besitzer oder Mitglieder erforderlich ist.

In diesem Fall würde alle zugeordneten Benutzer automatisch auf Vertrieb, bereitgestellt werden, zu anderen Gruppen hinzugefügt werden, deren rollenzuweisung in Vertrieb aktualisiert werden sollte. Benutzer wäre ermitteln und Vertrieb zugreifen, durch die Anwendung Access Systemsteuerung Office Web-Clients, oder sogar, indem Sie zu ihrer Organisation Vertrieb Anmeldeseite navigieren können. Administratoren möchten einfach Verwendung und zur Zuordnung Status mit Azure AD-Berichten angezeigt werden sollen.

Administratoren können [bedingte Azure AD-Zugriff](active-directory-conditional-access.md) auf Access-Richtlinien für bestimmte Rollen festlegen einsetzen. Diese Richtlinien können berücksichtigt werden, ob außerhalb der unternehmensumgebung und sogar kombinierte Authentifizierung oder Gerät Anforderungen Access in verschiedenen Fällen erzielen Zugriff zulässig ist.

## <a name="how-can-i-get-started"></a>Wie gehe ich vor erhalten?

Zunächst, wenn Sie nicht bereits Azure AD nutzen und ein IT-Administrator sind:

 - [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/) -können für eine kostenlose 30-Tage-Testversion heute registrieren und Ihre erste Cloudlösung in weniger als 5 Minuten mit diesem Link bereitstellen

Azure AD-Features, die gemeinsame Nutzung von Konten aktivieren umfassen:

- [Gruppenzuordnung](active-directory-accessmanagement-self-service-group-management.md)
- Hinzufügen von Applications zu Azure AD
- Erste Schritte mit Zuordnung
- Anwendung Zuordnung häufig gestellte Fragen
- [App Dashboard/Verwendungsberichte](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Wo kann ich mehr erfahren?

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Schützen von apps mit bedingten Zugriff](active-directory-conditional-access.md)
- [Self-service-Gruppe Management/SSAA](active-directory-accessmanagement-self-service-group-management.md)

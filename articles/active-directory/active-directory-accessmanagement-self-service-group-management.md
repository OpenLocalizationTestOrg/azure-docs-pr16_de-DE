<properties
    pageTitle="Einrichten von Azure Active Directory für Self-Service-Anwendung Access Management | Microsoft Azure"
    description="Die Self-service-Verwaltungskonsole ermöglicht Benutzern, erstellen und Verwalten von Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory und bietet Benutzern die Möglichkeit, die Anfrage Sicherheitsgruppe oder Office 365-Gruppenmitgliedschaft"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Einrichten von Azure Active Directory für die Self-service-Verwaltungskonsole

Die Self-service-Verwaltungskonsole kann Benutzer erstellen und Verwalten von Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory (Azure AD). Benutzer können auch Sicherheitsgruppe oder Office 365-Gruppenmitgliedschaft anfordern, und klicken Sie dann der Besitzer der Gruppe genehmigen oder ablehnen Mitgliedschaft kann. Auf diese Weise kann täglichen Kontrolle über Gruppenmitgliedschaft an Personen delegiert werden, die für die Mitgliedschaft im Business-Kontext zu verstehen. Self-service-Gruppe Verwaltungsfunktionen stehen nur für Sicherheitsgruppen und Office 365-Gruppen, aber nicht für e-Mail-aktivierten Sicherheitsgruppen oder Verteilerlisten.

Die Self-service-Verwaltungskonsole aktuell umfasst zwei grundlegende Szenarios: delegierte Verwaltung von Gruppen und die Self-service-Verwaltungskonsole.

- **Delegierte Verwaltung von Gruppen** 
   Beispiel ist ein Administrator, der Zugriff auf eine Anwendung SaaS verwaltet, auf denen das Unternehmen verwendet wird. Verwalten von Berechtigungen wird, damit dieser Administrator gefragt, den zum Erstellen einer neuen Gruppe Besitzer eines Unternehmens werden unübersichtlich, immer. Der Administrator weist Access für die Anwendung in die neue Gruppe und alle Personen, die bereits mit der Anwendung Zugriff auf zur Gruppe hinzugefügt. Der Besitzer eines Unternehmens kann dann weitere Benutzer hinzufügen, und diese Benutzer werden automatisch mit der Anwendung bereitgestellt. Der Besitzer eines Unternehmens muss nicht warten, bis der Administrator zum Verwalten des Zugriffs für Benutzer. Wenn der Administrator an einem Vorgesetzten in einer Unternehmensgruppe von anderen dieselbe Berechtigung erteilt, kann diese Person auch eigene Benutzern der Zugriff verwalten. Weder der Besitzer eines Unternehmens noch die Manager können Sie anzeigen oder Verwalten von Benutzern gegenseitig. Alle Benutzer anzuzeigen mit Zugriff auf die Anwendung und blockieren Zugriffsrechte bei Bedarf der Administrator weiterhin.

- **Die Self-service-Verwaltungskonsole** 
   ist ein Beispiel für dieses Szenario zwei Benutzer mit den beiden SharePoint Online-Websites, die unabhängig voneinander eingerichtet. Diese möchten gegenseitig Teams Zugriff auf ihre Websites gewähren. Um dies zu erreichen, erstellen sie eine Gruppe in Azure Active Directory und in SharePoint Online markiert jede von ihnen dieser Gruppe für den Zugriff auf ihre Websites. Wenn jemand Access sie über die Systemsteuerung Access anfordern, und nach der Genehmigung sie Zugriff auf beide SharePoint Online-Websites automatisch. Eine von ihnen beschließt später, dass alle Personen, die Zugriff auf die Website auch Zugriff auf eine bestimmte SaaS Anwendung beziehen soll. Der Administrator der Anwendung SaaS kann Zugriffsrechte für die Anwendung zu der SharePoint Online-Website hinzufügen. Danach alle Besprechungsanfragen, die genehmigt abrufen erhalten Zugriff auf die beiden SharePoint Online-Websites und auch diese Anwendung SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Verfügbarmachen einer Gruppe für Endbenutzer Self-service

1. Öffnen Sie im [Azure klassischen Portal](https://manage.windowsazure.com)Ihrer Azure AD-Verzeichnis aus.

2. Setzen Sie auf der Registerkarte **Konfigurieren** **delegierte Verwaltung** auf aktiviert.

3. Setzen Sie die **Benutzer können Sicherheitsgruppen** oder **Benutzer können Office Gruppen erstellen** auf aktiviert.

Wenn **Benutzer Sicherheitsgruppen erstellen können** aktiviert ist, werden alle Benutzer in Ihrem Verzeichnis zum Erstellen neuer Sicherheitsgruppen und Hinzufügen von Mitgliedern zu diesen Gruppen zulässig. Diese neuen Gruppen würde auch im Bereich Zugriff für alle anderen Benutzer angezeigt. Wenn die Einstellung auf die Gruppe dies zulässt, können andere Benutzer Anfragen zur Teilnahme an diesen Gruppen erstellen. Wenn **Benutzer können Sicherheitsgruppen erstellen** deaktiviert ist, werden Benutzer Gruppen können nicht erstellt werden und vorhandene Gruppen, für die sie einen Besitzer sind, nicht ändern. Sie können jedoch weiterhin die Mitgliedschaften dieser Gruppen verwalten und Anfragen von anderen Benutzern zur Teilnahme an ihren Gruppen genehmigen.

**Wer Self-Service für Sicherheitsgruppen verwenden, können Benutzer** können Sie auch um eine weitere abgestimmte Access Steuerung der Gruppe Self-service-Verwaltung für Ihre Benutzer zu erzielen. Wenn **Benutzer Gruppen erstellen können** aktiviert ist, werden alle Benutzer in Ihrem Verzeichnis zum Erstellen neuer Gruppen und Hinzufügen von Mitgliedern zu diesen Gruppen zulässig. Indem Sie auf einige **Benutzer, wer Self-Service für Sicherheitsgruppen verwenden kann,** auch festlegen, werden Sie beschränken Management nur eine eingeschränkte Gruppe von Benutzern zu gruppieren. Wenn dieser Schalter zu einigen festgelegt ist, müssen Sie in der Gruppe SSGMSecurityGroupsUsers Benutzer hinzufügen, bevor sie neue Gruppen erstellen und Hinzufügen von Mitgliedern zu können. **Wer Self-Service für Sicherheitsgruppen verwenden, können Benutzer** auf alle festlegen, aktivieren Sie alle Benutzer in Ihrem Verzeichnis zum Erstellen neuer Gruppen an.

Das Feld **Gruppe, die Self-Service für Sicherheitsgruppen verwenden können,** können Sie auch einen benutzerdefinierten Namen für die Gruppe angeben, deren Mitglieder Self-Service verwenden können.

## <a name="additional-information"></a>Weitere Informationen

Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets für die Konfiguration von gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

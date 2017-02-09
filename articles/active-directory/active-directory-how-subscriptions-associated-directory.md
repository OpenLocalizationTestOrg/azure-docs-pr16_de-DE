<properties
    pageTitle="Wie Azure-Abonnements Azure Active Directory zugeordnet sind | Microsoft Azure"
    description="Anmelden bei Microsoft Azure und Verwandte Probleme, wie beispielsweise die Beziehung zwischen einer Azure-Abonnement und Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="curtand"/>

# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Wie Azure-Abonnements Azure Active Directory zugeordnet sind

Dieses Thema enthält Informationen zum Anmelden bei Microsoft Azure und Verwandte Probleme, wie beispielsweise die Beziehung zwischen einer Azure-Abonnement und Azure Active Directory (Azure AD).

## <a name="accounts-that-you-can-use-to-sign-in"></a>Konten, mit denen Sie anmelden
Beginnen wir mit den Konten, die Sie verwenden können, anmelden. Es gibt zwei Arten: ein Microsoft-Konto (früher als Microsoft Live ID bezeichnet) und ein Konto geschäftlichen oder schulnotizbücher, die ein Konto in Azure AD gespeichert ist.

 Microsoft-Konto  | Azure AD-Konto
    ------------- | -------------
Das Ausführen von Microsoft Consumer Identität-system | Das Ausführen von Microsoft Business Identität-system
Authentifizierung für Dienste, die Consumer Prozessorientierung, wie Hotmail und MSN sind | Authentifizierung auf Dienste, die Business-orientierte, wie Office 365 sind
Nutzer Erstellen ihrer eigenen Microsoft-Konten, beispielsweise beim Registrieren für e-Mail | Unternehmen und Organisationen erstellen und Verwalten ihrer eigenen geschäftlichen oder schulnotizbücher-Konten
Identitäten sind erstellt und im Microsoft-Konto System gespeichert | Identitäten werden mit Azure oder ein anderes erstellt-service wie Office 365 und diese in einer Azure AD-Instanz der Organisation zugewiesene gespeichert sind

Auch Azure ursprünglich nur von Benutzern von Microsoft-Konto zugreifen, wenn können sie jetzt Zugriff durch Benutzer *sowohl* Systeme. Dies erfolgte, dass alle Azure Eigenschaften Trust Azure AD-für die Authentifizierung, Probleme Azure AD Organisations-Benutzer authentifiziert und durch Erstellen einer Beziehung Föderation, in dem Microsoft-Konto Consumer Identität System zum Authentifizieren von Benutzern Consumer vertraut Azure AD. Daher ist Azure AD Microsoft-Konten "Gast" sowie "systemeigener" Azure AD-Konten authentifizieren können.

Beispielsweise meldet hier ein Benutzers mit einem Microsoft-Konto bei klassischen Azure-Portal.

> [AZURE.NOTE]
> Azure klassischen-Portal anmelden msmith@hotmail.com müssen Sie ein Abonnement von Azure. Das Konto muss entweder ein Dienst oder Co-Administratoren des Abonnements.

![][1]

Da diese Adresse Hotmail ein Consumer Konto ist, wird der Anmeldung von Microsoft-Konto Consumer Identität System authentifiziert. Azure AD-Identität System gibt vertraut die Authentifizierung vom Microsoft-Konto System durchgeführt und ein Token Azure Services zugreifen kann.

## <a name="how-an-azure-subscription-is-related-to-azure-ad"></a>Beziehung zwischen einem Azure-Abonnement und Azure AD

Jede Azure-Abonnement verfügt über eine Vertrauensstellung mit einer Azure AD-Instanz. Dies bedeutet, dass sie dieses Verzeichnis um Benutzern Dienste und Geräte authentifizieren vertraut. Mehrere Abonnements können das gleiche Verzeichnis vertrauen, aber ein Abonnement vertraut nur ein Verzeichnis. Sie können sehen, welches Verzeichnis Ihr Abonnement unter der Registerkarte Einstellungen vertrauenswürdig ist. Sie können [die Abonnement Einstellungen bearbeiten](active-directory-understanding-resource-access.md) , um welches Verzeichnis ändern ihr vertraut.

Diese Vertrauensstellung, die ein Abonnement für ein Verzeichnis weist unterscheidet sich von der Beziehung, die ein Abonnement für alle anderen Ressourcen in Azure (Websites, Datenbanken usw.), enthält die untergeordneten Ressourcen eines Abonnements eher sind. Wenn ein Abonnement abläuft, reagiert Zugriff auf diese anderen Ressourcen, die mit dem Abonnement verknüpft ist auch. Aber Verzeichnis in Azure bleibt, und Sie können ein weiteres Abonnement dieses Verzeichnis zuordnen und fahren Sie mit der Directory Benutzer verwalten.

Die Azure AD-Erweiterung, die Sie im Rahmen Ihres Abonnements finden Sie unter funktioniert nicht auf ähnliche Weise wie die anderen Erweiterungen im klassischen Azure-Portal. Andere Erweiterungen im klassischen Azure-Portal sind auf das Abonnement Azure beschränkt. Was Sie in der Azure AD-Erweiterung finden Sie unter ändern sich nicht basierend auf Abonnement – es zeigt nur Verzeichnisse basierend auf dem Benutzer angemeldet.

Alle Benutzer verfügen über ein einzelnes Start Verzeichnis, das sie authentifiziert, aber sie können auch in anderen Verzeichnissen Gäste werden. In die Erweiterung Azure AD-wird jedes Verzeichnis angezeigt, die Ihr Benutzerkonto Mitglied ist. Beliebiges Verzeichnis, die Ihr Konto kein Mitglied ist, wird nicht angezeigt. Ein Verzeichnis kann ausgeben Token für die Arbeit oder Schule Konten für Microsoft Azure AD- oder Konto Benutzer (da Azure AD mit dem Microsoft-Konto-System verbunden ist).

Dieses Diagramm zeigt ein Abonnement für Michael Smith, nachdem er mithilfe eines Kontos Arbeit für Contoso angemeldet haben.

![][2]

## <a name="how-to-manage-a-subscription-and-a-directory"></a>So verwalten Sie ein Abonnement und ein Verzeichnis
Die Administratorrollen für ein Abonnement Azure verwalten Ressourcen, die mit dem Azure-Abonnement verknüpft. Diese Funktionen und die optimalen Methoden zum Verwalten Ihrer Abonnements fallen am [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Standardmäßig werden die Dienstadministrator Rolle zugewiesen werden, wenn Sie sich haben registriert. Wenn andere Benutzer anmelden und den Zugriff auf Dienste mithilfe des gleichen Abonnements müssen, können Sie diese als Co-Administratoren hinzufügen. Die Dienstadministrator und Co-Administratoren können entweder Microsoft Konten oder Arbeit oder Schule Konten aus dem Verzeichnis aus, dem mit dem Azure-Abonnement verknüpft ist.

Azure AD weist einen anderen Satz von Administratorrollen im Verzeichnis und Identität miteinander verwandt verwaltet. Globale Administrator eines Verzeichnisses kann beispielsweise Hinzufügen von Benutzern und Gruppen im Verzeichnis oder durch die kombinierte Authentifizierung für Benutzer. Ein Verzeichnis erstellt ein Benutzer die Rolle des globalen Administrators zugeordnet ist, und können sie Administratorrollen zuweisen, für andere Benutzer.

Wie bei Abonnement-Administratoren können die Administratorrollen Azure AD-werden von Microsoft-Konten oder die Arbeit oder Schule Konten. Azure AD-Administratorrollen werden auch von anderen Diensten wie Office 365 und Microsoft Intune verwendet. Weitere Informationen finden Sie unter [Zuweisen von Administratorrollen](active-directory-assign-admin-roles.md).

Aber so wichtig ist, die Administratoren Azure-Abonnement und Azure AD-Directory Administratoren sind zwei separaten Konzepte. Azure-Abonnement-Administratoren können Ressourcen in Azure verwalten und in der klassischen Azure-Portal (da das Azure klassische Portal eine Azure Ressource ist) die Active Directory-Erweiterung anzeigen. Directory-Administratoren können Eigenschaften im Verzeichnis verwalten.

Eine Person in beiden Rollen werden kann, aber dies ist nicht erforderlich. Ein Benutzer kann der globalen Administratorrolle Verzeichnis zugewiesen werden, aber nicht als Dienstadministrator oder gemeinsame Administrator eines Azure-Abonnements zugewiesen werden. Ohne ein Administrator des Abonnements zu verwenden, kann diese Benutzer klassischen Azure-Portal anmelden. Aber der Benutzer konnte Directory administrativen Aufgaben, die mit anderen Tools wie Azure AD PowerShell oder Office 365 Admin Center ausführen.

## <a name="why-cant-i-manage-the-directory-with-my-current-user-account"></a>Verwalten ich kann nicht warum Verzeichnis mit meiner aktuellen Benutzerkonto?

Manchmal kann ein Benutzer versuchen, melden Sie sich bei der Azure klassischen Portal mithilfe einer geschäftlichen oder Schule Konto vor für ein Abonnement Azure anmelden. In diesem Fall wird der Benutzer eine Meldung, dass kein für dieses Konto Abonnement. Die Nachricht wird eine Verknüpfung zum Starten eines kostenlosen Testabonnement enthalten.

Nach der Anmeldung für das kostenlose Testversion dem Benutzer für die Organisation in den Azure klassischen Portals, jedoch werden verwalten (die in der Lage ist, um Benutzer hinzufügen oder Bearbeiten eines vorhandenen Benutzereigenschaften) kann nicht Verzeichnis angezeigt wird, weil der Benutzer nicht über ein Verzeichnis globaler Administrator ist. Das Abonnement ermöglicht dem Benutzer das Azure klassische Portal verwenden und finden Sie unter der Azure-Active Directory-Erweiterung, jedoch die zusätzlichen Berechtigungen eines globalen Administrators für die Verwaltung der Directory benötigt werden.

## <a name="using-your-work-or-school-account-to-manage-an-azure-subscription-that-was-created-by-using-a-microsoft-account"></a>Ihr Konto geschäftlichen oder schulnotizbücher verwenden, um eine Azure-Abonnement zu verwalten, die mithilfe von Microsoft-Konto erstellt wurde

Als bewährte Methode [Melden Sie sich bei Azure als einer Organisation](sign-up-organization.md) und verwenden Sie eine Arbeit oder Schule Konto zum Verwalten von Ressourcen in Azure. Geschäftlichen oder schulnotizbücher Konten sind bevorzugte, da diese können zentral von der Organisation, die sie ausgestellt verwaltet werden, sie weitere Features, die als Microsoft-Konten weisen, und sie direkt von Azure Active Directory authentifiziert sind. Das gleiche Konto ermöglicht den Zugriff auf andere Microsoft-Onlinedienste die Unternehmen und Organisationen, wie Office 365 oder Microsoft Intune zur Verfügung stehen. Wenn Sie bereits über ein Konto, die Sie mit diesen andere Eigenschaften verwenden verfügen, möchten Sie wahrscheinlich Konto mit Azure verwenden. Sie haben auch noch eine Active Directory-Instanz Sichern von diesen Eigenschaften, die Ihre Azure-Abonnement vertrauen sollen.

Geschäftlichen oder schulnotizbücher Konten können auch auf mehrere Arten als ein Microsoft-Konto verwaltet werden. Beispielsweise kann ein Administrator Zurücksetzen des Kennworts eines einer einer Arbeit oder Schule Konto oder durch die kombinierte Authentifizierung dafür.

In einigen Fällen sollten Sie einen Benutzer in Ihrer Organisation kann zum Verwalten von Ressourcen, die mit einem Azure-Abonnement für Consumer Microsoft-Konto verknüpft sind. Weitere Informationen zum Übergang zu andere Konten verwalten Abonnements oder Verzeichnisse haben finden Sie unter [Verwalten Verzeichnis für Ihr Office 365-Abonnement in Azure](#manage-the-directory-for-your-office-365-subscription-in-azure).


## <a name="signing-in-when-you-used-your-work-email-for-your-microsoft-account"></a>Wenn Sie Ihre Arbeit e-Mail-Adresse für Ihr Microsoft-Konto verwendet haben, sich anzumelden

Wenn bei einem bestimmten Zeitpunkt in der Vergangenheit einen Microsoft-Konto mithilfe Ihrer e-Mail Arbeit als eine Benutzer-ID-Consumer erstellt haben, wird möglicherweise eine Seite mit der Frage, wählen Sie entweder das System Microsoft Azure-Konto oder das Microsoft Account System angezeigt.

![][3]

Sie müssen Benutzerkonten mit demselben Namen, eine in Azure Active Directory und anderen im Consumer Microsoft-Konto System. Sie sollten das Konto auswählen, das mit dem Azure-Abonnement verknüpft ist, die Sie verwenden möchten. Wenn eine angezeigt, die besagt, dass ein Abonnement für diesen Benutzer nicht vorhanden ist Fehlermeldung, die Option Sie wahrscheinlich nur geklappt. Melden Sie sich ab, und versuchen Sie es erneut. Melden Sie sich für Weitere Informationen zu Fehlern, die verhindern können, finden Sie unter [Problembehandlung von Fehlern bei "Wir konnten nicht mit Ihrem Konto verbunden sind keine-Abonnements finden"](https://social.msdn.microsoft.com/Forums/en-US/f952f398-f700-41a1-8729-be49599dd7e2/troubleshooting-we-were-unable-to-find-any-subscriptions-associated-with-your-account-errors-in?forum=windowsazuremanagement).

## <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Verwalten Sie Verzeichnis für Ihr Office 365-Abonnement in Azure

Angenommen, Sie für Office 365 angemeldet haben, bevor Sie sich für Azure registrieren. Nun möchten Sie das Verzeichnis für die Office 365-Abonnement im klassischen Azure-Portal zu verwalten. Es gibt zwei Möglichkeiten zur Verfügung, je nachdem, ob Sie sich bei angemeldet haben Azure oder Sie müssen nicht.

### <a name="i-do-not-have-a-subscription-for-azure"></a>Ich habe ein Abonnement für Azure keinen

In diesem Fall nur [Anmelden für Azure](sign-up-organization.md) mit dem gleichen Arbeit oder Schule-Konto, das Sie zum Anmelden bei Office 365 verwenden. Relevante Informationen aus der Office 365-Konto wird in der Azure Anmeldeformular bereits ausgefüllt werden. Ihr Konto wird der Dienst Administratorrolle des Abonnements zugewiesen.  

### <a name="i-do-have-a-subscription-for-azure-using-my-microsoft-account"></a>Ich habe ein Abonnement für Azure mit meinem Microsoft-Konto

Wenn Sie für Office 365 mit einem geschäftlichen oder schulnotizbücher-Konto registriert und klicken Sie dann für Azure mit einem Microsoft-Konto angemeldet haben, stehen Ihnen zwei Verzeichnisse: eine für Ihre Arbeit oder Schule und ein Standardverzeichnis, das erstellt wurde, wenn Sie für Azure haben angemeldet.

Um beide Verzeichnisse im klassischen Azure-Portal zu verwalten, führen Sie folgende Schritte aus.

> [AZURE.NOTE]
> Diese Schritte können nur ausgeführt werden, während ein Benutzer mit einer Microsoft-Konto angemeldet ist. Wenn der Benutzer mit einem geschäftlichen oder schulnotizbücher-Konto angemeldet ist, die Option **vorhandene Verzeichnis verwenden** ist nicht verfügbar, da ein Konto geschäftlichen oder schulnotizbücher nur von home-Verzeichnis authentifiziert werden kann (d. h., Verzeichnis, in dem das Konto geschäftlichen oder schulnotizbücher gespeichert ist, und der von der Arbeit oder Schule Eigentum).

1. Melden Sie sich mit Ihrem Microsoft-Konto Azure klassischen Portal aus.

2. Klicken Sie auf **neu** > **App Services** > **Active Directory** > **Verzeichnis** > **Benutzerdefinierte erstellen**.

3. Klicken Sie auf **vorhandene Verzeichnis verwenden** und überprüfen Sie **ich möchte nun abgemeldet** , und klicken Sie auf das Häkchen, um die Aktion abzuschließen.

4. Melden Sie sich Azure klassischen-Portal mit einem Konto an, die globale Administratorrechte für die geschäftlichen oder schulnotizbücher Verzeichnis enthält.

5. Wenn Sie dazu aufgefordert werden **Verzeichnis Contoso mit Azure verwenden?**, und klicken Sie auf **Weiter**.

6. Klicken Sie auf **jetzt abmelden**.

7. Melden Sie sich wieder einzuchecken Azure klassischen-Portal mit Ihrem Microsoft-Konto. Beide Verzeichnisse werden in der Active Directory-Erweiterung angezeigt.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Ändern von Administratoren für eine Azure Abonnement Informationen Sie [zum Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md)

- Weitere Informationen, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zu Ressourcen Access in Azure](active-directory-understanding-resource-access.md)

- Weitere Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)

- [Registrieren Sie sich bei Azure als einer Organisation](sign-up-organization.md)


<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

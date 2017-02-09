<properties
    pageTitle="Grundlegendes zum Zugriff auf Ressourcen in Azure | Microsoft Azure" 
    description="In diesem Thema wird zur Verwendung von Abonnement-Administratoren zum Steuern des Ressourcenzugriffs im vollständigen Azure-Portal Konzepte erläutert."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="understanding-resource-access-in-azure"></a>Grundlegendes zum Zugriff auf Ressourcen in Azure


> [AZURE.NOTE] In diesem Thema wird zur Verwendung von Abonnement-Administratoren zum Steuern des Ressourcenzugriffs im vollständigen Azure-Portal Konzepte erläutert. Als Alternative bietet im Portal Azure Vorschau [rollenbasierte Access Steuerelement](role-based-access-control-configure.md) , damit Azure Ressourcen genauer verwaltet werden können.

In Oktober 2013 wurden die Azure klassischen Portal und Service Management-APIs Azure Active Directory integriert akzeptieren, um die Grundlagen zum Verbessern der Benutzerfunktionalität für das Verwalten des Zugriffs auf Azure Ressourcen zu verschaffen. Azure-Active Directory bietet bereits großartige Funktionen wie Benutzermanagement, lokalen Verzeichnis synchronisieren, mehrstufige Authentifizierung und Anwendung Access-Steuerelement. Natürlich ist soll diese auch für die Verwaltung von Azure Ressourcen verfügbar Erhöhungen erfolgen.

Access-Steuerelement in Azure startet im Hinblick auf Abrechnung. Besitzer ein Azure-Konto zugegriffen werden, indem Sie im [Center für Azure-Konten](https://account.windowsazure.com/subscriptions)handelt es sich um das Konto Administrator (AA). Abonnements sind Container für Abrechnung, aber sie auch dienen als Sicherheitsgrenze: jedes Abonnement hat einen Dienst Administrator (SA), wer hinzufügen, entfernen und Azure Ressourcen in dieser Abonnement mithilfe der [Azure klassischen Portal](https://manage.windowsazure.com/)ändern können. Die Standard-SA von einem neuen Abonnement ist die AA, aber AA die SA in der Mitte der Azure-Konten ändern kann.

<br><br>![Azure-Konten][1]

Abonnements können auch keine Verknüpfung mit einem Verzeichnis. Das Verzeichnis definiert eine Reihe von Benutzern. Diese können Benutzer von der Arbeit oder Schule, das Verzeichnis erstellt werden, oder sie können externe Benutzer (d. h., Microsoft Accounts) sein. Abonnements sind durch eine Teilmenge von Benutzern als Dienst-Administrator (SA) oder gemeinsame Administrator (CA) zugewiesen wurden Verzeichnis zugegriffen werden. die einzige Ausnahme ist, dass Vorversionen Microsoft Accounts (vormals Windows Live ID) als SA CA zugeordnet werden kann, ohne sich im Verzeichnis.

<br><br>![Access Control in Azure][2]


Funktionen in der klassischen Azure-Portal ermöglicht SAs, die mit einer Microsoft Account Verzeichnis ändern, das ein Abonnement zugeordnet ist, mithilfe des Befehls **Directory bearbeiten** klicken Sie auf der Seite **Abonnements** in den **Einstellungen**angemeldet sind. Beachten Sie, dass dieser Vorgang für das Steuerelement Access diesen Abonnementtyp Auswirkungen hat.



> [AZURE.NOTE] Der Befehl **Bearbeiten Verzeichnis** im klassischen Azure-Portal ist nicht verfügbar für Benutzer, die angemeldet sind, sich mit einem Arbeit oder Schule zu berücksichtigen, da diese Konten anmelden können, nur bei Verzeichnis, denen sie angehören.

<br><br>![Einfache Benutzer Login Fluss][3]

Im einfachen Fall wird eine Organisation (beispielsweise "Contoso") erzwingen Abrechnung und Kontrolle über den gleichen Satz von Abonnements zugreifen. D. h. ist, das Verzeichnis mit Abonnements, die ein einzelnes Azure-Konto besitzt verknüpft. Bei erfolgreicher Anmeldung klassischen Azure-Portal finden Sie unter Benutzer zwei Auflistungen Ressourcen (in Orange in der vorherigen Abbildung dargestellt):


- Verzeichnisse durchsuchen, wo ihr Konto vorhanden ist (die Quelle ist oder als einen Fremdschlüssel Tilgungsanteile hinzugefügt). Beachten Sie, dass das Verzeichnis für die Anmeldung verwendet, damit Ihre Verzeichnisse immer angezeigt werden sollen, unabhängig davon, wo Sie angemeldet ist nicht für diese Berechnung, relevant.

- Ressourcen, die Teil des Abonnements, die mit dem Verzeichnis für Login verwendete verknüpft sind und die Benutzer kann zugreifen (Stelle, an der sie eine SA oder CA sind).


<br><br>![Benutzer mit mehreren Abonnements und Verzeichnisse durchsuchen][4]


Benutzer mit Abonnements über mehrere Verzeichnisse haben die Möglichkeit, die im aktuellen Kontext des Portals Azure klassischen mithilfe der Abonnementfilters wechseln. Im Hintergrund wird dadurch eine separate Anmeldung in ein anderes Verzeichnis, aber dies geschieht nahtlos mit einmaliges Anmelden (SSO).

JOIN-Operationen wie das Verschieben von Ressourcen zwischen Abonnements können schwieriger als Ergebnis dieser einzelnen Verzeichnisansicht des Abonnements sein. Um die Übertragung einer Ressource ausführen zu können, kann es erforderlichen ersten Verwenden des Befehls **Bearbeiten Directory** auf der Seite Abonnements in den **Einstellungen** zum Zuordnen der Abonnements in das gleiche Verzeichnis sein.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Ändern von Administratoren für eine Azure Abonnement Informationen Sie [zum Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md)

- Weitere Informationen zur Beziehung von Azure Active Directory zu Ihrem Abonnement Azure finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](active-directory-how-subscriptions-associated-directory.md)

- Weitere Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)



<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png

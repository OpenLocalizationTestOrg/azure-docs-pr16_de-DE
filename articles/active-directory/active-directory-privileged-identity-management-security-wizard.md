<properties
   pageTitle="Der Azure AD berechtigten Identitätsmanagement Datensicherheits-Assistent"
   description="Zum ersten Mal, das Sie die Erweiterung Azure Active Directory berechtigten Identitätsmanagement verwenden wird durch einen Assistenten angezeigt. In diesem Artikel werden die Schritte zum Verwenden des Assistenten."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Der Azure AD berechtigten Identitätsmanagement Datensicherheits-Assistent

Wenn Sie die erste Person in Ihrer Organisation Azure berechtigten Identität Management (PIM) ausgeführt haben, wird ein Assistent angezeigt. Der Assistent zu verstehen, das Sicherheitsrisiko berechtigten Identitäten und wie Sie PIM, um diese Risiken zu verringern. Sie brauchen Änderungen an den vorhandenen rollenzuweisungen im Assistenten vornehmen, wenn Sie es vorziehen, die später erledigen.

## <a name="what-to-expect"></a>Was Sie erwartet

Vor dem Starten Ihrer Organisation PIM verwenden, sind alle rollenzuweisungen dauerhaft: Benutzer sind immer in diesen Rollen, selbst wenn sie derzeit nicht über ihre Rechte benötigen.  Im ersten Schritt des Assistenten wird eine Liste aller Rollen hoher Berechtigung und wie viele Benutzer sind derzeit in diesen Rollen. Sie können einen Drilldown ausführen, zu einer bestimmten Rolle aus, um weitere Informationen zu Benutzern erfahren Sie, wenn eine oder mehrere dieser Formate nicht vertraut sind.

Im zweite Schritt des Assistenten bietet Ihnen die Möglichkeit zum Ändern des Administrators rollenzuweisungen.  

> [AZURE.WARNING]Es ist wichtig, dass Sie mindestens ein globaler Administrator und mehr als eine Rolle Stufe Administrator mit einem organisationskonto (kein Microsoft-Konto). Ist nur eine Rolle Stufe Administrator, wird die Organisation werden nicht PIM verwaltet werden, wenn die Firma gelöscht wird.
> Darüber hinaus behalten Sie rollenzuweisungen permanent, wenn ein Benutzer verfügt über ein Microsoft-Konto (ein Konto, mit denen sie Anmeldung bei Microsoft-Diensten wie Skype und Outlook.com bei). Wenn Sie beabsichtigen, für die Aktivierung für diese Rolle MFA erforderlich ist, wird dieser Benutzer gesperrt.


Nachdem Sie Änderungen vorgenommen haben, wird der Assistent nicht mehr angezeigt. Das nächste Mal von einem Administrator der Rolle Stufe verwenden PIM, sehen Sie das PIM Dashboard.  

- Wenn Sie hinzufügen oder Entfernen von Benutzern aus Rollen oder Zuordnungen permanent sein, um berechtigt, Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen der Rolle des Benutzers](active-directory-privileged-identity-management-how-to-add-role-to-user.md)ändern möchten.
- Wenn Sie mehr Benutzern Zugriff zum Verwalten von PIM gewähren möchten, lesen Sie weitere an, [wie Sie Zugriff gewähren möchten, klicken Sie in PIM verwalten](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

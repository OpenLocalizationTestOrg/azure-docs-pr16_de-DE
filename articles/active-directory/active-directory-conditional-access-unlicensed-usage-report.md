<properties
    pageTitle="Nicht lizenzierte Verwendungsbericht | Microsoft Azure"
    description="Der nicht lizenzierten Verwendung Bericht hilft nicht lizenzierter Benutzer zu identifizieren, die verwenden bezahlt Azure Active Directory-Funktionen."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Nicht lizenzierte Verwendungsbericht

Der nicht lizenzierten Verwendung Bericht hilft nicht lizenzierter Benutzer zu identifizieren, die verwenden bezahlt Azure Active Directory-Funktionen. Hier können Sie besser zu machen Verwendung der Lizenzen ein, die Sie erworben haben und um zu identifizieren, wenn Sie weitere Lizenzen benötigen möglicherweise wissen. 

Der Bericht zeigt aktive Verwendung kostenpflichtiges Features in der letzten 30 Tage an. 

## <a name="report-structure"></a>Struktur des Berichts
 
| Spaltenname          |    Beschreibung |
| :--                  | :--         |
| Nicht lizenzierten Benutzer      |    Name des Benutzers |
| Feature              | Der Featurename. Beispiel: bedingte Access |
| Anwendung auf zugegriffen wird | Der Name der Anwendung, die mit dem Feature zugegriffen wird. Beispiel: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Wenn ein Benutzerkonto gelöscht wurde, wird die Spalte 'Nicht lizenzierter Benutzer' mit Personalnummer wie 1003000090D8B285 aufgefüllt


## <a name="conditional-access-feature"></a>Funktion des bedingten Zugriffs

Beim Zugriff auf einen Dienst, der angewendet werden, wenn sie nicht über eine Lizenz Azure AD Premium verfügen bedingte Zugriffsrichtlinie enthält, werden nicht lizenzierter Benutzer gekennzeichnet. 

Dies gilt für MFA / Speicherort Richtlinien als auch Gerät Richtlinien beschrieben, die Intune verwenden.
 

## <a name="see-also"></a>Siehe auch

- [Verwenden von bedingten Access mit Office 365 und andere Azure Active Directory verbunden apps](active-directory-conditional-access.md)
- [Erste Schritte mit bedingten Azure AD-Zugriff](active-directory-conditional-access-azuread-connected-apps.md) 



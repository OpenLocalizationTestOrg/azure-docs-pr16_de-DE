<properties
   pageTitle="Administrative Einheiten verwalten in Azure Active Directory"
   description="Mithilfe von administrativen Einheiten für eine genauere Delegation von Berechtigungen in Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Administrative Einheiten verwalten in Azure AD - Public Preview-Version

In diesem Artikel werden die administrative Einheiten – einen neuen Azure Active Directory-Container von Ressourcen, die für das Zuweisen von Administratorberechtigungen über eine Teilmenge Benutzer und der Anwendung von Richtlinien auf eine Teilmenge der Benutzer verwendet werden kann. Aktivieren Sie im Azure Active Directory administrative Einheiten zentrale Administratoren Berechtigungen regionalen Administratoren delegieren oder Richtlinie genauer festlegen.

Dies ist sinnvoll, in Organisationen mit unabhängigen Bereichen, beispielsweise eine große University, die viele eigenständigen Schulen (Business Schule, technisch Schule usw.) bestehen die unabhängig voneinander sind. Solche Abteilungen verfügen über eigene IT-Administratoren, die Steuern des Zugriffs, Verwalten von Benutzern und Festlegen von Richtlinien speziell für ihre einer Division zurück. Erteilen von zentralen Administratoren können möchten diese Abteilung Administratorberechtigungen über die Benutzer in ihrer bestimmten Abteilungen. Genauer, kann in diesem Beispiel ein zentraler Administrator, beispielsweise eine administrative Einheit für eine bestimmte Schule (Business Schule) erstellen und füllen Sie es mit nur Benutzer der Schule. Klicken Sie dann erteilen Sie ein zentraler Administrator die Schule Business IT-Personal eines Suchbegriffs Rolle hinzufügen kann, also das IT-Personal Business Schule Administratorberechtigungen nur über die Business Schule administrative Einheit.

> [AZURE.IMPORTANT]
> Sie können erstellen und administrative Einheiten nur verwenden, wenn Sie Azure Active Directory Premium aktivieren. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md).

Aus der zentralen Administrator der Sicht ist eine administrative Einheit ein Verzeichnisobjekt, das erstellt und mit den Ressourcen ausgefüllt werden kann. **In dieser Version können diese Ressourcen nur Benutzer sein.** Wenn erstellt und aufgefüllt, kann die administrative Einheit als Bereich zum Einschränken der erteilten Berechtigung nur über in die administrative Einheit enthaltenen Ressourcen verwendet werden.

## <a name="managing-administrative-units"></a>Verwalten von administrativen Einheiten

In dieser Version Vorschau können Sie erstellen und Verwalten von administrativen Einheiten Azure Active Directory-Modul für Windows PowerShell-Cmdlets verwenden.

Weitere Informationen über softwareanforderungen und Azure AD-Modul installieren, und Informationen zu den Azure-Active Directory-Modul-Cmdlets für die Verwaltung von administrativen Einheiten, einschließlich Syntax, Parameter Beschreibungen und Beispielen finden Sie unter [Verwalten Azure Active Directory mithilfe von Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Nächste Schritte
[Azure Active Directory-Editionen](active-directory-editions.md)

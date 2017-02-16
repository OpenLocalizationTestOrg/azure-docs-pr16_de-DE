<properties
   pageTitle="Azure Datenkatalog Vorkenntnisse | Microsoft Azure"
   description="Azure Datenkatalog Vorkenntnisse – Sie müssen den ersten Schritten mit Azure Datenkatalog."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure Datenkatalog erforderliche Komponenten

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Was muss ich den ersten Schritten mit Azure Datenkatalog?

Es gibt ein paar Punkte, die, denen Sie benötigen, zu erledigen vor dem **Datenkatalog Azure**einrichten können. Keine Sorge – sie nehmen keine lange!

## <a name="azure-subscription"></a>Azure-Abonnement
Um Azure Datenkatalog eingerichtet haben, müssen Sie den Besitzer oder eines Azure-Abonnements gemeinsame Besitzer sein.

Azure-Abonnements können Sie Zugriff auf die Cloud-Service-Ressourcen wie Azure Datenkatalog organisieren. Diese auch Hilfe Sie steuern, wie Ressource: Einsatz gemeldet wird, in Rechnung gestellt, und für bezahlt. Jedes Abonnement kann eine andere Abrechnung und Bezahlung eingerichtet haben, anderen Abonnements und anderen Pläne nach Abteilung, Project, Landes-/ Office usw. zu haben. Jeder Cloud-Dienst auf ein Abonnement gehört, und muss ein Abonnement vor dem Einrichten von Azure Datenkatalog sein. Weitere Informationen finden Sie unter [Konten verwalten, Abonnements, und Administrative Rollen](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure-Active Directory
Um Azure Datenkatalog eingerichtet haben, müssen Sie sich mit einem Benutzerkonto Azure Active Directory angemeldet sein.

Azure Active Directory (Azure AD) bietet eine einfache Möglichkeit für Ihr Unternehmen Identität und greifen Sie sowohl in der Cloud und lokale verwalten. Benutzer können einer einzelnen Arbeit oder Schule Konto für einmaliges Anmelden in einem beliebigen Cloud und lokalen web-Anwendung. Azure Datenkatalog verwendet Azure AD Authentifizierung anmelden. Weitere Informationen finden Sie unter [Was ist Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] Im [Portal Azure](http://portal.azure.com/) ermöglicht Benutzern melden Sie sich mit einem persönlichen Microsoft Account oder einer Azure Active Directory-Arbeit oder Schule Konto an. Zum Einrichten von Azure Datenkatalog mithilfe des Azure-Portals oder im [Portal Datenkatalog](http://www.azuredatacatalog.com) verwenden müssen Sie sich mit einem Azure Active Directory-Konto nicht mit einem persönlichen Konto angemeldet sein.

## <a name="active-directory-policy-configuration"></a>Active Directory-Richtlinie-Konfiguration

In einigen Fällen können Benutzer eine Situation auftreten, in dem sie sich anmelden können Datenkatalog Azure-Portal an, aber bei dem Versuch, an der Quelle-Registration-Tool Anmelden eine Fehlermeldung angezeigt, die verhindert, dass sie auftretenden auf Protokollierung. Dieses Problem Verhalten kann auftreten, nur, wenn der Benutzer sich im Unternehmensnetzwerk befindet, oder nur, wenn der Benutzer von außerhalb des Unternehmensnetzwerks eine Verbindung herstellt auftreten.

Tools für die Datenquelle Registrierung verwendet formularbasierte Authentifizierung an-gegen Active Directory überprüft. Für die erfolgreiche Anmeldung muss formularbasierte Authentifizierung von einem Active Directory-Administrator in der globalen Richtlinie Authentifizierung aktiviert sein.

Die globale Authentifizierungsrichtlinie ermöglicht Authentifizierungsmethoden für Intranet- und extranet-Verbindungen, separat aktiviert werden, wie unten dargestellt. Fehler bei der Anmeldung können auftreten, wenn für das Netzwerk formularbasierte Authentifizierung nicht aktiviert ist, aus denen der Benutzer eine Verbindung herstellt.

 ![Active Directory-Authentifizierung globale Richtlinie](./media/data-catalog-prerequisites/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

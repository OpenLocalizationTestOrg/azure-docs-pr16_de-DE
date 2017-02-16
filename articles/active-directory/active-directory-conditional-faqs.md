<properties
    pageTitle="Bedingte Azure-Active Directory-Zugriff häufig gestellte Fragen zu | Microsoft Azure"
    description="Häufig gestellte Fragen zur bedingten Zugriff "
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

# <a name="azure-active-directory-conditional-access-faq"></a>Bedingte Azure-Active Directory-Zugriff häufig gestellte Fragen

## <a name="which-applications-work-with-conditional-access-policies"></a>Die Anwendungsmöglichkeiten mit bedingten Richtlinien arbeiten zu können?

**A:** Finden Sie im Thema [bedingte Access-was Applikationen werden unterstützt](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Werden bedingte Richtlinien für B2B Zusammenarbeit und für Gastbenutzer werden erzwungen?

**A:** Richtlinien werden für die Zusammenarbeit Benutzer B2B erzwungen. Jedoch in einigen Fällen ein Benutzers nicht möglicherweise die Richtlinie Anforderung erfüllen, wenn beispielsweise eine Organisation kombinierte Authentifizierung nicht unterstützt. 

Die Richtlinie wird derzeit nicht für SharePoint Gastbenutzer erzwungen. Die Beziehung Gast wird in SharePoint beibehalten. Gastbenutzer, die Konten keinen Zugriff unterliegen Richtlinien auf dem Authentifizierungsserver. Gast Access kann über SharePoint verwaltet werden.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Gilt eine SharePoint Online-Richtlinie auch für OneDrive for Business?

**A:** Ja.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Warum festlegen lässt keine Richtlinie auf Client-apps, wie Word oder Outlook?

**A:** Eine bedingte Zugriffsrichtlinie legt die Anforderungen für den Zugriff auf einen Dienst und wird erzwungen, wenn die Authentifizierung mit diesem Dienst geschieht. Die Richtlinie ist nicht direkt auf eine Clientanwendung festgelegt. Stattdessen wird sie angewendet, wenn es in einen Dienst Anrufe. Angenommen, eine Richtlinie auf SharePoint bezieht für das Anrufen von SharePoint-Clients und eine Richtlinie auf Exchange auf Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Wendet eine bedingte Zugriffsrichtlinie Dienstkonten an?

**A:** Bedingte Richtlinien beziehen sich auf alle Benutzerkonten. Dies umfasst Benutzerkonten als Dienstkonten verwendet. In vielen Fällen kann ein Dienstkonto unbeaufsichtigt ausgeführt wird, die keine Richtlinie zu erfüllen. Dies ist beispielsweise der Fall, wenn MFA erforderlich ist. In diesen Fällen können einer Richtlinie, die mithilfe der Einstellungen für die Richtlinie bedingte Access Services-Konten ausgeschlossen werden müssen. Weitere Informationen zum Anwenden einer Richtlinie für Benutzer hier.

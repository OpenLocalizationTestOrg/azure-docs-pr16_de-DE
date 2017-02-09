<properties
    pageTitle="Erste Schritte mit der Azure AD-API reporting | Microsoft Azure"
    description="Gewusst wie: Erste Schritte mit der API reporting Azure-Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Erste Schritte mit der API reporting Azure-Active Directory

*Dieses Thema ist Teil des [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Azure-Active Directory bietet Ihnen eine Vielzahl verschiedener Berichte. Die Daten der diese Berichte können den Clientanwendungen, wie z. B. SIEM Systeme, Audit und Business Intelligence-Tools sehr nützlich sein. Die Azure AD APIs programmgesteuerten Zugriff auf die Daten über eine Reihe von REST-basierten APIs liefern. Sie können diese APIs aus einer Vielzahl von Sprachen und Tools programming aufrufen.

Dieser Artikel bietet mit den Informationen zum Einstieg in die Azure AD erforderlichen reporting APIs.
Im nächsten Abschnitt Sie finden weitere Details zur Verwendung des Audits und APIs anmelden. Für alle anderen APIs finden Sie unter [Azure AD-Berichte und Ereignisse (Preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) .


Wenden Sie für Fragen, Probleme oder Feedback sich an [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


##<a name="learning-map"></a>Lernen die Karte

1. **Vorbereiten** - bevor Sie Ihre API Beispiele testen können, müssen Sie die [Voraussetzungen für die Berichterstattung API Azure AD-Zugriff auf](active-directory-reporting-api-prerequisites.md)ausführen.

2. **Durchsuchen** - Abrufen einer ersten Eindruck von der reporting APIs:

  - [Verwenden der Beispiele für die Überwachung API](active-directory-reporting-api-audit-samples.md) 
  - [Verwenden der Beispiele für die Anmeldung Aktivitätsbericht API](active-directory-reporting-api-sign-in-activity-samples.md)

3. **Anpassen** – erstellen eine eigenen Lösung: 

  - [Verwenden des Audits-API-Referenz](active-directory-reporting-api-audit-reference.md) 
  - [Verwenden den Bezug der Anmeldung Aktivität Bericht API](active-directory-reporting-api-sign-in-activity-reference.md)





## <a name="next-steps"></a>Nächste Schritte

Wenn Sie interessiert sind alle verfügbaren Azure AD Graph-API Endpunkte durch Navigieren zu sehen sind [https://graph.windows.net/tenant-name/reports/$ Metadata?api-Version Beta =](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).

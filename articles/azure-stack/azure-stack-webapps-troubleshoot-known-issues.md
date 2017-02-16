<properties
    pageTitle="Web Apps auf Azure Stapel - bekannte Probleme und Problembehandlung | Microsoft Azure"
    description="Eine umfassende Unterstützung für die Bereitstellung von Web Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps-Ressource Anbieter - bekannte Probleme und Problembehandlung

> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Wenn Sie Probleme beim Bereitstellen oder Arbeiten mit Web Apps auf Azure Stapel finden Sie in der folgenden Anleitung.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure Stapel Web Apps nicht verfügbar im Portal

![Azure Stapel Web Apps erstellen neue Web App][1]

Nachdem Sie die [Registrierung von den Azure Stapel Web Apps-Anbieter](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) beendet haben, kann nicht suchen im Web + Mobile Blade und Bitte probieren Sie Folgendes:
* **Abmelden des Portals** und **Schließen Sie den Browser** , und klicken Sie dann in eine **neue Browser-Fenster Login-Portal an** und versuchen Sie es erneut.

Wenn Sie immer noch nicht im Web und mobile Blade, versuchen Sie Folgendes angezeigt:

1.  Suchen Sie auf der **Azure Stapel Hostcomputer** in **Hyper-V-Manager** die **xRPVM** und **eine Verbindung herstellen** , mit dem virtuellen Computer ein.
2.  Öffnen Sie ein **Eingabeaufforderungsfenster als Administrator** , und führen Sie **IISRESET**
3.  Kehren Sie zu der **ClientVM** zurück, und öffnen Sie ein **neues Browserfenster**, **Melden Sie sich im Portal** , und versuchen Sie es erneut.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Bereitstellung schlägt fehl, wenn eine Web-App in einer neuen Ressourcengruppe erstellen

In der ersten Vorschau der Web Apps müssen **Alle Web Apps** in derselben Ressourcengruppe als Web Apps Ressourcenanbieter (**AppService-lokale**) erstellt werden.  Dies ist ein bekanntes Problem und wird in einer Vorschau zukünftigen aufgelöst werden.

## <a name="other-issues"></a>Sonstige Probleme

Wenn andere auftreten Posten Probleme mit Web Apps auf Azure Stapel bitte [im Forum Azure Stapel] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack), in dem die Mitglieder unseres Teams Beiträge Überwachung sind.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

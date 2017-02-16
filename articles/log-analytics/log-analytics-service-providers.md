<properties
    pageTitle="Melden Sie sich Analytics-Features für Dienstanbieter | Microsoft Azure"
    description="Log Analytics können verwaltete Dienstanbieter (MSPs), Großunternehmen unabhängigen Software-Anbietern und Hostinganbieter Dienstanbieter verwalten und Überwachen von Kunden lokalen oder Cloud-Infrastruktur-Servern."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Melden Sie sich Analytics-Features für Dienstanbieter

Log Analytics helfen verwalteten Dienstanbieter (MSPs), großen Unternehmen, unabhängigen Softwareanbietern und Hostinganbieter Dienstanbieter verwalten und Überwachen von Kunden lokalen oder Cloud-Infrastruktur-Servern. 

Große Unternehmen Dienstanbieter, viele gemeinsamkeiten freigeben, vor allem, wenn eine zentrale IT-Team, der für die Verwaltung von IT für viele verschiedene Business Einheiten. Zur Vereinfachung dieses Dokument verwendet den Begriff *Dienstanbieter* , aber die gleiche Funktionalität steht auch für Unternehmen und andere Kunden.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider

Für Partner und Dienstanbieter, die Teil des Programms [Cloud Lösung Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) sind, ist Log Analytics der Azure Dienste auf ein Abonnement CSP zur Verfügung. 

Zum Log Analytics sind die folgenden Funktionen in der *Cloud Solution Provider* Abonnements aktiviert.

Als ein *Cloud Solution Provider* können Sie folgende Aktionen ausführen:

+ Erstellen Sie Log Analytics-Arbeitsbereiche in einen Mandanten (Customer) Abonnement.
+ Access-Arbeitsbereiche Mandanten erstellt. 
+ Fügen Sie hinzu und entfernen Sie des Benutzerzugriffs auf den Arbeitsbereich, der mit der Verwaltung von Azure Benutzer. Wenn in einem Mandanten Arbeitsbereich im Portal OMS Seite die Benutzer Verwaltung unter ist Einstellungen nicht verfügbar
  - Log Analytics unterstützt keine rollenbasierte Access noch -ein Benutzers zugewiesen `reader` Berechtigung im Portal Azure ermöglicht es ihnen, die Konfiguration des Portals OMS ändern

Um einen Mandanten Abonnement angemeldet haben, müssen Sie den Mandanten Bezeichner anzugeben. Der Mandant Bezeichner ist häufig die letzte Teil der e-Mail-Adresse zum Anmelden verwendet.

+ Fügen Sie im Portal OMS `?tenant=contoso.com` in der URL für das Portal. Beispielsweise`mms.microsoft.com/?tenant=contoso.com`
+ Verwenden Sie PowerShell, die `-Tenant contoso.com` Parameter bei Verwendung von `Add-AzureRmAccount` Cmdlet
+ Die Mandanten-ID wird automatisch hinzugefügt, bei der Verwendung von der `OMS portal` links vom Azure-Portal zu öffnen, und melden Sie sich bei der OMS-Portal für den ausgewählten Arbeitsbereich

Sie können einen *Kunden* eines Anbieters Cloud-Lösung:

+ Erstellen von Log Analytics Arbeitsbereiche in einem CSP-Abonnement
+ Erstellt haben, indem Sie den Anbieter, den Access-Arbeitsbereiche
  -  Verwenden der `OMS portal` links vom Azure-Portal zu öffnen, und melden Sie sich bei der OMS-Portal für den ausgewählten Arbeitsbereich
+ Zeigen Sie an und verwenden Sie der Benutzer-Verwaltungsseite unter Einstellungen im Portal OMS

>[AZURE.NOTE] Die Sicherung und Wiederherstellung von Website-Lösungen für Protokoll Analytics können keine Verbindung zu einem Tresor Wiederherstellung Services und können nicht in einem Abonnement CSP konfiguriert werden.

## <a name="managing-multiple-customers-using-log-analytics"></a>Verwalten von mehreren Log Analytics mit Kunden 

Es wird empfohlen, dass Sie für jeden Kunden einen Protokoll Analytics-Arbeitsbereich erstellen, die Sie verwalten. Ein Arbeitsbereich Log Analytics bietet:

+ Eine geografische Position Daten gespeichert werden. 
+ Genauigkeit für Abrechnung 
+ Grad der Datenisolation 
+ Eindeutige Konfiguration

Durch das Erstellen eines Arbeitsbereichs pro Kunde, sind Sie Trennen des Kunden Daten und die Verwendung der einzelnen Kunden auch nachverfolgen können.

Wann und wie Sie mehrere Arbeitsbereiche erstellen Weitere Details finden Sie unter [Verwalten des Zugriffs zum Melden Analytics] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Erstellung und Konfiguration der Kunden-Arbeitsbereiche können mithilfe der [PowerShell](log-analytics-powershell-workspace-configuration.md) [Ressourcenmanager Vorlagen](log-analytics-template-workspace-configuration.md), oder verwenden die [REST-API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/)automatisierte werden.

Ermöglicht die Verwendung von Ressourcenmanager Vorlagen für Workspace-Konfiguration, dass Sie eine master-Konfiguration besitzen, die zum Erstellen und Konfigurieren von Arbeitsbereichen verwendet werden können. Sie können sicher sein, dass während der Erstellung von Arbeitsbereichen für Kunden sie automatisch an Ihre Bedürfnisse konfiguriert sind. Wenn Sie Ihren Anforderungen aktualisieren, wird die Vorlage wird aktualisiert, und klicken Sie dann erneut der vorhandenen Arbeitsbereiche angewendet. Dieses Verfahren wird sichergestellt, dass Ihre neue auch vorhandene Arbeitsbereiche entsprechen.    

Beim Verwalten mehrerer Log Analytics Arbeitsbereiche, wir empfehlen Integration von jeder Arbeitsbereich mit Ihrer vorhandenen ticketingsystem / Console Vorgänge mithilfe der [Benachrichtigungen](log-analytics-alerts.md) -Funktionalität. Durch die Integration mit Ihren vorhandenen Systemen, können Callcenter-Mitarbeiter weiterhin ihre vertrauten Prozesse befolgen. Log Analytics regelmäßig überprüft jeder Arbeitsbereich mit den benachrichtigen Kriterien, die Sie angeben und generiert eine Warnung aus, wenn die Aktion erforderlich ist.

Für Geschäftsleitung Ebene Berichte, die Zusammenfassen von Daten über Arbeitsbereiche können Sie die Integration Log Analytics und [PowerBI](log-analytics-powerbi.md). Wenn Sie mit einem anderen reporting System integriert werden soll müssen, können Sie die Suche-API (über PowerShell oder [REST](log-analytics-log-search-api.md)) verwenden, um Abfragen auszuführen und Suchergebnisse exportieren.

## <a name="next-steps"></a>Nächste Schritte

+ Automatisieren der Erstellung und Konfiguration der Arbeitsbereiche, die mithilfe von [Vorlagen für Ressourcenmanager](log-analytics-template-workspace-configuration.md)
+ Automatische Erstellung von Arbeitsbereichen mithilfe der [PowerShell](log-analytics-powershell-workspace-configuration.md) 
+ Verwenden von [Benachrichtigungen](log-analytics-alerts.md) in vorhandene Systeme integriert werden soll.
+ Generieren Sie Zusammenfassungsberichte [PowerBI](log-analytics-powerbi.md) verwenden

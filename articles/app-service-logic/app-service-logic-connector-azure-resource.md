<properties
   pageTitle="Verwenden den Verbinder Azure Ressource in Logik apps | Microsoft Azure-App-Verwaltungsdienst"
   description="So erstellen und Konfigurieren der app Azure Ressource Verbinder oder API und diese in einer app Logik in Azure-App-Dienst verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit der Azure Ressource Verbinder, und fügen Sie es zu Ihrer Anwendung Logik
>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus.

Verwenden Sie den Azure Ressource Verbinder, um einfach Azure Ressourcen innerhalb der app Logik verwalten.

## <a name="create-the-azure-resource-connector"></a>Erstellen des Ressource Azure-Verbinders
Um die Azure Ressource Connector-API app verwenden zu können, müssen Sie zuerst eine Instanz davon erstellen. Dies kann entweder Inline beim Erstellen einer app Logik oder durch Auswahl der app Azure Ressource-Manager Connector-API aus dem Azure Marketplace erfolgen.

Um ihn zu konfigurieren, müssen Sie Sie von einem Dienst Tilgungsanteile mit Berechtigungen zu tun ist, welchen es ist, dass die gewünschte Aktion in Azure festlegen müssen. Alle Anrufe im Auftrag von diesem Dienst Tilgungsanteile erfolgen, die Sie haben eingerichtet. Hier können Sie auf den Verbinder, um verwenden nur genau was zu tun ist der gewünschten Bereich, und nichts mehr.

David Ebbo hat [eine hervorragende Blogbeitrag](http://blog.davidebbo.com/2014/12/azure-service-principal.html) auf wie folgt geschrieben werden. Anweisungen Sie alle die vorhanden, und erhalten Sie Ihrem **Mandanten-ID**, **Client-ID** und **geheim**. Diese drei Felder sowie die **Abonnement-ID**, sind an, was für das Konfigurieren des Verbinders erforderlich sind.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Verwenden den Azure Ressource Verbinder im Logik apps-designer
### <a name="trigger"></a>Auslösen
Es gibt zwei Trigger, die in der Verbinder unterstützt werden:

Namen | Beschreibung
---- | -----------
Ereignis eintritt. | Auslösen einer Ressourcenansicht hinzu, in Ihrem Abonnement Eintreten ein Ereignisses.
Metrisch schneidet Schwellenwert |  Auslösen, wenn eine Metrik einen bestimmten Schwellenwert entspricht.

### <a name="action"></a>Aktion

Ebenso können Sie eine große Anzahl von Aktionen innerhalb Ihres Abonnements Azure bereitstellen:

Für die **Ressourcengruppen** können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Ressourcen-Gruppen auflisten | Liste aller Ressourcengruppen in das Abonnement.
Abrufen von Ressourcengruppe | Abrufen einer Ressourcengruppe anhand dessen Id an.
Ressourcengruppe erstellen | Erstellen oder Aktualisieren einer Ressourcengruppe.
Löschen von Ressourcengruppe | Löschen einer Ressourcengruppe an.

Für **Ressourcen** können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Wenn die Ressourcen | Liste der Ressourcen in Ihrem Abonnement nach verschiedenen Arten von Filtern.
Abrufen von Ressourcen | Abrufen einer einzelnen Ressource durch eine Ressourcen-ID an.
Erstellen oder Aktualisieren von Ressourcen | Erstellen Sie eine Ressource, oder aktualisieren Sie eine vorhandene Ressource. Sie müssen alle Eigenschaften für die Ressource angeben.
Ressource Aktion |  Führen Sie eine beliebige andere Aktion auf eine Ressource ein. Sie wissen, dass der Aktionsname und die Nutzlast, die diesem Vorgang (falls vorhanden) werden müssen.
Löschen von Ressourcen | Löschen einer Ressource an.

Für **Ressourcenanbieter** können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Liste Ressourcenanbieter | Listen Sie alle verfügbaren Ressourcenanbieter in das Abonnement aus.

Für die **Ressource Gruppe Bereitstellungen** können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Liste Bereitstellungen | Liste aller Bereitstellungen in einer Ressourcengruppe.
Abrufen der Bereitstellung | Abrufen einer Vorlage bereitstellungs anhand dessen Id an.
Erstellen der Bereitstellung | Bereitstellen einer Vorlage, um eine neue Gruppe Bereitstellung von Ressourcen zu erstellen.

Für **Ereignisse** zu Ressourcen können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Abrufen von Ereignissen | Abrufen von Ereignissen in einem Abonnement oder für eine Ressource.

Für **Kennzahlen** zu Ressourcen können Sie folgende Aktionen ausführen:

Namen | Beschreibung
---- | -----------
Kennzahlen abrufen | Abrufen einer Metrik für eine Ressource-ID an.

## <a name="do-more-with-your-connector"></a>Führen Sie den Verbinder
Jetzt, da der Verbinder erstellt wurde, können Sie eine Business Fluss mit einer app Logik hinzufügen. Finden Sie unter [Was Logik apps werden?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Wenn Sie mit Azure Logik apps Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [app Logik versuchen](https://tryappservice.azure.com/?appservice=logic), in dem Sie eine kurzlebige Starter Logik app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

Zeigen die REST-API Swagger Referenz am [Verbindern und API apps Bezug](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md

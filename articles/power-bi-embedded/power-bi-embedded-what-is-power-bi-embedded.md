<properties
   pageTitle="Was ist Microsoft Power BI eingebettete?"
   description="Power BI eingebettete ermöglicht es Ihnen, die Power BI-Berichte in Ihrem Web oder mobilen Anwendungen zu integrieren, damit Sie nicht zum Erstellen von benutzerdefinierter Lösungen zum Darstellen der Daten für Ihre Benutzer"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Was ist Microsoft Power BI eingebettete?

Mit **Power BI eingebettete**können Sie direkt in Ihrem Web oder mobilen Anwendungen Power BI-Berichte integrieren.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI eingebettete ist eine **Azure Service** , ISVs und app-Entwickler in Surface Power BI-Erfahrung mit Daten in ihrer Anwendung ermöglicht. Als Entwickler haben Sie Applikationen erstellt, und diese Applikationen haben ihre Benutzer und distinct Reihe von Features. Diese apps können auch geschehen, dass einige integrierte Datenelemente wie Diagramme und Berichte, die von Microsoft Power BI Embedded jetzt eingeschaltet werden können. Benutzer erforderlich nicht, dass ein Power BI-Konto, um Ihre app verwenden. Sie können weiterhin melden Sie sich an Ihrer Anwendung genauso wie zuvor, anzeigen und interagieren mit Power BI ohne zusätzliche Lizenzierung berichterstellung.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Eingebettete für Microsoft Power BI-Lizenzierung

In der **Microsoft Power BI eingebettete** Verwendungsmodell ist für Power BI-Lizenzierung nicht Aufgabe des Endbenutzers.  Stattdessen **werden gerendert** werden vom Entwickler der app, die die visuellen Objekte in Anspruch nimmt gekauft und unterliegen in das Abonnement, das diese Ressourcen besitzt.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI eingebettete konzeptionelles Modell

![](media\powerbi-embedded-whats-is\model.png)

Ressourcen für Power BI eingebettete werden wie jeden anderen Dienst Azure über die [Azure-Cloud-APIs](https://msdn.microsoft.com/library/mt712306.aspx)bereitgestellt. In diesem Fall ist die Ressource, die Sie Bereitstellen einer **Power BI-Arbeitsbereich-Websitesammlung**.

## <a name="workspace-collection"></a>Arbeitsbereich-Websitesammlung

Eine **Auflistung der Arbeitsbereich** ist der Container auf oberster Ebene Azure für Ressourcen, der 0 oder mehr **Arbeitsbereiche**enthält.  Eine **Arbeitsbereich** - **Auflistung** verfügt über alle Standardeigenschaften Azure sowie die folgenden:

-   **Zugriffstasten** – Tasten verwendet, wenn die Power BI-APIs (in einem späteren Abschnitt beschrieben) sicher aufrufen.
-   **Benutzer** – Azure Active Directory (AAD) Benutzer, die zur Verwaltung der Power BI-Sammlung Arbeitsbereich über das Azure-Portal oder Cloud-API über Administratorrechte verfügen.
-   **Region** – können als Teil der Bereitstellung eine **Auflistung der Arbeitsbereich**, Sie einen Bereich in die bereitzustellende auswählen. Weitere Informationen finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Arbeitsbereich

Ein **Arbeitsbereich** ist ein Container mit Power BI-Inhalte, die Datasets, Berichten und Dashboards enthalten sein kann. Ein **Arbeitsbereich** ist bei der anfänglichen Erstellung leer. Während der Vorschau werden Sie alle Inhalte, die mit Power BI-Desktop erstellen, und Sie werden in einem Ihrer Arbeitsbereiche mithilfe der [Power BI REST APIs](http://docs.powerbi.apiary.io/reference)hochladen.

## <a name="using-workspace-collections-and-workspaces"></a>Verwenden von Arbeitsbereich-Sammlungen und Arbeitsbereiche
**Arbeitsbereich-Sammlungen** und **Arbeitsbereiche** sind Container von Inhalten, die verwendet und in dem bewährte Methode das Design der Anwendung passt, den Sie gerade erstellen angeordnet sind. Es werden viele verschiedene Arten, dass Sie den Inhalt darin enthaltenen anordnen können. Sie können auswählen, platziert werden alle Inhalte in einem Arbeitsbereich, und verwenden Sie dann später app Token um den Inhalt dem Ihre Kunden weiter unterteilen. Sie können auch auf alle Ihre Kunden in getrennten Arbeitsbereichen setzen, damit es einige Trennung dazwischen gibt auswählen. Oder Sie auch Benutzer nach Region und nicht nach Kunde zu organisieren. Dieses flexible Design können Sie die beste Methode zum Organisieren von Inhalt auswählen.

## <a name="cached-datasets"></a>Zwischengespeicherte Datasets

Zwischengespeicherte Datasets können in der Vorschau verwendet werden.  Jedoch nicht zwischengespeicherte Daten aktualisiert werden, nachdem es in **Microsoft Power BI eingebettete**geladen wurde.

## <a name="authentication-and-authorization-with-app-tokens"></a>Authentifizierung und Autorisierung mit app Token

**Microsoft Power BI eingebettete** verzögert an Ihrer Anwendung alle notwendigen Benutzerauthentifizierung und Autorisierung ausführen. Es gibt keine explizite Anforderung, dass Ihre Endbenutzer Kunden von Azure Active Directory (Azure AD) sein.  In diesem Fall angibt Ihrer Anwendung zu **Microsoft Power BI eingebettete** Autorisierung zum Rendern einer Power BI-Berichts mithilfe der **Anwendung Authentifizierungstoken (Token App)**ein.  Je nach Bedarf an, wenn Ihre app zum Rendern eines Berichts soll, werden diese **App Token** erstellt.  Finden Sie unter [App Token](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Anwendung Authentifizierungstoken (Token App)** werden zum Authentifizieren **Eingebettete Microsoft Power BI**.  Es gibt drei Arten von **App Token**aus:

1.  Token - verwendet beim Bereitstellen eines neuen **Arbeitsbereichs** in einem **Arbeitsbereich Websitesammlung** bereitgestellt
2.  Entwicklung Token - Anrufe direkt an der **Power BI REST APIs** herstellen verwendet
3.  Das Einbetten von Tokens - verwendet, wenn Sie einen Bericht in der eingebetteten Iframe Rendern aufrufen

Diese Token werden für die verschiedenen Phasen Ihrer Interaktionen mit **Microsoft Power BI eingebettete**verwendet.  Die Token wurden dazu ausgelegt, damit Sie Berechtigungen aus der app an der Power BI delegieren können. Weitere Informationen finden Sie unter [App Token Datenfluss](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Siehe auch
- [Häufige Microsoft Power BI eingebettete Szenarien](power-bi-embedded-scenarios.md)
- [Erste Schritte mit Microsoft Power BI eingebettete](power-bi-embedded-get-started.md)

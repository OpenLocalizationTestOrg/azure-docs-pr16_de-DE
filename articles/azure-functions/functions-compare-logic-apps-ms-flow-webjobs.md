<properties
    pageTitle="Wählen Sie zwischen Fluss, Logik Apps, Funktionen und WebJobs | Microsoft Azure"
    description="Vergleichen und vergleichen Sie die Cloud Integration services von Microsoft und entscheiden, welche Dienste sollten Sie Folgendes verwenden."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft Fluss, Fluss, Logik apps, Azure Funktionen Azure Funktionen Webjobs, Webjobs, Verarbeitung, dynamische berechnen, ohne Server Architektur von Ereignissen"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Wählen Sie zwischen Fluss, Logik Apps, Funktionen und WebJobs

Dieser Artikel enthält eine Gegenüberstellung und in der Microsoft-Cloud, die alle Integrationsprobleme und Automatisierung von Geschäftsprozessen lösen können die folgenden Dienste:

- [Microsoft-Fluss](https://flow.microsoft.com/)
- [Azure Logik Apps](https://azure.microsoft.com/services/logic-apps/)
- [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
- [App-Verwaltungsdienst Azure WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Alle diese Dienste sind nützlich, wenn "zusammen Ankleben von unterschiedlichen Systemen". Sie können alle Eingabe-, Aktionen, Bedingungen und Ausgabe definieren. Sie können jede von ihnen auf einen Zeitplan oder Trigger ausgeführt werden. Jedoch jeden Dienst fügt eines eindeutigen Satzes von Wert und deren Vergleich ist keine Frage "ist der Dienst die beste?" aber eine der "welche Dienst am besten eignet sich für diese Situation?" Eine Kombination der folgenden Dienste ist häufig die beste Möglichkeit, eine skalierbare, vollständige bereitgestellten Integration Lösung schnell zu erstellen.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Fluss im Vergleich zu Logik Apps

Microsoft Flow und Azure Logik Apps können zusammen besprochen, da sie beide *Konfigurations-First* Integrationsservices, welche erleichtert das Erstellen von Prozessen und Workflows und Integration mit verschiedenen SaaS und Enterprise-Anwendung sind. 

- Fluss basiert auf Logik Apps
- Sie haben die gleichen Workflow-Designers
- [Verbinder](../connectors/apis-list.md) , die in einem arbeiten können auch in anderen arbeiten.

Zahlungen können Sie alle Office-Worker einfache Integration ausführen (z. B. SMS für wichtige e-Mails abrufen) ohne zu durchlaufen Entwickler oder IT. Logik Apps können andererseits, erweiterte oder wichtiger Integration (z. B. B2B Prozesse) aktivieren, wo Methoden für Enterprise-Ebene DevOps und Sicherheit erforderlich sind. Es ist für einen Workflow Business Komplexität Überstunden zunehmen Standard. Sie können entsprechend, beginnen Sie mit einen Fluss auf den ersten und dann wandeln Sie es in eine app Logik, je nach Bedarf.

Der folgenden Tabelle können Sie feststellen, ob Fluss oder Logik Apps für eine angegebene Integration am besten geeignet ist.

|               | Fluss                                                                             | Logik Apps                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Zielgruppe      | Office-Worker, Business-Benutzer                                                   | IT-Experten, Entwickler                                                                                 |
| Szenarien     | Self-Service                                                                     | Wichtiger                                                                                    |
| Entwurfstool   | Im Browser, nur UI                                                              | Im Browser und in [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md) [Code-Ansicht](../app-service-logic/app-service-logic-author-definitions.md) zur Verfügung |
| DevOps        | Ad-hoc-Herstellung entwickeln                                                    | Datenquellen-Steuerelements, testen, Support und Automatisierung und verwaltbarkeit in [Azure Ressourcenverwaltung](../app-service-logic/app-service-logic-arm-provision.md)|
| Admin-Benutzeroberfläche| [https://Flow.Microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Sicherheit      | Standard-Methoden: [Daten Hoheit](https://wikipedia.org/wiki/Technological_Sovereignty), [Verschlüsselung statisch](https://wikipedia.org/wiki/Data_at_rest#Encryption) für vertrauliche Daten usw.. | Sicherheit Assurance von Azure: [Azure Sicherheit](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Sicherheitscenter](https://azure.microsoft.com/services/security-center/), [Überwachungsprotokolle](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)und vieles mehr. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funktionen im Vergleich zu WebJobs

Wir können diskutieren Azure-Funktionen und Azure-App-Dienst WebJobs zusammen, da sie beide *Code-First* Integrationsservices sind und für Entwickler entwickelt. Sie können Sie als Antwort auf verschiedene Ereignisse, z. B. [Neue Speicher Blobs](functions-bindings-storage.md) oder [eine WebHook anfordern](functions-bindings-http-webhook.md)eines Skripts oder einen Teil eines Codes ausführen. Hier sind die gemeinsamkeiten aus: 

- Beide basieren auf [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md) und genießen Features wie [Datenquellen-Steuerelements](../app-service-web/app-service-continuous-deployment.md), [Authentifizierung](../app-service/app-service-authentication-overview.md)und [Überwachen](../app-service-web/web-sites-monitor.md).
- Beide sind Services Developer-konzentrieren können.
- Sowohl standard scripting und programming Sprachen unterstützen.
- Beide verfügen über NuGet und NPM unterstützen.

Funktionen ist die Weiterentwicklung der WebJobs, dauert die beste Dinge über WebJobs und diese verbessert. Die Verbesserungen umfassen: 

- Optimierten Entwickler, testen und Ausführen des Codes, direkt im Browser.
- Integrierte Integration Services mehr Azure und 3rd Party-Dienste wie [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Bezahlung pro Einsatz, brauchen nicht zu einer [App-Verwaltungsdienst planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)bezahlen.
- Automatische, [dynamische Skalierung](functions-scale.md).
- Für vorhandene Kunden des App-Dienstes, unter dem App-Serviceplan dennoch möglich (, Auslastung Ressourcen nutzen zu können).
- Integration in Logik Apps.

In der folgenden Tabelle sind die Unterschiede zwischen Funktionen und WebJobs zusammengefasst:

|                        | Funktionen                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Skalierung                | Configurationless skalieren                                                                                                                                                | mit App-Serviceplan skalieren        |
| Preise                | Bezahlung pro Einsatz oder einen Teil des App-Serviceplan                                                                                                                                  | Teil des App-Serviceplan           |
| Ausführen-Datentyp               | ausgelöst wurde, geplant (von Timer Trigger)                                                                                                                                  | ausgelöste, geplanten fortlaufender   |
| Auslösen von Ereignissen         | [Timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Azure Ereignis Hubs](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, Pufferzeit)](functions-bindings-http-webhook.md), [Azure App Dienst Mobile-Apps](functions-bindings-mobile-apps.md), [Azure Benachrichtigung Hubs](functions-bindings-notification-hubs.md), [Azure-Dienstbus](functions-bindings-service-bus.md), [Azure-Speicher](articles/functions-bindings-storage.md) | [Azure-Speicher](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) [Azure Dienstbus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Entwicklung im browser | x                                                                                                                                                                        |                                    |
| Fenster scripting       | Versuche                                                                                                                                                             | x                                  |
| PowerShell             | Versuche                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | Versuche                                                                                                                                                             | x                                  |
| PHP                    | Versuche                                                                                                                                                             | x                                  |
| Python                 | Versuche                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | Versuche                                                                                                                                                             | x                                  |

Schließlich abhängig, ob Sie verwenden von Funktionen oder WebJobs bereits mit App-Dienst Tätigkeit. Wenn Sie haben eine App Service-app für die Codeausschnitte ausgeführt werden soll, und sie zusammen in der gleichen DevOps Umgebung verwaltet werden sollen, sollten Sie WebJobs verwenden. Wenn Codeausschnitte für andere Azure Services oder sogar 3rd Party apps, ausführen möchten Sie Ihre Integration Codeausschnitte unabhängig von der App-Dienst apps verwalten möchten, oder wenn Sie Ihre Codeausschnitte aus einer Logik app anrufen möchten, sollten Sie die Verbesserungen beim in-Funktionen nutzen.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Datenfluss, Logik Apps und Funktionen zusammen

Wie zuvor erwähnt welcher Dienst für Sie am besten geeignet sind, hängt von der jeweiligen Situation. 

- Verwenden Sie dann zur Optimierung der einfachen Business Fluss.
- Wenn Ihr Integrationsszenario auch für den Verkehr erweiterte wird oder DevOps Funktionen und Sicherheit Compliances benötigten, verwenden Sie Logik Apps.
- Wenn Sie ein Schritt in Ihrem Integrationsszenario hochgradig benutzerdefinierte Transformation oder speziellen Code erforderlich ist, Schreiben Sie eine app (Funktion), und klicken Sie dann lösen Sie eine Funktion als eine Aktion in Ihrer app Logik aus.

Sie können eine app Logik in einen Fluss aufrufen. Sie können auch eine Funktion in einer app Logik und eine app Logik in einer Funktion aufrufen. Die Integration zwischen Fluss, Logik Apps und Funktionen weiterhin Überstunden zu verbessern. Sie können Erstellen eines Beitrags in einem Dienst und diese in den anderen Diensten verwenden. Daher lohnt sich eine Investition, die Sie in den folgenden drei Technologien vornehmen.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit jeder der Dienste durch das Erstellen Ihrer ersten Fluss Logik app, Funktion app oder WebJob. Klicken Sie auf eine der folgenden Links:

- [Erste Schritte mit Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Erstellen Sie Ihrer ersten Azure-Funktion](../azure-functions/functions-create-first-azure-function.md)
- [Bereitstellen Sie WebJobs mit Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Oder Weitere Informationen zu diesen Integration Services mit den folgenden Links:

- [Nutzung der Azure-Funktionen und Azure-App-Verwaltungsdienst für Szenarien zur Integration von Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integration von Charles Lamanna leicht gemacht](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logik Apps Live Webcast](http://aka.ms/logicappslive)
- [Microsoft Datenfluss häufig gestellte Fragen](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs Dokumentationsressourcen](../app-service-web/websites-webjobs-resources.md)

<properties
    pageTitle="Übersicht über Logik Apps Verbinder | Microsoft Azure"
    description="Übersicht über den Verbinder, die in einer app Logik verwendet werden können"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Verwenden von Verbinder in einer app Logik

Verbinder bieten schnellen Zugriff auf Ereignisse, Daten und Aktionen auf Dienste, Protokolle und Plattformen.  Die vollständige Liste der Verbinder, die Logik Apps unterstützt können [hier gefunden werden kann](apis-list.md).  Verbinder als einen Trigger oder eine Aktion in einem Logik-app verwendet werden können, und erfordern möglicherweise eine konfigurierte *Verbindung* verwenden (beispielsweise: autorisieren einen Twitter-Konto für den Zugriff oder in Ihrem Auftrag Posten).

## <a name="basics"></a>Grundlagen

Verbinder sind gehosteten Dienste, die Sie als Teil einer app Logik zur Integration mit anderen Diensten wie Dynamics, Azure, Vertrieb, [und weitere](apis-list.md)zugreifen können.  Sie sind bereitgestellt und verwaltet von Microsoft, damit Sie Ihre Workflows Integration mit Skalierung, Durchsatz und Sicherheit beachtet erstellen können.  Sie können einen Verbinder zu einer app Logik hinzufügen, indem Sie suchen und Auswählen einer Aktion Verbinder oder unter **Anzeigen von Microsoft verwaltete APIs**auslösen.

![Aktionsmenü zum Auslösen auswählen][1]

Jede Aktion Verbinder oder Trigger haben einen Satz von Eigenschaften konfigurieren.  Sie können klicken Sie auf die Schaltflächen Informationen erfahren Sie mehr über die Aktion oder deren Dokumentation [Weitere Informationen](apis-list.md)zu verweisen.

Wenn Sie mit einem anderen Dienst oder API, die nicht noch einen Verbinder, Sie können auch Logik apps über einen [benutzerdefinierten Verbinder erweitern](../app-service-logic/app-service-logic-create-api-app.md) oder nur direkt an den Dienst über ein Protokoll wie HTTP ist, integrieren möchten.

## <a name="triggers"></a>Trigger

Einige Verbinder haben Trigger, d. h. ein Ereignisses aus dieser Verbinder wird ausgelöst, eine app Logik und übergeben Daten als Teil der Trigger.  Ein Trigger ist immer der erste Schritt in einer app Logik.  Beliebte Trigger, zählen Operationen wie:
 
 * Serie - stündlich ausführen
 * Wenn eine HTTP-Anforderung empfangen wird
 * Wenn ein Element einer Warteschlange hinzugefügt wird
 * Wenn eine e-Mail-Nachricht empfangen wird
 
Einige Trigger der Chat, die über eine Benachrichtigung bei der app Logik ein Ereignis stattfindet, und andere Sprachmodule benötigen Sie eine Serienintervall auf, wie oft die app Logik den Dienst für ein Ereignis (auf alle 15 Sekunden) prüft konfiguriert.  

Wenn Sie ein Ereignis empfangen, die Logik app ausführen ausgelöst, und die Aktionen in der Workflow werden gestartet.  Sie werden auch auf alle Daten aus der Trigger in der gesamten Workflows zugreifen (zum Beispiel wird der Trigger 'auf einer neuen Tweet' der Tweet in ausführen übergeben).

## <a name="actions"></a>Aktionen

Die meisten Connectors verfügen über eine oder mehrere Aktionen, die als Teil der Workflow ausgeführt werden können.  Aktionen werden alle Schritte, die ausgeführt werden, nach dem Ausführen von einem Trigger ausgelöst.  Zum Hinzufügen einer Aktion klicken Sie auf die Schaltfläche **Neue Schritt** und suchen Sie nach der Verbinder verwenden möchten.  Sobald ausgewählt (und Nachher konfigurieren alle [Verbindungen](#connections) , die möglicherweise erforderlich) sehen Sie die Karte Aktion, die Sie konfigurieren können.  Sie können wählen Sie Daten aus den vorherigen Schritten, indem Sie auf eines der Token für Ausgaben aus, oder geben in einer anderen Konfiguration nach Bedarf.

![Konfigurieren einer Aktion Verbinder][2]

## <a name="connections"></a>Verbindungen

Die meisten Connectors ist es erforderlich, eine *Verbindung* zu konfigurieren, bevor Sie den Verbinder verwenden können.  Eine *Verbindung* ist keine Anmeldung oder Verbindung-Konfiguration für den Zugriff auf den Verbinder erforderlich.  Für Verbinder, die OAuth verwenden, erstellen Sie eine Verbindung bedeutet bei der Anmeldung in den Dienst (wie Office 365, Vertrieb oder GitHub), in dem Ihre Access-Token verschlüsselt und sicher in einer Azure geheimen Store gespeichert werden kann.  Andere Connectors (wie FTP und SQL) erfordern eine Verbindung, die Konfiguration wie Adresse des Servers, Benutzername und Kennwort enthält.  Diese Verbindung Konfigurationsdetails auch verschlüsselt und sicher gespeichert.  Verbindungen werden auf den Dienst zugreifen, solange der Dienst ermöglicht.  Für Azure Active Directory OAuth Verbindungen (wie Office 365 und Dynamics) können wir weiterhin das Access-Token endlos zu aktualisieren.  Andere Dienste möglicherweise Grenzwerte setzen, wie lange wir ohne das aktualisiert wird ein Token verwenden können.  Im Allgemeinen werden bestimmte Aktionen wie das Ändern des Kennworts alle Access Token ungültig.  

Verbindungen können angezeigt und in Azure verwaltet werden, indem Sie auf **Durchsuchen** und **API Verbindungen**werden.  Aus der Ressource API-Verbindungen können Sie anzeigen, bearbeiten, aktualisieren oder erneut zu autorisieren alle Verbindungen, die Sie erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihrer erste Logik-app](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Erfahren Sie häufig verwendet und Beispiele für Logik apps](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Erste Schritte mit Enterprise Integration Trigger und Aktionen](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
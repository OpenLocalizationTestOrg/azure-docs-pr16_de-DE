<properties
    pageTitle="Sicherheit für Benachrichtigung Hubs"
    description="In diesem Thema wird erläutert, Sicherheit für Azure Benachrichtigung Hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Sicherheit

##<a name="overview"></a>(Übersicht)

Dieses Thema beschreibt das Sicherheitsmodell von Azure Benachrichtigung Hubs. Da Benachrichtigung Hubs eine Entität Dienstbus sind, implementieren sie dasselbe Sicherheitsmodell wie Dienstbus aus. Weitere Informationen finden Sie unter den Themen [Service Bus Authentifizierung](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Freigegebene Signatur Zugriffsschutz (SAS) 

Benachrichtigung Hubs implementiert ein Schema für die Sicherheit auf Benutzerebene Entität aufgerufen SAS (Access-Signatur freigegeben). Dieses Schema ermöglicht messaging Elemente aus, um bis zu 12 Autorisierungsregeln in deren Beschreibung deklarieren, die auf dieser Entität Rechte gewährt.

Jede Regel enthält einen Namen, einen Schlüsselwert (Freigegebene Geheimnis) und eine Reihe von rechten, wie im Abschnitt "Sicherheitsansprüche." erläutert. Wenn Sie eine Benachrichtigung Hub zu erstellen, werden automatisch zwei Regeln erstellt: eine Abhören Rechte (die die Client-app verwendet) und eine mit der alle Rechte (die die app Back-End-verwendet).

Bei der Registrierung Management mithilfe Client-apps, ausgeführt, wenn die Informationen über gesendet Benachrichtigungen nicht vertrauliche (z. B. Wetter Updates), ein gängiges Verfahren zum Zugreifen auf eine Benachrichtigung Hub den Schlüsselwert, der Regel Abhören schreibgeschützten Zugriff auf die app Client gewähren, und geben Sie den Schlüsselwert, der die Regel Vollzugriff auf die app Back-End.

Es wird nicht empfohlen, dass Sie den Schlüsselwert in Windows Store-Client-apps einbetten. Eine Möglichkeit, vermeiden Sie das Einbetten von des Schlüsselwertes besteht darin, ihn aus der app Back-End-beim Start Abrufen der Clientanwendung haben.

Es ist wichtig zu verstehen, dass Abhören Zugriff auf der Schlüssel eine Client-app für eine beliebige Kategorie registrieren ermöglicht. Wenn Ihre app einschränken muss Registrierungen auf bestimmte Kategorien, um bestimmte Clients (z. B. wenn Tags Benutzer-IDs darstellen), muss Ihre app Back-End-Registrierungen durchführen. Weitere Informationen finden Sie unter Verwaltung der Registrierung. Beachten Sie, dass auf diese Weise die Client-app keine direkten Zugriff auf die Benachrichtigung Hubs haben.

##<a name="security-claims"></a>Sicherheitsansprüche

Für andere Personen ähnliche, Benachrichtigung Hub Vorgänge sind für drei Sicherheitsansprüche zulässig: abhören, zu senden und zu verwalten.

| Anfordern | Beschreibung | Vorgänge zulässig. |
|-------|-------------|--------------------|
| Abhören | Erstellen/aktualisieren, lesen und Löschen von einzelnen Registrierungen | Registrierung erstellen/aktualisieren.<br><br>Lesen der Registrierung<br><br>Lesen Sie alle Registrierungen für einen Ziehpunkt<br><br>Löschen der Registrierung |
| Senden | Senden von Nachrichten an den Hub Benachrichtigung | Senden der Nachricht |
| Verwalten | CRUDs Benachrichtigung Hubs (einschließlich aktualisieren PNS Anmeldeinformationen und Sicherheit Tasten), und finden Sie hier Registrierungen, die auf Grundlage von tags | Erstellen, aktualisieren und gelesen/löschen Benachrichtigung hubs<br><br>Lesen von Registrierungen nach tag |


Benachrichtigung Hubs akzeptieren gewährt werden, indem Sie Microsoft Azure Access Control Token und Signaturtoken generiert mit freigegebenen Schlüsseln direkt auf die Benachrichtigung Hub konfiguriert ist.
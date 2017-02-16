<properties 
    pageTitle="Remote Desktop Gateway und Azure mehrstufige Authentifizierung-Server mit RADIUS"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die Bereitstellung von Remote Desktop (RD) Gateway und Azure mehrstufige Authentifizierung-Server mit RADIUS dabei helfen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Remote Desktop Gateway und Azure mehrstufige Authentifizierung-Server mit RADIUS

In vielen Fällen verwendet Remote Desktop-Gateway der lokalen NPS zur Benutzerauthentifizierung an. Dieses Dokument beschreibt, wie RADIUS, aus der Remote Desktop-Gateway (über die lokale NPS) mehrstufige Authentifizierung Server weitergeleitet.

Die kombinierte Authentifizierungsserver sollte auf einem separaten Server, die dann Proxy die RADIUS-Anforderung wieder in der NPS auf dem Desktop Gateway Remoteserver wird installiert werden. Nachdem NPS Benutzername und Kennwort überprüft wurde, wird den zweiten Faktor der Authentifizierung, bevor Sie ein Ergebnis zurückgibt, mit dem Gateway führt eine Antwort auf die kombinierte Authentifizierungsserver zurückgegeben.





## <a name="configure-the-rd-gateway"></a>Konfigurieren des RD-Gateways

Das RD-Gateway muss konfiguriert sein, um RADIUS-Authentifizierung an einem Azure mehrstufige Authentifizierung-Server zu senden. Nach der Installation von RD-Gateway konfiguriert und funktioniert, wechseln Sie in den Eigenschaften RD-Gateway ist. Wechseln Sie zur Registerkarte RD Linienende Store, und ändern Sie, um einen zentralen Netzwerkrichtlinienserver anstelle von lokalen Netzwerkrichtlinienserver verwenden. Fügen Sie einen oder mehrere Azure mehrstufige Authentifizierungsserver als RADIUS-Server und geben Sie einen geheimen für jeden Server.





## <a name="configure-nps"></a>Konfigurieren von NPS

Das RD-Gateway verwendet NPS, um die RADIUS-Anforderung an Azure mehrstufige Authentifizierung zu senden. Ein Timeout muss geändert werden, um zu verhindern, dass das RD Gateway aus überschritten wurde, bevor kombinierte Authentifizierung durchgeführt wurde. Gehen Sie folgendermaßen vor, um NPS konfigurieren.

1. In NPS erweitern Sie des Menüs RADIUS-Clients und Server in der linken Spalte aus, und klicken Sie auf Remote-RADIUS-Server-Gruppen. Wechseln Sie in den Eigenschaften die Gruppe der Terminaldienste GATEWAY-SERVER. Bearbeiten Sie die RADIUS-Servern angezeigt, und wechseln Sie zur Registerkarte Lastenausgleich. "Ausgelassen Anzahl von Sekunden ohne Antwort, bevor Anforderung gilt" ändern und die "Anzahl von Sekunden zwischen Anfragen beim Server als nicht verfügbar identifiziert wird" auf 30-60 Sekunden. Klicken Sie auf der Registerkarte Authentifizierung/Konto auf, und stellen Sie sicher, dass angegebenen RADIUS Ports die Ports, denen übereinstimmen auf dem mehrstufige Authentifizierungsserver überwacht.
2. NPS muss auch zum Empfangen von RADIUS-Authentifizierung wieder vom Server Azure mehrstufige Authentifizierung konfiguriert sein. Klicken Sie im linken Menü auf RADIUS-Clients auf. Fügen Sie dem Azure mehrstufige Authentifizierungsserver als RADIUS-Client hinzu. Wählen Sie einen Anzeigenamen ein, und geben Sie einen geheimen.
3. Erweitern Sie den Abschnitt Richtlinien im linken Navigationsbereich, und klicken Sie auf Verbindung anfordern Richtlinien. Es sollte eine Verbindung Anforderungsrichtlinie aufgerufen Terminaldienste GATEWAY AUTORISIERUNGSRICHTLINIE erstellt wurde, wenn RD-Gateway konfiguriert wurde enthalten. Dieser Richtlinie leitet RADIUS-Anfragen auf den Server mehrstufige Authentifizierung.
4. Kopieren Sie diese Richtlinie, um eine neue zu erstellen. Fügen Sie in der neuen Richtlinie eine Bedingung, die den Anzeigenamen von Clients mit dem Anzeigenamen entspricht in Schritt 2 weiter oben für den Azure mehrstufige Authentifizierung Server RADIUS-Client festlegen. Ändern Sie den Authentifizierungsanbieter auf dem lokalen Computer an. Dieser Richtlinie ist sichergestellt, dass beim RADIUS-Anforderung vom Server Azure mehrstufige Authentifizierung empfangen wird, die Authentifizierung erfolgt lokal statt zurücksenden RADIUS-Anforderung an den Server Azure mehrstufige Authentifizierung die Bedingung Schleife bedingt. Um zu verhindern, dass die Bedingung Schleife, muss diese neue Richtlinie oberhalb der ursprünglichen angeordnet werden, die an den mehrstufige Authentifizierungsserver weiterleitet.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurieren von Azure kombinierte Authentifizierung


--------------------------------------------------------------------------------



Der Server Azure mehrstufige Authentifizierung wird als RADIUS-Proxy zwischen RD Gateway und NPS konfiguriert.  Es sollte auf einem Server Domänenverbund installiert werden, die vom Server RD-Gateway getrennt ist. Gehen Sie folgendermaßen vor, um Azure mehrstufige Authentifizierungsserver zu konfigurieren.

1. Öffnen Sie den Azure mehrstufige Authentifizierungsserver, und klicken Sie auf das Symbol RADIUS-Authentifizierung. Kontrollkästchen Sie das Aktivieren RADIUS-Authentifizierung.
2. Klicken Sie auf der Registerkarte Clients stellen Sie sicher, dass die Ports entsprechen, was in NPS konfiguriert ist, und klicken Sie auf Hinzufügen... Schaltfläche. Fügen Sie die RD-Gateway-Server-IP-Adresse, Name der Anwendung (optional) und ein gemeinsames Kennwort ein. Das gemeinsame Kennwort müssen auf den Azure mehrstufige Authentifizierung Server- und RD-Gateway identisch sein.
3. Klicken Sie auf der Registerkarte Ziel, und wählen Sie das Optionsfeld für RADIUS-Servern.
4. Klicken Sie auf Hinzufügen... Schaltfläche. Geben Sie die IP-Adresse, freigegebenen geheim und Ports des Netzwerkrichtlinienservers. Es sei denn, einer zentralen NPS verwenden, werden der RADIUS-Client und RADIUS Ziel identisch sein. Das gemeinsame Kennwort muss die eine Einrichtung im Abschnitt Client RADIUS des Netzwerkrichtlinienservers übereinstimmen.

![RADIUS-Authentifizierung](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

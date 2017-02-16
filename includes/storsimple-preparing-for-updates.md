<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Vorbereiten für updates
Sie müssen die folgenden Schritte ausführen, bevor Sie es scannen und das Update zu installieren:


1. Eine Cloud Momentaufnahme der Gerätedaten.

2. Sicherstellen Sie, dass Ihre IP-Adressen behoben Controller weitergeleitet werden und mit dem Internet verbinden können. Updates auf Ihr Gerät zu bedienen, wird diese feste IP-Adressen verwendet werden. Sie können dies testen, indem Sie das folgende Cmdlet auf jedem Controller aus der Windows PowerShell-Benutzeroberfläche des Geräts ausführen:

    `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `

    **Beispiel für die Ausgabe für Test-Verbindung beim feste IP-Adressen eine Verbindung mit dem Internet herstellen können**


        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source    Destination   IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200
        HCSNODE0  bing.com      204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source    Destination     IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Nachdem Sie diese manuelle Pre überprüft wurde erfolgreich abgeschlossen haben, können Sie fortfahren und Scannen und Updates installieren.

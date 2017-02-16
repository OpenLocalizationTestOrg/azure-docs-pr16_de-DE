##<a name="highly-available-solutions-with-azure-traffic-manager"></a>Hochgradig verfügbaren Lösungen mit Azure Datenverkehr-manager

Sie müssen entscheiden, ob Ihre Arbeitsbelastung der hohen Verfügbarkeit erfüllt sein können mithilfe von alleine Azure Datenverkehr-Manager, oder wenn Sie den Datenverkehr Manager mit anderen DNS-Lösungen oder Prozesse kombinieren müssen. Je nach Ihren Anforderungen können Sie Folgendes verwenden:

- **Datenverkehr Manager alleine**. Wenn ein 99,99 %, um Zeit für Ihre Arbeitsbelastung ausreicht, können Sie den Datenverkehr Manager allein verwenden. Bei einem Fehler im Dienst-Manager den Datenverkehr werden Benutzer nicht auf Ihre Arbeitsbelastung zugreifen, bis der Datenverkehr-Dienst-Manager erneut hergestellt wird.

- **Verwenden einer anderen Datenverkehr Manager-Lösung zusammen mit Azure Datenverkehr-Manager**. Bei einem Fehler in der Datenverkehr-Dienst-Manager können Sie Ihre CNAME-Eintrag für den Datenverkehr Managerdienst verweisen ändern. Zugriff auf Ihre Arbeitsbelastung ist jedoch verfügbar, und an allen Standorten Hostinganbieter Ihrer Arbeitsbelastung verteilt. Dies ist die am häufigsten teure Lösung, aber für Auslastung, die eine höhere Vereinbarung zum SERVICELEVEL benötigen kann erforderlich sein.

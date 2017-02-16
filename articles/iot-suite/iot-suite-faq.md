<properties
  pageTitle="Häufig gestellte Fragen zur Azure IoT-Suite | Microsoft Azure"
  description="Häufig gestellte Fragen zu IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Häufig gestellte Fragen zu IoT Suite

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Was ist der Unterschied zwischen eine Ressourcengruppe Azure-Portal löschen und dann auf Löschen, klicken Sie auf einer vorkonfigurierten-Lösung in azureiotsuite.com?

- Wenn Sie die vorkonfigurierte Lösung in [azureiotsuite.com]löschen[lnk-azureiotsuite], löschen Sie alle Ressourcen, die bei der Erstellung der vorkonfigurierten Lösung; bereitgestellt wurden Wenn Sie zusätzliche Ressourcen der Ressourcengruppe hinzugefügt haben, werden diese ebenfalls gelöscht. 

- Wenn Sie die Ressourcengruppe im [Portal Azure]löschen[lnk-azure-portal], Sie nur die Ressourcen in dieser Ressourcengruppe; löschen Sie müssen außerdem die zugeordnete der vorkonfigurierten Lösung im [Azure klassischen Portal]Azure Active Directory-Anwendung löschen[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Wie viele IoT Hub Instanzen kann ich in einem Abonnement bereitstellen? 

Zehn. Sie können eine [Azure unterstützen Ticket] erstellen[ link-azuresupportticket] zum Auslösen dieser Grenzwert, aber standardmäßig können Sie nur zehn IoT Hubs pro Abonnement, bereitstellen, wie [Azure-Abonnement Grenzwerte]umrandet[link-azuresublimits]. Daher, da jeder vorkonfigurierten Lösung eine neue IoT Hub Vorschriften, können Sie nur bis zu zehn vorkonfigurierte Lösungen in einem angegebenen Abonnement bereitstellen. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Wie viele DocumentDB Instanzen kann ich in einem Abonnement bereitstellen?

50. Sie können eine [Azure unterstützen Ticket] erstellen[ link-azuresupportticket] zum Auslösen dieser Grenzwert, aber standardmäßig können Sie nur 50 DocumentDB Instanzen pro Abonnement bereitstellen. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele kostenlose Bing Maps-APIs, die können ich in einem Abonnement bereitstellen?

Zwei. Sie können nur zwei internen Transaktionen Ebene 1 Bing Maps für Enterprise-Pläne in einem Azure-Abonnement erstellen. Die Überwachung remote-Lösung ist standardmäßig mit den internen Transaktionen Ebene1 Plan nach der Bereitstellung. Sie können bis zu zwei remote Überwachung Solutions in einem Abonnement mit keine Änderungen daher nur bereitstellen.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Habe ich eine remote Überwachung Lösung Bereitstellung mit eine statische Karte, wie füge ich einer interaktiven Bing-Karte? 
1. Holen Sie Ihre Bing Maps-API für Enterprise QueryKey [Azure-Portal][lnk-azure-portal]: 
 1. Navigieren Sie zu der Stelle, an der der Bing Maps-API für Enterprise im [Portal Azure ist]Ressourcengruppe[lnk-azure-portal].
 2. Klicken Sie auf alle Einstellungen, klicken Sie dann Key-Verwaltung. 
 3. Sehen Sie zwei Tasten: MasterKey und QueryKey. Kopieren Sie den Wert für QueryKey aus.

     > [AZURE.NOTE] Haben Sie noch eine Bing Maps-API für Enterprise-Konto? Erstellen Sie eine im [Portal Azure] [ lnk-azure-portal] von auf + neue, Suchen nach Bing Maps-API für Enterprise und geben Sie nach Aufforderung zu erstellen.

2. Ziehen Sie aus der [Azure-IoT-Remote-Überwachung]ab der aktuellen Code[lnk-remote-monitoring-github].

3. Ausführen ein lokalen oder cloud Bereitstellung Befehlszeile Bereitstellung Anleitungen im Ordner im Repository /docs/ folgen. 

4. Nachdem Sie eine lokale ausgeführt haben oder cloud-Bereitstellung, suchen im Stammordner für die *. user.config-Datei, die während der Bereitstellung erstellt. Öffnen Sie diese Datei in einem Text-Editor ein. 

5. Ändern Sie die folgende Zeile, um den Wert enthalten, den Sie in Ihrem QueryKey kopiert haben: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich Microsoft Azure für DreamSpark habe?
Sie können keine vorkonfigurierte Lösung zu diesem Zeitpunkt erstellt, mit einem [Microsoft Azure für DreamSpark] [ lnk-dreamspark] Konto. Sie können jedoch eine [kostenlose Testversion Konto Azure] erstellen[ lnk-30daytrial] in nur ein paar Minuten, mit deren Hilfe Sie eine vorkonfigurierte Lösung erstellen.

### <a name="how-do-i-delete-an-aad-tenant"></a>Wie lösche ich eine AAD Mandanten?

Finden Sie unter Eric Golpes Blogbeitrag [Exemplarische Löschen einer Azure AD-Mandanten][lnk-delete-aad-tennant].

## <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen von den IoT Suite vorkonfiguriert Lösungen vertraut machen:

- [Vorhersagen Wartung vorkonfiguriert Lösung (Übersicht)][lnk-predictive-overview]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx

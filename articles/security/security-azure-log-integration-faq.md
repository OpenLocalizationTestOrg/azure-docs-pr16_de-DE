<properties
   pageTitle="Häufig gestellte Fragen zur Azure Log-Integration | Microsoft Azure"
   description="Hier finden Sie Antworten auf Fragen zur Azure Log Integration."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) Azure Log-integration

Hier finden Sie Antworten auf Fragen zur Azure Log Integration, ein Dienst, der Sie unformatierte Protokollen aus Azure Ressourcen in Ihrem lokalen Informationen zu Sicherheit und Ereignis Management (SIEM) Systeme integrieren kann. Diese Integration bietet ein einheitliches Dashboard für alle Anlagen, lokal oder in der Cloud, damit Sie aggregieren, koordinieren, analysieren und für Sicherheitsereignisse im Zusammenhang mit der Anwendung benachrichtigen.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Wie kann ich feststellen, die Speicherkonten aus denen Integration von Azure Log Azure-virtuellen Computer Protokolle aus herausziehen ist?

Führen Sie den Befehl **Azlog Quellliste**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Wie kann ich die Proxy-Konfiguration aktualisieren?

Wenn Ihre Proxyeinstellung Azure-Speicher Access nicht direkt zulässt, öffnen Sie die **AZLOG. EXE-DATEI. CONFIG** Datei in **c:\Programme\Gemeinsame Dateien\Microsoft Azure Log Integration**. Aktualisieren Sie die Datei, um den Abschnitt **DefaultProxy** mit der Proxyadresse Ihrer Organisation gehören. Nach dem Aktualisieren beenden Sie, und starten Sie den Dienst mithilfe von Befehlen **Azlog Netto beenden** und **Azlog Netto starten**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Wie kann ich die Abonnementinformationen in Windows-Ereignisse anzeigen?

Fügen Sie die **Subscriptionid** an den Anzeigenamen beim Hinzufügen von der Quelle.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Das Ereignis XML hat die Metadaten aus, wie unten dargestellt, wozu auch die Personalnummer des Abonnements.

![XML-Ereignis][1]

## <a name="error-messages"></a>Fehlermeldungen

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Wenn der Befehl **Azlog Createazureid**ausführen zu können, Warum erhalte den folgenden Fehler ich?

Fehler:

  *Fehler beim Erstellen von AAD Anwendung - Mandanten 72f988bf-86f1-41af-91ab-2d7cd011db37-Grund = 'Verboten' - Nachricht = 'Keine ausreichenden Berechtigungen zum Abschließen des Vorgangs'.*

**Azlog Createazureid** versucht, einen Dienst Tilgungsanteile in alle Azure AD-Mandanten für Abonnements erstellen auf dem das Azure Login auf zugreifen kann. Ist Ihre Azure Anmeldung nur ein Gastbenutzer in dieser Azure AD-Mandanten, schlägt fehl, klicken Sie dann der Befehl mit 'Keine ausreichenden Berechtigungen zum Abschließen des Vorgangs'. Anfordern des mandantenadministrator aus, um Ihr Konto in den Mandanten als Benutzer hinzufügen.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Wenn der Befehl **Azlog autorisieren**ausgeführt, Warum erhalte den folgenden Fehler ich?

Fehler:

  *Erstellen von Rollenzuweisung - AuthorizationFailed Warnung: der Client janedo@microsoft.com' mit Objekt Id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' hat keine Autorisierung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich '/ Abonnements/70 d 95299-d689-4c 97-b971-0d8ff0000000'.*

Befehl **Azlog autorisieren** weist die Rolle des Reader zu den wichtigsten Azure AD-Dienst (erstellt mit **Azlog Createazureid**) die Abonnements zur Verfügung gestellt. Wenn die Azure keine Anmeldung eine gemeinsame Administrator oder einen Besitzer des Abonnements ist, schlägt fehl, es mit der Fehlermeldung 'Fehler beim Autorisierung'. Azure rollenbasierte Access-Steuerelement (RBAC) gemeinsame Administrator oder Besitzer ist erforderlich, um den Vorgang abzuschließen.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Wo finde ich die Definition der Eigenschaften im Überwachungsprotokoll?

Siehe:

- [Überwachen von Vorgängen mit Ressourcenmanager](../resource-group-audit.md)
- [Listet die Verwaltungsereignisse in einem Abonnement in Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Wo finde ich die Details auf Sicherheitscenter Azure-Benachrichtigungen?

Finden Sie unter [Verwaltung und Beantworten von Sicherheitshinweisen in Azure Sicherheitscenter](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Wie kann ich ändern, was mit virtueller Computer Diagnose gesammelt?

Einzelheiten zum Abrufen, ändern, finden Sie unter [Verwenden von PowerShell in einen virtuellen Computer mit Windows Azure-Diagnose aktivieren](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) , und legen Sie die Diagnose Azure in Windows *(WAD)* Konfiguration. Es folgt ein Beispiel:

### <a name="get-the-wad-config"></a>Abrufen der WAD config

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Ändern der WAD Config

Im folgende Beispiel ist eine Konfiguration, in dem nur EventID 4624 und EventId 4625 aus dem Sicherheitsereignisprotokoll erfasst. Microsoft Antimalware Ereignisse werden aus dem System-Ereignisprotokoll erfasst. Finden Sie unter [Verarbeitung Ereignisse] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) ausführliche Informationen über die Verwendung von XPath-Ausdrücken.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Legen Sie die Konfiguration der WAD

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Aktivieren Sie nachdem Sie Änderungen vorgenommen haben das Speicherkonto, um sicherzustellen, dass die richtigen Ereignisse erfasst wurden.

Wenn Sie Fragen zur Azure Log Integration haben, senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png

## <a name="overview-of-azure-resource-manager-templates"></a>Übersicht über Ressourcenmanager Azure-Vorlagen

Azure Ressourcenmanager Vorlagen können Sie die Infrastruktur Azure IaaS in Json-Sprache deklarativ festlegen möchten, indem Sie die Abhängigkeiten zwischen Ressourcen definieren. Ein detaillierter Überblick Azure Ressourcenmanager Vorlagen finden Sie im folgenden Artikel:

[Ressourcengruppe (Übersicht)](../articles/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Beispiel für Vorlage Codeausschnitt für Erweiterungen virtueller Computer
Bereitstellen von virtuellen Computer Erweiterungen als Teil einer Azure Ressourcenmanager erfordert Vorlage in der Vorlage deklarativ die Erweiterung Konfiguration festlegen.
So sieht das Format zum Festlegen der Erweiterung Konfigurations aus.

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

Wie Sie die von den oben genannten sehen können, enthält die Erweiterung Vorlage zwei Hauptteilen:

1. Name der Erweiterung, Publisher und version
2. Erweiterung Konfiguration.

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a>Bestimmen Sie Publisher, Typ und TypeHandlerVersion für jede Erweiterung

Azure-virtuellen Computer-Erweiterungen werden von Microsoft veröffentlicht und vertrauenswürdige 3rd Party Herausgeber und jede Erweiterung eindeutig durch den Herausgeber, Typ und die TypeHandlerVersion. Dies können wie folgt ermittelt werden:  
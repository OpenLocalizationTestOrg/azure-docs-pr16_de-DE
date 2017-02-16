Verwenden Sie zum Hinzufügen einer Kategorie zu einer Ressourcengruppe **Azure Gruppe festlegen**. Wenn die Ressourcengruppe keine vorhandenen Kategorien verfügt, werden in der Kategorie übergeben.

    azure group set -n tag-demo-group -t Dept=Finance

Kategorien werden als Ganzes aktualisiert. Wenn Sie eine Kategorie zu einer Ressourcengruppe hinzufügen, die vorhandene Tags enthält möchten, übergeben Sie alle Tags. 

    azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade

Kategorien werden von Ressourcen in einer Ressourcengruppe nicht geerbt. Verwenden Sie zum Hinzufügen einer Kategorie zu einer Ressource **Azure Ressource festlegen**. Sie müssen die Versionsnummer API für den Ressourcentyp übergeben, denen Sie die Kategorie auf Hinzufügen. Wenn Sie die Version API abgerufen werden müssen, verwenden Sie den folgenden Befehl mit der Anbieter für Ressourcen für den Typ, den Sie festlegen:

    azure provider show -n Microsoft.Storage --json

Suchen Sie in den Suchergebnissen nach den gewünschten Ressourcentyp.

    "resourceTypes": [
    {
      "resourceType": "storageAccounts",
      ...
      "apiVersions": [
        "2016-01-01",
        "2015-06-15",
        "2015-05-01-preview"
      ]
    }
    ...

Nun Bereitstellen dieser Version API Gruppe Ressourcenname, Ressourcenname, Ressourcenart und Tagwert als Parameter.

    azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01

Kategorien, die direkt auf Ressourcen und Ressourcengruppe vorhanden sein. Zum Anzeigen der vorhandenen Kategorien erhalten Sie einfach eine Ressourcengruppe und seine Ressourcen mit **Azure Gruppe anzeigen**.

    azure group show -n tag-demo-group --json
    
Die Metadaten für die Ressourcengruppe, einschließlich beliebige Tags angewendet zurückgibt.
    
    {
      "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
      "name": "tag-demo-group",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "location": "southcentralus",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production",
        "Project": "Upgrade"
      },
      ...

Sie können die Tags für eine bestimmte Ressource mithilfe von **Azure Ressource anzeigen**anzeigen.

    azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
    
Wenn Sie alle Ressourcen, die mit einem Tagwert abrufen möchten, verwenden:

    azure resource list -t Dept=Finance --json
    
Rufen Sie die Ressourcengruppen mit einem Wert Kategorie verwenden:

    azure group list -t Dept=Finance
        
Sie können vorhandene Tags in Ihrem Abonnement mit den folgenden Befehl anzeigen:

    azure tag list


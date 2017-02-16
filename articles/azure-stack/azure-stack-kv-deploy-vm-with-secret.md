<properties
    pageTitle="Bereitstellen ein virtuellen Computers mit einem Kennwort Azure Stapel Schlüssel Tresor gehörende Kehrmatrix | Microsoft Azure"
    description="Weitere Informationen zum Bereitstellen eines virtuellen Computers mit einem Kennwort in Azure Stapel Schlüssel Tresor gespeichert"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Bereitstellen eines virtuellen Computers durch das gespeicherte Kennwort in Tresor Schlüssel abrufen

Wenn einen sicheren Wert (wie etwa ein Kennwort) als Parameter während der Bereitstellung erforderlich ist, können Sie Wert als Geheimnis in einem Stapel Azure Key Tresor zu speichern und den Wert in anderen Ressourcenmanager Azure-Vorlagen verweisen. Sie einbeziehen nur einen Verweis auf die geheim in Ihre Vorlage, damit die geheim nie verfügbar gemacht wird. Sie nicht manuell den Wert für das Geheimnis jedes Mal eingeben müssen Sie die Ressourcen bereitstellen. Sie angeben, welche Benutzer oder Dienst Hauptbenutzer die geheim zugreifen können.

## <a name="reference-a-secret-with-static-id"></a>Bezug auf ein Geheimnis mit statischen-ID

Das Geheimnis von innerhalb einer Parameterdatei, die Werte in der Vorlage übergibt verwiesen werden. Sie verweisen die geheim und den Resource Identifier für die wichtigsten Tresor sowie den Namen der geheim übergeben. In diesem Beispiel muss die wichtigsten Tresor geheim bereits vorhanden sein. Sie verwenden einen statischen Wert für die Ressource-ID.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Der Parameter, der das Geheimnis akzeptiert sollte eine *Securestring*sein.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen einer Stichprobe app mit Schlüssel Tresor](azure-stack-kv-sample-app.md)

[Bereitstellen eines virtuellen Computers mit einem Zertifikat Tresor-Taste](azure-stack-kv-push-secret-into-vm.md)


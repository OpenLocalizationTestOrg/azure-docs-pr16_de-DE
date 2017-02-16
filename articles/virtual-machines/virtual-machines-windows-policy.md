<properties
    pageTitle="Anwenden von Richtlinien Azure Ressourcenmanager virtuellen Computern | Microsoft Azure"
    description="Anwenden eine Richtlinie zu einer Azure Ressourcenmanager Windows virtuellen Computern"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Anwenden von Richtlinien Azure Ressourcenmanager virtuellen Computern

Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln in der gesamten Organisation erzwingen. Durchsetzung von das gewünschte Verhalten helfen Risiken während der für den Erfolg des Unternehmens beitragen. In diesem Artikel wird erläutert, wie Sie Azure Ressourcenmanager Richtlinien verwenden können, um das gewünschte Verhalten für Ihre Organisation virtuellen Computern definieren.

Die Gliederung für die Schritte hierfür ist als unter

1. Azure Ressourcenmanager Richtlinie 101
2. Definieren einer Richtlinie für Ihre virtuellen Computern
3. Erstellen Sie die Richtlinie
4. Wenden Sie die Richtlinie

## <a name="azure-resource-manager-policy-101"></a>Azure Ressourcenmanager Richtlinie 101

Für die ersten Schritte mit Azure Ressourcenmanager Richtlinien, empfehlen wir, lesen den Artikel aus, und klicken Sie dann mit den Schritten in diesem Artikel fortgesetzt. Im folgenden Artikel beschreibt die grundlegende Definition und Struktur einer Richtlinie, wie Richtlinien ausgewertet abrufen und bietet verschiedene Beispiele für Policy-Definitionen.

* [Verwenden Sie die Richtlinie zum Verwalten von Ressourcen und Steuern des Zugriffs](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definieren einer Richtlinie für Ihre virtuellen Computern

Eine allgemeine Szenarien für ein Unternehmen möglicherweise nur ihre Benutzerberechtigungen zum Erstellen von virtuellen Computern von bestimmter Betriebssystemen, die Kompatibilität mit einer LOB-Anwendung getestet wurden. Verwendung einer Richtlinie Azure Ressourcenmanager kann diese Aufgabe in wenigen Schritten erfolgen. In diesem Beispiel Richtlinie werden wir dürfen nur Windows Server 2012 R2 Datacenter virtuellen Computern erstellt werden. Die Richtliniendefinition sieht wie unten aus.

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Die oben angegebenen Richtlinie kann einfach zu einem Szenario, in dem Sie möglicherweise alle Windows Server Datacenter Bild verwendet werden, für die Bereitstellung eines virtuellen Computers mit zulassen möchten, geändert werden die unter ändern

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>Felder für die Eigenschaften von virtuellen Computern

In der folgenden Tabelle werden die Eigenschaften von virtuellen Computern, die als Felder in der Richtliniendefinition Ihrer verwendet werden können. Weitere Felder für die Richtlinie finden Sie im folgenden Artikel:

* [Felder und Quellen](../resource-manager-policy.md#fields-and-sources)


| Feldname     | Beschreibung                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Gibt den Herausgeber des Bilds               |
| imageOffer     | Gibt an, das Angebot für die ausgewählte Image publisher |
| imageSku       | Gibt die SKU für das ausgewählte Angebot an             |
| imageVersion   | Gibt die Bildversion für den ausgewählten SKU     |

## <a name="create-the-policy"></a>Erstellen Sie die Richtlinie

Eine Richtlinie kann leicht mit den REST-API direkt oder den PowerShell-Cmdlets erstellt werden. Erstellen die Richtlinie, finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Wenden Sie die Richtlinie

Nach dem Erstellen der Richtlinie müssen Sie es auf einen definierten Bereich anwenden. Der Bereich kann ein Abonnement, Ressourcengruppe oder sogar die Ressource sein. Anwenden der Richtlinie, finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../resource-manager-policy.md#applying-a-policy)

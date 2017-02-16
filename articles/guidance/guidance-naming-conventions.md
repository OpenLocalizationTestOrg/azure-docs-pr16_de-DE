<properties
   pageTitle="Empfohlene Benennungskonvention für Ressourcen Azure | Anleitungen | Microsoft Azure"
   description="Empfohlene Benennungskonvention für Azure Ressourcen. So benennen Sie virtuelle Computer, Speicherkonten, Netzwerke, virtuelle Netzwerke, Subnetze und anderen Azure Einheiten"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Empfohlene Benennungskonvention für Azure Ressourcen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Die Auswahl eines Namens für eine Ressource in Microsoft Azure ist wichtig, da:

- Es ist es schwierig, einen Namen später ändern.
- Namen müssen die erfüllen ihre bestimmten Ressourcenart aus.

Konsistente Benennungskonventionen leichter Ressourcen zu suchen. Sie können auch die Rolle einer Ressource in eine Lösung angeben.

In diesem Artikel wird eine Zusammenfassung der Benennungskonventionen und Einschränkungen für Azure Ressourcen und geplante mehrere Empfehlungen für Benennungskonventionen.  Sie können diese Empfehlungen als Ausgangspunkt für eigene bestimmte Konventionen an Ihre Bedürfnisse verwenden.  

Der Schlüssel zum Erfolg mit Benennungskonventionen ist herstellen, und folgen sie über Ihre Applikationen und Organisationen. 

## <a name="naming-subscriptions"></a>Benennen von Abonnements

Benennen von Azure-Abonnements stellen ausführliche Namen Grundlegendes zu den Kontext und den Zweck der jedes Abonnement löschen.  Bei der Arbeit in einer Umgebung mit vielen Abonnements kann die Folgen einer freigegebenen Benennungskonvention Übersichtlichkeit verbessern.

Eine empfohlene Muster für naming Abonnements lautet:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Unternehmen würde normalerweise für jedes Abonnement identisch sein. Einige Unternehmen müssen jedoch untergeordnete Unternehmen innerhalb der Organisationsstruktur. Diese Unternehmen können von einer zentralen IT-Gruppe verwaltet werden. In diesen Fällen könnten sie unterschieden werden, indem die übergeordneten Firmennamen (*"Contoso"*) und die untergeordneten Firmennamen (*North Wind*) Probleme.

- Abteilung ist ein Name innerhalb der Organisation eine Gruppe von Personen arbeiten. Dieses Element innerhalb des Namespace als optional.

- Produktlinie ist, einen bestimmten Namen für ein Produkt oder eine Funktion, die innerhalb der Abteilung aus ausgeführt wird.
Dies ist im Allgemeinen optional für internen Diensten und Anwendungen. Es wird jedoch dringend empfohlen für öffentlich zugängliche Dienste verwenden, die einfach Trennung und Kennung (beispielsweise klare Trennung der Datensätze Abrechnung) erfordern.

- Umgebung ist die aussagekräftigen Namen für die Bereitstellung Lebenszyklus Applikationen oder Dienste, wie etwa Entwickler, f & a oder Prod.

| Unternehmen | Abteilung | Produktlinie oder einen bestimmten Dienst | Umgebung | Vollständiger Name  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Herstellung | Contoso-SocialGaming AwesomeService Herstellung |
| Contoso | SocialGaming | AwesomeService | Entwickler | Contoso SocialGaming AwesomeService Entwickler |
| Contoso | ES | InternalApps | Herstellung | Contoso IT InternalApps Herstellung |
| Contoso | ES | InternalApps | Entwickler | Contoso IT InternalApps Entwickler |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Verwenden von Affixtyp Mehrdeutigkeit zu vermeiden.

Benennen von Ressourcen in Azure empfiehlt es sich häufig Präfixe oder Suffixe verwenden, um die Art und den Kontext der Ressource zu identifizieren.  Während Sie alle Informationen zu geben, Metadaten, Kontext programmgesteuert verfügbar ist, vereinfacht die visuelle Kennung allgemeine Affixtyp anwenden.  Wenn in Ihrer Benennungskonvention Affixtyp einbinden, ist es wichtig, um klar anzugeben, ob das Affix am Anfang des Namens (Präfix) oder am Ende (Suffix) ist.  

Hier sind beispielsweise zwei mögliche Namen für eine Berechnung-Engine Hostinganbieter Dienst:

- SvcCalculationEngine (Präfix)
- CalculationEngineSvc (Suffix)

Affixtyp können auf verschiedene Aspekte verweisen, die die entsprechenden Ressourcen zu beschreiben. Die folgende Tabelle zeigt einige Beispiele für in der Regel verwendet.

| Seitenverhältnis | Beispiel | Notizen |
| ------ | ------- | ----- |
| Umgebung | Entwickler, FA, f & a | Gibt die Umgebung für die Ressource |
| Speicherort | UW (uns Westen), Ue (uns OST) | Identifiziert den Bereich, in dem die Ressourcen bereitgestellt wird |
| Instanz | 01, 02 | Für Ressourcen verfügen, die mehrere benannte Instanz (Webservern usw.). |
| Produkt oder Service | Dienst | Identifiziert das Produkt, Anwendung oder Dienst, der die Ressource unterstützt |
| Rolle | SQL, web, messaging | Gibt die Rolle der die zugeordnete Ressource |

Bei der Entwicklung einer spezielle Benennungskonvention für Ihr Unternehmen oder Projekten ist es wichtiger: Wählen Sie einen gemeinsamen Satz von Affixtyp und deren Position (Suffix oder Präfix).

## <a name="naming-rules-and-restrictions"></a>Benennen von Regeln und Einschränkungen

Geben Sie jeder Ressource oder einen bestimmten Dienst Azure erzwingt eine Reihe von Einschränkungen und Bereich benennen; eine Benennungskonvention oder Muster muss die erforderliche Benennungskonventionen und Umfang einhalten.  Während der Name eines virtuellen Computers einem DNS-Namen ordnet (und daher für alle Azure eindeutig sein erforderlich ist), wird der Namen eines VNET beispielsweise der Ressourcengruppe beschränkt, die sie in erstellt wurde.

Im Allgemeinen zu vermeiden, dass keine Sonderzeichen (`-` oder `_`) als die erste oder letzte Zeichen in einem beliebigen Namen. Diese Zeichen werden die meisten Gültigkeitsprüfungsregeln zum Fehlschlagen verursachen.

| Kategorie | Dienst oder juristische | Bereich | Länge | Zur Groß-und Kleinschreibung | Gültige Zeichen | Vorgeschlagener Muster | Beispiel |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Ressourcengruppe | Ressourcengruppe | Globale | 1-64 | Der Groß-/Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Ressourcengruppe | Festlegen der Verfügbarkeit | Ressourcengruppe | 1-80 | Der Groß-/Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Allgemeine | Kategorie | Zugeordnete Entität | 512 (Name), 256 (Wert) | Der Groß-/Kleinschreibung | Alphanumerische | `"key" : "value"` | `"department" : "Central IT"` |
| Berechnen | Virtuellen Computern | Ressourcengruppe | 1-15 | Der Groß-/Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<name>-<role>-<instance>` | `profx-sql-001` |
| Speicher | Speicher Kontonamen (Daten) | Globale | 3-24 | Kleinbuchstaben | Alphanumerische | `<service short name><type><number>` | `profxdata001` |
| Speicher | Speicher Kontonamen (Datenträger) | Globale | 3-24 | Kleinbuchstaben | Alphanumerische | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Speicher | Container mit dem Namen | Speicher-Konto | 3-63 |   Kleinbuchstaben | Alphanumerisch und Gedankenstrich | `<context>` | `logs` |
| Speicher | BLOB-name | Container | 1-1024 | Groß-/Kleinschreibung | Eine URL-Zeichen | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Speicher | Name der Warteschlange | Speicher-Konto | 3-63 | Kleinbuchstaben | Alphanumerisch und Gedankenstrich | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Speicher | Tabellenname | Speicher-Konto | 3-63 |Der Groß-/Kleinschreibung | Alphanumerische | `<service short name>-<context>` | `awesomeservice-logs` |
| Speicher | Dateiname | Speicher-Konto | 3-63 | Kleinbuchstaben | Alphanumerische | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Netzwerke | Virtuelle Netzwerk (VNet) | Ressourcengruppe | 2-64 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service short name>-[section]-vnet` | `profx-vnet` |
| Netzwerke | Subnetz | Übergeordnete VNet | 2-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<role>-subnet` | `gateway-subnet` |
| Netzwerke | Netzwerk-Benutzeroberfläche | Ressourcengruppe | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Netzwerke | Netzwerk-Sicherheitsgruppe | Ressourcengruppe | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Netzwerke | Netzwerk-Sicherheitsgruppe Regel | Ressourcengruppe | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<descriptive context>` | `sql-allow` |
| Netzwerke | Öffentliche IP-Adresse | Ressourcengruppe | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<vm or service name>-pip` | `profx-sql1-pip` |
| Netzwerke | Lastenausgleich | Ressourcengruppe | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service or role>-lb` | `profx-lb` |
| Netzwerke | Laden Sie angeglichene Regeln Config | Lastenausgleich | 1-80 | Groß-und Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Organisieren von Ressourcen mit Kategorien

Ressourcenmanager Azure unterstützt tagging Personen mit beliebigen Textzeichenfolgen zu identifizieren Kontext und Automatisierung zu optimieren.  Beispielsweise die Kategorie `"sqlVersion: "sql2014ee"` konnte virtuellen Computern in einer SQL Server 2014 Enterprise Edition-Bereitstellung für die Ausführung von einem automatischen Skript davor identifizieren.  Verbessern und optimieren Kontext am Rand des Benennungskonventionen ausgewählt, sollte Kategorien verwendet werden.

> [AZURE.TIP]Ein weiterer Vorteil von Kategorien ist, dass Kategorien Ressourcengruppen ermöglicht es Ihnen umfassen, link und zu koordinieren Einheiten über verschiedenartige Bereitstellungen.

Jeder Ressource oder Ressourcengruppe kann maximal **15** Kategorien haben. Der Tagname auf 512 Zeichen beschränkt ist, und der Kategorie Wert ist auf 256 Zeichen beschränkt.

Weitere Informationen zum Kategorisieren von Ressourcen finden Sie in [Verwenden von Kategorien, um Ihre Azure Ressourcen zu organisieren](../resource-group-using-tags.md).

Einige allgemeine tagging verwenden Fälle sind:

- **Abrechnung**; Gruppieren von Ressourcen und Zuordnen von diese Abrechnung oder Gebühr back-Codes.
- **Identifikation von Diensten Kontext**; Identifizieren Sie Gruppen von Ressourcen zwischen Gruppen von Ressourcen für allgemeine Vorgänge und Gruppierung
- **Access-Steuerelement und Sicherheitskontext**; Administratorrollen Kennung basierend auf Portfolio, System, Service, app, Instanz usw..

> [AZURE.TIP]Kategorisieren von früh – häufig Kategorie.  Haben Sie einen Basisplan tagging Farbschema direkte besser und Anpassung im Laufe der Zeit, anstatt nach dem Vorfall überarbeiten zu müssen.  

Beispiel für einige häufige tagging Ansätze:

| Tagname | Schlüssel | Beispiel | Kommentar |
| -------- | --- | ------- | ------- |
| Rechnung zu / internen Chargeback-ID | billTo  | `IT-Chargeback-1234` | Eine interne e/a- oder Abrechnung code |
| Operator oder direkt verantwortlichen Benutzer (DRI) | managedBy | `joe@contoso.com`  | Alias oder die e-Mail-Adresse |
| Projektname | Project-name | `myproject`  | Name der Zeile Projekt oder Produkt |
| Project-Version | Project-version | `3.4`  | Version der Zeile Projekt oder Produkt |
| Umgebung | Umgebung | `<Production, Staging, QA >` | Umgebung Bezeichner | 
| Ebene | Ebene | `Front End, Back End, Data` | Stufe oder Rolle/Kontext Kennung |
| Datenprofil | dataProfile | `Public, Confidential, Restricted, Internal` | Vertraulichkeit von Daten aus der Ressource |
 
## <a name="tips-and-tricks"></a>Tipps und Tricks

Einige Arten von Ressourcen erfordern möglicherweise zusätzliche Vorsicht zu benennen und Konventionen.

### <a name="virtual-machines"></a>Virtuellen Computern

Insbesondere in größere Topologien optimiert sorgfältig benennen virtuellen Computern die Rolle und den Zweck der einzelnen Computer identifizieren und weitere vorhersehbar Skripting aktivieren.

> [AZURE.WARNING]Jeder virtuellen Computern in Azure verfügt über eine Azure Ressourcenname, und ein Betriebssystem-Hostname.  
> Wenn der Ressourcenname und Hostname unterscheiden, die virtuellen Computern verwalten möglicherweise eine Herausforderung und vermieden werden sollte.
> Angenommen, wenn eine virtuellen Computern, die bereits aus einer VHD erstellt wird enthält Betriebssystem konfiguriert mit einem Hostnamen.

- [Benennungskonvention für Windows Server virtuellen Computern](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Speicherkonten und Speicher Einheiten

Es gibt zwei primäre verwenden Fälle für Speicherkonten - Datenträger für virtuelle Computer sichern, und Speichern von Daten in Blobs, Queues und Tabellen.  Speicherkonten für Datenträger virtueller Computer verwendet sollte führen Sie die Benennungskonvention für das übergeordnete Element Name des virtuellen Computers zuordnen (und mit Notwendigkeit für mehrere Speicherkonten High-End-virtuellen Computer SKUs, wenden Sie außerdem eine Zahl Suffix).

> [AZURE.TIP]Speicherkonten - soll eine Benennungskonvention für Daten oder Laufwerke – folgen, die mehrere Speicherkonten genutzt werden (d. h. immer mit einem numerischen Suffix) ermöglicht.

Es möglich, einen benutzerdefinierten Domänennamen für den Zugriff auf BLOB-Daten in Ihr Konto Azure-Speicher zu konfigurieren.
Der Standardendpunkt für den Dienst Blob ist `https://mystorage.blob.core.windows.net`.

Wenn Sie eine benutzerdefinierte Domäne (beispielsweise www.contoso.com) an den Endpunkt Blob für Ihr Speicherkonto zuordnen, können Sie auch aufrufen, aber BLOB-Daten in Ihr Speicherkonto mithilfe dieser Domäne. Mit einem benutzerdefinierten Domänennamen, beispielsweise `http://mystorage.blob.core.windows.net/mycontainer/myblob` zugegriffen werden kann, als `http://www.contoso.com/mycontainer/myblob`.

Weitere Informationen zum Konfigurieren dieses Features finden Sie in [konfigurieren einen benutzerdefinierten Domänennamen für Ihre Blob-Speicher-Endpunkt](../storage/storage-custom-domain-name.md).

Weitere Informationen zur Benennung von Blobs, Container und Tabellen:

- [Benennen und verweisen auf Container, Blobs und Metadaten](https://msdn.microsoft.com/library/dd135715.aspx)
- [Benennen Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx)
- [Benennen von Tabellen](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Ein Namen Blob kann eine beliebige Kombination von Zeichen enthalten, jedoch reservierte URL-Zeichen ordnungsgemäß geschützt werden müssen. Vermeiden Sie Blob-Namen, die mit einem Punkt (.), einen Schrägstrich (/), beenden oder eine Sequenz oder eine Kombination aus beiden. Schrägstrich wird traditionell das **virtuelle** Verzeichnistrennzeichen. Verwenden Sie keinen umgekehrten Schrägstrich (\) in einem Blob-Namen. Der Client APIs möglicherweise ermöglichen es, aber dann nicht ordnungsgemäß Hashing und Signaturen stimmen nicht überein.

Es ist nicht möglich, den Namen eines Speicherkontos oder Container ändern, nachdem er erstellt wurde.
Wenn Sie einen neuen Namen verwenden möchten, müssen Sie löschen und einen neuen erstellen.

> [AZURE.TIP] Es empfiehlt sich, dass Sie eine Benennungskonvention für alle Speicherkonten und Typen herstellen, ehe die Entwicklung einer neuen Dienst oder einer Anwendung.

## <a name="example---deploying-an-n-tier-service"></a>Beispiel – Bereitstellen eines mehrstufige-Diensts

In diesem Beispiel wir Definieren einer mehrstufige Dienstkonfiguration Front-End-IIS-Servern (in Windows Server virtuellen Computern gehostet) mit SQL Server (in zwei Windows Server-virtuellen Computern gehostet wird), einer ElasticSearch Cluster (in 6 Linux virtuellen Computern gehostet) aus, und die zugehörige Speicher-Konten virtuelle Netzwerke Ressource gruppieren und Lastenausgleich.

Zunächst wird die kontextbezogenen Konventionen für diese Anwendung definieren:

| Entität | Messe | Beschreibung  |
| ------ | ---------- | ------------ |  
| Service Name | `profx` | Der Kurzname der Anwendung oder Dienst bereitgestellt wird |
| Umgebung | `prod` | Dies ist für die Bereitstellung der Herstellung (im Gegensatz zu f & a, Test usw..) |

Von dieser Basislinie können wir dann, den Konventionen für jede Ressourcentypen zuordnen:

| Ressourcenart | Basis Messe | Beispiel |
| ------------- | --------------- | ------- |
| Abonnement | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Ressourcengruppe | `servicename-rg` | `profx-rg` |
| Virtuelles Netzwerk | `servicename-vnet` | `profx-vnet` |
| Subnetz | `role-subnet` | `sql-vnet` |
| Lastenausgleich | `servicename-lb` | `profx-lb` |
| Virtuellen Computern | `servicename-role[number]` | `profx-sql0` |
| Speicher-Konto | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Wie im folgenden Diagramm dargestellt:

![Anwendung Suchtopologie Diagramm] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Beispiel für Anwendung Suchtopologie")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Beispiel - Azure CLI Skript für die Bereitstellung von im Beispiel oben

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```

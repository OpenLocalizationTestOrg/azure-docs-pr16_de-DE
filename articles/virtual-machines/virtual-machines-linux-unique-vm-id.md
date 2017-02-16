<properties
   pageTitle="Zugreifen auf virtuellen Computer-ID"
   description="Zugreifen auf und Verwenden von Azure-virtuellen Computer einmalige Nr. werden"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Zugreifen auf und Verwenden von Azure-virtuellen Computer einmalige Nr.

Azure-virtuellen Computer eindeutige ID ist ein Bezeichner 128 codierte alle Azure NEUERUNGs SMBIOS gehörende Kehrmatrix und aktuell Plattform BIOS Befehle mit gelesen werden kann.

Azure-virtuellen Computer eindeutige ID ist eine schreibgeschützte Eigenschaft. Azure einmalige Nr. des virtuellen Computer werden nicht geändert, nach dem Neustart war(en) (entweder für geplanter ungeplanten), Start/Stopp heben Sie die Zuordnung, service, Reparatur oder direkte wiederherstellen. Wenn Sie der virtuellen Computer ist eine Momentaufnahme und kopiert haben, um eine neue Instanz zu erstellen, jedoch neue Azure-virtuellen Computer-ID konfiguriert.

> [AZURE.NOTE] Wenn Sie älteren virtuellen Computern erstellt und ausgeführt werden, da diese neue Funktion ab (18 September 2014), wenden Sie sich bitte Restart bereitgestellt haben-ID Ihre virtuellen Computer automatisch eine Azure eindeutige abrufen.


Auf Azure eindeutige virtueller Computer ID in den virtuellen Computer zugreifen zu können:


## <a name="create-a-vm"></a>Erstellen eines virtuellen Computers
 

Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Verbinden mit dem virtuellen Computer
 

Weitere Informationen finden Sie unter [SSH von Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Abfrage virtueller Computer einmalige Nr.

Befehl (Beispiel verwendet **Ubuntu**):

    sudo dmidecode | grep UUID
    
Beispiel für erwartete Ergebnisse:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Aufgrund von Big Endian bit Sortierung werden die tatsächliche eindeutige ID für virtueller Computer in diesem Fall:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure-virtuellen Computer eindeutige ID kann in anderen Szenarien verwendet werden, ob der virtuellen Computer auf Azure ausgeführt wird oder lokal und kann Ihren Anforderungen zur Lizenzierung, reporting oder allgemeine Verlauf, die Sie für die Bereitstellung Ihrer Azure IaaS möglicherweise helfen. Viele unabhängige Softwareanbieter Applications erstellen und diese auf Azure zertifizieren möglicherweise erforderlich, um eine Azure-virtuellen Computer während des gesamten Lebenszyklus zu identifizieren und können feststellen, ob der virtuellen Computer auf Azure ausgeführt wird lokal oder auf andere Cloud-Anbieter. Dieser Plattformbezeichner hilft beispielsweise feststellen, ob die Software ordnungsgemäß lizenziert ist oder dazu beitragen, um virtueller Computer Daten zur Quelle wie zu koordinieren unterstützen zum Festlegen der richtigen Metrik für die richtige Plattform und zu verfolgen und zu koordinieren diese Metrik dem anderen verwendet.

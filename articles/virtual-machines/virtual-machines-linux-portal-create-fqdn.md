<properties
   pageTitle="Erstellen von FQDN für einen virtuellen Azure-Portal | Microsoft Azure"
   description="Informationen zum Erstellen einer Fully Qualified Domain Name oder FQDN für ein Ressourcenmanager Grundlage virtuellen Computern Azure-Portal."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Erstellen Sie einen vollqualifizierten Domänennamen Azure-Portal
Bei der Erstellung eines virtuellen Computers (virtueller Computer) in der [Azure-Portal](https://portal.azure.com) mit dem Modell zur Bereitstellung von Ressourcenmanager wird automatisch eine öffentliche IP-Ressource für den virtuellen Computer erstellt. Sie verwenden diese IP-Adresse, den Remotezugriff auf des virtuellen Computer. Zwar im Portal keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)oder FQDN, standardmäßig erstellt wird, können Sie eine hinzufügen, nachdem Sie der virtuellen Computer erstellt haben. Dieser Artikel beschreibt die Schritte zum Erstellen eines DNS-Namen oder FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sie können jetzt Remote Herstellen einer Verbindung mit dem virtuellen Computer mit dem folgenden DNS-Namen wie mit `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun Ihre virtuellen Computer einen öffentlichen IP-Adresse und DNS-Namen ist, können Sie allgemeine Anwendungsframeworks oder Diensten wie Nginx, MongoDB, Docker, bereitstellen usw..

Sie können auch weitere Informationen zur [Verwendung Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) Tipps zur Bereitstellung Ihrer Azure erstellen.
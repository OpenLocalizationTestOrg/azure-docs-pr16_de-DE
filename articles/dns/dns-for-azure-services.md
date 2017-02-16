<properties
  pageTitle="Verwendung von Azure DNS mit anderen Diensten Azure | Microsoft Azure"
  description="Grundlegendes zum Azure DNS zum Auflösen Namens für andere Dienste Azure verwenden"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Verwendung von Azure DNS mit anderen Diensten Azure

Azure DNS wird eine gehostete DNS-Verwaltung und Namen Auflösung. So können Sie Öffentliche DNS-Namen für die anderen Anwendungen und Dienste, die Sie bereitgestellt haben in Azure erstellen. Erstellen einen Namen für eine Azure Service in Ihrer benutzerdefinierten Domäne ist so einfach wie das Hinzufügen eines Eintrags, der die richtige Art des Diensts.

* Für dynamisch zugewiesenen IP-Adressen müssen Sie einen DNS-CNAME-Eintrag erstellen, der die den DNS-Namen zugeordnet ist, die für Ihren Dienst Azure erstellt haben. DNS-Standards verhindern, dass Sie einen CNAME-Eintrag für die Zone Spitze verwenden.
* Für statisch reservierte IP-Adressen können Sie einen DNS A-Eintrag mit einem beliebigen Namen, einschließlich _naked_ Domänennamen in der Zone Spitze erstellen.

In der folgenden Tabelle werden die unterstützten Datensatztypen, die für verschiedene Azure Dienste verwendet werden können. Wie Sie aus dieser Tabelle sehen können, unterstützt Azure DNS nur DNS-Einträge für Internet zugänglichen Netzwerkressourcen. Azure DNS kann nicht für die Auflösung von interne private Adressen Namen verwendet werden.

| Azure Service | Netzwerk-Benutzeroberfläche | Beschreibung |
|---------------|-------------------|-------------|
| Application Gateway | Front-End-öffentliche IP-Adresse | Sie können einen DNS A oder CNAME-Eintrag erstellen. |
| Lastenausgleich | Front-End-öffentliche IP-Adresse | Sie können einen DNS A oder CNAME-Eintrag erstellen. Lastenausgleich kann eine IPv6 öffentlichen IP-Adresse verfügen, die dynamisch zugewiesen wird. Daher müssen Sie einen CNAME-Eintrag für eine IPv6-Adresse erstellen. |
| Datenverkehr-Manager | Öffentlicher name | Sie können nur einen CNAME erstellen, die den trafficmanager.net Namen zu Ihrem Profil Datenverkehr Manager zugewiesen zugeordnet ist. Weitere Informationen finden Sie unter [wie Datenverkehr-Manager zu erhalten](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Cloud-Dienst | Öffentliche IP-Adresse | Für statisch reservierte IP-Adressen können Sie einen DNS A-Eintrag erstellen. Für dynamisch zugewiesenen IP-Adressen müssen Sie einen CNAME-Eintrag erstellen, der die den Namen der _cloudapp.net_ zugeordnet ist. Diese Regel gilt für virtuellen Computern im klassischen Portal erstellt werden, weil sie als Cloud-Dienst bereitgestellt werden. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in der Cloud Services](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App-Dienst | Externe IP-Adresse | Für externe IP-Adressen können Sie einen DNS A-Eintrag erstellen. Andernfalls müssen Sie einen CNAME-Eintrag erstellen, der die den Namen der azurewebsites.net zugeordnet ist. Weitere Informationen finden Sie unter [zuordnen einen benutzerdefinierten Domänennamen zu einer Azure-app](../app-service-web/web-sites-custom-domain-name.md) |
| Ressourcenmanager virtuellen Computern | Öffentliche IP-Adresse | Ressourcenmanager virtuellen Computern können öffentliche IP-Adressen haben. Ein virtueller Computer mit einer öffentlichen IP-Adresse möglicherweise auch hinter einem Lastenausgleich. Sie können einen DNS A oder CNAME-Eintrag für die öffentliche Adresse erstellen. Diese benutzerdefinierten Namen kann die VIP auf dem Lastenausgleich umgehen verwendet werden. |
| Klassische virtuellen Computern | Öffentliche IP-Adresse | Klassische virtuellen Computern erstellt mithilfe der PowerShell oder CLI kann mit eine dynamische oder statische (reservierte) virtuelle Adresse konfiguriert werden. Sie können einen CNAME-DNS oder einen Datensatz Hilfethemas erstellen. |

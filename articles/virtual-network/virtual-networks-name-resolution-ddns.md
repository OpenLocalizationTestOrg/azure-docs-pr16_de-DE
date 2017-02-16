<properties
   pageTitle="Verwenden von dynamischen DNS zum Registrieren Hostnamen"
   description="Diese Seite bietet Details zum Einrichten von dynamischen DNS Hostnamen in Ihre eigenen DNS-Server zu registrieren."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Verwenden von dynamischen DNS zum Hostnamen in Ihrer eigenen DNS-Server zu registrieren.

[Azure bietet namensauflösung](virtual-networks-name-resolution-for-vms-and-role-instances.md) für virtuelle Computer (virtuelle Computer) und Rolleninstanzen aus. Wenn die von Azure bereitgestellten Ihrer namensauflösung hinausgehen muss, können Sie Ihre eigenen DNS-Server bereitstellen. Dies bietet Ihnen die Möglichkeit, Ihre DNS-Lösung Ihren eigenen spezifischen Anforderungen entsprechend anpassen können. Beispielsweise müssen Sie den Zugriff auf lokale Ressourcen über Controller Ihrer Active Directory-Domäne.

Wenn Ihre benutzerdefinierten DNS-Server als Azure-virtuellen Computern gehostet werden, können Sie Hostname Abfragen für die gleichen Vnet in Azure Hostnamen auflösen weiterleiten. Wenn Sie nicht dieses Routing verwenden möchten, können Sie Ihre virtuellen Computer Hostnamen in Ihrem DNS-Server mit dynamischen DNS registrieren.  Die Möglichkeit (z. B. Anmeldeinformationen) keinen Azure Datensätze direkt in Ihre DNS-Server erstellt wird, damit alternative Regelung häufig erforderlich sind. Hier sind einige häufige Szenarien mit alternativen aus.

## <a name="windows-clients"></a>Windows-clients

Windows-Clients nicht Domänenverbund versuchen unsicheren Dynamic DNS (DDNS) Updates, wenn er gestartet oder wenn ihre IP-Adresse ändert. Der DNS-Name ist der Hostname sowie die primäre DNS-Suffix an. Azure primären DNS Suffixes leer lässt, aber Sie können dies auf dem virtuellen Computer, über die [Benutzeroberfläche](https://technet.microsoft.com/library/cc794784.aspx) oder [mithilfe der Automatisierung](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix)festlegen.

Windows-Clients Domänenverbund registrieren ihre IP-Adressen der Domänencontroller mithilfe von secure dynamische DNS. Der Domäne Join-Vorgang legt das primäre DNS-Suffix auf dem Client und erstellt und verwaltet die Vertrauensstellung.

## <a name="linux-clients"></a>Linux-clients

Linux-Clients im Allgemeinen nicht registrieren sich bei der DNS-Server auf Start, und sie davon ausgehen, dass der DHCP-Server bedeutet. Azure des DHCP-Server verfügen nicht die Möglichkeit und die Anmeldeinformationen Datensätze in Ihrem DNS-Server registrieren.  Ein Tool namens *Nsupdate*, die in das Paket Bindung enthalten ist, können Sie um dynamische DNS-Aktualisierungen zu senden. Da das dynamische DNS-Protokoll standardisiert ist, können Sie *Nsupdate* , auch wenn Sie nicht binden auf der DNS-Server verwenden.

Sie können die Haken verwenden, die vom DHCP-Client zum Erstellen und verwalten den Hostnameneintrag in der DNS-Server bereitgestellt werden. Im DHCP-Zyklus führt der Client die Skripts in */etc/dhcp/dhclient-exit-hooks.d/*an. Hiermit kann die neue IP-Adresse mit *Nsupdate*registrieren. Beispiel:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Den Befehl *Nsupdate* können Sie auch sichere dynamische DNS-Updates ausführen. Beispielsweise, wenn Sie einen binden DNS-Server verwenden, wird ein Paar aus öffentlichen und dem privaten Schlüssel [generiert](http://linux.yyz.us/nsupdate/).  Der DNS-Server ist [so konfiguriert ist](http://linux.yyz.us/dns/ddns-server.html) , mit dem öffentlichen Teil des Schlüssels, damit sie die Signatur aus, klicken Sie auf die Anfrage überprüfen kann. Sie müssen die Option ' *-k* ' verwenden, um zu ermöglichen, aktualisieren Sie die Taste Paare zu *Nsupdate* in Reihenfolge für das dynamische DNS-Anforderung signiert werden.

Wenn Sie einen Windows-DNS-Server verwenden, können Sie die Kerberos-Authentifizierung mit dem Parameter *-g* in *Nsupdate* (in der Windows-Version von *Nsupdate*nicht verfügbar). Verwenden Sie hierzu *Kinit* , um die Anmeldeinformationen (z. B. von einer [Keytab-Datei](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)) zu laden. Klicken Sie dann übernehmen *Nsupdate -g* die Anmeldeinformationen aus dem Cache.

Bei Bedarf können Sie zu Ihrer virtuellen Computer ein DNS-Suche Suffix hinzufügen. Das DNS-Suffix wird in der Datei */etc/resolv.conf* angegeben. Die meisten Linux Distros automatisch verwalten den Inhalt dieser Datei, daher normalerweise nicht bearbeiten. Jedoch können Sie das Suffix mithilfe der DHCP-Client *an die Stelle* der Befehl überschreiben. Fügen Sie dazu in */etc/dhcp/dhclient.conf*hinzu:

        supersede domain-name <required-dns-suffix>;


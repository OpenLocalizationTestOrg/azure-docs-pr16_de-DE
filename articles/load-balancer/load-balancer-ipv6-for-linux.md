<properties
    pageTitle="Konfigurieren von DHCPv6 für Linux virtuellen Computern | Microsoft Azure"
    description="So konfigurieren Sie DHCPv6 für Linux virtuellen Computern."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, Azure Lastenausgleich, zwei Stapel, öffentliche IP-Adresse, native ipv6, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurieren von DHCPv6 für Linux virtuellen Computern

Einige der Linux virtuellen Computern Bilder in der Azure Marketplace haben keine DHCPv6 standardmäßig konfiguriert. Zur Unterstützung von IPv6 muss DHCPv6 in innerhalb der Verteilung Linux OS konfiguriert werden, die Sie verwenden. Verschiedene Linux-Versionen haben verschiedene Arten von DHCPv6 konfigurieren, da sie verschiedene Pakete verwenden.

>[AZURE.NOTE] Zuletzt verwendete SUSE Linux und CoreOS Bilder in der Azure Marketplace wurden DHCPv6 vorkonfiguriert. Wenn Sie mit diesen Bildern werden muss nicht zusätzliche geändert.

Dieses Dokument beschreibt das DHCPv6 aktivieren, damit Ihre Linux virtuellen Computern eine IPv6-Adresse erhält.

>[AZURE.WARNING] Bearbeiten nicht ordnungsgemäß Netzwerk Konfigurationsdateien kann nicht mehr Netzwerkzugriff auf Ihre virtuellen Computer verursacht. Wir empfehlen, dass Sie die Konfiguration Änderungen auf nicht genutzten testen. Die Anweisungen in diesem Artikel wurden auf die neuesten Versionen der Bilder in der Azure Marketplace Linux getestet. Welche Version von Linux ausführlichere Anweisungen finden Sie in der Dokumentation.

## <a name="ubuntu"></a>Ubuntu

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie die folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Konfiguration für die Schnittstelle eth0 mit der folgenden Konfiguration:

    * Bearbeiten Sie die Datei auf **Ubuntu 12.04 und 14.04**`/etc/network/interfaces.d/eth0.cfg`
    * Bearbeiten Sie die Datei auf **Ubuntu 16.04**`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Erneuern Sie IPv6-Adresse an:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie die folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Datei `/etc/network/interfaces` , und fügen Sie die folgende Konfiguration hinzu:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Erneuern Sie IPv6-Adresse an:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Bearbeiten Sie die Datei `/etc/sysconfig/network` , und fügen Sie den folgenden Parameter hinzu:

        NETWORKING_IPV6=yes

2. Bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0` , und fügen Sie die folgenden zwei Parameter hinzu:

        IPV6INIT=yes
        DHCPV6C=yes

3. Erneuern Sie IPv6-Adresse an:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & OpenSUSE 13

Zuletzt verwendete SLES und OpenSUSE Bilder in Azure wurden DHCPv6 vorkonfiguriert. Wenn Sie mit diesen Bildern werden muss nicht zusätzliche geändert. Wenn Sie einen virtuellen basierend auf ein Bild mit älteren oder benutzerdefinierten SUSE verfügen, verwenden Sie die folgenden Schritte aus:

1. Installieren der `dhcp-client` Verpacken, falls erforderlich:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und fügen Sie den folgenden Parameter hinzu:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse an:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 und OpenSUSE Sprung

Zuletzt verwendete SLES und OpenSUSE Bilder in Azure wurden DHCPv6 vorkonfiguriert. Wenn Sie mit diesen Bildern werden muss nicht zusätzliche geändert. Wenn Sie einen virtuellen basierend auf ein Bild mit älteren oder benutzerdefinierten SUSE verfügen, verwenden Sie die folgenden Schritte aus:

1. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und Ersetzen Sie für diesen Parameter

        #BOOTPROTO='dhcp4'

    mit dem folgenden Wert:

        BOOTPROTO='dhcp'

2. Fügen Sie den folgenden Parameter zu `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse an:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Zuletzt verwendete CoreOS Bilder in Azure wurden DHCPv6 vorkonfiguriert. Wenn Sie mit diesen Bildern werden muss nicht zusätzliche geändert. Wenn Sie einen virtuellen basierend auf einem älteren oder benutzerdefinierte CoreOS Bild haben, gehen Sie folgendermaßen vor:

1. Bearbeiten Sie die Datei`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Erneuern Sie die IPv6-Adresse an:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```

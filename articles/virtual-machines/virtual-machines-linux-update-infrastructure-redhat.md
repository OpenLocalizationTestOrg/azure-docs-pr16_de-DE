<properties
   pageTitle="Red Hat Update Infrastruktur (RHUI) | Microsoft Azure"
   description="Weitere Informationen Sie zu rot Hat Update Infrastruktur (RHUI) für bei Bedarf Red Hat Enterprise Linux Instanzen in Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update Infrastruktur (RHUI) für bei Bedarf Red Hat Enterprise Linux virtuellen Computern in Azure

Bei Bedarf Red Hat Enterprise Linux (RHEL) Bilder verfügbar in Azure Marketplace erstellte Maschinen sind Zugriff auf die in Azure bereitgestellt Rot Hat Update Infrastruktur (RHUI) registriert.  Bei Bedarf RHEL Instanzen haben Zugriff zu einem regionalen Yum Repository und periodische Updates empfangen.

Die Yum Repository-Liste, der vom RHUI verwaltet wird, wird in Ihrem RHEL Instanz während der Bereitstellung konfiguriert. Sie brauchen führen Sie zusätzliche Konfiguration - ausführen `yum update` nach Ihrer RHEL Instanz bereit sind, erhalten die neuesten Updates ist.

> [AZURE.NOTE] Zuletzt aktualisiert wurde (September 2016) für Azure RHUI Infrastruktur und Änderungen in der Konfiguration der vorhandenen RHEL Instanzen für den kontinuierlichen Zugriff auf die Azure RHUI erfordert. Finden Sie im Abschnitt RHUI Azure Infrastrukturupdate Details.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure Infrastruktur aktualisieren
Zum Zeitpunkt September 2016 stehen in Azure einen neuen Satz von Servern Rot Hat Update Infrastruktur (RHUI). Damit ein einzelner Endpunkt (Rhui-1.micrsoft.com) von einem beliebigen virtuellen Computer unabhängig davon Region verwendet werden kann, werden diese Server mit [Azure Datenverkehr Manager]( https://azure.microsoft.com/services/traffic-manager/) bereitgestellt. Sie können auch eine SSL-Zertifikat, die mit einer bekannten Zertifizierungsstelle (Baltimore Root) verkettet ist verwenden. Bereitstellen von automatischen dieses Update wäre für einige Kunden gefährlicher, die ACLs oder benutzerdefinierte routing-Tabellen für den RHUI Update-Servern aufweisen, damit Sie dieses Update ist "Teilnahme." Manuelle Schritte für Onboarding auf diese neuen Server stehen auf dieser Seite und ein vollständiges Skript für Onboarding automatisch und (bei der Überprüfung der einzelnen Schritte). Die neuen RHEL PAYG Bilder in der Azure Marketplace (dem September 2016 oder höhere Versionen) werden automatisch die neuen Azure RHUI Server zeigen und keine weiteren Maßnahmen erforderlich.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Die neue Azure RHUI Infrastruktur Onboarding Zeitachse

| Datum | Notiz |
| --- | --- |
|22 September 2016 | RHUI Servers und installieren Anweisungen zur Verwendung verfügbar. Virtuellen Computern bereitgestellt, mit dem neuen (September 2016 behoben) RHEL PAYG Marketplace Bilder werden automatisch die neuen RHUI Server verwendet, aber die vorhandene virtuellen Computern sind "Teilnahme"
|1 November 2016 | Legacy RHEL PAYG virtueller Computer Bilder (die die alten Azure RHUI Server verwenden) werden aus dem Katalog Azure Marketplace entfernt
|16 Januar 2017 | Die alten Azure RHUI Server außer Betrieb genommen werden. Aktualisieren Sie aller Ihrer betroffenen PAYG RHEL virtuellen Computern bis zu diesem Zeitpunkt Zugriff auf Azure RHUI verwalten

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Die IP-Adressen für die neuen RHUI Bereitstellung von Inhalten Server sind

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Manuelle Aktualisierungsverfahren verwenden Sie die neuen Azure RHUI Server

Herunterladen Sie (über Curl) der öffentlichen Schlüssel Signatur

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Überprüfen Sie die heruntergeladene key

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Überprüfen Sie die Ausgabe, vergewissern Sie sich `keyid` und `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Installieren Sie den öffentlichen Schlüssel

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Herunterladen Sie, überprüfen Sie, und installieren Sie Client u/Min

Download: Für RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Für RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Überprüfen Sie:

```
rpm -Kv azureclient.rpm
```

Aktivieren Sie in der Ausgabe ist die Signatur des Pakets OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Installieren der u/Min

```
sudo rpm -U azureclient.rpm
```

Nach Abschluss des Vorgangs stellen Sie sicher, dass Sie Azure RHUI Formular den virtuellen Computer zugreifen können

### <a name="all-in-one-script-for-automating-the-above-task"></a>All-in-One Skript zum Automatisieren von der oben genannten Vorgang
Verwenden Sie das folgende Skript Bedarf zum Automatisieren von der Aufgabe Aktualisierung betroffene virtuellen Computern an die neue RHUI Azure-Server.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI (Übersicht)
[Rot Hat Update Infrastruktur](https://access.redhat.com/products/red-hat-update-infrastructure) bietet eine hochgradig skalierbare Lösung zum Verwalten der Yum Repository Inhalte für Red Hat Enterprise Linux Cloud Instanzen von Red Hat-zertifizierten Cloud-Anbieter gehostet werden. Ausgehend von der übergeordneten Zellstoff Project ermöglicht RHUI Cloud-Anbieter lokal Repository Red Hat gehostete Inhalt spiegeln, benutzerdefinierte Repositorys mit eigene Inhalte erstellen und diese Repositorys für einer großen Gruppe von Endbenutzern über ein Lastenausgleich Inhalt Übermittlung System verfügbar machen.

## <a name="regions-where-rhui-is-available"></a>Regionen, in dem RHUI verfügbar ist.
RHUI steht in allen Regionen, wenn RHEL bei Bedarf Bilder verfügbar sind. Er enthält derzeit alle Öffentliche Regionen auf der Dashboardseite [Azure Status](https://azure.microsoft.com/status/) und Azure US-Regierung Regionen aufgeführt. Ihre Preis ist RHUI Access für virtuelle Computer nach der Bereitstellung von RHEL bei Bedarf Bilder enthält. Zusätzliche Regions-/nationale Cloud Verfügbarkeit wird aktualisiert werden, sobald wir die Verfügbarkeit von RHEL bei Bedarf in der Zukunft erweitern.

> [AZURE.NOTE] Zugriff auf RHUI Azure gehostet ist mit den virtuellen Computern in [Microsoft Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)beschränkt.

## <a name="get-updates-from-another-update-repository"></a>Abrufen von Updates aus einem anderen Update repository

Wenn Sie zum Abrufen von Updates von einem anderen Update Repository (statt Azure gehostete RHUI) müssen müssen Sie Ihre Instanzen von RHUI aufheben und diese erneut mit der gewünschten Update-Infrastruktur (z. B. Rot Hat Satelliten oder rot Hat Kunden Portal CDN) registrieren. Sie benötigen die entsprechenden Red Hat Abonnements für diese Dienste und der Registrierung [Rot Hat](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)cloudzugriff in Azure.

Aufheben von RHUI und erneut registrieren, zu Ihrer Update-Infrastruktur folgen die folgende Schritte aus.

1.  Bearbeiten von /etc/yum.repos.d/rh-cloud.repo und ändern Sie alle `enabled=1` auf `enabled=0`. Beispiel:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf bearbeiten und Ändern von `enabled=0` zu `enabled=1`.
3.  Klicken Sie dann die gewünschte Infrastruktur, z. B. Rot Hat Kundenportal registrieren. Führen Sie Red Hat Lösung Leitfaden zum [registrieren und Abonnieren eines Systems Rot Hat Kunden-Portal](https://access.redhat.com/solutions/253273)an.

> [AZURE.NOTE] Zugriff auf die RHUI Azure gehostet wird im Preis Bild RHEL nutzungsbasierte (PAYG) enthalten. Aufheben der Registrierung eines PAYG RHEL virtuellen Computers aus der Azure gehostete RHUI konvertiert nicht des virtuellen Computers zu bringen-Your-Besitzer-Lizenz (BYOL) Typ virtueller Computer und daher Sie möglicherweise werden dabei doppelte Gebühren, wenn Sie den gleichen virtuellen Computer mit einer anderen Quelle Aktualisierungen registrieren. 
> 
> Wenn Sie konsistent mit anderen als mit einer aktualisierten Infrastruktur Azure gehostete RHUI empfiehlt sich erstellen und Bereitstellen eigener Bilder (BYOL-Typ) [Erstellen und Hochladen Red Hat-basierten virtuellen Computern für Azure](virtual-machines-linux-redhat-create-upload-vhd.md) Artikel beschriebenen.

## <a name="next-steps"></a>Nächste Schritte
So erstellen Sie einen Red Hat Enterprise Linux virtuellen Computer aus Azure Marketplace nutzungsbasierte Bild- und nutzen Azure gehostete RHUI wechseln Sie zu [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Sie werden möglicherweise verwenden `yum update` in Ihrer RHEL Instanz ohne jede zusätzliche Setup.
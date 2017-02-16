<properties
   pageTitle="Systemanforderungen StorSimple Virtual Array"
   description="Erfahren Sie mehr über die Software und Netzwerke für Ihr StorSimple virtuelle Array"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Systemanforderungen StorSimple Virtual Array

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die wichtigen Systemanforderungen für Ihr Microsoft Azure StorSimple virtuelle Array (auch bekannt als lokalen StorSimple virtuelles Gerät oder StorSimple virtuelles Gerät) und für den Speicher-Clients den Zugriff auf die Matrix. Es empfiehlt sich, dass Sie die Informationen sorgfältig überprüfen, bevor Sie Ihrem System StorSimple bereitstellen, und klicken Sie dann darauf wieder nach Bedarf während der Bereitstellung und nachfolgenden Vorgang verweisen.

Die systemvoraussetzungen umfassen:

-   **Softwareanforderungen für Speicher-Clients** - beschreibt die unterstützte Virtualisierungsplattformen, Webbrowser, iSCSI-Initiatoren, SMB-Clients, minimale virtuelles Gerät Anforderungen und alle weiteren Anforderungen für diese Betriebssysteme.

-   **Anforderungen für das Gerät StorSimple networking** - enthält Informationen zu den Ports, die in Ihre Firewall für iSCSI, Cloud oder Management Datenverkehr geöffnet sein müssen.

Die StorSimple Anforderungen Systeminformationen in diesem Artikel veröffentlicht gilt nur für StorSimple virtuelle Matrizen zurück.

- Geräte 8000-Serie finden Sie unter [Systemanforderungen für Ihr StorSimple 8000 Reihe Gerät](storsimple-system-requirements.md).
 
- 7000 Reihe Geräten finden Sie unter [Systemanforderungen für Ihr Gerät StorSimple 5000 7000 - Serie](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Softwareanforderungen

Die softwareanforderungen enthalten die Informationen zu den unterstützten Webbrowsern, SMB-Versionen, Virtualisierungsplattformen und den minimalen virtuelles Gerät Anforderungen an.

### <a name="supported-virtualization-platforms"></a>Unterstützte Plattformen


| **Hypervisor** | **Version**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 und höher |
| VMware ESXi    | 5,5 und höher                        |

### <a name="virtual-device-requirements"></a>Virtuelles Gerät Anforderungen

| **Komponente**                                | **Anforderung**            |
|----------------------------------------------|----------------------------|
| Mindestanzahl der virtuellen Prozessoren (Kerne) | 4                          |
| Minimale RAM-Speicher                         | 8 GB                       |
| Datenträger Abstand<sup>1</sup>                       | OS Datenträger - 80 GB <br></br>Daten Datenträger - 500 GB auf 8 TB|
| Mindestanzahl der Netzwerk-Schnittstellen       | 1                          |
| Minimale Internet Bandbreite<sup>2</sup>       | 5/s                     |

<sup>1</sup> - dünn nach der Bereitstellung

<sup>2</sup> - sind die tägliche Daten Änderungsrate Netzwerk Anforderungen abhängig. Angenommen, wenn ein Gerät 10 GB oder weitere Änderungen während eines Tags sichern muss, kann dann die tägliche Sicherung über eine 5/s-Verbindung bis zu 4,25 Stunden dauern (wenn die Daten nicht komprimiert oder deduplizierte werden konnte).

### <a name="supported-web-browsers"></a>Unterstützte Webbrowser

| **Komponente**     | **Version** | **Weitere Anforderungen von Notizen** |
|-------------------|-----------------|-----------------------------------|
| Microsoft-Rand    | Neueste version  |                                   |
| InternetExplorer | Neueste version  | Getestet mit InternetExplorer 11  |
| Google Chrome     | Neueste version  | Getestet mit Chrome 46             |

### <a name="supported-storage-clients"></a>Unterstützte Speicher-clients 

Die folgenden softwareanforderungen sind für die iSCSI-Initiatoren, die auf Ihre StorSimple Virtual Array (als iSCSI-Server konfiguriert) zugreifen.

| **Unterstützte Betriebssysteme** | **Erforderliche Version** | **Weitere Anforderungen von Notizen** |
| --------------------------- | ---------------- | ------------- |
| WindowsServer              | 2008 R2 SP1, 2012 2012R2 |StorSimple erstellen kann, Speicherdefizite und vollständig bereitgestellte Datenmengen. Es kann keine teilweise bereitgestellte Datenmengen erstellt werden. StorSimple iSCSI Datenmengen werden nur für unterstützt: <ul><li>Einfache Datenträger auf grundlegende Windows-Festplatten.</li><li>Windows-NTFS für ein Volume formatieren.</li>|


Die folgenden softwareanforderungen sind für die SMB-Clients, die auf Ihre StorSimple Virtual Array (als Dateiserver konfiguriert) zugreifen.

| **SMB Version** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Kopieren oder Speichern von Windows Datenbankschutz File System (EFS) auf den Dateiserver StorSimple Virtual Array geschützte Dateien nicht; Dies führt zu einer nicht unterstützten Konfiguration. 

## <a name="networking-requirements"></a>Netzwerke Anforderungen 

Der folgenden Tabelle sind die Ports, die in Ihre Firewall für iSCSI, SMB, Cloud oder Management Datenverkehr geöffnet werden müssen. Bezieht sich in dieser Tabelle *in* oder *eingehenden* auf die Richtung, in der eingehenden Clientanfragen Ihrem Gerät zugreifen. *Auschecken* oder *ausgehende* verweist auf die Richtung, in denen sendet Ihr Gerät StorSimple extern, Daten jenseits der Bereitstellung: z. B. ausgehende mit dem Internet.

| **Port Nr.<sup>1</sup>** | **Vergrößern oder** | **Port-Bereich** | **Erforderlich**              | **Notizen**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Auschecken           | WAN-            | Nein                        | Ausgehender Port ist für den Zugriff auf das Internet zum Abrufen von Updates verwendet. <br></br>Der ausgehende Webproxy kann vom Benutzer konfiguriert. |
| TCP 443 (HTTPS)          | Auschecken           | WAN-            | Ja                       | Ausgehender Anschluss wird für den Zugriff auf Daten in der Cloud verwendet. <br></br>Der ausgehende Webproxy kann vom Benutzer konfiguriert. |
| UDP 53 (DNS)             | Auschecken           | WAN-            | In einigen Fällen; finden Sie unter Notizen. | Dieser Port ist erforderlich, nur, wenn Sie einen Internet-basiertem DNS-Server verwenden. <br></br> **Hinweis**: Wenn eine Dateiserver bereitstellen, wir empfehlen lokalen DNS-Server verwenden.|
| UDP 123 (NTP)            | Auschecken           | WAN-            | In einigen Fällen; finden Sie unter Notizen. | Dieser Port ist erforderlich, nur, wenn Sie einen internetbasierten NTP-Server verwenden.<br></br> **Hinweis:** Wenn eine Dateiserver bereitstellen, wird empfohlen, Zeit mit Ihrer Active Directory-Domänencontroller synchronisieren.  |
| TCP 80 (HTTP)           | In            | LAN            | Ja                       | Dies ist der eingehende Port für lokale Benutzeroberfläche auf dem Gerät StorSimple für lokale Verwaltung. <br></br> **Hinweis**: Zugriff auf die lokale Benutzeroberfläche über HTTP wird automatisch zu HTTPS umleiten.|
| TCP 443 (HTTPS)          | In            | LAN            | Ja                       | Dies ist der eingehende Port für lokale Benutzeroberfläche auf dem Gerät StorSimple für lokale Verwaltung.|
| TCP 3260 (iSCSI)         | In            | LAN            | Nein                        | Dieser Port wird verwendet, um die Daten über iSCSI zugreifen.|

<sup>1</sup> keine eingehenden Ports im öffentlichen Internet geöffnet werden müssen.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass die Firewall nicht geändert oder alle SSL-Datenverkehr zwischen StorSimple Gerät und Azure entschlüsseln.


### <a name="url-patterns-for-firewall-rules"></a>URL-Mustern für Firewall-Regeln 

Netzwerkadministratoren können häufig erweiterte Firewall-Regeln basierend auf den URL-Mustern zum Filtern der eingehenden und ausgehenden Datenverkehr konfigurieren. Ihre virtuelle Matrix und der StorSimple Manager-Dienst abhängig von anderen Microsoft-Programmen wie Azure-Dienstbus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern. Die folgenden Anwendungen zugeordneten URL-Muster können zum Konfigurieren der Firewall-Regeln verwendet werden. Es ist wichtig zu verstehen, dass die folgenden Anwendungen zugeordneten URL-Muster ändern können. Dieser Vorgang wiederum erfordert den Netzwerkadministrator zum Überwachen und Aktualisieren von Firewall-Regeln für Ihre StorSimple als und, falls erforderlich. 

Es empfiehlt sich, dass Sie die Firewall-Regeln für ausgehenden Datenverkehr, basierend auf StorSimple großzügig mit fester IP-Adressen in den meisten Fällen festlegen. Allerdings können Sie die folgenden Informationen erweiterte Firewall-Regeln festlegen, die zum Erstellen der secure Umgebungen erforderlich sind.

> [AZURE.NOTE] 
> 
> - Das Gerät (Quelle) IP-Adressen sollten immer alle Cloud aktivierten Netzwerk-Schnittstellen festgelegt werden. 
> - Das Ziel sollte [Azure Datacenter IP-Adressbereiche](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)IP-Adressen festgelegt werden.


| URL-Muster                                                      | Komponente/Funktionalität                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Dienstbus|
|`http://*.backup.windowsazure.com`|Gerät Registrierung|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicher-Konten und für die Überwachung |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  |
| `http://*.data.microsoft.com `                   | Telemetrieprotokoll-Dienst in Windows finden Sie unter das [Update für Customer Experience und Diagnose werden](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Als Nächstes

-   [Vorbereiten des Portals für die Bereitstellung Ihrer StorSimple Virtual Array](storsimple-ova-deploy1-portal-prep.md)

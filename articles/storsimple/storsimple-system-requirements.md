<properties
   pageTitle="Systemanforderungen StorSimple | Microsoft Azure"
   description="Beschreibt Software, Netzwerke, und hohen Verfügbarkeit Anforderungen und bewährte Methoden für eine Microsoft Azure StorSimple Lösung."
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
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple Software, hohe Verfügbarkeit und Netzwerke Anforderungen

## <a name="overview"></a>(Übersicht)

Willkommen bei Microsoft Azure StorSimple. In diesem Artikel werden die wichtigen systemvoraussetzungen und bewährte Methoden für den Speicher-Clients Zugriff auf das Gerät und Ihrem Gerät StorSimple. Es empfiehlt sich, dass Sie die Informationen sorgfältig überprüfen, bevor Sie Ihrem System StorSimple bereitstellen, und klicken Sie dann darauf wieder nach Bedarf während der Bereitstellung und nachfolgenden Vorgang verweisen.

Die systemvoraussetzungen umfassen:

- **Softwareanforderungen für Speicher-Clients** - beschreibt die unterstützten Betriebssysteme und alle weiteren Anforderungen für diese Betriebssysteme auf.
- **Anforderungen für das Gerät StorSimple networking** - enthält Informationen zu den Ports, die in Ihre Firewall für iSCSI, Cloud oder Management Datenverkehr geöffnet sein müssen.
- **Hohe Verfügbarkeit Anforderungen für StorSimple** - werden hohen Verfügbarkeit Anforderungen und bewährte Methoden für den StorSimple Gerät und Host Computer an. 


## <a name="software-requirements-for-storage-clients"></a>Softwareanforderungen für Speicher-clients

Die folgenden softwareanforderungen sind für den Speicher-Clients, die auf Ihrem Gerät StorSimple zugreifen.

| Unterstützte Betriebssysteme | Erforderliche Version | Weitere Anforderungen von Notizen |
| --------------------------- | ---------------- | ------------- |
| WindowsServer              | 2008 R2 SP1, 2012 2012R2 |StorSimple iSCSI Datenmengen werden für die Verwendung auf nur die folgenden Windows Datenträgertypen unterstützt:<ul><li>Einfaches Volume auf einfache Datenträger</li><li>Einfache und gespiegeltes Volume auf dynamischen Datenträger</li></ul>Windows Server 2012 dünnen bereitgestellt und ODX-Features werden unterstützt, wenn Sie ein StorSimple iSCSI Volume verwenden.<br><br>StorSimple erstellen kann, Speicherdefizite und vollständig bereitgestellte Datenmengen. Es kann keine teilweise bereitgestellte Datenmengen erstellt werden.<br><br>Formatieren eines Speicherdefizite Datenträgers kann sehr lange dauern. Es empfiehlt sich das Volume löschen, und klicken Sie dann einen neuen statt neu erstellen. Jedoch, wenn Sie immer noch möchten Sie lieber ein Volume neu formatieren:<ul><li>Führen Sie den folgenden Befehl vor der neu formatieren Speicherplatz freigeben verzögert zu vermeiden: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nachdem die Formatierung abgeschlossen ist, verwenden Sie den folgenden Befehl zum Freigeben von Speicherplatz wieder zu aktivieren:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Wenden Sie das Update für Windows Server 2012, wie in [KB 2878635](https://support.microsoft.com/kb/2870270) auf Ihren Computer Windows Server beschrieben.</li></ul></li></ul></ul> Wenn Sie StorSimple Snapshot-Manager oder StorSimple Netzwerkadapter für SharePoint konfigurieren möchten, wechseln Sie zu [softwareanforderungen für optionale Komponenten](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 und 6.0 | Mit VMWare vSphere unterstützt als iSCSI-Client. VAAI-Block Feature wird mit VMware vSphere auf StorSimple Geräten unterstützt.
| Linux RHEL/CentOS | 5, 6 und 7 | Unterstützung für Linux iSCSI-Clients mit Open-iSCSI-Initiator Versionen 5, 6 und 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] Betriebssystemen wird mit StorSimple derzeit nicht unterstützt.

## <a name="software-requirements-for-optional-components"></a>Softwareanforderungen für optionale Komponenten

Die folgenden softwareanforderungen sind für die optionalen StorSimple Komponenten (StorSimple Snapshot-Manager und StorSimple Netzwerkadapter für SharePoint).

| Komponente | Host-Plattform | Weitere Anforderungen von Notizen |
| --------------------------- | ---------------- | ------------- |
| StorSimple Snapshot-Manager | Windows Server 2008 R2 SP1, 2012 2012R2 | Verwenden von StorSimple Snapshot Manager unter Windows Server ist gespiegelt dynamische Datenträger sichern/wiederherstellen und für eine Anwendung konsistent Sicherungen erforderlich.<br> StorSimple Snapshot-Manager wird nur unter Windows Server 2008 R2 SP1 (64-Bit), Windows 2012 R2 und Windows Server 2012 unterstützt.<ul><li>Wenn Sie im Fenster Server 2012 verwenden, müssen Sie vor der Installation von StorSimple Snapshot-Manager .NET 3.5 – 4.5 installieren.</li><li>Wenn Sie Windows Server 2008 R2 SP1 arbeiten, installieren Sie Windows Management Framework 3.0 vor der Installation von StorSimple Snapshot-Manager.</li></ul> |
| StorSimple Netzwerkadapter für SharePoint | Windows Server 2008 R2 SP1, 2012 2012R2 |<ul><li>StorSimple Netzwerkadapter für SharePoint wird nur auf SharePoint 2010 und SharePoint 2013 unterstützt.</li><li>RSP-Code erfordert SQL Server Enterprise Edition, Version 2008 R2 oder 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Netzwerke Anforderungen für Ihr Gerät StorSimple

Ihr Gerät StorSimple ist einem gesperrten Gerät. Müssen jedoch Ports in Ihre Firewall für iSCSI, Cloud und Management Datenverkehr geöffnet werden. Die folgende Tabelle listet die Ports, die in der Firewall geöffnet werden müssen. Bezieht sich in dieser Tabelle *in* oder *eingehenden* auf die Richtung, in der eingehenden Clientanfragen Ihrem Gerät zugreifen. *Auschecken* oder *ausgehende* verweist auf die Richtung, in denen sendet Ihr Gerät StorSimple extern, Daten jenseits der Bereitstellung: z. B. ausgehende mit dem Internet.

| Port Nr.<sup>1,2</sup> | Vergrößern oder | Port-Bereich | Erforderlich | Notizen |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Auschecken |  WAN- | Nein |<ul><li>Ausgehender Port ist für den Zugriff auf das Internet zum Abrufen von Updates verwendet.</li><li>Der ausgehende Webproxy kann vom Benutzer konfiguriert.</li><li>Um Systemupdates zulassen möchten, muss auch dieses Port für den Controller feste IP-Adressen geöffnet sein.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Auschecken | WAN- | Ja |<ul><li>Ausgehender Anschluss wird für den Zugriff auf Daten in der Cloud verwendet.</li><li>Der ausgehende Webproxy kann vom Benutzer konfiguriert.</li><li>Um Systemupdates zulassen möchten, muss diese Port auch für den Controller feste IP-Adressen geöffnet sein.</li><li>Dieser Port wird auch auf die beiden Controller für Garbagecollection verwendet.</li></ul>|
|UDP 53 (DNS) | Auschecken | WAN- | In einigen Fällen; finden Sie unter Notizen. |Dieser Port ist erforderlich, nur, wenn Sie einen Internet-basiertem DNS-Server verwenden. |
| UDP 123 (NTP) | Auschecken | WAN- | In einigen Fällen; finden Sie unter Notizen. |Dieser Port ist erforderlich, nur, wenn Sie einen internetbasierten NTP-Server verwenden. |
| TCP 9354 | Auschecken | WAN- | Ja |Der ausgehende Anschluss wird durch das Gerät StorSimple zur Kommunikation mit der StorSimple Manager-Dienst verwendet. |
| 3260 (iSCSI) | In | LAN | Nein | Dieser Port wird verwendet, um die Daten über iSCSI zugreifen.|
| 5985 | In | LAN | Nein | Eingehender Port wird durch StorSimple Snapshot-Manager zur Kommunikation mit dem Gerät StorSimple verwendet.<br>Dieser Anschluss wird auch verwendet, wenn Sie Remote mit Windows PowerShell für StorSimple über HTTP verbinden. |
| 5986 | In | LAN | Nein | Dieser Anschluss wird verwendet, wenn Sie Remote mit Windows PowerShell für StorSimple über HTTPS verbinden. |

<sup>1</sup> keine eingehenden Ports im öffentlichen Internet geöffnet werden müssen.

<sup>2</sup> Wenn mehrere Ports Gateway-Konfiguration ausgeführt haben, wird die Reihenfolge der ausgehenden weitergeleiteten Datenverkehr bestimmt basierend auf der Weiterleitung nach Port [Port routing](#routing-metric)unter.

<sup>3</sup> der feste IP-Adressen auf Ihrem Gerät StorSimple Controller muss geroutet und eine Verbindung mit dem Internet herstellen. Die festen IP-Adressen werden für die Wartung der Updates auf dem Gerät verwendet. Wenn die Gerätecontroller über die feste IP-Adressen mit dem Internet verbinden können, werden Sie nicht auf Ihrem Gerät StorSimple aktualisieren sein.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass die Firewall nicht geändert oder alle SSL-Datenverkehr zwischen StorSimple Gerät und Azure entschlüsseln.

### <a name="url-patterns-for-firewall-rules"></a>URL-Mustern für Firewall-Regeln

Netzwerkadministratoren können häufig erweiterte Firewall-Regeln basierend auf den URL-Mustern zum Filtern der eingehenden und ausgehenden Datenverkehr konfigurieren. Ihr Gerät StorSimple und der StorSimple Manager-Dienst abhängig von anderen Microsoft-Programmen wie Azure-Dienstbus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern. Die folgenden Anwendungen zugeordneten URL-Muster können zum Konfigurieren der Firewall-Regeln verwendet werden. Es ist wichtig zu verstehen, dass die folgenden Anwendungen zugeordneten URL-Muster ändern können. Dieser Vorgang wiederum erfordert den Netzwerkadministrator zum Überwachen und Aktualisieren von Firewall-Regeln für Ihre StorSimple als und, falls erforderlich.

Es empfiehlt sich, dass Sie die Firewall-Regeln für ausgehenden Datenverkehr, basierend auf StorSimple großzügig mit fester IP-Adressen in den meisten Fällen festlegen. Allerdings können Sie die folgenden Informationen erweiterte Firewall-Regeln festlegen, die zum Erstellen der secure Umgebungen erforderlich sind.

> [AZURE.NOTE] Das Gerät (Quelle) IP-Adressen sollten immer alle aktivierten Netzwerkschnittstellen festgelegt werden. Das Ziel sollte [Azure Datacenter IP-Adressbereiche](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)IP-Adressen festgelegt werden.

#### <a name="url-patterns-for-azure-portal"></a>URL-Mustern für Azure-portal
| URL-Muster                                                      | Komponente/Funktionalität                                           | Gerät IP-Adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Dienstbus| Cloud-fähigen Netzwerk-Schnittstellen        |
|`https://*.backup.windowsazure.com`|Gerät Registrierung| Nur Daten 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |Cloud-fähigen Netzwerk-Schnittstellen |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicher-Konten und für die Überwachung | Cloud-fähigen Netzwerk-Schnittstellen        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                             | Controller nur mit fester IP-Adressen               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Controller nur mit fester IP-Adressen   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  | Cloud-fähigen Netzwerk-Schnittstellen        |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-Mustern for Government Azure-portal
| URL-Muster                                                      | Komponente/Funktionalität                                           | Gerät IP-Adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Dienstbus| Cloud-fähigen Netzwerk-Schnittstellen        |
|`https://*.backup.windowsazure.us`|Gerät Registrierung| Nur Daten 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |Cloud-fähigen Netzwerk-Schnittstellen |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicher-Konten und für die Überwachung | Cloud-fähigen Netzwerk-Schnittstellen        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                             | Controller nur mit fester IP-Adressen               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Controller nur mit fester IP-Adressen   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  | Cloud-fähigen Netzwerk-Schnittstellen        |

### <a name="routing-metric"></a>Weiterleitung Metrisch

Eine Weiterleitung Metrik ist die Schnittstellen und Gateways, die die Daten zu den angegebenen Netzwerken weiterleiten zugeordnet. Weiterleitung Metrisch routing-Protokoll dient zum Berechnen des optimale Pfads für ein bestimmtes Ziel, wenn es erfährt, dass mehrere Pfade zu demselben Ziel vorhanden sind. Je niedriger der Weiterleitung Metrik, je höher die Einstellung.

Im Kontext StorSimple Wenn mehrere Netzwerk-Schnittstellen und Gateways, für den Verkehr zu erstellen konfiguriert sind, wird die Weiterleitung Metrik kommen ins Spiel zum Festlegen der relativen Reihenfolge, in der die Schnittstellen gewöhnen. Die Weiterleitung Metrik kann vom Benutzer nicht geändert werden. Sie können jedoch die `Get-HcsRoutingTable` Cmdlet routing Tabelle (und Kennzahlen) auf Ihrem Gerät StorSimple ausgegeben. Weitere Informationen zum Cmdlet "Get-HcsRoutingTable" in [Problembehandlung StorSimple Bereitstellung](storsimple-troubleshoot-deployment.md).

Die Weiterleitung metrischen Algorithmen unterscheiden sich je nach der Softwareversion auf Ihrem Gerät StorSimple ausgeführt.

**Versionen vor Update 1**

Dies umfasst Softwareversionen vor Update 1, wie etwa die GA, 0.1, 0,2 und 0,3 Release. Die Reihenfolge Grundlage Weiterleitung Metrik lautet wie folgt aus:

   *Letzte 10 Switch-Schnittstelle konfiguriert > andere 10 Switch-Schnittstelle > 1 Switch-Schnittstelle der zuletzt konfigurierten > andere 1 Switch-Schnittstelle*


**Starten vor dem Update 2 und Update 1 Versionen**

Dies umfasst Softwareversionen, wie z. B. 1, 1.1 oder 1.2. Die Reihenfolge Grundlage Weiterleitung Metrik ist wie folgt entschieden:

   *Daten 0 > 10 Switch-Schnittstelle der zuletzt konfigurierten > andere 10 Switch-Schnittstelle > letzte 1 Switch-Schnittstelle konfiguriert > andere 1 Switch-Schnittstelle*

   In Update 1, erfolgt die Weiterleitung Metrik Datenseite 0 niedrigsten; Daher wird die gesamte Cloud-Verkehr über Daten 0 geleitet. Notieren Sie sich diesen Wenn mehr als eine Cloud-fähige Netzwerkschnittstelle auf Ihrem Gerät StorSimple vorhanden sind.


**Beginnend mit Update 2 Versionen**

Update 2 weist verschiedene Netzwerke miteinander Verbesserungen und die Weiterleitung Metrik hat sich geändert. Das Verhalten kann wie folgt erklärt werden.

- Eine Reihe von vordefinierten Werten zu Netzwerk-Schnittstellen zugewiesen wurden.   

- Erwägen Sie eine Beispieltabelle abgebildet mit Werten, die verschiedenen Netzwerkschnittstellen zugewiesen ist, wenn sie Cloud-aktiviert sind oder Cloud-deaktiviert, aber ein Gateway konfiguriert. Beachten Sie, dass die zugehörigen Werte hier nur Beispielwerte sind.


  	| Netzwerk-Benutzeroberfläche | Cloud aktiviert | Cloud mit deaktivierter mit gateway |
  	|-----|---------------|---------------------------|
  	| Daten 0  | 1            | -                        |
  	| Daten 1  | 2            | 20                       |
  	| Daten 2  | 3            | 30                       |
  	| Daten 3  | 4            | 40                       |
  	| Daten 4  | 5            | 50                       |
  	| Daten 5  | 6            | 60                       |


- Ist die Reihenfolge, in der die Cloud er über die Netzwerkschnittstellen weitergeleitet wird:

    *Daten 0 > Daten 1 > Datum 2 > Daten 3 > Daten 4 > Daten 5*

    Dies kann durch das folgende Beispiel erläutert werden.

    Erwägen Sie ein Gerät StorSimple mit zwei Cloud aktivierten Netzwerk-Schnittstellen Daten 0 und 5 Daten ein. Daten 1 bis 4 Daten Cloud-deaktiviert sind, aber einen Gateway konfigurierten haben. Kann ich die Reihenfolge, in der er für dieses Gerät weitergeleitet wird:

    *Daten 0 (1) > Daten 5 (6) > Daten 1 (20) > Daten 2 (30) > Daten 3 (40) > Daten 4 (50)*

    *die Zahlen in Klammern anzugeben, wo die jeweiligen routing Metrik.*

    Wenn Daten 0 fehlschlägt, werden der Datenverkehr Cloud bis 5 Daten weitergeleitet abrufen. Vorausgesetzt, dass wäre sowohl Daten 0 und 5 Daten zum Fehlschlagen ein Gateways auf alle anderen Netzwerk konfiguriert ist, werden der Cloud-Verkehr über Data 1 geleitet.


- Wenn eine Schnittstelle Cloud-fähigen Netzwerk fehlschlägt, sind 3 Wiederholungsversuche mit eine Verzögerung von 30 Sekunden in Verbindung mit der Benutzeroberfläche. Wenn alle die Versuche fehlschlagen, wird auf die nächste verfügbare Cloud-fähige Schnittstelle wie durch die routing-Tabelle bestimmt der Datenverkehr weitergeleitet. Wenn das Cloud-fähige Netzwerk Fail-Schnittstellen, wird das Gerät an den anderen Controller (keine Neustart in diesem Fall) über fehl.

- Ist ein Fehler VIP für eine Netzwerkschnittstelle iSCSI aktiviert, werden mit einer 2 Sekunden Verzögerung 3 Wiederholungsversuche. Dieses Verhalten weist aus früheren Versionen derselben immer noch. Wenn alle iSCSI-Netzwerk-Schnittstellen ein Fehler auftreten, wird ein Failover Controller (zusammen mit einem Neustart) auftreten.


- Eine Warnung wird auch auf Ihrem Gerät StorSimple ausgelöst, wenn ein Fehler VIP vorliegt. Weitere Informationen zu [Benachrichtigen Kurzübersicht](storsimple-manage-alerts.md)wechseln.

- Im Hinblick auf Wiederholungsversuche Vorrang iSCSI Cloud.

    Im folgenden Beispiel: A StorSimple Gerät verfügt über zwei Netzwerkschnittstellen aktiviert, Daten 0 und 1 Daten. Daten 0 sind Cloud aktiviert, während die Daten 1 beide Cloud ist und iSCSI aktiviert. Keine anderen Netzwerkschnittstellen auf diesem Gerät sind für Cloud oder iSCSI aktiviert.

    Daten 1 fehlschlägt, ihr zugewiesen ist der letzten iSCSI-Schnittstelle, wird in einem Controller Failover 1 Daten auf dem anderen Controller erstellt.


### <a name="networking-best-practices"></a>Bewährte Methoden für Netzwerke

Halten Sie in die folgenden bewährten Methoden, zusätzlich zu den Vorschriften über die Netzwerken, für die optimale Leistung Ihrer Lösung StorSimple:

- Stellen Sie sicher, dass Ihr Gerät StorSimple eine dedizierte 40/s Bandbreite (oder mehr) ist jederzeit zur Verfügung stehen. Diese Bandbreite nicht freigegeben werden soll (oder Zuteilung durch die Verwendung von QoS-Richtlinien sichergestellt werden soll) mit einem beliebigen anderen Anwendungen.

- Stellen Sie sicher, dass Netzwerkkonnektivität mit dem Internet jederzeit verfügbar ist. Gelegentliche oder unzuverlässige Internet-Verbindungen zu den Geräten, einschließlich keine Verbindung mit dem Internet machen, führt zu einer nicht unterstützten Konfiguration.

- Identifizieren Sie den Datenverkehr iSCSI und Cloud, indem Sie jeweils eigener Netzwerk-Schnittstellen auf Ihrem Gerät für iSCSI und Cloud-Zugriff. Weitere Informationen finden Sie unter so auf Ihrem Gerät StorSimple [Netzwerk-Schnittstellen ändern](storsimple-modify-device-config.md#modify-network-interfaces) .

- Verwenden Sie eine Link Aggregation Steuerelement Protocol (LACP) Konfiguration nicht für Ihre Netzwerk-Schnittstellen aus. Dies ist eine nicht unterstützte Konfiguration.


## <a name="high-availability-requirements-for-storsimple"></a>Hohe Verfügbarkeit Anforderungen für StorSimple

Hardware-Plattform, die mit der Lösung StorSimple enthalten ist wartet mit Verfügbarkeit und Zuverlässigkeit Funktionen, die eine Grundlage für eine hoch verfügbare und Fehlertoleranz Speicherinfrastruktur im Rechenzentrum bereitstellen. Es gibt jedoch Anforderungen sowie optimale Methoden, denen Sie einhalten sollte, um sicherzustellen, dass die Verfügbarkeit der Lösung StorSimple. Bevor Sie StorSimple bereitstellen, überprüfen Sie sorgfältig die folgenden Anforderungen und bewährte Methoden für die StorSimple Gerät und verbundenen Hostcomputer.

Weitere Informationen für die Überwachung und Verwaltung der Hardwarekomponenten von Ihrem Gerät StorSimple wechseln Sie zu [Verwenden der Dienst StorSimple Manager Hardware-Komponenten und den Status überwachen](storsimple-monitor-hardware-status.md) und [Austausch von StorSimple Hardware Komponenten](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Hohe Verfügbarkeit Anforderungen und Verfahren für Ihr Gerät StorSimple

Überprüfen Sie die folgende Informationen sorgfältig, um die hohen Verfügbarkeit von Ihrem Gerät StorSimple sicherzustellen.

#### <a name="pcms"></a>PCMs

StorSimple Geräten zählen redundante, austauschbare Power und Kühlmodule (PCMs). Jede PCM verfügt über ausreichend Kapazität, für den gesamten Rahmen Dienste bereitzustellen. Um eine hohe Verfügbarkeit sicherzustellen, müssen beide PCMs installiert sein.

- Herstellen einer Verbindung anderen Power Quellen Verfügbarkeit bereitstellen, wenn es sich bei eine Quelle Power schlägt fehl mit Ihrer PCMs.
- Wenn eine PCM fehlschlägt, fordern Sie Ersatz sofort an.
- Entfernen Sie eine fehlgeschlagene PCM nur, wenn Sie die Ersetzung haben und zur Installation bereit sind.
- Beide PCMs nicht gleichzeitig entfernen. Das Modul PCM enthält das Stützbatterie Modul. Entfernen beide die PCMs führt zu Fehlern in einer war(en) ohne Akku Schutz und der Zustand Gerät wird nicht gespeichert werden. Weitere Informationen zu den Akku wechseln Sie zu [Verwalten der Sicherungsbatterie Modul](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Controller Module

StorSimple Geräte gehören redundante, austauschbare Controller Module. Die Controller Module arbeiten in Aktiv/Passiv. Bei einem beliebigen Zeitpunkt eine Controller-Modul aktiv ist und ist Dienst bereitstellen, während das anderen Controller-Modul passiven ist. Das passive Controller-Modul eingeschaltet ist und Betrieb wird, wenn das aktiven Controller-Modul fehlschlägt oder entfernt. Jeder Controller-Modul verfügt über ausreichend Kapazität, für den gesamten Rahmen Dienste bereitzustellen. Beide Controller Module müssen installiert sein, um eine hohen Verfügbarkeit sicherzustellen.

- Stellen Sie sicher, dass alle Vorkommen beide Controller Module installiert sind.

- Wenn ein Modul Controller fehlschlägt, fordern Sie Ersatz sofort an.

- Entfernen Sie eine fehlgeschlagene Controller-Modul nur, wenn Sie die Ersetzung haben und zur Installation bereit sind. Entfernen ein Modul für längere Zeiträume den Luftstrom beeinträchtigt und somit auch die Kühlung des Systems.

- Stellen Sie sicher, dass die Netzwerk-Verbindungen mit beide Controller Module identisch sind und die verbundenen Netzwerk-Schnittstellen eine identische Netzwerkkonfiguration haben.

- Wenn ein Modul Controller fehlschlägt oder Ersatz benötigt, stellen Sie sicher, dass das Controller-Modul in einem aktiven Zustand ist, bevor Sie das fehlerhaften Controller-Modul ersetzen. Um zu überprüfen, dass ein Controller aktiv ist, wechseln Sie zum [Identifizieren der aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Entfernen Sie beide Controller Module nicht gleichzeitig an. Wenn ein Controller Failover ausgeführt wird, gehen Sie wie folgt nicht fahren Sie die standby Controller-Modul oder aus dem Rahmen entfernen.

- Warten Sie mindestens 5 Minuten vor dem Entfernen eines Controller-Modul nach einem Failover Controller.

#### <a name="network-interfaces"></a>Netzwerk-Schnittstellen

StorSimple Gerät Controller Module jedes haben vier 1 Gigabit- und zwei 10 Gigabit Ethernet-Netzwerk-Schnittstellen.

- Stellen Sie sicher, dass die Netzwerk-Verbindungen mit beide Controller Module identisch sind und das Netzwerk Schnittstellen, dass die Controller Modul Schnittstellen verbunden sind, damit eine identische Netzwerkkonfiguration.

- Wenn möglich, Bereitstellen Sie Netzwerk Verbindungen über unterschiedliche Optionen, um sicherzustellen, dass dienstverfügbarkeit bei einem Gerät.

- Wenn Sie nur oder das letzte verbleibende iSCSI-fähige Schnittstelle (mit IP-Adressen zugewiesen) abziehen, deaktivieren Sie zuerst die Benutzeroberfläche und ziehen Sie dann die Kabel. Wenn die Benutzeroberfläche zuerst entfernt wird, wird es den aktiven Controller über an den passiven Controller fehlschlägt verursachen. Wenn der passive Controller ebenfalls seiner entsprechenden Schnittstellen wurde entfernt wurden, werden dann sowohl die Controller mehrmals neu gestartet, bevor Sie auf einen Controller aus.

- Verbinden Sie mindestens zwei Schnittstellen mit dem Netzwerk aus jedem Modul Controller.

- Wenn Sie die beiden aktiviert haben Bereitstellen von 10 GbE-Schnittstellen, die über unterschiedliche Optionen.

- Verwenden Sie nach Möglichkeit MPIO auf Servern, um sicherzustellen, dass die Server ein Link, Netzwerk- oder Benutzeroberflächen-Fehler tolerieren können.

Für Weitere Informationen zu Netzwerken Ihr Gerät für hohen Verfügbarkeit und Leistung, wechseln Sie zu [Ihrem Gerät StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) [Installieren](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)oder Ihrem Gerät StorSimple 8600.

#### <a name="ssds-and-hdds"></a>SSDs und Festplatten

StorSimple Geräten zählen einfarbige Zustand Datenträger (SSDs) und Festplatten (Festplatten), die geschützt sind mit gespiegelt Leerzeichen. Verwenden von gegenüberliegenden Leerzeichen: Damit ist sichergestellt, dass das Gerät den Ausfall der SSDs oder Festplatten tolerieren kann.

- Stellen Sie sicher, dass alle SSD und Festplatte Module installiert sind.

- Wenn eine SSD oder Festplatte fehlschlägt, wird fordern Sie kein Ersatz sofort an.

- Wenn eine SSD oder Festplatte fehlschlägt oder Ersatz erfordert, stellen Sie sicher, dass Sie nur die SSD oder Festplatte, die ersetzt werden muss, entfernen.

- Entfernen Sie nicht mehr als eine SSD oder Festplatte aus dem System zu einem beliebigen Zeitpunkt Zeitpunkt.
Ein Fehler von 2 oder mehr Datenträger eines bestimmten Typs (Festplatte, SSD) oder aufeinander folgender Fehler innerhalb kürzester Zeit möglicherweise System Fehlfunktion und potenzieller Datenverlust führen. In diesem Fall, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um Unterstützung.

- Überwachen Sie während Ersatz der **Hardware Status** in der Seite **zum Warten** , für die Laufwerke SSDs und Festplatten. Grünes Häkchen Status zeigt an, dass die Datenträger sind fehlerfrei oder OK, während eine rote Ausrufezeichen verweisen zeigt an, einen Fehler beim SSD oder Festplatte.

- Es empfiehlt sich, dass Sie Cloud Momentaufnahmen für alle Datenträger konfiguriert werden, die Sie bei einem Systemfehler geschützt werden muss.

#### <a name="ebod-enclosure"></a>EBOD Einheit

StorSimple Gerätemodell 8600 enthält eine Anlage erweiterten Gruppe der Datenträger (EBOD) sowie die primäre Einheit an. Eine EBOD enthält EBOD Controller und Festplatten (Festplatten), die geschützt sind mit gespiegelt Leerzeichen. Verwenden von gegenüberliegenden Leerzeichen: Damit ist sichergestellt, dass das Gerät den Fehler an einen oder mehrere Festplatten tolerieren kann. Die Anlage EBOD ist mit der primären Einheit durch redundante SAS-Kabel verbunden.

- Stellen Sie sicher, dass beide EBOD Einheit Controller Module, sowohl SAS-Kabel und alle jederzeit die Festplatten installiert sind.

- Wenn eine EBOD Einheit Controller-Modul fehlschlägt, fordern Sie Ersatz sofort an.

- Wenn eine EBOD Einheit Controller-Modul fehlschlägt, stellen Sie sicher, dass das Controller-Modul aktiv ist, bevor Sie das fehlerhafte Modul ersetzen. Um zu überprüfen, dass ein Controller aktiv ist, wechseln Sie zum [Identifizieren der aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Während einer EBOD Controller Modul Ersatz, überwachen Sie kontinuierlich den Status der Komponente in der StorSimple Manager-Dienst, indem Sie den Zugriff auf die **Wartung** > **Hardwarestatus**.

- Wenn eine SAS-Kabel fehlschlägt oder Ersatz (Microsoft Support sollte beteiligt sein, um diese Feststellung vorzunehmen) erfordert, stellen Sie sicher, dass Sie nur das SAS-Kabel entfernen, das ersetzt werden muss.

- Gleichzeitig entfernen Sie nicht beide SAS-Kabel aus dem System an einer beliebigen Stelle in der Zeit.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Hohe Verfügbarkeit Empfehlungen für Ihre Host-Computern

Überprüfen Sie sorgfältig bewährten zur Sicherstellung der hohen Verfügbarkeit von Hosts bei einer Verbindung zu Ihrem Gerät StorSimple ein.

- Konfigurieren von StorSimple mit [zwei Knoten Datei Server Clusterkonfigurationen][1]. Durch das Entfernen einzelner interessante Fehler und Redundanz auf der Hostseite erstellen, wird die gesamte Lösung hochgradig verfügbar.

- Verwenden Sie ständig verfügbare (CA) verfügbaren Freigaben mit Windows Server 2012 (SMB 3.0) für eine hohe Verfügbarkeit bei Failover von Speicher-Controller ein. Weitere Informationen zum Konfigurieren von Datei-Server-Cluster und kontinuierlich verfügbare Freigaben mit Windows Server 2012 finden Sie in diesem [video Demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Nächste Schritte

- [Sie erhalten grundlegende Informationen zu den Grenzwerten StorSimple-System](storsimple-limits.md).
- Sie [erfahren, wie Sie Ihre Lösung StorSimple bereitstellen](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx

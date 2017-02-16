<properties 
   pageTitle="Informationen zu den neuesten Azure Gast OS Versionen | Microsoft Azure" 
   description="Neuigkeiten Release und SDK Kompatibilität für Azure Cloud Services Gast OS." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure Gast-BS-Versionen und SDK Kompatibilitätsmatrix
Bietet Ihnen aktuelle Informationen zu den neuesten Azure Gast OS für Cloud Services frei. Diese Informationen helfen Ihnen, Ihre Upgradepfad planen, bevor eine OS Gast deaktiviert ist. Wenn Sie Ihre Rollen zum *automatischen* Updates für OS Gast verwenden in [Azure Gast OS Update-Einstellungen][]beschriebenen konfiguriert haben, ist es nicht entscheidend, dass Sie diese Seite lesen.

> [AZURE.IMPORTANT] Diese Seite bezieht sich auf Cloud Services Web und Worker Rollen, die auf einem Gast-Betriebssystem ausführen. Dies geschieht zu IaaS virtuellen Computern **nicht angewendet** . 

<!-- -->

> [AZURE.TIP] Den [Gast OS Update RSS-Feed] abonnieren[ rss] der am häufigsten schnell alle Gast-BS Änderungen benachrichtigt werden soll.

Ist nicht sicher, über die Gast-BS oder wie frei Gast-BS Arbeit? Lesen Sie [diesen](#how-it-works) Abschnitt ein.

## <a name="news-updates"></a>Neuigkeiten
###### <a name="october-23-2016"></a>**Oktober 23 2016**
Windows Server 2016 wird als OS Familie 5 auf 1 2016 November, mit .NET 4.6 Support freigegeben werden.

###### <a name="september-13-2016"></a>**September 13 2016**
September Gast OS UC-Bereitstellung September 13 2016 wird gestartet, und Vorschau, um 13 2016 Oktober freigegeben werden.

###### <a name="august-9-2016"></a>**August 9 2016**
August Gast OS UC-Bereitstellung August 9 2016 wird gestartet, und Vorschau, um auf 8 2016 September freigegeben werden. 

###### <a name="july-13-2016"></a>**Juli 13 2016**
Juli Gast OS UC-Bereitstellung Juli 13 2016 wird gestartet, und Vorschau, um auf August 12 2016 freigegeben werden. 

###### <a name="june-15-2016"></a>**15 2016 Juni**
Juni Gast OS UC-Bereitstellung Juni 15 2016 wird gestartet, und Vorschau, um auf 14 2016 Juli freigegeben werden. 

###### <a name="may-17-2016"></a>**Mai 17 2016**
Mai Gast-BS UC-Bereitstellung 17 2016 Mai wird gestartet, und Vorschau, um auf 10 2016 Juni freigegeben werden. 

###### <a name="april-18-2016"></a>**18 2016 April**
April Gast OS UC-Bereitstellung April 18 2016 wird gestartet, und Vorschau, um auf Mai 12 2016 freigegeben werden. 

###### <a name="march-14-2016"></a>**März 14 2016**
März Gast OS UC-Bereitstellung März 14 2016 wird gestartet, und Vorschau, um auf April 8 2016 freigegeben werden. 

###### <a name="february-22-2016"></a>**Februar 22 2016**
Februar Gast OS UC-Bereitstellung Februar 22 2016 wird gestartet, und Vorschau, um auf 9 2016 März freigegeben werden.

###### <a name="january-18-2016"></a>**Januar 18 2016**
Januar Gast OS UC-Bereitstellung Januar 18 2016 wird gestartet, und Vorschau, um auf Februar 12 2016 freigegeben werden.

###### <a name="january-4-2016"></a>**Januar 4 2016**
November 201511-02 Gast OS wurde veröffentlicht auf 4 Januar 2016 für die Bereitstellung. Dieser Version des Betriebssystems wird nicht als Standard OS für die automatische Aktualisierung festgelegt, weshalb provisioning Zeitpunkt der Bereitstellung von Gast-BS bis November 201511-02 Betriebssystemversion etwas länger wäre. 

## <a name="releases"></a>Versionen

## <a name="family-4-releases"></a>Familie 4 Versionen
**Windows Server 2012 R2**

Unterstützt .NET 4.0, 4.5, 4.5.1, 4.5.2

>[AZURE.NOTE] Daten mit einer * Ankündigung geändert werden

| Konfigurationszeichenfolge           | Datum der Freigabe    | Deaktivieren Sie Datum  | Abgelaufene Datum |
| ------------------------------ | --------------- | ------------- | ---- |
| WA-GAST-OS-4.36_201609-01     | Okt 13 2016     | Beitrag 4,38     | TBD |
| WA-GAST-OS-4.35_201608-01     | September 13 2016    | Beitrag 4.37     | TBD |
| WA-GAST-OS-4.34_201607-01     | Aug 8 2016      | Beitrag 4.36     | TBD |
| WA-GAST-OS-4.33_201606-01     | Juli 13 2016    | Okt 13 2016   | TBD |
| WA-GAST-OS-4.32_201605-01     | 10 2016 Juni    | September 8 2016   | TBD |
| WA-GAST-OS-4.31_201604-01     | 2 2016 Mai      | Aug 13 2016   | TBD |
| WA-GAST-OS-4.30_201603-01     | 7 2016 April    | 10 2016 Juli  | TBD |
| WA-GAST-OS-4.29_201602-02     | 12 2016 März   | 2 2016 Juni   | TBD |
| WA-GAST-OS-4.28_201601-01     | Feb 12 2016     | 7 2016 Mai    | TBD | 
| WA-GAST-OS-4.27_201512-01     | Jan 12 2016     | 12 2016 April | TBD |
| ~~WA-GAST-OS-4.26_201511-02~~ | Jan 4 2016      | 12 2016 März | TBD |
| ~~WA-GAST-OS-4.26_201511-01~~ | 10 2015 Dez     | 12 2016 März | TBD |
| ~~WA-GAST-OS-4.25_201510-01~~ | Nov 6 2015      | Feb 12 2016   | TBD |
| ~~WA-GAST-OS-4.24_201509-01~~ | Okt 1 2015      | Jan 10 2016   | TBD |
| ~~WA-GAST-OS-4.23_201508-02~~ | Sep 9 2015      | 6 2015 Dez    | TBD |
| ~~WA-GAST-OS-4.22_201507-02~~ | Aug 7 2015      | 1 2015 Nov    | TBD |
| ~~WA-GAST-OS-4.21_201506-01~~ | Juli 9 2015     | Okt 9 2015    | TBD |
| ~~WA-GAST-OS-4.20_201505-02~~ | Juni 12 2015    | Sep 7 2015    | TBD |
| ~~WA-GAST-OS-4.19_201504-01~~ | April 17 2015   | Aug 9 2015    | TBD |

## <a name="family-3-releases"></a>Familie 3-Versionen

**WindowsServer 2012**

Unterstützt .NET 4.0, 4.5, 4.5.1, 4.5.2

>[AZURE.NOTE] Daten mit einer * Ankündigung geändert werden

| Konfigurationszeichenfolge           | Datum der Freigabe   | Deaktivieren Sie Datum  | Abgelaufene Datum |
| ------------------------------ | -------------- | ------------- | --- |
| WA-GAST-OS-3.43_201609-01     | Okt 13 2016    | Beitrag 3.45     | TBD |
| WA-GAST-OS-3.42_201608-01     | September 13 2016   | Beitrag 3.44     | TBD |
| WA-GAST-OS-3.41_201607-01     | Aug 8 2016     | Beitrag 3.43     | TBD |
| WA-GAST-OS-3.40_201606-01     | Juli 13 2016   | Okt 13 2016   | TBD |
| WA-GAST-OS-3.39_201605-01     | 10 2016 Juni   | September 8 2016   | TBD |
| WA-GAST-OS-3.38_201604-01     | 2 2016 Mai     | Aug 13 2016   | TBD |
| WA-GAST-OS-3.37_201603-01     | 7 2016 April   | 10 2016 Juli  | TBD |
| WA-GAST-OS-3.36_201602-02     | 12 2016 März  | 2 2016 Juni   | TBD |
| WA-GAST-OS-3.35_201601-01     | Feb 12 2016    | 7 2016 Mai    | TBD |
| WA-GAST-OS-3.34_201512-01     | Jan 12 2016    | 12 2016 April | TBD |
| ~~WA-GAST-OS-3.33_201511-02~~ | Jan 4 2016     | 12 2016 März | TBD |
| ~~WA-GAST-OS-3.33_201511-01~~ | 10 2015 Dez    | 12 2016 März | TBD |
| ~~WA-GAST-OS-3.32_201510-01~~ | Nov 6 2015     | Feb 12 2016   | TBD |
| ~~WA-GAST-OS-3.31_201509-01~~ | Okt 1 2015     | Jan 10 2016   | TBD |
| ~~WA-GAST-OS-3.30_201508-02~~ | Sep 9 2015     | 6 2015 Dez    | TBD |
| ~~WA-GAST-OS-3.29_201507-02~~ | Aug 7 2015     | 1 2015 Nov    | TBD |
| ~~WA-GAST-OS-3.28_201506-01~~ | Juli 9 2015    | Okt 9 2015    | TBD |
| ~~WA-GAST-OS-3.27_201505-02~~ | Juni 12 2015   | Sep 7 2015    | TBD |
| ~~WA-GAST-OS-3.26_201504-01~~ | April 17 2015  | Aug 9 2015    | TBD |


## <a name="family-2-releases"></a>Familie 2 frei

**Windows Server 2008 R2 SP1**

Unterstützt .NET 3.5, 4.0, 4.5, 4.5.1, 4.5.2

>[AZURE.NOTE] Daten mit einer * Ankündigung geändert werden

| Konfigurationszeichenfolge           | Datum der Freigabe  | Deaktivieren Sie Datum  | Abgelaufene Datum |
| ------------------------------ | ------------- | ------------  | --- |
| WA-GAST-OS-2.55_201609-01     | Okt 13 2016   | Beitrag 2.57     | TBD |
| WA-GAST-OS-2.54_201608-01     | September 13 2016  | Beitrag 2.56     | TBD |
| WA-GAST-OS-2.53_201607-01     | Aug 8 2016    | Beitrag 2,55     | TBD |
| WA-GAST-OS-2.52_201606-01     | Juli 13 2016  | Okt 13 2016   | TBD |
| WA-GAST-OS-2.51_201605-01     | 10 2016 Juni  | September 8 2016   | TBD |
| WA-GAST-OS-2.50_201604-01     | 2 2016 Mai    | Aug 13 2016   | TBD |
| WA-GAST-OS-2.49_201603-01     | 7 2016 April  | 10 2016 Juli  | TBD |
| WA-GAST-OS-2.48_201602-02     | 12 2016 März | 2 2016 Juni   | TBD |
| WA-GAST-OS-2.47_201601-01     | Feb 12 2016   | 7 2016 Mai    | TBD |
| WA-GAST-OS-2.46_201512-01     | Jan 12 2016   | 12 2016 April | TBD |
| ~~WA-GAST-OS-2.45_201511-02~~ | Jan 4 2016    | 12 2016 März | TBD |
| ~~WA-GAST-OS-2.45_201511-01~~ | 10 2015 Dez   | 12 2016 März | TBD |
| ~~WA-GAST-OS-2.44_201510-01~~ | Nov 6 2015    | Feb 12 2016   | TBD |
| ~~WA-GAST-OS-2.43_201509-01~~ | Okt 1 2015    | Jan 10 2016   | TBD |
| ~~WA-GAST-OS-2.42_201508-02~~ | Sep 9 2015    | 6 2015 Dez    | TBD |
| ~~WA-GAST-OS-2.41_201507-02~~ | Aug 7 2015    | 1 2015 Nov    | TBD |
| ~~WA-GAST-OS-2.40_201506-01~~ | Juli 9 2015   | Okt 9 2015    | TBD |
| ~~WA-GAST-OS-2.39_201505-02~~ | Juni 12 2015  | Sep 7 2015    | TBD |
| ~~WA-GAST-OS-2.38_201504-01~~ | April 17 2015 | Aug 9 2015    | TBD |

## <a name="msrc-patch-updates"></a>MSRC Patch updates
Die Liste der Updates, die monatliche Gast-BS-Release enthalten sind, steht [hier][patches].

## <a name="sdk-support"></a>SDK-Unterstützung

Obwohl die [Rente Richtlinie für den Azure SDK] [ retire policy sdk] gibt an, dass nur Versionen über 2.2 unterstützte, bestimmte Gast-BS Familien Sie frühere Versionen nutzen können. Sie sollten immer das neueste unterstützte SDK verwenden. 

| Gast OS Familie | Kompatible SDK-Versionen |
| --------------- | ----------------------- |
| 4               | Version 2.1 + |
| 3               | Version 1.8 |
| 2               | Version 1.3 + |
| 1               | Version 1.0 + |

## <a name="guest-os-release-information"></a>Informationen zur Gast OS Freigabe
Es gibt drei Datumsangaben, die Gast-BS Versionen wichtig sind: **lassen Sie wieder los** Datum, Datum **deaktiviert** und **Ablaufdatum** . Gast-BS gilt als verfügbar, wenn es im Portal ist und als Gast OS Ziel ausgewählt werden kann. Wenn ein Gast OS das Datum **deaktiviert** Treffer ist es aus Azure entfernt. Jedoch werden alle Cloud-Service-Anwendung die Gast-BS weiterhin normal ausgeführt werden. 

Das Fenster zwischen den **deaktiviert** und **das Ablaufdatum** bieten Ihnen einen Puffer in ganz einfach Umstieg von einem Gast-BS auf eine neuere. Wenn Sie *Automatische* als Ihr Betriebssystem Gast arbeiten, zwar immer auf die neueste Version, und nicht ablaufen kümmern müssen. 

Wenn **das Ablaufdatum** übergibt und alle noch verwenden diese Gast-BS Cloud-Dienst wird beendet, gelöscht oder aktualisiert wird. Sie können weitere Informationen über die Richtlinie Rente [hier][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Gast Familie Betriebssystemversion Erläuterung
Die Gast-BS Familien basieren auf freigegebene Versionen von Microsoft Windows Server. Das Gast-Betriebssystem ist vom Betriebssystem Azure Cloud Services ausgeführt. Je nach Betriebssystem Gast verfügt über eine Zahl Familie und die aktuelle Version. 

- **Gast-BS Familie**  
Ein Betriebssystem Windows Server lassen, dass ein Gast-BS auf basiert. Zum Beispiel basiert die *Familie 3* auf Windows Server 2012.

- **Betriebssystemversion Gast**  
Speziell für eine Gast-BS familiäre Bild sowie die entsprechenden [Microsoft Security Antwort Center (MSRC)] [ msrc] Patches, die am Tag verfügbar sind die neue Gast-BS-Version erstellt wird. Nicht alle Patches können enthalten sein. 

    Zahlen mit 0 beginnen und um 1 erhöht jedes Mal, wenn ein neuer Satz von Updates hinzugefügt wird. Nachfolgende Nullen werden nur angezeigt, wenn Sie wichtige. Version 2.10 ist eine andere Version als Version 2.1 viel höher.

- **Lassen Sie wieder los Gast-BS**  
Eine Version Gast-BS neu aufgelegt. Neu aufgelegt tritt auf, wenn Sie Microsoft Probleme findet während des Testens; Anfordern der Änderungen. Die neueste Version immer hat Vorrang vor jedem vorherigen frei, öffentlichen, oder nicht. Im klassische Azure-Portal wird nur Benutzer die neueste Version für eine bestimmte Version auswählen können. Bereitstellungen auf eine frühere Version ausgeführt sind nicht in der Regel durchgeführt abhängig davon, wie der Fehler sofort. 

Im folgenden Beispiel 2 der Familie, 12 ist die Version und "rel2" der Version.

**Lassen Sie wieder los Gast OS** - 2.12 rel2

**Konfigurationszeichenfolge in dieser Version** - WA-Gast-OS-2.12_201208-02

Die Konfigurationszeichenfolge für Gast-BS weist dieser Daten, eingebettete darin, zusammen mit einem Datum mit, welche Patches MSRC in dieser Version eingestuft wurden. In diesem Beispiel wurden MSRC Patches gefertigt für Windows Server 2008 R2 bis zu und einschließlich August 2012 für einbezogen werden berücksichtigt. Nur Patches, die speziell für diese Version von Windows Server anwenden, werden berücksichtigt. Angenommen, wenn Microsoft Office ein MSRC Patch gilt, wird es nicht berücksichtigt da dieses Produkt nicht Teil des Bilds Basis Windows Server ist. 

## <a name="guest-os-system-update-process"></a>Gast OS System Aktualisierungsvorgang
Diese Seite enthält Informationen zu anstehenden Gast OS-Versionen. Kunden haben uns mitgeteilt, dass sie wissen, wann wird eine freigegeben, da ihre Cloud-Dienstverwaltungsrollen neu gestartet werden, wenn sie auf "AutoUpdate" festgelegt werden soll. Gast-BS Versionen treten in der Regel mindestens 5 Tage nach der Aktualisierung des MSRC Release, bei dem der zweite Dienstag des Monats auftritt. Neue Versionen einbeziehen aller relevanten MSRC-Updates für jede Gast-BS Familie 

Microsoft Azure stellt ständig Updates zur Verfügung. Gast-BS wird nur eine solche Aktualisierung der Verkaufspipeline an. Ein Release kann anhand einer Anzahl von Faktoren beeinflusst werden auch zahlreiche hier aufgeführt. Darüber hinaus wird Azure auf Literal hundert Tausende von Computern ausgeführt. Dies bedeutet, dass es ist nicht möglich, geben Sie einen genauen Datum und Uhrzeit, die bei die Rollen neu gestartet wird. Wir werden aktualisiert den [Gast OS aktualisieren RSS-Feed] [ rss] mit den neuesten Informationen wir haben, aber erwägen Sie dieses Zeitraums ein ungefähren-Fenster. Beachten Sie, dass dies für Kunden und seine Arbeit an einem Plan ist zu beschränken oder nach einem Neustart Anzeigedauer problematisch werden. 

Wenn eine neue Version des Betriebssystems Gast veröffentlicht wird, können sie über Azure vollständig verteilen dauern. Wie Dienste auf neue Gast OS aktualisiert werden, werden diese neu gestartet, berücksichtigt Domänen aktualisieren. Dienste festgelegt, verwenden Sie "Automatisch" Updates erhalten eine Version ersten. Nach der Aktualisierung sehen Sie die neue Gast-BS-Version für den Dienst in der klassischen Azure-Portal aufgeführt. Neu veröffentlichten Sicherheitsupdates können während dieses Zeitraums auftreten. Einige Versionen möglicherweise über längere Zeiträume bereitgestellt werden und automatische Upgrade nach einem Neustart können nicht für viele Wochen nach dem Datum offizielle Release auftreten. Nachdem ein Gast OS verfügbar ist, können Sie dann explizit dieser Version aus dem Portal oder in Ihrer Konfigurationsdatei auswählen. 

Für viel wertvoller Informationen auf neu gestartet wurde und Zeiger auf Weitere Informationen technische Details Gast und Host OS Aktualisierungen, finden Sie im MSDN Blogbeitrag Betitelte [Rolle Instanz Neustart fällig zu Updates für OS][restarts].

Wenn Sie Ihre Gast-BS manuell aktualisieren möchten, lesen Sie die [Gast-BS Rente Richtlinie][retirepolicy].


## <a name="guest-os-supportability-and-retirement-policy"></a>Unterstützung für Gast OS und Rente Richtlinie
Ist die Richtlinie für Gast-BS-Unterstützung und Rente Erläuterung [hier][retirepolicy].

[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Gast OS Update-Einstellungen]: cloud-services-how-to-configure.md
[rss]: http://sxp.microsoft.com/feeds/3.0/msdntn/WindowsAzureOSUpdates
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
 

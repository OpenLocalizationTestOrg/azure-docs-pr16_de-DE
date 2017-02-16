<properties 
    pageTitle="Konfigurieren Sie eine Verbindung aus Azure Suche Indexer mit SQL Server auf eine Azure-virtuellen Computern | Microsoft Azure | Indexer" 
    description="Verschlüsselte Verbindungen aktivieren und Konfigurieren des Firewalls für Verbindungen mit SQL Server auf eine Azure-virtuellen Computern (virtueller Computer) in einen Indexer Azure nach zu ermöglichen." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Konfigurieren Sie eine Verbindung aus Azure Suche Indexer mit SQL Server ein Azure-virtuellen Computers

Wie in der [Herstellen einer Verbindung von Azure SQL-Datenbank zu verwenden von Indexern Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions)erwähnt, Indexer für **SQL Server auf Azure-virtuellen Computern** (oder **SQL Azure-virtuellen Computern** kurz) erstellen Azure-Suche unterstützt wird, aber es gibt ein paar Sicherheits-Komponenten zu ersten erledigen. 

**Dauer des Vorgangs:** Etwa 30 Minuten installiert Voraussetzung Sie bereits ein Zertifikat des virtuellen Computers.

## <a name="enable-encrypted-connections"></a>Aktivieren von verschlüsselten Verbindungen

Azure Suche erfordert einen verschlüsselten Kanal für alle Indexer Anfragen über einen öffentlichen Internet-Verbindung. Dieser Abschnitt listet die Schritte aus, damit dies funktioniert.

1. Überprüfen Sie die Eigenschaften des Zertifikats an, stellen Sie sicher, dass der Name der Betreff der vollqualifizierten Domänennamen (FULLY) den Azure-virtuellen Computer ist. Sie können zum Anzeigen der Eigenschaften eines Tools wie CertUtils oder der Zertifikate-Snap-in verwenden. Sie können den vollqualifizierten Domänennamen aus der virtuellen Computer Service-Karte vorausgesetzt des Essentials Abschnitt im Feld **öffentlichen IP-Adresse/DNS benennen Bezeichnung** im [Portal Azure](https://portal.azure.com/)erhalten.

    - Für virtuelle Computer mit der neueren **Ressourcenmanager** Vorlage erstellt, wird der vollqualifizierten Domänennamen als formatiert `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Für ältere virtuelle Computer als eine **klassische** virtueller Computer erstellt haben, wird der vollqualifizierten Domänennamen als formatiert `<your-cloud-service-name.cloudapp.net>`. 

2. Konfigurieren von SQL Server, um das Zertifikat verwenden den Registrierungs-Editor (Regedit) verwenden. 

    Obwohl SQL Server-Konfigurations-Manager für diese Aufgabe häufig verwendet wird, können nicht Sie es in diesem Szenario verwenden. Es kann das importierte Zertifikat nicht finden, weil der vollqualifizierten Domänennamen für den virtuellen Computer auf Azure den vollqualifizierten Domänennamen entspricht wie durch den virtuellen Computer bestimmt (er identifiziert die Domäne als entweder auf dem lokalen Computer oder im Netzwerk-Domäne, mit der sie verbunden ist). Wenn die Namen nicht entsprechen, verwenden Sie "regedit" ein, um das Zertifikat anzugeben.

    - Regedit, navigieren Sie zu diesen Registrierungsschlüssel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    Die `[MSSQL13.MSSQLSERVER]` Teil richtet sich nach Version und den Instanznamen. 

    - Legen Sie den Wert für den Schlüssel des **Zertifikats** in den **Fingerabdruck** SSL-Zertifikat, das Sie auf dem virtuellen Computer importiert.

    Es gibt mehrere Methoden, um den Fingerabdruck an, einige besser aus. Wenn Sie über die **Zertifikate** -Snap-in in MMC kopieren, werden Sie vermutlich Abholen eines unsichtbar führenden Zeichen [wie in diesem Artikel Unterstützung beschrieben](https://support.microsoft.com/kb/2023869/), was zu einem Fehler führt, wenn Sie versuchen, eine Verbindung herzustellen. Mehrere problemumgehungen vorhanden sein, zum Beheben des Problems. Ist am einfachsten über die RÜCKTASTE, und klicken Sie dann erneut das erste Zeichen des Fingerabdrucks so entfernen Sie das führende Zeichen in das Feld Schlüsselwert regedit ein. Alternativ können Sie ein anderes Tool verwenden, um den Fingerabdruck zu kopieren.

3. Erteilen von Berechtigungen zur Verwaltung des Dienstkontos. 

    Stellen Sie sicher, dass das SQL Server-Dienstkonto die geeigneten Berechtigungen für den privaten Schlüssel des Zertifikats SSL gewährt wird. Wenn Sie diesen Schritt nächste, wird SQL Server nicht gestartet. Sie können die **Zertifikate** -Snap-in oder **CertUtils** für diese Aufgabe verwenden.

4. Starten Sie den SQL Server-Dienst an.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfigurieren von SQL Server-Konnektivität auf dem virtuellen Computer

Nachdem Sie die verschlüsselte Verbindung erforderlich Azure-Suche eingerichtet haben, sind zusätzliche Konfigurationsschritte eingebauten mit SQL Server auf Azure-virtuellen Computern. Wenn dies nicht erfolgt noch, besteht der nächste Schritt zum Abschließen Konfiguration mit einem der folgenden Artikeln:

- Ein **Ressourcenmanager** virtueller Computer finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf Azure Ressourcenmanager verwenden](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- Eine **klassische** virtueller Computer finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf klassische Azure](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Insbesondere überprüfen Sie im Abschnitt in jeder Artikel für "über das Internet verbinden".

## <a name="configure-the-network-security-group-nsg"></a>Konfigurieren der Netzwerk-Sicherheitsgruppe (NSG)

Es ist nicht ungewöhnliche so konfigurieren Sie die NSG und den entsprechenden Endpunkt aus Azure oder die Access Control Liste (ACL) um Ihre Azure-virtuellen Computer an andere Parteien barrierefrei zu machen. Wahrscheinlich Sie diese, bevor Sie an, damit Ihre eigene Anwendungslogik Verbindung zu Ihrem SQL Azure-virtuellen Computer vorgenommen haben. Es ist nicht bei einer Suche Azure-Verbindung zu Ihrem SQL Azure-virtuellen Computer unterschiedlich. 

Die folgenden Links Bereitstellen von Anweisungen NSG Konfiguration für Bereitstellungen virtueller Computer. Gehen Sie folgendermaßen vor, um ACL Endpunkt Azure Suche basierend auf dessen IP-Adresse.

> [AZURE.NOTE] Hintergrundinformationen finden Sie unter [Neuigkeiten einer Sicherheitsgruppe Netzwerk?](../virtual-network/virtual-networks-nsg.md)

- Ein **Ressourcenmanager** virtueller Computer finden Sie unter [So erstellen Sie NSGs für Cloud-Bereitstellungen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Für eine **klassische** virtueller Computer finden Sie unter [NSGs für klassischen Bereitstellungen erstellen](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-Adressen bergen einige Probleme, die einfach zu lösen sind, wenn Sie das Problem und mögliche problemumgehung bekannt sind. Die restlichen Abschnitte enthalten Empfehlungen für die Behandlung von Problemen im Zusammenhang mit IP-Adressen in der ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Einschränken des Zugriffs auf die Suche Dienst IP-Adresse

Es wird dringend empfohlen, dass Sie den Zugriff auf die IP-Adresse Ihre Suchdiensts in die ACL, statt an Ihrem SQL Azure-virtuellen Computern keine Verbindungsanfragen weit geöffneter beschränken. Sie können die IP-Adresse einfach feststellen, indem Sie den vollqualifizierten Domänennamen anpingen (z. B. `<your-search-service-name>.search.windows.net`) der Suchdienst.

#### <a name="managing-ip-address-fluctuations"></a>Verwalten von IP-Adresse Fluktuationen

Wenn der Suchdienst nur eine Suche Einheit (d. h., eine Replikation und eine Partition) enthält, ändert sich die IP-Adresse während routinemäßige Dienst neu gestartet wurde, eine vorhandene Zugriffssteuerungsliste mit der Suchdienst IP-Adresse ungültig.

Eine Möglichkeit besteht darin, den nachfolgende Connectivity-Fehler zu vermeiden besteht darin, mehr als eine Replikation und eine Partition in Azure Suche verwenden. Auf diese Weise erhöht die Kosten, aber damit auch das IP-Adresse Problem gelöst. In Azure Suche ändern nicht IP-Adressen, wenn Sie mehr als eine Suche Einheit haben.

Ein zweiter Ansatz ist in Verbindung treten, und konfigurieren Sie dann die ACLs in der NSG zulassen. Erwarten Sie im Durchschnitt IP-Adressen nach ein paar Wochen ändern. Für Kunden, die führen Sie auf der Grundlage seltene gesteuert Indizierung, möglicherweise dieser Ansatz realisierbar.

Eine dritte realisierbare (aber nicht besonders sichere) Methode besteht darin, den IP-Adresse des Bereichs Azure Bereich angeben, in dem der Suchdienst bereitgestellt wird. Die Liste der IP-Adressbereiche aus der öffentliche IP-Adressen Azure Ressourcen zugeordnet sind, wird bei [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)veröffentlicht. 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Die Suche Azure Portals IP-Adressen enthalten

Wenn Sie zum Erstellen eines Indexers Azure-Portal verwenden, benötigt Azure Suche Portal Logik auch Zugriff auf Ihre SQL Azure-virtuellen Computer während der Erstellungszeit. Suche Azure Portals IP-Adressen finden, indem Sie anpingen `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Nächste Schritte

Mit dem Weg Konfiguration können Sie jetzt eine SQL Server Azure-virtuellen Computers als Datenquelle für einen Indexer Azure-Suche angeben. Weitere Informationen finden Sie unter [Herstellen einer Verbindung von Azure SQL-Datenbank zum Verwenden von Indexern Azure-Suche](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .

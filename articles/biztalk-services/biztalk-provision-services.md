<properties
    pageTitle="Azure BizTalk-Dienste Azure-Portal erstellen | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen oder zum Erstellen von Azure BizTalk-Dienste Azure-Portal; MABS, WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Erstellen Sie mithilfe des Azure-Portals BizTalk-Dienste

Erstellen Sie im Azure-Portal Azure BizTalk-Dienste.

> [AZURE.TIP] Zum Anmelden bei der Azure-Portal, benötigen Sie ein Azure-Konto und Azure-Abonnement. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto in wenigen Minuten erstellen. Finden Sie unter [Azure kostenlose Testversion](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Erstellen eines BizTalk-Dienstes
Je nach der Edition ausgewählt haben, möglicherweise nicht alle BizTalk Service Einstellungen zur Verfügung.

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im Navigationsbereich unten **neu**aus:  
![Wählen Sie die Schaltfläche ' Neu '][NEWButton]

3. Wählen Sie im **APP-Dienste** > **BIZTALK-Dienst** > **benutzerdefinierten erstellen**:  
![Wählen Sie BizTalk Service und benutzerdefinierte erstellen][NewBizTalkService]

4. Geben Sie die BizTalk Service-Einstellungen aus:

    <table border="1">
    <tr>
    <td><strong>Name der BizTalk-Dienst</strong></td>
    <td>Sie können einen beliebigen Namen eingeben, aber seien Sie spezifisch. Einige Beispiele:<br/><br/>
    <em>MyCompany</em>. biztalk.windows.net<br/>
    <em>Mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" wird auf den Namen, geben Sie ein, automatisch hinzugefügt. Dies erstellt eine URL, die verwendet wird, wie Ihre BizTalk Service Zugriff auf <strong>https://<em>Myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Wenn Sie in der Phase testen/Entwicklung sind, wählen Sie <strong>Entwicklertools</strong>. Wenn Sie in der Herstellung Phase sind, verwenden Sie die <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk-Dienste: Editionen Diagramm</a> bestimmen, ob <strong>Premium</strong>, <strong>Standard</strong>oder <strong>grundlegende</strong> die richtige Version für Ihr Unternehmen Szenario ist.
    </td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Wählen Sie die geografische Region Ihrer BizTalk Service hosten.</td>
    </tr>
    <tr>
    <td><strong>Domänen-URL</strong></td>
    <td><strong>Optional</strong>. Standardmäßig ist die URL für die Domäne <em>YourBizTalkServiceName</em>. biztalk.windows.net. Eine benutzerdefinierte Domäne kann auch eingegeben werden. Beispielsweise, wenn Ihre Domäne <em>Contoso</em>ist, können Sie eingeben: <br/><br/>
    <em>MyCompany</em>. "contoso.com"<br/>
    <em>MyCompanyMyApplication</em>. "contoso.com"<br/>
    <em>MyApplication</em>. "contoso.com"<br/>
    <em>YourBizTalkServiceName</em>. "contoso.com"<br/>
    </td>
    </tr>
    </table>
Wählen Sie den nächsten Pfeil.

5. Geben Sie den Speicher und die Datenbank-Einstellungen:

    <table border="1">
    <tr>
    <td><strong>Überwachung/Archivierung Speicher-Konto</strong></td>
    <td>Wählen Sie ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues Speicherkonto. <br/><br/>Wenn Sie ein neues Speicherkonto erstellen, geben Sie den <strong>Kontonamen Speicher</strong>.</td>
    </tr>
    <tr>
    <td><strong>Verlauf-Datenbank</strong></td>
    <td>Wenn Sie eine vorhandene Azure SQL-Datenbank verwenden, kann es von einer anderen BizTalk Service verwendet werden. Benötigen Sie den Namen Login und Kennwort eingegeben an, dass Azure SQL-Datenbankserver erstellt wurde.<br/><br/><strong>Tipp</strong> Erstellen Sie die Datenbank Verlauf und Überwachung/Archivierung Speicher-Konto in derselben Region als BizTalk-Service ein.</td>
    </tr>
    </table>
Wählen Sie den nächsten Pfeil.

6. Geben Sie die Einstellungen für die Datenbank aus:

    <table border="1">
    <tr>
    <td><strong>Namen</strong></td>
    <td>Verfügbar, wenn <strong>eine neue Instanz von SQL-Datenbank erstellen</strong> , die im vorherigen Bildschirm ausgewählt ist.
    <br/><br/>
   Geben Sie einen Namen der SQL-Datenbank von Ihrem BizTalk Service verwendet werden soll.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Verfügbar, wenn <strong>eine neue Instanz von SQL-Datenbank erstellen</strong> , die im vorherigen Bildschirm ausgewählt ist.
    <br/><br/>
   Wählen Sie eine vorhandene SQL-Datenbankserver aus, oder erstellen Sie einen neuen SQL-Datenbankserver.</td>
    </tr>
    <tr>
    <td><strong>Server-Benutzername</strong></td>
    <td>Geben Sie den Benutzernamen Login.</td>
    </tr>
    <tr>
    <td><strong>Server-Kennwort</strong></td>
    <td>Geben Sie das Kennwort ein.</td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Verfügbar, wenn <strong>eine neue Instanz von SQL-Datenbank erstellen</strong> ausgewählt ist. Wählen Sie die geografische Region, die SQL-Datenbank zu hosten.</td>
    </tr>
    </table>

Wählen Sie das Häkchen-Assistenten auszuführen. Die Statusanzeige wird angezeigt:  
![Symbol für Fortschritt zeigt an, wenn Sie fertig sind][ProgressComplete]

Wenn Sie fertig sind, wird der Azure BizTalk-Dienst erstellt wurden und für die Anwendung bereit. Die Standardeinstellungen sind ausreichend. Wenn Sie die Standardeinstellungen ändern möchten, wählen Sie im linken Navigationsbereich **BIZTALK-Dienste** aus, und wählen Sie dann Ihre BizTalk Service. Zusätzliche Einstellungen werden auf den [Registerkarten Dashboard, Monitor, und skalieren](biztalk-dashboard-monitor-scale-tabs.md) oben angezeigt.

Je nach Status des BizTalk Service gibt es einige Vorgänge, die nicht abgeschlossen werden können. Eine Liste dieser Vorgänge wechseln Sie zu [BizTalk Services Zustand Diagramm](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Nach der Bereitstellung Schritte

-  [Installieren Sie das Zertifikat auf einem lokalen computer](#InstallCert)
-  [Hinzufügen eines Zertifikats einsatzbereit](#AddCert)
-  [Abrufen von Access Control namespace](#ACS)

#### <a name="a-nameinstallcertainstall-the-certificate-on-a-local-computer"></a><a name="InstallCert"></a>Installieren Sie das Zertifikat auf einem lokalen computer
Als Bestandteil der BizTalk Service bereitgestellt wird ein selbst signiertes Zertifikat erstellt und mit Ihrem BizTalk Service-Abonnement verknüpft ist. Sie müssen dieses Zertifikat herunterladen und installieren Sie es auf Computern aus, in dem Sie entweder BizTalk Service Applications bereitstellen oder Senden von Nachrichten an eine BizTalk Service-Endpunkts an.

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im linken Navigationsbereich **BIZTALK-Dienste** aus, und wählen Sie dann Ihr Abonnement BizTalk Service.
3. Wählen Sie die Registerkarte **Dashboard** aus.
4. Wählen Sie die **SSL-Zertifikat herunterladen**:  
![Ändern der SSL-Zertifikat][QuickGlance]
5. Doppelklicken Sie auf das Zertifikat, und führen Sie durch den Assistenten, um das Zertifikat zu installieren. Stellen Sie sicher, dass Sie das Zertifikat unter dem Speicher **Trusted Root Zertifizierungsstellen** installieren.

#### <a name="a-nameaddcertaadd-a-production-ready-certificate"></a><a name="AddCert"></a>Hinzufügen eines Zertifikats einsatzbereit
Das selbst signiertes Zertifikat, das Erstellen von BizTalk-Diensten zur Verwendung in Entwicklung Umgebungen nur bestimmt wird automatisch erstellt wird. Ersetzen Sie für die Herstellung Szenarien es mit einem Zertifikat einsatzbereit.

1. Wählen Sie auf der Registerkarte **Dashboard** **Update SSL-Zertifikat**ein.
2. Navigieren Sie zu Ihrer privaten SSL-Zertifikat (*CertificateName*PFX-Datei), das Ihren Namen BizTalk Service enthält, geben Sie das Kennwort ein, und klicken Sie dann auf das Häkchen.

#### <a name="a-nameacsaget-the-access-control-namespace"></a><a name="ACS"></a>Abrufen von Access Control namespace

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im linken Navigationsbereich **BIZTALK-Dienste** aus, und wählen Sie dann Ihre BizTalk Service.
3. Wählen Sie in der Taskleiste **Verbindungsinformationen**ein:  
![Wählen Sie die Verbindungsinformationen][ACSConnectInfo]

4. Kopieren Sie die Steuerung des Benutzerzugriffs Werte ein.

Wenn Sie ein BizTalk Service-Projekt aus Visual Studio bereitstellen, geben Sie in Access Control Namespace an. Access Control Namespace wird automatisch für Ihre BizTalk Service erstellt.

Die Steuerung des Benutzerzugriffs-Werte können mit jeder Anwendung verwendet werden. Wenn Azure BizTalk-Dienste erstellt wurde, steuert dieser Access Control Namespace mit der BizTalk-Service-Bereitstellung der Authentifizierung aus. Wenn Sie das Abonnement ändern oder die Namespaces zu verwalten, wählen Sie im linken Navigationsbereich **ACTIVE DIRECTORY** und wählen Sie dann Ihren Namespace möchten. Die Taskleiste Listen die Optionen.

Mit einem Klick **Verwalten** öffnen Access Steuerelement Verwaltungsportal. Im Verwaltungsportal Access Steuerelement verwendet BizTalk Service **Dienstidentitäten**:  
![ACS-Dienst Identitäten in den Zugriff steuern Verwaltungsportal][ACSServiceIdentities]

Die Steuerung des Benutzerzugriffs Dienstidentität ist eine Reihe von Anmeldeinformationen, die zulassen von Applications oder Clients direkt mit Access Control authentifizieren und ein Token zu erhalten.

> [AZURE.IMPORTANT]Der BizTalk Service verwendet **Besitzer** für die Identität des Standard-Dienst und den Wert für **das Kennwort** ein. Wenn Sie den Wert symmetrischen Schlüssel anstelle des Werts Kennwort verwenden, kann der folgende Fehler auftreten.<br/><br/>*Mit dem Access-Steuerelement-Verwaltungsdienst-Konto mit den angegebenen Anmeldeinformationen konnte keine Verbindung hergestellt werden.*

[Verwalten Ihrer ACS-Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) listet einige Richtlinien und Empfehlungen.

## <a name="requirements-explained"></a>Erläuterung der Anforderungen

Diese Anforderungen gelten nicht für die kostenlose Edition.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Was Sie benötigen</strong></td>
        <td><strong>Warum Sie benötigen</strong></td>
</tr>
<tr>
<td>Azure-Abonnement</td>
<td>Das Abonnement bestimmt, wer Azure-Portal anmelden können. Der Inhaber des Kontos erstellt das Abonnement zur <a HREF="https://account.windowsazure.com/Subscriptions">Azure-Abonnements</a>.
<br/><br/>
Azure-Konto kann mehrere Abonnements vorhanden sind und von jedem Benutzer zulässig ist verwaltet werden kann. Angenommen, der Kontoinhaber Azure-ein Abonnement mit dem Namen <em>BizTalkServiceSubscription</em> erstellt und ermöglicht die BizTalk-Administratoren innerhalb des Unternehmens (z. B. ContosoBTSAdmins@live.com) Zugriff auf Abonnement. In diesem Szenario BizTalk-Administratoren Azure-Portal anmelden und vollständigen zum über Administratorrechte verfügen alle gehosteten Dienste in das Abonnement, einschließlich Azure BizTalk-Dienste. BizTalk-Administratoren sind nicht die Kontoinhaber Azure-und daher keinen Zugriff auf eine beliebige Abrechnungsinformationen.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Verwalten von Abonnements und Speicherkonten Azure-Portal</a> enthält weitere Informationen.
</td>
</tr>
<tr>
<td>SQL Azure-Datenbank</td>
<td>Speichert die Tabellen, Ansichten und gespeicherten Prozeduren, die von der BizTalk Service, einschließlich der Verlauf Daten verwendet.
<br/><br/>
Beim Erstellen eines BizTalk Service können Sie können einem vorhandenen Azure SQL Server, SQL Azure-Datenbank oder automatisches Erstellen einer neuen Server oder Datenbank.
<br/><br/>
Die Skalierung der SQL-Datenbank wird automatisch konfiguriert. In der Regel ist die Standardwert für Skalierung für eine BizTalk Service ausreichend. Ändern des Maßstabs wirkt sich auf die Preise. Finden Sie unter <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Konten und Abrechnung in SQL Azure-Datenbank</a>
<br/><br/>
<strong>Notizen</strong>
<br/>
<ul>
<li> Wenn Sie eine neue Azure SQL-Server und eine Datenbank erstellen, wird automatisch Azure Services aktiviert. Der BizTalk Service erfordert Azure Services aktiviert werden.</li>
<li>Wenn Sie eine neue Azure SQL-Datenbank auf einem vorhandenen Azure SQL Server erstellen, werden die Firewallregeln des Servers nicht geändert werden. Daher ist es möglich, andere Dienste Azure Zugriff auf die Datenbanken des Servers nicht zulässig sind.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Access Control namespace</td>
<td>Mit den Diensten von Azure BizTalk authentifiziert. Wenn Sie ein BizTalk Service-Projekt aus Visual Studio bereitstellen, geben Sie in Access Control Namespace an. Wenn Sie einen BizTalk-Service erstellen, wird Access Control Namespace automatisch erstellt.</td>
</tr>

<tr>
<td>Azure-Speicher-Konto</td>
<td>Ermöglicht den Zugriff auf die Tabellen, Blobs und Warteschlangen von Ihrem BizTalk Service verwendet, um Folgendes zu speichern:

<ul>
<li>Protokolldateien, die der BizTalk Service zu überwachen. Die Ausgabe die Überwachung wird auch in der Registerkarte **Überwachen** der Azure-Portal angezeigt.</li>
<li>Beim Erstellen eines X12 oder AS2 Vertrag zwischen Partner können Sie das Feature Archivierung Nachrichteneigenschaften speichern aktivieren. Diese Daten werden in dem Konto Speicher gespeichert.</li>
</ul>
<br/>
Wenn Sie einen BizTalk-Service erstellen, können Sie vorhandenes Speicherkonto verwenden oder automatisches Erstellen ein neues Kontos mit Speicher.
<br/><br/>
Die Standardeinstellungen für Speicher sind ausreichend für einen BizTalk Service.
<br/><br/>
Wenn Sie ein Speicherkonto erstellen, werden einer primären und Sekundärschlüssel automatisch erstellt. Diese Schlüssel Steuern des Zugriffs auf Ihr Konto Speicher. Der BizTalk Service verwendet automatisch den Primärschlüssel.
<br/><br/>
Weitere Informationen finden Sie unter <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Speicher</a> .
</td>
</tr>

<tr>
<td>Private SSL-Zertifikat</td>
<td>
Wenn eine Azure BizTalk-Dienst erstellt wird, wird auch HTTPS-URL, die Ihren Namen BizTalk Service enthält erstellt. Diese URL wird automatisch ein selbst signiertes Zertifikat der Entwicklung schreibgeschützten Verwendung konfiguriert. Für die Herstellung benötigen Sie ein privates SSL-Zertifikat.
<br/><br/>
<strong>Wichtig SSL Zertifikatinformationen</strong>

<ul>
<li>Ablaufdatum des Zertifikats muss kleiner als 5 Jahre.</li>
<li>Alle privaten Zertifikate anfordern eines Kennworts. Kennen Sie dieses Kennwort, und teilen Sie als bewährte Methode, dieses Kennwort ein, mit Ihren Administratoren.</li>
<li>Selbstsignierte Zertifikaten werden in einer Umgebung Test/Entwicklung verwendet. Bei Verwendung von selbstsignierten Zertifikaten, importieren Sie das Zertifikat, zu Ihrer persönlichen Zertifikats speichern und die vertrauenswürdige Zertifikat Serverzertifikat.</li>
</ul>
<br/>Wenn die Anforderung Herstellung zu der Zertifizierungsstelle senden, geben Sie die folgenden Zertifikateigenschaften:
<br/>

<ul>
<li><strong>Enhanced Key Usage</strong>: mindestens Azure BizTalk Services erfordert Serverauthentifizierung.</li>
<li><strong>Allgemeine Namen</strong>: Geben Sie den vollqualifizierten Domänennamen (FQDN) Ihrer Azure BizTalk Dienst-URL ein. In diesem Artikel Informationen dazu finden Sie unter <a HREF="#BizTalk">Erstellen eines BizTalk-Dienstes</a> .</li>
</ul>
<br/>
Ein neues oder ein anderes Zertifikat kann hinzugefügt werden, nachdem der BizTalk Service erstellt wurde.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybrid-Verbindungen

Beim Erstellen eines Azure BizTalk-Diensts ist die Registerkarte **Verbindungen Hybrid** verfügbar:

![Registerkarte Hybrid-Verbindungen][HybridConnectionTab]

Hybrid Verbindungen verwendeten Verbindung eine Azure-Website oder ein Azure mobilen Dienst auf eine beliebige lokale Ressource, die eine statische TCP-, wie SQL Server, MySQL, HTTP-Web-APIs, Mobile Dienste und die meisten benutzerdefinierten Webdienste verwendet.  Hybrid-Verbindungen und der Dienst BizTalk unterscheiden. Der Dienst BizTalk wird in Verbindung mit einem lokalen Zeile des Business BRANCHENSPEZIFISCHE System Azure BizTalk-Dienste verwendet.

 Finden Sie unter [Hybrid Verbindungen](integration-hybrid-connection-overview.md) Weitere, einschließlich erstellen und Verwalten von Hybrid Verbindungen.


## <a name="next-steps"></a>Nächste Schritte

Nun eine BizTalk Service erstellt wird, machen Sie sich mit den anderen vertraut [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](biztalk-dashboard-monitor-scale-tabs.md). Ihre BizTalk Service ist für die Anwendung bereit. Zum Erstellen von Applications beginnen möchten, wechseln Sie zu [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)<br/>
- [BizTalk-Dienste: Bundesland Diagramm](biztalk-service-state-chart.md)<br/>
- [BizTalk-Dienste: Sichern und Wiederherstellen](biztalk-backup-restore.md)<br/>
- [BizTalk-Dienste: Begrenzungsebene](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](biztalk-issuer-name-issuer-key.md)<br/>
- [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybrid-Verbindungen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png

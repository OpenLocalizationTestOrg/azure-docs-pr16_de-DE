<properties
    pageTitle="Azure-API Management häufig gestellte Fragen zu | Microsoft Azure"
    description="Erfahren Sie, die Antworten auf häufig gestellte Fragen, Muster sowie optimale Methoden bei Azure-API Verwaltung."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure-API Management häufig gestellte Fragen

Erhalten Sie Antworten auf häufig gestellte Fragen, Muster und bewährte Methoden für die Verwaltung von Azure-API ein.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

-   [Wie kann ich dem Microsoft Azure-API-Team eine Frage stellen?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Was bedeutet es, wenn ein Feature in der Vorschau ist?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Wie können die Verbindung zwischen dem Gateway-API Management und Back-End-Dienste werden gesichert?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Wie kopiere ich meine API Management Service-Instanz auf eine neue Instanz?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Kann ich meine API Management Instanz programmgesteuert verwalten?](#can-i-manage-my-api-management-instance-programmatically)
-   [Wie füge ich einen Benutzer zur Gruppe Administratoren?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Warum ist die Richtlinie, die ich hinzufügen in den Richtlinien-Editor nicht verfügbar?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Wie verwende ich die versionsverwaltung API in API Management?](#how-do-i-use-api-versioning-in-api-management)
-   [Wie richte ich mehrere Umgebungen in einer einzigen API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Kann ich SOAP-API Verwaltung verwenden?](#can-i-use-soap-with-api-management)
-   [Ist die Konstante API Management Gateway IP-Adresse? Kann ich es in Firewall-Regeln verwenden?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Kann ich einen OAuth 2.0 Autorisierung Server mit dem AD FS-Sicherheit konfigurieren?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Welche routing Methode werden API Management wird in Bereitstellungen zu mehreren geografischen Standorten verwendet?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Kann ich eine Ressourcenmanager Azure-Vorlage zum Erstellen einer Instanz der API Management-Dienst verwenden?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Kann ich ein selbst signiertes SSL-Zertifikat für ein Back-End verwenden?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Warum erhalte ich eine fehlgeschlagene Authentifizierung, wenn ich versuche, ein Repository GIT Klonen?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Funktioniert-API Management mit Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Kann ich eine API Verwaltungsdienst aus einem Abonnement zu einem anderen verschieben?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Wie kann ich dem Microsoft Azure-API-Team eine Frage stellen?

Sie können uns wenden, mithilfe einer der folgenden Optionen:

-   Stellen Sie Ihre Fragen in unseren [API Management MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Senden eine e-Mail an <apimgmt@microsoft.com>.
-   Senden Sie uns eine Anforderung Feature im [Forum Azure Feedback](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Was bedeutet es, wenn ein Feature in der Vorschau ist?

Wenn ein Feature in der Vorschau ist, bedeutet suchen wir aktiv nach sind Feedback auf wie das Feature für Sie funktioniert. Ein Feature in der Vorschau funktional abgeschlossen ist, aber es ist möglich, dass wir eine abgeschnitten werden als Antwort auf Kunden-Feedback ändern vornehmen, werden. Es empfiehlt sich, dass Sie auf ein Feature in der Vorschau in Ihrem Unternehmen verlassen nicht. Wenn Sie Feedback zu den Features von Vorschau haben, informieren Sie uns über eine der Optionen in Kontakt [wie kann ich dem Microsoft Azure-API-Team eine Frage?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Wie können die Verbindung zwischen dem Gateway-API Management und Back-End-Dienste werden gesichert?

Sie haben mehrere Optionen zum Sichern der Verbindung zwischen dem Gateway-API Management und Ihre Back-End-Dienste. Sie können:

-   Verwenden Sie HTTP-Standardauthentifizierung. Weitere Informationen finden Sie unter [Konfigurieren von API Settings](api-management-howto-create-apis.md#configure-api-settings).
- Verwenden Sie gemeinsamen SSL-Authentifizierung, wie unter [How to secure Back-End-Dienste mithilfe von Client Zertifikatauthentifizierung in Azure-API Management](api-management-howto-mutual-certificates.md)beschrieben.
- Verwenden Sie mithilfe der IP-auf Ihre Back-End-Dienst an. Wenn Sie eine Instanz Standard oder Premium Ebene-API Management haben, bleibt die IP-Adresse des Gateways konstant. Sie können Ihrer weißen Liste für diese IP-Adresse zulassen festlegen. Sie können die IP-Adresse Ihrer API Management-Instanz auf dem Dashboard im Azure-Portal erhalten.
- Herstellen einer Verbindung eine Azure-virtuellen Netzwerk mit Ihrer-API Management-Instanz. Weitere Informationen finden Sie unter [zum Einrichten von VPN-Verbindungen in Azure-API Management](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Wie kopiere ich meine API Management Service-Instanz auf eine neue Instanz?

Wenn Sie eine Instanz-API Management auf eine neue Instanz kopieren möchten, stehen Ihnen mehrere Optionen zur Verfügung. Sie können:

-   Sollten die Sicherung und Wiederherstellung Funktion bei API-Verwaltung. Weitere Informationen finden Sie unter [So implementieren der Wiederherstellung mithilfe der Dienst sichern und Wiederherstellen in Azure-API Management](api-management-howto-disaster-recovery-backup-restore.md).
-   Erstellen Sie eigener sichern und Wiederherstellen Sie Feature mithilfe der [API Management REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Verwenden Sie die REST-API zu speichern und Wiederherstellen der Elemente aus, den gewünschte Instanz für den Dienst.
-   Herunterladen Sie die Dienstkonfiguration mithilfe der Git, und Laden Sie es in eine neue Instanz. Weitere Informationen finden Sie unter [Informationen zum Speichern und konfigurieren Ihre Dienstkonfiguration -API Management mithilfe von Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kann ich meine API Management Instanz programmgesteuert verwalten?

Ja, können Sie die API Management mithilfe von programmgesteuert verwalten:

-   Die [API Management REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Microsoft Azure Service ApiManagement Management Bibliothek SDK](http://aka.ms/apimsdk).
-   Die [Bereitstellung von Service](https://msdn.microsoft.com/library/mt619282.aspx) und [Servicemanagement](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-Cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Wie füge ich einen Benutzer zur Gruppe Administratoren?

Hier ist, wie Sie einen Benutzer zur Gruppe Administratoren hinzufügen können:

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2. Wechseln Sie zu der Ressourcengruppe, die die API Management Instanz enthält, die Sie aktualisieren möchten.
3. API-Verwaltung weisen Sie der **Api Management** Teilnehmerrolle dem Benutzer zu.

Nun kann die neu hinzugefügte Mitwirkender Azure PowerShell- [Cmdlets](https://msdn.microsoft.com/library/mt613507.aspx)verwenden. So sieht so melden Sie sich als Administrator aus:

1. Verwenden der `Login-AzureRmAccount` -Cmdlet zum Anmelden.
2. Festlegen des Kontexts in das Abonnement, die mithilfe der Dienst wurde `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Abrufen eine einzelne anmelden URL mithilfe von `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Verwenden Sie die URL das Administratorportal Zugriff auf ein.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Warum ist die Richtlinie, die ich hinzufügen in den Richtlinien-Editor nicht verfügbar?

Wenn die Richtlinie, die Sie hinzufügen möchten positioniert abgeblendet oder schattiert ist die Richtlinieneditor, werden Sie sicher, dass Sie den richtigen Bereich für die Richtlinie ausführen. Jedes Informationsverwaltungsrichtlinien-Anweisung dient zur Verwendung in bestimmten Bereichen und Richtlinie Abschnitte. Die Richtlinie Abschnitte und Bereiche für eine Richtlinie finden Sie unter den Grenzwert für die Verwendung Abschnitt [API Informationsverwaltungsrichtlinien](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Wie verwende ich die versionsverwaltung API in API Management?

Es gibt einige Optionen Versioning API in API Management verwenden:

-   Sie können API Verwaltung APIs zum Darstellen der unterschiedlichen Versionen konfigurieren. Angenommen, haben Sie zwei verschiedene APIs, MyAPIv1 und MyAPIv2. Ein Entwickler kann es sich um die Version auswählen, die der Entwickler kann verwenden.
-   Sie können auch Ihre API mit einer URL konfigurieren, die eine Versionssegment, z. B. https://my.api enthalten nicht. Konfigurieren Sie dann ein Versionssegment des Vorgangs [Schreiben Sie URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) -Vorlage ein. Beispielsweise können Sie eine [URL-Vorlage](api-management-howto-add-operations.md#url-template) mit der Bezeichnung/Resource und einer [URL schreiben](api-management-howto-add-operations.md#rewrite-url-template) Vorlage mit der Bezeichnung /v1/Resource einen Vorgang enthält. Sie können den Wert Segment separat für jeden Vorgang ändern.
-   Wenn Sie ein "Standard" Versionssegment unter der-APIs-URL beibehalten möchten, klicken Sie auf ausgewählte Vorgänge, Festlegen einer Richtlinie, die die Richtlinie [festlegen Back-End-Dienst](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) wird verwendet, um den Pfad der Back-End-Anforderung ändern.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Wie richte ich mehrere Umgebungen in einer einzigen API?

Um mehrere Umgebungen, beispielsweise eine testen und einer Umgebung Herstellung einer einzigen API, eingerichtet haben Sie zwei Optionen. Sie können:

-   Host verschiedene APIs auf den gleichen Mandanten.
-   Die gleichen APIs auf anderen Mandanten zu hosten.

### <a name="can-i-use-soap-with-api-management"></a>Kann ich SOAP-API Verwaltung verwenden?

[SOAP Pass-Through-](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) Unterstützung ist jetzt verfügbar. Administratoren können die WSDL SOAP-Dienst importieren und Azure-API Management wird ein SOAP-front-End erstellen. Portal Dokumentation für Entwickler, Test Console, Richtlinien und Analytics sind alle verfügbaren für SOAP-Dienste.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Ist die Konstante API Management Gateway IP-Adresse? Kann ich es in Firewall-Regeln verwenden?

Bei den Ebenen Standard- und Premium ist die öffentliche IP-Adresse für den Mandanten-API Management statisch für die Gültigkeitsdauer des Mandanten, mit einigen Ausnahmen. Die IP-Adressänderungen in folgenden Fällen:

-   Der Dienst wird gelöscht und anschließend neu erstellt.
-   Das Dienst-Abonnement ist unterbrochen (z. B. für Nonpayment) und dann wiederhergestellt.
-   Hinzufügen oder Entfernen von Azure-virtuellen Netzwerk (Virtual Network nur auf der Premium Stufe können).

Für mehrere Region Bereitstellungen die regionale Adresse ändert, wenn die Region frei werden, und klicken Sie dann blockiertes (Sie können mehrere Region Bereitstellung nur auf der Premium Stufe verwenden).

Premium Ebene Mandanten, die für die Bereitstellung mit mehreren Region konfiguriert sind, werden eine öffentliche IP-Adresse pro Region zugewiesen.

Sie können Ihre IP-Adresse (oder die Adressen in einer Bereitstellung mit mehreren Region) auf der Seite Mandanten Azure-Portal erhalten.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kann ich einen OAuth 2.0 Autorisierung Server mit dem AD FS-Sicherheit konfigurieren?

Um weitere Informationen zum Konfigurieren eines OAuth 2.0 Autorisierung Servers mit Active Directory Federation Services (AD FS) Sicherheit finden Sie unter [Verwenden ADFS bei API-Verwaltung](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welche routing Methode werden API Management wird in Bereitstellungen zu mehreren geografischen Standorten verwendet?

Projektmanagement-API mithilfe der [Leistung Datenverkehr routing Methode](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) in Bereitstellungen zu mehreren geografischen Standorten. Eingehender Datenverkehr wird mit dem nächsten API Gateway geleitet. Wenn eine Region Offlinemodus wechselt, wird die eingehender Datenverkehr automatisch mit dem nächsten nächstgelegenen Gateway weitergeleitet. Weitere Informationen zum routing Methoden in [den Datenverkehr Manager routing Methoden](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kann ich eine Ressourcenmanager Azure-Vorlage zum Erstellen einer Instanz der API Management-Dienst verwenden?

Ja. [Azure-API Verwaltungsdienst](http://aka.ms/apimtemplate) Schnellstart Vorlagen anzuzeigen.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kann ich ein selbst signiertes SSL-Zertifikat für ein Back-End verwenden?

Ja. So sieht wie ein selbst signiertes Zertifikat von Secure Sockets Layer (SSL) für ein Back-End verwendet:

1. Erstellen einer [Back-End-](https://msdn.microsoft.com/library/azure/dn935030.aspx) Entität mithilfe der API Verwaltung.
2. Legen Sie die Eigenschaft **SkipCertificateChainValidation** auf **true**.
3. Wenn Sie nicht mehr selbstsignierte Zertifikaten zulassen möchten, löschen Sie die Back-End-Entität, oder legen Sie die Eigenschaft **SkipCertificateChainValidation** auf **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Warum erhalte ich eine fehlgeschlagene Authentifizierung, wenn ich versuche, ein Repository Git Klonen?

Wenn Sie Git Credential Manager verwenden oder wenn Sie versuchen, ein Repository Git mit Visual Studio klonen, können Sie ein bekanntes Problem mit im Dialogfeld Windows-Anmeldeinformationen auftreten. Das Dialogfeld Kennwortlänge auf 127 Zeichen beschränkt, und das Kennwort Microsoft generierten Werte abgeschnitten. Wir arbeiten, klicken Sie auf das Kennwort verkürzen. Jetzt verwenden Sie, um Ihre Git Repository Klonen Git Bash.

### <a name="does-api-management-work-with-azure-expressroute"></a>Funktioniert-API Management mit Azure ExpressRoute?

Ja. Projektmanagement-API funktioniert mit Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kann ich eine API Verwaltungsdienst aus einem Abonnement zu einem anderen verschieben?

Ja. Informationen hierzu finden Sie unter [Verschieben von Ressourcen zu einer neuen Ressourcengruppe oder Abonnements](../resource-group-move-resources.md).

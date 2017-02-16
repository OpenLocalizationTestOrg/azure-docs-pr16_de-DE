<properties 
    pageTitle="Informationen zum Speichern und konfigurieren Ihre API Management Dienstkonfiguration Git verwenden" 
    description="Informationen Sie zum Speichern und Konfigurieren der API Management Dienstkonfiguration Git verwenden." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Informationen zum Speichern und konfigurieren Ihre API Management Dienstkonfiguration Git verwenden

>[AZURE.IMPORTANT] Git Konfiguration für Management-API gibt es zurzeit in der Vorschau. Es ist funktional abgeschlossen, aber ist in der Vorschau, da wir Ihr Feedback zu diesem Feature aktiv Suchvorgänge sind. Es ist möglich, dass wir möglicherweise Sie eine abgeschnitten werden Stellen, damit es nicht je nachdem das Feature für die Verwendung in der Herstellung Umgebungen empfiehlt sich Reaktion auf Feedback von Kunden zu ändern. Wenn Sie Feedback oder Fragen haben, informieren Sie uns am `apimgmt@microsoft.com`.

Jede Instanz der API Management Service unterhält eine Konfigurationsdatenbank, die Informationen über die Konfiguration und Metadaten für die Instanz des Diensts enthält. Die Instanz des Diensts können geändert werden nach dem Ändern einer Einstellung im Portal Publisher, mithilfe eines PowerShell-Cmdlets oder tätigen eines Anrufs REST-API. Darüber hinaus können Sie auch Ihre Instanz Dienstkonfiguration Git verwenden, aktivieren Dienst Management Szenarien wie verwalten:

-   Konfiguration Versioning - herunterladen und speichern unterschiedliche Versionen von Ihrem Dienstkonfiguration
-   Konfiguration Änderungen gruppenweise – nehmen Sie Änderungen an mehrere Teile der Dienstkonfiguration in Ihrem lokalen Repository und die Änderungen an den Server mit einem einzigen Vorgang integrieren
-   Vertraute Git Toolchain und Workflow - verwenden Git Tools und Workflows, denen Sie bereits vertraut sind.

Das folgende Diagramm zeigt einen Überblick über die verschiedenen Verfahren zum Konfigurieren Ihrer API Management Service-Instanz.

![Git konfigurieren][api-management-git-configure]

Vorgenommenen Änderungen an den Dienst von Publisher-Portal, PowerShell-Cmdlets oder die REST-API Sie verwalten Ihre Datenbank mit Service Konfiguration der `https://{name}.management.azure-api.net` Endpunkt, wie er auf der rechten Seite des Diagramms angezeigt. Der linken Seite des Diagramms veranschaulicht, wie Sie Ihre Verwendung Git Dienstkonfiguration verwalten können und Git Repository des Diensts befindet sich am `https://{name}.scm.azure-api.net`.

Die folgenden Schritte bieten einen Überblick über die Verwaltung Ihrer API Management Service-Instanz Git verwenden.

1.  Aktivieren des Git-Zugriffs in Ihrem Dienst
2.  Speichern Sie Ihre Service-Konfigurations-Datenbank an Ihre Git repository
3.  Klonen der Git Repo auf Ihrem Computer
4.  Ziehen Sie die neuesten Repo auf Ihrem lokalen Computer, und klicken Sie auf Commit ausführen und Pushbenachrichtigungen Änderungen zurück, um Ihre repo
5.  Bereitstellen von Änderungen aus Ihrem Repo in Ihre Service-Konfigurations-Datenbank

In diesem Artikel wird beschrieben, wie aktivieren und verwenden zum Verwalten Ihrer Dienstkonfiguration Git und enthält Informationen für die Dateien und Ordner im Repository Git.

## <a name="to-enable-git-access"></a>Zum Aktivieren des Zugriffs Git

Sie können den Status Ihrer Git Konfiguration schnell anzeigen, indem der Git-Symbol in der oberen rechten Ecke des Portals Publisher anzeigen. In diesem Beispiel weist Git Access noch nicht aktiviert wurde.

![Git status][api-management-git-icon-enable]

Um anzuzeigen, und konfigurieren Sie Ihre Git Konfiguration, können Sie entweder klicken Sie auf das Symbol Git oder klicken Sie im Menü **Sicherheit** auf und navigieren Sie zur Registerkarte **Konfigurationsrepository** .

![Aktivieren Sie GIT][api-management-enable-git]

Zum Aktivieren des Zugriffs Git das Kontrollkästchen Sie **Zugriff Git aktivieren** .

Nach kurzer die Änderung gespeichert ist, und eine bestätigungsmeldung wird angezeigt. Beachten Sie, dass Git Symbol in Farbe, um anzugeben, dass Access Git aktiviert ist, und die Meldung zum Verbindungsstatus jetzt zeigt an, dass es sich bei, es sind nicht gespeicherte Änderungen an den Repository geändert hat. Dies ist, da die API Management-Konfigurationsdatenbank noch nicht im Repository gespeichert wurde.

![Git aktiviert][api-management-git-enabled]

>[AZURE.IMPORTANT] Vertrauliche Daten, die nicht definiert sind, wie Eigenschaften im Repository gespeichert werden und im Verlauf, bis Sie bleibt erneut Git Zugriff aktivieren und deaktivieren. Eigenschaften bieten eine sichere Stelle zum Verwalten der Konstante Zeichenfolgenwerte in allen API Konfiguration und Richtlinien geheimen Daten, einschließlich, so Sie diese direkt in Ihrer Richtlinie Anweisungen zu speichern müssen. Weitere Informationen finden Sie unter [Eigenschaften in Azure-API Informationsverwaltungsrichtlinien verwenden](api-management-howto-properties.md).

Informationen zum Aktivieren oder Deaktivieren von Git Access die REST-API verwenden finden Sie unter [Aktivieren oder Deaktivieren von Git Access die REST-API verwenden](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Die Dienstkonfiguration zum Git Repository gespeichert.

Dieser erste Schritt vor das Repository Klonen ist zu den aktuellen Status für die Dienstkonfiguration im Repository gespeichert. Klicken Sie auf **Konfiguration an Repository speichern**.

![Speichern Sie die Konfiguration][api-management-save-configuration]

Nehmen Sie die gewünschten Änderungen auf dem Bestätigungsbildschirm zur, und klicken Sie auf **Ok,** um zu speichern.

![Speichern Sie die Konfiguration][api-management-save-configuration-confirm]

Die Konfiguration wird nach kurzer gespeichert, und der Status der Konfiguration des Repositorys wird angezeigt, einschließlich Datum und Uhrzeit der letzten Änderung der Konfiguration und die letzte Synchronisierung zwischen die Dienstkonfiguration und dem Repository.

![Konfigurationsstatus][api-management-configuration-status]

Die Konfiguration zum Repository gespeichert wurde, können dupliziert werden.

Informationen zu diesem Vorgang die REST-API verwenden finden Sie unter [Konfiguration Momentaufnahme mithilfe der REST-API abzuschließen](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Um das Repository auf den lokalen Computer klonen

Um ein Repository klonen, benötigen Sie die URL zu Ihrem Repository, einen Benutzernamen und ein Kennwort ein. Der Benutzername und der URL werden am oberen Rand der Registerkarte **Konfigurationsrepository** angezeigt.

![Git Klonen][api-management-configuration-git-clone]

Das Kennwort wird am unteren Rand der Registerkarte **Konfigurationsrepository** generiert.

![Kennwort generieren][api-management-generate-password]

Um ein Kennwort zu generieren, zunächst sicherstellen Sie, dass der **Ablauf** auf das gewünschte Ablaufdatum und die Uhrzeit festgelegt ist, und klicken Sie dann auf **Token generieren**.

![Kennwort][api-management-password]

>[AZURE.IMPORTANT] Notieren Sie sich dieses Kennwort. Nachdem Sie diese Seite verlassen wird das Kennwort nicht wieder angezeigt werden.

In den folgenden Beispielen verwenden Sie das Tool Git Bash [Git für](http://www.git-scm.com/downloads) Windows, jedoch können Sie alle Git Tools, die Sie kennen.

Öffnen Sie der Git Tools in den gewünschten Ordner aus, und führen Sie den folgenden Befehl zum Klonen Git Repository auf Ihrem Computer, mit dem Befehl von Publisher-Portal bereitgestellt.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Geben Sie den Benutzernamen und das Kennwort ein, wenn Sie dazu aufgefordert werden.

Wenn Sie Fehler ausgegeben werden, versuchen Sie ändern Ihre `git clone` Befehl aus, um den Benutzernamen und das Kennwort, enthalten, wie im folgenden Beispiel gezeigt.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Wenn dies einen Fehler enthält, versuchen Sie URL-Codierung im Abschnitt Kennwort des Befehls. Eine schnelle Möglichkeit, dies zu tun ist in Visual Studio öffnen, und geben Sie den folgenden Befehl in das **Direktfenster**. Um das **Direktfenster**zu öffnen, öffnen Sie eine Lösung oder Projekt in Visual Studio (oder erstellen Sie eine neue leere), und wählen Sie **Windows**, **direkt** über das Menü **Debuggen** aus.

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Verwenden Sie das verschlüsselte Kennwort zusammen mit Ihrer Benutzer Name und Repository, um den Befehl Git erstellen.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Nachdem das Repository wird dupliziert können anzeigen und in Ihrem lokalen Dateisystem mit arbeiten. Weitere Informationen finden Sie unter [Datei- und Ordnernamen Struktur Bezug der lokalen Git Repository](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Zum Aktualisieren Ihres lokalen Repositorys für die aktuelle Instanz Dienstkonfiguration

Wenn Sie Änderungen an Ihrer API Management Service-Instanz in Publisher-Portal oder verwenden die REST-API vornehmen, müssen Sie diese Änderungen an den Repository speichern, bevor Ihr lokales Repository mit den neuesten Änderungen aktualisiert werden können. Hierzu klicken Sie auf der Registerkarte **Konfigurationsrepository** im Publisher-Portal auf **Konfiguration an Repository speichern** , und klicken Sie dann geben Sie den folgenden Befehl in Ihrem lokalen Repository.

    git pull

Vor dem Ausführen `git pull` stellen Sie sicher, dass Sie in den Ordner für Ihr lokales Repository befinden. Wenn Sie gerade abgeschlossen haben die `git clone` Befehl, und klicken Sie dann Sie Verzeichnis zu Ihrer Repo ändern müssen, indem Sie einen Befehl wie den folgenden.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Änderungen aus Ihrem lokalen Repo an den Server Repo zu übertragen.

Änderungen von Ihrem lokalen Repository, um an den Server Repository müssen Sie Ihre Änderungen übernehmen und dann zum Server-Repository zu übertragen. Um die Änderungen zu übernehmen, öffnen Sie Ihre Git Befehlstool, wechseln Sie zu dem Verzeichnis von Ihrem lokalen Repository und folgende Befehle.

    git add --all
    git commit -m "Description of your changes"

Um alle die Commit auf dem Server zu verschieben, führen Sie den folgenden Befehl ein.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Bereitstellen einer beliebigen Dienstkonfiguration ändert sich die API Management Service-Instanz

Sobald die lokalen Änderungen sind zugesichert und zum Server-Repository abgelegt, können Sie diese zu Ihrer API Management Service-Instanz bereitstellen.

![Bereitstellen][api-management-configuration-deploy]

Informationen zu diesem Vorgang die REST-API verwenden finden Sie unter [Bereitstellen Git Änderungen Konfigurationsdatenbank die REST-API verwenden](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Datei- und Ordnernamen Struktur Bezug der lokalen Git repository

Die Dateien und Ordner im lokalen Git Repository enthalten die Informationen zur Instanz Diensts.

| Element                       | Beschreibung                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| Stammordner-api-Verwaltung | Enthält der obersten Ebene Konfiguration für die Instanz des Diensts                                  |
| APIs Ordner                | Enthält die Konfiguration für die Instanz des Diensts-apis                            |
| Ordner und Gruppen              | Enthält die Konfiguration für die Gruppen in die Instanz des Diensts                          |
| Richtlinienordner            | Enthält die Richtlinien in die Instanz des Diensts                                              |
| PortalStyles Ordner        | Enthält die Konfiguration für Entwickler Portal Anpassungen in die Instanz des Diensts |
| Produkte-Ordner            | Enthält die Konfiguration für Produkte in die Instanz des Diensts                        |
| Vorlagenordner           | Enthält die Konfiguration für die e-Mail-Vorlagen in die Instanz des Diensts                 |

Jeder Ordner kann eine oder mehrere Dateien enthalten, und in einigen Fällen einen oder mehrere Ordner, beispielsweise eines Ordners für jede API, Produkt oder Gruppe. Die Dateien in den einzelnen Ordnern sind bestimmte für der Entität vom Typ durch den Namen des Ordners beschrieben.

| Dateityp | Zweck                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Von Konfigurationsinformationen zur jeweiligen Person                  |
| HTML-Code      | Eine Beschreibung der Einheit, die häufig in der Entwicklerportal angezeigt |
| XML       | Richtlinie Anweisungen                                                      |
| CSS       | Stylesheets für Entwickler Portal Anpassung                        |

Diese Dateien erstellt, gelöscht, bearbeitet, und verwaltet werden können auf Ihrem lokalen Dateisystem und die Änderungen bereitgestellt zurück, zu der der API Management Service-Instanz.

>[AZURE.NOTE] Die folgenden Elemente sind nicht in der Git Repository enthalten und können nicht mit Git konfiguriert werden.
>
>-    Benutzer
>-    Abonnements
>-    Eigenschaften
>-    Developer Portal Personen als Formatvorlagen

### <a name="root-api-management-folder"></a>Stammordner-api-Verwaltung

Die Quadratwurzel `api-management` Ordner enthält eine `configuration.json` Datei, die Informationen über die Instanz des Diensts in folgendem Format auf oberster Ebene enthält.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Die ersten vier Einstellungen (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, und `UserRegistrationTermsConsentRequired`) Karte, um die folgenden Einstellungen auf der Registerkarte **Identitäten** im Abschnitt **Sicherheit** .

| Einstellung der Identität                     | Ordnet                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | Kontrollkästchen **Umleiten anonyme Benutzer - Anmeldeseite** |
| UserRegistrationTerms                | **Nutzungsbedingungen für Benutzer beim Registrieren** Textfeld               |
| UserRegistrationTermsEnabled         | Kontrollkästchen **Anzeigen auf der Anmeldeseite rechtliche Hinweise**         |
| UserRegistrationTermsConsentRequired | Kontrollkästchen **Zustimmung erforderlich**                          |

![Identität Einstellungen][api-management-identity-settings]

Die folgenden vier Einstellungen (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, und `DelegationValidationKey`) Karte, um die folgenden Einstellungen auf der Registerkarte **Delegierung** im Abschnitt **Sicherheit** .

| Delegation Einstellung           | Ordnet                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Kontrollkästchen **Stellvertretung Anmeldung und Anmeldung**    |
| DelegationUrl                | Textfeld **Delegation Endpunkt-URL**        |
| DelegatedSubscriptionEnabled | Kontrollkästchen **Stellvertretung Produkt-Abonnements** |
| DelegationValidationKey      | **Schlüssel für Stellvertretung die Überprüfung** Textfeld        |

![Delegation-Einstellungen][api-management-delegation-settings]

Die endgültige Einstellung, `$ref-policy`, ordnet die globale Richtlinie Anweisungen Datei für die Instanz des Diensts.

### <a name="apis-folder"></a>APIs Ordner

Die `apis` Ordner enthält einen Ordner für jede API in die Instanz des Diensts die folgenden Elemente enthält.

-   `apis\<api name>\configuration.json`– Dies ist die Konfiguration für die-API und enthält Informationen über die URL der Back-End-Dienst und die Vorgänge. Dies ist die gleichen Informationen, die zurückgegeben wird, wenn Sie wurden aufrufen, [erhalten Sie eine bestimmte API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) mit `export=true` in `application/json` Format.
-   `apis\<api name>\api.description.html`– Dies ist die Beschreibung der API und entspricht der `description` Eigenschaft der [API Entität](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`-diesen Ordner enthält `<operation name>.description.html` Dateien, die für die Vorgänge in der API zuordnen. Jede Datei enthält die Beschreibung eines einzelnen Vorgangs in der API der zugeordnet ist die `description` Eigenschaft der [Vorgang Entität](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in die REST-API.

### <a name="groups-folder"></a>Ordner und Gruppen

Die `groups` Ordner enthält einen Ordner für jede Gruppe in der Dienstinstanz definiert.

-   `groups\<group name>\configuration.json`– Dies ist die Konfiguration für die Gruppe. Dies ist die gleichen Informationen, die zurückgegeben wird, wenn Sie den Vorgang [erhalten Sie eine bestimmte Gruppe](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) aufrufen wurden.
-   `groups\<group name>\description.html`– Dies ist die Beschreibung der Gruppe und entspricht der `description` Eigenschaft der [Gruppe Entität](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>Richtlinienordner

Die `policies` Ordner enthält die Richtlinie Anweisungen für Ihre Service-Instanz.

-   `policies\global.xml`-Richtlinien definiert im globalen Gültigkeitsbereich für Ihre Service-Instanz enthält.
-   `policies\apis\<api name>\`– Wenn Sie alle Richtlinien im API Gültigkeitsbereich definiert haben, werden sie in diesem Ordner enthalten.
-   `policies\apis\<api name>\<operation name>\`Ordner – Wenn Sie alle Richtlinien im Vorgang Gültigkeitsbereich definiert haben, werden sie in diesem Ordner in enthalten `<operation name>.xml` Dateien, die die Richtlinie Anweisungen für jeden Vorgang zugeordnet.
-   `policies\products\`– Wenn Sie alle Richtlinien im Produkt Gültigkeitsbereich definiert haben, werden sie in diesem Ordner, die enthält enthalten `<product name>.xml` Dateien, die die Richtlinie Anweisungen für jedes Produkt zuordnen.

### <a name="portalstyles-folder"></a>PortalStyles Ordner

Die `portalStyles` Ordner enthält Konfiguration und Stylesheets für Entwickler Portal Anpassungen für die Instanz des Diensts.

-   `portalStyles\configuration.json`-enthält die Namen der von der Entwicklerportal verwendeten Formatvorlagen
-   `portalStyles\<style name>.css`-Jede `<style name>.css` Datei enthält Formatvorlagen für die Entwicklerportal (`Preview.css` und `Production.css` standardmäßig).

### <a name="products-folder"></a>Produkte-Ordner

Die `products` Ordner enthält einen Ordner für jedes Produkt in der Dienstinstanz definiert.

-   `products\<product name>\configuration.json`– Dies ist die Konfiguration für das Produkt. Dies ist die gleichen Informationen, die zurückgegeben wird, wenn Sie den Vorgang [erhalten Sie ein bestimmtes Produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) aufrufen wurden.
-   `products\<product name>\product.description.html`– Dies ist die Beschreibung des Produkts und entspricht der `description` Eigenschaft der [Product-Entität](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in die REST-API.

### <a name="templates"></a>Vorlagen

Die `templates` Ordner enthält die Konfiguration für die [e-Mail-Vorlagen](api-management-howto-configure-notifications.md) von Instanz des Diensts.

-   `<template name>\configuration.json`– Dies ist die Konfiguration für die e-Mail-Vorlage.
-   `<template name>\body.html`– Dies ist der Textkörper der e-Mail-Vorlage.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen Methoden zum Verwalten Ihrer Service-Instanz finden Sie unter:

-   Verwalten Sie Ihrer Service-Instanz mithilfe der folgenden PowerShell-cmdlets
    -   [Bereitstellung von Service PowerShell-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Servicemanagement PowerShell-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Verwalten Sie Ihrer Service-Instanz im Publisher-Portal
    -   [Verwalten Sie Ihre erste API](api-management-get-started.md)
-   Verwalten Sie Ihrer Service-Instanz mithilfe der REST-API
    -   [API Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Schauen Sie sich einen Überblick video

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png





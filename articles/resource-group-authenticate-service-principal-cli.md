<properties
   pageTitle="Erstellen der wichtigsten Dienst mit Azure CLI | Microsoft Azure"
   description="Beschreibt, wie Azure CLI zum Erstellen einer Active Directory-Anwendung und Dienst Tilgungsanteile, und gewähren sie den Zugriff auf Ressourcen über rollenbasierte Access-Steuerelement verwenden. Es wird gezeigt, wie mit einem Kennwort oder Zertifikat Anwendung authentifizieren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Verwenden Sie zum Erstellen einer Tilgungsanteile Dienst, den Zugriff auf Ressourcen Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wenn Sie haben eine Anwendung oder das Skript, die Zugriff auf Ressourcen benötigt, möchten Sie wahrscheinlich nicht Ausführen dieses Prozesses unter Ihren eigenen Anmeldeinformationen. Möglicherweise müssen Sie andere Berechtigungen, die für die Anwendung aus, und nicht möchten, dass die Anwendung weiterhin verwenden Ihre Anmeldeinformationen ein, wenn Ihre Zuständigkeiten ändern. In diesem Fall erstellen Sie eine Identität für die Anwendung, die Anmeldeinformationen für die Authentifizierung und rollenzuweisungen enthält. Jedes Mal, wenn die app ausgeführt wird, authentifiziert er sich mit diesen Anmeldeinformationen an. In diesem Thema wird gezeigt, wie mit [Azure CLI für Mac, Linux und Windows](xplat-cli-install.md) zum Einrichten der Anwendung, die unter einem eigenen Anmeldeinformationen und Identität ausgeführt werden können.

Mit Azure CLI haben Sie zwei Optionen für die Authentifizierung einer AD-Anwendungs:

 - Kennwort
 - Zertifikat

In diesem Thema wird gezeigt, wie beide Optionen in Azure CLI zu verwenden. Wenn Sie beabsichtigen, von einem Framework Programmierung (solche Python, Ruby oder Node.js) in Azure anzumelden, möglicherweise Kennwortauthentifizierung die beste Option. Finden Sie bevor Sie sich entscheiden, ob Sie ein Kennwort oder Zertifikat verwenden im Abschnitt [Sample Applications](#sample-applications) Beispiele für in den verschiedenen Framework authentifizieren aus.

## <a name="active-directory-concepts"></a>Active Directory-Konzepte

In diesem Artikel erstellen Sie zwei Objekte - die Anwendung Active Directory (AD) und den wichtigsten Dienst an. Die AD-Anwendung ist die globale Darstellung der Anwendung. Sie enthält die Anmeldeinformationen (eine-Id und entweder ein Kennwort oder Zertifikat). Der wichtigsten Dienst ist die lokale Darstellung Ihrer Anwendung in einem Active Directory. Sie enthält die rollenzuweisung. Dieses Thema befasst sich eine einzelne-Mandanten-Anwendung, in dem die Anwendung in nur eine Organisation ausgeführt werden soll. Normalerweise verwenden Sie Single-Mandanten Applikationen für Applikationen Branchen, die ausgeführt werden, innerhalb Ihrer Organisation. In einer Anwendung Single-Mandanten müssen Sie eine AD-app und einem Dienst Tilgungsanteile.

Benötige Sie vielleicht – Warum beide Objekte ich? Dieser Ansatz ist sinnvoller, wenn Sie mit mehreren Mandanten Applications in Betracht ziehen. Sie verwenden in der Regel mit mehreren Mandanten Applikationen für Software als Service (SaaS) Applikationen, ausgeführt wird eine Anwendung in vielen verschiedenen Abonnements. Für Applikationen mit mehreren Mandanten Sie verfügen über eine AD-app und mehrere Dienst Hauptbenutzer (eine in jeder Active Directory, den Zugriff auf die app gewährt). Wenn Sie eine Anwendung mit mehreren Mandanten eingerichtet haben, finden Sie unter [Entwicklers Leitfaden für Autorisierung API Ressourcenmanager Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um dieses Thema abgeschlossen haben, müssen Sie über die erforderlichen Berechtigungen in Ihrer Azure Active Directory und in Ihrem Azure-Abonnement verfügen. Insbesondere müssen Sie möglicherweise Erstellen einer app in Active Directory und die Dienst Tilgungsanteile zu einer Rolle zuweisen. 

In Ihrem Active Directory muss Ihr Konto Administrator (z. B. **Globaler Administrator** oder **Benutzeradministrator**). Wenn Ihr Konto **die Benutzerrolle** zugewiesen ist, müssen Sie einen Administrator Ihre Berechtigungen erhöhen können.

Ihr Konto muss in Ihrem Abonnement verfügen `Microsoft.Authorization/*/Write` Access, das über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Access](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) -Administratorrolle gewährt wird. Wenn Ihr Konto die Rolle " **Mitwirkender** " zugewiesen ist, erhalten Sie eine Fehlermeldung, wenn Sie versuchen, die der Tilgungsanteile einer Rolle zuzuweisen. Erneut, müssen Sie Ihr Abonnementadministrator ausreichend Zugriff gewähren.

Jetzt, fahren Sie mit einem Abschnitt für entweder [Kennwort](#create-service-principal-with-password) oder [Zertifikat](#create-service-principal-with-certificate) Authentifizierung.

## <a name="create-service-principal-with-password"></a>Erstellen der wichtigsten Dienst mit Kennwort

In diesem Abschnitt gehen vor, um die AD-Anwendung mit einem Kennwort erstellen und Zuweisen von Rollen Reader zu den wichtigsten Dienst.

Lassen Sie uns führen Sie folgende Schritte aus.

1. Melden Sie sich bei Ihrem Konto aus.

        azure login

1. Sie haben zwei Optionen für die AD-Anwendung zu erstellen. Sie können entweder die AD-Anwendung und dem Dienst Hauptbenutzer in einem Schritt, oder diese erstellen getrennt. Erstellen sie in einem Schritt, wenn Sie nicht benötigte Homepage und URIs für Ihre app-ID angeben. Erstellen sie getrennt, wenn Sie diese Werte für eine Web-app festlegen müssen. In diesem Schritt werden beide Optionen angezeigt.

     - Zum der AD-Anwendung und dem Hauptbenutzer in einem Schritt erstellen Geben Sie den Namen der app und ein Kennwort ein, wie in den folgenden Befehl dargestellt:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Um die AD-Anwendung zu erstellen, geben Sie den Namen der app, eine Homepage URI, Bezeichner URIs und ein Kennwort, wie in den folgenden Befehl dargestellt:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Dieser Befehl gibt AppId-Wert. Zum Erstellen einer Tilgungsanteile Dienst bereitgestellt werden dieser Wert wird als Parameter in den folgenden Befehl aus:
     
            azure ad sp create -a <AppId>
     
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, finden Sie eine Fehlermeldung, die angibt, "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
    
     Bei beiden Optionen wird die neue Dienst Tilgungsanteile zurückgegeben. Die **Objekt-Id** ist erforderlich, wenn Sie Berechtigungen erteilen. Die Guid mit den **Dienst Tilgungsanteile Namen** aufgeführt ist erforderlich, bei der Anmeldung bei. Guid ist den gleichen Wert wie die app-Id an. Dieser Wert wird in der Stichprobe-Clientanwendungen wie die **Client-ID**bezeichnet. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Erteilen Sie die wichtigsten Berechtigungen für Dienste für Ihr Abonnement. In diesem Beispiel fügen Sie die Dienst Tilgungsanteile die Rolle **Leser** , das über die Berechtigung zum Lesen aller Ressourcen in das Abonnement gewährt. Anderen Rollen verwendet werden, finden Sie unter [RBAC: Standardrollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie für den Parameter **ServicePrincipalName** **ObjectId** , die Sie beim Erstellen der Anwendung verwendet. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über die erforderlichen Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung weist darauf hin Ihrem Konto **keine Autorisierung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich ' / Abonnements / {Guid}' enthält**. 

Das war's schon! Der AD-Anwendung und der Dienst Tilgungsanteile werden ausgerichtet. Im nächste Abschnitt wird gezeigt, wie Anmeldung mit den Anmeldeinformationen über Azure CLI erstellt wird. Wenn Sie die Anmeldeinformationen in der Anwendung Code verwenden möchten, müssen Sie nicht in diesem Thema nicht verarbeiten. Sie können auf die [Sample Applications](#sample-applications) Beispiele für Protokollierung mit Ihrer Anwendung-Id und Ihr Kennwort ein springen. 

### <a name="provide-credentials-through-azure-cli"></a>Geben Sie Anmeldeinformationen über Azure CLI

Nun müssen Sie melden Sie sich, wie die Anwendung zum Ausführen von Vorgängen.

1. Immer, wenn Sie als eine Tilgungsanteile Dienst anzumelden, müssen Sie die Mandanten-Id des Verzeichnisses für die AD-app bereitstellen. Ein Mandanten ist eine Instanz von Active Directory. Rufen Sie die Mandanten-Id für Ihr Abonnement gerade authentifizierten verwenden Sie zu können:

        azure account show

     Diese Funktion gibt zurück:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn Sie einen anderen Abonnementtyp die Mandanten-Id abrufen müssen, verwenden Sie den folgenden Befehl aus:

        azure account show -s {subscription-id}

2. Wenn die Client-Id verwenden Sie für die Anmeldung bei abgerufen werden müssen, verwenden Sie:

        azure ad sp show -c exampleapp --json

     Der Wert, der für die Anmeldung bei ist die Guid in den Dienst Benutzerprinzipalnamen aufgeführt.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Melden Sie sich als der Tilgungsanteile Dienst an.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Sie werden aufgefordert, das Kennwort aufgefordert. Geben Sie das Kennwort ein, die, das Sie beim Erstellen der Anwendungs AD angegeben.

        info:    Executing command login
        Password: ********

Sie sind jetzt als die Tilgungsanteile Dienst für den Dienst Hauptbenutzer authentifiziert, die Sie erstellt haben.

## <a name="create-service-principal-with-certificate"></a>Dienst mit Zertifikat Hauptbenutzer erstellen

In diesem Abschnitt führen Sie die Schritte an:

- Erstellen eines selbstsignierten Zertifikats
- Erstellen der AD-Anwendungs mit das Zertifikat und der Dienst Tilgungsanteile
- der Dienst Tilgungsanteile Rolle zuweisen

Wenn Sie die folgenden Schritte müssen Sie [OpenSSL](http://www.openssl.org/) installiert haben.

1. Erstellen Sie ein selbst signiertes Zertifikat.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinieren Sie die öffentlichen und privaten Schlüsseln.

        cat privkey.pem cert.pem > examplecert.pem

3. Öffnen Sie die Datei **examplecert.pem** , und suchen Sie nach der langen Folge von Zeichen zwischen **---beginnen Zertifikat (...)** und **---Beenden Zertifikat (...)**. Kopieren Sie die Zertifikatsdaten. Sie können diese Daten als Parameter übergeben, wenn vom Dienst Hauptbenutzer erstellt werden sollen.

1. Melden Sie sich bei Ihrem Konto aus.

        azure login

1. Sie haben zwei Optionen für die AD-Anwendung zu erstellen. Sie können entweder die AD-Anwendung und dem Dienst Hauptbenutzer in einem Schritt, oder diese erstellen getrennt. Erstellen Sie diese in einem Schritt, wenn Sie nicht benötigte Homepage und URIs für Ihre app-ID angeben. Erstellen sie getrennt, wenn Sie diese Werte für eine Web-app festlegen müssen. In diesem Schritt werden beide Optionen angezeigt.

     - Zum der AD-Anwendung und dem Hauptbenutzer in einem Schritt erstellen Geben Sie den Namen der app und die Zertifikatsdaten Siehe in den folgenden Befehl aus:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Um die AD-Anwendung zu erstellen, geben Sie den Namen der app, eine Homepage URI, Bezeichner URIs und die Zertifikatsdaten, wie in den folgenden Befehl dargestellt:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Dieser Befehl gibt AppId-Wert. Zum Erstellen einer Tilgungsanteile Dienst bereitgestellt werden dieser Wert wird als Parameter in den folgenden Befehl aus:
     
            azure ad sp create -a <AppId>
  
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, finden Sie eine Fehlermeldung, die angibt, "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
    
     Bei beiden Optionen wird die neue Dienst Tilgungsanteile zurückgegeben. Die Objekt-Id ist erforderlich, wenn Sie Berechtigungen erteilen. Die Guid mit den **Dienst Tilgungsanteile Namen** aufgeführt ist erforderlich, bei der Anmeldung bei. Guid ist den gleichen Wert wie die app-Id an. Dieser Wert wird in der Stichprobe-Clientanwendungen wie die **Client-ID**bezeichnet. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Erteilen Sie die wichtigsten Berechtigungen für Dienste für Ihr Abonnement. In diesem Beispiel fügen Sie die Dienst Tilgungsanteile die Rolle **Leser** , das über die Berechtigung zum Lesen aller Ressourcen in das Abonnement gewährt. Anderen Rollen verwendet werden, finden Sie unter [RBAC: Standardrollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie für den Parameter **ServicePrincipalName** **ObjectId** , die Sie beim Erstellen der Anwendung verwendet. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über die erforderlichen Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung weist darauf hin Ihrem Konto **keine Autorisierung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich ' / Abonnements / {Guid}' enthält**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Bereitstellen von Zertifikat durch automatischen Azure CLI-Skript

Nun müssen Sie melden Sie sich, wie die Anwendung zum Ausführen von Vorgängen.

1. Immer, wenn Sie als eine Tilgungsanteile Dienst anzumelden, müssen Sie die Mandanten-Id des Verzeichnisses für die AD-app bereitstellen. Ein Mandanten ist eine Instanz von Active Directory. Rufen Sie die Mandanten-Id für Ihr Abonnement gerade authentifizierten verwenden Sie zu können:

        azure account show

     Diese Funktion gibt zurück:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn Sie einen anderen Abonnementtyp die Mandanten-Id abrufen müssen, verwenden Sie den folgenden Befehl aus:

        azure account show -s {subscription-id}

1. Verwenden, um den Fingerabdruck des Zertifikats abrufen und nicht mehr benötigte Zeichen zu entfernen:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Die gibt eines Werts Fingerabdruck ähnelt:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Wenn die Client-Id verwenden Sie für die Anmeldung bei abgerufen werden müssen, verwenden Sie:

        azure ad sp show -c exampleapp

     Der Wert, der für die Anmeldung bei ist die Guid in den Dienst Benutzerprinzipalnamen aufgeführt.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Melden Sie sich als der Tilgungsanteile Dienst an.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Sie sind jetzt als Hauptbenutzer Dienst für den Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## <a name="sample-applications"></a>Beispiel für Applikationen

Im folgenden Beispiel Applikationen zeigen, wie als der Tilgungsanteile Dienst anmelden.

**.NET**

- [Bereitstellen einer SSH aktiviert virtueller Computer mit einer Vorlage mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppe mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Erste Schritte mit - Bereitstellen mit Azure Ressourcenmanager Vorlage - Ressourcen in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Erste Schritte mit - Ressourcengruppe verwalten - Ressourcen in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Python aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Verwalten von Azure Ressourcen- und Ressourcengruppen mit Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Node.js aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppe mit Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Bereitstellen einer SSH virtueller Computer mit einer Vorlage in Ruby aktiviert](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Verwalten von Azure Ressourcen- und Ressourcengruppen mit Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Nächste Schritte
  
- Die detaillierten Schritte zur Integration von einer Anwendung in Azure für die Verwaltung von Ressourcen finden Sie unter [Entwicklers Leitfaden für Autorisierung API Ressourcenmanager Azure](resource-manager-api-authentication.md).
- Um weitere Informationen zur Verwendung von Zertifikaten und Azure CLI zu erhalten, finden Sie unter [Authentifizierung basierenden Zertifikats mit Azure Service Hauptbenutzer von Linux Befehlszeile aus](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 

<properties
    pageTitle="Taste Tresor mit CLI verwalten | Microsoft Azure"
    description="Verwenden Sie dieses Lernprogramms, um allgemeine Aufgaben in Schlüssel Tresor automatisieren mithilfe der CLI"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Verwalten von Key Tresor mit CLI #
Azure-Taste Tresor ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter die [Taste Tresor Preise Seite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Verwenden Sie dieses Lernprogramm Azure-Taste Tresor zum Erstellen eines abgesicherten Containers (eine Tresor) in Azure, zum Speichern und Verwalten von cryptographic Tasten und vertrauliche Informationen in Azure behilflich helfen. Es führt Sie durch das Verfahren Schnittstelle Azure Plattformen Line einer Tresor zu erstellen, die einen Schlüssel oder Kennwort, das Sie dann mit der Anwendung Azure verwenden können. Es werden dann wie eine Anwendung klicken Sie dann die Taste oder Kennwort verwenden kann.

**Geschätzte Dauer:** 20 Minuten

>[AZURE.NOTE]  In diesem Lernprogramm enthält Anweisungen zum Azure-Anwendung schreiben, dass eine der Schritte enthält, die angezeigt wird, wie Sie eine Anwendung verwenden einen Schlüssel oder geheim im Key Tresor autorisieren, nicht.
>
>Sie können keine derzeit Azure-Taste Tresor Azure-Portal konfigurieren. Verwenden Sie stattdessen diese Plattformen Line Benutzeroberflächen-Anweisungen. Oder Azure PowerShell Anweisungen finden Sie unter [Lernprogramm entspricht](key-vault-get-started.md).

Überblick über die Taste Tresor Azure erhalten finden Sie unter [Neuigkeiten Azure-Taste Tresor?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten
Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Ein Microsoft Azure-Abonnement. Wenn Sie eine nicht verfügen, können Sie für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial)registrieren.
- Line Schnittstelle Version 0.9.1 oder höher. Um die neueste Version zu installieren, und verbinden Ihrem Abonnement Azure, finden Sie unter [Installieren und Konfigurieren der Azure Plattformen Line Schnittstelle](../xplat-cli-install.md).
- Eine Anwendung, die zum Verwenden der Schlüssel oder das Kennwort, die Sie in diesem Lernprogramm erstellen konfiguriert werden. Eine Beispiel-Anwendung wird vom [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343)zur Verfügung. Anweisungen finden Sie unter der zugehörigen Infodatei.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Aufrufen der Hilfe mit Azure Plattformen Line Benutzeroberfläche

In diesem Lernprogramm wird davon ausgegangen, dass Sie mit der Benutzeroberfläche Line (Bash, Terminal, Befehlszeile) vertraut sind.

Die – Hilfe oder h - Parameter zum Anzeigen von Hilfe für bestimmte Befehle verwendet werden kann. Alternativ der Azure-Hilfe [Befehl], [Optionen] Format auch verwendet werden kann, die gleiche Informationen zurückgegeben. Alle folgenden Befehle wird beispielsweise die gleiche Informationen zurück:

    azure account set --help

    azure account set -h

    azure help account set

Im Zweifelsfall zu den Parametern durch einen Befehl erforderlich, finden Sie Hilfe mit – Hilfe -h oder Azure Hilfe [Befehl].

Sie können auch die folgenden Lernprogramme beim Abrufen der vertrauten Azure Ressourcenmanager in Azure Plattformen Line Schnittstelle lesen:

- [So installieren und Konfigurieren von Azure Plattformen Command Line Interface](../xplat-cli-install.md)
- [Schnittstelle Azure Plattformen Line mit Azure Ressourcenmanager](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Verbinden mit Ihrer Abonnements

Wenn Sie mit einem Unternehmenskonto angemeldet haben, verwenden Sie den folgenden Befehl aus:

    azure login -u username -p password

oder wenn Sie melden Sie sich an, indem Sie interaktiv eingeben möchten.

    azure login

>[AZURE.NOTE]  Die Login-Methode funktioniert nur mit Organisations-Konto. Ein Organisations-Konto ist ein Benutzer, der von Ihrer Organisation verwaltet und in Ihrer Organisation Azure Active Directory-Mandanten definiert ist.


Wenn Sie aktuell kein Organisations-Konto und einem Microsoft-Konto anmelden bei Ihrem Abonnement Azure verwenden, können Sie einfach eine mithilfe der folgenden Schritte erstellen.

1.  Melden Sie sich an die Anmeldung bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com/), und klicken Sie in Active Directory.
2.  Wenn keine Verzeichnis vorhanden ist, wählen Sie erstellen Ihre Verzeichnis aus, und geben Sie die angeforderte Informationen.
3.  Wählen Sie Ihr Verzeichnis aus, und fügen Sie einen neuen Benutzer. Diese neue Benutzer ist ein Organisations-Konto an. Während der Erstellung des Benutzers werden beide eine e-Mail-Adresse für den Benutzer und ein temporäres Kennwort angegeben werden. Speichern Sie diese Informationen, wie er in einem anderen Schritt verwendet wird.
4.  Klicken Sie im Portal wählen Sie Einstellungen aus, und wählen Sie dann auf Administratoren. Wählen Sie hinzufügen aus, und fügen Sie den neuen Benutzer als Co-Administrator. Dadurch wird das Organisations-Konto in Ihr Azure-Abonnement zu verwalten.
5.  Schließlich melden Sie sich aus der Azure-Portal an, und melden Sie sich dann wieder in die neue Organisations-Konto verwenden. Wenn dies das erste Mal Protokollierung mit diesem Konto ist, werden Sie aufgefordert, das Kennwort ändern.

Weitere Informationen zu mit einem Unternehmenskonto mit Microsoft Azure finden Sie unter [Anmelden für Microsoft Azure als einer Organisation](../active-directory/sign-up-organization.md).

Wenn Sie mehrere Abonnements vorhanden sind, und geben Sie eine bestimmte für Azure-Taste Tresor verwenden möchten, geben Sie Folgendes ein, um die Abonnements für Ihr Konto finden Sie unter:

    azure account list

Klicken Sie dann zum Angeben des Abonnements verwenden, geben Sie Folgendes ein:

    azure account set <subscription name>

Weitere Informationen zum Konfigurieren von Azure Plattformen Line Benutzeroberflächen finden Sie unter [So installieren und Konfigurieren von Azure Plattformen Line Benutzeroberflächen](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Wechseln Sie bei der Verwendung von Azure Ressourcenmanager

Die Taste Tresor erfordert Azure Ressourcenmanager, also Geben Sie Folgendes in Azure Ressourcenmanager Modus wechseln:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Erstellen einer neuen Ressourcengruppe

Wenn Ressourcenmanager Azure verwenden, werden alle zugehörige Ressourcen innerhalb einer Ressourcengruppe erstellt. Wir werden eine neuen Ressourcengruppe 'ContosoResourceGroup' für die in diesem Lernprogramm erstellen.

    azure group create 'ContosoResourceGroup' 'East Asia'

Der erste Parameter ist Gruppe Ressourcenname und der zweite Parameter ist der Ort. Verwenden Sie für den Speicherort, den Befehl `azure location list` So geben Sie einen anderen Speicherort in der Zeile in diesem Beispiel identifizieren müssen. Wenn Sie weitere Informationen benötigen, geben Sie Folgendes ein:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrieren des Schlüssel Tresor-Anbieters für Ressourcen
Stellen Sie sicher, dass die Taste Tresor-Anbieter für Ressourcen in Ihrem Abonnement registriert ist:

`azure provider register Microsoft.KeyVault`

Dieses Dokument muss nur einmal pro Abonnement ausgeführt werden.


## <a name="create-a-key-vault"></a>Erstellen eines Key Tresor

Verwenden der `azure keyvault create` Befehl zum Erstellen eines Key Tresor. Dieses Skript weist drei erforderliche Parameter: eine Gruppe Ressourcenname, einen Namen für die wichtigsten Tresor und die geografische Position.

Wenn Sie den Namen Tresor ContosoKeyVault, den Namen der Ressource Gruppe von ContosoResourceGroup und den Speicherort der Ostasien verwenden, geben Sie beispielsweise:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Die Ausgabe dieses Befehls zeigt die Eigenschaften des Webparts für die wichtigsten Tresor, den Sie soeben erstellt haben. Die beiden wichtigsten Eigenschaften sind:

- **Name**: In diesem Beispiel ist dies ContosoKeyVault. Verwenden Sie diesen Namen für andere Schlüssel Tresor Cmdlets.
- **VaultUri**: In diesem Beispiel ist dies https://contosokeyvault.vault.azure.net. Anwendungen, die Ihrem Tresor durch seine REST-API verwenden müssen diesen URI verwenden.

Ihr Konto Azure ist jetzt auszuführende Vorgänge für diesen Key Tresor autorisiert. Ist noch niemand aus.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Hinzufügen einer Taste oder geheim zu den wichtigsten Tresor

Wenn Sie Azure-Taste Tresor einen Schlüssel Software geschützt für Sie erstellen möchten, verwenden Sie die `azure key create` Befehl aus, und geben Sie Folgendes ein:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Jedoch, wenn Sie einen vorhandenen Schlüssel in einer .pem Datei als lokale Dateien in einer Datei namens softkey.pem, die Sie in die Taste Tresor Azure hochladen möchten gespeichert haben, geben Sie vor, um die Taste aus Importieren der. PEM-Datei, die die Taste von Software im Dienst Schlüssel Tresor geschützt:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Sie können nun die Taste verweisen, die Sie erstellt oder geladen Azure-Taste Tresor, mithilfe des URI. Verwenden Sie die aktuelle Version abzurufenden **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** , und verwenden Sie **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** , um diese bestimmte Version abrufen.

Zum Hinzufügen einer geheim zum Tresor, welche ist ein Kennwort, das mit dem Namen SQLPassword und enthält, das den Wert der Pa$ $w0rd zum Azure-Taste Tresor, geben Sie Folgendes ein:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Sie können jetzt dieses Kennwort verweisen, das Sie Azure-Taste Tresor, mithilfe des-URI hinzugefügt. Verwenden Sie die aktuelle Version abzurufenden **https://ContosoVault.vault.azure.net/secrets/SQLPassword** , und verwenden Sie **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** , um diese bestimmte Version abrufen.

Sehen Sie sich den Schlüssel oder Schlüssel ein, den Sie soeben erstellt haben:

- Um den Key anzeigen möchten, geben Sie Folgendes ein:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Wenn Ihre geheim anzeigen möchten, geben Sie Folgendes ein:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Registrieren Sie sich eine Anwendung mit Azure Active Directory

Dieser Schritt würde normalerweise von einem Entwickler, auf einem anderen Computer ausgeführt werden. Es ist nicht spezifisch für Azure-Taste Tresor jedoch sind hier, Vollständigkeit enthalten.


>[AZURE.IMPORTANT] Zum Abschließen des Lernprogramms, Ihr Konto, dem Tresor und der Anwendung, die Sie in diesem Schritt registrieren müssen alle im gleichen Azure Verzeichnis sein.

Anwendungen, die eine Key Tresor verwenden müssen Authentifizierung über ein Token aus Azure Active Directory. Dazu muss der Besitzer der Anwendung zuerst die Anwendung in ihren Azure Active Directory registrieren. Am Ende der Registrierung wird der Besitzer der Anwendung die folgenden Werte:


- Eine **Anwendung-ID** (auch bekannt als eine Client-ID) und die **Taste Authentifizierung** (auch bekannt als das freigegebene Geheimnis). Die Anwendung muss beide Werte in Azure Active Directory, ein Token abzurufen präsentieren. Wie die Anwendung dazu konfiguriert ist, hängt von der Anwendung ab. Für die Taste Tresor Stichprobe-Anwendung legt der Besitzer der Anwendung diese Werte in der App aus.



Die Anwendung in Azure Active Directory registrieren:

1. Melden Sie sich Azure-Portal an.
2. Klicken Sie auf der linken Seite klicken Sie auf **Active Directory**, und wählen Sie dann aus dem Verzeichnis aus, in dem Sie eine Anwendung registrieren. <br> <br> Hinweis: Sie müssen dasselbe Verzeichnis auswählen, das das Azure-Abonnement enthält, mit dem Sie den Key Tresor erstellt haben. Wenn Sie nicht welches Verzeichnis dies wissen ist, klicken Sie auf **Einstellungen**, identifizieren das Abonnement, mit dem Sie den Key Tresor erstellt haben, und notieren Sie den Namen des Verzeichnisses in der letzten Spalte angezeigt.

3. Klicken Sie auf **ANWENDUNGEN**. Wenn Sie keine apps zu Ihrem Verzeichnis hinzugefügt wurden, wird dieser Seite nur die Verknüpfung **Hinzufügen einer App** angezeigt. Klicken Sie auf den Link, oder Sie können Sie die **Hinzufügen** auf der Befehlsleiste klicken.
4.  Die **Anwendung hinzufügen** -Assistenten auf der **Was möchten Sie tun?** Seite, klicken Sie auf **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.
5.  Klicken Sie auf der Seite **Teilen Sie uns über die Anwendung** Geben Sie einen Namen für die Anwendung, und wählen Sie **WEB Anwendung und/oder WEB-API** (die Standardeinstellung). Klicken Sie auf das Symbol "Weiter".
6.  Geben Sie auf der Seite **Eigenschaften der App** **SIGN ON URL** und **APP-ID-URI** für eine Webanwendung ein. Wenn eine Anwendung nicht diese Werte verfügt, können Sie diese können diesen Schritt stellen (z. B. http://test1.contoso.com für beide Felder könnten angeben werden). Es spielt keine Rolle, wenn diese Sites vorhanden sind. ausschlaggebend ist, dass die app-ID-URI für jede Anwendung für jede Anwendung in Ihrem Verzeichnis unterscheidet. Verzeichnis verwendet diese Zeichenfolge, um Ihre app zu identifizieren.
7.  Klicken Sie auf das Symbol "abgeschlossen", zum Speichern der Änderungen im Assistenten.
8.  Klicken Sie auf der Seite Schnellstart auf **Konfigurieren**.
9.  Führen Sie einen Bildlauf zum Abschnitt **Tasten** , wählen Sie die Dauer aus, und klicken Sie dann auf **Speichern**. Die Seite wird aktualisiert und zeigt nun einen Schlüsselwert. Konfigurieren Sie die Anwendung mit dieser Schlüsselwert und den **CLIENT-ID** -Wert. (Anweisungen für diese Konfiguration werden anwendungsspezifische.)
10. Kopieren Sie den Client-ID-Wert von dieser Seite aus, den Sie im nächsten Schritt zum Festlegen von Berechtigungen auf Ihrem Tresor verwenden möchten.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorisieren Sie die Anwendung verwendet den Schlüssel oder geheim

Wenn die Anwendung Zugriff auf die Taste oder geheim im Tresor zustimmen möchten, verwenden Sie die `azure keyvault set-policy` Befehl.

Angenommen, wenn der Name Ihres Tresor ContosoKeyVault ist und die Anwendung zu autorisieren eine Client-ID des 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed hat und Sie die Anwendung entschlüsseln, und melden Sie sich mit Tasten in Ihrem Tresor autorisieren möchten, führen Sie dann die folgenden:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Wenn Sie auf Windows-Befehlszeile ausgeführt werden, sollten Sie einfache Anführungszeichen in doppelte Anführungszeichen ersetzen und escape-auch die internen doppelten Anführungszeichen. Beispiel: "[\"entschlüsseln\",\"melden Sie sich\"]".

Wenn Sie die gleiche Anwendung zum Lesen von vertrauliche Informationen in Ihrem Tresor autorisieren möchten, führen Sie Folgendes aus:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Wenn Sie ein Hardwaresicherheitsmodul (HSM) verwenden möchten. ##

Für zusätzliche Sicherheit können Sie importieren oder Generieren von Tasten in Hardware Security Module (HSMs), die die Begrenzungslinie HSM nie lassen. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Wenn diese Anforderung nicht für Sie gilt, überspringen Sie diesen Abschnitt, und wechseln Sie zum [Löschen des wichtigsten Tresors und Tasten und Kennwörter zugeordnet ist](#delete-the-key-vault-and-associated-keys-and-secrets).

Um diese Schlüssel HSM geschützt zu erstellen, müssen Sie ein Abonnement Tresor verfügen, HSM geschützte Schlüssel unterstützt.

Wenn Sie die Keyvault erstellen, fügen Sie den Parameter 'Sku' hinzu:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Sie können diesen Tresor Software geschützt Tasten (wie zuvor angezeigt) und HSM-geschützte Schlüssel hinzufügen. Um einen Schlüssel HSM geschützt zu erstellen, legen Sie den Parameter Ziel 'HSM':

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Mit dem folgenden Befehl können um einen Schlüssel aus einer .pem-Datei auf Ihrem Computer zu importieren. Dieser Befehl importiert die Taste in HSMs Schlüssel Tresor Dienst an:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Mit dem nächste Befehl Importieren einer "zeigen Sie Ihren eigenen Schlüssel" Paket (BYOK). Auf diese Weise können Sie den Key in Ihrem lokalen HSM generieren und erläutert in UTC-Taste Tresor ohne die Taste verlassen die Begrenzungslinie HSM übertragen:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Ausführlichere Informationen zum Generieren dieses BYOK Pakets, finden Sie unter [HSM-Protected Tasten mit Azure Schlüssel Tresor verwenden](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Löschen Sie die Taste Tresor und zugeordneten Schlüssel und Kennwörter

Wenn Sie die Taste Tresor und den Schlüssel oder geheim, die ihn enthält nicht mehr benötigen, können Sie den Taste Tresor löschen, mithilfe des Befehls der Azure Keyvault löschen:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Alternativ können Sie eine gesamte Azure Ressourcengruppe, wozu auch die wichtigsten Tresor und anderen Ressourcen, die Sie in dieser Gruppe einbezogen löschen:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere Azure Plattformen Line Interface-Befehle

Andere Befehle, kann es sinnvoll, für die Verwaltung von Azure-Taste Tresor.

Dieser Befehl listet ein tabellarische Anzeigen aller Tasten und ausgewählten Eigenschaften:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Dieser Befehl listet ein tabellarische Anzeigen aller geheimen Namen und ausgewählten Eigenschaften:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Hier ist ein Beispiel für einen bestimmten Schlüssel zu entfernen:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Hier ist ein Beispiel für eine bestimmte geheim zu entfernen:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Nächste Schritte

Verweise für die Programmierung, finden Sie unter [Azure Schlüssel Tresor developer's Guide](key-vault-developers-guide.md).

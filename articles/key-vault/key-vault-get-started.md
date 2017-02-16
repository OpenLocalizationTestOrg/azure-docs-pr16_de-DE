<properties
    pageTitle="Erste Schritte mit Azure-Taste Tresor | Microsoft Azure"
    description="Verwenden Sie dieses Lernprogramm Azure-Taste Tresor zum Erstellen eines Containers gesicherten in Azure, zum Speichern und Verwalten von cryptographic Tasten und vertrauliche Informationen in Azure behilflich helfen."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Erste Schritte mit Azure-Taste Tresor #
Azure-Taste Tresor ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter die [Taste Tresor Preise Seite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Verwenden Sie dieses Lernprogramm Azure-Taste Tresor zum Erstellen eines abgesicherten Containers (eine Tresor) in Azure, zum Speichern und Verwalten von cryptographic Tasten und vertrauliche Informationen in Azure behilflich helfen. Es führt Sie durch den Prozess der Verwendung von Azure PowerShell einer Tresor zu erstellen, die einen Schlüssel oder Kennwort, das Sie dann mit der Anwendung Azure verwenden können. Es werden dann wie eine Anwendung dieser Schlüssel oder Kennwort verwenden kann.

**Geschätzte Dauer:** 20 Minuten

>[AZURE.NOTE]  In diesem Lernprogramm enthält Anleitungen zum Schreiben von Azure-Anwendung, die einer der Schritte enthält, und zwar so eine Anwendung verwenden einen Schlüssel oder geheim im Key Tresor autorisieren nicht.
>
>In diesem Lernprogramm verwendet Azure PowerShell. Plattformen Line Benutzeroberflächen-Anweisungen finden Sie unter [Lernprogramm entspricht](key-vault-manage-with-cli.md).

Überblick über die Taste Tresor Azure erhalten finden Sie unter [Neuigkeiten Azure-Taste Tresor?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Ein Microsoft Azure-Abonnement. Wenn Sie eine nicht verfügen, können Sie für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)anmelden.
- Azure PowerShell **Mindestversion von 1.1.0**. Zum Azure PowerShell installieren und ordnen Sie sie Ihr Abonnement Azure finden Sie unter [Informationen zum Installieren und konfigurieren Sie Azure PowerShell](../powershell-install-configure.md). Wenn Sie Azure PowerShell bereits installiert haben und die Version der Azure-PowerShell-Konsole nicht kennen, geben Sie `(Get-Module azure -ListAvailable).Version`. Wenn Sie Azure PowerShell Version 0.9.1 bis 0.9.8 installiert haben, können Sie immer noch in diesem Lernprogramm mit einigen geringfügigen Änderungen. Angenommen, Sie verwenden müssen die `Switch-AzureMode AzureResourceManager` Befehl und einige der Azure-Taste Tresor Befehle wurden geändert. Eine Liste der Schlüssel Tresor-Cmdlets für die Versionen 0.9.1 bis 0.9.8 finden Sie unter [Azure Schlüssel Tresor Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Eine Anwendung, die zum Verwenden der Schlüssel oder das Kennwort, die Sie in diesem Lernprogramm erstellen konfiguriert werden. Eine Beispiel-Anwendung wird vom [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343)zur Verfügung. Anweisungen finden Sie unter der zugehörigen Infodatei.


In diesem Lernprogramm für Anfänger Azure PowerShell ausgelegt ist, aber es wird davon ausgegangen, dass Sie die grundlegende Konzepte, wie Module, Cmdlets und Sitzungen verstehen. Weitere Informationen finden Sie unter [Erste Schritte mit Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Verwenden Sie ausführliche Hilfe für alle Cmdlet, die Sie in diesem Lernprogramm finden Sie unter, um das Cmdlet **Hilfe** .

    Get-Help <cmdlet-name> -Detailed

Geben Sie beispielsweise für die **Anmeldung-AzureRmAccount** -Cmdlet Hilfe hierzu:

    Get-Help Login-AzureRmAccount -Detailed

Sie können auch die folgenden Lernprogramme abzurufenden vertraut Azure Ressourcenmanager in Azure PowerShell lesen:

- [So installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
- [Mithilfe der Azure PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md)


## <a name="a-idconnectaconnect-to-your-subscriptions"></a><a id="connect"></a>Verbinden mit Ihrer Abonnements ##

Starten einer Sitzung Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto mit den folgenden Befehl aus:  

    Login-AzureRmAccount 

Beachten Sie, dass, wenn Sie einer bestimmten Instanz von Azure, beispielsweise Azure Government, verwenden Sie den Parameter-Umgebung, die mit diesem Befehl verwenden. Beispiel:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Geben Sie im Popupmenü Browserfenster Ihren Azure-Konto-Benutzernamen und Ihr Kennwort ein. Azure PowerShell Ruft alle Abonnements, die mit diesem Konto und standardmäßig zugeordnet sind, verwendet die erste ab.

Wenn Sie mehrere Abonnements vorhanden sind, und geben Sie eine bestimmte für Azure-Taste Tresor verwenden möchten, geben Sie Folgendes ein, um die Abonnements für Ihr Konto finden Sie unter:

    Get-AzureRmSubscription

Klicken Sie dann zum Angeben des Abonnements verwenden, geben Sie Folgendes ein:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Weitere Informationen zum Konfigurieren von Azure PowerShell finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


## <a name="a-idresourceacreate-a-new-resource-group"></a><a id="resource"></a>Erstellen einer neuen Ressourcengruppe ##

Wenn Sie Ressourcenmanager Azure verwenden, werden alle zugehörige Ressourcen innerhalb einer Ressourcengruppe erstellt. Wir erstellen Sie eine neue Ressourcengruppe mit dem Namen **ContosoResourceGroup** für dieses Lernprogramms:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a name="a-idvaultacreate-a-key-vault"></a><a id="vault"></a>Erstellen eines Key Tresor ##

Verwenden Sie das Cmdlet " [New-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) ", um eine Key Tresor zu erstellen. Dieses Cmdlet weist drei erforderliche Parameter: eine **Gruppe Ressourcenname**, einen **Key Tresor Namen**und den **geographischen Standort**.

Wenn Sie den Namen Tresor **ContosoKeyVault**, den Namen der Ressource Gruppe von **ContosoResourceGroup**und den Speicherort der **Ostasien**verwenden, geben Sie beispielsweise:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Dieses Cmdlet die Ausgabe zeigt die Eigenschaften des Webparts für die wichtigsten Tresor, den Sie soeben erstellt haben. Die beiden wichtigsten Eigenschaften sind:

- **Tresor Namen**: im Beispiel **ContosoKeyVault**hier. Verwenden Sie diesen Namen für andere Schlüssel Tresor Cmdlets.
- **Tresor URI**: im Beispiel https://contosokeyvault.vault.azure.net/ hier. Anwendungen, die Ihrem Tresor durch seine REST-API verwenden müssen diesen URI verwenden.

Ihr Konto Azure ist jetzt auszuführende Vorgänge für diesen Key Tresor autorisiert. Ist noch niemand aus.

>[AZURE.NOTE]  Wenn den Fehler, die **das Abonnement nicht registriert ist, um den Namespace 'Microsoft.KeyVault' verwenden** angezeigt, wenn Sie versuchen, führen Sie zum Erstellen von Ihrem neuen Key Tresor `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` , und führen Sie den Befehl neu-AzureRmKeyVault erneut aus. Weitere Informationen finden Sie unter [Register-AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a name="a-idaddaadd-a-key-or-secret-to-the-key-vault"></a><a id="add"></a>Hinzufügen einer Taste oder geheim zu den wichtigsten Tresor ##

Wenn Sie Azure-Taste Tresor einen Schlüssel Software geschützt für Sie erstellen möchten, verwenden Sie das Cmdlet [AzureKeyVaultKey hinzufügen](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) , und geben Sie Folgendes ein:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Jedoch, wenn Sie einen vorhandenen Schlüssel für Software geschützt, in haben ein. PFX-Datei gespeichert werden, zu Laufwerk C:\ in einer Datei namens softkey.pfx, die Sie zum Schlüssel Tresor Azure, hochladen möchten geben Sie Folgendes festlegen der Variable **Securepfxpwd** für ein Kennwort **123** für die. PFX-Datei:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Geben Sie Folgendes ein, um die Taste aus Importieren der. PFX-Datei, die die Taste von Software im Dienst Schlüssel Tresor geschützt:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Sie können jetzt Schlüssel verweisen, die Sie erstellt oder geladen Azure-Taste Tresor, mithilfe des URI. Verwenden Sie die aktuelle Version abzurufenden **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** , und verwenden Sie **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** , um diese bestimmte Version abrufen.  

Um den URI für diese Taste anzeigen möchten, geben Sie Folgendes ein:

    $Key.key.kid

Um einen geheimen der Tresor hinzufügen, ist ein Kennwort, das mit dem Namen SQLPassword und hat den Wert von Pa$ w0rd zum Azure-Taste Tresor, konvertieren Sie zuerst den Wert von Pa$ $w0rd in eine sichere Zeichenfolge durch Eingeben der folgenden:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Geben Sie dann Folgendes ein:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Sie können jetzt dieses Kennwort verweisen, das Sie Azure-Taste Tresor, mithilfe des-URI hinzugefügt. Verwenden Sie die aktuelle Version abzurufenden **https://ContosoVault.vault.azure.net/secrets/SQLPassword** , und verwenden Sie **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** , um diese bestimmte Version abrufen.

Um den URI für diese geheim anzeigen möchten, geben Sie Folgendes ein:

    $secret.Id

Sehen Sie sich den Schlüssel oder Schlüssel ein, den Sie soeben erstellt haben:

- Um den Key anzeigen möchten, geben Sie Folgendes ein:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Wenn Ihre geheim anzeigen möchten, geben Sie Folgendes ein:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Nun kann Ihren Key Tresor und Key oder geheim für Applikationen zu verwenden. Sie müssen Clientanwendungen über diese autorisieren.  

## <a name="a-idregisteraregister-an-application-with-azure-active-directory"></a><a id="register"></a>Registrieren Sie sich eine Anwendung mit Azure Active Directory ##

Dieser Schritt würde normalerweise von einem Entwickler, auf einem anderen Computer ausgeführt werden. Es ist nicht spezifisch für Azure-Taste Tresor, jedoch sind hier Vollständigkeit enthalten.


>[AZURE.IMPORTANT] Zum Abschließen des Lernprogramms, Ihr Konto, dem Tresor und der Anwendung, die Sie in diesem Schritt registrieren müssen alle im gleichen Azure Verzeichnis sein.

Anwendungen, die eine Key Tresor verwenden müssen Authentifizierung über ein Token aus Azure Active Directory. Dazu muss der Besitzer der Anwendung zuerst die Anwendung in ihren Azure Active Directory registrieren. Am Ende der Registrierung wird der Besitzer der Anwendung die folgenden Werte:


- Eine **Anwendung-ID** (auch bekannt als eine Client-ID) und die **Taste Authentifizierung** (auch bekannt als das freigegebene Geheimnis). Die Anwendung muss beide Werte in Azure Active Directory, ein Token abzurufen präsentieren. Wie die Anwendung dazu konfiguriert ist, hängt von der Anwendung ab. Für die Taste Tresor Stichprobe-Anwendung legt der Besitzer der Anwendung diese Werte in der App aus.

Die Anwendung in Azure Active Directory registrieren:

1. Melden Sie sich zum klassischen Azure-Portal aus.
2. Klicken Sie auf der linken Seite klicken Sie auf **Active Directory**, und wählen Sie dann aus dem Verzeichnis aus, in dem Sie eine Anwendung registrieren. <br> <br> **Hinweis:** Sie müssen das gleiche Verzeichnis auswählen, das das Azure-Abonnement enthält, mit dem Sie den Key Tresor erstellt haben. Wenn Sie nicht welches Verzeichnis dies wissen ist, klicken Sie auf **Einstellungen**, identifizieren das Abonnement, mit dem Sie den Key Tresor erstellt haben, und notieren Sie den Namen des Verzeichnisses in der letzten Spalte angezeigt.

3. Klicken Sie auf **ANWENDUNGEN**. Wenn Sie keine apps zu Ihrem Verzeichnis hinzugefügt wurden, werden auf dieser Seite nur auf den Link **App hinzufügen** . Klicken Sie auf den Link, oder Sie können auch auf **Hinzufügen** auf der Befehlsleiste.
4.  Die **Anwendung hinzufügen** -Assistenten auf der **Was möchten Sie tun?** Seite, klicken Sie auf **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.
5.  Klicken Sie auf der Seite **Teilen Sie uns über die Anwendung** Geben Sie einen Namen für die Anwendung, und wählen Sie dann auf **WEB Anwendung und/oder WEB-API** (die Standardeinstellung). Klicken Sie auf das Symbol " **Weiter** ".
6.  Geben Sie auf der Seite **Eigenschaften der App** **SIGN ON URL** und **APP-ID-URI** für eine Webanwendung ein. Wenn eine Anwendung nicht diese Werte verfügt, können Sie diese können diesen Schritt stellen (z. B. http://test1.contoso.com für beide Felder könnten angeben werden). Es spielt keine Rolle, wenn diese Sites vorhanden sind. Ausschlaggebend ist, dass die app-ID-URI für jede Anwendung für jede Anwendung in Ihrem Verzeichnis unterscheidet. Verzeichnis verwendet diese Zeichenfolge, um Ihre app zu identifizieren.
7.  Klicken Sie auf das Symbol " **abgeschlossen** " zum Speichern der Änderungen im Assistenten.
8.  Klicken Sie auf der Seite **Schnellstart** auf **Konfigurieren**.
9.  Führen Sie einen Bildlauf zum Abschnitt **Tasten** , wählen Sie die Dauer aus, und klicken Sie dann auf **Speichern**. Die Seite wird aktualisiert und zeigt nun einen Schlüsselwert. Konfigurieren Sie die Anwendung mit dieser Schlüsselwert und den **CLIENT-ID** -Wert. (Anweisungen für diese Konfiguration sind anwendungsspezifische.)
10. Kopieren Sie den Client-ID-Wert von dieser Seite aus, den Sie im nächsten Schritt zum Festlegen von Berechtigungen auf Ihrem Tresor verwenden möchten.

## <a name="a-idauthorizeaauthorize-the-application-to-use-the-key-or-secret"></a><a id="authorize"></a>Autorisieren Sie die Anwendung verwendet den Schlüssel oder geheim ##

Wenn die Anwendung Zugriff auf die Taste oder geheim im Tresor zustimmen möchten, verwenden Sie das Cmdlet "  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) " ein.

Ist Ihr Name Tresor **ContosoKeyVault** und die Anwendung zu autorisieren verfügt über eine Client-ID des 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, und Sie die Anwendung entschlüsseln, und melden Sie sich mit Tasten in Ihrem Tresor autorisieren möchten, führen Sie beispielsweise die folgenden:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Wenn Sie die gleiche Anwendung zum Lesen von vertrauliche Informationen in Ihrem Tresor autorisieren möchten, führen Sie Folgendes aus:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a name="a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm"></a><a id="HSM"></a>Wenn Sie ein Hardwaresicherheitsmodul (HSM) verwenden möchten. ##

Für zusätzliche Sicherheit können Sie importieren oder Generieren von Tasten in Hardware Security Module (HSMs), die die Begrenzungslinie HSM nie lassen. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Wenn diese Anforderung nicht für Sie gilt, überspringen Sie diesen Abschnitt, und wechseln Sie zum [Löschen des wichtigsten Tresors und Tasten und Kennwörter zugeordnet ist](#delete).

Um diese Schlüssel HSM geschützt zu erstellen, müssen Sie die [Azure Schlüssel Tresor Premium Service Stufe zur Unterstützung von Tasten HSM geschützt](https://azure.microsoft.com/pricing/free-trial/)verwenden. Beachten Sie darüber hinaus dieses Feature ist nicht verfügbar für Azure China ein.


Wenn Sie den Taste Tresor erstellen, fügen Sie den Parameter **- SKU** hinzu:


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Sie können diesen Key Tresor Software geschützt Tasten (wie zuvor angezeigt) und HSM-geschützte Schlüssel hinzufügen. Zum Erstellen eines HSM geschützte Schlüssels legen Sie die **-Ziel** 'HSM'-Parameter:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Sie können den folgenden Befehl verwenden, um einen Schlüssel aus importieren ein. PFX-Datei auf Ihrem Computer. Dieser Befehl importiert die Taste in HSMs Schlüssel Tresor Dienst an:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Mit dem nächste Befehl Importieren einer "zeigen Sie Ihren eigenen Schlüssel" Paket (BYOK). Dieses Szenario können Sie den Key in Ihrem lokalen HSM generieren, und erläutert in UTC-Taste Tresor ohne die Taste verlassen die Begrenzungslinie HSM übertragen:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Ausführlichere Anweisungen dazu, wie Sie diese BYOK Paket generieren finden Sie unter [generieren und durchstellen HSM geschützt Tasten zum Azure Schlüssel Tresor](key-vault-hsm-protected-keys.md).

## <a name="a-iddeleteadelete-the-key-vault-and-associated-keys-and-secrets"></a><a id="delete"></a>Löschen Sie die Taste Tresor und zugeordneten Schlüssel und Kennwörter ##

Wenn Sie die Taste Tresor und den Schlüssel oder geheim, die ihn enthält nicht mehr benötigen, können Sie den Taste Tresor mithilfe des Cmdlets [Entfernen-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) löschen:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Alternativ können Sie eine gesamte Azure Ressourcengruppe, wozu auch die wichtigsten Tresor und anderen Ressourcen, die Sie in dieser Gruppe einbezogen löschen:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a name="a-idotheraother-azure-powershell-cmdlets"></a><a id="other"></a>Andere Azure PowerShell-Cmdlets ##

Andere Befehle, die Sie möglicherweise für die Verwaltung von Azure-Taste Tresor hilfreich finden:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Dieser Befehl ruft eine tabellarische Anzeigen aller Tasten und ausgewählten Eigenschaften ab.
- `$Keys[0]`: Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel
- `Get-AzureKeyVaultSecret`: Dieser Befehl listet ein tabellarische Anzeigen aller geheimen Namen und ausgewählten Eigenschaften.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Beispiel So entfernen Sie einen bestimmten Schlüssel.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Beispiel So entfernen Sie eine bestimmte geheim.


## <a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte ##

Ein auszuführenden Lernprogramm, die Azure-Taste Tresor in einer Webanwendung verwendet wird, finden Sie unter [Verwenden Azure Schlüssel Tresor aus einer Web-Anwendung](key-vault-use-from-web-application.md).

Um anzuzeigen, wie Ihre Key Tresor verwendet wird, finden Sie unter [Azure Schlüssel Tresor Protokollierung](key-vault-logging.md).

Eine Übersicht über die neuesten Azure-PowerShell-Cmdlets für die Taste Tresor Azure finden Sie unter [Azure Schlüssel Tresor Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Verweise für die Programmierung, finden Sie unter [Azure Schlüssel Tresor developer's Guide](key-vault-developers-guide.md).

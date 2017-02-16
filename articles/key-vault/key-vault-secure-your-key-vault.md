<properties
    pageTitle="Sichern Ihrer Key Tresor | Microsoft Azure"
    description="Verwalten Sie Zugriffsberechtigungen für Key Tresor für die Verwaltung von Depots und Tasten und Kennwörter an. Authentifizierung und Autorisierung Modell für Key Tresor und so Ihre Key Tresor secure"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Sichern Sie Ihrer Key Tresor

Azure-Taste Tresor ist ein Clouddienst, der Schlüssel für die Verschlüsselung und Schlüssel (z. B. Zertifikate, Verbindungszeichenfolgen, Kennwörter) e Cloud sichert. Da diese Daten vertraulich ist und geschäftliche Relevanz, Sichern des Zugriffs auf Ihre Key Depots so, dass nur autorisierte soll können Anwendungen und Benutzer Key Tresor zugreifen. In diesem Artikel bietet einen Überblick über die wichtigsten Tresor Access-Modell, Authentifizierung und Autorisierung erläutert und beschrieben, wie Sie Zugriff auf wichtige Tresor Cloud e mit Beispiel sichern.

## <a name="overview"></a>(Übersicht)

Zugriff auf eine wichtige Tresor wird gesteuert, über zwei separate Schnittstellen: Management Plane und Data Plane. Für beide Ebenen ist ordnungsgemäße Authentifizierung und Autorisierung erforderlich, bevor ein Anrufer (einen Benutzer oder eine Anwendung) Zugriff auf wichtige Tresor abrufen kann. Authentifizierung stellt die Identität des Anrufers her, während die Autorisierung bestimmt, welche Vorgänge Anrufer ausführen darf.

Verwenden Sie für die Authentifizierung sowohl Management Plane und Data Plane Azure Active Directory. Für die Autorisierung verwendet Management Ebene rollenbasierte Access-Steuerelements (RBAC), während Daten-Ebene Key Tresor Zugriffsrichtlinie verwendet.

Hier ist eine kurze Übersicht über die folgenden Themen behandelt:

[Authentifizierung mit Azure Active Directory](#authentication-using-azure-active-direcrory) - in diesem Abschnitt wird erläutert, wie ein Anrufer mit Azure Active Directory eine Key Tresor über Management Plane und Data Plane Zugriff auf authentifiziert. 

[Management Plane und Data Plane](#management-plane-and-data-plane) - Verwaltung Plane und Data Plane sind zwei Access Ebenen, die für den Zugriff auf Ihre Key Tresor verwendet. Jede Ebene Access unterstützt bestimmte Vorgänge. In diesem Abschnitt werden die Endpunkte Access, Vorgänge unterstützt und Steuerelement angewandte durch jede Ebene zugreifen. 

[Management Ebene Access Control](#management-plane-access-control) - In diesem Abschnitt werden betrachten wir gewähren des Zugriffs auf Management Ebene Vorgänge mit rollenbasierte Access-Steuerelement.

[Daten plane Steuerung des Benutzerzugriffs](#data-plane-access-control) – in diesem Abschnitt beschrieben, wie Sie die wichtigsten Tresor Zugriffsrichtlinie zum Steuern des Zugriffs für Daten Ebene verwenden.

[Beispiel](#example) – in diesem Beispiel wird beschrieben, wie Sie die Einrichtung von Access-Steuerelement, um den Key Tresor drei verschiedenen Teams (Sicherheitsteam, Entwickler/Operatoren und Auditoren) bestimmte Vorgänge zum Entwickeln, verwalten und Überwachen einer Anwendungs in Azure ausführen dürfen.


## <a name="authentication-using-azure-active-directory"></a>Verwenden von Azure Active Directory Authentifizierung

Wenn Sie eine Key Tresor in einem Azure-Abonnement erstellen, ist es automatisch das Abonnement des Azure Active Directory-Mandanten zugeordnet. In diesem Mandanten auf diesen Key Tresor zugreifen müssen alle Anrufer (Anwender und Applications) registriert sein. Eine Anwendung oder ein Benutzer muss sich mit Azure Active Directory Key Tresor Zugriff auf authentifizieren. Dies gilt für beide Management Ebene und Daten Ebene-Zugriff. In beiden Fällen kann eine Anwendung Key Tresor auf zwei Arten zugreifen:

-  **Benutzer + Access app** - ist dies in der Regel für Applikationen, die im Auftrag eines Benutzers angemeldet Key Tresor zugreifen. Azure PowerShell, Azure-Portal sind Beispiele für diese Art des Zugriffs auf. Es gibt zwei Möglichkeiten, um Benutzern Zugriff gewähren: eine Möglichkeit besteht darin, gewähren des Zugriffs für Benutzer, damit die wichtigsten Tresor aus einer beliebigen Anwendung zugreifen können, und die andere Möglichkeit besteht darin, erteilen einem Benutzerzugriff auf wichtige Tresor nur, wenn sie eine bestimmte Anwendung (zusammengesetzten Identität genannt) verwenden. 
-  **app schreibgeschützten Zugriff** - Anwendung, die Daemon Services ausführen, Hintergrund Aufträge usw.. Der Anwendung Identität erhält Zugriff auf die wichtigsten Tresor.

In den beiden Typen von Applications die Anwendung mit Azure Active Directory verhindert, die nicht [Unterstützte Authentifizierungsmethoden](../active-directory/active-directory-authentication-scenarios.md) authentifiziert und erhält ein Token. Verwendete Authentifizierungsmethode hängt von den Anwendungstyp ab. Klicken Sie dann die Anwendung dieses Token verwendet und sendet REST-API Anforderung an Key Tresor. Bei der Verwaltung Ebene des Zugriffs werden die Anforderungen über Azure Ressourcenmanager Endpunkt geleitet. Beim Zugriff auf Daten Ebene Vaulting der Applikationen kommuniziert direkt auf eine andere Taste Endpunkt aus. Finden Sie weitere Details auf die [gesamte Authentifizierung Fluss](../active-directory/active-directory-protocols-oauth-code.md)aus. 

Der Namen der Ressource für den Anwendung ein Token anfordert unterscheidet sich je nachdem, ob die Anwendung Management Flugzeug oder Daten Ebene zugreifen wird. Der Namen der Ressource ist daher Management Flugzeug oder Daten Ebene Endpunkten in der Tabelle in einem der folgenden Abschnitte je nach der Azure-Umgebung beschrieben.

Haben Sie eine einzelne Verfahren für die Authentifizierung sowohl Management und Daten Ebene bietet eine eigene Vorteile:

- Organisationen können zentral Zugriff auf alle wichtigen Tresore in ihrer Organisation steuern.
- Wenn ein Benutzer lässt, diese sofort keinen Zugriff mehr auf alle wichtigen Tresore in der Organisation
- Organisationen können Authentifizierung über die Optionen in Azure Active Directory (z. B. aktivieren mehrstufige Authentifizierung zur Erhöhung der Sicherheit) anpassen.

## <a name="management-plane-and-data-plane"></a>Management Plane und Data plane

Azure-Taste Tresor ist eine Azure Service über Modell zur Bereitstellung von Azure Ressourcenmanager verfügbar. Beim Erstellen eines Key Tresor erhalten Sie einen virtuellen Container, in dem Sie andere Objekte wie Schlüssel, Kennwörter und Zertifikate erstellen können. Dann können Sie den Key-Verwaltungsebene und Daten Ebene einsetzen, um bestimmte Vorgänge ausführen Tresor zugreifen. Management Ebene Benutzeroberflächen dient zum Verwalten Ihrer Key Tresor selbst, z. B. erstellen, löschen, Aktualisieren der Attribute Key Tresor und Einrichten von Access-Richtlinien für Daten Ebene. Daten Ebene Benutzeroberfläche dient zum Hinzufügen, löschen, ändern und verwenden Sie die Tasten, Kennwörter und Zertifikate in Ihrem Key Tresor gespeichert.

Die Managementschnittstellen Ebene und Daten Ebene über verschiedene Endpunkte zugegriffen werden (siehe Tabelle). Die zweite Spalte in der Tabelle beschreibt die DNS-Namen für diese Endpunkte in unterschiedlichen Azure-Umgebungen. Die dritte Spalte beschreibt die Vorgänge, die Sie aus jeder Access-Ebene ausführen können. Jede Ebene Access verfügt auch über eine eigene Zugriffskontrollmechanismus: für Management Ebene Access Steuerelement mit Azure Resource Manager Role-Based Access Steuerelement (RBAC) festgelegt ist, während er sich für Ebene Access-Steuerelement mit Key Tresor Zugriffsrichtlinie festgelegt ist.

| Access-Ebene | Access-Endpunkte | Vorgänge | Access angibt|
|--------------|------------------|--------------------|--------|
| Projektmanagement-Ebene|**Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Deutschland:**<br> Management.microsoftazure.de:443 | Key Tresor erstellen gelesen /, aktualisieren und löschen <br> Festlegen von Richtlinien für Key Tresor access<br>Festlegen von Tags für Key Tresor | Azure-Manager rollenbasierte erfolgt die Steuerung (RBAC) |
| Daten Ebene | **Globale:**<br> &lt;Tresor-Name&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Tresor-Name&gt;. vault.azure.cn:443<br><br> **Azure US-Regierung:**<br> &lt;Tresor-Name&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresor-Name&gt;. vault.microsoftazure.de:443 | Tastenkombinationen: Entschlüsseln, verschlüsseln, UnwrapKey, WrapKey, vergewissern Sie sich, melden Sie sich, erhalten, Liste, aktualisieren, erstellen, importieren und löschen, Sicherung wiederherstellen<br><br> Für vertrauliche Daten: Abrufen, Liste festlegen, löschen | Key Tresor Zugriffsrichtlinie|

Die Verwaltung Ebene und Daten Ebene Access-Steuerelemente arbeiten unabhängig. Beispielsweise wenn Sie eine Anwendungszugriff zum Verwenden von Tastenkombinationen in einem Key Tresor gewähren möchten, müssen Sie nur Daten Ebene Access mit Key Tresor-Richtlinien zu erteilen und keine Verwaltung Ebene des Zugriffs für diese Anwendung erforderlich ist. Und umgekehrt, wenn einen Benutzer in der Lage Tresor Eigenschaften und Kategorien zu lesen, aber keine Zugang zu Tasten, Schlüssel oder Zertifikate sein soll, können Sie diese Benutzer erteilen 'Lesen' Zugriff mit RBAC und keinen Zugriff auf Daten Ebene ist erforderlich.

## <a name="management-plane-access-control"></a>Management Ebene Access-Steuerelements

Vorgänge, die den wichtigsten Tresor selbst beeinflussen die Ebene Management besteht aus. Sie können beispielsweise erstellen oder löschen eine Key Tresor. Sie können eine Liste der +++ in einem Abonnement erhalten. Sie können abrufen Key Tresor Eigenschaften (z. B. SKU, Tags), und legen Key Tresor-Richtlinien, die steuern, die Benutzer und Anwendungen, die Tasten und vertrauliche Informationen in die wichtigsten Tresor zugreifen können. Management Ebene Access Steuerelement verwendet RBAC. Finden Sie die vollständige Liste der wichtigsten Tresor Vorgänge, die über Management Ebene in der Tabelle im vorhergehenden Abschnitt ausgeführt werden können. 

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Access Control (RBAC)
Jede Azure Abonnement umfasst eine Azure-Active Directory. Benutzer, Gruppen und Applications aus diesem Verzeichnis können gewährt Zugriff Ressourcen im Azure-Abonnement zu verwalten, die das Modell zur Bereitstellung von Azure Ressourcenmanager verwenden. Diese Art von Access-Steuerelement wird als rollenbasierte Access Control (RBAC) bezeichnet. Zum Verwalten von in Access können Sie das [Azure-Portal](https://portal.azure.com/), die [CLI Azure-Tools](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)oder der [Azure Ressourcenmanager REST-APIs](https://msdn.microsoft.com/library/azure/dn906885.aspx)verwenden.

Erstellen Sie mit dem Modell Azure Ressourcenmanager den Key Tresor in einer Ressource gruppieren und Steuerelement Zugriff zur Verwaltungsebene des diesen Key Tresor mithilfe von Azure Active Directory auf. Beispielsweise können Sie Benutzer oder eine Gruppe Möglichkeit zum Verwalten von Key Depots in einer bestimmten Ressourcengruppe erteilen.

Sie können für Benutzer, Gruppen und Anwendungen mit einem bestimmten Bereich Zugriff gewähren, indem Sie entsprechende RBAC-Rollen zuweisen. Wenn Sie einem Benutzer zum Verwalten von Key Depots Zugriff gewähren möchten Sie beispielsweise eine vordefinierte Rolle 'Key Tresor Mitwirkender' an diesen Benutzer mit einem bestimmten Bereich zuweisen. Sie können der Umfang wäre in diesem Fall ein Abonnement, eine Ressourcengruppe oder nur eine bestimmte Key Tresor. Eine Rolle zugewiesen Ebene Abonnement gilt für alle Ressourcengruppen und Ressourcen innerhalb dieses Abonnement. Eine Rolle auf Ressource Gruppenebene zugewiesen gilt für alle Ressourcen in dieser Ressourcengruppe. Eine Rolle für eine bestimmte Ressource zugeordneten gilt nur für diese Ressource. Es gibt mehrere vordefinierte Rollen (finden Sie unter [RBAC: Standardrollen](../active-directory/role-based-access-built-in-roles.md)), und wenn die vordefinierten Rollen nicht Ihren Anforderungen passen Sie können Sie auch eigene Rollen definieren.

>[AZURE.IMPORTANT] Beachten Sie, dass wenn ein Benutzer weist Mitwirkender (RBAC) zu einer Key Tresor Management Ebene Anna kann sich selbst zu erteilen, den Zugriff auf Daten Ebene, durch Festlegen von Key Tresor Zugriffsrichtlinie, die Zugriff auf Daten Ebene steuert. Daher empfiehlt es genau steuern, wer 'Mitwirkender' Zugriff auf Ihre Key Depots hat, um sicherzustellen, dass nur autorisierte Personen zugreifen und diese Verwalten Ihrer Key Depots, Schlüssel, Kennwörter und Zertifikate.

## <a name="data-plane-access-control"></a>Ebene Access-Steuerelement

Die wichtigsten Tresor Daten Ebene besteht aus Vorgänge, die Einfluss auf die Objekte in einem Key Tresor, wie Schlüssel, Kennwörter und Zertifikate.  Schlüssel, wie Vorgänge erstellen, importieren, aktualisieren, Liste, Sichern und Wiederherstellen-Taste cryptographic JOIN-Operationen wie Zeichen umfasst, vergewissern Sie sich, verschlüsseln, entschlüsseln, umgebrochen wird, entpacken und Tags und andere Attribute für Schlüssel festlegen. Auf ähnliche Weise für vertrauliche Daten, die darin enthaltenen, abrufen, festlegen Sie, Liste, und löschen.

Zugreifen auf Ebene der Daten wird durch das Festlegen von Richtlinien für eine Key Tresor gewährt. Ein Benutzer, Gruppe oder einer Anwendung müssen Teilnehmerberechtigungen (RBAC) für Management-Ebene eines Key Tresor Access-Richtlinien für die wichtigsten Tresor festlegen können. Benutzern, einer Gruppe oder einer Anwendung kann bestimmte Vorgänge für Schlüssel oder Kennwörter in einem Key Tresor ausführen Zugriff erteilt werden. Key Tresor unterstützt bis zu 16 Einträge der Access-Richtlinie für eine Key Tresor. Erstellen Sie eine Sicherheitsgruppe Azure Active Directory und Hinzufügen von Benutzern zu dieser Gruppe mehrere Benutzer zu einer Key Tresor Daten Ebene Zugriff gewähren.

### <a name="key-vault-access-policies"></a>Key Tresor-Richtlinien

Key Tresor-Richtlinien Erteilen von Berechtigungen zur Schlüssel, Kennwörter und Zertifikate getrennt. Beispielsweise können Sie einem Benutzerzugriff auf nur Schlüssel, aber keine Berechtigungen für vertrauliche Daten verleihen. Sind jedoch die Berechtigungen für den Schlüssel oder Kennwörter oder Zertifikate Zugriff auf Ebene der Tresor. Key Tresor Zugriffsrichtlinie also unterstützt Berechtigungen auf Objektebene nicht. [Azure-Portal](https://portal.azure.com/), die [CLI Azure-Tools](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)oder der [wichtigsten Tresor Management REST-APIs](https://msdn.microsoft.com/library/azure/mt620024.aspx) können Sie die Access-Richtlinien für eine Key Tresor festlegen.

>[AZURE.IMPORTANT] Beachten Sie, dass Key Tresor-Richtlinien auf der Ebene Tresor anwenden. Wenn ein Benutzer über die Berechtigung zum Erstellen und Löschen von Tasten gewährt wird, kann Anna beispielsweise Vorgängen auf alle Schlüssel in die wichtigsten Tresor ausführen.

## <a name="example"></a>Beispiel

Angenommen, Sie entwickeln eine Anwendung, die ein Zertifikat für SSL, Azure-Speicher zum Speichern von Daten, und verwendet einen RSA 2048-Bit-Schlüssel für Vorgänge anmelden. Angenommen, diese Anwendung eines virtuellen Computers (oder virtuellen Computer Skalierung festlegen) ausgeführt wird. Sie können eine Key Tresor verwenden, um alle vertraulichen Daten zu speichern und Key Tresor bootstrap Zertifikat gespeichert, das von der Anwendung verwendet wird, die Authentifizierung mit Azure Active Directory verwenden.

Ja, hier eine Zusammenfassung aller Tasten und vertraulichen Daten in einem Key Tresor gespeichert werden soll.

- **SSL-Zertifikat** - verwendet für SSL
- **Storage Key** - verwendet, um bei Speicher-Konto zugreifen können.
- **RSA 2048-Bit-Schlüssel** – für Vorgänge Signieren nicht verwendet
- **Bootstrap Zertifikat** - zur Authentifizierung von Azure Active Directory, um den Zugriff auf wichtige Tresor die Taste Speicher abgerufen werden sollen, und verwenden den RSA-Schlüssel zum Signieren verwendet.

Jetzt treffen wir uns auf die Personen, die verwalten, bereitstellen und Überwachung dieser Anwendung. In diesem Beispiel verwenden wir drei Rollen.

- **Sicherheitsteam** - Hierbei handelt es sich in der Regel aus 'Office von der CSO (Leiter Sicherheitsbeauftragter)' IT-Personal oder entspricht, verantwortlich gemischte Sicherheitsgründen der Kennwörter wie SSL-Zertifikate, RSA-Schlüssel zum Signieren, Verbindungszeichenfolgen für Datenbanken, Schlüssel für Speicher-Konto verwendet.
- **Entwickler/Operatoren** - Hierbei handelt es sich um die Leute, mit denen Entwickeln dieser Anwendungs, und klicken Sie dann in Azure bereitstellen. Sie sind nicht Teil des Teams und damit sollten nicht haben sie Zugriff auf alle vertraulichen Daten, wie z. B. SSL-Zertifikate, RSA-Schlüssel normalerweise die Anwendung, die sie bereitstellen Zugriff auf die haben sollen.
- **Prüfer** – Dies ist in der Regel eine andere Gruppe von Personen, die aus dem Entwickler und allgemeine IT-Personal isoliert. Deren Zuständigkeit ist zum Überprüfen der richtigen Verwendung und Wartung Zertifikate, Schlüssel usw. und Einhaltung Sicherheitsstandards Daten zu gewährleisten. 

Es gibt eine weitere Rolle, die außerhalb des Gültigkeitsbereichs von dieser Anwendung, sondern nur relevante hier angegeben werden, und die wäre das Abonnement (oder die Ressourcengruppe) Administrator. Abonnement Standortadministrator legt fest anfängliche Zugriffsberechtigungen für das Sicherheitsteam. Hier wird davon ausgegangen, dass der Abonnementadministrator Zugriff auf das Sicherheitsteam zu einer Ressourcengruppe, in der alle der für diese Anwendung wohnen benötigten Ressourcen erteilt hat.

Nun sehen, welche Aktionen jede Rolle im Zusammenhang mit dieser Anwendung ausführt.

- **Sicherheitsteam**
    - Erstellen von Key Depots
    - Schaltet die Taste Vaulting Protokollierung
    - Hinzufügen von Tasten/Schlüssel
    - Der Schlüssel für die Wiederherstellung Sicherungsdatei erstellen
    - Festlegen von Key Tresor Zugriffsrichtlinie Erteilen von Berechtigungen für Anwender und Applications zum Ausführen von bestimmter Vorgängen
    - Regelmäßig einsatzbereit Tasten/Schlüssel
- **Entwickler/Operatoren**
    - Abrufen von verweisen, um zu starten und SSL-Zertifikate (Fingerabdrücke), Speicherschlüssel (geheim URI) und bei der Anmeldung Sicherheitsteam Keys (URI-Taste)
    - Entwickeln Sie und Bereitstellen Sie der Anwendung, die Tasten und Kennwörter greift programmgesteuert auf
- **Auditoren**
    - Überprüfen Verwendungsprotokolle zur Bestätigung der richtigen Schlüssel/geheim Verwendung und Einhaltung Sicherheitsstandards Daten

Nun sehen, welche Zugriffsberechtigungen für Key Tresor durch jede Rolle (und der Anwendung), die Ihnen zugewiesenen Aufgaben ausführen erforderlich sind. 

| Benutzerrolle    | Berechtigungen für Management Ebene | Daten Ebene Berechtigungen |
|--------------|------------------------------|------------------------|
|Sicherheitsteam|Key Tresor Mitwirkender|Tasten: Sichern, erstellen, löschen, erhalten, importieren, list und Wiederherstellen <br> Kennwörter: alle|
|Entwickler/Operator| Key Tresor Berechtigung bereitstellen, damit die virtuellen Computern, die sie bereitstellen geheimen Daten aus dem Key Tresor abgerufen werden können | Keine |
|Auditoren| Keine | Tasten: Liste<br>Kennwörter: Liste|
|Anwendung| Keine | Tasten: anmelden<br>Kennwörter: Abrufen |

>[AZURE.NOTE] Auditoren müssen über die Berechtigung für Schlüssel und Kennwörter Liste, damit sie prüfen können Attribute für Schlüssel und Kennwörter, die nicht in die Protokolle, wie etwa Kategorien, ausgegeben sind Aktivierung und das Ablaufdatum.

Zusätzlich zu Berechtigungen für Key Tresor benötigen alle drei Rollen auch Zugriff auf Weitere Ressourcen. Beispielsweise werden sollen, Bereitstellen von virtuellen Computern (oder Web Apps usw.). Entwickler/Operatoren benötigen ferner 'Mitwirkender' Zugriff auf diese Ressourcentypen zur Verfügung. Auditoren benötigen Lesezugriff auf das Speicherkonto, in dem die Protokolle Key Tresor gespeichert sind.

Da der Fokus in diesem Artikel Zugriff auf Ihre Key Tresor Schutz ist, die relevanten Abschnitte, die in diesem Thema betreffen veranschaulichen wir nur an und Details zum Bereitstellen von Zertifikaten, den Zugriff auf Schlüssel und Kennwörter programmgesteuert usw. überspringen. Diese Details sind bereits an anderer Stelle abgedeckt. Bereitstellen von Zertifikaten in Key Tresor zu virtuellen Computern gespeichert wird in einem [Blogbeitrag](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)behandelt, und es ist [Stichprobe Code](https://www.microsoft.com/download/details.aspx?id=45343) zur Verfügung, das veranschaulicht, wie Sie bootstrap Zertifikat zum Authentifizieren Azure AD Key Tresor zugreifen können.

Die meisten Zugriffsberechtigungen mit Azure-Portal erteilt werden können, um genau abgestimmte Berechtigungen erteilen möglicherweise müssen Sie jedoch mit Azure PowerShell (oder Azure CLI), um das gewünschte Ergebnis erzielen. 

Nehmen Sie die folgenden PowerShell Codeausschnitte an:

- Der Azure-Active Directory-Administrator hat Sicherheitsgruppen erstellt, die die drei Rollen, nämlich Contoso-Team Sicherheit, Contoso-App-Devops, Contoso-App Auditors darstellen. Der Administrator hat auch Benutzer den Gruppen hinzugefügt, die sie gehören.

- **ContosoAppRG** ist die Ressourcengruppe, in denen alle Ressourcen befinden. **Contosologstorage** ist, in dem die Protokolle gespeichert werden. 

- Key Tresor **ContosoKeyVault** und Speicher Konto für Key Tresor Protokolle **Contosologstorage** muss an derselben Stelle Azure


Der Abonnementadministrator weist 'wichtiger Tresor Mitwirkender' und ' Benutzer Access' Administratorrollen zuerst an das Sicherheitsteam. Dadurch wird das Sicherheitsteam zum Verwalten des Zugriffs auf andere Ressourcen und Verwalten von Key Depots in der Ressourcengruppe ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Das folgende Skript veranschaulicht, wie das Sicherheitsteam eines Key Tresor, Setup Protokollierung, und legen Sie die Zugriffsberechtigungen für andere Rollen und die Anwendung erstellen kann. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Die benutzerdefinierte Rolle definiert ist, wird nur das Abonnement zugeordnet werden, wo die ContosoAppRG Ressourcengruppe erstellt haben. Wenn die gleichen benutzerdefinierten Rollen für andere Projekte in anderen Abonnements verwendet werden soll, konnte deren Bereich weitere Abonnements hinzugefügt haben.

Die benutzerdefinierten rollenzuweisung für die Entwickler/Operatoren für die Berechtigung "Bereitstellen/Aktion" wird der Ressourcengruppe beschränkt. Auf diese Weise nur die virtuellen Computern erstellt, in der Ressourcengruppe 'ContosoAppRG' wird das Geheimnis (SSL-Zertifikat und bootstrap Zertifikat) angezeigt. Alle virtuellen Computern, die Mitglied des Teams Entwickler/Ops in andere Ressourcengruppe erstellt werden nicht keine geheimen Schlüssel abrufen, auch wenn sie die geheimen URIs Gewusst hat.

In diesem Beispiel wird ein einfaches Szenario dargestellt. Praxis Szenarien möglicherweise komplexere und möglicherweise müssen Sie die Berechtigungen, um den Key Tresor entsprechend Ihren Anforderungen anpassen. In diesem Beispiel, wird angenommen, das Sicherheitsteam werden, geben die Taste und geheimen Bezüge (URIs und Fingerabdrücke) diesem Team Entwickler/Operatoren in deren Clientanwendungen verwiesen werden müssen. Daher brauchen sie Entwickler/Operatoren alle Daten Ebene Zugriff gewähren. Beachten Sie, dass in diesem Beispiel liegt der Schwerpunkt auf Sichern von Ihrem Key Tresor. Ähnliche Aspekte sollten [Ihre virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/security/), [Speicherkonten](../storage/storage-security-guide.md) und anderen Ressourcen Azure ebenfalls gesichert angegeben sein.

>[AZURE.NOTE] Hinweis: In diesem Beispiel wird gezeigt, wie Key Tresor Access wird in der Herstellung gesperrt. Der Entwickler sollte aufweisen, eigene Abonnements oder Resourcegroup diese vollständige Berechtigungen zum Verwalten ihrer Depots, virtuellen Computern und Speicher-Konto, in dem sie die Anwendung entwickeln.


## <a name="resources"></a>Ressourcen

-   [Rollenbasierte Access-Azure-Active Directory-Steuerelements](../active-directory/role-based-access-control-configure.md)

    In diesem Artikel wird erläutert, die Access-Control Azure Active Directory rollenbasierte und deren Funktionsweise.

-   [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md)

    In diesem Artikel werden alle integrierten Rollen in RBAC verfügbar.

-   [Grundlegendes zu Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md)

    In diesem Artikel wird erläutert, die Bereitstellung Ressourcenmanager und klassischen Bereitstellungsmodelle und erläutert die Vorteile von den Gruppen Ressourcenmanager und Ressourcen

-    [Verwalten von Access rollenbasierte Steuerelement mit Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)

     In diesem Artikel wird erläutert, wie rollenbasierte Access-Steuerelement mit Azure PowerShell verwalten

-   [Verwalten von Access rollenbasierte Steuerelements in die REST-API](../active-directory/role-based-access-control-manage-access-rest.md)

    In diesem Artikel veranschaulicht, wie die REST-API RBAC verwalten.

-   [Rollenbasierte Access-Steuerelement für Microsoft Azure aus Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dies ist ein Link zu einem Video auf Channel 9 aus der 2015 MS Ignite Konferenz. In dieser Sitzung Diskussion zu Verwaltungs- und reporting-Funktionen in Azure Zugriff auf Dateien und bewährte Verfahren für Sichern des Zugriffs auf Azure mit Azure Active Directory-Abonnements durchsuchen.

-   [Autorisierung des Zugriffs auf OAuth 2.0 und Azure Active Directory mithilfe von Webanwendungen](../active-directory/active-directory-protocols-oauth-code.md)

    In diesem Artikel werden die gesamten OAuth 2.0 Ablauf authentifizieren mit Azure Active Directory.

-   [Key Tresor Management REST-APIs](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Dieses Dokument ist der Bezug für die restlichen APIs zum Verwalten von Ihrem Key Tresor programmgesteuert, einschließlich Zugriffsrichtlinie Key Tresor festlegen.

-   [Key Tresor REST-APIs](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Link zur Key Tresor Dokumentation zur REST-API.

-   [Key Access-Steuerelement](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Link zur Dokumentation der Schlüssel Access Steuerelement.

-   [Geheimnis Access-Steuerelement](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Link zur Dokumentation der Schlüssel Access Steuerelement.

-   [Festlegen](https://msdn.microsoft.com/library/mt603625.aspx) und [Entfernen von](https://msdn.microsoft.com/library/mt619427.aspx) Key Tresor Zugriffsrichtlinie ab, die mithilfe der PowerShell

    Enthält Links zu Dokumentation für PowerShell-Cmdlets zum Verwalten von Key Tresor Zugriffsrichtlinie verweisen.

## <a name="next-steps"></a>Nächste Schritte

Ein beim Abrufen Schritte für einen Administrator finden Sie unter [Erste Schritte mit Azure Key Tresor](key-vault-get-started.md).

Weitere Informationen zur Verwendung des für Key Tresor Protokollierung finden Sie unter [Azure Key Tresor Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Tasten und Kennwörter mit Azure Key Tresor finden Sie unter [Informationen zum Schlüssel und Kennwörter](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Wenn Sie Fragen zu den wichtigsten Tresor haben, besuchen Sie die [Azure Key Tresor Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

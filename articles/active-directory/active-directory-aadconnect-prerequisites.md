<properties
   pageTitle="Azure AD-verbinden: Voraussetzungen und Hardware | Microsoft Azure"
   description="In diesem Thema werden die erforderlichen Komponenten und den hardwareanforderungen für Azure AD-verbinden"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Erforderliche Komponenten für Azure AD verbinden
In diesem Thema werden die erforderlichen Komponenten und den hardwareanforderungen für Azure AD verbinden.

## <a name="before-you-install-azure-ad-connect"></a>Vor der Installation von Azure AD-verbinden
Vor der Installation von Azure AD verbinden, gibt es ein paar Punkte, die Sie benötigen.

### <a name="azure-ad"></a>Azure AD
- Ein Azure-Abonnements oder von einem [Testabonnement Azure](https://azure.microsoft.com/pricing/free-trial/). Diese Option ist nur für den Zugriff auf das Portal Azure und nicht für die Verwendung von Azure AD verbinden erforderlich. Wenn Sie PowerShell oder Office 365 verwenden, benötigen Sie kein Azure-Abonnement Azure AD verbinden verwenden. Wenn Sie eine Office 365-Lizenz verfügen, können Sie auch im Office 365-Portal. Mit einer bezahltes Office 365-Lizenz können Sie auch Azure-Portal vom Office 365-Portal zu verschaffen.
    - Sie können auch die Funktionalität der Azure AD-Vorschau im [Portal Azure](https://portal.azure.com)verwenden. Dieses Portal ist nicht mit eine Azure Lizenz erforderlich.
- [Hinzufügen und überprüfen die Domäne](active-directory-add-domain.md) Sie in Azure AD verwenden möchten. Beispielsweise wenn Sie beabsichtigen, verwenden "contoso.com" für die Benutzer aus, und stellen Sie sicher diese Domäne überprüft wurde, und Sie werden nicht nur die Standarddomäne contoso.onmicrosoft.com verwenden.
- Ein Azure AD-Mandanten kann von Standardobjekten 50k. Wenn Sie Ihre Domäne überprüft haben, wird die Beschränkung auf 300 k Objekte erhöht. Wenn Sie noch mehr Objekte in Azure AD benötigen, die Sie eine Supportanfrage, damit die Beschränkung erhöht noch feiner zu öffnen müssen. Wenn Sie mehr als 500 k Objekte benötigen benötigen Sie eine Lizenz, wie Office 365, Azure AD grundlegende, Azure AD Premium oder Enterprise Mobilität Suite.

### <a name="prepare-your-on-premises-data"></a>Bereiten Sie Ihrer lokalen Daten vor
- Überprüfen Sie [optional synchronisieren-Features, die Sie in Azure AD aktivieren können](active-directory-aadconnectsyncservice-features.md) , und welche Features, die Sie aktivieren sollten auswerten.

### <a name="on-premises-servers-and-environment"></a>Lokalen Servern und Ihrer Umgebung
- Die AD-Schema Version Funktionsebenen muss WindowsServer 2003 oder höher. Die Domänencontroller können mit jeder Version ausgeführt werden, solange die Schema und Gesamtstruktur Ebenen Anforderungen erfüllt sind.
- Wenn Sie beabsichtigen, verwenden Sie das Feature **Kennwort abgeschlossenen writebackvorgängen** muss die Domänencontroller unter Windows Server 2008 (mit dem aktuellsten SP) oder höher. Wenn Ihre DCs auf 2008 (Pre-R2) sind müssen Sie auch [Update KB2386717](http://support.microsoft.com/kb/2386717)anwenden.
- Der Domänencontroller Azure AD untersuchten darf nicht schreibgeschützt sein. Es wird nicht unterstützt, um eine RODC (schreibgeschützt Domain Controller) verwenden und Azure AD verbinden werden keine schreiben leitet befolgt.
- Verbinden von Azure AD kann nicht auf Small Business Server oder Windows Server Essentials installiert sein. Der Server muss Windows Server standard oder höher verwenden.
- Der Azure AD verbinden-Server muss eine vollständige Benutzeroberfläche installiert sein. Es wird nicht unterstützt, um auf Server Core zu installieren.
- Verbinden von Azure AD muss unter Windows Server 2008 oder höher installiert sein. Dieser Server einer Domänencontroller oder einem Mitgliedsserver möglicherweise bei Verwendung von express-Einstellungen. Wenn Sie benutzerdefinierte Einstellungen verwenden, wird der Server kann auch eigenständig sein und muss nicht mit einer Domäne verbunden sein.
- Wenn Sie Azure AD Verbinden unter Windows Server 2008 installiert haben, stellen Sie sicher, um die neuesten Updates von Windows Update anzuwenden. Die Installation ist nicht mit einem Server ohne Patch starten.
- Wenn Sie das Feature **Synchronisierung von Kennwörtern**verwenden möchten, muss der Server Azure AD Verbinden unter Windows Server 2008 R2 SP1 oder höher.
- Der Server Azure AD-Verbindung herstellen müssen [.NET Framework 4.5.1](#component-prerequisites) oder höher und [Microsoft PowerShell 3.0](#component-prerequisites) oder höher installiert sein.
- Falls Active Directory Federation Services bereitgestellt wird, muss die Server, auf dem AD FS oder des Proxyservers für Web-Anwendung installiert werden, Windows Server 2012 R2 oder höher. [Windows remote Management](#windows-remote-management) muss auf den folgenden Servern für den remote-Installation aktiviert sein.
- Falls Active Directory Federation Services bereitgestellt wird, benötigen Sie [SSL-Zertifikate](#ssl-certificate-requirements).
- Falls Active Directory Federation Services bereitgestellt wird, müssen Sie [mit einer namensauflösung von](#name-resolution-for-federation-servers)zu konfigurieren.
- Azure AD verbinden erfordert eine SQL Server-Datenbank zum Speichern von Identitätsdaten. Standardmäßig wird eine SQL Server 2012 Express-LocalDB (eine light-Version von SQL Server Express) installiert und des Dienstkontos für den Dienst wird auf dem lokalen Computer erstellt. SQL Server Express sind 10GB Größe maximal, die Sie zum Verwalten von ungefähr 100.000 Objekten ermöglicht. Wenn Sie eine höhere Lautstärke Directory-Objekte verwalten müssen, müssen Sie den Assistenten zum Installieren einer anderen Installation von SQL Server verweisen.
- Wenn Sie einem separaten SQL Server verwenden, gelten diese Anforderungen:
    - Verbinden von Azure AD unterstützt alle Arten von Microsoft SQL Server von SQL Server 2008 (mit Service Pack 4), um SQL Server 2014. Microsoft Azure SQL-Datenbank wird als Datenbank **nicht unterstützt** .
    - Sie müssen eine SQL-Sortierung Groß-und Kleinschreibung verwenden. Diese identifiziert werden, mit einer \_CI_ in ihren Namen. Es ist **nicht unterstützt** , mit einer Groß-/Kleinschreibung beachtet Sortierung, identifizierten \_CS_ in ihren Namen.
    - Sie können nur eine Synchronisierung-Engine pro Datenbankinstanz haben. Es ist **nicht unterstützt** , die Datenbankinstanz für FIM/MIM synchronisieren, DirSync oder Azure AD synchronisieren freizugeben.

### <a name="accounts"></a>Konten
- Ein Azure AD globaler Administrator-Konto für Verzeichnis Azure AD-mit integrieren möchten. Dies muss eine **Schule oder Organisation Konto** und kein **Microsoft-Konto**sein.
- Wenn Sie express-Einstellungen verwenden oder von DirSync aktualisieren, müssen Sie ein Konto Unternehmensadministrator für Ihre lokalen Active Directory verfügen.
- [Konten in Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) , wenn Sie den Pfad der benutzerdefinierten Einstellungen Installation verwenden.

### <a name="azure-ad-connect-server-configuration"></a>Azure AD verbinden Server-Konfiguration
- Wenn Ihre globalen Administratoren MFA aktiviert haben, muss der URL **https://secure.aadcdn.microsoftonline-p.com** in der Liste der vertrauenswürdigen Websites sein. Sie werden aufgefordert, dies für die Liste der vertrauenswürdigen Websites hinzufügen, wenn er nicht hinzugefügt wird, bevor Sie eine Herausforderung MFA dazu aufgefordert werden. Internet Explorer können Sie es Ihren vertrauenswürdigen Websites hinzufügen.

### <a name="connectivity"></a>Konnektivität
- Der Azure AD verbinden-Server benötigt DNS-Auflösung für Intranet- und Internet. Der DNS-Server muss Namen sowohl auf Ihrem lokalen Active Directory als auch die Endpunkte Azure AD-auflösen können.
- Wenn Sie Firewalls auf Ihr Intranet und Sie zwischen den Servern Azure AD verbinden und Ihre Domänencontroller Ports zu öffnen müssen, klicken Sie dann [Azure AD verbinden Ports](active-directory-aadconnect-ports.md) Weitere Informationen finden Sie.
- Wenn es sich bei Ihrem Proxy oder die Firewall Grenzwert für die URLs zugegriffen werden kann, klicken Sie dann die URLs, die in [Office 365-URLs und IP-Adresse Bereiche](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) dokumentierten geöffnet werden muss.
    - Wenn Sie die Microsoft-Cloud in Deutschland oder der Cloud Microsoft Azure Government verwenden, klicken Sie dann finden Sie unter [Azure AD verbinden synchronisieren Dienst Instanzen Aspekte](active-directory-aadconnect-instances.md) URLs.
- Verbinden von Azure AD ist standardmäßig TLS 1.0 zur Kommunikation mit Azure AD verwenden. Sie können dies auf TLS 1.2 ändern, gemäß die Anweisungen in [Aktivieren TLS 1.2 für Azure AD verbinden](#enable-tls-12-for-azure-ad-connect).
- Wenn Sie einen ausgehenden Proxy zum Herstellen einer Verbindung mit dem Internet verwenden, muss die folgende Einstellung in der Datei **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** für den Assistenten zum Installieren von und Azure AD verbinden synchronisieren, kann die Verbindung zum Internet und Azure AD-hinzugefügt werden. Am Ende der Datei muss diesen Text eingegeben werden. In diesem Code &lt;PROXYADRESS&gt; den tatsächlichen Proxy-IP-Adresse oder Hostnamen Namen darstellt.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Wenn der Proxyserver-Authentifizierung erforderlich ist, das [Dienstkonto](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) muss sich in der Domäne befinden, und Sie müssen des Installationspfads benutzerdefinierten Einstellungen verwenden, um ein [benutzerdefiniertes Dienstkonto](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components)angeben. Sie benötigen ferner eine andere Änderung "Machine.config". Antworten mit dieser Änderung in "Machine.config" den Assistenten zum Installieren und synchronisieren-Engine zu Authentifizierung Besprechungsanfragen aus dem Proxyserver. Im Assistenten zum Installieren von alle werden Seiten, Ausschließen der Seite **Konfigurieren** , die bei der Anmeldeinformationen des Benutzers verwendet. Auf der Seite am Ende des Assistenten **Konfigurieren** wird im Kontext mit dem [Dienstkontos](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) gewechselt werden, die von Ihnen erstellt wurde. Abschnitt "Machine.config" sollte wie folgt aussehen.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Weitere Informationen zu den [standardmäßigen Proxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx)finden Sie unter MSDN.

Wenn Sie Probleme mit der Konnektivität haben, finden Sie unter [Behandeln von Verbindungsproblemen](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Andere
- Optional: Ein Test-Benutzerkonto zum Überprüfen der Synchronisierung.

## <a name="component-prerequisites"></a>Voraussetzungen für die Komponente

### <a name="powershell-and-net-framework"></a>PowerShell und .net Framework
Verbinden von Azure AD hängt davon ab, Microsoft PowerShell und .NET Framework 4.5.1. Sie benötigen diese Version oder eine höhere Version auf dem Server installiert. Abhängig von Ihrer Version des Windows Server führen Sie folgende Schritte aus:

- Windows Server 2012R2
  - Microsoft PowerShell standardmäßig installiert ist, ist keine Maßnahme erforderlich.
  - .NET Framework 4.5.1 und spätere Versionen sind über Windows Update angeboten. Stellen Sie sicher, dass Sie die neuesten Updates in Windows Server in der Systemsteuerung installiert haben.
- Windows Server 2008 R2 und WindowsServer 2012
  - Die neueste Version von Microsoft PowerShell steht in **Windows Management Framework 4.0**, auf [Microsoft Download Center](http://www.microsoft.com/downloads)zur Verfügung.
  - .NET Framework 4.5.1 und spätere Versionen sind [Microsoft Download](http://www.microsoft.com/downloads)Center verfügbar.
- WindowsServer 2008
  - Die neueste unterstützte Version der PowerShell steht in **Windows Management Framework 3.0**, [Microsoft Download](http://www.microsoft.com/downloads)Center zur Verfügung.
 - .NET Framework 4.5.1 und spätere Versionen sind [Microsoft Download](http://www.microsoft.com/downloads)Center verfügbar.

### <a name="enable-tls-12-for-azure-ad-connect"></a>Aktivieren Sie TLS 1.2 für Azure AD verbinden
Verbinden von Azure AD stellt TLS 1.0 standardmäßig für die Verschlüsselung der Kommunikation zwischen synchronisieren-Engine-Server und Azure AD-dar. Sie können dies ändern, indem Sie .net Applications mit TLS 1.2 standardmäßig auf dem Server zu konfigurieren. Weitere Informationen zu TLS 1.2 finden Sie im [Microsoft Security Ihren 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. Unter Windows Server 2008 kann TLS 1.2 aktiviert werden. Sie benötigen Windows Server 2008 R2 oder höher. Stellen Sie sicher, dass Sie das .net 4.5.1 Update für Ihr Betriebssystem installiert haben, finden Sie unter [Microsoft Security Ihren 2960358](https://technet.microsoft.com/security/advisory/2960358). Möglicherweise müssen diese oder einer späteren Version, die bereits auf dem Server installiert ist.
2. Wenn Sie Windows Server 2008 R2 verwenden, stellen Sie sicher, dass TLS 1.2 aktiviert ist. Klicken Sie auf Windows Server 2012-Server und höher sollte TLS 1.2 bereits aktiviert sein.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Festlegen Sie bei allen Betriebssystemen diesen Registrierungsschlüssel, und starten Sie den Server.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Wenn Sie TLS 1.2 zwischen synchronisieren-Engine-Server und einer SQL Server-Remotecomputer aktivieren möchten, stellen Sie sicher, dass Sie die erforderlichen Versionen für [TLS 1.2-Unterstützung für Microsoft SQL Server](https://support.microsoft.com/kb/3135244)installiert haben.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Erforderliche Komponenten für Föderation Installation und Konfiguration

### <a name="windows-remote-management"></a>Windows Remote Management
Wenn Azure AD-Verbindung mit Active Directory Federation Services oder des Proxyservers der Web-Anwendung bereitstellen, überprüfen Sie die nachfolgenden Anforderungen, um sicherzustellen, dass der Konnektivität und Konfiguration erfolgreich durchgeführt werden können:

- Wenn der Ziel-Server Domäne verbunden ist, stellen Sie sicher, dass Windows Remote verwaltete aktiviert ist
    - Verwenden Sie in einem erhöhten PSH-Befehlsfenster Befehl`Enable-PSRemoting –force`
- Ist der Ziel-Server ein nicht-Domäne hinzugefügten WAP Computer, gibt es ein paar weitere Anforderungen
    - Klicken Sie auf dem Zielcomputer (WAP Computer):
         - Vergewissern Sie sich die Winrm (Windows Remote Management / WS-Management) Dienst ausgeführt wird, über das Services-Snap-in
         - Verwenden Sie in einem erhöhten PSH-Befehlsfenster Befehl`Enable-PSRemoting –force`
    - Auf dem Computer, auf dem der Assistent ausgeführt wird (wenn der Ziel-Computer außerhalb von Domänen verknüpfte oder nicht vertrauenswürdigen Domäne ist):
        - Verwenden Sie den Befehl in einem erhöhten PSH-Befehl-Fenster`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - Im Server-Manager:
             - DMZ WAP Host zur maschinellen Ressourcenpool hinzufügen (Server-Manager-verwalten >...-Servern hinzufügen > Verwenden Sie die Registerkarte DNS)
             - Registerkarte Server-Manager alle Server: Klicken Sie mit der rechten Maustaste auf WAP-Server, und wählen Sie verwalten As..., geben Sie die Anmeldeinformationen für lokale (nicht Domäne) für den Computer WAP
             - Remote Connectivity PSH-, auf der Registerkarte Server-Manager alle Servern überprüft: Klicken Sie mit der rechten Maustaste auf WAP-Server, und wählen Sie Windows PowerShell aus.  Eine remote PSH-Sitzung sollten öffnen, um sicherzustellen, dass remote PowerShell-Sitzungen hergestellt werden können.

### <a name="ssl-certificate-requirements"></a>SSL Zertifikat Anforderungen
**Wichtig:** es wurde dringend empfohlen, das gleiche Zertifikat für SSL über alle Knoten Ihrer AD FS-Farm als auch alle Webanwendung Proxy-Servern verwendet.

- Das Zertifikat muss ein X509 Zertifikat.
- Sie können ein selbst signiertes Zertifikat auf Föderation Servern in einer Umgebung für Übung verwenden. Für eine Umgebung Herstellung wird jedoch empfohlen, dass Sie das Zertifikat von einer öffentlichen Zertifizierungsstelle beziehen.
    - Wenn Sie ein Zertifikat verwenden, die nicht öffentlich vertrauenswürdig ist, stellen Sie sicher, dass das Zertifikat auf allen Web-Anwendungsproxy-Servern installiert vertrauenswürdigen sowohl auf dem lokalen Server, und klicken Sie auf alle Föderation Server
- Die Identität des Zertifikats muss der Name des Dienstes Föderation (z. B. sts.contoso.com) übereinstimmen.
    - Die Identität ist entweder eine Betreff alternativen Namen (SAN) Erweiterung des DNS-Typ Name oder, wenn keine SAN Einträge vorhanden sind, wird der Name des Betreff als allgemeine Namen angegeben.  
    - Mehrere SAN Einträge können in das Zertifikat vorhanden sein, sofern einer von ihnen der Name des Dienstes Föderation entspricht.
    - Wenn Sie planen, teilnehmen Arbeitsplatz zu verwenden, ist eine weitere SAN erforderlich, mit dem Wert **Enterpriseregistration.** gefolgt von des Suffixes Hauptbenutzer UPN (User Name) Ihrer Organisation, beispielsweise **enterpriseregistration.contoso.com**.
- Zertifikate basierend auf CryptoAPI nächsten Generation (CNG) Tasten und Speicherung Schlüssel Anbieter werden nicht unterstützt. Dies bedeutet, dass Sie eine Zertifikat basierend auf einem CSP (cryptographic Service Provider) und nicht KSP (Key Storage Provider) verwenden müssen.
- Platzhalter Zertifikate werden unterstützt.

### <a name="name-resolution-for-federation-servers"></a>Mit einer namensauflösung von für die Föderation
- Einrichten von DNS-Einträge für die AD FS Federation Service Name (z. B. sts.contoso.com) für sowohl im Intranet (dem internen DNS-Server) und den extranet (öffentlichen DNS über Ihre domänenregistrierungsstelle). Für das Intranet DNS Eintrag stellen Sie sicher, dass Sie A verwenden Datensätzen und nicht CNAME-Einträge. Dies ist erforderlich für Windows-Authentifizierung über Ihre Domäne hinzugefügten Computer ordnungsgemäß funktioniert.
- Wenn Sie mehr als eine AD FS-Server oder Web-Anwendung Proxyserver bereitstellen, müssen sicherstellen Sie, dass Sie Ihre Lastenausgleich konfiguriert haben und die DNS-Einträge für die AD FS Föderation Dienstnamen (beispielsweise sts.contoso.com) auf den Lastenausgleich verweisen.
- Für integrierte Windows-Authentifizierung für Browser Applikationen mithilfe von Internet Explorer in Ihr Intranet entwickelt stellen Sie sicher, dass der Name des AD FS Föderation Dienstes (beispielsweise sts.contoso.com) der Intranetzone in Internet Explorer hinzugefügt wird. Dies kann über Gruppenrichtlinien gesteuert und für alle Ihre Domäne verbundene Computer bereitgestellt werden.

## <a name="azure-ad-connect-supporting-components"></a>Unterstützen von Komponenten Azure AD-verbinden
Im folgenden finden eine Liste der Komponenten, die auf dem Server Azure AD verbinden installiert, Azure AD verbinden installiert ist. Diese Liste ist für eine einfache Expressinstallation.  Wenn Sie einen anderen SQL Server auf der Seite installieren Synchronisierung Dienste verwenden, ist SQL Express LocalDB nicht lokal installiert.

- Azure AD verbinden Dienststatus
- Microsoft Online Services-Anmeldeassistent für IT-Experten (jedoch keine Abhängigkeit daran installiert)
- Microsoft SQL Server 2012 Befehlszeilendienstprogramme
- Microsoft SQL Server 2012 Express LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 Verteilung Paket

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardwareanforderungen für Azure AD-verbinden
Die nachstehende Tabelle zeigt die Mindestanforderungen für Computer synchronisieren Azure AD verbinden.

| Anzahl von Objekten in Active Directory. | CPU | Arbeitsspeicher | Größe der Festplatte |
| ------------------------------------- | --- | ------ | --------------- |
| Weniger als 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10.000 – 50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50.000 – 100,000 | 1,6 GHz | 16 GB | 100 GB |
| Für mindestens 100.000 Objekte ist die Vollversion von SQL Server erforderlich|  |  |  |
| 100,000 bis 300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300.000 – 600.000 | 1,6 GHz | 32 GB | 450 GB |
| Mehr als 600.000 | 1,6 GHz | 32 GB | 500 GB |

Die Mindestanforderungen für AD FS oder Anwendungsserver Web-Computern sind folgende:

- CPU: Zwei core 1,6 GHz oder höher
- Arbeitsspeicher: 2GB oder höher
- Azure virtueller Computer: Konfiguration A2 oder höher

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

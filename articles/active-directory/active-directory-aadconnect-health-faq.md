<properties
    pageTitle="Azure AD verbinden Gesundheit häufig gestellte Fragen"
    description="Hier finden Sie Antworten auf Fragen zur Azure AD verbinden Dienststatus. Hier behandelt Fragen zu den Dienst verwenden, einschließlich des Modells, Funktionen, Einschränkungen und Support."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Verbinden von Azure AD Gesundheit häufig gestellte Fragen (FAQ)

Hier finden Sie Antworten auf Fragen zur Azure AD verbinden Dienststatus. Hier behandelt Fragen zu den Dienst verwenden, einschließlich des Modells, Funktionen, Einschränkungen und Support.

## <a name="general-questions"></a>Allgemeine Fragen



**F: Ich Verwalten mehrerer Azure AD-Verzeichnisse. Wie wechsle ich diejenige mit Azure Active Directory Premium?**

Sie können Wechseln zwischen verschiedenen Azure AD-Mandanten, indem Sie die aktuell signierte im Feld Benutzername auf der oberen rechten Ecke und das entsprechende Konto auswählen. Wenn das Konto hier nicht aufgelistet ist, wählen Sie Abmelden aus, und verwenden Sie dann die Anmeldeinformationen globaler Administrator des Verzeichnisses mit den Azure Active Directory Premium Anmelden aktiviert hat.

## <a name="installation-questions"></a>Fragen zur Installation



**F: Was ist die Auswirkung der Azure AD verbinden Health Agent auf einzelne Server zu installieren?**

Der Einfluss der Installation von der Microsoft Azure AD verbinden Health Agents ADFS, Web Anwendungsproxy-Servern, Azure AD verbinden (Sycn) Servern, Domain Controller ist minimal in Bezug auf die CPU, Arbeitsspeicher Verbrauch Bandbreite und Speicherplatz.

Die folgenden Zahlen sind eine Annäherung an.

- CPU-Auslastung: ~ 1 % vergrößern
- Belegt: bis zu 10 % des gesamten Systemspeichers

>[AZURE.NOTE] Im Falle der Agent werden nicht in Azure kommunizieren speichert der Agent Daten lokal speichern und auf eine Höchstgrenze. Der Agent überschreibt die "Cache" Daten auf Basis "mindestens kürzlich gewartet".

- Lokaler Pufferspeicher für Azure AD Health Agents verbinden: ~ 20 MB
- AD FS-Servern empfiehlt es sich, dass Sie Bereitstellen einer soviel Speicherplatz 1024 MB (1 GB) für den AD FS überwachenden Kanal für Azure AD-verbinden Health Agents, die Audit-Daten zu verarbeiten, bevor sie überschrieben wird.

**F: müssen ich meine Server während der Installation der Azure AD verbinden Health Agents neu starten?**

Nein. Die Installation der Agents müssen nicht zu, dass Sie den Server neu starten. Installation von einige vorbereitende Schritte möglicherweise jedoch einen Neustart des Servers erforderlich.

Beispielsweise ist unter Windows Server 2008 R2 die Installation von .net Framework 4.5 Neustart des Servers erforderlich.


**F: bedeutet Azure AD verbinden Health Services arbeiten über einen Pass-Through-http-Proxy?**

Ja.  Für auf vertraut Vorgänge, können Sie den Dienststatus Agent zum Weiterleiten von ausgehenden http-Anfragen mit einem HTTP-Proxy konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren Azure AD verbinden Health Agent HTTP-Proxy verwendet.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Wenn Sie einen Proxy während der Registrierung Agent konfigurieren müssen, müssen Sie Ihre Proxyeinstellungen von Internet Explorer im Voraus zu ändern.
1. Öffnen von InternetExplorer-Einstellungen > Internet -> Optionen -> Verbindungen -> LAN-Einstellungen.
2. Wählen Sie einen Proxyserver für LAN verwenden.
3. Wählen Sie erweitert, wenn Sie andere Proxy-Ports für HTTP und HTTPS/Secure haben.

**F: unterstützt Azure AD-verbinden Health Services Standardauthentifizierung bei einer Verbindung mit HTTP-Proxys?**

Nein. Ein Verfahren zum Angeben von beliebigen Benutzername und Kennwort für die Standardauthentifizierung wird derzeit nicht unterstützt.


**F: Welche Version von AD DS werden durch Azure AD verbinden Gesundheit für AD DS unterstützt?**

Überwachen von AD DS wird unterstützt, während Sie auf die folgenden BS-Versionen installiert:

- Windows Server 2008 R2
- WindowsServer 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Vorgänge Fragen



**F: muss ich auf Meine AD FS-Proxy-Servern oder meine Web Anwendung Proxy-Servern Überwachung aktivieren?**

Nein, Überwachung muss nicht auf den Servern Web Application Proxy (WAP) aktiviert werden. Aktivieren sie auf dem AD FS-Servern.


**F: wie gelöst Azure AD verbinden Gesundheit Benachrichtigungen erhalten?**

Azure AD verbinden Gesundheit Benachrichtigungen erhalten auf einer Bedingung Erfolg gelöst. Azure AD verbinden Health Agents erkennen und die Konditionen Erfolg Dienst in regelmäßigen Abständen Bericht. Für einige Benachrichtigungen ist das unterdrücken zeitbasierte. Kurzum, wenn demselben Fehler nicht innerhalb von 72 Stunden aus der zweiten Generation benachrichtigen einhalten, wird die Benachrichtigung automatisch aufgelöst.




**F: muss welche Firewallports ich für den Azure AD verbinden Health Agent entwickelt öffnen?**

Sie müssen TCP/UDP Ports 443 und 5671 für den Azure AD verbinden Health Agent zur Kommunikation mit der Azure AD-Dienststatus-Endpunkte können geöffnet haben.


**F: Warum finden Sie zwei Server mit demselben Namen im Azure AD verbinden Dienststatus-Portal**

Wenn Sie einen Agent von einem Server entfernen, wird der Server nicht automatisch aus dem Azure AD-Portal verbinden entfernt.  Wenn Sie manuell einen Agent von einem Server entfernt oder den Server selbst entfernt, müssen Sie den Server-Eintrag aus dem Portal Azure AD verbinden Gesundheit manuell zu löschen. Weitere Informationen finden Sie unter [eine Instanz Server oder einen bestimmten Dienst löschen.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Wenn Sie einem neuen Image einen Server versehen oder einen neuen Server mit den gleichen Details (z. B. Computernamen) erstellt und nicht aus dem Portal Azure AD verbinden Gesundheit Sie den Server bereits registrierten entfernen, installiert den Agent auf dem neuen Server, möglicherweise zwei Einträge mit demselben Namen angezeigt.  
In diesem Fall sollten Sie den Eintrag manuell zu den älteren Server gehören löschen. Die Daten für diesen Server sollten veraltete.

## <a name="related-links"></a>Links zu verwandten Themen

* [Azure AD verbinden Dienststatus](active-directory-aadconnect-health.md)
* [Azure AD verbinden Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Azure AD verbinden Gesundheit Vorgänge](active-directory-aadconnect-health-operations.md)
* [Azure AD-Dienststatus mit AD FS verbinden](active-directory-aadconnect-health-adfs.md)
* [Mithilfe von Azure AD verbinden Gesundheit für synchronisieren](active-directory-aadconnect-health-sync.md)
* [Azure AD-Dienststatus in AD DS verbinden](active-directory-aadconnect-health-adds.md)
* [Azure AD verbinden Versionsverlauf Dienststatus](active-directory-aadconnect-health-version-history.md)

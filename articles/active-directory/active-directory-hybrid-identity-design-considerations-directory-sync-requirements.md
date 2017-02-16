<properties
    pageTitle="Azure Active Directory Hybrid Identität Entwurf Faktoren - bestimmen Directory-Synchronisierung Anforderungen | Microsoft Azure"
    description="Identifizieren, welche Anforderungen erforderlich sind, für die Synchronisierung von allen Benutzern zwischen on = lokale und Cloud für Unternehmen."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Ermitteln der Directory-Synchronisierung erforderlich
Synchronisierung ist das Bereitstellen der Benutzer einer Identität in der Cloud entsprechend ihrer lokalen Identität. Ob nicht synchronisierte Konto für Authentifizierung oder partnerverbundkontakte Authentifizierung verwendet wird, müssen die Benutzer weiterhin eine Identität in der Cloud haben.  Diese Identität müssen gepflegt und regelmäßig aktualisiert werden.  Die Updates können viele Formulare, aus Titel Änderungen Kennwörter geändert werden.  

Starten Sie durch die Organisationen lokalen Identität Lösung und Benutzer Anforderungen auswerten. Diese Bewertung ist wichtig, die technischen Vorschriften für wie Benutzeridentitäten erstellt wird und in der Cloud aufrechterhalten definieren.  Bei den meisten Organisationen Active Directory ist lokal und lokalen Verzeichnis, dem Benutzer von werden synchronisiert aus, jedoch in einigen Fällen dies nicht der Fall sein wird.  

Vergewissern Sie sich die folgenden Fragen beantworten:


- Haben Sie eine Active Directory-Struktur, mehrere oder keine?
 - Wie viele Azure AD-Verzeichnisse durchsuchen, werden Sie zu synchronisieren?
 
    1. Verwenden Sie filtern?
    2. Haben Sie mehrere Azure AD verbinden Server geplant?
  
- Führen Sie aktuell eine Synchronisation tool lokale?
  - Wenn Ja, bedeutet die Benutzer an, wenn Benutzer eine virtuelle/Verzeichnisintegration Identitäten haben?
- Haben Sie alle anderen Verzeichnis lokalen, die an (z. B. LDAP-Verzeichnis, HR-Datenbank usw.) synchronisiert werden soll?
  - Werden alle GALSync Aktionen werden?
  - Was ist im aktuellen Zustand des Benutzerprinzipalnamen in Ihrer Organisation? 
  - Haben Sie ein anderes Verzeichnis, dem Benutzerauthentifizierung?
  - Verwendet Ihr Unternehmen Microsoft Exchange?
    - Planen sie von einer Exchange-hybridbereitstellung Probleme?

Jetzt, da Sie eine Vorstellung zu Ihren Anforderungen Synchronisierung haben, müssen Sie festlegen, welches Tool auf diese Anforderungen entsprechen richtig ist.  Microsoft bietet verschiedene Tools zur Verzeichnisintegration und Synchronisierung zu erreichen.  Der [Hybrid Identität Directory Integration Tools-Vergleichstabelle](active-directory-hybrid-identity-design-considerations-tools-comparison.md) für Weitere Informationen finden Sie unter. 
   
Jetzt, da Sie Ihren Anforderungen Synchronisierung und Tools die hierfür für Ihr Unternehmen haben, müssen Sie die Anwendungen ausgewertet werden soll, in denen diese Directory-Dienste verwendet. Diese Bewertung ist wichtig, definieren die technischen Anforderungen diesen Anwendungen in der Cloud integriert werden soll. Vergewissern Sie sich die folgenden Fragen beantworten:

- Werden diese Applications in die Cloud verschoben und Verzeichnis verwenden?
- Gibt es spezielle Attribute, die in der Cloud, damit diese Anwendung sie erfolgreich verwenden dürfen synchronisiert werden müssen?
- Müssen diese Applikationen neu geschrieben werden, der Cloud-Authentifizierung nutzen?
- Werden diese Applikationen weiterhin lokale live, während der Benutzer mit der Cloud Identität darauf zugreifen?

Sie müssen außerdem die Sicherheit Anforderungen und Einschränkungen Verzeichnissynchronisation zu bestimmen. Diese Bewertung ist wichtig, erhalten eine Liste mit den Anforderungen, die zum Erstellen und Verwalten von Benutzeridentitäten in der Cloud benötigt werden. Vergewissern Sie sich die folgenden Fragen beantworten:

- Wo wird der Synchronisierungsserver gefunden werden?
- Werden Domäne hinzugefügt?
- Wird der Server in einem eingeschränkten Netzwerk hinter einem Firewall, wie etwa einer DMZ befinden?
  - Werden Sie können die erforderlichen Firewallports, um die Synchronisierung unterstützen öffnen?
- Haben Sie einen Plan zur Wiederherstellung für die Synchronisierungsserver?
- Verfügen Sie über ein Konto mit den geeigneten Berechtigungen für alle Gesamtstrukturen bilden sollen mit zu synchronisieren?
 - Wenn Ihr Unternehmen die Antwort auf diese Frage wissen nicht, lesen Sie den Abschnitt "Berechtigungen für die Synchronisierung von Kennwörtern" im Artikel [Installieren der Azure Active Directory-Synchronisierung-Dienst](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) und zu ermitteln Sie, wenn Sie bereits über ein Konto mit diesen Berechtigungen verfügen oder wenn Sie einen erstellen müssen.
- Wenn Sie haben ist für Multi-Gesamtstruktur synchronisieren Sync-Server keine zu jeder Gesamtstruktur abrufen?
 
>[AZURE.NOTE]
Vergewissern Sie sich zum Erfassen von Notizen für jede Antwort und die Gründe für die Antwort zu verstehen. [Ermitteln Reaktion Anforderungen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) wird über die verfügbaren Optionen wechseln. Durch Probleme beantwortet diese Fragen, die Sie auswählen, werden die beste Option ist für Ihr Unternehmen geeignet ist.

## <a name="next-steps"></a>Nächste Schritte
[Ermitteln Sie die kombinierte authentifizierungsanforderungen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Siehe auch
[Entwurf Aspekte (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)

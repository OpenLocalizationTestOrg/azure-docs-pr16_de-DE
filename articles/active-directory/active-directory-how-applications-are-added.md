<properties
   pageTitle="Wie werden Applikationen Azure Active Directory hinzugefügt."
   description="Dieser Artikel beschreibt, wie eine Instanz der Azure Active Directory Applications hinzugefügt werden."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Wie und warum Applikationen Azure AD hinzugefügt werden

Eine für die ursprünglich verblüffenden Aspekte beim Anzeigen einer Liste von Applications in Ihre Azure Active Directory-Instanz ist Grundlegendes zu, in dem die Anwendungen stammen, und warum sie dort angezeigt werden.  In diesem Artikel wird, bieten eine auf hoher Ebene Übersicht wie Applications im Verzeichnis dargestellt werden und bieten Ihnen Kontext, die dabei Sie verstehen helfen, wie eine Anwendung gesucht haben, werden in Ihrem Verzeichnis.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Bietet auf welche Dienste Azure AD auf Anwendungen?

Applikationen hinzugefügt Azure AD nutzen, eine oder mehrere der Dienstleistungen, die ihn enthält.  Dienste gehören:

* App-Authentifizierung und Autorisierung
* Benutzerauthentifizierung und Autorisierung
* Einmaliges Anmelden (SSO) mit Föderation oder Kennwort
* Benutzer bereitgestellt und Synchronisierung
* Rollenbasierte Access-Steuerelement Verwenden Sie zum Definieren von Anwendungsrollen zum Ausführen von Rollen Verzeichnis basiert Autorisierung Prüfungen in einer app.
* oAuth Autorisierung Services (verwendet von Office 365 und andere Microsoft-apps zum Autorisieren des Zugriffs auf APIs/Ressourcen.)
* Veröffentlichen der Anwendung & Proxy; Veröffentlichen einer app aus einem privaten Netzwerk mit dem internet

## <a name="how-are-applications-represented-in-the-directory"></a>Wie werden Applikationen im Verzeichnis dargestellt?

Anwendungen werden in Azure AD 2 Objekte dargestellt: ein Objekt Application und ein Hauptbenutzer Service-Objekt.  Liegt eine Anwendungsobjekt registriert in einer "Start" / "Besitzer" oder "Veröffentlichen" Directory und mindestens service wichtigsten Objekte, die die Anwendung in jedem Verzeichnis, in dem sie fungiert, darstellt.  

Das Anwendungsobjekt beschrieben, die app Azure AD (der Dienst mit mehreren Mandanten) und möglicherweise eine der folgenden Aktionen enthalten: (*Hinweis*: Dies ist keine vollständige Liste.)

* Namen, Logo und Publisher
* Kennwörter (symmetrischen und/oder asymmetrische Schlüssel verwendet, um die app authentifizieren)
* API Abhängigkeiten (oAuth)
* APIs/Ressourcen/Bereiche veröffentlicht (oAuth)
* App Rollen (RBAC)
* SSO-Metadaten und Konfiguration (SSO)
* Benutzer, die Bereitstellung von Metadaten und Konfiguration
* Proxy-Metadaten und Konfiguration

Die Dienst Tilgungsanteile ist, einen Eintrag der Anwendung in jedem Verzeichnis, wo die Anwendung fungiert, einschließlich sein home-Verzeichnis.  Der Dienst Tilgungsanteile:

* Verweist auf Anwendungsobjekt über die app-Id-Eigenschaft
* Lokale Benutzer Datensätze und Gruppe app-rollenzuweisungen
* Lokale Benutzer und Administrator Berechtigungen gewährt bei der app-Einträge
    * Beispiel: Berechtigungen für die app zu einer bestimmten Benutzer-e-Mails
* Datensätze lokale Richtlinien einschließlich bedingte Zugriffsrichtlinie
* Datensätze lokale alternative lokale Einstellungen für eine app
    * Ansprüche Transformationsregeln
    * Attribut Zuordnungen (Benutzer bereitgestellt)
    * Mandanten Sie bestimmten app Rollen (wenn die app benutzerdefinierte Rollen unterstützt)
    * Namen/Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Ein Diagramm der Anwendungsobjekte und Dienst Hauptbenutzer über Verzeichnisse durchsuchen

![Ein Diagramm veranschaulichen, wie die Anwendung Objekte und Hauptbenutzer mit Azure AD-Instanzen vorhandenen service.][apps_service_principals_directory]

Wie Sie aus dem Diagramm oben sehen können.  Microsoft bietet zwei Verzeichnisse verwendet Applikationen veröffentlichen, intern (auf der linken Seite).

* Eine für Microsoft-Apps (Microsoft Services Directory)
* Eine für integrierte 3rd Party Apps (App-Katalog Verzeichnis)

Anwendung Herausgeber/Lieferanten, die Integration mit Azure AD sind erforderlich, um die Veröffentlichung Verzeichnis haben.  (Einige SAAS Verzeichnis).

Einbeziehen von Applications, die Sie selbst hinzugefügt:

* Apps, die Sie entwickelt (mit AAD integriert)
* Apps, die Sie für verbunden-einmaliges Anmelden
* Apps, die Sie mit der Anwendung Azure AD-Proxy veröffentlicht.

### <a name="a-couple-of-notes-and-exceptions"></a>Ein paar Notizen und Ausnahmen

* Zeigen Sie nicht alle Dienst Hauptbenutzer zurück zur Anwendungsobjekte.  Nicht wahr? Wenn Azure AD ursprünglich erstellt wurde die Dienstleistungen für Applikationen sehr viel begrenzter wurden und die Tilgungsanteile Service wurde für die eine app Identität ausreichend.  Der wichtigsten ursprüngliche Dienst wurde in Form des Windows Server Active Directory-Dienstkontos näher.  Aus diesem Grund ist es immer noch möglich, Dienst Hauptbenutzer mithilfe der PowerShell Azure AD ohne Erstellen eines Anwendungsobjekts zu erstellen.  Die Graph-API benötigt ein app-Objekts vor dem Erstellen eines Diensts Hauptbenutzer.
* Die oben beschriebenen Informationen nicht für alle wird derzeit verfügbar gemacht programmgesteuert.  Nur sind in der Benutzeroberfläche verfügbar:
    * Ansprüche Transformationsregeln
    * Attribut Zuordnungen (Benutzer bereitgestellt)
* Weitere detaillierte Informationen Kapitalrückzahlung Dienst und Anwendungsobjekte finden Sie in der Dokumentation Azure AD Graph REST-API.  *Hinweis*: der Azure AD Graph-API-Dokumentation richtet sich am ehesten mit einem Schema verweisen Azure AD, die derzeit verfügbar ist.  
    * [Anwendung](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Dienst Tilgungsanteile](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Wie werden meine Azure AD-Instanz apps hinzugefügt?
Es gibt viele Arten, um die eine app Azure AD hinzugefügt werden kann:

* Hinzufügen einer app aus dem [Azure Active Directory-App-Katalog](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Melden Sie sich nach oben/in einer 3rd Party App mit Azure Active Directory integriert (zum Beispiel: [Smartsheet](https://app.smartsheet.com/b/home) oder [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Während der Anmeldung nach oben/in Benutzer werden aufgefordert, die Zugriffsberechtigung bei der app für ihr Profil und andere Berechtigungen erteilen.  Die erste Person, die Zustimmung erteilen bewirkt, dass ein Dienst Tilgungsanteile, die app darstellt Verzeichnis hinzugefügt werden.
* Melden Sie sich nach oben/in Microsoft online Services wie [Office 365](http://products.office.com/)
    * Wenn Sie Office 365 abonnieren oder beginnen eine Testversion, die eine oder mehrere Dienst Hauptbenutzer, im Verzeichnis erstellt wurden, die die verschiedenen Dienste, mit denen alle der Funktionen im Zusammenhang mit Office 365 vorführen darstellt.
    * Einige Office 365-Diensten wie SharePoint erstellen Dienst Hauptbenutzer auf eine kontinuierlich sicheren Kommunikation zwischen Komponenten, einschließlich Workflows dürfen.
* Hinzufügen eine app, die Sie zur Entwicklung von sind in der Azure-Verwaltungsportal finden Sie unter: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Fügen Sie eine app, die Sie beherzigen mit Visual Studio finden Sie unter:
    * [ASP.Net Authentifizierungsmethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Verbundende Dienste](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Hinzufügen einer app zu verwenden, um den [Azure AD-Anwendungsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx) verwenden
* Verbinden einer app für einmaliges Anmelden mit SAML oder Kennwort SSO
* Viele andere einschließlich verschiedene Entwicklertools Erfahrung in Azure auftritt und/in API Explorer über Developer Center

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wer hat die Berechtigung zum Hinzufügen von Applications Meine Azure AD-Instanz?

Nur globale Administratoren können:

* Hinzufügen von apps aus dem Azure AD-app-Katalog (vordefinierte integrierte 3rd Party Apps)
* Veröffentlichen einer über den Azure AD-Anwendungsproxy-app

Alle Benutzer in Ihrem Verzeichnis besitzen Berechtigungen zum Hinzufügen von Applications, die sie entwickeln und nach Maßgabe über die Anwendungsmöglichkeiten Weise freigeben/Zugriff auf ihre Organisation Daten gewähren.  *Denken Sie daran, eine app Benutzer anmelden oben/und Erteilen von Berechtigungen dazu führen, dass ein Dienst Tilgungsanteile erstellt wird.*

Diese möglicherweise Anfangs sound betreffend, aber beibehalten die folgenden Punkte:

* Apps wurden, Windows Server Active Directory für die Benutzerauthentifizierung für viele Jahre zu nutzen, ohne dass die Anwendung im Verzeichnis registriert/aufgezeichnet werden können.  Jetzt wird die Organisation verbessert werden können Sichtbarkeit zu genau wie viele apps im Verzeichnis und wofür verwenden.
* Keine Notwendigkeit der Admin-app für die Veröffentlichung/Registrierung Vorgang leistungsgesteuert.  Mit Active Directory Federation Services war es wahrscheinlich, dass ein Administrator hat eine app als vertrauende für Entwickler hinzuzufügen.  Jetzt können Entwickler Self-Service.
* Benutzer, die bei der Anmeldung in/auf apps, die über ihre Organisation-Konten für geschäftliche Zwecke ist eine gute Sache.  Wenn die Organisation später verlassen wird Access zu ihrem Konto in der Anwendung unterbrochen verwendeten wurden.
* Probleme einen Datensatz aus, welche Daten der Anwendung eine gute Sache ist freigegeben wurde.  Daten sind mehr als je zuvor Portable und einen Eintrag löschen, wer welche Daten für die Anwendungsmöglichkeiten freigegeben sind hilfreich.
* Apps, die Azure AD für oAuth verwenden entscheiden Sie genau welche Berechtigungen, die Benutzer können auf Anwendungen erteilen und die Berechtigungen erfordern Administrator zustimmen.  Es sollte wechseln, ohne angezeigt, die besagt, dass nur Administratoren größere Bereiche und weitere signifikante Berechtigungen Zustimmung können.
* Benutzer hinzufügen und gleicht apps auf ihre Daten zugreifen sind überwacht Ereignisse, damit Sie die Überwachungsberichte innerhalb des Portals Azure-Verwaltung, um zu ermitteln, wie eine app Verzeichnis hinzugefügt wurde anzeigen können.

**Hinweis:** *Microsoft selbst mit der Standard-Konfiguration für nun viele Monate ausgeführt wurde.*

Alle, die unter dem Gesichtspunkt sind ist es möglich, um zu verhindern, dass Benutzer in Ihrem Verzeichnis hinzufügen von Applications und aus nach Maßgabe nutzen, welche Informationen sie freigeben für Applikationen durch Directory-Konfiguration im Verwaltungsportal Azure ändern.  Die folgende Konfiguration kann innerhalb der Azure-Verwaltungsportal auf des Verzeichnisses "Konfigurieren" Registerkarte zugegriffen werden.

![Ein Screenshot der Benutzeroberfläche zum Konfigurieren der Einstellungen für die integrierten app][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Hinzufügen von Applications zu Azure AD und wie Dienste für apps zu konfigurieren.

* Entwickler: [erfahren Sie, wie eine Anwendung mit AAD integriert werden soll.](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Entwickler: [Überprüfen Stichprobe Code für apps mit Azure Active Directory auf Github integriert](https://github.com/AzureADSamples)
* Entwickler und IT-Experten: [Überprüfen der REST-API-Dokumentation für die Azure Active Directory Graph-API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-Experten: [Informationen zum Verwenden von Azure Active Directory vorab Spezialisten aus dem App-Katalog](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-Experten: [Suchen nach Lernprogramme zur Konfiguration eines bestimmten vorab integrierte apps](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-Experten: [erfahren Sie, wie Sie eine app mithilfe der Azure Active Directory-Anwendungsproxy veröffentlichen](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Siehe auch

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg

<properties
    pageTitle="Azure Active Directory Federation Kompatibilitätsliste"
    description="Diese Seite enthält Identitätsanbieter nicht von Microsoft, die verwendet werden können, um einmaliges Anmelden implementieren."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure Active Directory Federation Kompatibilitätsliste
Azure-Active Directory bietet einmalige Anmelden auf und erweiterte Anwendung Zugriffsschutz für Office 365 und andere Microsoft Online Services für Hybrid und nur-Cloud-Implementierungen ohne alle nicht-Microsoft-Lösung. Office 365, wie die meisten des Microsoft Online Services ist für Directory Services, Authentifizierung und Autorisierung Azure Active Directory integriert. Azure-Active Directory bietet auch einmaliges Anmelden für Tausende von Applications SaaS und lokalen web-Applikationen. Finden Sie in der Galerie für unterstützte SaaS Applikationen Azure Active Directory.

Organisationen, die in nicht-Microsoft Föderation Lösungen investiert haben, enthält dieses Thema Anleitungen für einmaliges Anmelden für ihre Windows Server Active Directory-Benutzer mit Microsoft Online Services mithilfe von nicht-Microsoft-Identitätsanbieter "Azure-Active Directory Federation Kompatibilität aus der Liste aus" unter konfigurieren. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford Computer Gruppe](http://oxfordcomputergroup.com/)eines dritten im Auftrag von Microsoft, getestet diese einzelnen anmelden Erfahrung mit nicht-Microsoft-Identitätsanbieter anhand einer Reihe von Fällen allgemeine mit Azure Active Directory.

Informationen dazu, wie Sie Ihre Drittanbieter-Identitätsanbieter hier aufgeführten zugreifen können, wenden Sie sich an Oxford Computer bei [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford Computer Gruppe getestet nur die Föderation Funktionalität dieser einzelnen Szenarien anmelden. Oxford Computer Gruppe keine Tests die Synchronisation, Authentifizierung mit zwei Faktoren, usw. Komponenten diese Szenarios für einzelne Zeichen durchführen.

>Verwendung der Anmeldung durch alternative Benutzerprinzipalnamen-ID wird auch nicht in diesem Programm getestet.



- [Azure-Active Directory](#azure-active-directory)
- [Optimale IDM virtuelle Identität Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli Partnersuche Identität Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ 4.0.1 Access-Manager](#netiq-access-manager-401) 
- [BIG-IP-mit Access Richtlinie Manager BIG-IP-Version 11.3 x – 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware Version 2.1 Arbeitsbereich-Portal](#vmware-workspace-portal-version-21) 
- [Melden Sie sich & 5.3 wechseln](#signampgo-53) 
- [IceWall Föderation Version 3.0](#icewall-federation-version-30) 
- [Sichere Cloud Zertifizierungsstelle](#ca-secure-cloud) 
- [Dell eine Identität Cloud Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil Single Sign-On 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Da diese Drittanbieter-Produkte sind, bietet Microsoft keine Unterstützung für die Bereitstellung, Konfiguration, Problembehandlung, bewährte Methoden, usw. Probleme und Fragen in Bezug auf diesen Identitätsanbieter. Wenden Sie sich an den unterstützten Drittanbietern direkt, für Support und Fragen in Bezug auf diesen Identitätsanbieter.

>Diese Drittanbieter-Identitätsanbieter wurden Zusammenarbeit mit Microsoft-Cloud-Diensten mit WS-Federation und WS-Trust Protokolle nur geprüft. Tests nicht enthalten mit dem SAML-Protokoll.

## <a name="azure-active-directory"></a>Azure-Active Directory 
Azure-Active Directory können Benutzer authentifizieren, indem Sie eine Föderation mit Ihrem lokalen Active Directory oder ohne einen lokalen Föderation Server bestimmte Kennwort synchronisieren zu können. 

Im folgenden werden die Szenario-Support-Matrix für diese Erfahrung anmelden: 


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|
|Modernes Anwendungen verwenden, wie Office 2016 ADAL| Unterstützt|Keine|

Weitere Informationen zur Verwendung von Azure Active Directory mit AD FS finden Sie unter [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Weitere Informationen zur Verwendung von Azure Active Directory mit Kennwort Synchronisieren finden Sie unter [Azure AD verbinden](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimale IDM virtuelle Identität Server Federation Services 
Optimale IDM virtuelle Identität Server Federation Services können Benutzer authentifizieren, die sich in Kunden lokalen Active Directory befinden.

Im folgenden finden Sie das Szenario Supportübersicht diese einzelnen Erfahrung anmelden:


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Für Weitere Informationen zu ClientAccess finden Sie unter Richtlinien [Einschränken des Zugriffs auf Office 365-Dienste basierend auf den Speicherort der Client.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 implementiert den häufig verwendeten WS Verbund Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine (frühere Versionen müssen auf 6.11 aktualisieren|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

So konfigurieren Sie diese Anweisungen PingFederate STS zu Ihrer Active Directory-Benutzer, die einzelne anmelden Oberfläche zur Verfügung stellen die PDF-Datei herunterladen [hier.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Die PingFederate Anweisungen zum Konfigurieren dieser STS auf die einzelne anmelden Oberfläche Ihrer Active Directory-Benutzern zur Verfügung stellen, finden Sie unter [hier.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Die PingFederate Anweisungen zum Konfigurieren dieser STS auf die einzelne anmelden Oberfläche Ihrer Active Directory-Benutzern zur Verfügung stellen, finden Sie unter [hier.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify Flexibilität bei einer partnerverbundkontakte einzelnen anmelden für Office 365 ohne die Anforderung einen lokalen Föderation Server hosten.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Access-Client-Steuerelemente wird nicht unterstützt. 

Weitere Informationen zu Centrify, finden Sie unter [hier.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli Partnersuche Identität Manager 6.2.2 
IBM Tivoli Partnersuche Identität Manager 6.2.2 mit IBM Security Access Manager für Microsoft Applications 1.4 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu IBM Tivoli Partnersuche Identität Manager finden Sie unter [IBM Security Access Manager für Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einzelnen anmelden und das Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu SecureAuth finden Sie unter [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 kumulative Version 4
CA SiteMinder Föderation 12.52 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu CA SiteMinder, finden Sie unter [CA SiteMinder Föderation.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne Cloud Federation Service (CFS) 3.0 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu RadiantOne CFS, finden Sie unter [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 


| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung erfordert die Einrichtung eines weiteren Webserver und Okta Anwendung.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu Okta finden Sie unter [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin im Mai 2014 getesteten implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu OneLogin finden Sie unter [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ 4.0.1 Access-Manager 
NetIQ Access Manager 4.0.1 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |* Kerberos Verträge unterstützt.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

* NetIQ unterstützt Kerberos-Authentifizierung über die Konfiguration von einem Kerberos-Vertrag.  Informationen zu dieser Konfiguration wenden Sie sich an NetIQ oder zeigen Sie im Setuphandbuch an. Weitere Informationen zu NetIQ Access Manager finden Sie unter [Access-Manager NetIQ.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP-mit Access Richtlinie Manager BIG-IP-Version 11.3 x – 11.6 x 
Mit Access Richtlinie Manager (APM) BIG IP BIG-IP-Version 11.3 X – implementiert 11.6 x den häufig verwendeten SAML Identität Standard, um einen einzelnen anmelden und das Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen: 

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Nicht unterstützt |Nicht unterstützt|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu BIG-IP-Access-Richtlinien-Manager finden Sie unter [BIG-IP-Access-Richtlinien-Manager.](https://f5.com/products/modules/access-policy-manager) 

So konfigurieren Sie diese Anweisungen BIG-IP-Access Richtlinie Manager STS zu Ihrer Active Directory-Benutzer, die einzelne anmelden Oberfläche zur Verfügung stellen die PDF-Datei herunterladen [hier.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware Version 2.1 Arbeitsbereich-Portal 
Arbeitsbereich-Portal VMware Version 2.1 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu VMware Arbeitsbereich Portal Version 2.1, laden Sie die PDF-Datei [hier.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Melden Sie sich & 5.3 wechseln 
Melden Sie sich & Start 5.3 implementiert der häufigsten verwendete WS Föderation/WS-Trust Identität standard ein einmaliges Anmelden und Attribut Exchange Framework eingeben.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Kerberos-Verträge unterstützt |
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|


Melden Sie sich & Go 5.3 unterstützt Kerberos-Authentifizierung über die Konfiguration von einem Kerberos-Vertrag.  Um Unterstützung bei dieser Konfiguration, wenden Sie sich an Ilex oder Anzeigen von Handbuch für das [hier.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Föderation Version 3.0 
IceWall Föderation Version 3.0 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu IceWall Föderation, finden Sie unter [hier](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) und [hier.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>Sichere Cloud Zertifizierungsstelle 

Zertifizierungsstelle Secure Cloud implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu Zertifizierungsstellen Secure Cloud, finden Sie unter [CA Secure Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell eine Identität Cloud Access Manager v7.1 
Dell eine Identität Cloud Access Manager implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu Dell eine Identität Cloud Access Manager finden Sie unter [Dell eine Identität Cloud Access Manager.](http://software.dell.com/products/cloud-access-manager)

 Die Anweisungen zum Konfigurieren dieser STS die einzelne anmelden Oberfläche auf Ihre Office 365-Benutzer bereit, finden Sie unter [Konfigurieren von Office 365-Benutzer.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil Single Sign-On 4.5 
AuthAnvil einzelne melden Sie sich auf 4.5 implementiert den häufig verwendeten WS Föderation/WS-Trust Identität Standard, um einen einmaligen Anmeldens und Attribut Exchange Framework bereitzustellen.

So sieht der Szenario-Support-Matrix für diese Erfahrung für einzelne Zeichen:

| Client |Support  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Webzugriff Exchange und SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|


Weitere Informationen finden Sie unter [AuthAnvil Single Sign On.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)

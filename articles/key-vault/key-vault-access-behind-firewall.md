<properties
    pageTitle="Zugriff auf-Taste Tresor hinter der Firewall | Microsoft Azure"
    description="Erfahren Sie, wie eine Anwendung einen Firewall-Taste Tresor zugreifen"
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
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Zugreifen auf-Taste Tresor hinter der firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>F: Mein Key Tresor Clientanwendung durch eine Firewall geschützt werden muss, welche Ports/Hosts/IP-Adressen sollte ich zum Aktivieren des Zugriffs auf wichtige Tresor öffnen?

Zugriff auf eine wichtige Tresor muss Ihre Clientanwendung Key Tresor mehrere Endpunkte für verschiedene Funktionalitäten zugreifen können.

- Authentifizierung (über Azure-Active Directory)
- Verwaltung von Key Tresor (enthält erstellen gelesen /, aktualisieren und löschen und auch Einstellung-Richtlinien) bis Azure Ressourcenmanager
- Zugreifen auf und Verwalten von Objekten (Schlüssel und Kennwörter) gehörende Kehrmatrix Key Tresor selbst, geht über die wichtigsten Tresor bestimmten Endpunkt (z. B. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Es gibt einige Variationen, abhängig von Ihrer Umgebung und Konfiguration.   

## <a name="ports"></a>Ports

Gesamten Verkehr zu Key Tresor für alle 3 Funktionen (Authentifizierung, Verwaltung und Daten Ebene Access) wechselt über HTTPS: Port 443. Jedoch für Zertifikatsperrlisten werden gelegentlich Datenverkehr HTTP (Port 80). Clients, die Unterstützung von OCSP Zertifikatsperrlisten dürfen nicht erreichen, aber möglicherweise gelegentlich [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)erreichen.  

## <a name="authentication"></a>Authentifizierung

Taste Tresor Clientanwendung müssen Azure Active Directory Endpunkte für die Authentifizierung zugreifen. Der Endpunkt verwendet, hängt von der Konfiguration der AAD Mandanten und den Typ der Tilgungsanteile – Benutzerprinzipalnamen, Dienst Tilgungsanteile und den Typ des Kontos, z. B. Microsoft Account oder Organisations-ID an.  

| Der Tilgungsanteile Typ | Endpunkt-Anschluss: |
|----------------|---------------|
| Benutzer mit Microsoft Account<br> (z. B.user@hotmail.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443<br><br> und <br>Login.Live.com:443   |
| Verwenden die Organisations-ID mit AAD (z. B. Benutzerdienst/der Tilgungsanteileuser@contoso.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443 |
| Benutzerdienst/Tilgungsanteile Organisations -ID + ADFS oder anderen partnerverbundkontakte Endpunkt (z. B. verwendenuser@contoso.com) | Die oben angegebenen Endpunkte für Organisations-ID sowie ADFS oder anderen partnerverbundkontakte Endpunkten |

Es gibt andere komplexen Szenarien aus. Näheres [Azure Active Directory Authentifizierung Datenfluss](/documentation/articles/active-directory-authentication-scenarios/), [Integration von Applications mit Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) und [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx) für Weitere Informationen.  

## <a name="key-vault-management"></a>Key Tresor Projektmanagement

Für die Verwaltung Tresor Schlüssel (CRUD und Festlegen der Zugriffsrichtlinie) muss die Clientanwendung Key Tresor Ressourcenmanager Azure-Endpunkt zugreifen.  

| Typ des Vorgangs | Endpunkt-Anschluss: |
|----------------|---------------|
| Taste Tresor Control Plane Vorgänge<br> über Azure Ressourcenmanager | **Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Deutschland:**<br> Management.microsoftazure.de:443 |
| Azure-Active Directory-Diagramm-API | **Globale:**<br> Graph.Windows.NET:443<br><br> **Azure China:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Graph.Windows.NET:443<br><br> **Azure Deutschland:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Tresor Vorgänge

Für alle wichtigen Tresor Objekt (Schlüssel und Kennwörter) Management und cryptographic Vorgänge muss Key Tresor Client den Endpunkt Key Tresor zugreifen. Je nachdem, wo der Ihrem Tresor Schlüssel unterscheidet das Endpunkt DNS-Suffix. Ist die Taste Tresor Endpunkt Format: < Tresor-Name >. < Region-spezifische-Dns-Suffix > wie in der nachstehenden Tabelle beschrieben.  

| Typ des Vorgangs | Endpunkt-Anschluss: |
|----------------|---------------|
| Taste Tresor Operationen wie cryptographic Vorgänge Schlüssel, erstellt gelesen/aktualisieren und Löschen von Tasten und Kennwörter, festlegen/Get Tags und andere Attribute auf Key Tresor Objekte (Schlüssel/Schlüssel)     | **Globale:**<br> &lt;Tresor-Name&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Tresor-Name&gt;. vault.azure.cn:443<br><br> **Azure US-Regierung:**<br> &lt;Tresor-Name&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresor-Name&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-Adressbereiche

Taste Tresor Dienst verwendet wiederum andere Azure Ressourcen wie PaaS Infrastruktur, daher ist es nicht möglich, einen bestimmten IP-Adressen enthalten, die Endpunkte Key Tresor zu einem beliebigen Zeitpunkt. Jedoch wenn Ihre Firewall unterstützt nur IP-Adresse Bereiche dann Lizenzinformationen Sie im Dokument [Microsoft Azure Datacenter IP-Adressbereiche finden](https://www.microsoft.com/download/details.aspx?id=41653) .   Für die Authentifizierung und Identität (Azure Active Directory) muss eine Anwendung an die Endpunkte beschrieben [Anmelde- und Identität Adressen](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)eine Verbindung herstellen können.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Fragen zu Schlüssel Tresor haben, besuchen Sie die [Azure Schlüssel Tresor-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

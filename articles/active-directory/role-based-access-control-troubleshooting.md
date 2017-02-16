<properties
    pageTitle="Rollenbasierte Behandeln von Problemen Steuerelement | Microsoft Azure"
    description="Erhalten von Hilfe bei Problemen oder Fragen zur Rolle basierend Access Control Ressourcen."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Rollenbasierte Access Control Problembehandlung

## <a name="introduction"></a>Einführung

[Rollenbasierte Access Control](role-based-access-control-configure.md) ist ein leistungsfähiges Feature, das Sie delegieren abgestimmte Zugriff auf Ressourcen in Azure ermöglicht. Dies bedeutet, dass Sie der Meinung sind können sicher Erteilen von einer bestimmten Person das Recht, was genau sie benötigen, verwenden und nicht mehr. Jedoch manchmal das Ressourcenmodell für Azure Ressourcen kann kompliziert sein, und es kann schwierig sein, verstehen genau wie Sie Berechtigungen zu erteilen.

Dieses Dokument können Sie die wissen, was Sie zu erwarten, wenn einige der Rollen Azure-Portal zu verwenden. Diese drei Rollen behandeln alle Ressourcentypen:

- Besitzer  
- Mitwirkenden  
- Reader  

Besitzer und Mitwirkenden haben Vollzugriff auf die Verwaltungsoption kann nicht, aber ein Mitwirkenden Zugriff auf andere Benutzer oder Gruppen. Interessanter ein wenig mit der Rolle Reader, damit das ist, in dem wir einige Zeit in der Regel ausgeben. Finden Sie unter [Erste Schritte-Artikel Access Control rollenbasierte](role-based-access-control-configure.md) Details zum Zugriff gewähren ein.

## <a name="app-service-workloads"></a>App-Dienst Auslastung

### <a name="write-access-capabilities"></a>Schreibzugriff-Funktionen

Wenn Sie ein Benutzer schreibgeschützten Zugriff auf eine einzelne Web app gewähren, werden einige Features deaktiviert, dass Sie nicht erwarten könnte. Die folgenden Verwaltungsfunktionen erfordern den Zugriff auf eine Web-app (Mitwirkender oder Besitzer) **Schreiben** , und in jedem Szenario schreibgeschützt zur Verfügung.

- Befehle (z. B. starten, beenden, usw.)
- Ändern der Einstellungen wie allgemeine Konfiguration, skalierungseinstellungen, zusätzliche Einstellungen und Überwachung Einstellungen
- Zugreifen auf Anmeldeinformationen für die Veröffentlichung und anderen vertraulichen wie app-Einstellungen und Verbindungszeichenfolgen
- Streaming von Protokollen
- Diagnoseprotokolle Konfiguration
- Console (Befehlszeile)
- Aktive und aktuelle Bereitstellungen (für lokale Git fortlaufender Bereitstellung)
- Geschätzte ausgeben
- Webtests
- Virtuelle Netzwerk (nur für einen Reader, wenn ein virtuelles Netzwerk von einem Benutzer mit Schreibzugriff zuvor konfiguriert wurde sichtbar).

Wenn Sie eine der folgenden Kacheln zugreifen können, müssen Sie bitten Sie Ihren Administrator für Mitwirkender Zugriff auf das Web app.

### <a name="dealing-with-related-resources"></a>Umgang mit zugehörige Ressourcen

Web apps werden durch das Vorhandensein von ein paar andere Ressourcen kompliziert, die Interaktion zwischen. So sieht eine typische Ressourcengruppe mit einige Websites aus:

![Web app-Ressourcengruppe](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Als Ergebnis, wenn Sie eine Person erteilen Zugriff auf nur die Web-app, viele der Funktionen auf der Website vorher in der Azure-Portal deaktiviert wird.

Diese Elemente erfordern **Schreiben** den Zugriff auf den **App-Serviceplan** , die Ihre Website entspricht:  

- Anzeigen des Web app der Ebene (Free oder Standard) Preise  
- Maßstab Konfiguration (Anzahl der Instanzen, die Größe des virtuellen Computers, automatisch skalieren Einstellungen)  
- Kontingente (Speicher, Bandbreite, CPU)  

Diese Elemente erfordern **Schreiben** den Zugriff auf die gesamte **Ressourcengruppe** , die Ihre Website enthält:  

- SSL-Zertifikate und Bindungen (Dies ist da SSL-Zertifikate zwischen verschiedenen Websites in der gleichen Ressourcengruppe und geografischen Position genutzt werden können)  
- Warnungsregeln  
- Automatisch skalieren-Einstellungen  
- Anwendung Einsichten Komponenten  
- Webtests  

## <a name="virtual-machine-workloads"></a>Auslastung virtuellen Computern

Viel benötigen wie mit Web apps, einige Features auf das Blade virtuellen Computers Schreibzugriff oder andere Ressourcen in der Ressourcengruppe des virtuellen Computers.

Virtuellen Computern beziehen sich auf den Namen, virtuelle Netzwerke, Speicherkonten und Warnungsregeln Domäne.

Diese Elemente erfordern den Zugriff auf den **virtuellen Computern** **Schreiben** :

- Endpunkte  
- IP-Adressen  
- Datenträger  
- Extensions  

Diese erfordern **Schreiben** den Zugriff auf den **virtuellen Computern**, und die **Ressourcengruppe** (zusammen mit den Domänennamen), die es ist:  

- Festlegen der Verfügbarkeit  
- Laden Sie angeglichene festlegen  
- Warnungsregeln  

Wenn Sie eine der folgenden Kacheln zugreifen können, müssen Sie bitten Sie Ihren Administrator für Mitwirkender Zugriff auf die Ressourcengruppe.

## <a name="see-more"></a>Weitere Informationen finden Sie
- [Access-basierten-Steuerelement Rolle](role-based-access-control-configure.md): Erste Schritte mit RBAC Azure-Portal.
- [Integrierte Rollen](role-based-access-built-in-roles.md): Anzeigen von Details zu der Rollen, die in RBAC standard gehören.
- [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): Informationen zum Erstellen von benutzerdefinierter Rollen an Ihre Bedürfnisse Access anpassen.
- [Erstellen einer Access ändern Verlaufsbericht](role-based-access-control-access-change-history-report.md): Nachverfolgen von geändert rollenzuweisungen in RBAC.

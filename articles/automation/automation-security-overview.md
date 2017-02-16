<properties
   pageTitle="Sicherheit Azure Automatisierung | Microsoft Azure"
   description="Dieser Artikel bietet einen Überblick über die Automatisierung Sicherheit und die verschiedenen Authentifizierungsmethoden für Automatisierung Konten in Azure Automatisierung."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Automatisierung Sicherheit, sichere Automatisierung" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Sicherheit von Azure Automatisierung
Azure automatisieren können Sie zum Automatisieren von Aufgaben für Ressourcen in Azure, lokal und mit anderen Anbietern Cloud z. B. Amazon Web Services (AWS).  In der Reihenfolge für eine Runbooks seine erforderlichen Aktionen ausführen benötigen sie Berechtigungen sicheren Zugriff auf Ressourcen der minimalen Rechte innerhalb des Abonnements erforderlich.  
In diesem Artikel behandeln wir die verschiedenen Authentifizierung Szenarios von Azure-Automatisierung unterstützt, und es wird gezeigt, wie Sie basierend auf der Umgebung oder Umgebungen, die Sie verwalten müssen.  

## <a name="automation-account-overview"></a>Automatisierung Benutzerkonten (Übersicht)
Wenn Sie Azure Automatisierung zum ersten Mal starten, müssen Sie mindestens ein Automatisierung Konto erstellen. Automatisierung Konten können zum Isolieren Automatisierung Ressourcen (Runbooks, Anlagen, Konfigurationen) aus den Ressourcen, die in anderen Konten Automatisierung enthalten sind. Automatisierung Konten können Sie um Ressourcen in separaten logischen Umgebungen zu trennen. Sie können beispielsweise ein Konto für Entwicklung, ein weiteres für die Herstellung und einen anderen für Ihre lokalen Umgebung verwenden.  Ein Konto Azure Automatisierung unterscheidet sich von Ihrem Microsoft-Konto oder die Konten in Ihr Abonnement Azure erstellt.

Die Automatisierung Ressourcen für jedes Konto Automatisierung eines einzelnen Azure Bereichs zugeordnet sind, aber Automatisierung Konten Ressourcen in einem beliebigen Bereich verwalten können. Der wichtigste Grund Automatisierung Konten in verschiedenen Regionen erstellen wäre, wenn Sie Richtlinien einsetzen, die Daten und Ressourcen in einen bestimmten Bereich isoliert werden müssen.

>[AZURE.NOTE]Automatisierung Konten und die Ressourcen enthalten, die im Portal Azure erstellt werden, kann nicht in der klassischen Azure-Portal zugegriffen werden. Wenn Sie diese Konten oder ihre Ressourcen mit Windows PowerShell verwalten möchten, müssen Sie die Ressourcenmanager Azure Module verwenden.

Alle Aufgaben, die Sie für Ressourcen mithilfe von Azure Ressourcenmanager und Azure-Cmdlets in Azure Automatisierung ausführen müssen in Azure Azure Active Directory organisationsidentität Anmeldeinformationen-basierten Authentifizierung authentifizieren.  Authentifizierung war die ursprüngliche Authentifizierungsmethode mit Azure Servicemanagement Modus, aber kompliziert, einrichten.  Authentifizierung für Azure mit Azure AD-Benutzer wurde eingeführt zurück in 2014, nicht nur vereinfachen das Konfigurieren eines Kontos Authentifizierung, aber auch die Möglichkeit, die nicht interaktiv Azure für ein einzelnes Benutzerkonto authentifizieren, die mit sowohl Azure Ressourcenmanager und klassischen Ressourcen gearbeitet unterstützen.   

Aktuell verwendet werden, wenn Sie ein neues Konto für die Automatisierung der Azure-Portal erstellen, wird automatisch erstellt:

-  Führen Sie als Konto erstellt eine neue Dienst Tilgungsanteile in Azure Active Directory, ein Zertifikat, und weist das Mitwirkender rollenbasierte Access-Steuerelement (RBAC), die zum Verwalten von Ressourcenmanager Ressourcen mithilfe des Runbooks verwendet wird.
-  Klassische Ausführen als Konto durch Hochladen eines Zertifikats Management, die zum Verwalten von Azure Service oder klassische Runbooks mit Ressourcen verwendet wird.  

Steuerung des Benutzerzugriffs rollenbasierte steht mit Azure Ressourcenmanager zulässige Aktionen einer Azure AD-Benutzerkonto und Ausführen als Konto gewähren, und diesen Dienst Tilgungsanteile authentifizieren.  Lesen Sie bitte [rollenbasierte Access-Steuerelements in Azure Automatisierung Artikel](../automation/automation-role-based-access-control.md) Weitere Informationen zum Entwickeln des Modells für die Verwaltung von Berechtigungen Automatisierung helfen.  

Runbooks für ein Hybrid Runbooks Worker Datencenters oder gegen computing Services in AWS ausgeführt kann nicht dieselbe Methode verwenden, die in der Regel für die Authentifizierung von Runbooks zu Azure Ressourcen verwendet wird.  Dies ist, da diese Ressourcen außerhalb Azure ausgeführt werden und daher eigene Sicherheitsanmeldeinformationen Automatisierung erfordert authentifizieren Ressourcen, die sie lokal zugreifen werden definiert.  

## <a name="authentication-methods"></a>Authentifizierungsmethoden

In der folgenden Tabelle werden die verschiedenen Authentifizierungsmethoden für jede Umgebung von Azure Automatisierung und der Artikel zur Einrichtung Authentifizierung für Ihre Runbooks unterstützt zusammengefasst.

Methode  |  Umgebung  | Artikel
----------|----------|----------
Azure AD-Benutzerkonto | Azure Ressourcenmanager und Azure Servicemanagement | [Authentifizieren Sie Runbooks mit Azure AD-Benutzerkonto](../automation/automation-sec-configure-aduser-account.md)
Als Konto Azure ausführen | Azure Ressourcenmanager | [Authentifizieren Sie Runbooks mit Azure ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md)
Als Konto Azure klassischen ausführen | Azure Servicemanagement | [Authentifizieren Sie Runbooks mit Azure ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md)
Windows-Authentifizierung | Lokale Datacenter | [Runbooks für Hybrid Runbooks Worker authentifizieren](../automation/automation-hybrid-runbook-worker.md)
AWS Anmeldeinformationen | Amazon-Webdiensten | [Authentifizieren Sie Runbooks mit Amazon-Webdiensten (AWS)](../automation/automation-sec-configure-aws-account.md)




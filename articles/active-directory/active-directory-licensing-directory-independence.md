<properties
   pageTitle="Hinzufügen und Verwalten von mehreren Azure Active Directory-Verzeichnissen | Microsoft Azure"
   description="Anleitungen und bewährte Methoden zum Hinzufügen und Verwalten von Ihrer Azure Active Directory Verzeichnisse durchsuchen, Verzeichnisse als eine vollständig unabhängige Ressourcen an"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Hinzufügen und Verwalten von mehreren Azure Active Directory Verzeichnisse durchsuchen

In Azure Active Directory (Azure AD), wird jedes Verzeichnis eine vollständig unabhängigen Ressource: ein Peer, voll ausgestatteten und logisch unabhängig von anderen Verzeichnisse durchsuchen, die Sie verwalten. Es gibt keine hierarchische Beziehung zwischen Verzeichnissen aus. Diese Unabhängigkeit zwischen Verzeichnissen beinhaltet Ressource Unabhängigkeit, administrative Unabhängigkeit und Synchronisierung Unabhängigkeit.

##<a name="resource-independence"></a>Ressource Unabhängigkeit

Wenn Sie erstellen oder einer Ressource in einem Verzeichnis löschen, hat es keine Auswirkung auf eine beliebige Ressource in einem anderen Verzeichnis, mit Ausnahme von externen Benutzern, die nachfolgend beschriebenen teilweise. Wenn Sie eine benutzerdefinierte Domäne "contoso.com" mit einem Verzeichnis verwenden, können sie mit einem beliebigen anderen Verzeichnis verwendet werden.

##<a name="administrative-independence"></a>Administrative Unabhängigkeit

Wenn ein Benutzer ohne Administratorrechte des Verzeichnisses 'Contoso' Testverzeichnis 'Test' klicken Sie dann erstellt:
- Standardmäßig wird der Benutzer, der ein Verzeichnis erstellt, als externer Benutzer in diesem neuen Verzeichnis hinzugefügt, und die globalen Administratorrolle in diesem Verzeichnis zugewiesen.
- Die Administratoren von Verzeichnis 'Contoso' haben keine direkten Administratorberechtigungen in Verzeichnis 'Test' aus, es sei denn, ein Administrator von 'Test' speziell ihnen diese Berechtigungen erteilt. Administratoren von 'Contoso' können Steuern des Zugriffs auf Verzeichnis "Test", wenn sie das Benutzerkonto steuern, das welche die erstellte 'Test'.
- Wenn Sie ändern (hinzufügen oder entfernen) einer Administratorrolle für einen Benutzer im Verzeichnis ein, die Änderung hat keinen Einfluss auf alle Administratorrolle, die der Benutzer gegebenenfalls in ein anderes Verzeichnis haben.

##<a name="synchronization-independence"></a>Synchronisierung Unabhängigkeit

Sie können jedes Azure AD-Verzeichnis unabhängig voneinander zum Abrufen von Daten aus einer einzelnen Instanz des entweder synchronisiert konfigurieren:
  - Das Tool Verzeichnissynchronisation (DirSync) für die Synchronisierung von Daten mit einer einzelnen Active Directory-Struktur.
  - Azure Active Directory Connector für Forefront Identität Manager zum Synchronisieren von Daten mit einem oder mehreren lokalen Gesamtstrukturen und/oder nicht Azure AD-Datenquellen.

##<a name="add-an-azure-ad-directory"></a>Fügen Sie eine Azure AD-Verzeichnis

Um eine Azure AD-Verzeichnis im klassischen Azure-Portal hinzuzufügen, wählen Sie die Erweiterung Azure Active Directory auf der linken Seite, und tippen Sie auf **Hinzufügen**.

> [AZURE.NOTE]   Im Gegensatz zu anderen Ressourcen Azure sind Ihre Verzeichnisse nicht untergeordneten Ressourcen eines Azure-Abonnements. Wenn Sie Abbrechen oder das Azure-Abonnement zulassen abläuft, können Sie weiterhin Ihre Directory Daten mithilfe von Azure PowerShell, die Azure Graph-API oder andere Schnittstellen wie Office 365 Admin Center zugreifen. Sie können auch ein anderes Abonnement Verzeichnis zuordnen.

Eine allgemeine Übersicht über zur Lizenzierung Azure AD-Problemen sowie optimale Methoden, finden Sie unter [Was ist Azure Active Directory-Lizenzierung?](active-directory-licensing-what-is.md).

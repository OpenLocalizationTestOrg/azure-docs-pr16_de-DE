
<properties
    pageTitle="App-Anforderungen für Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über die Anforderungen für apps, die Sie in Azure RemoteApp verwenden möchten."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>App-Anforderungen

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp unterstützt 32-Bit- oder 64-Bit-Version des Windows-basiertem von Applications aus einem Bild Windows Server 2012 R2 streaming. Die meisten vorhandene 32-Bit- oder 64-Bit-Windows-basierten Anwendungen ausführen "ungeändert" in Azure RemoteApp (Remote Desktop Services oder früher bekannt als Terminaldienste) Umgebung. Jedoch gibt es einen Unterschied zwischen ausgeführt und gut ausgeführt - einige Applikationen ordnungsgemäß funktionieren und Tja, während andere nicht ausführen. Im folgenden finden Sie Anleitung für die Entwicklung von Applications in einer Umgebung Remote Desktop Services und testen, um die Kompatibilität sicherzustellen.

Tipp: Wir arbeiten auf einige Arbeitsbeispiele für apps für Sie zu erstellen. Neue Themen sehen, die mit Microsoft Access, QuickBooks und App-V in RemoteApp besprechen.

## <a name="requirements"></a>Anforderungen
Diese drei Anforderungen, Hilfe Wenn gefolgt, aus Ihrer Anwendung auch in RemoteApp ausführen:

1.  Programme, die alle [Zertifizierung Anforderungen für Windows-desktop-apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) entsprechen und [Remote Desktop Services programming Richtlinien](https://msdn.microsoft.com/library/aa383490.aspx) entsprechen, vollständige Kompatibilität mit RemoteApp müssen.
2.  Applikationen sollten nie Daten lokal speichern auf das Bild oder die RemoteApp Instanzen, die verloren gehen können.  Nach dem Erstellen einer Websitesammlungs RemoteApp die Instanzen werden geklont sind statusfrei und sollte nur Applikationen enthalten. Daten in einer externen Datenquelle oder in das Profil des Benutzers zu speichern.
3.  Benutzerdefinierte Bilder sollten nie Daten enthalten, die verloren gehen können.  

## <a name="testing-your-apps"></a>Testen Ihrer apps
Mit den folgenden Schritten zum Testen von Applications:

1.  Installieren von Windows Server 2012 R2 und Ihrer Anwendung
2.  Remotedesktop aktivieren
3.  Erstellen Sie zwei Benutzerkonten BenutzerA und UserB, beide Benutzerkonten der Remote Desktop-Sicherheitsgruppe hinzufügen.
4.  Prüfen Sie Multi-Session-Kompatibilität, indem Sie zwei gleichzeitigen RDS Sitzungen zu dem Computer, während Sie die Anwendung gestartet.
5.  Überprüfen Sie die app-Verhalten

## <a name="application-development-guidelines"></a>Richtlinien für die Entwicklung
Verwenden Sie die folgenden Richtlinien für die Entwicklung von Applications für RemoteApp.

### <a name="multiple-users"></a>Mehrere Benutzer

- Installieren einer [Anwendung für einen einzelnen Benutzer ](https://msdn.microsoft.com/library/aa380661.aspx)kann Probleme in einer Umgebung mit mehreren Benutzern erstellen.
- Applikationen sollte benutzerspezifische Speicherorte, unabhängig von der globalen Informationen, der für alle Benutzer gilt [benutzerspezifische Informationen zu speichern](https://msdn.microsoft.com/library/aa383452.aspx) .
- RemoteApp verwendet mehrere [Namespaces für Kernelobjekte](https://msdn.microsoft.com/library/aa382954.aspx); ein globaler Namespace wird hauptsächlich von Diensten in Client-/Server-Clientanwendungen verwendet.
- Es ist nicht davon ausgehen, dass es sich bei dem Namen des Computers oder die [IP-Adresse](https://msdn.microsoft.com/library/aa382942.aspx) zugewiesen an den Computer eines einzelnen Benutzers zugeordnet sind, da mehrere Benutzer gleichzeitig auf einem Server Remote Desktop Session Host (RD Session Host) angemeldet werden können.

### <a name="performance"></a>Leistung
- Deaktivieren Sie [Grafikeffekte](https://msdn.microsoft.com/library/aa380822.aspx) , bevor Sie Ihre app RemoteApp hinzufügen.
- Um CPU-Verfügbarkeit für alle Benutzer zu maximieren, [Hintergrundaufgaben](https://msdn.microsoft.com/library/aa380665.aspx) deaktivieren oder effiziente Hintergrundaufgaben, die nicht Ressourcen ankommt sind erstellen.
- Optimieren von, und Anwendung [Thread Verwendung](https://msdn.microsoft.com/library/aa383520.aspx) für eine Umgebung mit mehreren Benutzern, mit mehreren Prozessoren zu verteilen.
- Um die Leistung zu optimieren, empfiehlt es für Applikationen, um zu [ermitteln](https://msdn.microsoft.com/library/aa380798.aspx) , ob sie in einer Clientsitzung ausgeführt werden.

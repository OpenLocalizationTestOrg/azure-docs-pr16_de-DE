<properties 
    pageTitle="Hinzufügen ein Zertifikats im Speicher Java Zertifizierungsstelle | Microsoft Azure" 
    description="Erfahren Sie, wie Sie ein Zertifikat einer (Zertifizierungsstelle CA) zum Java CA Zertifikat (Cacerts) Store Twilio Dienst oder Azure-Dienstbus hinzufügen." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Hinzufügen eines Zertifikats zum Java Zertifizierungsstelle Zertifikate Store
Die folgenden Schritte gezeigt, wie ein Zertifikat einer (Zertifizierungsstelle CA) zum Store Java CA Zertifikat (Cacerts) hinzufügen. Im Beispiel verwendete ist für das Zertifikat der Zertifizierungsstelle vom Dienst Twilio erforderlich. Informationen finden Sie weiter unten im Thema behandelt Installieren des Zertifikats für den Azure-Dienstbus. 

Sie können Keytool verwenden, um das Zertifikat Zertifizierungsstelle vor Ihrer JDK komprimieren und zu den Azure Ordner des Projekts **Approot** hinzufügen hinzuzufügen, oder Sie konnte eine Azure Start-Aufgabe, die Keytool verwendet, um das Zertifikat hinzuzufügen ausführen. In diesem Beispiel wird davon ausgegangen, dass Sie ein Zertifizierungsstellenzertifikat vor der JDK ZIP wird hinzufügen möchten. Darüber hinaus ein bestimmtes Zertifizierungsstelle im Beispiel verwendet werden, aber die Schritte zum Abrufen eines anderen Zertifizierungsstelle und importieren es in den Cacerts Speicher ist vergleichbar.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Hinzufügen ein Zertifikats im Speicher cacerts

1. Führen Sie über die Befehlszeile, die auf Ihre JDKs **Jdk\jre\lib\security** Ordner festgelegt ist vor, um zu sehen, welche Zertifikate installiert sind:

    `keytool -list -keystore cacerts`

    Sie werden aufgefordert, das Kennwort speichern. Das standardmäßige Kennwort lautet **Ändern**. (Wenn Sie das Kennwort ändern möchten, finden Sie unter der Keytool-Dokumentation unter <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) In diesem Beispiel wird vorausgesetzt, dass das Zertifikat mit MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nicht aufgeführt ist, und es (dieses spezielle Zertifikat wurde vom Dienst Twilio API erforderlich) importiert werden soll.
2. Beziehen Sie das Zertifikat aus der Liste der Zertifikate am [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/)aufgeführt. Mit der rechten Maustaste in des Links für das Zertifikat mit Seriennummer 35:DE:F4:CF, und klicken Sie auf den Ordner **Jdk\jre\lib\security** gespeichert. Bei der in diesem Beispiel wird es in eine Datei namens gespeichert wurde **Equifax\_Secure\_Zertifikat\_Authority.cer**.
3. Importieren Sie das Zertifikat über den folgenden Befehl ein:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Wenn Sie aufgefordert, dieses Zertifikat, vertrauen, wenn das Zertifikat MD5 Fingerabdruck 67:CB:9 D aufweist: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, Antworten Sie **j**ein.
4. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass das CA-Zertifikat erfolgreich importiert wurde:

    `keytool -list -keystore cacerts`

5. JDK Packen und des Projekts Azure **Approot** Ordner hinzuzufügen.

Informationen zu Keytool finden Sie unter <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure Root Certificates

Das Zertifikat Baltimore CyberTrust Root vertrauen müssen Ihrer Anwendung, die Azure-Dienste (z. B. Azure-Dienstbus) verwenden. (15 April 2013 Anfang, Beginn haben Azure aus der globalen GTE CyberTrust Root werden im Stammordner des Baltimore CyberTrust migrieren. Diese Migration benötigte mehrere Monate.)

Das Zertifikat möglicherweise bereits installiert sein, Ihre Cacerts Store Baltimore also denken Sie daran, führen Sie die **Keytool-Liste** Befehl nachsehen, ob es bereits vorhanden ist.

Wenn Sie die Baltimore CyberTrust Root hinzufügen müssen, verfügt über fortlaufende Zahl 02:00:00:b9 und SHA1 Fingerabdruck D4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Sie können von <https://cacert.omniroot.com/bc2025.crt>, auf eine lokale Datei mit der Erweiterung **CER**gespeichert werden soll, und klicken Sie dann importiert **Keytool** verwenden (siehe oben) heruntergeladen werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Zertifikate der Stamm von Azure verwendet finden Sie unter [Azure Root Zertifikat-Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Weitere Informationen zu Java finden Sie im [Java Developer Center](/develop/java/).

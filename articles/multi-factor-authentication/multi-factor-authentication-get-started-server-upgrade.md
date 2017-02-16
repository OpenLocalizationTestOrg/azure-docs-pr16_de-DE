<properties 
    pageTitle="Aktualisieren des PhoneFactor-Agents auf Server Azure kombinierte Authentifizierung"
    description="Dieses Dokument beschreibt, wie Sie erste Schritte mit Azure MFA-Server und wie Sie die ältere Phonefactor-Agents aktualisieren."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Aktualisieren des PhoneFactor-Agents auf Server Azure kombinierte Authentifizierung

Upgrade von der PhoneFactor Agent 5.x oder ältere Azure mehrstufige Authentifizierung Server erfordert die PhoneFactor Agent und die verbundenen Komponenten deinstalliert werden, bevor Sie die kombinierte Authentifizierungsserver und zugehörigen verbundenen Komponenten installiert werden können.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Aktualisieren Sie den PhoneFactor-Agent auf Azure mehrstufige Authentifizierungsserver
<ol>
<li>Sichern Sie zuerst die PhoneFactor-Datendatei ein. Speicherort der Standardinstallation ist c:\Programme\Microsoft Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Wenn der Benutzerportal installiert ist:</li>
<ol>
<li>Navigieren Sie zu dem Ordner installieren und die Datei web.config sichern. Speicherort der Standardinstallation ist C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Wenn Sie ein benutzerdefinierte Design im Portal hinzugefügt haben, Sichern Sie Ihre benutzerdefinierten Ordnern unterhalb des Verzeichnisses C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Deinstallieren Sie die Benutzer-Portal durch den PhoneFactor-Agent (nur verfügbar, wenn auf dem gleichen Server wie der PhoneFactor-Agent installiert) oder über die Windows-Programme und Funktionen.</li></ol>




<li>Wenn die Mobile-App-Webdienst installiert ist:
<ol>
<li>Wechseln Sie zu dem Ordner installieren und die Datei web.config sichern. Speicherort der Standardinstallation ist C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Deinstallieren Sie den Mobile-App Webdienst über Windows Programme und Funktionen.</li></ol>

<li>Wenn der Web Service SDK installiert ist, deinstallieren Sie es durch den PhoneFactor-Agent oder Windows-Programme und Funktionen.

<li>Deinstallieren Sie den Agent PhoneFactor durch die Windows-Programme und Funktionen.

<li>Installieren Sie den kombinierte Authentifizierungsserver. Beachten Sie, dass der Pfad zum aufgenommen wird aus der Registrierung aus der vorherigen PhoneFactor Agent-Installation, damit es an derselben Stelle (z. B. c:\Programme Files\PhoneFactor) installieren, sollten. Neue Installationen, einen anderen Standardwert Pfad (z.B. c:\Programme Files\Multi zweifaktorielle Varianzanalyse Authentifizierungsserver) installieren müssen. Die Datendatei links vom vorherigen PhoneFactor Agent sollte während der Installation aktualisiert werden, damit Ihre Benutzer und Einstellungen noch vorhanden sein sollte, nach der Installation von den neuen mehrstufige Authentifizierungsserver.

<li>Wenn Sie dazu aufgefordert werden, aktivieren Sie die kombinierte Authentifizierungsserver, und stellen Sie sicher, dass sie die richtige Replikations-Gruppe zugewiesen ist.

<li>Wenn der Web Service SDK zuvor installiert war, installieren Sie das neue Web Service SDK über die kombinierte Authentifizierung Server-Benutzeroberfläche an. Beachten Sie, dass der Standardnamen virtuelle Verzeichnis jetzt "MultiFactorAuthWebServiceSdk" statt "PhoneFactorWebServiceSdk" ist. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation mit den neuen Namen für die standardmäßige zulassen, müssen Sie andernfalls die URL in einem beliebigen Clientanwendungen zu ändern, die im Web Service SDK wie an der richtigen Position verweisen User Portal und Mobile-App-Webdienst verweisen.

<li>Wenn das Benutzerportal zuvor auf dem Server der PhoneFactor-Agent installiert wurde, installieren Sie das neue mehrstufige Authentifizierung Benutzerportal über die kombinierte Authentifizierung Server-Benutzeroberfläche an. Beachten Sie, dass der Standardnamen virtuelle Verzeichnis jetzt "MultiFactorAuth" statt "PhoneFactor" ist. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation mit den neuen Namen für die standardmäßige zulassen, sollten Sie andernfalls klicken Sie auf das Symbol User Portal in die kombinierte Authentifizierungsserver und Aktualisieren der Benutzer Portal-URL auf der Registerkarte Einstellungen.

<li>Wenn der Benutzerportal und/oder Mobile-App-Webdienst zuvor auf einem anderen Server des PhoneFactor-Agents installiert wurde:
<ol>
<li>Wechseln Sie zu dem Speicherort installieren (z. B. c:\Programme Files\PhoneFactor), und kopieren Sie die entsprechenden Installer(s) an den anderen Server. 32-Bit- und 64-Bit-Installer für die Benutzer-Portal und Mobile-App-Webdiensts sind vorhanden. Sie werden MultiFactorAuthenticationUserPortalSetupXX.msi und MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi Hilfethemas bezeichnet.</li>
<li>Um die Benutzer-Portal auf dem Webserver installiert haben, öffnen Sie ein Eingabeaufforderungsfenster als Administrator aus, und führen Sie die MultiFactorAuthenticationUserPortalSetupXX.msi. Beachten Sie, dass der Standardnamen virtuelle Verzeichnis jetzt "MultiFactorAuth" statt "PhoneFactor" ist. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation mit den neuen Namen für die standardmäßige zulassen, sollten Sie andernfalls klicken Sie auf das Symbol User Portal in die kombinierte Authentifizierungsserver und Aktualisieren der Benutzer Portal-URL auf der Registerkarte Einstellungen. Vorhandene Benutzer über die neue URL informiert werden müssen.</li>
<li>Wechseln Sie Benutzer-Portal installieren Speicherort (z. B. C:\inetpub\wwwroot\MultiFactorAuth) und die Datei web.config bearbeiten. Kopieren Sie die Werte in den Abschnitten AppSettings und ApplicationSettings aus der ursprünglichen web.config-Datei, die in die neue Webkonfigurationsdatei vor dem Upgrade gesichert wurde. Wenn der neue virtuelle Verzeichnis Standardnamen bei der Installation der Web Service SDK von gehalten wurde, ändern Sie die URL im Abschnitt ApplicationSettings an den richtigen Ort verweisen. Wenn alle anderen Standardeinstellungen in der vorherigen Webkonfigurationsdatei geändert wurden, wenden Sie diese derselben Änderungen in die neue web.config-Datei ein.</li>
<li>Um die Mobile-App-Webdienst auf dem Webserver installiert haben, öffnen Sie ein Eingabeaufforderungsfenster als Administrator aus, und führen Sie die MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Beachten Sie, dass der Standardnamen virtuelle Verzeichnis jetzt "MultiFactorAuthMobileAppWebService" statt "PhoneFactorPhoneAppWebService" ist. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Sie möchten einen kürzeren Namen zu erleichtern, geben Sie auf ihren mobilen Geräten Endbenutzer auswählen. Wenn Sie die Installation mit den neuen Namen für die standardmäßige zulassen, sollten Sie andernfalls klicken Sie auf das Symbol Mobile-App in die kombinierte Authentifizierungsserver und aktualisieren die Mobile-App Webdienst-URL.</li>
<li>Wechseln Sie zum Webdienst Mobile-App installieren Speicherort (z. B. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) und die Datei web.config bearbeiten. Kopieren Sie die Werte in den Abschnitten AppSettings und ApplicationSettings aus der ursprünglichen web.config-Datei, die in die neue Webkonfigurationsdatei vor dem Upgrade gesichert wurde. Wenn der neue virtuelle Verzeichnis Standardnamen bei der Installation der Web Service SDK von gehalten wurde, ändern Sie die URL im Abschnitt ApplicationSettings an den richtigen Ort verweisen. Wenn alle anderen Standardeinstellungen in der vorherigen Webkonfigurationsdatei geändert wurden, wenden Sie diese derselben Änderungen in die neue web.config-Datei ein.</li></ol>

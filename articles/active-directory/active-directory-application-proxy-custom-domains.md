<properties
    pageTitle="Arbeiten mit benutzerdefinierten Domänen in Azure AD-Anwendungsproxy | Microsoft Azure"
    description="Deckblätter arbeiten wie mit benutzerdefinierten Domänen in Azure AD-Anwendungsproxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeiten mit benutzerdefinierten Domänen in Azure AD-Anwendungsproxy

Eine Standarddomäne verwenden, können Sie dieselbe URL festlegen, wie der internen und externen URL für den Zugriff auf die Anwendung, damit Ihre Benutzer nur eine URL haben, denken Sie daran, die Zugriff auf die app, ganz gleich, wo sie auf aus zugreifen. Dies ermöglicht Ihnen außerdem, eine einzelne Verknüpfung im Bereich Access für die Anwendung zu erstellen. Wenn Sie die Standarddomäne von Azure AD-Anwendungsproxy bereitgestellten verwenden, ist es keine zusätzliche Konfiguration Sie Ihre Domäne zu aktivieren müssen. Den Fall, dass Sie eine benutzerdefinierte Domäne verwenden, gibt es ein paar Punkte, die Sie tun müssen, um sicherzustellen, dass die Anwendungsproxy erkennt Ihrer Domäne und seine Zertifikate überprüft.

## <a name="selecting-your-custom-domain"></a>Auswählen Ihrer benutzerdefinierte Domäne

1. Veröffentlichen Sie die Anwendung gemäß den Anweisungen in [Clientanwendungen veröffentlichen, mit der Anwendungsproxy](active-directory-application-proxy-publish.md)ein.
2. Nachdem Sie die Anwendung in der Liste der Programme angezeigt wird, wählen Sie ihn aus, und klicken Sie auf **Konfigurieren**.
3. Geben Sie unter **Externe URL**die Ihre benutzerdefinierte Domäne aus.
4. Wenn Ihre externe URL Https ist, werden Sie aufgefordert, ein Zertifikat hochladen, damit diese Azure die URL der Anwendung überprüfen kann. Sie können auch ein Zertifikat Platzhalterzeichen hochladen, die die externe URL der Anwendung entspricht. Diese Domäne muss in der Liste der Ihrer [Azure überprüft Domänen](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure benötigen Sie ein Zertifikat für die URL für die Domäne der Anwendung oder ein Platzhalterzeichen Zertifikat, das die externe URL für die Anwendung entspricht.
5. Vergewissern Sie sich einen DNS-Eintrag hinzufügen, der die interne URL an die Anwendung weiterleitet, die Ihnen die gleiche URL für den internen und externen Zugriff und einer einzelnen Verknüpfung mit der Anwendung in der Liste der Anwendungen ermöglicht.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Häufig gestellte Fragen zum Arbeiten mit benutzerdefinierten Domänen

F: markieren kann ich ein Zertifikat bereits hochgeladen, ohne sie erneut hochladen?  
A: zuvor hochgeladene Zertifikate automatisch zur Anwendung gebunden sind, und es ist genau ein Zertifikat für den Namen der Anwendung Host.  

F: wie kann ich ein Zertifikat hinzufügen und welches Format in das exportierte Zertifikat hochgeladen werden sollte?  
A: das Zertifikat sollte die Konfiguration der Seite Anwendung hochgeladen werden. Das Zertifikat sollte eine PFX-Datei sein.  

F: werden können ECC-Zertifikate verwendet?  
A: Es ist keine explizite Einschränkung auf Signatur Methoden aus.  

F: werden können SAN signierten Zertifikaten verwendet?  
A: Ja.  

F: werden können Zertifikate Platzhalterzeichen verwendet?  
A: Ja.  

F: werden kann ein anderes Zertifikat für jeden Antrag verwendet?  
A: Ja, es sei denn, die zwei Anwendungen den gleichen externen Host freigeben.  

F: Wenn ich eine neue Domäne registriert haben, kann ich die Domäne verwenden?  
A: Ja, wird die Liste der Domänen aus des Mandanten überprüft Domänenliste eingezogen.  

F: Was passiert, wenn ein Zertifikat abläuft?  
A: Sie erhalten eine Warnung im Abschnitt Zertifikat auf der Seite Konfiguration Anwendung. Wenn ein Benutzer versucht, die Anwendung zuzugreifen, wird eine sicherheitswarnung angezeigt.  

F: Was tun kann ich, wenn ich ein Zertifikat für einen angegebenen app ersetzen möchten?  
A: Hochladen Sie ein neues Zertifikat von der Seite der Anwendung konfigurieren.  

F: kann ich löschen ein Zertifikat und Ersetzen Sie ihn?  
A: Wenn Sie ein neues Zertifikat, hochladen, wenn das alte Zertifikat nicht von einer anderen Anwendung verwendet wird, wird es automatisch gelöscht.  

F: Was passiert, wenn ein Zertifikat gesperrt wird?  
A: Sperrung überprüft werden nicht für die Feiertage durchgeführt. Wenn ein Benutzer versucht, den Zugriff auf die Anwendung, je nach Browser, wird möglicherweise eine sicherheitswarnung angezeigt.  

F: verwenden kann ich ein selbst signiertes Zertifikat?  
A: Ja, sind selbstsignierte Zertifikaten zulässig. Beachten Sie, dass, wenn Sie eine private Zertifizierungsstelle verwenden, der CDP (zertifikatsperrung Punkt Verteilungspunkt) für das Zertifikat öffentlich sein sollte.  

F: Gibt es ein Speicherort zum alle Zertifikate Meine Mandanten finden Sie unter?  
A: Dies ist in der aktuellen Version nicht unterstützt.  


## <a name="see-also"></a>Siehe auch

- [Veröffentlichen von Applications mit Proxy-Anwendung](active-directory-application-proxy-publish.md)
- [Einmaliges Anmelden aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivieren von bedingten Zugriff](active-directory-application-proxy-conditional-access.md)
- [Fügen Sie Ihren benutzerdefinierten Domänennamen zu Azure AD hinzu](active-directory-add-domain.md)

Sehen Sie für die neuesten Informationen und Updates sich die [Anwendungsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)

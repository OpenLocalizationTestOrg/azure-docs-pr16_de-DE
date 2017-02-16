<properties 
   pageTitle="Azure mobilen Engagement Problembehandlungsleitfadens - APIs" 
   description="Problembehandlung von Führungslinien für Azure mobilen Engagement - APIs" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Problembehandlung bei der API Probleme

Nachfolgend werden mögliche Probleme mit der Interaktion von Administratoren mit Azure Mobile Engagement über die APIs auftreten können.

## <a name="syntax-issues"></a>Hinweise zur Syntax

### <a name="issue"></a>Problem
- Syntaxfehler mithilfe der API (oder unerwartetes Verhalten).

### <a name="causes"></a>Bewirkt, dass

- Hinweise zur Syntax:
    - Vergewissern Sie sich an die Syntax der bestimmten API prüfen, die Sie verwenden, um zu bestätigen, dass die Option verfügbar ist.
    - Ein allgemeine Problem mit API Verwendung besteht darin, die API erreicht haben und die Pushbenachrichtigungen-API (die meisten Aufgaben sollten API erreichen statt der API Pushbenachrichtigungen durchgeführt) verwechselt werden. 
    - Ein anderer häufiger Fehler mit SDK Integration und die Verwendung der API besteht darin, die SDK-Taste und die Taste API Begriff.
    - Skripts, die die APIs Verbindung müssen mindestens 10 Minuten Daten zu senden, oder die Verbindung wird Timeout (besonders häufig in Monitor API Skripts warten auf Daten). Wenn Zeitlimit verhindern möchten, müssen Sie Ihr Skript einen Ping XMPP alle 10 Minuten zu senden, die Sitzung mit dem Server beibehalten werden soll.

### <a name="see-also"></a>Siehe auch
 
- [API-Dokumentation][Link 4]
- [XMPP Protokoll Informationen]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Kann nicht die API verwenden, um zur Verfügung, in der Azure Mobile Engagement Benutzeroberfläche dieselbe Aktion ausführen

### <a name="issue"></a>Problem
- Eine Aktion, die von der Benutzeroberfläche in Azure Mobile Engagement funktioniert arbeiten nicht aus der verknüpften Azure Mobile Engagement-API.

### <a name="causes"></a>Bewirkt, dass

- Bestätigen, dass Sie die gleiche Aktion, von der Benutzeroberfläche in Azure Mobile Engagement ausführen können zeigt, dass Sie dieses Feature von Azure Mobile Engagement ordnungsgemäß mit dem SDK integriert haben.

### <a name="see-also"></a>Siehe auch
 
- [Benutzeroberfläche Dokumentation][Link 1]
 
## <a name="error-messages"></a>Fehlermeldungen

### <a name="issue"></a>Problem
- Fehlercodes mithilfe der-API zur Laufzeit oder in einem Protokolle angezeigt.

### <a name="causes"></a>Bewirkt, dass

- Es folgt eine zusammengesetzte Liste Allgemeine API Status Codes Zahlen für den Verweis und über der vorläufigen Problembehandlung:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Siehe auch

- [API-Dokumentation - detaillierte Fehler auf jede bestimmte API][Link 4]
 
## <a name="silent-failures"></a>Automatische Fehlern

### <a name="issue"></a>Problem
- API Aktion schlägt fehl, jedoch keine Fehlermeldung angezeigt, die zur Laufzeit oder in einem Protokolle angezeigt.

### <a name="causes"></a>Bewirkt, dass

- Viele Elemente werden werden in der Azure Mobile Engagement Benutzeroberfläche deaktiviert werden, wenn sie richtig, integriert werden nicht aber automatisch aus der-API fehl, also denken Sie daran, die die gleiche Funktionalität von der Benutzeroberfläche aus, um festzustellen, ob er immer funktioniert, testen.
- Azure Mobile Engagement und viele erweiterten Features von Azure Mobile Engagement Sie versuchen, Sie verwenden möchten, müssen einzeln integriert werden in Ihrer app mit dem SDK als getrennten Schritten, bevor Sie sie verwenden können.

### <a name="see-also"></a>Siehe auch

- [Problembehandlungsleitfadens - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 

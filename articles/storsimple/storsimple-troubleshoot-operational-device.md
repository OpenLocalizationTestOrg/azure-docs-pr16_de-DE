<properties 
   pageTitle="Behandeln von Problemen mit einem bereitgestellten StorSimple Gerät | Microsoft Azure"
   description="Beschreibt, wie diagnostizieren und beheben aufgetretenen auf einem StorSimple Gerät, das aktuell bereitgestellten und funktionsfähig ist."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Behandeln von Problemen mit einem Betrieb StorSimple-Gerät

## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält nützliche Leitfaden für die Problembehandlung zum Beheben von Problemen mit der Konfiguration, die möglicherweise auftreten, nachdem Ihr Gerät StorSimple bereitgestellten und funktionsfähig ist. Es werden häufige Probleme, zu den möglichen Ursachen und Schritte empfohlen, mit denen Sie Probleme beheben, die auftreten können, wenn Sie Microsoft Azure StorSimple ausführen. Diese Informationen gelten für das StorSimple lokalen physischen Gerät und das StorSimple virtuelle Gerät.

Am Ende dieses Artikels, die Sie eine Liste der Fehlercodes finden können, die während des Betriebs Microsoft Azure StorSimple auftreten können, sowie die Schritte erhalten Sie die Fehler zu beheben. 

## <a name="setup-wizard-process-for-operational-devices"></a>Setup-Assistenten-Prozess für funktionsfähige Geräte

Verwenden Sie den Setupassistenten ([Aufrufen-HcsSetupWizard][1]) überprüfen Sie die Gerätekonfiguration und Ausführen von Maßnahmen, falls erforderlich.

Wenn Sie den Setup-Assistenten auf einem Gerät zuvor konfiguriert und Betrieb ausführen, unterscheidet sich Prozessfluss. Sie können nur die folgenden Einträge ändern:

- IP-Adresse, Subnetz-Maske und gateway
- Primärer DNS-server
- Primäre NTP-server
- Optionale Web Proxy-Konfiguration

Der Setup-Assistenten führt die Vorgänge im Zusammenhang mit Kennwort Sammlung und Gerät Registrierung keine.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Während des Setup-Assistenten nachfolgende ausgeführt auftretende Fehler

Die folgende Tabelle beschreibt die Fehler, die möglicherweise beim Ausführen der Setup-Assistenten auf einem Gerät Betrieb, den möglichen Ursachen der Fehler auftreten, und Aktionen Lösungsmöglichkeiten empfohlen. 

| Nein. | Fehlermeldung oder Bedingung | Zu den möglichen Ursachen | Empfohlene Aktion |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Fehler 350032: Dieses Gerät wurde bereits deaktiviert. | Dieser Fehler wird angezeigt, wenn Sie den Setup-Assistenten auf einem Gerät ausführen, die deaktiviert wird. | [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) für den nächsten Schritten fort. Eine deaktivierte Geräte kann nicht im Dienst platziert werden. Eine Fabrik zurücksetzen kann erforderlich sein, bevor das Gerät wieder aktiviert werden kann. |
|  2  | Rufen Sie HcsSetupWizard: ERROR_INVALID_FUNCTION (Ausnahme: 0 x 80070001) | Das DNS-Server-Update schlägt fehl. DNS-Einstellungen globalen Einstellungen sind und über alle aktivierten Netzwerkschnittstellen angewendet werden. | Aktivieren Sie die Benutzeroberfläche und wenden Sie die DNS-Einstellungen erneut an. Dies kann im Netzwerk für andere aktivierten Schnittstellen beeinträchtigen, da diese Einstellungen global sind. |
|  3  | Das Gerät, die im ServicePortal StorSimple Manager online zu sein scheint, aber wenn Sie versuchen, schließen Sie die minimale Einrichtung und speichern Sie die Konfiguration, die nicht durchgeführt. | Während der ersten Einrichtung wurde der Webproxy nicht konfiguriert, obwohl ein tatsächlichen Proxyserver enthielt. | Verwenden Sie das [Cmdlet Test-HcsmConnection] [ 2] um den Fehler zu suchen. [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) , wenn Sie nicht zur Behebung des Problems können. |
|  4  | Aufrufen HcsSetupWizard: Wert liegt nicht innerhalb des erwarteten Bereichs. | Eine falsche Subnetz-Maske erzeugt diesen Fehler an. Zu den möglichen Ursachen sind: <ul><li> Das Eingabeformat Subnetz ist leer oder fehlt.</li><li>Das IPv6-Präfix Format ist falsch.</li><li>Die Benutzeroberfläche ist Cloud aktiviert, aber das Gateway ist fehlender oder falscher.</li></ul>Beachten Sie, dass Daten 0 automatisch Cloud-aktiviert, wenn Sie über den Setup-Assistenten konfiguriert ist. | Um das Problem zu ermitteln, verwenden Sie Subnetz 0.0.0.0 oder 256.256.256.256, und klicken Sie dann sehen Sie sich die Ausgabe. Geben Sie richtigen Werte für die Subnetz-Maske, Gateway und IPv6-Präfix, je nach Bedarf. |
 
## <a name="error-codes"></a>Fehlercodes

Fehler werden in der richtigen Reihenfolge aufgelistet.

|Fehlernummer|Fehlertext oder Beschreibung|Empfohlene Benutzeraktion|
|:---|:---|:---|
|10502|Fehler beim Zugriff auf Ihr Speicherkonto.|Warten Sie einige Minuten, und versuchen Sie es dann erneut. Wenn der Fehler auftritt, melden Sie Microsoft-Support wenden, für den nächsten Schritten fort.|
|40017|Die Sicherung Fehler wie ein in die Sicherung Richtlinie angegebenen Datenträger auf dem Gerät nicht gefunden wurde.|Wiederholen Sie die Sicherung Vorgang, wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. für die nächsten Schritte.|
|40018|Zusätzliche Fehler des wie keine in der Sicherungsdatei Richtlinie angegebenen Lautstärke auf dem Gerät gefunden wurden. |Wiederholen Sie die Sicherung Vorgang, wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. für die nächsten Schritte.|
|390061|Das System ist ausgelastet oder nicht verfügbar.|Warten Sie einige Minuten, und versuchen Sie es dann erneut. Wenn der Fehler auftritt, melden Sie Microsoft-Support wenden, für den nächsten Schritten fort.|
|390143|Fehler mit dem Fehlercode 390143. (Ein Fehler aufgetreten.)|Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort.|

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie zur Behebung des Problems, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um Unterstützung können nicht genutzt werden können. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx

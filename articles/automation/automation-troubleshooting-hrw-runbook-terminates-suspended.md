<properties
   pageTitle="Hybrid Runbooks Worker: Ein Auftrags Runbooks beendet wird mit dem Status angehalten | Microsoft Azure"
   description="Symptome Ursachen und Lösungen für Hybrid Runbooks Worker Auftrag Beendigung zurück."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbooks Worker: Ein Auftrags Runbooks beendet wird mit dem Status angehalten

## <a name="summary"></a>Zusammenfassung

Ihre Runbooks wird die App angehalten kurz nach dem Versuch, es dreimal auszuführen. Es gibt Geschäftsbedingungen Unterbrechen des Runbooks abgeschlossen werden kann, können und die zugehörigen Fehlermeldung keine zusätzlichen Informationen ein, der angibt, warum enthält. Dieser Artikel enthält Schritte zur Problembehandlung bei Problemen im Zusammenhang mit der Hybrid Runbooks Worker Runbooks Ausführungsfehlern.

Wenn Ihre Azure Problem nicht in diesem Artikel behandelt wird, besuchen Sie die Azure-Foren auf [MSDN und den Stapelüberlauf](https://azure.microsoft.com/support/forums/)ein. Sie können das Problem auf diese Foren oder zu Posten [ @AzureSupport auf Twitter](https://twitter.com/AzureSupport). Darüber hinaus können Sie eine Supportanfrage Azure-Datei, indem Sie auf der Website [Azure unterstützen](https://azure.microsoft.com/support/options/) **Unterstützung** auswählen.

## <a name="symptom"></a>Problem

Runbooks Ausführung fehlschlägt und der zurückgegebene Fehler ist, "das Projekt, was 'Aktivieren' ausgeführt werden kann, da der Prozess unerwartet beendet. Der Vorgang Auftrag wurde 3 Mal aufgetreten."


## <a name="cause"></a>Ursache

Es gibt verschiedene Ursachen für den Fehler: 

  1. Der Hybrid Worker befindet sich hinter dem Proxyserver oder Firewalls
  2. Der Computer, auf den die Hybrid Worker ausgeführt wird verfügt über weniger als die Hardware- [Anforderungen](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Die Runbooks kann nicht mit lokalen Ressourcen authentifizieren.


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Ursache 1: Hybrid Runbooks Worker befindet sich hinter dem Proxyserver oder einen firewall

Der Computer, auf den die Hybrid Runbooks Worker ausgeführt wird hinter einem Firewall oder des Proxyservers Server und ausgehende Netzwerkzugriff möglicherweise nicht zulässig oder nicht richtig konfiguriert ist.

### <a name="solution"></a>Lösung

Überprüfen des Computers hat ausgehenden Zugriff auf *. cloudapp.net Ports 443, 9354 und 30000-30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Ursache 2: Computer verfügt über weniger als Hardware-Mindestanforderungen

Die Hybrid Runbooks Worker-Computern sollte die Mindestanforderungen erfüllen, bevor Sie festlegen, um dieses Feature zu hosten. Andernfalls je nach Ressource Nutzung von anderen Hintergrundprozesse und Konflikten, die während der Ausführung von Runbooks verursacht, der Computer wird machen über ausgelastet ist und Runbooks Auftrag verzögert oder Zeitlimit. 

### <a name="solution"></a>Lösung 

Zuerst bestätigen Sie, dass der Computer, der das Feature Hybrid Runbooks Worker ausführen definiert die Mindestanforderungen erfüllt.  Ist dies der Fall, überwachen Sie CPU- und Auslastung Ermittlung eines Korrelationskoeffizienten zwischen die Leistung von Hybrid Runbooks Arbeitsprozesse und Windows.  Wenn Arbeitsspeicher oder CPU-Überlastung vorhanden ist, kann dies die Notwendigkeit aktualisieren oder hinzufügen zusätzliche Prozessoren oder vergrößern Arbeitsspeicher, um die Adresse der Ressourcenengpass und Beheben des Fehlers hindeuten. Alternativ Auswählen einer anderen berechnen Ressource, die die Mindestanforderungen und die Dezimalstellen unterstützen kann, wenn Arbeitsbelastung Abnahme darauf hinzuweisen, dass eine Steigerung benötigt wird.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Ursache 3: Runbooks kann nicht mit lokalen Ressourcen authentifizieren.

### <a name="solution"></a>Lösung

Aktivieren Sie das **Microsoft-SMA** Ereignisprotokoll für ein entsprechendes Ereignis mit *Win32-Prozess beendet mit Code [4294967295]*Beschreibung ein.  Die Ursache dieses Fehlers ist Sie noch nicht konfiguriert Authentifizierung in Ihrer Runbooks oder die Ausführen als Anmeldeinformationen für die Hybrid Worker Gruppe angegeben.  Aufforderung zur Überarbeitung [Runbooks Berechtigungen](automation-hybrid-runbook-worker.md#runbook-permissions) , um zu bestätigen, dass Sie Authentifizierung für Ihre Runbooks ordnungsgemäß konfiguriert haben.  


 


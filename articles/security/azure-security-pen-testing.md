<properties
   pageTitle="Testen der Stift | Microsoft Azure"
   description="Im Artikel bietet einen Überblick über den Einstieg Prozess (Pentest) testen und ausführen wie Pentest anhand Ihrer apps in Azure Infrastruktur ausgeführt."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Testen der Stift

Einer der großen Vorteile von mit Microsoft Azure für Test und Bereitstellung ist, dass Sie keinen aufzustellen eine lokale Infrastruktur zum Entwickeln, testen und Bereitstellen einer Anwendung müssen. Alle die Infrastruktur ist durch die Microsoft Azure-Plattformdienste durchgeführt. Sie müssen nicht zu den Fertigungszyklus, beim Abrufen, "Abstich und Stapeln" sorgen Ihrer eigenen lokalen Hardware.

Dies ist hervorragend – aber auch noch um sicherzustellen, dass Sie Ihre normale Sicherheit ausführen fristgerechte. Eines der Dinge, die Sie ausführen müssen Penetration ist die Anwendungen, die Sie bereitstellen in Azure testen.

Sie wissen möglicherweise bereits, dass Microsoft [Durchdringungstests unserer Azure Umgebung](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)ausführt. Dadurch wird uns unsere Plattform zu verbessern und unsere Aktionen im Hinblick auf Sicherheit Steuerelemente zu verbessern, Einführung in die neue Steuerelemente für Sicherheit und Verbesserung unserer Sicherheitsprozesse führt.

Wir nicht Stift testen Sie die Anwendung für Sie, aber wir verstehen, Stift Testen auf Ihrer eigenen Anwendung ausführen müssen und möchten. Das ist gut, da, wenn Sie die Sicherheit Ihrer Anwendung verbessern, Ihnen helfen Azure Teil des gesamten Netzwerks sicherer zu machen.

Wenn Sie den Stift Testen Ihrer Anwendung bereitgestellt werden, kann dies wie Angriffen uns aussehen. Wir [kontinuierlich überwachen](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) für Muster Angriffen und einen Vorfall Antwort-Prozess wird gestartet werden, wenn Sie zu Beginn müssen wir. Es wird nicht Ihnen helfen und es nicht helfen Sie uns, wenn wir Reaktion auf einen Vorfall aufgrund Ihrer eigenen Fälligkeitsdatum Diligence Stift Tests auslösen.

Was zu tun ist?

Wenn Sie bereit sind, Stift Testen Ihrer Anwendung Azure gehostete bereitgestellt, Sie lassen Sie uns wissen müssen. Wenn wir wissen, dass Sie mit der Ausführung der spezifische Tests werden vertraut sind, wird nicht wir versehentlich fahren Sie (beispielsweise Blockierung die IP-Adresse, der Sie aus testen sind), solange die Tests entsprechen den Azure Stift testen allgemeinen Geschäftsbedingungen.
Standard-Tests, die Sie ausführen können umfassen Folgendes:

- Tests auf Ihre Endpunkten in der [geöffneten Web Anwendung Sicherheit Project (OWASP) Top 10 Schwachstellen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) für eine Steigerung
- Die Endpunkte [Fuzztesten](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
- [Scannen von Ports](https://en.wikipedia.org/wiki/Port_scanner) , von Ihrer Endpunkte

Alle Arten von Angriffen [Dienstausfall (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) ist ein Typ Tests zurück, die Sie ausführen können. Dies umfasst eine DoS-Angriffen selbst initiieren oder zum Ausführen von verknüpften Tests, die möglicherweise zu ermitteln, führen Sie vor oder jede Art von DoS-Angriffen zu reproduzieren.

Prüfen Sie bereit sind, den ersten Schritten mit Stift Ihrer Anwendung in einem Microsoft Azure gehostet wird? Wenn dies der Fall ist, und klicken Sie dann am auf über in der [Penetration Test Übersicht](https://security-forms.azure.com/penetration-testing/terms) Seite (und klicken Sie auf die Schaltfläche Testen anfordern am unteren Rand der Seite erstellen. Außerdem finden Sie weitere Informationen zu den Stift testen Geschäftsbedingungen und hilfreiche Links auf, wie Sie Sicherheitsfehler im Zusammenhang mit Azure oder einen anderen Microsoft-Dienst Berichte erstellen können.

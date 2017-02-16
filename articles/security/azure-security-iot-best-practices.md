<properties
   pageTitle="Das Internet der Dinge bewährte Methoden für Sicherheit | Microsoft Azure"
   description="Der Artikel enthält eine Liste curated Microsoft Internet der Dinge bewährte Methoden für Sicherheit und allgemeine Empfehlungen."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Das Internet der Dinge bewährte Methoden für Sicherheit

Sichern der Infrastruktur Internet der Dinge (IoT) ist eine kritische Unternehmen für jeden die IoT Lösungen. Aufgrund der Anzahl der betreffenden Geräte und die verteilte Natur dieser Geräte die Auswirkungen ein Ereignisses Sicherheit von Millionen von Geräten IoT manipulieren Zusammenhang ist wichtige und weit verbreitet auswirken.

Aus diesem Grund benötigt IoT Sicherheit eine Sicherheit Verteidigungsstrategie. Daten in der Cloud secure werden müssen und wie es über private und öffentliche Netzwerke verschoben wird. Methoden direkte IoT Geräte selbst sicher bereitgestellt werden müssen. Jede Ebene von Gerät aus, in die cloud Back-End-Netzwerk benötigt Garantien sicherer Sicherheit.

Bewährte Methoden IoT können auf folgende Weise eingestuft werden:

- IoT Hardwarehersteller oder integrator
- IoT Lösungsentwickler
- IoT Lösung Bereitsteller
- IoT Lösung operator

Dieser Artikel enthält eine Übersicht über [Internet der Dinge bewährte Methoden für Sicherheit](../iot-suite/iot-security-best-practices.md). Lizenzinformationen finden Sie diesen Artikel Ausführlichere Informationen.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT Hardwarehersteller oder integrator

Führen Sie die folgenden bewährten Methoden, wenn Sie eine IoT Hardwarehersteller oder einem Integrator Hardware sind:

- **Bereich Hardware zu Mindestanforderungen**: Hardware-Design sollte enthalten minimale Features, die für den Vorgang an der Hardware und nichts mehr erforderlich ist. 
- **Stellen Nachweis manipulieren Hardware**: in Verfahren zur Erkennung physische Manipulation der Hardware, z. B., öffnen das Gerät Deckblatt, entfernen einen Teil des Geräts usw. erstellen. 
- **Erstellen, um sichere Hardware**: Wenn [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) zulässig sein soll, erstellen Sie Sicherheitsfeatures wie sichere und verschlüsselte Speicher und TPM Trusted Platform Module-basierte Start-Funktionalität.
- **Stellen Sie sicher Upgrades**: Firmware aktualisieren, während die Gültigkeitsdauer des Geräts unvermeidbare ist.

## <a name="iot-solution-developer"></a>IoT Lösungsentwickler

Führen Sie die folgenden bewährten Methoden, wenn Sie eine IoT Lösungsentwickler sind:

- **Folgen sicherer Software Development Methoden**: sicheren Software zur Entwicklung von Grund auf an, über die Sicherheit von den Beginn des Projekts bis ganz nach deren Implementierung, testen und Bereitstellen erfordert.
- **Wählen Sie open Source-Software Vorsicht**: open Source-Software bietet die Möglichkeit, schnell Lösungen entwickeln.
- **Integrieren Vorsicht**: zahlreiche die Software Sicherheitsfehler bei der Grenze für Bibliotheken und APIs vorhanden sind. 

## <a name="iot-solution-deployer"></a>IoT Lösung Bereitsteller

Führen Sie die folgenden bewährten Methoden, wenn Sie eine IoT Lösung Bereitsteller sind:

- **Bereitstellen von Hardware sicher**: IoT Bereitstellungen erfordern möglicherweise Hardware im unsicheren Speicherorten, z. B. in öffentlichen Leerzeichen oder unbeaufsichtigt Gebietsschemas bereitgestellt werden.
- **Schützen Sie Authentifizierung Tasten**: während der Bereitstellung jedem Gerät erfordert Geräte-IDs und die zugehörigen Authentifizierung Tasten durch die Cloud-Dienst generiert. Schützen Sie diese Schlüssel physisch auch nach der Bereitstellung. Alle betroffenen Schlüssel kann von einem bösartiger Gerät um zu ausgeben als ein vorhandenes Gerät verwendet werden.

## <a name="iot-solution-operator"></a>IoT Lösung operator

Führen Sie die folgenden bewährten Methoden, wenn Sie einen IoT Lösung Operator sind:

- **Systeme auf dem neuesten Stand zu bleiben**: Stellen Sie sicher, Gerätebetriebssysteme und alle Gerätetreiber auf die neuesten Version aktualisiert werden. 
- **Schutz vor bösartiger Aktivität**: Wenn das Betriebssystem zulässt, setzen Sie die neuesten Funktionen von Anti-Virus und Anti-Malware auf jedem Gerätebetriebssystem. 
- **Audit häufig**: Überwachung von IoT Infrastruktur für Sicherheits Probleme ist Schlüssel aus, wenn auf Sicherheitsvorfälle reagiert.
- **Schützen Sie die Infrastruktur IoT physisch**: die schlechtesten Angriffen gegen IoT Infrastruktur werden physische Zugriff auf Geräte mit gestartet.
- **Schützen Cloud Anmeldeinformationen**: Cloud Authentifizierungsinformationen für das Konfigurieren und Betrieb einer bereitstellungs IoT verwendet werden möglicherweise die einfachste Möglichkeit zum zugreifen und einem IoT System zu beeinträchtigen. 

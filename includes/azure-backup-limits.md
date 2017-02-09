 (Sicherung Depots<properties
   Seitentitel = "Azure Backup limits table"
   Beschreibung = "System Grenzwerte für werden Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Die folgenden Grenzwerte gelten für Azure sichern.

| Grenzwert für Bezeichner | Standardmäßige Grenzwert |
|---|---|
|Anzahl von Servern/Maschinen, die für jeden Tresor registriert werden kann|50 für Windows Server/Client/SCDPM <br/> 200 für IaaS virtuellen Computern|
|Größe des als Datenquelle für die in Azure Tresor Speicher gespeicherten Daten|54400 GB max<sup>1</sup>|
|Anzahl der Sicherungsdatei Depots, die in jedem Azure Abonnement erstellt werden können|25 (Sicherung Depots) <br/> 25 Wiederherstellung Services Vaulting pro region|
|Wie oft die Sicherung pro Tag geplant werden können|3 pro Tag für Windows Server-Client <br/> 2 pro Tag für SCDPM <br/> Einmal täglich für IaaS virtuellen Computern|
|Mit einer Azure-virtuellen Computern für die Sicherung verbundenen Daten-Datenträger|16|

- <sup>1</sup> Die 54400 GB Beschränkung gilt nicht für IaaS VM sichern.


<properties
    pageTitle="Azure Government Dokumentation | Microsoft Azure"
    description="Dies stellt einen Vergleich der Features und Hinweise zur Entwicklung von Applications für Azure Government"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Berechnen von Azure Government

##  <a name="virtual-machines"></a>Virtuellen Computern

Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung finden Sie unter [Azure-virtuellen Computern Größen](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variationen

Die folgenden virtuellen Computer SKUs sind in Azure Government allgemein verfügbar (GA):

VIRTUELLER COMPUTER SKU|US Gov VA|US Gov IA|Notizen
---|---|---|---
A|GA|GA|Keine
Dv1|GA|-|Keine
DSv1|GA|-|Keine
Dv2|GA|GA|15 ist bald verfügbar.
F|GA|GA|Keine
G|Geplanter|-|Keine

###  <a name="data-considerations"></a>Aspekte der Daten

Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government für Azure virtuellen Computern an:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daten eingegeben haben, gespeichert und verarbeitet innerhalb ein virtuellen Computers kann gesteuert exportieren Daten enthalten. Binärdateien in Azure virtuellen Computern ausgeführt werden. Statische Authentifikatoren, z. B. Kennwörter und Smartcard-PIN für den Zugriff auf Azure-Plattformkomponenten. Private Schlüssel von Zertifikaten Azure-Plattformkomponenten zur Verwaltung verwendet wird. SQL-Verbindungszeichenfolgen.  Anderen Sicherheit Informationen/vertraulichen, z. B. Zertifikate, Schlüssel für die Verschlüsselung, Master Tasten und Speicher Tasten in Azure Services gespeichert.  | Metadaten darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten von Ihrer Azure-virtuellen Computern eingegeben.  Geben Sie in die folgenden Felder nicht Regulated/gesteuert Daten: Mandanten Rolle, Ressourcengruppen, Bereitstellung Namen, Ressourcennamen, Resource-Tags  

## <a name="next-steps"></a>Nächste Schritte

Abonnieren Sie zusätzliche Informationen und Updates, die <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>

<properties 
    pageTitle="Zur Lizenzierung Microsoft® interpolierten Streaming-Client Portieren Kit" 
    description="Informationen zur Lizenzierung von Microsoft® interpolierten Streaming Client Portieren Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Zur Lizenzierung Microsoft® interpolierten Streaming-Client Portieren Kit

##<a name="overview"></a>(Übersicht)

Microsoft interpolierten Streaming Client Portieren Kit (**SSPK** kurz) ist eine reibungslose Streaming Client-Implementierung, die eingebettete Gerätehersteller, Kabel und Mobilfunkanbieter, Inhalte Dienstanbieter, Telefonhörer Hersteller, unabhängigen Softwareanbietern und Lösungsanbieter zum Erstellen von Produkten und Diensten für streaming adaptive streaming von Inhalten in interpolierten Streaming Format helfen optimiert ist. SSPK ist eine unabhängige Implementierung Gerät und Plattform interpolierten Streaming-Client, der vom Lizenznehmer zu einem beliebigen Gerät Plattform portiert werden kann. 

Nachstehend ist eine Architektur auf hoher Ebene und IIS interpolierten Streaming Portieren Kit ist die interpolierten Streaming Client-Implementierung von Microsoft bereitgestellt und umfasst die grundlegende Logik für die Wiedergabe von Inhalten interpolierten Streaming. Dies ist dann durch die entsprechende Schnittstellen Implementierung von Partnern für ein bestimmtes Gerät oder Plattform portiert. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Beschreibung

SSPK wird die hervorragende Business Wert anbieten Ausdrücke lizenziert. SSPK-Lizenz bietet die Branche mit:

- Interpolierten Portieren Kit Streaming Quelle in C++ 
  - implementiert interpolierten Streaming-Client-Funktion
  - Analyse, Heuristik, Pufferung Logik usw. Format hinzugefügt.
- Player-Anwendung APIs 
  - Programming Interface für die Interaktion mit einem Media Player-Anwendung
- Schnittstelle Plattform Layer (PAL) 
  - Programming Schnittstellen für die Interaktion mit dem Betriebssystem (Threads, Sockets)
- Hardware (Layer, HAL) Schnittstelle 
  - Programming Schnittstellen für die Interaktion mit Hardware A / V-Decodern (Decodierung; Rendern)
- Digital Rights Management (DRM)-Oberfläche 
  - Programming Interface für den Umgang mit DRM durch DRM Abstraction Layer (DAL)
  - Microsoft PlayReady Portieren Kit ausgeliefert getrennt wird jedoch über diese Schnittstelle integriert. Weitere Informationen zur Lizenzierung von Microsoft PlayReady Device finden Sie [hier](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Implementierung Beispiele 
  - Beispiel für PAL Implementierung für Linux
  - Beispiel für HAL Implementierung für GStreamer

##<a name="licensing-options"></a>Lizenzierungsoptionen

Microsoft interpolierten Streaming Client Portieren Kit wird zur Verfügung gestellt Lizenznehmern unter zwei unterschiedlichen Lizenzvertrag: eine für die Entwicklung von interpolierten Streaming Client Zwischenzeit Produkte und einen anderen für interpolierten Streaming Client abgeschlossen-Produkten für den Endbenutzer verteilen.
 
- Für Chipsatzhersteller, Systemintegratoren oder unabhängigen Softwareanbietern, die eine Quellcode Portieren Kit zum Entwickeln Zwischenzeit Produkte benötigen, sollten Microsoft interpolierten Streaming Client Portieren Kit **Zwischenzeit-Lizenz** ausgeführt werden.
- Gerätehersteller oder ISVs, die Verteilungsrechte bei interpolierten Streaming Client abgeschlossen-Produkten für den Endbenutzer benötigen, sollte der Microsoft interpolierten Streaming Client Portieren Kit **Abgeschlossen-Lizenz** ausgeführt werden.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft-optimierten Streaming Client Portieren Kit Zwischenzeit-Lizenz

Klicken Sie unter diese Lizenz bietet Microsoft ein interpolierten Streaming Client Portieren Kit und das notwendigen geistige Eigentum zu entwickeln und Verteilen von interpolierten Streaming Client Zwischenzeit Produkte an andere interpolierten Streaming Client Portieren Kit Gerät Lizenznehmern, die interpolierten Streaming Client abgeschlossen-Produkte verteilen.

####<a name="fee-structure"></a>Gebührenstruktur

Eine 50.000 einmalige Lizenzgebühr bietet Zugriff auf die interpolierten Streaming Client Portieren Kit. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft-optimierten Streaming Client Portieren Kit abgeschlossen-Lizenz

Klicken Sie unter diese Lizenz bietet Microsoft alle notwendigen geistige Eigentum interpolierten Streaming Client Zwischenzeit Produkte aus anderen Lizenznehmern interpolierten Streaming Client Portieren Kit erhalten und Unternehmen Marke interpolierten Streaming Client abgeschlossen-Produkte für den Endbenutzer verteilen.

####<a name="fee-structure"></a>Gebührenstruktur

Die interpolierten Streaming Client Endprodukts wird im Rahmen eines Modells Royalty als unter angeboten:

- $0.10 pro Gerät Implementierung geliefert
- Die Royalty wird jedes Jahr am 50.000 US-Dollar Obergrenze.
- Keine Lizenz für die ersten 10.000 Gerät Implementierungen jedes Jahr 

##<a name="licensing-procedure-and-sspk-access"></a>Verfahren zur Lizenzierung und SSPK Zugriff

Senden Sie eine e-Mail [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) für alle Lizenzierung Abfragen.

Im [Portal SSPK Verteilung](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) zugegriffen werden registrierte Zwischenzeit Lizenznehmern zu.

Zwischenplan und endgültige SSPK Lizenznehmern können technische Fragen zum Übermitteln [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft-optimierten Client zwischenzeitlichen Produkt Vertrag Lizenznehmern Streaming

- Adroit Business Solutions, Inc.
- Erweiterte digitalen übertragen SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Alticast Unternehmen
- Amazon digitalen Services, Inc.
- AVC Multimedia Software Co., Ltd.
- Cavium, Inc.
- EchoStar erwerben Unternehmen
- Enseo, Inc.
- Fluendo SA
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Umfassen globale Services BW
- MediaTek Inc.
- MStar Co Ltd.
- Nintendo Co., Ltd.
- OpenTV, Inc.
- Safran digitalen Beschränkung
- Sichuan Changhong Plan für Elektrik Co. Ltd.
- SoftAtHome
- Sony Unternehmen
- Tatung Technology Inc.
- TCL Technoly Electronics (Huizhou) Co., Ltd.
- Vestel Elektronik Sanayi speichern Ticaret A.S.
- VisualOn, Inc.
- ZTE Unternehmen

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft-optimierten Client Endprodukts Vertrag Lizenznehmern Streaming

- Erweiterte digitalen übertragen SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Amazon digitalen Services, Inc.
- AmTRAN Technology Co., Ltd.
- Arcadyan Technologie Unternehmen
- ATMACA ELEKTRONİK SAN. TİC ZU SPEICHERN. A.Ş
- British Sky Übertragung Beschränkung
- CastPal Technology Inc., Shenzhen
- Compal Electronics, Inc.
- Dongguan digitalen AV Technology Corp., Ltd.
- EchoStar erwerben Unternehmen
- Enseo, Inc.
- Filmflex Filme Beschränkung
- Fluendo SA
- Gibson Innovationen Beschränkung
- Haier Informationen Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co. Ltd.
- Hon Hai Genauigkeit Industry Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- Unternehmen KDDI
- Nintendo Co., Ltd.
- Orange SA
- Safran digitalen Beschränkung
- Sagemcom Breitbandnetzen SAS
- Shenzhen Coship Electronics CO. Ltd.
- Shenzhen Jiuzhou Plan für Elektrik Co. Ltd.
- Shenzhen Skyworth digitalen Technologie Co., Ltd.
- Sichuan Changhong Plan für Elektrik Co., Ltd.
- Skardin Industrie Corp.
- Sky Deutschland Fernsehen GmbH & Co KG
- SmarDTV SA
- SoftAtHome
- Sony Unternehmen
- Eingeschränkte TCL überseeischen Marketing (Macau kommerzielle Offshore)
- Farbenfrohen Übermittlung Technologien SAS
- Tongfang globale Ltd.
- Toshiba Leben Produkte und Dienstleistungen Unternehmen
- Universeller Medien Unternehmen /Slovakia/ s.r.o.
- VIZIO, Inc.
- Wistron Unternehmen
- ZTE Unternehmen

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

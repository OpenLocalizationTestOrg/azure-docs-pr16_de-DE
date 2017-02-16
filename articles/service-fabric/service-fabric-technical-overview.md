<properties
   pageTitle="Übersicht über die Terminologie Dienst Fabric | Microsoft Azure"
   description="Eine Übersicht über die Terminologie Dienst Stoffbahn. Erläutert wichtige Terminologie Konzepte und Begriffe, die in den Rest der Dokumentation verwendet werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="chackdan;subramar"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="service-fabric-terminology-overview"></a>Übersicht über die Terminologie Fabric Service

Dienst ist, ist eine verteilte Systeme-Plattform, die Verpacken, bereitstellen und Verwalten von skalierbar und zuverlässig Microservices erleichtert. In diesem Thema werden die Terminologie Dienst Fabric akzeptieren, um die Begriffe in der Dokumentation verwendet werden.

## <a name="infrastructure-concepts"></a>Infrastruktur Konzepte
**Cluster**: eine Verbindung mit dem Netzwerk Reihe von virtuellen oder physischen Maschinen, in dem Ihre Microservices bereitgestellt und verwaltet werden.  Cluster können Tausende von Autos skalieren.

**Knoten**: einen Computer oder virtueller Computer, die Teil eines Cluster ist, wird als Knoten bezeichnet. Jeder Knoten ist ein Knotenname (einer Zeichenfolge) zugewiesen. Knoten verfügen über Eigenschaften wie Platzierungseigenschaften. Jeder Computer oder in virtuellen Computer ist einen Windows-Dienst automatisch gestartet werden, `FabricHost.exe`, die beim Starten ausgeführt und dann beginnt zwei ausführbare Dateien: `Fabric.exe` und `FabricGateway.exe`. Diese zwei Programmdateien bilden zusammen den Knoten. Zum Testen von Szenarien, können Sie mehrere Knoten auf einem einzelnen Computer oder virtueller Computer hosten, indem Sie mehrere Instanzen des ausführen `Fabric.exe` und `FabricGateway.exe`.

## <a name="application-concepts"></a>Anwendungskonzepte
**Anwendungstyp**: der Name/Version, die eine Auflistung von Diensttypen zugewiesen ist. Im Sinne einer `ApplicationManifest.xml` Datei eingebettet in ein Paketverzeichnis Anwendung, die dann auf den Dienst Fabric-Cluster Bild Store kopiert wird. Sie können eine benannte Anwendung dann aus dieser Anwendungstyp im Cluster erstellen.

Lesen Sie den [Anwendungsmodell](service-fabric-application-model.md) Artikel Weitere Informationen.

**Paket Anwendung**: Datenträger Verzeichnis mit des Anwendungstyps `ApplicationManifest.xml` Datei. Verweist auf den Service-Paketen für jeden Dienst, der den Anwendungstyp bildet. Die Dateien im Verzeichnis Paket Anwendung werden zum Dienst Fabric-Cluster Bild Store kopiert. Beispielsweise könnten ein Anwendungspaket für einen Anwendungstyp e-Mail-Verweise auf ein Warteschlange-Service-Paket, ein Front-End-Service-Paket und eine Datenbank Service-Paket enthalten.

**Mit dem Namen der Anwendung**: nach dem Kopieren eines Anwendungspakets auf das Bild Store, erstellen Sie eine Instanz der Anwendung im Cluster durch die Anwendungspaket Anwendungstyp (deren Namen-Version mit) angeben. Jede Instanz der Anwendung Typ zugeordnet ist ein URI-Name, der wie folgt aussieht: `"fabric:/MyNamedApp"`. In einem Cluster können Sie mehrere benannte Applications aus einem einzelnen Anwendungstyp erstellen. Sie können auch benannte Applications aus anderen Anwendungstypen erstellen. Jede benannte Anwendung wird unabhängig verwalteten und welche Version.      

**Diensttyp**: der Name/Version des Diensts Code-Paketen, Data Pakete und Konfigurationspakete zugewiesen. Definierte in einer `ServiceManifest.xml` Datei eingebettet in ein Paket Dienst und der Dienst Paketverzeichnis dann optimiert Paket eine Anwendung `ApplicationManifest.xml` Datei. Innerhalb der Cluster können nach dem Erstellen einer benannten Anwendungs, Sie einen benannten Dienst von einem der Anwendungstyp Service-Typen erstellen. Des Diensttyps `ServiceManifest.xml` Datei beschreibt den Dienst.

Lesen Sie den [Anwendungsmodell](service-fabric-application-model.md) Artikel Weitere Informationen.

Es gibt zwei Arten von Diensten aus:

- **Statusfreie:** Verwenden Sie einen statusfreien Dienst aus, wenn beständigen Dienststatus in einer externen Speicherdienst beispielsweise Azure-Speicher, Azure SQL-Datenbank oder DocumentDB Azure gespeichert ist. Verwenden Sie einen statusfreien Dienst aus, wenn der Dienst überhaupt keine permanenten Speicher hat. Beispielsweise einem Taschenrechner Webdienst where Werte an den Dienst übergeben werden, eine Berechnung mit diesen Werten ausgeführt wird und ein Ergebnis wird zurückgegeben.

- **Stateful:** Verwenden Sie eine dynamische Service, wenn Sie Dienst-Struktur bis Dienststatus über deren zuverlässigen Websitesammlungen oder zuverlässigen Akteuren programming Modelle verwalten möchten. Angeben wie viele Partitionen zu Ihrem Zustand über (für Skalierbarkeit) verteilt beim Erstellen eines benannten Diensts. Geben Sie auch an, wie oft Ihr Bundesland über Knoten (für Zuverlässigkeit) repliziert. Jeder benannte Dienst verfügt über eine einzelne primäres Replikat und der mehrere sekundäre. Sie ändern Ihre benannten Dienststatus an die primäre schreiben. Dienst Fabric repliziert dieser Status klicken Sie dann auf alle sekundären Replikate Synchronisieren von Ihrem Zustand. Dienst Fabric erkennt automatisch auf, wenn ein primäres Replikat schlägt fehl, und höher eine vorhandene sekundäre Replikation an einen Primärschlüssel gestuft. Dienst Fabric erstellt dann eine neue-sekundäre-Replikation.  

**Service-Paket**: ein Verzeichnis mit des Diensttyps `ServiceManifest.xml` Datei. Diese Datei verweist auf die Code, statische Daten und Konfigurationspakete für den Diensttyp. Die Dateien im Verzeichnis Paket Dienst nach des Anwendungstyps verwiesen werden `ApplicationManifest.xml` Datei. Beispielsweise könnten ein Service-Paket schlagen Sie in den Code, statische Daten und Konfigurationspakete, aus denen ein Datenbankdienst besteht.

**Mit der Bezeichnung Dienst**: nach dem Erstellen einer benannten Anwendungs, können Sie eine Instanz eines seiner Dienst Typen innerhalb der Cluster erstellen, indem Sie den Diensttyp (deren Namen-Version mit) angeben. Jede Instanz der Dienst Typ wird unter der benannten Anwendung URI ausgelegte Namen URI zugewiesen. Beispielsweise, wenn Sie eine "Meine Datenbank" mit dem Namen Service innerhalb einer Anwendung mit der Bezeichnung "MyNamedApp" erstellen, der URI sieht wie folgt aus: `"fabric:/MyNamedApp/MyDatabase"`. Innerhalb einer benannten Anwendung können Sie mehrere benannte Dienste erstellen. Jede benannte Dienst kann eine eigene Partitionsschema und Instanz/Replikat ermittelt.

**Code-Paket**: Verzeichnis Datenträger, die den Diensttyp ausführbare Dateien (normalerweise EXE/DLL-Dateien) enthält. Die Dateien im Verzeichnis mit Paket durch des Diensttyps verwiesen werden `ServiceManifest.xml` Datei. Beim Erstellen ein benannter Dienst wird das Paket Code in ausgewählten zum Ausführen des Diensts benannten einen oder mehrere Knoten kopiert. Klicken Sie dann startet der Code ausführen. Es gibt zwei Arten von Code Paket ausführbare Dateien aus:

- **Gast Programmdateien**: ausführbare Dateien, die als ausgeführt werden – wird auf dem Host-Betriebssystem (Windows oder Linux). D. h., diese Programmdateien nicht link zu oder Dienst Fabric Laufzeitdateien verweisen und daher kann keine Dienst Fabric programming Modellen verwenden. Diese Programmdateien können nicht auf einige Dienst Fabric Features wie naming Service für die Ermittlung der Endpunkt verwenden. Gast ausführbare Dateien können nicht jede Dienstinstanz laden Kennzahlen bestimmten Bericht.

- **Dienst Host Programmdateien**: ausführbare Dateien, Dienst Fabric programming Modelle durch Verknüpfen von Service Fabric Runtime-Dateien, aktivieren Sie Dienst Fabric-Funktionen verwenden. Beispielsweise eine Dienstinstanz der benannten kann Endpunkte mit Service-Fabric Naming Service registrieren, und Laden Kennzahlen kann auch Berichte.      

**Paket mit Daten**: ein Verzeichnis mit den Diensttyp statische, schreibgeschützte Datendateien (normalerweise Foto, Audio und video). Die Dateien in das Paket Datenverzeichnis durch des Diensttyps verwiesen werden `ServiceManifest.xml` Datei. Beim Erstellen ein benannter Dienst wird an einen oder mehrere Knoten ausgewählt, die zum Ausführen der benannten Paket mit den Daten kopiert.  Der Code wird automatisch ausgeführt und kann nun auf die Datendateien zugreifen.

**Konfigurations-Paket**: Verzeichnis Datenträger, die den Diensttyp statische, schreibgeschützte Konfigurationsdateien (normalerweise Text) enthält. Die Dateien im Verzeichnis Paket Konfiguration durch des Diensttyps verwiesen werden `ServiceManifest.xml` Datei. Beim Erstellen ein benannter Dienst werden die Dateien in das Konfigurationspaket in ausgewählten zum Ausführen des Diensts benannten einen oder mehrere Knoten kopiert. Klicken Sie dann der Code wird gestartet und kann nun auf die Konfigurationsdateien zugreifen.

**Partitionsschema**: beim Erstellen eines benannten Diensts Sie ein Partitionsschema angeben. Services mit großen Mengen von Bundesstaat teilen die Daten partitionsübergreifend, die sie über die Cluster Knoten Doppelseite. Dadurch wird dem benannten Dienst Zustand zu skalieren. Innerhalb einer Partition besitzen zustandsloser benannten Dienste Instanzen, dynamische benannten Services Replikate. In der Regel verfügen zustandsloser benannten Dienste immer nur eine Partition da er keine internen Status haben. Die Partition Instanzen bieten für Verfügbarkeit; Wenn nur eine Instanz fehlschlägt, anderen Instanzen weiterhin normal ausgeführt werden und Dienst Fabric erstellen Sie dann eine neue Instanz. Stateful namens Services verwalten, deren Status innerhalb Replikate und jede Partition hat einen eigenen Replikatgruppe mit dem Status, die für die synchron gehalten werden. Ein Replikat fehlschlägt, erstellt Fabric Service ein neues Replikat aus vorhandenen Replikationen.

Im Artikel [Partition Dienst Fabric zuverlässigen Services](service-fabric-concepts-partitioning.md) für Weitere Informationen zu lesen.

## <a name="system-services"></a>Systemdienste
Es gibt Dienste aufgeführt, die in jeder Cluster erstellt werden, die die Plattformfunktionen Stoffbahn Dienst bereitstellen.

**DNS-Dienst**: jeder Dienst Fabric Cluster weist einen Naming-Dienst, der Dienstnamen an einem Speicherort im Cluster aufgelöst wird. Verwalten von Dienstnamen und Eigenschaften, ähnlich wie mit einer Internet Service DNS (Domain Name) für den Cluster. Clients kommunizieren sicher mit einem beliebigen Knoten im Cluster mithilfe von Service benennen einen Dienstnamen und die Position auflösen.  Clients erhalten die ist-Computer IP-Adresse und den Port, in dem sie derzeit ausgeführt wird. Sie können die Dienste und Clients zum Auflösen von der aktuellen Netzwerkspeicherort trotz Applikationen verschobene innerhalb des Clusters beispielsweise aufgrund von Fehlern, einer Ressource Lastenausgleich oder das Ändern der Größe der Cluster zur entwickeln.

Lesen Sie weitere Informationen über den Client [kommunizieren mit den Diensten](service-fabric-connect-and-communicate-with-services.md) und service Kommunikation APIs, die mit dem Dienst Naming zusammenarbeiten.

**Abbildung des Store Service**: jeder Dienst Fabric Cluster verfügt über eine Abbildung Speicherdienst, in dem bereitgestellten, Versionsnummern Anwendungspakete gespeichert werden. Kopieren eines Anwendungspakets zum Bild Store, und den in dieser Anwendungspaket enthaltenen Anwendungstyp registrieren. Nach der Anwendungstyp bereitgestellt wird, erstellen Sie es einer benannten Applications. Nachdem alle zugehörigen benannten Applikationen gelöscht wurden, können Registrierung einen Anwendungstyp aus der Abbildung Speicherdienst aufheben.

Lesen Sie den Artikel [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md) für Weitere Informationen zum Bereitstellen von Applications in der Abbildung Store Service.

## <a name="built-in-programming-models"></a>Integrierte programming Datenmodellen
.NET Framework Programmierung Modelle stehen für Sie zur Erstellung von Diensten Fabric Service zur Verfügung:

**Zuverlässigen Services**: ein API zustandslosen und dynamische Dienste zu erstellen. Dynamische Service gespeichert, deren Status in zuverlässigen Websitesammlungen (beispielsweise ein Wörterbuch oder eine Warteschlange). Sie erhalten auch in einer Vielzahl von Communication Stapel wie Web-API und Windows Communication Foundation (WCF) zu schließen.

**Zuverlässigen Akteuren**: ein API zustandslosen und dynamische Objekte durch das virtuelle programming Akteur-Modell zu erstellen. Dieses Modell kann hilfreich sein, wenn Sie viele unabhängige Einheiten der Berechnung Bundesstaat/haben. Da dieses Modell ein aktivieren-basierten Threadmodell verwendet wird, empfiehlt es sich, Code, die zu anderen Akteuren oder Dienste, da ein einzelner Akteur anderen eingehenden Anfragen verarbeitet werden kann, bis alle ausgehenden Anfragen abgeschlossen haben Anrufe zu vermeiden.

Lesen Sie weitere Informationen im Artikel [Auswählen eines Modells Programming des Diensts](service-fabric-choose-framework.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Fabric Dienst:

- [Übersicht über Fabric Service](service-fabric-overview.md)
- [Warum eine Microservices Ansatz zum Erstellen von Clientanwendungen?](service-fabric-overview-microservices.md)
- [Anwendungsszenarien](service-fabric-application-scenarios.md)

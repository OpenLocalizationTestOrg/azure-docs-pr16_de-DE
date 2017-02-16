<properties
   pageTitle="Allgemeine FabricClient Ausnahmen | Microsoft Azure"
   description="Beschreibt die allgemeine Ausnahmen und Fehler, die durch die APIs FabricClient beim Ausführen der Anwendung und Cluster Management Vorgänge ausgelöst werden können."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Allgemeine Ausnahmen und Fehler bei der Arbeit mit den FabricClient-APIs
Die [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -APIs können Cluster und Anwendung Administratoren Verwaltungsaufgaben auf einem Fabric Service-Anwendung, Dienst oder Cluster. Beispielsweise Bereitstellung der Anwendung, Upgrade und entfernen, aktivieren die Integrität einer Cluster oder einen Dienst testen. Entwickler und Clusteradministratoren können die APIs FabricClient Tools für die Verwaltung der Dienst Fabric Cluster und Anwendungen entwickeln.

Es gibt viele verschiedene Typen von Vorgänge, die mit FabricClient ausgeführt werden können.  Jede Methode kann Ausnahmen für Fehler aufgrund fehlerhafte Eingabe, Laufzeitfehler oder vorübergehende Infrastrukturprobleme lösen.  Finden Sie in der Dokumentation API Bezug zum Suchen nach einer bestimmten Methode welche Ausnahmen ausgelöst werden. Es gibt einige Ausnahmen, jedoch die von vielen verschiedenen [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) APIs ausgelöst werden können. Die folgende Tabelle enthält die Ausnahmen, die für die APIs FabricClient sind.

|Ausnahme| Wird ausgelöst, wenn|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Das Objekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) ist im geschlossenen Zustand. Löschen Sie das Objekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) , das Sie ein neues [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) Objekt instanziieren und verwenden. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Der Vorgang überschritten. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) wird zurückgegeben, wenn die Operation mehr als MaxOperationTimeout abgeschlossen hat.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Fehler bei die Überprüfung Access für den Vorgang. E_ACCESSDENIED zurückgegeben.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Beim Ausführen des Vorgangs ist ein Laufzeitfehler aufgetreten. Eines der FabricClient Verfahren können potenziell [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)auslösen, die Eigenschaft [Fehlercode](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) zeigt die genaue Ursache der Ausnahme. Fehlercodes werden in der [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) -Enumeration definiert.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Der Vorgang ist aufgrund einer vorübergehenden Fehler Bedingung irgendeines fehlgeschlagen. Angenommen, möglicherweise ein Vorgang fehl, weil ein Quorum von Replikaten vorübergehend nicht erreichbar ist. Vorübergehende Ausnahmen entsprechen fehlgeschlagene Vorgänge, die wiederholt werden können.|

Einige häufige [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) Fehler, die in einem [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)zurückgegeben werden können:

|Fehler| Bedingung|
|---------|:-----------|
|CommunicationError|Ein Kommunikationsfehler verursacht den Vorgang an ein Fehler auftreten, wiederholen Sie den Vorgang.|
|InvalidCredentialType|Der Anmeldeinformationstyp sind ungültig.|
|InvalidX509FindType|Die X509FindType ist ungültig.|
|InvalidX509StoreLocation|Die X509 Speicherort ist ungültig.|
|InvalidX509StoreName|Die X509 Store Name ist ungültig.|
|InvalidX509Thumbprint|Die X509 Zertifikat Fingerabdruck Zeichenfolge ist ungültig.|
|InvalidProtectionLevel|Die Schutzebene ist ungültig.|
|InvalidX509Store|Die X509 Zertifikatspeicher kann nicht geöffnet werden.|
|InvalidSubjectName|Der Betreff Name ist ungültig.|
|InvalidAllowedCommonNameList|Das Format der allgemeinen Namen Liste Zeichenfolge ist ungültig. Sie sollten eine kommagetrennte Liste ein.|

<properties
   pageTitle="Zugreifen auf Azure-virtuellen Computern vom Server-Explorer | Microsoft Azure"
   description="Abrufen eine Übersicht zum Anzeigen erstellen und Verwalten von Azure-virtuellen Computern (virtuellen Computern) im Server-Explorer in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Zugreifen auf Azure-virtuellen Computern vom Server-Explorer

Mithilfe der Server-Explorer in Visual Studio können Sie Informationen zu Ihren virtuellen Computern von Azure gehostet anzeigen.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Zugreifen auf virtuellen Computern im Server-Explorer

Wenn Sie nach Azure gehosteten virtuellen Computer verfügen, können Sie im Server-Explorer zugreifen. Sie müssen zuerst zu Ihrem Abonnement Azure melden Sie sich bei Ihrem mobilen Dienste anzeigen. Melden Sie sich, öffnen das Kontextmenü für den Azure-Knoten im Server-Explorer, und wählen Sie die **Verbindung herstellen mit Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Um Informationen zu Ihren virtuellen Computern erhalten

1. Auswählen eines virtuellen Computers im Server-Explorer, und wählen Sie F4, um deren Eigenschaftenfenster anzeigen.

    Die folgende Tabelle zeigt, welche Eigenschaften verfügbar sind, aber sie sind alle schreibgeschützt. Wenn sie ändern möchten, verwenden Sie die [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus.

  	|Eigenschaft|Beschreibung|
  	|---|---|
  	|DNS-Name|Die URL für die Internetadresse des virtuellen Computers.|
  	|Umgebung|Für einen virtuellen Computer ist der Wert dieser Eigenschaft immer Herstellung aus.|
  	|Namen|Der Name des virtuellen Computers.|
  	|Größe|Die Größe des virtuellen Computers, was die Menge von Arbeitsspeicher und Speicherplatz widerspiegelt, die zur Verfügung. Weitere Informationen finden Sie unter How To: Konfigurieren von virtuellen Computern Größen.|
  	|Status|Werte einzubeziehen Felder starten, Schritte, beenden, beendet und Status abrufen. Wenn der Status abrufen von angezeigt wird, ist der aktuelle Status unbekannt. Die Werte für diese Eigenschaft unterscheiden sich von den Werten, die in [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwendet werden.|
  	|SubscriptionID|Die Abonnement-ID für Ihr Konto Azure. Anzeigen der Eigenschaften für ein Abonnement können Sie diese Informationen im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885) anzeigen.|

1. Wählen Sie einen Endpunktknoten, und klicken Sie dann zeigen Sie im Fenster **Eigenschaften an** .

1. Die folgende Tabelle beschreibt die verfügbaren Eigenschaften von Endpunkten, aber sie sind schreibgeschützt. Verwenden Sie zum Hinzufügen oder bearbeiten die Endpunkte für einen virtuellen Computer, der [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus. 

  	|Eigenschaft|Beschreibung|
  	|---|---|
  	|Namen|Ein Bezeichner für den Endpunkt.|
  	|Port "Privat"|Der Port für den Netzwerkzugriff internen an Ihrer Anwendung.|
  	|Protokoll|Das Protokoll, das der Transport Layer für diesen Endpunkt verwendet TCP oder UDP.|
  	|Öffentlicher Port|Der Port, der für den öffentlichen Zugang an Ihrer Anwendung verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure Rollen in Visual Studio finden Sie unter [Verwenden von Remotedesktop mit Azure Rollen](vs-azure-tools-remote-desktop-roles.md).

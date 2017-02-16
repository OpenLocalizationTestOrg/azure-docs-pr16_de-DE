<properties
    pageTitle="Ziehen von öffentlichen Daten in Azure Ereignis Hubs | Microsoft Azure"
    description="Übersicht über den Ereignis untergeordneten Servern Importieren von Webbeispiel"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Ziehen von öffentlichen Daten in Azure Ereignis Hubs

In normalen Szenarien Internet der Dinge (IoT) müssen Sie die Geräte, die Sie zu Pushbenachrichtigungen Daten zum Azure, entweder auf ein Ereignis Azure-Hub oder einen Hub IoT Programmierung verwenden können. Beide diese Hubs sind in Azure Felder und Optionen für das Speichern, analysieren und mit einer Vielzahl von Tools zur Verfügung gestellt, klicken Sie auf Microsoft Azure visualisieren. Beide erfordern jedoch, dass Sie die Daten einfügen, als JSON formatiert und auf bestimmte Weise gesichert schieben. Dadurch wird die folgende Frage. Was tun Sie, wenn Sie Daten aus öffentlichen oder privaten Quellen vorverlegen, wo die Daten werden als Feed irgendeiner oder Webdienst verfügbar gemacht, aber Sie verfügen nicht über die Möglichkeit zu ändern, wie die Daten veröffentlicht werden, soll? Erwägen Sie das Wetter oder Datenverkehr oder Aktienkursen - Sie ist nicht zu entnehmen NOAA, oder WSDOT oder NASDAQ einer Pushbenachrichtigungen an Ihre Verteiler Ereignis konfigurieren. Um dieses Problem zu lösen, haben wir geschrieben und öffnen eine kleinen Cloud Stichprobe, die Sie ändern und bereitstellen können, die ziehen Sie die Daten aus einer Quelle solche und schieben Sie ihn an Ihre Ereignis Verteiler intern. Hier können Sie ausführen, beliebig mit, Betreff, zu der Lizenzbedingungen vom Producer natürlich. Die Anwendung finden Sie [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Beachten Sie, dass der Code in diesem Beispiel nur zum Extrahieren von Daten aus typische Web Feeds und das Schreiben in ein Ereignis Azure-Hub angezeigt werden. Dies ist nicht soll eine Anwendung Herstellung und keine Versuche unternommen wurden, für die Verwendung geeignet auf diese Weise wird – Strictfly einer DIY, nur Entwicklertools konzentrieren Beispiel. Darüber hinaus ist das Vorhandensein in diesem Beispiel nicht käme empfohlen, dass Sie Daten **Abrufen** in Azure statt **Pushbenachrichtigungen** sollten sie. Sie sollten überprüfen Sicherheit, Leistung, Funktionalität und Kosten Faktoren, bevor Sie auf eine End-to-End-Architektur aus.

## <a name="application-structure"></a>Anwendungsstruktur

Die Anwendung in c# geschrieben ist, und die [Beschreibung der Stichprobe](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) enthält alle Informationen, die Sie ändern, erstellen und veröffentlichen Sie die Anwendung müssen. Die folgenden Abschnitte enthalten einen detaillierten Überblick über die Funktionsweise der Anwendungs.

Zunächst unter der Voraussetzung, dass Sie Zugriff auf einen Datenfeed haben. Sie möchten beispielsweise den Datenverkehr Daten aus der Washington Zustand Abteilung des Verkehrs oder die Wetterdaten NOAA, um benutzerdefinierte Berichte anzuzeigen oder zum Kombinieren von Daten mit anderen Daten in Ihrer Anwendung abonnierte wird. Sie müssen außerdem einen Azure Ereignis Hub eingerichtet haben, und die Verbindung wissen Zeichenfolge erforderlich, darauf zuzugreifen.

Wenn die Lösung GenericWebToEH startet, wird eine Konfigurationsdatei (App.config) können Sie eine Anzahl von Elementen zu gelangen gelesen:

1. Die URL, oder eine Liste der URLs, für die Website, die Daten zu veröffentlichen. Idealerweise Dies ist eine Website, die Daten in JSON, wie WSDOT optimiert veröffentlicht [hier](http://www.wsdot.wa.gov/Traffic/api/). 
2. Anmeldeinformationen für die URL, falls erforderlich. Viele öffentliche Quellen benötigen keine Anmeldeinformationen, oder Sie können die Anmeldeinformationen in der URL-Zeichenfolge setzen. Andere setzen voraus, dass Sie separat angeben. (Beachten Sie, dass nur einen Satz von Anmeldeinformationen in dieser Anwendung angeben können, damit er nur funktioniert, wenn Sie nur eine URL, keine Liste der URLs angeben.)
3. Die Verbindungszeichenfolge und den Namen der Ereignis-Hub in diesem Ereignis Hubs Namespace, zu dem Sie die Daten Pushbenachrichtigungen werden. Sie finden diese Informationen im Azure-Portal.
4. Ein Ruheintervall, in Millisekunden an, für das Intervall zwischen die Website öffentlicher Daten abrufen. Wenn Sie diese Option erfordert einige Gedanken. Wenn Sie zu selten Umfrage, können Sie Daten verpasst haben; Andererseits, wenn Sie so oft Umfrage, erhalten Sie eine große Datenmenge wiederholende oder schlechter noch, Sie möglicherweise blockiert als ein schädliche Bot. Beachten Sie, wie oft die Datenquelle aktualisiert werden – Wetter oder Datenverkehr Daten möglicherweise aktualisiert, wenn Sie alle 15 Minuten, aber Stock Angebote vielleicht jeder einige Sekunden dauern, je nachdem, wo Sie erhalten. 
5. Eine Kennzeichnung in der Anwendung feststellen, ob die Daten als JSON oder XML-eingeht. Da Sie die Daten an ein Ereignis Verteiler Pushbenachrichtigungen müssen, muss die Anwendung ein Modul zum Konvertieren von XML in JSON vor dem senden.

Nach dem Lesen der Konfigurationsdatei, in einer Schleife - Zugriff auf der öffentlichen Website, die Daten, falls notwendig, Schreiben sie an Ihrem Ereignis Verteiler, und klicken Sie dann die Ruheintervall warten, bevor Sie es über alle erneut konvertieren, geht die Anwendung. Insbesondere:

  * Lesen Sie die öffentliche Website ein. Für sofort zu senden empfangen von Daten ist die Instanz der Klasse RawXMLWithHeaderToJsonReader von Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs verwendet. Es liest Quellstream in die GetData()-Methode und teilt dann darauf, um kleinere Einheiten (d. h. Records) GetXmlFromOriginalText verwenden. 
  Diese Methode wird XML lesen sowie um regelkonformes JSON oder JSON-array. Dann Verarbeitung gestartet wird, mithilfe von MergeToXML Konfiguration von App.config (Standard = leer).
  * Die empfangen und Senden von Daten werden als eine Schleife in der Methode Process() in Program.cs implementiert. 
  Nach dem Empfang Ausgabeergebnisse aus Iservice1, getrennt die Methode reiht Werte an den Ereignis-Hub an.

## <a name="next-steps"></a>Nächste Schritte

Um die Lösung bereitstellen, Klonen Sie oder Herunterladen Sie die Anwendung [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) , bearbeiten Sie die App, erstellen sie und schließlich veröffentlichen. Nachdem Sie die Anwendung veröffentlicht haben, sehen sie in der klassischen Azure-Portal unter Cloud Services ausgeführt, und Sie können einige der Konfiguration Einstellungen (beispielsweise das Ereignis Hub Ziel und die Ruheintervall) auf der Registerkarte **Konfigurieren** ändern.

Finden Sie unter Weitere Ereignis Hubs Beispiele im [Katalog Azure Beispiele](https://azure.microsoft.com/documentation/samples/?service=event-hubs) und auf [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).

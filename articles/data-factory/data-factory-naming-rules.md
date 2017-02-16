<properties 
    pageTitle="Daten Factory - Regeln benennen | Microsoft Azure" 
    description="Beschreibt Benennungskonventionen für Daten Factory Personen an." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure-Daten Factory - Benennung von Regeln 
Die folgende Tabelle enthält die Regeln für Daten Factory Elemente.



Namen | Eindeutigkeit des Benutzernamens | Validierungen
:--- | :-------------- | :----------------
Daten Factory | Für Microsoft Azure eindeutig. Namen Groß-und Kleinschreibung beachtet, d. h., MyDF und Mydf finden Sie in der gleichen Daten Factory. |<ul><li>Jede Factory Daten ist genau ein Azure-Abonnement verknüpft.</li><li>Objektnamen müssen mit einem Buchstaben oder einer Zahl beginnen, und können nur Buchstaben, Zahlen und der Bindestrich (-) enthalten.</li><li>Jeder Bindestrichs (-) muss sofort davor und dahinter einen Buchstaben oder eine Zahl sein. Aufeinander folgende Striche sind im Containernamen nicht zulässig.</li><li>Name kann 3-63 Zeichen lang sein.</li></ul>
Verknüpfte Services/Tabellen/Pipelines | In einer Fabrik Daten mit eindeutigen. Namen Groß-und Kleinschreibung beachtet. | <ul><li>Maximale Anzahl von Zeichen in einem Tabellennamen: 260.</li><li>Objektnamen müssen mit einem Buchstaben Zahl oder eines Unterstrichs (_) beginnen.</li><li>Folgenden Zeichen sind nicht zulässig: ".", "+", "?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul>
Ressourcengruppe | Für Microsoft Azure eindeutig. Namen Groß-und Kleinschreibung beachtet. | <ul><li>Maximale Anzahl von Zeichen: 1000.</li><li>Namen kann Buchstaben, Ziffern und die folgenden Zeichen enthalten: "-", "_",","und".".</li></ul>
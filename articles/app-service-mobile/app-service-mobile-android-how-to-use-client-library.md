<properties
    pageTitle="So verwenden Sie die Android Mobile-Apps-Client-Bibliothek"
    description="Verwendung von Android Client SDK für Mobile-Apps Azure."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>So verwenden Sie die Clientbibliothek Android-für Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

In diesem Handbuch wird gezeigt, wie das Android Client SDK für Mobile-Apps verwenden, um allgemeine Szenarien implementieren, z. B.:

- Abfragen von Daten (einfügen, aktualisieren und löschen).
- Authentifizierung.
- Behandeln von Fehlern.
- Anpassen des Clients. 

Bedeutet auch eine tief greifende in allgemeine Client-Code in die meisten mobilen apps verwendet werden.

In diesem Handbuch Schwerpunkt auf dem Android clientseitige-SDK.  Weitere Informationen zu den serverseitigen SDKs für Mobile-Apps finden Sie unter [Arbeiten mit .NET Back-End-SDK] [ 10] oder [zum Verwenden der Node.js Back-End-SDK][11].

## <a name="reference-documentation"></a>Dokumentation zur

Sie können den [Javadocs-API-Referenz] finden[ 12] für die Clientbibliothek Android-auf GitHub.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Die Azure Mobile Apps Android SDK unterstützt API Ebenen 19 bis 24 (KitKat bis Nougat).  

Die "Server Bewegung"-Authentifizierung verwendet eine WebView für die präsentierten Benutzeroberfläche an. Wenn das Gerät nicht Vorführen einer WebView UI kann, werden andere Methoden der Authentifizierung, die außerhalb des Gültigkeitsbereichs des Produkts ist dann benötigt.  Dieses SDK eignet sich nicht für anzeigen oder vom Typ auf ähnliche Weise eingeschränkter Geräte.

## <a name="setup-and-prerequisites"></a>Setup und erforderliche Komponenten

Führen Sie das [Mobile-Apps Schnellstart](app-service-mobile-android-get-started.md) -Lernprogramm.  Dieser Aufgabe wird sichergestellt, dass alle erforderlichen Komponenten für die Entwicklung von Azure Mobile-Apps erfüllt sind.  Schnellstart können Sie Ihr Konto konfigurieren und Erstellen Ihrer ersten Mobile-App Back-End.

Wenn Sie sich entscheiden, nicht das Schnellstart-Lernprogramm abzuschließen, führen Sie die folgenden Aufgaben:

- [Erstellen einer Mobile-App Back-End-] [ 13] Android-App verwenden.
- In Android Studio, [Aktualisieren Sie die Dateien Gradle erstellen](#gradle-build).
- [Aktivieren von Internet-Berechtigung](#enable-internet).

###<a name="a-namegradle-buildaupdate-the-gradle-build-file"></a><a name="gradle-build"></a>Aktualisieren Sie die Gradle-Datei erstellen

Ändern Sie die beiden Dateien **build.gradle** :

1. Fügen Sie diesen Code zur *Ebene **build.gradle** Projektdatei in den *Buildscript* -Tag* ein:

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Fügen Sie zu der *app Modul* Ebene **build.gradle** Datei innerhalb der Kategorie *Abhängigkeiten* diesen Code ein:

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Aktuell ist die neueste Version 3.1.0. Die unterstützten Versionen aufgelistet sind [hier][14].

###<a name="a-nameenable-internetaenable-internet-permission"></a><a name="enable-internet"></a>Aktivieren von Internet-Berechtigung

Um Azure zugreifen zu können, müssen Ihre app die INTERNET-Berechtigung aktiviert. Wenn sie noch nicht aktiviert ist, fügen Sie die folgende Zeile des Codes zu Ihrer Datei **AndroidManifest.xml** ein:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Die Grundlagen aneignen

In diesem Abschnitt werden einige der Code in die Schnellstart-app, die beschrieben, wie mit Azure Mobile-Apps erläutert.  

###<a name="a-namedata-objectadefine-client-data-classes"></a><a name="data-object"></a>Daten-Client-Klassen definieren

Definieren Sie Zugriff auf Daten aus SQL Azure-Tabellen Client Datenklassen entsprechen den Tabellen in der Mobile-App Back-End-ein. In den Beispielen in diesem Thema wird vorausgesetzt, eine Tabelle mit dem Namen **ToDoItem**, die weist die folgenden Spalten:

- ID
- Text
- Führen Sie die

Das entsprechende eingegebene clientseitige Objekt:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Der Code befindet sich in einer Datei namens **ToDoItem.java**.

Wenn Sie die SQL Azure-Tabelle weitere Spalten enthält, möchten Sie die entsprechenden Felder auf diesen Kurs hinzufügen.  Angenommen, wenn das DTO (Data Transferobjekt) enthalten hat eine ganze Zahl Prioritätenspalte, und klicken Sie dann können Sie dieses Feld sowie deren Get-Methode und Set-Methoden hinzufügen:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

So erstellen Sie zusätzliche Tabellen in Ihre Mobile-Apps Back-End-Informationen finden Sie unter [wie: definieren einen Tabelle Controller] [ 15] (.NET Back-End-) oder [definieren Tabellen mithilfe der dynamischen Schemas] [ 16] (Node.js Back-End-). Für eine Node.js Back-End-können Sie auch die Einstellung **einfache Tabellen** im [Portal Azure]verwenden.

###<a name="a-namecreate-clientahow-to-create-the-client-context"></a><a name="create-client"></a>So: Erstellen den Clientkontext

Dieser Code erstellt, das **MobileServiceClient** -Objekt, das zum Zugreifen auf Ihre Mobile-App Back-End-verwendet wird. Der Code geht in die `onCreate` -Methode der **Aktivität** Klasse in *AndroidManifest.xml* als **Primär** Aktion und **Startprogramm für das** Kategorie angegeben. In den Schnellstart-Code geht es in der Datei **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

In diesem Code ersetzen `MobileAppUrl` mit der URL für Ihre Mobile-App Back-End-, die im [Portal Azure] in das Blade für Ihre Mobile-App Back-End-gefunden werden können. Für diese Zeile Code kompiliert müssen Sie auch mit der folgenden Anweisung **Importieren** hinzufügen:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="a-nameinstantiatingahow-to-create-a-table-reference"></a><a name="instantiating"></a>So: Erstellen eines 3D-Bezugs Tabelle

Die einfachste Möglichkeit zum Abfragen und Ändern von Daten in die Back-End-wird mithilfe der *eingegebenen Modell Programmierung*, da Java eine Stimme eingegebenen Sprache ist. Dieses Modell bietet nahtlose JSON-Serialisierung und Deserialisierung mithilfe der [Gson] [ 3] Bibliothek beim Senden von Daten zwischen Clientobjekten und Tabellen in die Back-End-SQL Azure.

Zum Öffnen eine Tabelle erstellen Sie zuerst eine [MobileServiceTable] [ 8] Objekt durch Aufrufen der **getTable** Methode für die [MobileServiceClient][9].  Diese Methode weist zwei überladenen:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

In den folgenden Code wird **mClient** ein Verweis auf das Objekt MobileServiceClient.  Die erste Überladung wird verwendet, wobei den Klassennamen und den Namen der Tabelle gleich sind und der im Schnellstart verwendet wird:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Die zweite Überladung wird verwendet, wenn der Tabellenname vom Klassennamen unterscheidet: der erste Parameter ist der Tabellenname.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="a-namebindingahow-to-bind-data-to-the-user-interface"></a><a name="binding"></a>So: Binden von Daten auf der Benutzeroberfläche

Binden von Daten umfasst drei Komponenten:

- Die Datenquelle
- Das Bildschirmlayout
- Die Netzwerkadapter, die die beiden ausgedrückt.

In unserem Beispielcode zurück wir die Daten aus der Tabelle Mobile Apps SQL Azure **ToDoItem** , in einer Matrix zurück. Diese Aktivität ist ein gemeinsames Muster für Applikationen Daten.  Datenbankabfragen zurückgeben oft eine Auflistung von Zeilen, die der Kunden in einer Liste oder einem Array abruft. In diesem Beispiel wird die Matrix der Datenquelle.

Der Code gibt ein Bildschirmlayout, die definiert die Ansicht der Daten, die angezeigt wird, auf dem Gerät.  Die beiden zusammen mit einem Netzwerkadapter, die in diesem Code eine Erweiterung gebunden werden von der **ArrayAdapter&lt;ToDoItem&gt; ** Class.

#### <a name="a-namelayoutahow-to-define-the-layout"></a><a name="layout"></a>So: Definieren des Layouts

Das Layout wird durch mehrere XML-Codeausschnitte definiert. Wenn ein vorhandenes Layout, entspricht der folgende Code **Listenansicht** wir mit unserer Server-Daten füllen möchten.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Im vorhergehenden Code gibt das Attribut *Listitem* die Id des Layouts für eine einzelne Zeile in der Liste. Dieser Code gibt an, ein Kontrollkästchen und den dazugehörigen Text und ruft für jedes Element in der Liste einmal instanziiert. Bei diesem Layout wird das **ID-** Feld nicht angezeigt, und würde angeben eines Layouts komplexeren zusätzliche Felder in der Anzeige. Dieser Code ist in der Datei **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="a-nameadapterahow-to-define-the-adapter"></a><a name="adapter"></a>So: definieren die Netzwerkadapter

Da die Datenquelle unsere Ansicht ein Array von **ToDoItem**, ist wir Unterklasse unsere Netzwerkadapter aus einer **ArrayAdapter&lt;ToDoItem&gt; ** Class. Diese Unterklasse erzeugt eine Ansicht für jede **ToDoItem** mit dem Layout **Row_list_to_do** .

In unserem Code definieren wir die folgende Klasse, die Erweiterung ist die **ArrayAdapter&lt;E&gt; ** Klasse:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Überschreiben Sie die Netzwerkadapter **GetView** -Methode. Beispiel:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Wir erstellen eine Instanz dieser Klasse in unseren Aktivität wie folgt:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Der zweite Parameter an den Konstruktor ToDoItemAdapter ist ein Verweis auf das Layout. Wir können jetzt **Listenansicht** instanziieren und Zuweisen der Netzwerkadapter in der **Listenansicht**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="a-nameapiathe-api-structure"></a><a name="api"></a>Die Struktur API

Mobile-Apps Tabellenvorgänge und benutzerdefinierte API-Aufrufe sind asynchrone. Verwenden Sie die [Zukunft] und [AsyncTask] Objekte, für die asynchronen Methoden betreffen Abfragen, einfügen, aktualisieren und löschen. Verwenden der Verfügbarkeit vereinfacht mehrere Vorgänge in einem Hintergrundthread ausführen, ohne mehrere geschachtelte Rückrufe behandelt.

Überprüfen der Datei **ToDoActivity.java** Android Schnellstart Projekt aus dem [Azure-Portal] für ein Beispiel an.

#### <a name="a-nameuse-adapterahow-to-use-the-adapter"></a><a name="use-adapter"></a>So: Verwenden Sie die Netzwerkadapter

Sie können nun Daten Bindung verwenden. Mit dem folgende Code veranschaulicht, wie Elemente in der Tabelle erhalten und füllt den lokalen Netzwerkadapter mit zurückgegebenen Elemente.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Rufen Sie die Netzwerkadapter jederzeit der Tabelle **ToDoItem ändern** . Da Änderungen auf Basis von einem Datensatz fertig sind, behandeln eine einzelne Zeile anstelle einer Auflistung. Wenn Sie ein Element einfügen, rufen Sie die Methode **Hinzufügen** , am Netzwerkadapter. Beim Löschen, rufen Sie die Methode **Entfernen** .

##<a name="a-namequeryingahow-to-query-data-from-your-mobile-app-backend"></a><a name="querying"></a>Wie: Abfragen von Daten aus Ihrem Mobile-App Back-End-

In diesem Abschnitt beschrieben Abfragen an die Mobile-App Back-End-ausgeben, wozu auch die folgenden Aufgaben:

- [Alle Elemente zurück]
- [Filtern Sie die zurückgegebene Daten]
- [Sortieren der Daten zurückgegeben]
- [Daten in Seiten zurück]
- [Wählen Sie bestimmte Spalten aus.]
- [Verketten Abfragemethoden](#chaining)

### <a name="a-nameshowallahow-to-return-all-items-from-a-table"></a><a name="showAll"></a>So: alle Elemente aus einer Tabelle zurück

Die folgende Abfrage gibt alle Elemente in der Tabelle **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

Die *Ergebnisse* Variable gibt das Resultset aus der Abfrage als Liste an.

### <a name="a-namefilteringahow-to-filter-returned-data"></a><a name="filtering"></a>So: Filter zurückgegebenen Daten

Die Ausführung der folgenden Abfrage gibt alle Elemente aus der Tabelle **ToDoItem** , wobei entspricht **abgeschlossen** **falsch**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** ist der Bezug auf die mobile Service-Tabelle, die wir zuvor erstellt haben.

Definieren eines Filters mit der **Stelle, an der** Methode Anruf auf die Tabelle verweisen. Die **where** -Methode folgt eine **Feld** Methode, gefolgt von einer Methode, die das logische Prädikat angibt. Einschließen von mögliche Prädikat Methoden **Eq** (gleich), **Neuer** (nicht gleich), **Gt** (größer als), **Seite** (größer als oder gleich), **Lt** (kleiner als), **le** (kleiner als oder gleich). Diese Methoden können Sie die Zeichenfolge und die Anzahl der Felder auf bestimmte Werte zu vergleichen.

Sie können Daten filtern. Die folgenden Methoden können Sie der gesamte Datumsfeld oder Teile des Datums verglichen: **Jahr**, **Monat**, **Tag**, **Stunde**, **Minute**und **zweiten**. Im folgende Beispiel fügt einen Filter für Elemente, deren *Fälligkeitsdatum* 2013 gleich, hinzu.

    mToDoTable.where().year("due").eq(2013).execute().get();

Die folgenden Methoden unterstützen komplexe Filter Zeichenfolge Feldern: **StartsWith** **EndsWith**, **Verketten**, **Teilzeichenfolge**, **IndexOf**, **Ersetzen**, **ToLower**, **ToUpper**, **Zuschneiden**und **Länge**. Im folgende Beispiel Filter für Tabellenzeilen, in dem *die Textspalte* mit "PRI0." beginnt

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Die folgenden Operator Methoden werden auf Zahlenfelder unterstützt: **Hinzufügen**, **Sub**, **Mul**, **Div**, **mod**, **Untergrenze**, **Obergrenze**und **runden**. Im folgende Beispiel Filter für Tabellenzeilen, in dem die **Dauer** eine gerade Zahl ist.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Sie können Prädikate mit folgenden logischen Methoden kombinieren: **und**, **oder** und **nicht**. Im folgende Beispiel kombiniert zwei von den vorherigen Beispielen.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Gruppieren und Verschachteln von logischen Operatoren:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Ausführlichere Informationen und Beispiele für Filtern finden Sie unter [Erforschen der Vielfalt des Modells Abfrage Android-Client](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="a-namesortingahow-to-sort-returned-data"></a><a name="sorting"></a>So: Sortieren zurückgegebenen Daten

Im folgende Code gibt, dass alle Elemente aus einer Tabelle **ToDoItems** sortiert nach dem *Text* -Feld aufsteigend. *mToDoTable* ist der Bezug auf die Back-End-Tabelle, die Sie zuvor erstellt haben:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Erste Parameter der Methode **OrderBy** ist eine Zeichenfolge, die den Namen des Felds, nach dem sortiert gleich. Der zweite Parameter verwendet die **QueryOrder** -Enumeration, um anzugeben, ob aufsteigend oder Absteigend sortieren.  Wenn Sie mithilfe der Methode ***,*** filtern, muss die ***where*** -Methode vor der Methode ***OrderBy*** aufgerufen werden.

### <a name="a-namepagingahow-to-return-data-in-pages"></a><a name="paging"></a>So: in Seiten Daten zurück

Das erste Beispiel zeigt, wie der obersten fünf Elemente aus einer Tabelle zu markieren. Die Abfrage gibt die Elemente aus einer Tabelle mit **ToDoItems**. **mToDoTable** ist der Bezug auf die Back-End-Tabelle, die Sie zuvor erstellt haben:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Hier ist eine Abfrage, die überspringt die ersten fünf Elemente, und klicken Sie dann den nächsten fünf gibt:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="a-nameselectingahow-to-select-specific-columns"></a><a name="selecting"></a>So: Wählen Sie bestimmte Spalten aus.

Mit dem folgende Code wird gezeigt, wie eine Tabelle mit **ToDoItems**alle Elemente zurück, aber zeigt nur die Felder **abgeschlossen** und **Text** an. **mToDoTable** ist der Bezug auf die Back-End-Tabelle, die wir zuvor erstellt haben.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Die Parameter für die select-Funktion gibt die Zeichenfolgennamen von Spalten in der Tabelle, die zurückgegeben werden soll.

Die **select** -Methode muss Methoden wie **, wo** und **OrderBy**folgen. Es kann Seitennavigation Methoden wie **oben**folgen.

### <a name="a-namechainingahow-to-concatenate-query-methods"></a><a name="chaining"></a>So: Abfragemethoden verketten

Die verwendeten Back-End-Tabellen Abfragen Methoden können verkettet werden. Verketten Abfragemethoden, können Sie bestimmte Spalten der gefilterten Zeilen auswählen, die sortiert und seitenweise durchlaufen werden. Sie können komplexe logische Filter erstellen.
Jede Abfragemethode gibt ein Abfrageobjekt an. Um die Reihe von Methoden zu beenden, und führen Sie die Abfrage tatsächlich, rufen Sie die Methode **Ausführen** . Beispiel:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Die verkettete Abfragemethoden müssen wie folgt angeordnet werden:

1. Filtermethoden (**,**).
2. (**OrderBy**) Sortiermethoden.
3. (**Wählen Sie**) Auswahlmethoden.
4. Methoden von Seitennavigation (**Überspringen** und **oben**).

##<a name="a-nameinsertingahow-to-insert-data-into-the-backend"></a><a name="inserting"></a>So: Einfügen von Daten in die Back-End-

Instanziiere eine Instanz der *ToDoItem* -Klasse, und legen Sie dessen Eigenschaften.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Verwenden Sie dann **insert()** , um ein Objekt einzufügen:

    ToDoItem entity = mToDoTable.insert(item).get();

Die zurückgegebenen Entität Übereinstimmungen bereits die Daten in die Back-End-Tabelle eingefügt enthalten die ID und alle anderen Werte, die auf die Back-End-festgelegt.

Mobile-Apps Tabellen erfordern eine primäre Schlüsselspalte mit dem Namen **Id**an. Standardmäßig ist diese Spalte eine Zeichenfolge zurück. Der Standardwert der Spalte ID ist eine GUID.  Sie können andere eindeutige Werte, wie e-Mail-Adressen oder Benutzernamen bereitstellen. Wenn ein ID Zeichenfolgenwert für eine eingefügten Datensatz nicht bereitgestellt wird, wird die Back-End-eine neue GUID generiert.

String-ID-Werte bieten die folgenden Vorteile:

+ IDs können generiert werden, ohne vorher eine Schleife in die Datenbank zu erstellen.
+ Einträge sind leichter aus verschiedenen Tabellen oder Datenbanken zusammenführen.
+ ID-Werte nahtlose besser in einer Anwendung Logik.

String-ID-Werte sind **erforderlich** für offlinesynchronisierung Support.

##<a name="a-nameupdatingahow-to-update-data-in-a-mobile-app"></a><a name="updating"></a>So: Aktualisieren von Daten in einer mobile-app

Übergeben Sie das neue Objekt zum Aktualisieren von Daten in einer Tabelle, um **die Update()-Methode** .

    mToDoTable.update(item).get();

In diesem Beispiel ist *Element* ein Verweis auf eine Zeile in der Tabelle *ToDoItem* mit einigen darauf vorgenommenen Änderungen an.
Die Zeile mit der gleichen **Id** wird aktualisiert.

##<a name="a-namedeletingahow-to-delete-data-in-a-mobile-app"></a><a name="deleting"></a>So: Löschen der Daten in einer mobile-app

Im folgende Code veranschaulicht, wie Daten aus einer Tabelle zu löschen, indem Sie das Objekt angeben.

    mToDoTable.delete(item);

Sie können auch ein Element löschen, indem Sie das Feld **Id** des zu löschenden Zeile angeben.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="a-namelookupahow-to-look-up-a-specific-item"></a><a name="lookup"></a>So: Nachschlagen eines bestimmten Elements

Nachschlagen eines Elements mit einem bestimmten **Id** -Feld aus, mit der **lookUp()** -Methode:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="a-nameuntypedahow-to-work-with-untyped-data"></a><a name="untyped"></a>So: Arbeiten mit nicht typisierten Daten

Das Modell das nicht typisierte Programmierung bietet Ihnen genaue Kontrolle über JSON-Serialisierung.  Es gibt einige häufige Szenarien, wo Sie möglicherweise ein nicht typisiertes programming Modell verwenden möchten. Wenn beispielsweise die Back-End-Tabelle viele Spalten enthält, und Sie müssen nur eine Teilmenge der Spalten verweisen.  Das eingegebene Modell müssen Sie alle mobilen apps Tabellenspalten in der Datenklasse definieren.  

Die meisten Anrufe API für den Zugriff auf Daten sind ähnlich wie die eingegebenen programming Anrufe. Der wichtigste Unterschied ist, dass im Modell nicht typisierten Sie Methoden für das **MobileServiceJsonTable** -Objekt, nicht auf das Objekt **MobileServiceTable** aufzurufen.

### <a name="a-namejsoninstanceahow-to-create-an-instance-of-an-untyped-table"></a><a name="json_instance"></a>So: erstellen eine Instanz von eine nicht typisierte Tabelle

Ähnlich wie die eingegebenen Modell beginnen Sie mit der erste eines Tabellenverweis, aber in diesem Fall ist es ein Objekt **MobileServicesJsonTable** . Den Bezug abrufen, indem er den **getTable** Methode für eine Instanz des Clients:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Nachdem Sie eine Instanz der **MobileServiceJsonTable**erstellt haben, muss es praktisch derselben API als mit dem eingegebenen programming Modell verfügbar. In einigen Fällen übernehmen die Methoden einen nicht typisierten Parameter statt eingegebenen Parameter.

### <a name="a-namejsoninsertahow-to-insert-into-an-untyped-table"></a><a name="json_insert"></a>So: in eine nicht typisierte Tabelle einfügen

Mit dem folgende Code veranschaulicht, wie eine einfügen möchten. Der erste Schritt besteht im Erstellen einer [JsonObject][1], welche Teil der [Gson] ist[ 3] Bibliothek.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Verwenden Sie dann, **insert()** , das nicht typisierte Objekt in der Tabelle einfügen.

    mJsonToDoTable.insert(jsonItem).get();

Wenn Sie die ID des eingefügten Objekts erhalten müssen, verwenden Sie die **getAsJsonPrimitive()** Methode.

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="a-namejsondeleteahow-to-delete-from-an-untyped-table"></a><a name="json_delete"></a>So: eine nicht typisierte Tabelle löschen

Im folgende Code veranschaulicht, wie eine Instanz, in diesem Fall der gleichen Instanz von einer **JsonObject** löschen, die im vorherigen *Einfügen* Beispiel erstellt wurde. Der Code ist identisch mit der eingegebenen Groß-/Kleinschreibung, aber die Methode hat eine andere Signatur aus, da sie eine **JsonObject**verweist auf.

         mToDoTable.delete(jsonItem);

Sie können auch eine Instanz direkt mithilfe der ID löschen:

         mToDoTable.delete(ID);

### <a name="a-namejsongetahow-to-return-all-rows-from-an-untyped-table"></a><a name="json_get"></a>So: alle Zeilen aus einer Tabelle nicht typisierten zurückgeben

Der folgende Code wird gezeigt, wie eine gesamte Tabelle abgerufen werden. Da Sie eine JSON-Tabelle verwenden, können Sie nur einen Teil der Tabelle Spalten Selektives Abrufen.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Demselben Satz von Filtern, Filtern und das seitenweise Methoden, mit denen die eingegebenen Modell können sind für nicht typisierte Modell verfügbar.

##<a name="a-namecustom-apiahow-to-call-a-custom-api"></a><a name="custom-api"></a>So: eine benutzerdefinierte API anrufen

Eine benutzerdefinierte API können Sie zum Definieren von benutzerdefinierter Endpunkten, die Serverfunktionalität verfügbar zu machen, die keine zuordnen zu einer einfügen, aktualisieren, löschen, und beim Lesen. Mithilfe einer benutzerdefinierten API haben mehr Kontrolle über messaging, Sie einschließlich lesen und HTTP-Nachrichtenköpfen festlegen und ein Textkörper Nachrichtenformat als JSON definieren.

Aus einem Android-Client rufen Sie die **InvokeApi** -Methode, um den benutzerdefinierten API Endpunkt aufzurufen. Im folgenden Beispiel wird gezeigt, wie aufrufen, einen API-Endpunkt mit dem Namen **CompleteAll**, die eine Auflistungsklasse mit dem Namen **MarkAllResult**zurückgibt.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

Die Methode **InvokeApi** heißt auf dem Client, sendet eine POST-Anforderung an die neue benutzerdefinierte-API. Das Ergebnis von der benutzerdefinierten-API zurückgegeben wird eine Nachricht im Dialogfeld angezeigt, wie Fehler aufgetreten sind. Andere Versionen von **InvokeApi** können Sie optional ein Objekt im Hauptteil Anforderung senden, geben Sie die HTTP-Methode und Abfrageparameter mit der Anforderung senden. Nicht typisierte Versionen von **InvokeApi** werden ebenfalls bereitgestellt.

##<a name="a-nameauthenticationahow-to-add-authentication-to-your-app"></a><a name="authentication"></a>So: Authentifizierung für Ihre app hinzufügen

Lernprogramme bereits wird ausführlich beschrieben wie diese Features hinzufügen.

App-Dienst unterstützt [app Benutzerauthentifizierung](app-service-mobile-android-get-started-users.md) mithilfe einer Vielzahl von externe Identitätsanbieter: Facebook, Google, Microsoft Account, Twitter und Azure Active Directory. Sie können Tabellen zum Einschränken des Zugriffs für bestimmte Vorgänge, die nur authentifizierten Benutzern Berechtigungen festlegen. Die Identität des authentifizierten Benutzer können Sie auch in der Back-End-Autorisierungsregeln implementiert wird.

Es werden zwei Authentifizierung Zahlungen unterstützt: einen Fluss **Server** und einen Fluss **Client** . Server illustrieren stellt die einfachste Authentifizierung Oberfläche aus, wie sie von der Identität Anbieter Web-Oberfläche abhängig ist.  Keine zusätzliche SDKs müssen Fluss Serverauthentifizierung implementieren. Fluss Serverauthentifizierung bietet keine Tiefe Integration in dem mobilen Gerät und ist nur für Nachweis des Konzepts Szenarien empfohlen.

Client illustrieren ermöglicht verbesserte Integration in Geräte-spezifischen Funktionen wie einmaliges Anmelden wie er von SDKs bereitgestellten vom Identitätsanbieter abhängt.  Beispielsweise können Sie die Facebook-SDK in der mobilen Anwendung integrieren.  Des mobile Clients vertauscht in der Facebook-app und Ihre anmelden, bevor Sie wieder in der mobilen app austauschen bestätigt.

Aktivieren Sie die Authentifizierung in Ihrer app sind vier Schritte erforderlich:

- Registrieren Sie Ihre app für die Authentifizierung mit einem Identitätsanbieter aus.
- Konfigurieren der App-Dienst Back-End.
- Einschränken der Tabellenberechtigungen für authentifizierte Benutzer nur auf der App-Dienst Back-End.
- Hinzufügen von Code für die Authentifizierung zu Ihrer Anwendung.

Sie können Tabellen zum Einschränken des Zugriffs für bestimmte Vorgänge, die nur authentifizierten Benutzern Berechtigungen festlegen. Die SID von einem authentifizierten Benutzer können Sie auch um Anfragen zu ändern.  Weitere Informationen finden Sie [Erste Schritte mit der Authentifizierung] und der Dokumentation Server SDK so wird's gemacht.

### <a name="a-namecachingahow-to-add-authentication-code-to-your-app"></a><a name="caching"></a>So: Hinzufügen von Code für die Authentifizierung zu Ihrer Anwendung

Der folgende Code startet einen Fluss Login-Prozess mit dem Google-Anbieter:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Die ID des Benutzers angemeldeten aus einer **MobileServiceUser** mithilfe der Methode **GetUserId** zu erhalten. Beispiel zum Aufrufen das asynchrone Login APIs verwenden finden Sie unter [Erste Schritte mit der Authentifizierung].

### <a name="a-namecachingahow-to-cache-authentication-tokens"></a><a name="caching"></a>So: Authentifizierungstoken im Cache

Authentifizierungstoken zwischenspeichern, müssen Sie die Benutzer-ID und Authentifizierungstoken lokal auf dem Gerät zu speichern. Das nächste Mal die app gestartet wird, überprüfen Sie den Cache, und wenn diese Werte vorhanden sind, können Sie das Protokoll im Verfahren überspringen und aktivieren den Client mit diesen Daten. Diese Daten sind jedoch vertraulich, und es sollte verschlüsselt für Sicherheit für den Fall, dass das Telefon gestohlen ruft gespeichert werden.

Sie können eine vollständige Beispiel für die Cache Authentifizierungstoken [Cache Authentifizierung Token]Abschnitt finden Sie unter[7].

Wenn Sie versuchen, eine abgelaufene Token verwenden, erhalten Sie Reaktionen *401 nicht autorisiert* . Sie können mithilfe von Filtern Authentifizierungsfehler behandelt werden.  Filter Achsenabschnitt Anfragen an die Back-End-App-Dienst an. Der Filtercode überprüft die Antwort auf eine 401, wird den Anmeldung Prozess ausgelöst und Lebensläufen klicken Sie dann auf die Anfrage, die die 401 generiert. 

## <a name="a-nameadalahow-to-authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>So: Benutzerauthentifizierung mit der Active Directory-Authentifizierung-Bibliothek

Die Active Directory Authentifizierung Bibliothek (ADAL) können sich Benutzer in Ihrer Anwendung mit Azure Active Directory. Mithilfe einer Client-Fluss Login empfiehlt sich häufig, mit der `loginAsync()` Methoden dargestelltes bietet eine weitere systemeigenen UX Verhalten und zur weiteren Anpassung ermöglicht.

1. Konfigurieren Sie für AAD Anmeldung Ihre mobile-app Back-End-anhand des Lernprogramms [zum Konfigurieren der App-Verwaltungsdienst für Active Directory-Benutzernamen](app-service-mobile-how-to-configure-active-directory-authentication.md) ein. Vergewissern Sie sich in der Registrierung einer systemeigenen Clientanwendung die optional Schritt abgeschlossen haben.

2. Installieren Sie ADAL durch Ändern der build.gradle-Datei, um die folgenden Definitionen enthalten:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Fügen Sie den folgenden Code an Ihrer Anwendung, machen die folgenden Ersatz:

* Ersetzen Sie **Einfügen-ZERTIFIZIERUNGSSTELLE-HERE** mit dem Namen der den Mandanten, in dem Sie die Anwendung bereitgestellt. Das Format sollte https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann auf der Registerkarte Domäne in Ihrem Azure Active Directory im [Azure klassischen Portal] kopiert werden.

* Ersetzen Sie mit der Client-ID für Ihre mobile-app Back-End- **Einfügen-Ressource-ID-HERE** ein. Sie können die Client-ID von der Registerkarte **Erweitert** unter **Azure Active Directory-Einstellungen** im Portal erhalten.

* Ersetzen Sie **Einfügen-CLIENT-ID-HERE** durch die Client-ID, die Sie in die native Client-Anwendung kopiert haben.

* **Einfügen-REDIRECT-URI-HERE** mit Ihrer Website _/.auth/login/done_ Endpunkt, mit dem HTTPS-Schema zu ersetzen. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_sein.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>So: Hinzufügen der Pushbenachrichtigung zu Ihrer Anwendung

Sie können [einen Überblick lesen] [ 6] , die erläutert, wie Microsoft Azure Benachrichtigung Hubs für eine Vielzahl von Pushbenachrichtigungen unterstützt.  In [diesem Lernprogramm][5], Pushbenachrichtigungen erhält eine Benachrichtigung an alle Geräte jedes Mal, wenn ein Datensatz eingefügt wird.

## <a name="how-to-add-offline-sync-to-your-app"></a>So: Hinzufügen von offlinesynchronisierung zu Ihrer Anwendung

Schnellstart-Lernprogramm enthält Code, der offlinesynchronisierung implementiert. Suchen Sie nach Code, mit dem Präfix Kommentare:

    // Offline Sync

Indem Sie die folgenden Codezeilen auskommentieren können Sie offlinesynchronisierung implementieren, und Sie können andere Mobile-Apps Code ähnlichen Code hinzufügen.

##<a name="a-namecustomizingahow-to-customize-the-client"></a><a name="customizing"></a>So: Anpassen den Client

Es gibt mehrere Methoden für das Standardverhalten des Clients anzupassen.

### <a name="a-nameheadersahow-to-customize-request-headers"></a><a name="headers"></a>So: Anpassen von Kopfzeilen anfordern

Konfigurieren eines **ServiceFilter** um jede Anforderung einen benutzerdefinierten HTTP-Header hinzuzufügen:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="a-nameserializationahow-to-customize-serialization"></a><a name="serialization"></a>So: Serialisierung anpassen

Der Client wird davon ausgegangen, dass die Tabellennamen, Spaltennamen und Daten eingibt, auf die Back-End-alle genau die Datenobjekte im Client definiert entsprechen. Kann es verschiedene Gründe, warum die Namen der Server und Client möglicherweise nicht übereinstimmt. In Ihrem Szenario sollten Sie die folgenden Arten von Anpassungen gehen Sie wie folgt:

- Die Spaltennamen in der Tabelle-App verwendet übereinstimmen nicht den Namen, die Sie im Client verwenden.
- Verwenden Sie eine App-Service-Tabelle, die einen anderen Namen als die Klasse enthält, die sie in dem Client zugeordnet ist.
- Aktivieren Sie automatische Eigenschaft Großschreibung.
- Komplexe Eigenschaften eines Objekts hinzufügen.

### <a name="a-namecolumnsahow-to-map-different-client-and-server-names"></a><a name="columns"></a>So: Zuordnen von verschiedenen Client- und Server-Namen

Nehmen Sie an, dass Ihre Java-Client-Code Java-Schreibweise Standardnamen für die **ToDoItem** Objekteigenschaften, beispielsweise die folgenden Eigenschaften verwendet:

- Teil
- mText
- mComplete
- mDuration

Serialisieren Sie die Kundennamen in JSON-Namen, die die Spaltennamen aus der Tabelle auf dem Server **ToDoItem** entsprechen. Im folgenden Code wird die [Gson] [ 3] Bibliothek, um die Eigenschaften Anmerkungen zu erstellen:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="a-nametableahow-to-map-different-table-names-between-the-client-and-the-backend"></a><a name="table"></a>So: Zuordnen von anderen Tabellennamen zwischen dem Client und die Back-End-

Ordnen der Client Tabellenname einen Tabellennamen verschiedene mobile Dienste mithilfe von Überschreiben von der [getTable()] [ 4] Methode:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="a-nameconversionsahow-to-automate-column-name-mappings"></a><a name="conversions"></a>So: Automatisieren Namen spaltenzuordnungen

Sie können angeben, dass eine Konvertierungsstrategie, die auf jeder Spalte angewendet wird, mithilfe der [Gson] [ 3] API. Android Clientbibliothek verwendet [Gson] [ 3] Hintergrundinformationen zum Serialisieren Java-Objekte mit JSON-Daten, bevor die Daten zur Azure-App-Dienst gesendet werden.  Der folgende Code verwendet die **setFieldNamingStrategy()** Methode, um die Strategie festlegen. In diesem Beispiel wird das erste Zeichen (ein "m"), löschen und Kleinbuchstaben des nächsten Zeichens für jede Feldname. Beispielsweise würde "Teil" in "Id." umzuwandeln

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Dieser Code muss ausgeführt werden, bevor Sie die **MobileServiceClient**verwenden.

### <a name="a-namecomplexahow-to-store-an-object-or-array-property-into-a-table"></a><a name="complex"></a>So: Speichern Sie eine Objekt oder Array-Eigenschaft in einer Tabelle

Bisher haben unseren Beispielen Serialisierung primitive Typen wie ganze Zahlen und Zeichenfolgen beteiligt.  Grundtypen serialisieren einfach in JSON.  Wenn wir ein komplexes Objekt, das automatisch serialisieren nicht JSON hinzufügen möchten, müssen wir die JSON-Serialisierungsmethode bereitzustellen.  Um ein Beispiel zum Bereitstellen von benutzerdefinierten JSON-Serialisierung von angezeigt wird, überprüfen Sie den Blogbeitrag [Anpassen Serialisierung mithilfe der Gson-Bibliothek im Mobile Services Android-Client][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Alle Elemente zurück]: #showAll
[Filtern Sie die zurückgegebene Daten]: #filtering
[Sortieren der Daten zurückgegeben]: #sorting
[Daten in Seiten zurück]: #paging
[Wählen Sie bestimmte Spalten aus.]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure-portal]: https://portal.azure.com
[Erste Schritte mit der Authentifizierung]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Zukünftiger]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
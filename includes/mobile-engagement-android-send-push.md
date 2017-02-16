
###<a name="update-manifest-file-to-enable-notifications"></a>Aktualisieren der Manifestdatei Benachrichtigungen aktivieren

Kopieren Sie unten die Ressourcen innerhalb der app messaging in Ihrer Manifest.xml zwischen den `<application>` und `</application>` Kategorien.

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

###<a name="specify-an-icon-for-notifications"></a>Geben Sie ein Symbol für Benachrichtigungen

Fügen Sie den folgenden XML-Ausschnitt Ihrer Manifest.xml zwischen den `<application>` und `</application>` Kategorien.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Dadurch wird das Symbol, das angezeigt wird, sowohl in System und in der app-Benachrichtigungen definiert. Es ist jedoch für Benachrichtigungen System obligatorisch innerhalb der app-Benachrichtigungen freigestellt. Android wird System Benachrichtigungen mit ungültigen Symbole ablehnt.

Stellen Sie sicher, dass Sie ein Symbol, das in einem vorhanden ist die **zeichenbaren** Ordner (wie ``engagement_close.png``). **MipMap** Ordner nicht unterstützt.

>[AZURE.NOTE] Sie sollten nicht auf das **Startprogramm für das** Symbol verwenden. Es wurde von einer anderen Auflösung und ist in der Regel in den Ordnern Mipmap, das wir nicht unterstützen.

Für real-apps, können Sie ein Symbol, das für Benachrichtigungen pro [Android Entwurfsrichtlinien](http://developer.android.com/design/patterns/notifications.html)geeignet ist.

>[AZURE.TIP] Richtige Symbol Lösungen verwenden, können Sie um sicherzustellen, dass sich [in diesen Beispielen](https://www.google.com/design/icons)ansehen.
Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Benachrichtigung** , klicken Sie auf ein Symbol, und klicken Sie dann auf `PNGS` der zeichenbaren Symbolsatz herunterladen. Sie können finden Sie unter welche zeichenbaren Ordnern mit welcher Auflösung für jede Version des Symbols verwendet werden soll.

###<a name="enable-your-app-to-receive-gcm-push-notifications"></a>Aktivieren Sie Ihre app GCM Pushbenachrichtigungen erhalten

1. Fügen Sie die folgenden in Ihrem Manifest.xml zwischen den `<application>` und `</application>` tags, nachdem die **Absender-ID-** Ersetzen der Firebase Project-Konsole abgerufen. Die \n ist beabsichtigt daher sollten Sie sicherstellen, dass Sie die Zahl mit Project beenden.

        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />

2. Fügen Sie den folgenden Code in Ihre Manifest.xml zwischen den `<application>` und `</application>` Kategorien. Ersetzen Sie den Paketnamen <Your package name>.

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>

3. Fügen Sie den letzten Satz von Berechtigungen, die vor dem hervorgehoben werden die `<application>` Kategorie. Ersetzen Sie `<Your package name>` nach dem Paketnamen der ist-der Anwendung.

        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />





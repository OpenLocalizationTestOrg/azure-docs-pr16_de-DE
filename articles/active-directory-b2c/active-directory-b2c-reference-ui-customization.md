<properties
    pageTitle="Azure Active Directory B2C: Anpassen der Benutzeroberfläche (UI) | Microsoft Azure"
    description="Klicken Sie auf die Anpassungsfeatures der Benutzeroberfläche (UI) in Azure Active Directory B2C Themas"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Anpassen der Azure AD B2C-Benutzeroberfläche (UI)

Benutzerfunktionalität ist in einer Anwendung Consumer zugänglichen höchste Priorität. Es ist der Unterschied zwischen einer guten Anwendung und eine gute, und zwischen lediglich aktive Nutzer und wirklich engagierten Dateien. Azure Active Directory (Azure AD) B2C können Sie die Consumer Anmeldung, Anmeldung (*Siehe Hinweis unten*), Profil bearbeiten, anpassen und Kennwort zurücksetzen Seiten mit Pixel perfekten Steuerelement.

> [AZURE.NOTE]
Aktuell lokales Konto anmelden, Seiten anzeigen, deren Accompaning Kennwort zurücksetzen Seiten und Überprüfung e-Mails nur mithilfe des [Unternehmens branding Feature](../active-directory/active-directory-add-company-branding.md) und nicht durch die Verfahren in diesem Artikel beschriebenen angepasst werden können.

In diesem Artikel erfahren Sie mehr über:

- Die Seite Benutzer Benutzeroberflächen-Anpassung-Funktion.
- Ein Tools, das Ihnen helfen testen das Seite Benutzeroberfläche Anpassung-Feature, das mit unseren Stichprobe Inhalt.
- Die Core Benutzeroberflächenelemente in jede Art von Seite.
- Bewährte Methoden bei der dieses Feature nach.

## <a name="the-page-ui-customization-feature"></a>Die Seite UI Anpassung-Funktion

Mit dem Anpassungsfeature der Benutzeroberfläche für Seite können Sie das Aussehen und Verhalten der Consumer Anmeldung, Anmeldung anpassen, das Zurücksetzen von Kennwörtern und Profil bearbeiten Seiten (durch Konfigurieren von [Richtlinien](active-directory-b2c-reference-policies.md)). Ihre Nutzer müssen nahtlose Erfahrung beim Navigieren zwischen Ihrer Anwendung und Seiten Azure AD B2C realisiert.

Im Gegensatz zu anderen Diensten, wo Benutzeroberflächenoptionen sind beschränkt oder sind nur verfügbar über APIs, verwendet Azure AD B2C einen modernen (und einfacheren) Ansatz zur Anpassung der Benutzeroberfläche Seite.

Und so funktioniert es: Azure AD B2C Code in Ihre Consumer Browser ausgeführt und verwendet einen modernen Ansatz [Cross-Origin Ressource freigeben (CORS)](http://www.w3.org/TR/cors/) aufgerufen Inhalt von einer URL zu laden, die Sie in einer Richtlinie angeben. Sie können verschiedene URLs für unterschiedliche Seiten angeben. Der Code werden zusammengeführt Benutzeroberflächenelemente aus Azure AD B2C für den Inhalt von der URL geladen und an Ihre Consumer angezeigt. Sie müssen lediglich:

1. Um regelkonformes HTML5 Inhalt mit Erstellen einer `<div id="api"></div>` Element (ein leeres Element sein muss) irgendwo der `<body>`. Dieses Element-Marken, an der Azure AD B2C Inhalt eingefügt wird.
2. Hosten von Inhalten auf einer HTTPS-Endpunkt (mit CORS zulässig).
3. Formatieren Sie die Elemente der Benutzeroberfläche, die in Azure AD B2C Fügt ein.

## <a name="test-out-the-ui-customization-feature"></a>Testen des Benutzeroberfläche Anpassung-Features

Wenn Sie das Feature der Benutzeroberfläche Anpassung ausprobieren, indem Sie mit unseren Stichprobe HTML und CSS-Inhalte möchten, haben wir Sie [eines einfachen Helper Tools](active-directory-b2c-reference-ui-customization-helper-tool.md) bereitgestellt, die hochgeladen wird und Stichprobe Inhalte auf Ihren Azure Blob-Speicher konfiguriert.

> [AZURE.NOTE]
Sie können an einer beliebigen Stelle den Benutzeroberflächeninhalt hosten: auf Webservern, CDNs, AWS a3, Datei Freigabe Systeme usw.. Solange der Inhalt auf einer öffentlich zugänglichen HTTPS-Endpunkt (mit CORS zulässig) gehostet wird, sind Sie gut zu wechseln. Wir verwenden die Azure Blob-Speicher für nur Erläuterung.

### <a name="the-most-basic-html-content-for-testing"></a>Zum Testen der grundlegendsten HTML-Inhalt

Abgebildet, wird der grundlegendsten HTML-Inhalt, auf den Sie verwenden können, um diese Funktion zu testen. Verwenden einer früheren Version hochladen, und konfigurieren diesen Inhalt auf Ihrer Azure Blob-Speicher bereitgestellten Helper-Tools. Klicken Sie dann zu überprüfen, ob die grundlegende, nicht stilisierter Schaltflächen und Formularfeldern auf jeder Seite angezeigten und funktionsfähig sind.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Die Core Benutzeroberflächenelemente in jede Art von Seite

In den folgenden Abschnitten finden Sie Beispiele für HTML5 Fragmente, die in Azure AD B2C zusammengeführt der `<div id="api"></div>` Element befindet sich in den Inhalt. **Fügen Sie diese Fragmente nicht in den Inhalt der HTML 5.** Der Dienst Azure AD B2C fügt diese zur Laufzeit. Verwenden Sie diese Beispiele eigene Stylesheets Entwurf aus.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C Inhalt "Identität Anbieter Auswahl Webseite" eingefügt

Diese Seite enthält eine Liste der Identitätsanbieter, die der Benutzer bei der Anmeldung oder Anmeldung auswählen kann. Dies sind entweder für soziale Netzwerke Identitätsanbieter wie Facebook und Google + oder lokale Konten (auf der Grundlage der e-Mail-Adresse oder die Benutzer Namen).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C-Inhalte in das Feld "Anmeldeseite lokales Konto" eingefügt

Diese Seite enthält ein Anmeldeformular, die der Benutzer zum Füllen Sie beim Anmelden für ein lokales Konto, das auf eine e-Mail-Adresse oder einen Benutzernamen basiert hat. Das Formular kann verschiedene Eingabewerte Steuerelemente wie Texteingabefeld, Kennwort Texteingabefeld ein, Optionsfeld, Dropdownfeldern Einfachauswahl und Mehrfachauswahl Kontrollkästchen enthalten.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C Inhalt in "" für soziale Netzwerke Konto Anmeldeseite"eingefügt

Diese Seite enthält ein Anmeldeformular, die der Consumer beim Anmelden, verwenden ein vorhandenes Konto aus einem sozialen Identitätsanbieter wie Facebook oder Google + ausfüllen. Diese Seite ähnelt der lokalen Anmeldung Kontoseite (Siehe im vorherigen Abschnitt) mit Ausnahme der Eingabefelder Kennwort.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C Inhalt "einheitliche Anmeldung oder Anmeldung Webseite eingefügt"

Diese Seite übernimmt Anmeldung und Anmeldung der Verbraucher, wer für soziale Netzwerke Identitätsanbieter wie Facebook oder Google + oder lokale Konten verwenden können.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C Inhalte auf der Seite"kombinierte Authentifizierung" eingefügt

Auf dieser Seite können Benutzer ihre Telefonnummern (mit Text oder VoIP) bei der Anmeldung oder Anmeldung überprüfen.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C Inhalt in der "" Fehlerseite"eingefügt

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Punkte, die Sie beim Erstellen Ihrer eigenen Inhalts beachten sollten

Wenn Sie beabsichtigen, die Seite Benutzeroberfläche Anpassung-Funktion verwenden, lesen Sie die folgenden bewährten Methoden:

- Nicht kopieren Sie Azure AD B2C Standardinhalt, und versuchen Sie, es zu ändern. Es wird empfohlen, Ihre HTML5 Inhalte von Grund auf neu zu erstellen und Standardinhalt als Referenz verwenden.
- In allen Seiten (mit Ausnahme der Fehlerseiten) served die Anmeldung, Anmeldung und Richtlinien Profil bearbeiten, Stylesheets, die Sie bereitstellen, müssen die Standardformatvorlagen außer Kraft setzen, die wir in diese Seiten hinzufügen der <head> Satzteile. In allen Seiten nach der Anmeldung oder Anmeldung und Richtlinien für Kennwörter zurücksetzen und die Fehlerseiten auf alle Richtlinien served, müssen Sie alle die Sucherergebnisse angeben selbst.
- Aus Gründen der Sicherheit können nicht wir Sie JavaScript in den Inhalt enthalten. Die meisten, Sie benötigen sollten sofort verfügbar ist. Wenn dies nicht der Fall ist, verwenden Sie [Benutzer Voicemail](http://feedback.azure.com/forums/169401-azure-active-directory) , um neue Funktionalität anzufordern.
- Unterstützte Browser-Versionen:
    - InternetExplorer 11
    - InternetExplorer 10
    - Internet Explorer 9 (begrenzt)
    - Internet Explorer 8 (begrenzt)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0

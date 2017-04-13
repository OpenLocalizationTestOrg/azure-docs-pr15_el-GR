<properties
    pageTitle="Azure Active Directory B2C: Προσαρμογή του περιβάλλοντος εργασίας χρήστη | Microsoft Azure"
    description="Ένα θέμα στη τις δυνατότητες προσαρμογής του περιβάλλοντος εργασίας χρήστη του Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Προσαρμογή του Azure AD B2C περιβάλλοντος εργασίας χρήστη (UI)

Εμπειρία χρήστη είναι υπέρτατη σε μια εφαρμογή καταναλωτή τοποθεσία. Είναι η διαφορά μεταξύ μιας καλής εφαρμογής και ένα εξαιρετικό και μεταξύ απλώς ενεργό καταναλωτών και πραγματικά κατειλημμένο από αυτές. Azure Active Directory (Azure AD) B2C σάς επιτρέπει να προσαρμόσετε καταναλωτή εγγραφής, εισόδου (*δείτε σημείωση παρακάτω*), επεξεργασία, το προφίλ και σελίδες με pixel τέλειο ελέγχου επαναφοράς κωδικού πρόσβασης.

> [AZURE.NOTE]
Προς το παρόν, λογαριασμού τοπικού εισόδου σελίδων, τον κωδικό πρόσβασης του accompaning επαναφορά των σελίδων και μηνύματα ηλεκτρονικού ταχυδρομείου επαλήθευσης μπορεί να προσαρμοστεί μόνο με τη χρήση της [δυνατότητας εμπορικής προσαρμογής εταιρείας](../active-directory/active-directory-add-company-branding.md) και όχι από τους μηχανισμούς που περιγράφονται σε αυτό το άρθρο.

Σε αυτό το άρθρο, θα Διαβάστε σχετικά με:

- Η σελίδα δυνατότητα περιβάλλοντος εργασίας χρήστη (UI) προσαρμογής.
- Ένα εργαλείο που θα σας βοηθήσει να ελέγξετε τη δυνατότητα προσαρμογής περιβάλλοντος εργασίας Χρήστη σελίδας χρησιμοποιώντας το δείγμα περιεχομένου.
- Τα κύρια στοιχεία περιβάλλοντος εργασίας Χρήστη σε κάθε τύπο σελίδας.
- Βέλτιστες πρακτικές κατά την άσκηση αυτήν τη δυνατότητα.

## <a name="the-page-ui-customization-feature"></a>Η δυνατότητα προσαρμογής σελίδα περιβάλλοντος εργασίας Χρήστη

Με τη δυνατότητα προσαρμογής σελίδα περιβάλλοντος εργασίας Χρήστη, μπορείτε να προσαρμόσετε την εμφάνιση και αίσθηση του καταναλωτή εγγραφής, εισόδου, επαναφορά του κωδικού πρόσβασης και επεξεργασίας προφίλ σελίδες (με ρύθμιση των παραμέτρων [πολιτικών](active-directory-b2c-reference-policies.md)). Οι χρήστες του θα έχουν απρόσκοπτη εμπειρίες κατά την περιήγηση μεταξύ της εφαρμογής και των σελίδων που σερβιρίστηκε από Azure AD B2C.

Σε αντίθεση με άλλες υπηρεσίες όπου επιλογές περιβάλλοντος εργασίας Χρήστη περιορίζονται ή διατίθενται μόνο μέσω APIs, Azure AD B2C χρησιμοποιεί μια προσέγγιση σύγχρονα (και απλοποίησης) για την προσαρμογή περιβάλλοντος εργασίας Χρήστη της σελίδας.

Ακολουθεί πώς λειτουργεί: Azure AD B2C εκτελεί κώδικα στο πρόγραμμα περιήγησης του καταναλωτή και χρησιμοποιεί μια σύγχρονη προσέγγιση που ονομάζεται [Κοινή χρήση πόρων μεταξύ Origin (CORS)](http://www.w3.org/TR/cors/) για να φορτώσετε περιεχόμενο από μια διεύθυνση URL που καθορίζετε σε μια πολιτική. Μπορείτε να καθορίσετε διαφορετικές διευθύνσεις URL για διαφορετικές σελίδες. Ο κώδικας συγχωνεύει στοιχεία περιβάλλοντος εργασίας Χρήστη από το Azure AD B2C με το περιεχόμενο που φορτώνεται από τη διεύθυνση URL και εμφανίζει τη σελίδα σας καταναλωτή. Το μόνο που πρέπει να κάνετε είναι:

1. Δημιουργία HTML5 μορφοποιηθεί σωστά με το περιεχόμενο ενός `<div id="api"></div>` στοιχείο (πρέπει να είναι ένα κενό στοιχείο) που βρίσκεται σε ένα σημείο στο το `<body>`. Σε αυτό το στοιχείο σημάδια όπου έχει εισαχθεί το περιεχόμενο Azure AD B2C.
2. Φιλοξενία του περιεχομένου σε ένα τελικό σημείο HTTPS (με CORS επιτρέπεται).
3. Στυλ για τα στοιχεία περιβάλλοντος εργασίας Χρήστη που Azure AD B2C εισάγει στο.

## <a name="test-out-the-ui-customization-feature"></a>Δοκιμάστε τη δυνατότητα προσαρμογής του περιβάλλοντος εργασίας Χρήστη

Εάν θέλετε να δοκιμάσετε τη δυνατότητα προσαρμογής του περιβάλλοντος εργασίας Χρήστη, χρησιμοποιώντας το δείγμα HTML και CSS περιεχόμενο, σας παρέχουμε που [απλό βοηθητική εφαρμογή του εργαλείου](active-directory-b2c-reference-ui-customization-helper-tool.md) την οποία αποστέλλει ρυθμίζει τις παραμέτρους δείγμα περιεχομένου ο χώρος αποθήκευσης αντικειμένων Blob του Azure.

> [AZURE.NOTE]
Μπορείτε να φιλοξενήσετε σε οποιοδήποτε σημείο του περιεχομένου του περιβάλλοντος εργασίας Χρήστη: σε διακομιστές web, CDN, AWS S3, συστήματα κοινής χρήσης αρχείων, κ.λπ. Με την προϋπόθεση ότι το περιεχόμενο φιλοξενείται σε μια διαθέσιμη στο κοινό τελικού σημείου HTTPS (με CORS επιτρέπεται), είστε έτοιμοι. Χρησιμοποιούμε χώρο αποθήκευσης αντικειμένων Blob του Azure επεξήγηση μόνο.

### <a name="the-most-basic-html-content-for-testing"></a>Το πιο βασικές περιεχόμενο HTML για σκοπούς δοκιμής

Φαίνεται παρακάτω, η πιο βασικές HTML είναι περιεχομένου που μπορείτε να χρησιμοποιήσετε για να δοκιμάσετε αυτήν τη δυνατότητα. Χρησιμοποιήστε το ίδιο εργαλείο Βοήθειας που παρέχεται παραπάνω για την αποστολή και ρύθμιση παραμέτρων αυτό το περιεχόμενο στο χώρο αποθήκευσης αντικειμένων Blob Azure σας. Στη συνέχεια, μπορείτε να επαληθεύσετε ότι το βασικό, μη στιλιστικά κουμπιά & πεδία φόρμας σε κάθε σελίδα είναι εμφανιζόμενη και λειτουργική.

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

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Τα κύρια στοιχεία περιβάλλοντος εργασίας Χρήστη σε κάθε τύπο της σελίδας

Στις ενότητες που ακολουθούν, θα βρείτε παραδείγματα HTML5 τμημάτων που Azure AD B2C συγχωνεύει σε το `<div id="api"></div>` στοιχείο που βρίσκεται στο περιεχόμενό σας. **Μην εισαγάγετε αυτά τα τμήματα στο περιεχόμενό σας HTML 5.** Η υπηρεσία Azure AD B2C εισάγει τους στο κατά το χρόνο εκτέλεσης. Χρησιμοποιήστε αυτά τα παραδείγματα για να σχεδιάσετε τα δικά σας φύλλα στυλ.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C περιεχομένου που έχουν εισαχθεί σε "ταυτότητα παροχής σελίδα επιλογής του"

Αυτή η σελίδα περιέχει μια λίστα με υπηρεσίες παροχής ταυτότητας που μπορούν να επιλέξουν το χρήστη κατά τη διάρκεια εγγραφής ή εισόδου. Αυτές είναι οι υπηρεσίες παροχής ταυτότητας κοινωνικών όπως το Facebook και το Google + ή Τοπικοί λογαριασμοί (που βασίζεται σε μήνυμα ηλεκτρονικού ταχυδρομείου διεύθυνση ή το όνομα χρήστη).

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

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C περιεχομένου που έχουν εισαχθεί σε "τοπικό σελίδα του λογαριασμού εγγραφής"

Αυτή η σελίδα περιέχει μια φόρμα εγγραφής που ο χρήστης πρέπει να συμπληρώσετε Όταν εγγράφεστε ένα τοπικό λογαριασμό που βασίζεται σε μια διεύθυνση ηλεκτρονικού ταχυδρομείου ή ένα όνομα χρήστη. Η φόρμα μπορεί να περιέχει διαφορετικών στοιχείων ελέγχου εισαγωγής όπως πλαίσιο εισαγωγής κειμένου, πλαίσιο εισαγωγής κωδικού πρόσβασης, κουμπί επιλογής, απλής επιλογής αναπτυσσόμενων πλαισίων και πολλαπλής επιλογής των πλαισίων ελέγχου.

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

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C περιεχομένου που έχουν εισαχθεί σε "" κοινωνικών σελίδα του λογαριασμού εγγραφής"

Αυτή η σελίδα περιέχει μια φόρμα εγγραφής που έχει τον καταναλωτή για να συμπληρώσετε κατά την είσοδό σας χρησιμοποιώντας έναν υπάρχοντα λογαριασμό από μια υπηρεσία παροχής ταυτότητας κοινωνικών όπως το Facebook ή το Google +. Αυτή η σελίδα είναι παρόμοια με τη λογαριασμού τοπικού σελίδα εγγραφής (φαίνεται στην προηγούμενη ενότητα) με εξαίρεση τα πεδία καταχώρησης κωδικού πρόσβασης.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C περιεχομένου που έχουν εισαχθεί σε της "ενοποιημένης εγγραφής ή εισόδου σελίδας"

Αυτή η σελίδα χειρίζεται τόσο εγγραφής & εισόδου του καταναλωτή, που μπορούν να χρησιμοποιήσουν υπηρεσίες παροχής ταυτότητας κοινωνικών όπως το Facebook ή Google + ή Τοπικοί λογαριασμοί.

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

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C περιεχόμενο που έχει εισαχθεί σε αυτήν τη σελίδα"Έλεγχος ταυτότητας πολλών παραγόντων"

Σε αυτήν τη σελίδα, οι χρήστες να επαληθεύσετε τους αριθμούς τηλεφώνου (με χρήση κειμένου ή σε φωνητικό) κατά τη διάρκεια εγγραφής ή εισόδου.

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

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C περιεχομένου που έχουν εισαχθεί σε το "" σελίδα σφάλματος"

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

## <a name="things-to-remember-when-building-your-own-content"></a>Πράγματα που πρέπει να θυμάστε κατά τη δημιουργία το δικό σας περιεχόμενο

Εάν σχεδιάζετε να χρησιμοποιήσετε τη δυνατότητα προσαρμογής UI σελίδα, διαβάστε τις ακόλουθες βέλτιστες πρακτικές:

- Δεν αντιγράψτε το Azure AD B2C προεπιλεγμένο περιεχόμενο και επιχειρήσουν να το τροποποιήσουν. Είναι προτιμότερο να δημιουργήσετε το περιεχόμενό σας HTML5 από την αρχή και να χρησιμοποιήσετε το προεπιλεγμένο περιεχόμενο ως αναφορά.
- Σε όλες τις σελίδες (εκτός από τις σελίδες σφάλματος) που σερβιρίστηκε από την είσοδο, εγγραφής και επεξεργασία προφίλ πολιτικές, φύλλα στυλ που παρέχετε θα πρέπει να παρακάμψετε τα προεπιλεγμένα φύλλα στυλ που προσθέσουμε σε αυτές τις σελίδες στο το <head> τμήματα. Σε όλες τις σελίδες που σερβιρίστηκε με την εγγραφή ή εισόδου και πολιτικές επαναφορά κωδικού πρόσβασης και των σελίδων σφάλματος σε όλες τις πολιτικές, θα πρέπει να παρέχουν όλα τα στυλ στον εαυτό σας.
- Για λόγους ασφαλείας, θα σας δεν σας επιτρέπουν να συμπεριλάβετε οποιαδήποτε JavaScript στο περιεχόμενό σας. Περισσότερες από ό, τι χρειάζεστε πρέπει να είναι διαθέσιμες από το πλαίσιο. Εάν όχι, χρησιμοποιήστε [Τη φωνή χρήστη](http://feedback.azure.com/forums/169401-azure-active-directory) για να ζητήσετε νέες λειτουργίες.
- Υποστηριζόμενες εκδόσεις προγραμμάτων περιήγησης:
    - Internet Explorer 11
    - Τον Internet Explorer 10
    - Internet Explorer 9 (περιορισμένη)
    - Internet Explorer 8 (περιορισμένη)
    - Το Google Chrome 43.0
    - Το Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0

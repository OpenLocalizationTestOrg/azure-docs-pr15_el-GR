<properties
    pageTitle="Χρήση υπηρεσίας διαχείρισης API για τη δημιουργία αιτήσεων HTTP"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε πολιτικές αίτησης και απόκρισης API διαχείρισης για να καλέσετε εξωτερικές υπηρεσίες από το API"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Χρήση εξωτερικών υπηρεσιών από την υπηρεσία διαχείρισης API Azure

Τις πολιτικές που είναι διαθέσιμα στην υπηρεσία Azure API διαχείρισης μπορεί να γίνει μια ευρεία ποικιλία χρήσιμες εργασίες με βάση καθαρά η εισερχόμενη αίτηση, την εξερχόμενη απόκριση και βασικές πληροφορίες. Ωστόσο, που μπορείτε να αλληλεπιδράσετε με εξωτερικές υπηρεσίες από το API διαχείρισης πολιτικών ανοίγει πολλές περισσότερες ευκαιρίες.

Θα σας είδατε προηγουμένως πώς θα σας να αλληλεπιδράτε με την [υπηρεσία Azure συμβάν διανομέας για καταγραφή, παρακολούθηση και ανάλυση](api-management-log-to-eventhub-sample.md). Σε αυτό το άρθρο θα σας θα δείχνουν πολιτικές που σας επιτρέπουν να αλληλεπιδράτε με τις εξωτερικές HTTP βάσει υπηρεσίας. Οι πολιτικές αυτές μπορεί να χρησιμοποιηθεί για την ενεργοποίηση της απομακρυσμένης συμβάντα ή για την ανάκτηση πληροφοριών που θα χρησιμοποιηθεί για να χειρίζεστε το αρχικό αίτησης και απόκρισης με κάποιον τρόπο.

## <a name="send-one-way-request"></a>Αποστολή μίας-Τρόπος-αίτηση-
Πιθανώς η απλούστερος εξωτερική επικοινωνία είναι το στυλ fire και ξεχάσετε της αίτησης που επιτρέπει σε μια εξωτερική υπηρεσία για να ειδοποιείστε για κάποιο είδος σημαντικό συμβάν. Μπορούμε να χρησιμοποιήσουμε την πολιτική ελέγχου ροής `choose` για τον εντοπισμό οποιοδήποτε είδος συνθήκη που σας ενδιαφέρει και, στη συνέχεια, εάν η συνθήκη ικανοποιείται, που μπορούν να κάνουμε μια εξωτερική αίτηση HTTP με χρήση της πολιτικής [μία-Τρόπος-αίτηση αποστολής](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Αυτό θα μπορούσε να μια αίτηση για ένα σύστημα ανταλλαγής μηνυμάτων όπως Hipchat ή αδράνεια ή αλληλογραφίας API όπως SendGrid ή MailChimp ή για συμβάντα κρίσιμες υποστήριξης κάτι όπως PagerDuty. Όλα αυτά τα συστήματα ανταλλαγής μηνυμάτων έχουν απλή API HTTP που θα σας να ανακαλέσετε εύκολα.

### <a name="alerting-with-slack"></a>Προειδοποίηση με αδράνεια
Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να στείλετε ένα μήνυμα σε ένα κανάλι συνομιλίας του αδράνειας, εάν ο κωδικός κατάστασης απόκρισης HTTP είναι μεγαλύτερο ή ίσο με 500. Μια περιοχή 500 σφάλμα υποδεικνύει ένα πρόβλημα με μας παρασκηνίου API που το πρόγραμμα-πελάτη του μας API δεν μπορεί να επιλύσει τον εαυτό τους. Απαιτεί συνήθως κάποιο είδος παρέμβαση από το τμήμα.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Αδράνεια έχει την έννοια της άγκιστρα εισερχομένων web. Όταν ρυθμίζετε τις παραμέτρους μιας κλεισίματος εισερχομένων web, η αδράνεια δημιουργεί μια ειδική διεύθυνση URL που σας επιτρέπει να κάνετε μια απλή αίτηση POST και για να μεταβιβάσετε ένα μήνυμα σε το κανάλι αδράνειας. Στο κυρίως σώμα JSON που δημιουργούμε βασίζεται σε μια μορφή που ορίζονται από αδράνεια.

![Κλείσιμο αδράνειας Web](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Είναι fire και ξεχάσετε αρκετά καλή;
Υπάρχουν ορισμένες ισορροπιών κατά τη χρήση στυλ fire και ξεχάσετε της αίτησης. Εάν για κάποιο λόγο, η αίτηση αποτυγχάνει και, στη συνέχεια, την αποτυχία δεν θα αναφερθούν. Σε αυτήν την περίπτωση συγκεκριμένο την πολυπλοκότητα του χρειάζεται μια δευτερεύουσα αποτυχία αναφοράς συστήματος και αναμονή για απάντηση επιπλέον απόδοσης κόστους δεν είναι απαραίτητος. Για σενάρια όπου είναι απαραίτητο για να ελέγξετε την απάντηση, στη συνέχεια, η [αίτηση αποστολής](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) πολιτική είναι καλύτερη επιλογή.

## <a name="send-request"></a>Αίτηση αποστολής
Το `send-request` πολιτική επιτρέπει τη χρησιμοποιώντας μια εξωτερική υπηρεσία για να εκτελέσετε λειτουργίες επεξεργασίας σύνθετη και επιστροφής δεδομένων για το API διαχείρισης την υπηρεσία που μπορούν να χρησιμοποιηθούν για περαιτέρω επεξεργασία πολιτικής.

### <a name="authorizing-reference-tokens"></a>Εξουσιοδότησης διακριτικά αναφοράς
Συνάρτηση κύριων της API διαχείρισης προστατεύει πόρους υποστήριξης. Εάν ο διακομιστής εξουσιοδότησης που χρησιμοποιούνται από το API δημιουργεί [τα διακριτικά JWT](http://jwt.io/) ως μέρος του τη ροή OAuth2, όπως [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , στη συνέχεια, μπορείτε να χρησιμοποιήσετε το `validate-jwt` πολιτικής για να επιβεβαιώσετε την εγκυρότητα του διακριτικού. Ωστόσο, ορισμένες διακομιστές εξουσιοδότησης δημιουργήσετε τι ονομάζονται [αναφορά διακριτικά](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) που δεν μπορεί να επαληθευτεί χωρίς να πραγματοποιήσετε μια κλήση στο διακομιστή εξουσιοδότησης.

### <a name="standardized-introspection"></a>Τυποποιημένη introspection
Στο παρελθόν έχει γίνει τυποποιημένη Τρόπος επαληθεύει ένα διακριτικό αναφοράς με ένα διακομιστή εξουσιοδότησης. Ωστόσο μια προτεινόμενη πρόσφατα τυπική [RFC 7662](https://tools.ietf.org/html/rfc7662) δημοσιεύτηκε από το IETF που καθορίζει τον τρόπο ένα διακομιστή πόρων να επαληθεύσετε την εγκυρότητα ενός διακριτικού.

### <a name="extracting-the-token"></a>Εξαγωγή το διακριτικό
Το πρώτο βήμα είναι να εξαγάγετε το διακριτικό από την κεφαλίδα εξουσιοδότησης. Η τιμή κεφαλίδας πρέπει να μορφοποιηθούν με το `Bearer` συνδυασμός εξουσιοδότησης, ένα διάστημα και, στη συνέχεια, το διακριτικό εξουσιοδότησης σύμφωνα με [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Δυστυχώς, υπάρχουν περιπτώσεις όπου παραλείπεται το συνδυασμό εξουσιοδότησης. Για να υπόψη κατά την ανάλυση, Διαίρεση η τιμή κεφαλίδας σε ένα κενό διάστημα και επιλέξτε τελευταίας συμβολοσειράς από στον επιστρεφόμενο πίνακα συμβολοσειρών. Παρέχει λύση για εσφαλμένη μορφοποιημένο εξουσιοδότησης κεφαλίδες.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Πραγματοποίηση της αίτησης επικύρωσης
Μόλις έχουμε το διακριτικό εξουσιοδότησης, θα σας μπορούν να κάνουν την αίτηση για την επικύρωση το διακριτικό. RFC 7662 καλεί αυτό introspection διαδικασία και απαιτεί που έχετε `POST` φόρμας HTML για τον πόρο introspection. Η φόρμα HTML πρέπει να περιέχει τουλάχιστον ένα ζεύγος κλειδιού/τιμής με τον αριθμό-κλειδί `token`. Αυτή η αίτηση στο διακομιστή εξουσιοδότησης επίσης πρέπει να έχουν πιστοποιηθεί για να βεβαιωθείτε ότι δεν είναι δυνατό να μεταβείτε αλιεία κακόβουλα προγράμματα-πελάτες για έγκυρη διακριτικά.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Έλεγχος της απόκρισης
Το `response-variable-name` χαρακτηριστικό χρησιμοποιείται για να δώσετε πρόσβαση επιστρέφεται απάντηση. Το όνομα που έχει οριστεί σε αυτήν την ιδιότητα μπορούν να χρησιμοποιηθούν ως έναν αριθμό-κλειδί στο το `context.Variables` πρόσβασης στο λεξικό του `IResponse` αντικειμένου.

Από το αντικείμενο απάντησης θα σας μπορεί να ανακτήσει το κυρίως κείμενο και RFC 7622 ενημερώνει μας ότι η απόκριση πρέπει να είναι ένα αντικείμενο JSON και πρέπει να περιέχει τουλάχιστον μια ιδιότητα που ονομάζεται `active` που είναι μια τιμή boolean. Όταν `active` είναι αληθής, στη συνέχεια, το διακριτικό θεωρείται έγκυρο.

### <a name="reporting-failure"></a>Αποτυχία δημιουργίας αναφορών
Χρησιμοποιούμε μια `<choose>` πολιτικής για τον εντοπισμό εάν το διακριτικό δεν είναι έγκυρο και εάν Ναι, επιστρέψτε 401 απάντηση.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Σύμφωνα με [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) που περιγράφει τον τρόπο `bearer` διακριτικά πρέπει να χρησιμοποιείται, θα σας επίσης να επιστρέψει μια `WWW-Authenticate` κεφαλίδας με την απόκριση 401. Το πρόθεμα WWW-τον έλεγχο ταυτότητας προορίζεται για να υποδείξετε ένα πρόγραμμα-πελάτη σχετικά με τη δημιουργία πρόσκλησης σε εξουσιοδοτημένες σωστά. Λόγω του μεγάλη ποικιλία μεθόδους που μπορείτε να κάνετε με το πλαίσιο OAuth2, είναι δύσκολο να επικοινωνούν όλες τις απαραίτητες πληροφορίες. Ευτυχώς υπάρχουν προσπάθειες βρίσκεται σε εξέλιξη και να βοηθήσουν [προγράμματα-πελάτες Ανακαλύψτε πώς μπορείτε να εγκρίνετε σωστά αιτήσεις σε ένα διακομιστή του πόρου](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Λύση ολοκληρωμένου
Εισάγετε όλα τα τμήματα μαζί, λαμβάνουμε την ακόλουθη πολιτική:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Αυτό είναι ένα μόνο από πολλά παραδείγματα του πώς το `send-request` πολιτική μπορεί να χρησιμοποιηθεί για να ενσωματώσετε χρήσιμες εξωτερικές υπηρεσίες τη διαδικασία των προσκλήσεων και απαντήσεων ροή μέσω της υπηρεσίας διαχείρισης API.

## <a name="response-composition"></a>Σύνθεση απάντησης
Το `send-request` πολιτική μπορεί να χρησιμοποιηθεί για τη βελτίωση αίτησης πρωτεύοντος ενός συστήματος υπολογιστή στο παρασκήνιο, όπως θα σας είδατε στο προηγούμενο παράδειγμα ή μπορεί να χρησιμοποιηθεί ως μια πλήρη αντικατάσταση για της κλήσης παρασκηνίου. Χρησιμοποιώντας αυτήν την τεχνική μπορούμε να δημιουργήσουμε εύκολα σύνθετη πόροι που έχουν συναθροιστεί από πολλές διαφορετικές συστήματα.

### <a name="building-a-dashboard"></a>Δημιουργία ενός πίνακα εργαλείων   
Μερικές φορές θέλετε να έχετε τη δυνατότητα για να εκθέσει πληροφορίες που υπάρχει στο πολλά συστήματα παρασκηνίου, για παράδειγμα, για να καθοδηγήσετε ενός πίνακα εργαλείων. Τα KPI προέρχονται από όλα διαφορετικό πίσω-τελειώνει, αλλά προτιμάτε να παρέχει άμεση πρόσβαση σε αυτά και θα ήταν καλό εάν όλες οι πληροφορίες που ήταν δυνατή η ανάκτηση σε μία μόνο αίτηση. Ίσως κάποιες από τις πληροφορίες παρασκηνίου χρειάζεται κάποια σε φέτες και τον τεμαχισμό σε ρολά και πρώτα λίγο sanitizing! Αδυναμία cache αυτόν τον πόρο σύνθετο θα ήταν χρήσιμη η μείωση της φόρτωσης παρασκηνίου, όπως γνωρίζετε ότι οι χρήστες διαθέτουν μια συνήθεια της hammering το πλήκτρο F5 για να δείτε εάν μπορεί να αλλάξει τους underperforming μετρήσεις.    

### <a name="faking-the-resource"></a>Faking τον πόρο
Είναι το πρώτο βήμα για τη δημιουργία πόρων μας πίνακα εργαλείων για να ρυθμίσετε μια νέα λειτουργία στην πύλη του publisher API διαχείρισης. Αυτό θα είναι μια λειτουργία κράτησης θέσης χρησιμοποιείται για να ρυθμίσετε την πολιτική μας σύνθεσης για να δημιουργήσετε μας δυναμικής πόρων.

![Η λειτουργία πίνακα εργαλείων](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Πραγματοποίηση τις αιτήσεις
Μία φορά την `dashboard` έχει δημιουργηθεί η λειτουργία θα σας να ρυθμίσετε τις παραμέτρους μιας πολιτικής ειδικά για αυτήν την λειτουργία. 

![Η λειτουργία πίνακα εργαλείων](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Το πρώτο βήμα είναι να εξαγάγετε τις παραμέτρους του ερωτήματος από την εισερχόμενη αίτηση, έτσι ώστε να σας να προωθείτε τους για να μας παρασκηνίου. Σε αυτό το παράδειγμα μας πίνακα εργαλείων εμφανίζει πληροφορίες με βάση μια χρονική περίοδο ένα επομένως έχει μια `fromDate` και `toDate` παραμέτρου. Μπορούμε να χρησιμοποιήσουμε το `set-variable` πολιτικής για να εξαγάγετε τις πληροφορίες από τη διεύθυνση URL της αίτησης.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Μόλις έχουμε αυτές τις πληροφορίες που μπορούν να κάνουμε προσκλήσεις σε όλα τα συστήματα παρασκηνίου. Κάθε αίτηση δημιουργεί μια νέα διεύθυνση URL με τα στοιχεία των παραμέτρων και καλεί το αντίστοιχο διακομιστή και αποθηκεύει την απάντηση σε μια μεταβλητή περιβάλλοντος.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Αυτές οι αιτήσεις θα εκτελεστεί σε σειρά, που δεν είναι ιδανική. Σε μια προσεχή έκδοση θα σας θα εισαγωγή μια νέα πολιτική που ονομάζεται `wait` που θα Ενεργοποίηση όλων αυτών των αιτήσεων για την εκτέλεση παράλληλα.

### <a name="responding"></a>Απάντηση

Για να δημιουργήσετε σύνθετα απάντηση μπορούμε να χρησιμοποιήσουμε την πολιτική [επιστροφής-απόκρισης](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) . Το `set-body` στοιχείο να χρησιμοποιήσετε μια παράσταση για να δημιουργήσετε μια νέα `JObject` με όλες τις αναπαραστάσεις στοιχείο ενσωματωμένο ως ιδιότητες.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Την πλήρη πολιτική έχει ως εξής:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

Στη ρύθμιση παραμέτρων της θέσης αντικειμένου θα σας να ρυθμίσετε τις παραμέτρους του πόρου πίνακα εργαλείων για να είναι cache για τουλάχιστον μία ώρα, επειδή θα σας κατανοήσετε τη φύση των δεδομένων λειτουργίας σημαίνει ότι ακόμα και αν είναι ενημερωμένο, θα εξακολουθήσει να είναι αρκετά αποτελεσματική για να μεταφέρετε πολύτιμες πληροφορίες στους χρήστες ώρας.

## <a name="summary"></a>Σύνοψη
Azure υπηρεσίας API διαχείρισης παρέχει ευέλικτες πολιτικές που μπορούν να εφαρμοστούν επιλεκτικής σε κυκλοφορία HTTP και σας δίνει τη δυνατότητα σύνθεσης από υπηρεσίες υποστήριξης. Εάν θέλετε να βελτίωση της πύλης API με ειδοποίησης λειτουργίες, επαλήθευση, επικύρωση δυνατότητες ή να δημιουργήσετε νέα σύνθετη πόρους με βάση πολλές υπηρεσίες υποστήριξης, το `send-request` και σχετικές πολιτικές Ανοίξτε στον κόσμο των δυνατοτήτων.

## <a name="watch-a-video-overview-of-these-policies"></a>Παρακολουθήστε ένα βίντεο Επισκόπηση αυτών των πολιτικών
Για περισσότερες πληροφορίες σχετικά με την [Αποστολή μίας-Τρόπος-αίτηση-](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) [αίτηση αποστολής](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)και [επιστροφής-απόκρισης](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) πολιτικές που αναφέρονται σε αυτό το άρθρο, επικοινωνήστε παρακολουθήστε το βίντεο που ακολουθεί.

> [AZURE.VIDEO send-request-and-return-response-policies]

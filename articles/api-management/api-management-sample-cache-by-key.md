<properties
    pageTitle="Προσαρμοσμένο σε cache Azure API διαχείρισης"
    description="Μάθετε πώς μπορείτε να cache στοιχείων με αριθμό-κλειδί Azure API διαχείρισης"
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

# <a name="custom-caching-in-azure-api-management"></a>Προσαρμοσμένο σε cache Azure API διαχείρισης
Azure υπηρεσίας API διαχείρισης έχει ενσωματωμένη υποστήριξη για [προσωρινή αποθήκευση απόκρισης HTTP](api-management-howto-cache.md) χρησιμοποιώντας τη διεύθυνση URL πόρος ως τον αριθμό-κλειδί. Το κλειδί μπορεί να τροποποιηθεί από κεφαλίδες αίτησης χρησιμοποιώντας το `vary-by` ιδιότητες. Αυτό είναι χρήσιμο για προσωρινή αποθήκευση ολόκληρου αποκρίσεις HTTP (γνωστά και ως αναπαραστάσεις), αλλά μερικές φορές είναι χρήσιμο να cache μόνο ένα τμήμα του μια απεικόνιση. Οι νέες πολιτικές [τιμή cache αναζήτησης](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) και [τιμή αποθήκευσης cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) παρέχουν τη δυνατότητα να αποθηκεύετε και να ανακτήσετε αυθαίρετο κομμάτια δεδομένα από το ορισμοί πολιτικής. Αυτή η δυνατότητα προσθέτει τιμή της πολιτικής προηγουμένως θεσπισθέντα [αίτηση αποστολής](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) επειδή μπορείτε τώρα να αποθηκεύσετε προσωρινά απαντήσεων από εξωτερικές υπηρεσίες.

## <a name="architecture"></a>Αρχιτεκτονική  
Υπηρεσία διαχείρισης API χρησιμοποιεί προσωρινής μνήμης δεδομένων κοινόχρηστου ανά μισθωτή, έτσι ώστε, καθώς αλλάζετε το μέγεθος έως πολλές μονάδες μπορείτε θα εξακολουθείτε να έχετε πρόσβαση στην ίδια δεδομένα στο cache. Ωστόσο, όταν εργάζεστε με μια ανάπτυξη πολλών περιοχών υπάρχουν ανεξάρτητη μνήμης cache μέσα σε κάθε μία από τις περιοχές. Λόγω αυτό, είναι σημαντικό να μην αντιμετωπίζει το cache ως χώρος αποθήκευσης δεδομένων, όπου το είναι η μόνη προέλευση κάποια πληροφορία. Εάν κάνατε και αργότερα αποφασίσατε να επωφεληθείτε από την ανάπτυξη πολλών περιοχών, στη συνέχεια, οι πελάτες με τους χρήστες που ταξιδιού μπορεί να χάσουν την πρόσβαση σε αυτά τα δεδομένα στο cache.

## <a name="fragment-caching"></a>Προσωρινή αποθήκευση τμήματος
Υπάρχουν ορισμένες περιπτώσεις όπου απαντήσεων που επιστρέφονται περιέχει ένα τμήμα των δεδομένων που είναι ακριβό για να προσδιορίσετε και να παραμένει ακόμη Φρέσκο για ένα λογικό χρονικό διάστημα. Ως παράδειγμα, εξετάστε το ενδεχόμενο μια υπηρεσία που δημιουργήθηκε από μια αεροπορική εταιρεία που παρέχει πληροφορίες σχετικά με τις κρατήσεις πτήσεων, κατάσταση πτήσεων, κ.λπ. Εάν ο χρήστης είναι μέλος του προγράμματος σημεία αεροπορικών εταιρειών, θα πρέπει επίσης πληροφορίες σχετικά με την τρέχουσα κατάσταση και απόστασης αθροισμένος. Αυτές οι πληροφορίες που σχετίζονται με το χρήστη μπορούν να αποθηκευτούν σε ένα διαφορετικό σύστημα, αλλά μπορεί να θέλετε να συμπεριλάβετε σε απαντήσεις επιστρέφονται σχετικά με την κατάσταση πτήσεων και κρατήσεις. Αυτό μπορεί να γίνει χρησιμοποιώντας μια διεργασία που ονομάζεται τμήμα σε cache. Η κύρια αναπαράσταση μπορεί να επιστραφεί από το διακομιστή προέλευσης χρησιμοποιώντας κάποιο είδος διακριτικού για να υποδείξετε πού είναι οι πληροφορίες που σχετίζονται με το χρήστη να εισαχθεί. 

Εξετάστε την ακόλουθη απόκριση JSON από έναν υπολογιστή στο παρασκήνιο API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

Και δευτερεύοντα πόρων στο `/userprofile/{userid}` που μοιάζει με,

     { "username" : "Bob Smith", "Status" : "Gold" }

Για να προσδιορίσετε το κατάλληλο του χρήστη για να συμπεριλάβετε, πρέπει να προσδιορίσετε ποιος είναι ο τελικός χρήστης. Αυτός ο μηχανισμός είναι εξαρτημένα υλοποίηση. Ως παράδειγμα, χρησιμοποιώ το `Subject` διεκδίκηση από μια `JWT` διακριτικού. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Αποθηκεύει αυτό `enduserid` τιμή σε μια μεταβλητή περιβάλλοντος για μελλοντική χρήση. Το επόμενο βήμα είναι να προσδιορίσετε εάν μια προηγούμενη αίτηση έχει ήδη ανακτώνται τις πληροφορίες χρήστη και αποθηκεύονται στο cache. Για αυτό χρησιμοποιήσουμε το `cache-lookup-value` πολιτικής.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Εάν δεν υπάρχει καταχώρηση στο cache που αντιστοιχεί με την τιμή του κλειδιού, στη συνέχεια, χωρίς `userprofile` μεταβλητή περιβάλλοντος θα δημιουργηθεί. Γίνεται έλεγχος την επιτυχία της αναζήτησης χρησιμοποιώντας το `choose` έλεγχος ροής πολιτικής.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Εάν το `userprofile` μεταβλητή περιβάλλοντος δεν υπάρχει και, στη συνέχεια, θα πρέπει να κάνετε μια αίτηση HTTP για να ανακτήσετε.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Χρησιμοποιούμε το `enduserid` για να δημιουργήσετε τη διεύθυνση URL για τον πόρο προφίλ χρήστη. Μόλις έχουμε την απάντηση, να αποσπάσετε το σώμα κειμένου από την απάντηση και να αποθηκεύσετε ξανά σε μεταβλητή περιβάλλοντος.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Για να αποφύγετε μας χρειάζεται να κάνετε αυτήν την αίτηση HTTP ξανά, όταν ο ίδιος χρήστης κάνει μια άλλη αίτηση, μπορεί να αποθηκεύει το προφίλ χρήστη στο cache.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Αποθηκεύει την τιμή στο cache χρησιμοποιώντας ακριβώς το ίδιο κλειδί που θα σας αρχικά επιχειρήσατε να ανακτήσετε με. Η διάρκεια που θα σας επιλέξτε για να αποθηκεύσετε την τιμή πρέπει να είναι με βάση τον τρόπο συχνά οι αλλαγές πληροφορίες και πώς ανοχή σφαλμάτων χρήστες είναι για ενημερωμένες πληροφορίες. 

Είναι σημαντικό να γνωρίζετε ότι η ανάκτηση από το cache εξακολουθεί να είναι μια εκτός της διαδικασίας, αίτηση δικτύου και ενδεχομένως να την προσθέσετε δεκάδες χιλιοστά του δευτερολέπτου για την αίτηση. Τα πλεονεκτήματα προέρχονται κατά τον καθορισμό τις πληροφορίες του προφίλ χρήστη διαρκεί περισσότερο από που λόγω να χρειάζεται να βάσης δεδομένων ερωτήματα ή συγκέντρωση πληροφοριών από πολλές πίσω-τερματίζει την σημαντικά.

Το τελικό βήμα της διαδικασίας είναι να ενημερώσετε το επιστρεφόμενο απάντηση με μας πληροφορίες προφίλ χρήστη.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Να επιλέξετε να συμπεριλάβετε τα εισαγωγικά ως μέρος του διακριτικού ώστε ακόμα και όταν δεν συμβαίνει αντικατάσταση, η απόκριση ήταν εξακολουθεί να ισχύει JSON. Αυτό ήταν κυρίως για να κάνετε ευκολότερη εντοπισμού.

Αφού συνδυάσετε μαζί όλα αυτά τα βήματα, το τελικό αποτέλεσμα είναι μια πολιτική που να μοιάζει σαν το ακόλουθο.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Αυτή η προσέγγιση σε cache χρησιμοποιείται κυρίως σε τοποθεσίες web όπου HTML αποτελείται στην πλευρά του διακομιστή, έτσι ώστε να αποδίδονται ως μία σελίδα. Ωστόσο, μπορεί να είναι χρήσιμη σε APIs όπου οι υπολογιστές-πελάτες δεν μπορεί να κάνει προγράμματος-πελάτη πλευρά σε cache HTTP ή είναι επιθυμητό δεν για να τοποθετήσετε ότι την ευθύνη του υπολογιστή-πελάτη.

Αυτό το ίδιο είδος σε cache τμήματος μπορεί επίσης να πραγματοποιηθεί στους διακομιστές web παρασκηνίου χρησιμοποιώντας μια Redis σε cache διακομιστή, ωστόσο, με την υπηρεσία διαχείρισης API για την εκτέλεση αυτής της εργασίας είναι χρήσιμη όταν του στο cache τμημάτων προέρχονται από διαφορετικούς τελειώνει πίσω από το κύριο απαντήσεων.

## <a name="transparent-versioning"></a>Διαφανή τήρηση ιστορικού εκδόσεων
Είναι κοινή πρακτική για πολλές εκδόσεις διαφορετική υλοποίηση των API υποστήριξη οποιαδήποτε στιγμή. Αυτό είναι ίσως να υποστηρίζει διαφορετικά περιβάλλοντα, όπως την, δοκιμής, παραγωγής, κ.λπ., ή μπορεί να είναι για την υποστήριξη παλαιότερων εκδόσεων του API για να δώσετε χρόνο για καταναλωτές API για τη μετεγκατάσταση σε νεότερες εκδόσεις. 

Χειρισμός αυτό αντί για που απαιτούν προγραμματιστές προγράμματος-πελάτη για να αλλάξετε τις διευθύνσεις URL από μία προσέγγιση `/v1/customers` να `/v2/customers` είναι για την αποθήκευση δεδομένων προφίλ του καταναλωτή ποια έκδοση του API επιθυμούν αυτήν τη στιγμή για να χρησιμοποιήσετε και να καλέσετε τη διεύθυνση URL κατάλληλο παρασκηνίου. Για να καθορίσετε τη διεύθυνση URL σωστή παρασκηνίου για να καλέσετε για ένα συγκεκριμένο πρόγραμμα-πελάτη, είναι απαραίτητο να ερωτήματος ορισμένα δεδομένα ρύθμισης παραμέτρων. Από την προσωρινή αποθήκευση αυτών των δεδομένων ρύθμισης παραμέτρων, θα σας να ελαχιστοποιήσετε το μειονέκτημα στις επιδόσεις από αυτή αυτήν την αναζήτηση.

Το πρώτο βήμα είναι να προσδιορίσετε το αναγνωριστικό που χρησιμοποιείται για τη ρύθμιση παραμέτρων επιθυμητή έκδοση. Σε αυτό το παράδειγμα, να επιλέξετε να συσχετίσετε την έκδοση για να αριθμού-κλειδιού προϊόντος συνδρομής. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Στη συνέχεια, κάνουμε μιας αναζήτησης cache για να δείτε εάν θα σας ήδη που έχουν ανακτηθεί την έκδοση προγράμματος-πελάτη θέλετε.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Στη συνέχεια, θα σας Ελέγξτε εάν δεν βρέθηκε το στο cache.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Εάν θα σας δεν, στη συνέχεια, θα σας μεταβείτε και να το ανακτήσετε.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Εξαγάγετε το σώμα κειμένου απάντηση από την απάντηση.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Αποθηκεύστε το ξανά στο cache για μελλοντική χρήση.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

Και τέλος ενημερώστε τη διεύθυνση URL υποστήριξης για να επιλέξετε την έκδοση της υπηρεσίας επιθυμητό από το πρόγραμμα-πελάτη.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Η πολιτική εντελώς είναι ως εξής.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Ενεργοποίηση των καταναλωτών API για να ελέγξετε με διαφάνεια ποια έκδοση παρασκηνίου γίνεται πρόσβαση από προγράμματα-πελάτες χωρίς να χρειάζεται να ενημερώστε και αναπτύξτε ξανά προγράμματα-πελάτες είναι μια κομψή λύση που αντιμετωπίζει πολλά API διαχείρισης εκδόσεων ανησυχίες.

## <a name="tenant-isolation"></a>Απομόνωσης μισθωτή

Σε μεγαλύτερες αναπτύξεις πολλών μισθωτών ορισμένες εταιρείες δημιουργήσετε ξεχωριστές ομάδες μισθωτές σε distinct αναπτύξεις υλικού παρασκηνίου. Αυτό ελαχιστοποιεί τον αριθμό των πελατών που επηρεάζονται από ένα θέμα υλικού στον υπολογιστή στο παρασκήνιο. Επιτρέπει επίσης να ανάπτυξης σταδιακά νέες εκδόσεις λογισμικού. Ιδανικά αυτήν την αρχιτεκτονική παρασκηνίου πρέπει να είναι διαφανή API των καταναλωτών. Αυτό μπορεί να είναι δυνατό με παρόμοιο τρόπο σε διαφανές διαχείριση εκδόσεων, επειδή βασίζεται σε την ίδια τεχνική χειρισμού της διεύθυνσης URL παρασκηνίου χρησιμοποιώντας την κατάσταση παραμέτρων ανά API του αριθμού-κλειδιού.  

Αντί να επιστρέφει μια προτιμώμενη έκδοση του API για κάθε κλειδί συνδρομή, θα επιστρέψει ένα αναγνωριστικό που συσχετίζει ένα μισθωτή στην ομάδα υλικού που του έχουν ανατεθεί. Αυτό το αναγνωριστικό μπορεί να χρησιμοποιηθεί για να δημιουργήσετε τη διεύθυνση URL κατάλληλο παρασκηνίου.

## <a name="summary"></a>Σύνοψη
Το ελευθερίας για να χρησιμοποιήσετε το cache Azure API διαχείρισης για την αποθήκευση οποιοδήποτε είδος δεδομένων επιτρέπει επαρκή πρόσβαση στα δεδομένα ρύθμισης παραμέτρων που μπορούν να επηρεάσουν τον τρόπο μια εσωτερική αίτηση υποβάλλεται σε επεξεργασία. Μπορεί επίσης να χρησιμοποιηθεί για την αποθήκευση δεδομένων τμήματα που να ενισχύσετε απαντήσεων, επιστρέφονται από έναν υπολογιστή στο παρασκήνιο API.

## <a name="next-steps"></a>Επόμενα βήματα
Δώστε μας τα σχόλιά σας στο νήμα Disqus για αυτό το θέμα εάν υπάρχουν άλλα σενάρια που έχουν ενεργοποιηθεί οι πολιτικές για εσάς, ή εάν υπάρχουν σενάρια που θα θέλατε να επιτύχετε αλλά κάντε αισθάνεστε δεν είναι δυνατή αυτήν τη στιγμή.

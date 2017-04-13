<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε ιδιότητες στις πολιτικές διαχείρισης API Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε ιδιότητες στις πολιτικές διαχείρισης API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Πώς μπορείτε να χρησιμοποιήσετε ιδιότητες στις πολιτικές διαχείρισης API Azure

Πολιτικές διαχείρισης API είναι μια δυνατότητα του συστήματος που επιτρέπουν τον εκδότη για να αλλάξετε τη συμπεριφορά του API μέσω της ρύθμισης παραμέτρων. Οι πολιτικές είναι μια συλλογή από δηλώσεις που εκτελούνται διαδοχικά στην αίτηση ή απόκριση του API. Προτάσεις πολιτικής είναι δυνατό να δημιουργηθεί με το ακριβές κείμενο τιμές, πολιτικής παραστάσεις και ιδιότητες. 

Κάθε παρουσία της υπηρεσίας διαχείρισης API έχει μια συλλογή properties του ζεύγη κλειδιού/τιμής σε η παρουσία της υπηρεσίας. Αυτές οι ιδιότητες μπορεί να χρησιμοποιηθεί για τη Διαχείριση σταθερές τιμές συμβολοσειράς σε όλα τα API ρύθμισης παραμέτρων και τις πολιτικές. Κάθε ιδιότητα περιλαμβάνει τα παρακάτω χαρακτηριστικά.


| Χαρακτηριστικό | Τύπος            | Περιγραφή                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Όνομα      | συμβολοσειρά          | Το όνομα της ιδιότητας. Αυτό μπορεί να περιέχουν μόνο γράμματα, ψηφία, περίοδο, παύλα και χαρακτήρες υπογράμμισης. |
| Τιμή     | συμβολοσειρά          | Η τιμή της ιδιότητας. Αυτό δεν μπορεί να είναι κενό ή να αποτελείται μόνο από κενό διάστημα.                           |
| Μυστικό    | δυαδική τιμή         | Καθορίζει εάν η τιμή είναι μια μυστικό και θα πρέπει να είναι κρυπτογραφημένα ή όχι.                                |
| Ετικέτες      | πίνακας με συμβολοσειράς | Προαιρετικό των ετικετών που όταν που παρέχεται μπορεί να χρησιμοποιηθεί για να φιλτράρετε τη λίστα ιδιοτήτων.                               |

Ιδιότητες έχουν ρυθμιστεί στην πύλη του publisher, στην καρτέλα **Ιδιότητες** . Στο παρακάτω παράδειγμα, τρεις ιδιότητες έχουν ρυθμιστεί.

![Ιδιότητες][api-management-properties]

Τιμές ιδιοτήτων μπορούν να περιέχουν συμβολοσειρές κειμένου και [παραστάσεις πολιτικής](https://msdn.microsoft.com/library/azure/dn910913.aspx). Ο παρακάτω πίνακας εμφανίζει τις προηγούμενες τρεις δείγμα ιδιότητες και τα χαρακτηριστικά τους. Η τιμή του `ExpressionProperty` είναι μια πολιτική παράσταση που επιστρέφει μια συμβολοσειρά που περιέχει την τρέχουσα ημερομηνία και ώρα. Η ιδιότητα `ContosoHeaderValue` έχει επισημανθεί ως ένα μυστικό, ώστε η τιμή δεν εμφανίζεται.

| Όνομα               | Τιμή                      | Μυστικό | Ετικέτες    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | FALSE  | Το contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | TRUE   | Το contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | FALSE  |         |

## <a name="to-use-a-property"></a>Για να χρησιμοποιήσετε μια ιδιότητα

Για να χρησιμοποιήσετε μια ιδιότητα σε μια πολιτική, τοποθετήστε το όνομα της ιδιότητας μέσα σε ένα ζεύγος διπλές αγκύλες όπως `{{ContosoHeader}}`, όπως φαίνεται στο παρακάτω παράδειγμα.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

Σε αυτό το παράδειγμα, `ContosoHeader` χρησιμοποιείται ως το όνομα της κεφαλίδας σε μια `set-header` πολιτικής, και `ContosoHeaderValue` χρησιμοποιείται ως η τιμή αυτής της κεφαλίδας. Κατά την αξιολόγηση της αυτή την πολιτική κατά τη διάρκεια μιας αίτησης ή απόκρισης για την πύλη διαχείρισης API, `{{ContosoHeader}}` και `{{ContosoHeaderValue}}` αντικαθίστανται με τις τιμές τους αντίστοιχους ιδιότητα.

Ιδιότητες μπορούν να χρησιμοποιηθούν ως ολοκληρωμένη χαρακτηριστικό ή στοιχείο τιμές, όπως φαίνεται στο προηγούμενο παράδειγμα, αλλά αυτές μπορεί επίσης να που έχουν εισαχθεί σε ή σε συνδυασμό με μέρος μιας παράστασης ακριβές κείμενο, όπως φαίνεται στο ακόλουθο παράδειγμα:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Ιδιότητες μπορεί επίσης να περιέχει παραστάσεις πολιτικής. Στο παρακάτω παράδειγμα, το `ExpressionProperty` χρησιμοποιείται.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Κατά την αξιολόγηση αυτή την πολιτική, `{{ExpressionProperty}}` έχει αντικατασταθεί με την τιμή: `@(DateTime.Now.ToString())`. Επειδή η τιμή είναι μια πολιτική παράσταση, αξιολογείται η παράσταση και την πολιτική συνεχίζει με την εκτέλεση.

Μπορείτε να δοκιμάσετε αυτό εκτός στην πύλη για προγραμματιστές καλώντας μια λειτουργία που έχει μια πολιτική με τις ιδιότητες στην εμβέλεια. Στο παρακάτω παράδειγμα, μια λειτουργία ονομάζεται με το προηγούμενο παράδειγμα δύο `set-header` πολιτικές με τις ιδιότητες. Σημειώστε ότι η απόκριση περιέχει δύο προσαρμοσμένες κεφαλίδες που έχουν ρυθμιστεί με χρήση πολιτικών με τις ιδιότητες.

![Πύλη για προγραμματιστές][api-management-send-results]

Αν κοιτάξετε το [API επιθεώρηση ανίχνευσης](api-management-howto-api-inspector.md) για κλήση που περιλαμβάνει τις δύο προηγούμενες πολιτικές δείγμα με τις ιδιότητες, μπορείτε να δείτε τις δύο `set-header` πολιτικές με τις τιμές ιδιοτήτων που έχουν εισαχθεί καθώς και την αξιολόγηση της παράστασης πολιτικής για την ιδιότητα που περιείχε η παράσταση πολιτικής.

![Επιθεώρηση API ανίχνευσης][api-management-api-inspector-trace]

Σημειώστε ότι ενώ τιμές ιδιοτήτων μπορεί να περιέχει παραστάσεις πολιτικής, τιμές ιδιοτήτων δεν είναι δυνατό να περιέχουν άλλες ιδιότητες. Εάν κειμένου που περιέχει μια αναφορά ιδιότητα χρησιμοποιείται για μια ιδιότητα τιμή, όπως είναι οι `Property value text {{MyProperty}}`, αυτήν την ιδιότητα αναφοράς δεν θα αντικατασταθούν και θα συμπεριληφθούν ως τμήμα της η τιμή της ιδιότητας.

## <a name="to-create-a-property"></a>Για να δημιουργήσετε μια ιδιότητα

Για να δημιουργήσετε μια ιδιότητα, κάντε κλικ στην επιλογή **Προσθήκη ιδιοτήτων** στην καρτέλα **Ιδιότητες** .

![Προσθήκη ιδιότητας][api-management-properties-add-property-menu]

**Όνομα** και η **τιμή** είναι απαιτούμενες τιμές. Εάν αυτή η τιμή ιδιότητας είναι ένα μυστικό, επιλέξτε το πλαίσιο ελέγχου **αυτή είναι μια μυστικό** . Πληκτρολογήστε μία ή περισσότερες προαιρετικό ετικέτες για να σας βοηθήσει με την οργάνωση του ιδιότητες και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Προσθήκη ιδιότητας][api-management-properties-add-property]

Κατά την αποθήκευση μιας νέας ιδιότητας, το πλαίσιο κειμένου **αναζήτησης ιδιότητα** συμπληρώνεται με το όνομα της νέας ιδιότητας και εμφανίζεται η νέα ιδιότητα. Για να εμφανίσετε όλες τις ιδιότητες, καταργήστε την επιλογή από το πλαίσιο κειμένου **αναζήτησης ιδιότητα** και πατήστε το πλήκτρο enter.

![Ιδιότητες][api-management-properties-property-saved]

Για πληροφορίες σχετικά με τη δημιουργία ιδιότητας χρησιμοποιώντας το REST API, ανατρέξτε στο θέμα [Δημιουργία ιδιότητας χρησιμοποιώντας το REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Για να επεξεργαστείτε μια ιδιότητα

Για να επεξεργαστείτε μια ιδιότητα, κάντε κλικ στο κουμπί **Επεξεργασία** δίπλα από την ιδιότητα για να επεξεργαστείτε.

![Επεξεργασία ιδιότητας][api-management-properties-edit]

Κάντε τις αλλαγές που θέλετε και κάντε κλικ στην επιλογή **Αποθήκευση**. Εάν αλλάξετε το όνομα της ιδιότητας, τις πολιτικές που αναφέρονται σε αυτήν την ιδιότητα ενημερώνονται αυτόματα για να χρησιμοποιήσετε το νέο όνομα.

![Επεξεργασία ιδιότητας][api-management-properties-edit-property]

Για πληροφορίες σχετικά με την Επεξεργασία ιδιότητας χρησιμοποιώντας το REST API, ανατρέξτε στο θέμα [Επεξεργασία ιδιότητας χρησιμοποιώντας το REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Για να διαγράψετε μια ιδιότητα

Για να διαγράψετε μια ιδιότητα, κάντε κλικ στην επιλογή **Διαγραφή** δίπλα από την ιδιότητα για να διαγράψετε.

![Διαγραφή ιδιότητας][api-management-properties-delete]

Κάντε κλικ στην επιλογή **Ναι, διαγράψτε το** για να τον επιβεβαιώσετε.

![Επιβεβαίωση διαγραφής][api-management-delete-confirm]

>[AZURE.IMPORTANT] Εάν η ιδιότητα γίνεται αναφορά από τις πολιτικές, δεν θα μπορείτε να το διαγράψετε με επιτυχία μέχρι να καταργήσετε την ιδιότητα από όλες τις πολιτικές που χρησιμοποιείτε.

Για πληροφορίες σχετικά με τη διαγραφή μιας ιδιότητας χρησιμοποιώντας το REST API, ανατρέξτε στο θέμα [Διαγραφή ιδιότητας χρησιμοποιώντας το REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Αναζήτηση και φίλτρο ιδιοτήτων

Στην καρτέλα **Ιδιότητες** περιλαμβάνει αναζήτηση και το φιλτράρισμα δυνατότητες που σας βοηθούν να διαχειριστείτε τις ιδιότητες. Για να φιλτράρετε τη λίστα ιδιοτήτων κατά όνομα ιδιότητας, εισαγάγετε έναν όρο αναζήτησης στο πλαίσιο κειμένου **αναζήτησης ιδιότητα** . Για να εμφανίσετε όλες τις ιδιότητες, καταργήστε την επιλογή από το πλαίσιο κειμένου **αναζήτησης ιδιότητα** και πατήστε το πλήκτρο enter.

![Αναζήτηση][api-management-properties-search]

Για να φιλτράρετε τη λίστα ιδιοτήτων ετικέτας τιμών, πληκτρολογήστε μία ή περισσότερες ετικέτες στο πλαίσιο κειμένου **Φιλτράρισμα κατά ετικέτες** . Για να εμφανίσετε όλες τις ιδιότητες, καταργήστε την επιλογή του **φίλτρου από ετικέτες** κειμένου και το πλήκτρο enter.

![Φίλτρο][api-management-properties-filter]

## <a name="next-steps"></a>Επόμενα βήματα

-   Μάθετε περισσότερα σχετικά με την εργασία με τις πολιτικές
    -   [Πολιτικές διαχείρισης API](api-management-howto-policies.md)
    -   [Αναφορά πολιτικής](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Πολιτική παραστάσεις](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Παρακολουθήστε μια επισκόπηση του βίντεο

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

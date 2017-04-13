<properties 
    pageTitle="Πώς μπορείτε να προσαρμόσετε την πύλη για προγραμματιστές Azure API διαχείρισης με τη χρήση προτύπων | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να προσαρμόσετε την πύλη για προγραμματιστές Azure API διαχείρισης με τη χρήση προτύπων." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Πώς μπορείτε να προσαρμόσετε την πύλη για προγραμματιστές Azure API διαχείρισης με τη χρήση προτύπων

Azure API διαχείρισης παρέχει πολλές δυνατότητες προσαρμογής επιτρέπουν στους διαχειριστές να [προσαρμόσετε την εμφάνιση και αίσθηση της πύλης για προγραμματιστές](api-management-customize-portal.md), καθώς και να προσαρμόσετε το περιεχόμενο από τις σελίδες πύλης προγραμματιστής χρησιμοποιώντας ένα σύνολο από πρότυπα που να ρυθμίσετε τις παραμέτρους του περιεχομένου των σελίδων τον εαυτό τους. Χρήση [DotLiquid](http://dotliquidmarkup.org/) σύνταξη και ένα σύνολο παρεχόμενη Τοπικοί πόροι συμβολοσειράς, τα εικονίδια και στοιχεία ελέγχου σελίδας, έχετε μεγάλη ευελιξία για να ρυθμίσετε τις παραμέτρους του περιεχομένου των σελίδων που βλέπετε ότι ταιριάζουν με τη χρήση αυτών των προτύπων.

## <a name="developer-portal-templates-overview"></a>Επισκόπηση προτύπων πύλης για προγραμματιστές

Πρότυπα πύλης για προγραμματιστές πραγματοποιείται από τους διαχειριστές της παρουσίας υπηρεσίας API διαχείρισης στην πύλη για προγραμματιστές. Για να διαχειριστείτε τα πρότυπα για προγραμματιστές, μεταβείτε στο σας παρουσία της υπηρεσίας διαχείρισης API στην πύλη κλασική Azure και κάντε κλικ στο κουμπί **Αναζήτηση**.

![Πύλη για προγραμματιστές][api-management-browse]

Εάν είστε ήδη στην πύλη του publisher, μπορείτε να αποκτήσετε πρόσβαση στην πύλη για προγραμματιστές κάνοντας κλικ στην επιλογή **πύλη για προγραμματιστές**.

![Μενού πύλης για προγραμματιστές][api-management-developer-portal-menu]

Για να αποκτήσετε πρόσβαση τα πρότυπα πύλης προγραμματιστής, κάντε κλικ στο εικονίδιο Προσαρμογή στα αριστερά για να εμφανίσετε το μενού "Προσαρμογή" και κάντε κλικ στην επιλογή **πρότυπα**.

![Πρότυπα πύλης για προγραμματιστές][api-management-customize-menu]

Η λίστα προτύπων εμφανίζει διάφορες κατηγορίες προτύπων που καλύπτεται από διαφορετικές σελίδες στην πύλη προγραμματιστής. Κάθε πρότυπο είναι διαφορετικό, αλλά τα βήματα για να τα επεξεργαστείτε και να δημοσιεύσετε τις αλλαγές είναι η ίδια. Για να επεξεργαστείτε ένα πρότυπο, κάντε κλικ στο όνομα του προτύπου.

![Πρότυπα πύλης για προγραμματιστές][api-management-templates-menu]

Κάνοντας κλικ στην επιλογή πρότυπο μεταφέρει στη σελίδα της πύλης για προγραμματιστές που έχει δυνατότητες προσαρμογής από αυτό το πρότυπο. Σε αυτό το παράδειγμα στη **λίστα προϊόντων** εμφανίζεται το πρότυπο. Το πρότυπο **λίστας προϊόντων** στοιχεία ελέγχου στην περιοχή της οθόνης που υποδεικνύεται από το κόκκινο ορθογώνιο. 

![Το πρότυπο λίστας προϊόντων][api-management-developer-portal-templates-overview]

Ορισμένα πρότυπα, όπως τα πρότυπα **Προφίλ χρήστη** , προσαρμογή διάφορα μέρη του στην ίδια σελίδα. 

![Πρότυπα προφίλ χρήστη][api-management-user-profile-templates]

Το πρόγραμμα επεξεργασίας για κάθε πρότυπο πύλης προγραμματιστής έχει δύο ενότητες που εμφανίζεται στο κάτω μέρος της σελίδας. Στην αριστερή πλευρά εμφανίζει το παράθυρο επεξεργασίας για το πρότυπο και, στη δεξιά πλευρά εμφανίζει το μοντέλο δεδομένων για το πρότυπο. 

Το πρότυπο επεξεργασία παραθύρου περιέχει τις σημάνσεις που ελέγχει την εμφάνιση και τη συμπεριφορά του την αντίστοιχη σελίδα στην πύλη για προγραμματιστές. Η επισήμανση στο πρότυπο χρησιμοποιεί τη σύνταξη [DotLiquid](http://dotliquidmarkup.org/) . Ένα πρόγραμμα επεξεργασίας δημοφιλείς για DotLiquid είναι [DotLiquid για σχεδιαστές](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Τυχόν αλλαγές που κάνατε στο πρότυπο κατά τη διάρκεια της επεξεργασίας εμφανίζονται σε πραγματικό χρόνο στο πρόγραμμα περιήγησης, αλλά δεν είναι ορατή στους πελάτες σας μέχρι να [Αποθήκευση](#to-save-a-template) και [Δημοσίευση](#to-publish-a-template) στο πρότυπο.

![Σήμανση προτύπου][api-management-template]

Παράθυρο **δεδομένων πρότυπο** παρέχει έναν οδηγό στο μοντέλο δεδομένων για το οντοτήτων που είναι διαθέσιμες για χρήση σε ένα συγκεκριμένο πρότυπο. Παρέχει αυτόν τον οδηγό, εμφανίζοντας τα δεδομένα πραγματικού χρόνου που εμφανίζονται τη συγκεκριμένη στιγμή στην πύλη προγραμματιστής. Μπορείτε να αναπτύξετε τα παράθυρα πρότυπο, κάνοντας κλικ στο ορθογώνιο στην επάνω δεξιά γωνία του παραθύρου " **πρότυπο δεδομένων** ".

![Πρότυπο μοντέλο δεδομένων][api-management-template-data]

Στο προηγούμενο παράδειγμα υπάρχουν δύο προϊόντων που εμφανίζεται στην πύλη για προγραμματιστές που ανακτήθηκαν από τα δεδομένα που εμφανίζονται στο παράθυρο " **πρότυπο δεδομένων** ", όπως φαίνεται στο παρακάτω παράδειγμα.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Η επισήμανση στο πρότυπο **λίστα προϊόντων** επεξεργάζεται τα δεδομένα για την παροχή το επιθυμητό αποτέλεσμα με επαναλήψεις μέσω της συλλογής προϊόντων για να εμφανίσετε πληροφορίες και μια σύνδεση σε κάθε μεμονωμένο προϊόν. Σημείωση το `<search-control>` και `<page-control>` στοιχεία σε τις σημάνσεις. Αυτά τα ελέγχου της εμφάνισης την αναζήτηση και σελιδοποίηση στοιχεία ελέγχου στη σελίδα. `ProductsStrings|PageTitleProducts`είναι μια αναφορά μεταφρασμένα συμβολοσειράς που περιέχει το `h2` κείμενο κεφαλίδας για τη σελίδα. Για μια λίστα των συμβολοσειρά πόροι, στοιχεία ελέγχου σελίδας και εικονίδια που είναι διαθέσιμες για χρήση σε πρότυπα πύλης για προγραμματιστές, ανατρέξτε στο θέμα [API διαχείρισης για προγραμματιστές πύλης πρότυπα αναφοράς](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Για να αποθηκεύσετε ένα πρότυπο

Για να αποθηκεύσετε ένα πρότυπο, κάντε κλικ στην επιλογή Αποθήκευση στο πρόγραμμα επεξεργασίας για το πρότυπο.

![Αποθήκευση προτύπου][api-management-save-template]

Αποθηκευμένες αλλαγές δεν είναι πραγματικό στην πύλη για προγραμματιστές μέχρι δημοσίευσή τους.

## <a name="to-publish-a-template"></a>Για να δημοσιεύσετε ένα πρότυπο

Αποθηκευμένο πρότυπα είναι δυνατό να δημοσιευτεί είτε μεμονωμένα είτε όλες μαζί. Για να δημοσιεύσετε ένα πρότυπο μεμονωμένα, κάντε κλικ στην επιλογή δημοσίευση στο πρόγραμμα επεξεργασίας για το πρότυπο.

![Δημοσίευση προτύπου][api-management-publish-template]

Κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε και να κάνετε το πρότυπο live από την πύλη για προγραμματιστές.

![Επιβεβαίωση δημοσίευση][api-management-publish-template-confirm]

Για να δημοσιεύσετε όλες τις εκδόσεις προς το παρόν αδημοσίευτων πρότυπο, κάντε κλικ στην επιλογή **Δημοσίευση** στη λίστα προτύπων. Πρότυπα αδημοσίευτων έχουν καθοριστεί από έναν αστερίσκο μετά το όνομα του προτύπου. Σε αυτό το παράδειγμα, τα πρότυπα **λίστα προϊόντων** και των **προϊόντων** που δημοσιεύονται.

![Δημοσίευση προτύπων][api-management-publish-templates]

Κάντε κλικ στην επιλογή **Δημοσίευση προσαρμογές** για επιβεβαίωση.

![Επιβεβαίωση δημοσίευση][api-management-publish-customizations]

Πρότυπα που μόλις δημοσιευμένη είναι αποτελεσματικές αμέσως στην πύλη για προγραμματιστές.

## <a name="to-revert-a-template-to-the-previous-version"></a>Για να επαναφέρετε ένα πρότυπο για την προηγούμενη έκδοση

Για να επαναφέρετε ένα πρότυπο για την προηγούμενη δημοσιευμένη έκδοση, κάντε κλικ στην επιλογή Επαναφορά στο πρόγραμμα επεξεργασίας για το πρότυπο.

![Επαναφορά προτύπου][api-management-revert-template]

Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση.

![Επιβεβαίωση][api-management-revert-template-confirm]

Την έκδοση που έχει ήδη δημοσιευθεί ενός προτύπου είναι ζωντανή στην πύλη για προγραμματιστές μόλις ολοκληρωθεί η λειτουργία επαναφοράς.

## <a name="to-restore-a-template-to-the-default-version"></a>Για να επαναφέρετε ένα πρότυπο στην προεπιλεγμένη έκδοση

Επαναφορά πρότυπα τους προεπιλεγμένη έκδοση είναι μια διαδικασία δύο βημάτων. Πρώτα πρέπει να είναι δυνατή η επαναφορά των προτύπων και, στη συνέχεια, πρέπει να δημοσιεύονται οι επαναφέρει εκδόσεις.

Για να επαναφέρετε ένα μεμονωμένο πρότυπο στην προεπιλεγμένη έκδοση, κάντε κλικ στην επιλογή Επαναφορά στο πρόγραμμα επεξεργασίας για το πρότυπο.

![Επαναφορά προτύπου][api-management-reset-template]

Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση.

![Επιβεβαίωση][api-management-reset-template-confirm]

Για να επαναφέρετε τις προεπιλεγμένες εκδόσεις όλα τα πρότυπα, κάντε κλικ στην επιλογή **Επαναφορά προεπιλεγμένα πρότυπα** στη λίστα πρότυπο.

![Επαναφορά προτύπων][api-management-restore-templates]

Τα πρότυπα επαναφέρει πρέπει, στη συνέχεια, να δημοσιευτεί μεμονωμένα ή όλες ταυτόχρονα, ακολουθώντας τα βήματα για να [δημοσιεύσετε ένα πρότυπο](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Αναφορά προγραμματιστών πύλης προτύπων

Για αναφορά πληροφορίες για πρότυπα πύλης για προγραμματιστές, πόροι συμβολοσειράς, εικονίδια και στοιχεία ελέγχου σελίδας, ανατρέξτε στο θέμα [αναφορά πύλης πρότυπα για προγραμματιστές API διαχείρισης](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Παρακολουθήστε μια επισκόπηση του βίντεο

Παρακολουθήστε το βίντεο που ακολουθεί για να δείτε πώς μπορείτε να προσθέσετε έναν πίνακα συζητήσεων και χαρακτηρισμούς στις σελίδες API και της λειτουργίας στην πύλη του προγραμματιστή με τη χρήση προτύπων.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png








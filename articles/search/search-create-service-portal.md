<properties
    pageTitle="Δημιουργία μιας υπηρεσίας αναζήτησης Azure με την πύλη Azure | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Μάθετε πώς μπορείτε να παρέχετε μια υπηρεσία αναζήτησης Azure με την πύλη Azure."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Δημιουργία μιας υπηρεσίας αναζήτησης Azure με την πύλη Azure

Αυτός ο οδηγός θα σας καθοδηγήσει στη διαδικασία δημιουργίας (ή παροχής) μιας υπηρεσίας αναζήτησης Azure με την [Πύλη Azure](https://portal.azure.com/).

Αυτός ο οδηγός προϋποθέτει ότι έχετε ήδη μια συνδρομή Azure και να συνδεθείτε στην πύλη του Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Εύρεση Azure αναζήτησης στην πύλη του Azure
1. Μεταβείτε στην [Πύλη του Azure](https://portal.azure.com/) και συνδεθείτε.
1. Κάντε κλικ στο σύμβολο πρόσθεσης ("+") στην επάνω αριστερή γωνία.
2. Επιλέξτε **δεδομένα + χώρος αποθήκευσης**.
3. Επιλέξτε **Αναζήτηση Azure**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Επιλέξτε ένα όνομα υπηρεσίας και τελικού σημείου διεύθυνση URL της υπηρεσίας
1. Το όνομα της υπηρεσίας θα είναι μέρος της διεύθυνσης URL τελικού σημείου της υπηρεσίας αναζήτησης Azure σας βάσει των οποίων θα κάνετε τις κλήσεις API για να διαχειριστείτε και να χρησιμοποιήσετε την υπηρεσία αναζήτησης.
2. Στο πεδίο **διεύθυνση URL** , πληκτρολογήστε το όνομα της υπηρεσίας. Το όνομα της υπηρεσίας:
  * πρέπει να περιέχει μόνο πεζά γράμματα, ψηφία ή παύλες ("-")
  * Δεν μπορείτε να χρησιμοποιήσετε μια παύλα ("-") ως 2 πρώτους χαρακτήρες ή τελευταία μεμονωμένο χαρακτήρα
  * δεν μπορούν να περιέχουν παύλες διαδοχικές ("--")
  * περιορίζεται από 2 έως 60 χαρακτήρες μήκος


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Επιλέξτε μια συνδρομή που θα θέλετε να κρατήσετε την υπηρεσία
Εάν έχετε περισσότερες από μία συνδρομές, μπορείτε να επιλέξετε ποια θα περιλαμβάνει αυτήν την υπηρεσία αναζήτησης Azure.

## <a name="select-a-resource-group-for-your-service"></a>Επιλέξτε μια ομάδα πόρων για την υπηρεσία σας
Δημιουργία νέας ομάδας πόρων ή επιλέξτε ένα υπάρχον. Μια ομάδα πόρων είναι μια συλλογή των υπηρεσιών Azure και τους πόρους που χρησιμοποιούνται μαζί. Για παράδειγμα, εάν χρησιμοποιείτε Azure αναζήτησης για να δημιουργήσετε ευρετήριο σε μια βάση δεδομένων SQL, στη συνέχεια, και τα δύο από αυτές τις υπηρεσίες πρέπει να είναι μέρος της ίδιας ομάδας πόρων.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Επιλέξτε τη θέση όπου θα φιλοξενούνται της υπηρεσίας
Ως υπηρεσία Azure, αναζήτηση Azure είναι διαθέσιμο για να φιλοξενείται σε κέντρα δεδομένων όλο τον κόσμο. Σημείωση που [μπορεί να διαφέρουν από τις τιμές](https://azure.microsoft.com/pricing/details/search/) από Γεωγραφία.

## <a name="select-your-pricing-tier"></a>Επιλέξτε το επίπεδο τιμολόγησης
[Αναζήτηση Azure αυτήν τη στιγμή είναι διαθέσιμο σε πολλά επίπεδα τιμολόγησης](https://azure.microsoft.com/pricing/details/search/): δωρεάν, βασικές ή τυπική. Κάθε επίπεδο έχει το δικό της [δυναμικότητας και τα όρια](search-limits-quotas-capacity.md). Για οδηγίες, ανατρέξτε στο θέμα [επιλογή μια τιμολόγησης σειρά ή SKU](search-sku-tier.md) .

Σε αυτήν την περίπτωση, θα σας έχει επιλέξει το τυπικό επίπεδο για την υπηρεσία μας.

## <a name="select-the-create-button-to-provision-your-service"></a>Επιλέξτε το κουμπί "Δημιουργία" για την παροχή της υπηρεσίας

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Κλιμάκωση της υπηρεσίας

Μετά την παροχή της υπηρεσίας της υπηρεσίας, μπορείτε να κλιμάκωση ώστε να ανταποκρίνεται στις ανάγκες σας. Εάν έχετε επιλέξει την τυπική σειρά της υπηρεσίας αναζήτησης Azure, που μπορούν να κλιμάκωση της υπηρεσίας σε δύο διαστάσεις: αντίγραφα και τα διαμερίσματα. Εάν έχετε επιλέξει τη βασική σειρά, μπορείτε να προσθέσετε μόνο αντίγραφα.

*__Τα διαμερίσματα__* επιτρέπουν την υπηρεσία για την αποθήκευση και αναζήτηση σε περισσότερες έγγραφα.

*__Αντίγραφα__* επιτρέπουν την υπηρεσία για να χειριστείτε μια υψηλότερη φόρτωσης ερωτημάτων αναζήτησης - [μια υπηρεσία απαιτεί 2 αντίγραφα για την επίτευξη μιας SLA μόνο για ανάγνωση και απαιτεί 3 αντίγραφα για την επίτευξη μιας SLA ανάγνωσης/εγγραφής](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Μεταβείτε στην υπηρεσία αναζήτησης Azure διαχείρισης blade στην πύλη του Azure.
2. Στο το blade **Ρυθμίσεις** , επιλέξτε **κλίμακα**.
3. Μπορείτε να κλιμάκωση της υπηρεσίας, προσθέτοντας αντίγραφα ή τα διαμερίσματα.
  * Δεν είναι δυνατό να κλιμακωθεί την υπηρεσία πέρα από τις μονάδες 36 αναζήτησης. Το συνολικό αριθμό μονάδων αναζήτηση είναι το γινόμενο των αντίγραφα και τα διαμερίσματα (αντίγραφα * διαμερίσματα = συνολικό μονάδες αναζήτησης).
  * Εάν έχετε επιλέξει τη βασική σειρά, μπορείτε να κλιμάκωση μόνο 3 αντίγραφα. Βασικές υπηρεσίες είναι συνδεδεμένα σε ένα μεμονωμένο διαμερίσματα.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Επόμενο
Μετά την προμήθεια μιας υπηρεσίας αναζήτησης Azure, θα είστε έτοιμοι για να [ορίσετε ένα ευρετήριο αναζήτησης Azure](search-what-is-an-index.md) , ώστε να μπορείτε να αποστείλετε και να πραγματοποιήσετε αναζήτηση των δεδομένων σας.

Για ένα γρήγορο πρόγραμμα εκμάθησης, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure αναζήτησης στην πύλη](search-get-started-portal.md) .
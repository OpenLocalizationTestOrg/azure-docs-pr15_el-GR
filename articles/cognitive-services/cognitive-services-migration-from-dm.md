
<properties
    pageTitle="Μετεγκατάσταση στο Azure γνωστική υπηρεσίες συστάσεις API από το DataMarket συστάσεις API | Microsoft Azure"
    description="Azure μηχανικής εκμάθησης συστάσεις--μετεγκατάσταση σε συστάσεις γνωστική υπηρεσίας"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Μετεγκατάσταση στο Azure γνωστική υπηρεσίες συστάσεις API από το DataMarket συστάσεις API
Αυτό το άρθρο σας δείχνει πώς μπορείτε να κάνετε μετεγκατάσταση από το [Microsoft DataMarket συστάσεις API](https://datamarket.azure.com/dataset/amla/recommendations) για το [Microsoft Azure γνωστική συστάσεις API των υπηρεσιών](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

Το API συστάσεις DataMarket θα σταματήσει αποδοχή νέων πελατών 31 Δεκεμβρίου 2016, και θα είναι καταργηθεί 28 Φεβρουαρίου 2017.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Πώς μπορώ να ξεκινήσω με χρήση του API συστάσεις γνωστική υπηρεσιών Azure;

Για τη μετεγκατάσταση για το γνωστική API συστάσεις υπηρεσίες, κάντε τα εξής:

1.  Εάν δεν έχετε ήδη μια συνδρομή Azure, [εγγραφείτε](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) για έναν. 

1.  Λάβετε οδηγίες βήμα προς βήμα από τον [Οδηγό γρήγορης εκκίνησης για](cognitive-services-recommendations-quick-start.md) να εγγραφείτε για το γνωστική συστάσεις το API των υπηρεσιών και χρησιμοποιήστε το μέσω προγραμματισμού. 

1.  Η [σελίδα προορισμού γνωστική API συστάσεις υπηρεσιών](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) για να μάθετε περισσότερα σχετικά με την υπηρεσία και να βρείτε τεκμηρίωση μετάβαση.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Να χρησιμοποιείται το περιβάλλον εργασίας Χρήστη συστάσεις για να δημιουργήσετε μοντέλα μου. Υπάρχει ένα παρόμοιο εργαλείο για την γνωστική συστάσεις το API των υπηρεσιών;

Απολύτως! Η διεύθυνση URL για το νέο [Περιβάλλον εργασίας Χρήστη συστάσεις](http://recommendations-portal.azurewebsites.net/) είναι http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Τα διαπιστευτήριά σας DataMarket δεν λειτουργούν εδώ. Εγγραφείτε για μια συνδρομή στην πύλη του Azure και βρείτε το κλειδί λογαριασμού που χρειάζεται να χρησιμοποιήσετε το νέο [Περιβάλλον εργασίας Χρήστη συστάσεις](http://recommendations-portal.azurewebsites.net/).
Εάν δεν έχετε ένα κλειδί λογαριασμού, ανατρέξτε στο θέμα εργασία 1 του [Οδηγού γρήγορης εκκίνησης](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Στη νέα μορφή API είναι το ίδιο με το API συστάσεις DataMarket;

Το API υποστηρίζει το ίδιο σενάρια και διαδικασία ροών ως αυτά τα σενάρια που υποστηρίζονται στην έκδοση DataMarket, αλλά η πραγματική διασύνδεση API έχει ενημερωθεί ώστε να συμφωνούν με τις [Οδηγίες του Microsoft REST API](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Τα API είναι πιο συνεπείς και εργασίας καλύτερα με τα εργαλεία που υποστήριξη Swagger.

Για να κατανοήσετε κάθε ένα από τα API, ανατρέξτε στο θέμα η [Εξερεύνηση API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Χρησιμοποιήστε το *δοκιμάστε* το κουμπί για να δοκιμάσετε μια κλήση API. Η μορφή για τα αρχεία που καταναλώνεται από το API συστάσεις (αρχεία καταλόγου και η χρήση) δεν έχει αλλάξει, ώστε να μπορείτε να συνεχίσετε να χρησιμοποιείτε το ίδιο αρχεία ή/και την υποδομή που έχουν δημιουργηθεί για τη δημιουργία αυτών των αρχείων.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Ποιες είναι ορισμένες νέες δυνατότητες στο γνωστική API συστάσεις υπηρεσιών;

Τα τελευταία δύο μήνες, θα σας έχουν κυκλοφορήσει νέες δυνατότητες για το γνωστική API συστάσεις υπηρεσίες:
-   Συστάσεις περιβάλλοντος εργασίας Χρήστη για την εκπαίδευση και έλεγχος μοντέλα χωρίς να χρειάζεται να συντάξετε μια μονή γραμμή του κώδικα
-   Μαζική βαθμολογίας για την παροχή χιλιάδες συστάσεις ταυτόχρονα
-   Δημιουργία υποστήριξη μετρικά ερώτημα την ποιότητα της συστάσεις μοντέλα
-   Υποστήριξη για επιχειρησιακών κανόνων
-   Δυνατότητα απαρίθμηση και λήψη αρχείων χρήσης και καταλόγου
-   Κατάταξης υποστήριξη Δόμηση ερώτημα για την ποιότητα των δυνατοτήτων του στοιχείου σε ένα μοντέλο συστάσεων
-   Προστέθηκε δυνατότητα για να πραγματοποιήσετε αναζήτηση για ένα προϊόν στον κατάλογο

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Όταν Microsoft διακόψετε την υποστήριξη του API συστάσεις DataMarket;

[Συστάσεις API σε DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) σταματά να δέχεται νέων πελατών μετά τις 31 Δεκεμβρίου 2016, και θα καταργηθεί εντελώς από 28 Φεβρουαρίου 2017. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Τι γίνεται εάν δεν έχω τα δεδομένα που πρέπει να αναδημιουργήσετε μου μοντέλα στο γνωστική API συστάσεις υπηρεσιών;

Θέλουμε να κάνετε αυτό Μετάβαση τόσο εύκολη όσο το δυνατόν πιο για εσάς. Μπορούμε να σας βοηθήσουμε να μετακινήσετε το παλιό μοντέλα από το λογαριασμό σας DataMarket στη συνδρομή σας στο νέο Azure γνωστική συστάσεις το API των υπηρεσιών. Επικοινωνήστε μαζί μας στη [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Θα σας ζητήσουμε να δώσετε τα DataMarket Αναγνωριστικό συνδρομής και το Αναγνωριστικό συνδρομής γνωστική υπηρεσίες, πριν από την μας συσχέτιση των μοντέλων με νέο λογαριασμό σας.

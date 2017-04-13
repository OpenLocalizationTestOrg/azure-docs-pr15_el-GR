<properties
    pageTitle="Πώς μπορείτε να αναπτύξετε μια παρουσία της υπηρεσίας διαχείρισης API Azure σε πολλές περιοχές Azure"
    description="Μάθετε πώς μπορείτε να αναπτύξετε μια παρουσία της υπηρεσίας διαχείρισης API Azure σε πολλές περιοχές Azure." 
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

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Πώς μπορείτε να αναπτύξετε μια παρουσία της υπηρεσίας διαχείρισης API Azure σε πολλές περιοχές Azure

API διαχείρισης υποστηρίζει ανάπτυξη πολλών περιοχών που σας επιτρέπει API εκδότες να διανείμετε μια μεμονωμένη υπηρεσία διαχείρισης API σε οποιονδήποτε αριθμό επιθυμητή Azure περιοχές. Αυτό βοηθά στη μείωση της αίτηση λανθάνων χρόνος θεωρούνται από γεωγραφικά κατανεμημένο API καταναλωτές και επίσης βελτιώνει την υπηρεσία διαθεσιμότητας αν αποσυνδεθεί μία περιοχή. 

Όταν δημιουργηθεί μια υπηρεσία API διαχείρισης αρχικά, περιέχει μόνο μία [μονάδα][] και βρίσκονται σε μια μεμονωμένη Azure περιοχή, τα οποία έχει οριστεί ως η κύρια περιοχή. Πρόσθετες περιοχές μπορούν να προστεθούν εύκολα μέσω Azure κλασική πύλης. Διακομιστής πύλης API διαχείρισης έχει αναπτυχθεί σε κάθε περιοχή και την κυκλοφορία κλήση θα δρομολογούνται στην πλησιέστερη πύλη. Εάν μια περιοχή χωρίς σύνδεση, η κίνηση είναι αυτόματα εκ νέου άμεση στην επόμενη πλησιέστερη πύλη. 

> [AZURE.IMPORTANT] Ανάπτυξη πολλών περιοχών διατίθεται μόνο στο επίπεδο **[Premium][]** .

## <a name="add-region"> </a>Αναπτύξετε μια παρουσία της υπηρεσίας διαχείρισης API σε μια νέα περιοχή

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη του Publisher][api-management-management-console]

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

Μεταβείτε στην καρτέλα **κλίμακα** στην πύλη κλασική Azure για το API διαχείρισης παρουσία της υπηρεσίας. 

![Καρτέλα "κλίμακα"][api-management-scale-service]

Για να αναπτύξετε μια νέα περιοχή, κάντε κλικ στην αναπτυσσόμενη λίστα κάτω από την κύρια περιοχή και επιλέξτε μια περιοχή από τη λίστα.

![Προσθήκη περιοχής][api-management-add-region]

Όταν είναι επιλεγμένη η περιοχή, επιλέξτε τον αριθμό μονάδων για τη νέα περιοχή από την αναπτυσσόμενη λίστα μονάδα.

![Καθορισμός μονάδες][api-management-select-units]

Όταν έχουν ρυθμιστεί το επιθυμητό περιοχές και τις μονάδες, κάντε κλικ στην επιλογή **Αποθήκευση**.

## <a name="remove-region"> </a>Διαγράψετε μια παρουσία της υπηρεσίας διαχείρισης API από μια περιοχή

Για να καταργήσετε μια παρουσία της υπηρεσίας διαχείρισης API από μια περιοχή, μεταβείτε στην καρτέλα **κλίμακα** στην πύλη κλασική Azure για το API διαχείρισης παρουσία της υπηρεσίας. 

![Καρτέλα "κλίμακα"][api-management-scale-service]

Κάντε κλικ στο **X** στα δεξιά της περιοχής που θέλετε να καταργήσετε.  

![Κατάργηση περιοχής][api-management-remove-region]

Αφού έχουν καταργηθεί τις περιοχές που θέλετε, κάντε κλικ στην επιλογή **Αποθήκευση**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance
[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[μονάδα]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/


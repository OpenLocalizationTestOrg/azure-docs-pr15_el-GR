<properties
    pageTitle="Απεικόνιση και αντιμετώπιση προβλημάτων ανάλυσης ροή εργασιών | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να απεικονίσετε μια ανάλυση ροή διοχέτευσης εργασία για αυτοεξυπηρέτησης αντιμετώπιση προβλημάτων με χρήση της δυνατότητας διάγραμμα Διαγνωστικά."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Απεικόνιση και αντιμετώπιση προβλημάτων ανάλυσης ροής εργασιών

Στην ανάλυση ροής, όπως και με άλλες τεχνολογίες που βασίζεται στο cloud, αντιμετώπιση προβλημάτων μερικές φορές απαιτείται να ερευνήσει γιατί μια εργασία δεν παράγει το αναμενόμενο αποτέλεσμα (ή κανένα αποτέλεσμα για το θέμα). Με αυτό υπόψη, ροή αναλυτικών στοιχείων παρέχει τη δυνατότητα για την απεικόνιση μιας ροής εργασίας. Αυτό είναι επίσης εύχρηστο ως εργαλείο μοντελοποίηση και περιλαμβάνει το όφελος πλευρά για αυτές τις απαίτηση τεκμηρίωση της εργασίας τους.

Στον πίνακα απεικόνιση τα δεδομένα εισόδου είναι ορατές καθώς και το ερώτημα που εκτελείται και, στη συνέχεια, όλα τα εξόδους έχει ρυθμιστεί. Ζητήματα συνδεσιμότητας ή ρύθμισης παραμέτρων μπορεί να γίνει πιο εμφανή και μπορεί να είναι χρήσιμο για να δείτε μια οπτική αναπαράσταση των παραμέτρων σας.

## <a name="using-the-diagnosis-diagram-tool"></a>Χρήση του εργαλείου διάγνωση διαγράμματος

Για να αποκτήσετε πρόσβαση σε αυτό το visualizer, απλώς κάντε κλικ στην επιλογή του κουμπιού "Διάγραμμα διάγνωση" στην το blade "Ρυθμίσεις" από το της ανάλυσης ροής εργασίας.

![Stream-Analytics-Troubleshoot-Visualization-Diagnosis-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Κάθε εισόδου και εξόδου είναι χρωματική κωδικοποίηση για να υποδείξετε την τρέχουσα κατάσταση αυτού του στοιχείου, όπως φαίνεται παρακάτω.

![Stream-Analytics-Troubleshoot-Visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Όταν ο χρήστης θέλει να κοιτάξετε ενδιάμεσου ερωτήματος βήματα για να κατανοήσετε τα μοτίβα ροής δεδομένων μέσα σε μια εργασία, το εργαλείο απεικόνισης παρέχει μια προβολή από την ανάλυση του ερωτήματος σε βήματα του στοιχείου και την ακολουθία ροής. Κάνετε κλικ σε κάθε βήμα ερωτήματος θα εμφανίσει την αντίστοιχη ενότητα σε ένα ερώτημα επεξεργασίας παράθυρο όπως απεικονίζεται. 

![Stream-Analytics-Troubleshoot-Visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)

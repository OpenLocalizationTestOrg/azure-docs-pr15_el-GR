<properties
    pageTitle="Βήμα 6: Πρόσβαση στην υπηρεσία web μηχανικής εκμάθησης | Microsoft Azure"
    description="Η ανάπτυξη λύσης πρόβλεψης αναλυτικές οδηγίες βήμα 6: πρόσβαση σε μια ενεργή υπηρεσία web Azure μηχανικής εκμάθησης."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Αναλυτικές οδηγίες βήμα 6: Πρόσβαση στην υπηρεσία web Azure μηχανικής εκμάθησης

Αυτό είναι το τελευταίο βήμα του αναλυτικές οδηγίες, [ανάπτυξη μιας λύσης πρόβλεψης ανάλυσης στο Azure μηχανικής εκμάθησης](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Δημιουργία χώρου εργασίας μηχανικής εκμάθησης](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Αποστολή υπάρχοντα δεδομένα](machine-learning-walkthrough-2-upload-data.md)
3.  [Δημιουργήστε μια νέα έρευνας](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Εκπαίδευση και αξιολόγηση των μοντέλων](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Ανάπτυξη της υπηρεσίας web](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Πρόσβαση στην υπηρεσία web**

----------

Στο προηγούμενο βήμα σε αυτές τις οδηγίες θα σας αναπτυχθεί μια υπηρεσία web που χρησιμοποιεί το μοντέλο πιστωτική κινδύνου πρόβλεψης. Τώρα, οι χρήστες πρέπει να είναι σε θέση να αποστολή δεδομένων σε αυτό και να λαμβάνετε αποτελέσματα. 

Η υπηρεσία web είναι μια υπηρεσία Azure web που μπορούν να λαμβάνουν και να επιστρέψετε δεδομένα χρησιμοποιώντας το ΥΠΌΛΟΙΠΟ APIs με έναν από τους δύο τρόπους:  

-   **Αίτηση/απάντηση** - ο χρήστης στέλνει μία ή περισσότερες γραμμές δεδομένων πιστωτικής στην υπηρεσία, χρησιμοποιώντας μια πρωτοκόλλου HTTP και η υπηρεσία αποκρίνεται με ένα ή περισσότερα σύνολα των αποτελεσμάτων.
-   **Μαζική εκτέλεσης** - ο χρήστης αποθηκεύει μία ή περισσότερες γραμμές δεδομένων πιστωτικής σε μια αντικειμένων blob του Azure και, στη συνέχεια, αποστέλλει στη θέση blob στην υπηρεσία. Η υπηρεσία αξιολογεί όλες τις γραμμές δεδομένων στο αντικείμενο blob εισαγωγής, αποθηκεύει τα αποτελέσματα σε μια άλλη blob και επιστρέφει τη διεύθυνση URL της αυτό το κοντέινερ.  

Ο πιο γρήγορος και ευκολότερος τρόπος για να αποκτήσετε πρόσβαση στην υπηρεσία web είναι τα πρότυπα εφαρμογών Web διαθέσιμη στο το [Azure Marketplace εφαρμογής Web](https://azure.microsoft.com/marketplace/web-applications/all/).
Αυτά τα πρότυπα εφαρμογών web μπορεί να δημιουργήσει προσαρμοσμένης εφαρμογής web που γνωρίζει εισαγωγής δεδομένων της υπηρεσίας web και τι θα επιστρέψει. Το μόνο που πρέπει να κάνετε είναι να παρέχουν πρόσβαση σε υπηρεσία web και των δεδομένων σας και τα υπόλοιπα κάνει το πρότυπο.

Για περισσότερες πληροφορίες σχετικά με τα πρότυπα εφαρμογών web, ανατρέξτε στο θέμα [ανάλωση μιας υπηρεσίας web Azure μηχανικής εκμάθησης με ένα πρότυπο εφαρμογής web](machine-learning-consume-web-service-with-web-app-template.md).

Μπορείτε επίσης να αναπτύξετε μια προσαρμοσμένη εφαρμογή για πρόσβαση στην υπηρεσία web με χρήση κωδικού starter που παρέχεται για εσάς στο R, C# και Python γλώσσες προγραμματισμού.
Μπορείτε να βρείτε όλες τις λεπτομέρειες πώς [για την εκμετάλλευση μιας υπηρεσίας web Azure μηχανικής εκμάθησης που έχουν δημοσιευτεί από μια μηχανικής εκμάθησης έρευνας](machine-learning-consume-web-services.md).

<properties
    pageTitle="Μια λύση πρόβλεψης για πιστωτικό κίνδυνο με μηχανικής εκμάθησης | Microsoft Azure"
    description="Μια λεπτομερή ανάλυση που δείχνει πώς μπορείτε να δημιουργήσετε μια λύση πρόβλεψης ανάλυσης για την εκτίμηση κινδύνου πιστωτικής στο Azure μηχανικής εκμάθησης Studio."
    keywords="πιστωτικό κίνδυνο, λύση πρόβλεψης ανάλυση, εκτίμηση κινδύνου"
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Αναλυτικές οδηγίες: Ανάπτυξη μιας λύσης πρόβλεψης ανάλυσης για την αξιολόγηση κινδύνου πιστωτικής στο Azure μηχανικής εκμάθησης

Ας υποθέσουμε ότι πρέπει να πρόβλεψης ενός ατόμου πιστωτικό κίνδυνο με βάση τις πληροφορίες που σας δίνουν σε μια εφαρμογή πιστωτικής.  

Εκτίμηση κινδύνου πιστωτικής είναι σύνθετη πρόβλημα, φυσικά, αλλά ας απλοποιήσετε λίγο τις παραμέτρους της ερώτησης. Στη συνέχεια, θα σας να το χρησιμοποιήσετε ως παράδειγμα πώς μπορείτε να χρησιμοποιήσετε Microsoft Azure μηχανικής εκμάθησης με μηχανικής εκμάθησης Studio και την υπηρεσία web μηχανικής εκμάθησης για να δημιουργήσετε μια τέτοια λύση πρόβλεψης ανάλυσης.  

Σε αυτές τις λεπτομερείς οδηγίες, θα σας θα ακολουθήσετε τη διαδικασία ανάπτυξης μοντέλου πρόβλεψης ανάλυση μηχανικής εκμάθησης Studio και, στη συνέχεια, την ανάπτυξη ως υπηρεσία web Azure μηχανικής εκμάθησης. Θα ξεκινήσετε με δεδομένα του κινδύνου διαθέσιμη στο κοινό πιστωτικής, αναπτύσσουμε και εκπαίδευση μοντέλου πρόβλεψης που βασίζονται σε αυτά τα δεδομένα, και, στη συνέχεια, να αναπτύξετε το μοντέλο ως υπηρεσία web που μπορούν να χρησιμοποιηθούν από άλλους για πιστωτική εκτίμηση κινδύνου.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Για να κάνετε λήψη και να εκτυπώσετε ένα διάγραμμα που παρέχει μια επισκόπηση των δυνατοτήτων του μηχανικής εκμάθησης Studio, ανατρέξτε στο θέμα [Επισκόπηση διαγράμματος Azure μηχανικής εκμάθησης Studio δυνατότητες](machine-learning-studio-overview-diagram.md).

Για να δημιουργήσετε μια λύση εκτίμηση κινδύνου πιστωτικής, θα σας θα ακολουθήσετε τα παρακάτω βήματα:  

1.  [Δημιουργία χώρου εργασίας μηχανικής εκμάθησης](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Αποστολή υπάρχοντα δεδομένα](machine-learning-walkthrough-2-upload-data.md)
3.  [Δημιουργήστε μια νέα έρευνας](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Εκπαίδευση και αξιολόγηση των μοντέλων](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Ανάπτυξη της υπηρεσίας web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Πρόσβαση στην υπηρεσία web](machine-learning-walkthrough-6-access-web-service.md)

Αυτόν τον Οδηγό βασίζεται σε μια απλοποιημένη εκδοχή του το [δυαδικό Classfication: πιστωτικής πρόβλεψη κινδύνου](http://go.microsoft.com/fwlink/?LinkID=525270) δείγμα έρευνας στη [Συλλογή πληροφοριών Cortana](http://gallery.cortanaintelligence.com/).

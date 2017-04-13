<properties
    pageTitle="Επιτρέπουν την εφαρμογής απόρρητο Azure στοίβας κλειδί θάλαμο revtrieve | Microsoft Azure"
    description="Χρήση εφαρμογής δείγμα για να εργαστείτε με θάλαμο κλειδί στοίβας Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Εκτελέστε το δείγμα εφαρμογής για το πλήκτρο θάλαμο 

Σε αυτόν τον οδηγό, θα χρησιμοποιήσετε ένα δείγμα εφαρμογής για να ανακτήσετε απορρήτου και τους κωδικούς πρόσβασης από θάλαμο αριθμού-κλειδιού.

## <a name="download-the-samples-and-prepare"></a>Κάντε λήψη των δειγμάτων και προετοιμασία

Κάντε λήψη των δειγμάτων προγράμματος-πελάτη Azure θάλαμο αριθμού-κλειδιού από τη [σελίδα δείγματα θάλαμο κλειδί Azure προγράμματος-πελάτη](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Εξαγάγετε τα περιεχόμενα του αρχείου .zip στον τοπικό σας υπολογιστή.

Διαβάστε το αρχείο **README.md** (αυτό είναι ένα αρχείο κειμένου) και, στη συνέχεια, ακολουθήστε τις οδηγίες.

## <a name="run-sample-1--hellokeyvault"></a>Εκτέλεση δείγμα #1--HelloKeyVault
HelloKeyVault είναι μια εφαρμογή κονσόλας που παρουσιάζει τα βασικά σενάρια που υποστηρίζονται από θάλαμο αριθμού-κλειδιού:

  1. Δημιουργία και εισαγωγή έναν αριθμό-κλειδί (πλήκτρο λογισμικού ή HSM)
  2. Κρυπτογράφηση μιας μυστικό χρησιμοποιώντας ένα πλήκτρο περιεχομένου
  3. Αναδίπλωση του περιεχομένου κλειδιού χρησιμοποιώντας έναν αριθμό-κλειδί θάλαμο αριθμού-κλειδιού
  4. Χωρίς αναδίπλωση τον αριθμό-κλειδί περιεχομένου
  5. Το μυστικό αποκρυπτογράφηση
  6. Ορίστε ένα μυστικό

Αυτή η εφαρμογή κονσόλας θα πρέπει να εκτελούνται χωρίς αλλαγές, με τη διαφορά ότι οι ρυθμίσεις κατάλληλη ρύθμιση παραμέτρων στο App.Config θα ενημερωθούν σύμφωνα με τα παρακάτω βήματα:

1. Ενημερώστε τις ρυθμίσεις παραμέτρων εφαρμογής σε HelloKeyVault\App.config με σας διεύθυνση URL θάλαμο κεφαλαίου Αναγνωριστικό εφαρμογής και μυστικό. Οι πληροφορίες μπορεί να δημιουργηθεί προαιρετικά χρησιμοποιώντας **scripts\GetAppConfigSettings.ps1**.
2. Ενημερώστε τις τιμές των υποχρεωτικές μεταβλητών στη GetAppConfigSettings.ps1.
3. Εκκίνηση του παραθύρου του Windows PowerShell.
4. Εκτελέστε τη δέσμη ενεργειών GetAppConfigSettings.ps1 μέσα στο παράθυρο του PowerShell.
5. Αντιγράψτε τα αποτελέσματα της δέσμης ενεργειών στο αρχείο HelloKeyVault\App.config.


## <a name="next-steps"></a>Επόμενα βήματα

[Ανάπτυξη μια Εικονική με κωδικό πρόσβασης θάλαμο αριθμού-κλειδιού](azure-stack-kv-deploy-vm-with-secret.md)

[Ανάπτυξη μια Εικονική με ένα πιστοποιητικό θάλαμο αριθμού-κλειδιού](azure-stack-kv-push-secret-into-vm.md)
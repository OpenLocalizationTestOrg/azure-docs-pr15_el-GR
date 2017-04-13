<properties
    pageTitle="Προ-φόρτωση περιουσιακών στοιχείων σε ένα τελικό σημείο Azure CDN | Microsoft Azure"
    description="Μάθετε πώς να προ-φόρτωση στο cache περιεχομένου σε ένα τελικό σημείο CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Προ-φόρτωση περιουσιακών στοιχείων σε ένα τελικό σημείο Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Από προεπιλογή, στοιχεία αποθηκεύονται στο cache πρώτα όπως ζητούνται. Αυτό σημαίνει ότι η πρώτη αίτηση από κάθε περιοχής μπορεί να διαρκέσει περισσότερο, επειδή δεν θα έχουν τους διακομιστές άκρο του περιεχομένου στο cache και θα πρέπει να προωθήσετε την αίτηση στο διακομιστή προέλευσης. Προ-φόρτωση περιεχομένου αποφεύγεται αυτό πρώτη επισκέψεων λανθάνοντος χρόνου.

Εκτός από την παροχή μια καλύτερη εμπειρία πελατών, πριν από τη φόρτωση στο cache τους πόρους σας μπορεί επίσης να μειώσει κίνηση του δικτύου στο διακομιστή προέλευσης.

> [AZURE.NOTE] Πριν από τη φόρτωση περιουσιακών στοιχείων είναι χρήσιμη για μεγάλο συμβάντα ή το περιεχόμενο που θα είναι διαθέσιμο ταυτόχρονα σε μεγάλο αριθμό χρηστών, όπως μια νέα έκδοση ταινίας ή μια ενημερωμένη έκδοση λογισμικού.

Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε προ-φόρτωση στο cache περιεχομένου σε όλους τους κόμβους άκρη Azure CDN.

## <a name="walkthrough"></a>Αναλυτικές οδηγίες

1. Στην [Πύλη του Azure](https://portal.azure.com), μεταβείτε στο προφίλ CDN που περιέχει το τελικό σημείο που θέλετε να φορτώσετε εκ των προτέρων.  Ανοίγει το blade προφίλ.

2. Κάντε κλικ στην επιλογή το τελικό σημείο στη λίστα.  Ανοίγει το blade τελικού σημείου.

3. Από το τελικό σημείο blade CDN, κάντε κλικ στο κουμπί φόρτωση.

    ![CDN blade τελικού σημείου](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Ανοίγει η φόρτωση blade.

    ![CDN φόρτωσης blade](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Πληκτρολογήστε την πλήρη διαδρομή του κάθε περιουσιακών στοιχείων που θέλετε να φορτώσετε (π.χ., `/pictures/kitten.png`) στο πλαίσιο κειμένου **διαδρομή** .

    > [AZURE.TIP] Περισσότερα πλαίσια κειμένου **διαδρομή** θα εμφανίζονται μετά την εισαγωγή του κειμένου ώστε να μπορείτε να δημιουργήσετε μια λίστα με πολλά στοιχεία.  Μπορείτε να διαγράψετε στοιχεία από τη λίστα κάνοντας κλικ στο κουμπί με τα αποσιωπητικά (...).
    >
    > Διαδρομές πρέπει να είναι μια σχετική διεύθυνση URL που ταιριάζει με την ακόλουθη [κανονική έκφραση](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Κάθε πάγιο πρέπει να έχει το δικό της διαδρομής.  Δεν υπάρχει λειτουργία μπαλαντέρ για προ-φόρτωση περιουσιακών στοιχείων.

    ![Κουμπί φόρτωσης](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Κάντε κλικ στο κουμπί **Φόρτωση** .

    ![Κουμπί φόρτωσης](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Υπάρχει περιορισμός των αιτήσεων 10 φόρτωσης ανά λεπτό ανά CDN προφίλ.

## <a name="see-also"></a>Δείτε επίσης
- [Εκκαθάριση τελικού σημείου Azure CDN](cdn-purge-endpoint.md)
- [Azure CDN REST API αναφορά - εκκαθάριση ή προ-φόρτωση ενός ορίου](https://msdn.microsoft.com/library/mt634451.aspx)
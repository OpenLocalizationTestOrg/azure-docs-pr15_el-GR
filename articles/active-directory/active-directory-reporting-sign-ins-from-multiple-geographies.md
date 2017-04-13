<properties
    pageTitle="Πραγματοποιήστε είσοδο πρόσθετα από πολλά geographies"
    description="Μια αναφορά που δηλώνει χρήστες όπου δύο είσοδος πρόσθετα φαίνεται να προέρχονται από διαφορετικές περιοχές και το χρονικό διάστημα μεταξύ το σύμβολο πρόσθετα δεν επιτρέπει στο χρήστη να έχει διανύσει ανάμεσα σε αυτές τις περιοχές."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-from-multiple-geographies"></a>Σύμβολο πρόσθετα από πολλά geographies

Αυτή η αναφορά περιλαμβάνει επιτυχής πρόσθετα εισόδου από ένα χρήστη, όπου δύο πρόσθετα εισόδου φαίνεται να προέρχονται από διαφορετικές περιοχές και το χρονικό διάστημα μεταξύ τα πρόσθετα εισόδου δεν επιτρέπει στο χρήστη να έχουν που διανύθηκαν ανάμεσα σε αυτές τις περιοχές. Οι πιθανές αιτίες περιλαμβάνουν:

- Χρήστη είναι η κοινή χρήση τον κωδικό πρόσβασής τους με άλλους χρήστες

- Χρήστης χρησιμοποιεί μια σύνδεση απομακρυσμένης επιφάνειας εργασίας για να ξεκινήσετε ένα πρόγραμμα περιήγησης web για είσοδο

- Ένας εισβολέας έχει πραγματοποιήσει είσοδο με το λογαριασμό χρήστη από διαφορετική χώρα

- Χρήστης χρησιμοποιεί ένα VPN ή διακομιστή μεσολάβησης

- Χρήστης είναι συνδεδεμένος στο από πολλές συσκευές την ίδια στιγμή, όπως έναν επιτραπέζιο υπολογιστή και σε κινητό τηλέφωνο, και η διεύθυνση IP του κινητού τηλεφώνου είναι ασυνήθιστο.

Αποτελέσματα από αυτήν την αναφορά θα σας δείξουν την επιτυχή εισόδου συμβάντα, μαζί με την ώρα μεταξύ τα πρόσθετα εισόδου, τις περιοχές όπου τα πρόσθετα εισόδου φαίνεται να προέρχονται από και το χρόνο ταξιδιού εκτιμώμενη ανάμεσα σε αυτές τις περιοχές. Το χρόνο ταξιδιού φαίνεται είναι μόνο μια εκτίμηση και μπορεί να διαφέρει από το χρόνο ταξιδιού πραγματική μεταξύ των θέσεων.


![Πραγματοποιήστε είσοδο πρόσθετα από πολλά geographies](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)
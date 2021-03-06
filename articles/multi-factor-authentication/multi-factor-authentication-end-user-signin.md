<properties
    pageTitle="Azure εμπειρία εισόδου MFA με έλεγχο ταυτότητας πολλών παραγόντων Azure"
    description="Αυτή η σελίδα θα σας δώσει οδηγίες σε πού να απευθυνθείτε για να δείτε τις διάφορες μεθόδους εισόδου που είναι διαθέσιμη με το Azure MFA."
    keywords="έλεγχος ταυτότητας χρήστη, εμπειρία εισόδου, πραγματοποιήστε είσοδο με το κινητό τηλέφωνο, πραγματοποιήστε είσοδο με το τηλέφωνο του γραφείου"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Είσοδος εμπειρία με έλεγχο ταυτότητας πολλών παραγόντων Azure
> [AZURE.NOTE]  Την ακόλουθη τεκμηρίωση που παρέχεται σε αυτήν τη σελίδα εμφανίζει μια τυπική εμπειρία εισόδου.  Για βοήθεια σχετικά με την είσοδο στο ανατρέξτε στο άρθρο [αντιμετωπίζετε προβλήματα με έλεγχο ταυτότητας πολλών παραγόντων Azure](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Τι θα σας εμπειρία εισόδου;
Ανάλογα με το πώς μπορείτε να εισέλθετε και να χρησιμοποιήσετε έλεγχο ταυτότητας πολλών παραγόντων, την εμπειρία σας θα διαφέρουν.  Σε αυτήν την ενότητα παρέχουμε πληροφορίες σχετικά με το τι να περιμένετε κατά την είσοδο.  Επιλέξτε αυτήν που περιγράφει καλύτερα τις ενέργειες:


Τι κάνεις?|Περιγραφή
:------------- | :------------- |
[Είσοδος με τηλέφωνο κινητό ή του office](#signing-in-with-mobile-or-office-phone) | Αυτό είναι τι μπορείτε να περιμένετε από είσοδο χρησιμοποιώντας το τηλέφωνο κινητό ή του office.
[Είσοδος με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας με χρήση ειδοποιήσεων](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Αυτό είναι το τι μπορείτε να περιμένετε χρησιμοποιώντας την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας με τις ειδοποιήσεις.
[Είσοδος με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας με κωδικό επαλήθευσης](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Αυτό είναι το τι μπορείτε να περιμένετε με χρήση του ελέγχου ταυτότητας Microsoft thapp με έναν κωδικό επαλήθευσης.
[Είσοδος με μια εναλλακτική μέθοδο](#signing-in-with-an-alternate-method)|Αυτό θα σας δείξουν τι να περιμένετε εάν θέλετε να χρησιμοποιήσετε μια εναλλακτική μέθοδο.

## <a name="signing-in-with-mobile-or-office-phone"></a>Είσοδος με τηλέφωνο κινητό ή του office

Οι ακόλουθες πληροφορίες θα περιγράφουν την εμπειρία με τη χρήση ελέγχου ταυτότητας πολλαπλών παραγόντων με κινητό ή του office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Για να συνδεθείτε με μια κλήση σε γραφείο σας ή το κινητό τηλέφωνο

- Είσοδος σε μια εφαρμογή ή υπηρεσία, όπως Office 365 χρησιμοποιώντας το όνομα χρήστη και τον κωδικό πρόσβασης.
- Microsoft θα καλέσει.

![Κλήσεις της Microsoft](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Απαντήστε σε τηλέφωνο και να πατήσετε το πλήκτρο #.

![Απάντηση](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Θα πρέπει τώρα να έχετε πραγματοποιήσει είσοδο.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Είσοδος με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας με χρήση ειδοποιήσεων

Οι ακόλουθες πληροφορίες θα περιγράφουν την εμπειρία με τη χρήση ελέγχου ταυτότητας πολλαπλών παραγόντων με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας όταν αποστέλλετε μια ειδοποίηση.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Για να συνδεθείτε με μια ειδοποίηση αποστέλλονται την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας

- Είσοδος σε μια εφαρμογή ή υπηρεσία, όπως Office 365 χρησιμοποιώντας το όνομα χρήστη και τον κωδικό πρόσβασης.
- Η Microsoft θα στείλει μια ειδοποίηση.

![Microsoft στέλνει την ειδοποίηση](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Απαντήστε σε τηλέφωνο και να πατήσετε το πλήκτρο επαλήθευση.  Εάν η εταιρεία σας απαιτεί ένα PIN που θα σας ζητηθεί για αυτό εδώ.

![Επαλήθευση](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Το πρόγραμμα εγκατάστασης](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Θα πρέπει τώρα να έχετε πραγματοποιήσει είσοδο.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Είσοδος με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας με κωδικό επαλήθευσης

Οι ακόλουθες πληροφορίες θα περιγράφουν την εμπειρία με τη χρήση ελέγχου ταυτότητας πολλαπλών παραγόντων με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας όταν χρησιμοποιείτε με έναν κωδικό επαλήθευσης.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Για να συνδεθείτε χρησιμοποιώντας έναν κωδικό επαλήθευσης με την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας

- Είσοδος σε μια εφαρμογή ή υπηρεσία, όπως Office 365 χρησιμοποιώντας το όνομα χρήστη και τον κωδικό πρόσβασης.
- Microsoft θα γίνεται ερώτηση για έναν κωδικό επαλήθευσης.

![Εισαγάγετε κωδικό επαλήθευσης](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Ανοίξτε την εφαρμογή Microsoft υπηρεσία ελέγχου ταυτότητας στο τηλέφωνό σας και πληκτρολογήστε τον κωδικό στο πλαίσιο όπου που θέλετε να συνδεθείτε.

![Λάβετε τον κωδικό](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Θα πρέπει τώρα να έχετε πραγματοποιήσει είσοδο.


## <a name="signing-in-with-an-alternate-method"></a>Είσοδος με μια εναλλακτική μέθοδο


Στην παρακάτω ενότητα θα σας δείξουν πώς μπορείτε να συνδεθείτε με μια εναλλακτική μέθοδο όταν η κύρια μέθοδος ενδέχεται να μην είναι διαθέσιμη.

### <a name="to-sign-in-with-an-alternate-method"></a>Για να συνδεθείτε με μια εναλλακτική μέθοδο

- Είσοδος σε μια εφαρμογή ή υπηρεσία, όπως Office 365 χρησιμοποιώντας το όνομα χρήστη και τον κωδικό πρόσβασης.
- Επιλέξτε χρήση διαφορετικής επιλογής επαλήθευσης.  Που θα εμφανίζεται με μια επιλογή από διαφορετικές επιλογές. Ο αριθμός που βλέπετε βασιστεί πόσες έχετε το πρόγραμμα εγκατάστασης.

![Χρησιμοποιήστε τη μέθοδο εναλλακτική](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Επιλέξτε μια εναλλακτική μέθοδο και πραγματοποιήστε είσοδο.

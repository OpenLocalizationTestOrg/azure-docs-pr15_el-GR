<properties
    pageTitle="Γρήγορα αποτελέσματα υπηρεσίας παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Γρήγορα αποτελέσματα με μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure
Επαλήθευση δύο βημάτων είναι διαθέσιμη από προεπιλογή για καθολικοί διαχειριστές που έχουν Azure Active Directory και τους χρήστες του Office 365. Ωστόσο, εάν θέλετε να εκμεταλλευτείτε τις [δυνατότητες για προχωρημένους](multi-factor-authentication-whats-next.md) , στη συνέχεια, που θα πρέπει να αγοράσετε την πλήρη έκδοση του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure (MFA).

> [AZURE.NOTE]  Μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure χρησιμοποιείται για να επωφεληθείτε από τις δυνατότητες που παρέχονται από την πλήρη έκδοση του Azure MFA. Πρόκειται για τους χρήστες που **δεν έχουν άδειες χρήσης μέσω Azure MFA, Azure AD Premium, ή EMS**.  Azure MFA Azure AD Premium και EMS περιλαμβάνουν την πλήρη έκδοση του Azure MFA από προεπιλογή.  Εάν έχετε άδειες χρήσης, στη συνέχεια, δεν χρειάζεται μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure.

Μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure είναι απαραίτητη για να κάνετε λήψη του SDK.

> [AZURE.IMPORTANT]  Για να λάβετε το SDK, δημιουργήστε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure ακόμα και αν έχετε Azure MFA, AAD Premium ή EMS άδειες χρήσης.  Εάν δημιουργείτε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure για το σκοπό και αδειών χρήσης που έχετε ήδη, φροντίστε να δημιουργήσετε την υπηρεσία παροχής με το μοντέλο **Ανά χρήστη με δυνατότητα** . Στη συνέχεια, σύνδεση στην υπηρεσία παροχής με τον κατάλογο που περιέχει τις άδειες χρήσης Azure MFA, Azure AD Premium ή EMS.  Αυτό εξασφαλίζει ότι μόνο χρέωσης εάν έχετε περισσότερες Μοναδικοί χρήστες χρησιμοποιώντας το SDK από τον αριθμό των αδειών χρήσης που διαθέτετε.


## <a name="to-create-a-multi-factor-auth-provider"></a>Για να δημιουργήσετε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων

Χρησιμοποιήστε τα ακόλουθα βήματα για να δημιουργήσετε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure.

1. Είσοδος στην [πύλη του Azure κλασική](https://manage.windowsazure.com) ως διαχειριστής.
2. Στα αριστερά, επιλέξτε την **Υπηρεσία καταλόγου Active Directory**.
3. Στη σελίδα υπηρεσίας καταλόγου Active Directory, στο επάνω μέρος, επιλέξτε **Υπηρεσιών παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων**.
![Δημιουργία μιας υπηρεσίας παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Στο κάτω μέρος, κάντε κλικ στην επιλογή **Δημιουργία**.
![Δημιουργία μιας υπηρεσίας παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Στην περιοχή εφαρμογή υπηρεσιών, επιλέξτε **Υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων**
![δημιουργώντας μια υπηρεσία παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Επιλέξτε **Δημιουργία γρήγορης**.
![Δημιουργία μιας υπηρεσίας παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Συμπληρώστε τα παρακάτω πεδία και επιλέξτε **Δημιουργία**.
    1. **Όνομα** -στο όνομα της υπηρεσίας παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων.
    2. **Χρήση μοντέλο** , εάν θέλετε να επιτρέψετε σε μεμονωμένους χρήστες ή πληρωμή ανά επαλήθευσης. Επιλέξτε μία από τις δύο επιλογές:
        - Ανά έλεγχος ταυτότητας – αγοράς μοντέλο ότι οι χρεώσεις ανά ελέγχου ταυτότητας. Χρησιμοποιείται συνήθως για σενάρια που χρησιμοποιούν έλεγχο ταυτότητας πολλών παραγόντων Azure σε μια εφαρμογή καταναλωτή τοποθεσία.
        - Ανά χρήστη ενεργοποιημένο – αγοράς μοντέλου ότι οι χρεώσεις ανά ενεργοποιημένη χρήστη. Χρησιμοποιείται συνήθως για πρόσβασης υπαλλήλου σε εφαρμογές, όπως το Office 365. Ενεργοποιήστε αυτήν την επιλογή εάν έχετε ορισμένοι χρήστες που έχουν ήδη άδεια χρήσης για το Azure MFA.
    2. **Κατάλογος** – μισθωτή του Azure Active Directory που σχετίζεται με την υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων. Λάβετε υπόψη τα εξής:
        - Δεν χρειάζεται μια καταλόγου Azure AD για να δημιουργήσετε μια υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων. Αφήστε το κενό πλαίσιο αν μόνο σκοπεύετε να χρησιμοποιήσετε το διακομιστή ελέγχου ταυτότητας πολλαπλών παραγόντων Azure ή SDK.
        - Η υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων πρέπει να σχετίζονται με έναν κατάλογο Azure AD για να επωφεληθείτε από τις δυνατότητες για προχωρημένους.
        - Azure AD Connect, AAD Sync ή DirSync είναι μόνο μια απαίτηση εάν πραγματοποιείτε συγχρονισμό το περιβάλλον της υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης με έναν κατάλογο Azure AD.  Εάν χρησιμοποιείτε μόνο μια καταλόγου Azure AD που δεν έχει συγχρονιστεί, στη συνέχεια, αυτό δεν απαιτείται.
![Δημιουργία μιας υπηρεσίας παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Αφού κάνετε κλικ στην επιλογή Δημιουργία, την υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων δημιουργείται και θα πρέπει να δείτε ένα μήνυμα που δηλώνει: **υπηρεσία παροχής ελέγχου ταυτότητας πολλαπλών παραγόντων που δημιουργήθηκε με επιτυχία**. Κάντε κλικ στο κουμπί **Ok**.
![Δημιουργία μιας υπηρεσίας παροχής MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
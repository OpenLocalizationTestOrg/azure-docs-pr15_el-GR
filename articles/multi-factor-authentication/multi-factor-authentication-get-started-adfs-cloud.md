<properties
    pageTitle="Εξασφάλιση πόρων cloud με Azure MFA και AD FS"
    description="Αυτή είναι η σελίδα ελέγχου ταυτότητας πολλαπλών παραγόντων Azure που περιγράφει πώς μπορείτε να ξεκινήσετε με το Azure MFA και AD FS στο cloud."
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

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Ασφάλεια των πόρων cloud με έλεγχο ταυτότητας πολλών παραγόντων Azure και AD FS

Εάν η εταιρεία σας έχει συνενωθεί με Azure Active Directory, χρησιμοποιήστε τον έλεγχο ταυτότητας πολλών παραγόντων Azure ή υπηρεσίες Active Directory Federation Services για την ασφάλιση πόρους που είναι προσβάσιμες από Azure AD. Χρησιμοποιήστε τις ακόλουθες διαδικασίες για την ασφάλιση Azure Active Directory πόρους με έλεγχο ταυτότητας πολλών παραγόντων Azure ή υπηρεσίες Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Εξασφάλιση πόρων Azure AD χρησιμοποιώντας AD FS

Για να προστατεύσετε τον πόρο cloud, πρώτα Ενεργοποίηση λογαριασμού για τους χρήστες, στη συνέχεια, ρύθμιση κανόνα αξιώσεων. Ακολουθήστε αυτήν τη διαδικασία για να δείτε τα βήματα:

1. Χρησιμοποιήστε τα βήματα που περιγράφονται στην ενότητα [Έλεγχος ταυτότητας πολλών παραγόντων Turn-on για τους χρήστες](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) για να ενεργοποιήσετε ένα λογαριασμό.
2. Ξεκινήστε την Κονσόλα διαχείρισης AD FS.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Μεταβείτε στο **Βασίζεστε εμπιστεύεται κατασκευαστή** και κάντε δεξί κλικ στη το βασίζεστε πάρτι εμπιστεύεστε. Επιλέξτε την εντολή **Επεξεργασία κανόνες διεκδίκηση...**
4. Κάντε κλικ στην επιλογή **Προσθήκη κανόνα...**
5. Από την αναπτυσσόμενη λίστα, επιλέξτε **Αποστολή αξιώσεων χρησιμοποιώντας έναν προσαρμοσμένο κανόνα** και κάντε κλικ στο κουμπί **Επόμενο**.
6. Πληκτρολογήστε ένα όνομα για τον κανόνα διεκδίκηση.
7. Στην περιοχή Προσαρμογή κανόνα: Προσθέστε το ακόλουθο κείμενο:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Αντίστοιχο διεκδίκηση:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Κάντε κλικ στο **κουμπί OK** , στη συνέχεια, **Τέλος**. Κλείστε την Κονσόλα διαχείρισης AD FS.

Οι χρήστες, στη συνέχεια, να ολοκληρώσουν είσοδο με τη μέθοδο εσωτερικής εγκατάστασης (όπως έξυπνης κάρτας).

## <a name="trusted-ips-for-federated-users"></a>Αξιόπιστη διευθύνσεις IP για εξωτερικούς χρήστες
Διευθύνσεις IP του αξιόπιστων επιτρέπουν στους διαχειριστές να επαλήθευσης δύο βημάτων παράκαμψη για συγκεκριμένες διευθύνσεις IP ή για εξωτερικούς χρήστες που έχουν αιτήσεις που προέρχονται από μέσα σε intranet τις δικές τους. Οι παρακάτω ενότητες περιγράφουν τον τρόπο ρύθμισης παραμέτρων Azure πολλαπλών παραγόντων ελέγχου ταυτότητας αξιόπιστες διευθύνσεις IP του με εξωτερικούς χρήστες και επαλήθευση δύο βημάτων παράκαμψη όταν μια αίτηση προέρχεται από μέσα σε ένα intranet εξωτερικούς χρήστες. Αυτό είναι δυνατό, ρυθμίζοντας τις παραμέτρους AD FS να χρησιμοποιήσετε μια διαβίβασης ή να φιλτράρετε ένα πρότυπο εισερχόμενες αξιώσεων με τον τύπο διεκδίκηση μέσα σε εταιρικό δίκτυο.

Αυτό το παράδειγμα χρησιμοποιεί το Office 365 για μας βασίζεστε πάρτι εμπιστεύεται.

### <a name="configure-the-ad-fs-claims-rules"></a>Ρύθμιση παραμέτρων των κανόνων αξιώσεων AD FS

Το πρώτο πράγμα που πρέπει να κάνετε είναι να ρυθμίσετε τις παραμέτρους των απαιτήσεων AD FS. Θα δημιουργήσουμε κανόνες δύο αξιώσεων, ένα για τον τύπο διεκδίκηση μέσα σε εταιρικό δίκτυο και ένα πρόσθετο για να διατηρήσετε τους χρήστες μας πραγματοποιήσει είσοδο.

1. Ανοίξτε τη Διαχείριση AD FS.
2. Στα αριστερά, επιλέξτε **Βασίζεστε εμπιστεύεται κατασκευαστή**.
3. Κάντε δεξί κλικ στην **Πλατφόρμα ταυτότητας του Microsoft Office 365** και επιλέξτε **Επεξεργασία διεκδίκηση κανόνων...** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Σχετικά με τους κανόνες Μετασχηματισμός έκδοσης, κάντε κλικ στην επιλογή **Προσθήκη κανόνα.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Στην προσθήκη μετασχηματισμός διεκδίκηση κανόνα οδηγού, επιλέξτε **διέλευση ή φίλτρο μια εισερχόμενη απαίτηση** από την αναπτυσσόμενη λίστα και κάντε κλικ στο κουμπί **Επόμενο**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Στο πλαίσιο δίπλα στο όνομα του κανόνα διεκδίκηση, ονομάστε τον κανόνα σας. Για παράδειγμα: InsideCorpNet.
7. Από την αναπτυσσόμενη λίστα, δίπλα στο στοιχείο εισερχομένων τύπος δήλωσης, επιλέξτε **Μέσα σε εταιρικό δίκτυο**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Κάντε κλικ στο κουμπί **Τέλος**.
9. Σχετικά με τους κανόνες Μετασχηματισμός έκδοσης, κάντε κλικ στην επιλογή **Προσθήκη κανόνα**.
10. Στην προσθήκη μετασχηματισμός διεκδίκηση κανόνα οδηγού, επιλέξτε **Αποστολή αξιώσεων χρησιμοποιώντας έναν κανόνα προσαρμοσμένη** από την αναπτυσσόμενη λίστα και κάντε κλικ στο κουμπί **Επόμενο**.
11. Στο πλαίσιο στην περιοχή όνομα κανόνα διεκδίκηση: Εισαγάγετε *Διατήρηση χρήστες συνδεδεμένοι στο*.
12. Στο πλαίσιο Προσαρμογή κανόνα, πληκτρολογήστε:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Κάντε κλικ στο κουμπί **Τέλος**.
14. Κάντε κλικ στο κουμπί **εφαρμογή**.
15. Κάντε κλικ στο κουμπί **Ok**.
16. Κλείστε το AD FS διαχείρισης.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Ρύθμιση παραμέτρων Azure πολλαπλών παραγόντων ελέγχου ταυτότητας αξιόπιστες διευθύνσεις IP με εξωτερικούς χρήστες
Τώρα που η αξιώσεων είναι στη θέση, θα σας να ρυθμίσετε τις παραμέτρους αξιόπιστων διευθύνσεις IP.

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](https://manage.windowsazure.com).
2. Στα αριστερά, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.
3. Στην περιοχή καταλόγου, επιλέξτε τον κατάλογο όπου θέλετε να ρυθμίσετε αξιόπιστων διευθύνσεις IP.
4. Στον κατάλογο που έχετε επιλέξει, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.
5. Στην ενότητα έλεγχος ταυτότητας πολλών παραγόντων, κάντε κλικ στην επιλογή **Διαχείριση ρυθμίσεων της υπηρεσίας**.
6. Στη σελίδα "Ρυθμίσεις υπηρεσίας", στην περιοχή αξιόπιστων διευθύνσεις IP, επιλέξτε **παραλείψετε παραγόντων-έλεγχος ταυτότητας πολλών για αιτήσεις από εξωτερικούς χρήστες στο μου intranet.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Κάντε κλικ στην επιλογή **Αποθήκευση**.
8. Όταν έχει εφαρμοστεί τις ενημερώσεις, κάντε κλικ στο κουμπί **Κλείσιμο**.


Αυτό είναι! Σε αυτό το σημείο, εξωτερικούς χρήστες του Office 365 μόνο πρέπει να πρέπει να χρησιμοποιήσετε MFA όταν αξίωση προέρχεται από εκτός του εταιρικού intranet.

<properties
    pageTitle="Εργασία με αξιώσεων υπόψη εφαρμογές στο διακομιστή μεσολάβησης εφαρμογής"
    description="Περιγράφει πώς μπορείτε να αρχίσετε να το χρησιμοποιείτε με το διακομιστή μεσολάβησης εφαρμογής Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Εργασία με αξιώσεων υπόψη τις εφαρμογές στον διακομιστή μεσολάβησης εφαρμογής

Γνωρίζετε εφαρμογές αξιώσεων εκτέλεση ανακατεύθυνσης για την ασφάλεια διακριτικού υπηρεσίας (STS), οποίο με τη σειρά του ζητά διαπιστευτήρια από το χρήστη έναντι ενός διακριτικού πριν από την ανακατεύθυνση του χρήστη στην εφαρμογή. Για να ενεργοποιήσετε το διακομιστή μεσολάβησης εφαρμογής για να εργαστείτε με αυτές τις ανακατευθύνσεις, ακολουθήστε τα παρακάτω βήματα πρέπει να ληφθούν.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πριν να εκτελέσετε αυτήν τη διαδικασία, βεβαιωθείτε ότι το STS ανακατευθύνει την εφαρμογή υπόψη αξιώσεων είναι διαθέσιμη εκτός του δικτύου σας εσωτερικής εγκατάστασης.

## <a name="azure-classic-portal-configuration"></a>Azure κλασική ρύθμισης παραμέτρων πύλης

1. Δημοσίευση εφαρμογής σας σύμφωνα με τις οδηγίες που περιγράφονται σε [δημοσίευση εφαρμογών με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-publish.md).
2. Στη λίστα των εφαρμογών, επιλέξτε την εφαρμογή υπόψη αξιώσεων και κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.
3. Εάν επιλέξατε **διέλευση** ως τη **Μέθοδο προκαταρκτικού ελέγχου ταυτότητας**, βεβαιωθείτε ότι για να επιλέξετε **HTTPS** ως το συνδυασμό **Εξωτερική διεύθυνση URL** .
4. Εάν επιλέξατε **Azure Active Directory** ως τη **Μέθοδο προκαταρκτικού ελέγχου ταυτότητας**, επιλέξτε **καμία** ως το **Εσωτερικό μεθόδου ελέγχου ταυτότητας**.


## <a name="adfs-configuration"></a>Ρύθμιση παραμέτρων ADFS

1. Ανοίξτε τη Διαχείριση ADFS.
2. Μεταβείτε στην **Βασίζεστε εμπιστεύεται πάρτι**, κάντε δεξί κλικ στην την εφαρμογή που δημοσιεύετε με το διακομιστή μεσολάβησης εφαρμογής και επιλέξτε **Ιδιότητες**.  
  ![Υπηρεσία αξιοπιστίας εμπιστεύεται πάρτι δεξί κλικ στο όνομα εφαρμογής - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Στην καρτέλα **τελικά σημεία** , στην περιοχή **Τύπος τελικό σημείο**, επιλέξτε **WS ομοσπονδίας**.
4. Στην περιοχή **Αξιόπιστων διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που πληκτρολογήσατε στο διακομιστή μεσολάβησης εφαρμογής στην περιοχή **Εξωτερική διεύθυνση URL** και κάντε κλικ στο **κουμπί OK**.  
  ![Προσθήκη τελικού σημείου - Ορισμός τιμής αξιόπιστων διεύθυνση URL - στιγμιότυπο οθόνης](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Δείτε επίσης

- [Δημοσίευση εφαρμογών με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-publish.md)
- [Ενεργοποίηση της καθολικής σύνδεσης σε](active-directory-application-proxy-sso-using-kcd.md)
- [Αντιμετώπιση προβλημάτων που αντιμετωπίζετε με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-troubleshoot.md)
- [Ενεργοποίηση εφαρμογών εγγενές πρόγραμμα-πελάτη για να αλληλεπιδράσετε με εφαρμογές του διακομιστή μεσολάβησης](active-directory-application-proxy-native-client.md)

Για τις πιο πρόσφατες ειδήσεις και ενημερώσεις, ανατρέξτε στο θέμα το [ιστολόγιο του διακομιστή μεσολάβησης εφαρμογής](http://blogs.technet.com/b/applicationproxyblog/)

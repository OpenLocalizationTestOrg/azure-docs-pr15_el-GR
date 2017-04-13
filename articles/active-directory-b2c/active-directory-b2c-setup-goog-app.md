<properties
    pageTitle="Azure Active Directory B2C: Google + ρύθμισης παραμέτρων | Microsoft Azure"
    description="Δώστε εγγραφής και είσοδος για καταναλωτές με λογαριασμούς Google + στις εφαρμογές σας που προστατεύονται με Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Παρέχουν εγγραφής και είσοδος για καταναλωτές με λογαριασμούς του Google +

## <a name="create-a-google-application"></a>Δημιουργία εφαρμογής Google +

Για να χρησιμοποιήσετε Google + ως υπηρεσία παροχής ταυτότητας στο B2C Azure Active Directory (Azure AD), χρειάζεστε για να δημιουργήσετε μια εφαρμογή Google + και να παράσχει το σωστό παραμέτρους. Χρειάζεστε ένα λογαριασμό Google + για να το κάνετε αυτό. Εάν δεν έχετε, μπορείτε να το λάβετε στο [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Μεταβείτε στην [Κονσόλα Google τους προγραμματιστές](https://console.developers.google.com/) και συνδεθείτε με Google + λογαριασμός τα διαπιστευτήριά σας.
2. Κάντε κλικ στην επιλογή **Δημιουργία έργου**, πληκτρολογήστε ένα **όνομα έργου**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Google + - γρήγορα αποτελέσματα](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - νέου έργου](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Κάντε κλικ στην επιλογή **Διαχείριση API** και, στη συνέχεια, κάντε κλικ στην επιλογή **διαπιστευτηρίων** στο αριστερό παράθυρο περιήγησης.
4. Κάντε κλικ στην καρτέλα **διακριτικό συγκατάθεση οθόνης** στο επάνω μέρος.

    ![Google + - διαπιστευτήρια](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Επιλέξτε ή καθορίστε μια έγκυρη **διεύθυνση ηλεκτρονικού ταχυδρομείου**, δώστε ένα **όνομα προϊόντος**και κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Google + - διακριτικό συγκατάθεση οθόνης](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Κάντε κλικ στην επιλογή **νέα διαπιστευτήρια** και, στη συνέχεια, επιλέξτε **OAuth Αναγνωριστικό υπολογιστή-πελάτη**.

    ![Google + - διακριτικό συγκατάθεση οθόνης](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Στην περιοχή **τύπος εφαρμογής**, επιλέξτε **την εφαρμογή Web**.

    ![Google + - διακριτικό συγκατάθεση οθόνης](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Δώστε ένα **όνομα** για την εφαρμογή σας, πληκτρολογήστε `https://login.microsoftonline.com` στο πεδίο **προελεύσεις εξουσιοδοτημένοι JavaScript** , και `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` στο πεδίο **εξουσιοδοτημένες ανακατεύθυνση URIs** . Αντικαταστήστε **{μισθωτή}** με το όνομα του μισθωτή (για παράδειγμα, contosob2c.onmicrosoft.com). Η τιμή **{μισθωτή}** είναι διάκριση πεζών-κεφαλαίων. Κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Google + - δημιουργία Αναγνωριστικό υπολογιστή-πελάτη](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Αντιγράψτε τις τιμές του **Αναγνωριστικού προγράμματος-πελάτη** και του **προγράμματος-πελάτη μυστικό**. Θα χρειαστείτε και τα δύο για να ρυθμίσετε τις παραμέτρους Google + ως υπηρεσία παροχής ταυτότητας στο μισθωτή σας. **Μυστικό προγράμματος-πελάτη** είναι ένα σημαντικό ασφαλείας διαπιστευτηρίων.

    ![Google + - μυστικό προγράμματος-πελάτη](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Ρύθμιση παραμέτρων Google + ως υπηρεσία παροχής ταυτότητας στο μισθωτή σας

1. Ακολουθήστε τα παρακάτω βήματα για να [μεταβείτε σε το blade δυνατότητες B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) στην πύλη του Azure.
2. Στην το blade δυνατότητες B2C, κάντε κλικ στην επιλογή **υπηρεσίες παροχής ταυτότητας**.
3. Κάντε κλικ στο κουμπί **+ Προσθήκη** στο επάνω μέρος του blade.
4. Δώστε ένα φιλικό **όνομα** για τη ρύθμιση παραμέτρων της υπηρεσίας παροχής ταυτότητας. Για παράδειγμα, πληκτρολογήστε "G +".
5. Κάντε κλικ στην επιλογή **Τύπος υπηρεσίας παροχής ταυτότητας**, επιλέξτε **Google**και κάντε κλικ στο κουμπί **OK**.
6. Κάντε κλικ στην επιλογή **ρύθμιση αυτής της υπηρεσίας παροχής ταυτότητας** και πληκτρολογήστε το Αναγνωριστικό υπολογιστή-πελάτη και το πρόγραμμα-πελάτη μυστικό της Google εφαρμογής + που δημιουργήσατε νωρίτερα.
7. Κάντε κλικ στο κουμπί **OK** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία** για να αποθηκεύσετε τις ρυθμίσεις Google +.

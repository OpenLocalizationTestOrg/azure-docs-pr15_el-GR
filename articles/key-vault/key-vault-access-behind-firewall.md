<properties
    pageTitle="Πρόσβαση στο πλήκτρο θάλαμο πίσω από το τείχος προστασίας | Microsoft Azure"
    description="Μάθετε πώς να αποκτήσετε πρόσβαση θάλαμο κλειδί από μια εφαρμογή τείχος προστασίας"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Πρόσβαση σε θάλαμο κλειδί πίσω από το τείχος προστασίας
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>Ε: μου εφαρμογή προγράμματος-πελάτη κλειδιού θάλαμο πρέπει να βρίσκεται πίσω από ένα τείχος προστασίας, τι διευθύνσεις θύρες/hosts IP θα πρέπει να ανοίγω για να ενεργοποιήσετε την πρόσβαση σε βασικές θάλαμο;

Για να αποκτήσετε πρόσβαση σε ένα πλήκτρο θάλαμο την εφαρμογή-πελάτη κλειδιού θάλαμο πρέπει να μπορούν να έχουν πρόσβαση σε πολλά τελικών σημείων ελέγχου για διάφορες λειτουργίες.

- Έλεγχος ταυτότητας (μέσω Azure Active Directory)
- Διαχείριση του αριθμού-κλειδιού θάλαμο (το οποίο περιλαμβάνει δημιουργία/ανάγνωση/ενημέρωση/διαγραφή, καθώς και τη ρύθμιση πολιτικές πρόσβασης) μέσω της διαχείρισης πόρων Azure
- Πρόσβαση και τη διαχείριση αντικειμένων (πλήκτρα και απορρήτου) που είναι αποθηκευμένα στο πλήκτρο θάλαμο ίδια, μεταβαίνει μέσω του κλειδιού θάλαμο συγκεκριμένες τελικού σημείου (π.χ. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Ανάλογα με τη ρύθμιση των παραμέτρων και το περιβάλλον, υπάρχουν ορισμένες παραλλαγές.   

## <a name="ports"></a>Θύρες

Όλη την κυκλοφορία στο πλήκτρο θάλαμο για όλες τις 3 συναρτήσεις (πρόσβασης επιπέδου ελέγχου ταυτότητας, διαχείριση και δεδομένα) καλύπτει HTTPS: θύρα 443. Ωστόσο για CRL, θα υπάρχουν μερικές φορές κυκλοφορία HTTP (θύρα 80). Προγράμματα-πελάτες που υποστηρίζουν OCSP δεν πρέπει να φτάσετε CRL, αλλά μερικές φορές μπορεί να φτάσει [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Έλεγχος ταυτότητας

Εφαρμογή προγράμματος-πελάτη θάλαμο αριθμού-κλειδιού θα χρειαστεί για να αποκτήσετε πρόσβαση τελικά σημεία Azure Active Directory για τον έλεγχο ταυτότητας. Το τελικό σημείο χρησιμοποιούνται εξαρτάται από τη ρύθμιση παραμέτρων του μισθωτή AAD και τον τύπο του κεφαλαίου--αρχικού χρήστη, υπηρεσία κεφάλαιο και τον τύπο του λογαριασμού, π.χ. λογαριασμό Microsoft ή αναγνωριστικό οργανισμού.  

| Τύπος κεφάλαιο | Τελικό σημείο: θύρα |
|----------------|---------------|
| Χρήστη που χρησιμοποιεί το λογαριασμό της Microsoft<br> (π.χ.user@hotmail.com) | **Καθολικό:**<br> Login.microsoftonline.com:443<br><br> **Azure Κίνα:**<br> Login.chinacloudapi.CN:443<br><br>**Azure κυβέρνηση των η.π.α.:**<br> us.microsoftonline.com:443 σύνδεσης<br><br>**Azure Γερμανία:**<br> Login.microsoftonline.de:443<br><br> και <br>Login.Live.com:443   |
| Υπηρεσία/χρήστη κεφάλαιο Χρήση Αναγνωριστικό εταιρείας με AAD (π.χ.user@contoso.com) | **Καθολικό:**<br> Login.microsoftonline.com:443<br><br> **Azure Κίνα:**<br> Login.chinacloudapi.CN:443<br><br>**Azure κυβέρνηση των η.π.α.:**<br> us.microsoftonline.com:443 σύνδεσης<br><br>**Azure Γερμανία:**<br> Login.microsoftonline.de:443 |
| Κεφάλαιο/υπηρεσία χρήστη με την εταιρεία Αναγνωριστικό + ADFS ή άλλες εξωτερικές τελικού σημείου (π.χ.user@contoso.com) | Όλα τα παραπάνω τελικά σημεία για το Αναγνωριστικό εταιρείας συν ADFS ή άλλες εξωτερικές τελικά σημεία |

Υπάρχουν άλλες πιθανές σύνθετα σενάρια. Ανατρέξτε [Azure Active Directory ελέγχου ταυτότητας ροής](/documentation/articles/active-directory-authentication-scenarios/), [Εφαρμογές ενοποίηση με το Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) και [Πρωτόκολλα ελέγχου ταυτότητας Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx) για πρόσθετες πληροφορίες.  

## <a name="key-vault-management"></a>Διαχείριση βασικών θάλαμο

Για τη Διαχείριση θάλαμο αριθμού-κλειδιού (CRUD και ρύθμιση πολιτικής πρόσβασης), την εφαρμογή-πελάτη κλειδιού θάλαμο πρέπει να έχετε πρόσβαση από διαχειριστή πόρων Azure τελικού σημείου.  

| Τύπος λειτουργίας | Τελικό σημείο: θύρα |
|----------------|---------------|
| Επίπεδο στοιχείου ελέγχου θάλαμο πλήκτρο λειτουργιών<br> μέσω Azure από διαχειριστή πόρων | **Καθολικό:**<br> Management.Azure.com:443<br><br> **Azure Κίνα:**<br> Management.chinacloudapi.CN:443<br><br> **Azure κυβέρνηση των η.π.α.:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Γερμανία:**<br> Management.microsoftazure.de:443 |
| Γράφημα καταλόγου Azure Active Directory API | **Καθολικό:**<br> Graph.Windows.NET:443<br><br> **Azure Κίνα:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure κυβέρνηση των η.π.α.:**<br> Graph.Windows.NET:443<br><br> **Azure Γερμανία:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Λειτουργίες κλειδιού θάλαμο

Για όλα τα διαχείρισης κλειδιών θάλαμο αντικειμένου (πλήκτρα και απορρήτου) και λειτουργίες κρυπτογράφησης, κλειδιού θάλαμο προγράμματος-πελάτη πρέπει να πρόσβαση το τελικό σημείο κλειδιού θάλαμο. Ανάλογα με τη θέση των θάλαμο τον αριθμό-κλειδί, διαφέρει το επίθημα DNS τελικού σημείου. Το τελικό σημείο θάλαμο κλειδί έχει τη μορφή: < όνομα-θάλαμο >. < περιοχή---επίθημα dns > όπως περιγράφεται στον παρακάτω πίνακα.  

| Τύπος λειτουργίας | Τελικό σημείο: θύρα |
|----------------|---------------|
| Πλήκτρο θάλαμο λειτουργίες όπως λειτουργίες κρυπτογράφησης σε κλειδιά, δημιουργήθηκε/ανάγνωση/ενημέρωση/διαγραφή πλήκτρα και απόρρητο, αποστολή/λήψη ετικετών και άλλα χαρακτηριστικά σε αντικείμενα κλειδιού θάλαμο (πλήκτρα απόρρητο)     | **Καθολικό:**<br> &lt;όνομα θάλαμο&gt;. vault.azure.net:443<br><br> **Azure Κίνα:**<br> &lt;όνομα θάλαμο&gt;. vault.azure.cn:443<br><br> **Azure κυβέρνηση των η.π.α.:**<br> &lt;όνομα θάλαμο&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Γερμανία:**<br> &lt;όνομα θάλαμο&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>Περιοχές διευθύνσεων IP

Πλήκτρο θάλαμο υπηρεσία χρησιμοποιεί με τη σειρά άλλων Azure πόρους, όπως την υποδομή PaaS, έτσι δεν είναι δυνατό να δώσετε μια συγκεκριμένη περιοχή διευθύνσεων IP που θα έχουν τα τελικά σημεία υπηρεσίας κλειδιού θάλαμο οποιαδήποτε στιγμή. Ωστόσο εάν το τείχος προστασίας σας υποστηρίζει μόνο περιοχές, στη συνέχεια, ανατρέξτε στο έγγραφο [Περιοχές διευθύνσεων IP του κέντρου δεδομένων του Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653) διεύθυνση IP.   Για τον έλεγχο ταυτότητας και ταυτότητα (Azure Active Directory), την εφαρμογή σας πρέπει να μπορείτε να συνδεθείτε με τα τελικά σημεία που περιγράφονται σε [έλεγχο ταυτότητας και τις διευθύνσεις ταυτότητα](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Επόμενα βήματα

- Εάν έχετε ερωτήσεις σχετικά με το κλειδί θάλαμο, επισκεφθείτε τα [Φόρουμ θάλαμο κλειδί Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
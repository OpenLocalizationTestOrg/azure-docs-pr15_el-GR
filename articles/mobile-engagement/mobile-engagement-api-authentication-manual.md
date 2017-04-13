<properties 
    pageTitle="Ο έλεγχος ταυτότητας με κινητό δέσμευση REST API - μη αυτόματη ρύθμιση"
    description="Περιγράφει τον τρόπο ρύθμισης του ελέγχου ταυτότητας για κινητό δέσμευση REST API με μη αυτόματο τρόπο" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Ο έλεγχος ταυτότητας με κινητό δέσμευση REST API - μη αυτόματη ρύθμιση

Αυτή είναι μια προσάρτημα τεκμηρίωση για να [Έλεγχος ταυτότητας με κινητό δέσμευση REST API](mobile-engagement-api-authentication.md). Βεβαιωθείτε ότι μπορείτε να διαβάσετε αυτό πρώτα, για να λάβετε το περιβάλλον. Αυτό περιγράφει έναν εναλλακτικό τρόπο για να κάνετε τη μεμονωμένη ρύθμιση για τη ρύθμιση του ελέγχου ταυτότητας για τα API ΥΠΌΛΟΙΠΑ δέσμευση Mobile με την πύλη Azure. 

>[AZURE.NOTE] Τις παρακάτω οδηγίες βασίζονται σε αυτόν τον [Οδηγό υπηρεσίας καταλόγου Active Directory](../resource-group-create-service-principal-portal.md) και προσαρμοσμένο για τι απαιτείται για τον έλεγχο ταυτότητας για API δέσμευση Mobile. Επομένως, αναφέρεστε σε αυτό, εάν θέλετε να κατανοήσετε τα παρακάτω βήματα με λεπτομέρειες. 

1. Συνδεθείτε στο λογαριασμό σας στο Azure μέσω της [κλασικής πύλη](https://manage.windowsazure.com/).

2. Επιλέξτε την **Υπηρεσία καταλόγου Active Directory** από το αριστερό παράθυρο.

     ![Επιλέξτε την υπηρεσία καταλόγου Active Directory][1]

3. Επιλέξτε την **Προεπιλεγμένη υπηρεσία καταλόγου Active Directory** στην πύλη Azure. 

     ![Επιλέξτε καταλόγου][2]

    >[AZURE.IMPORTANT] Αυτή η προσέγγιση λειτουργεί μόνο όταν εργάζεστε στην προεπιλεγμένη υπηρεσία καταλόγου Active Directory του λογαριασμού σας και δεν θα λειτουργεί εάν κάνετε αυτό σε μια υπηρεσία καταλόγου Active Directory που έχετε δημιουργήσει στο λογαριασμό σας. 

4. Για να προβάλετε τις εφαρμογές στον κατάλογό σας, κάντε κλικ στην επιλογή **εφαρμογές**.

     ![Προβολή εφαρμογών][3]

5. Κάντε κλικ στην εντολή **ΠΡΟΣΘΉΚΗ**. 

     ![Προσθήκη εφαρμογής][4]

6. Κάντε κλικ στην εντολή **Προσθήκη εφαρμογής ανάπτυξη εταιρεία μου**

     ![νέα εφαρμογή][5]

6. Συμπληρώστε το όνομα της εφαρμογής και επιλέξτε τον τύπο της εφαρμογής ως **ΕΦΑΡΜΟΓΉΣ WEB ή/και το API WEB** και κάντε κλικ στο κουμπί Επόμενο.

     ![όνομα εφαρμογής][6]

7. Μπορείτε να παράσχετε εικονική διευθύνσεων URL **ΕΙΣΌΔΟΥ ON διεύθυνση URL** και **URI Αναγνωριστικό Εφαρμογής**. Δεν χρησιμοποιούνται για το σενάριο και δεν επικυρώνονται τις διευθύνσεις URL τον εαυτό τους.  

     ![Ιδιότητες εφαρμογής][7]

8. Στο τέλος αυτού του, θα έχετε μια εφαρμογή AAD με το όνομα που έχετε ορίσει προηγουμένως όπως τα εξής. Αυτό είναι το **AD\_Εφαρμογή\_ΌΝΟΜΑ** και σημειώστε την.  

     ![όνομα εφαρμογής][8]

9. Κάντε κλικ στο όνομα της εφαρμογής και κάντε κλικ στη **Ρύθμιση παραμέτρων**.

     ![ρύθμιση παραμέτρων εφαρμογής][9]

10. Σημειώστε το Αναγνωριστικό ΠΕΛΆΤΗ που θα χρησιμοποιηθεί ως **προγράμματος-ΠΕΛΆΤΗ\_Αναγνωριστικό** για το API κλήσεων. 

     ![ρύθμιση παραμέτρων εφαρμογής][10]

11. Κάντε κύλιση προς τα κάτω στην ενότητα **πλήκτρα** και προσθέστε έναν αριθμό-κλειδί με κατά προτίμηση διάρκεια 2 έτη (λήξη) και κάντε κλικ στην επιλογή **Αποθήκευση**. 

     ![ρύθμιση παραμέτρων εφαρμογής][11]


12. Αμέσως, αντιγράψτε την τιμή η οποία εμφανίζεται για τον αριθμό-κλειδί που εμφανίζεται μόνο τώρα και δεν είναι αποθηκευμένο έτσι δεν θα εμφανίζεται ποτέ ξανά. Εάν χάσετε την, στη συνέχεια, θα πρέπει να δημιουργήσετε ένα νέο αριθμό-κλειδί. Αυτό θα είναι το **CLIENT_SECRET** για τις κλήσεις API. 

     ![ρύθμιση παραμέτρων εφαρμογής][12]

    >[AZURE.IMPORTANT] Αυτό το κλειδί θα λήξει στο τέλος της διάρκειας που καθορίσατε, ώστε να βεβαιωθείτε ότι να την ανανεώσετε κατά το χρόνο διαφορετικά σας τον έλεγχο ταυτότητας API δεν θα λειτουργούν πλέον. Μπορείτε επίσης να διαγράψετε και να αναδημιουργήσετε αυτό το κλειδί, εάν πιστεύετε ότι έχει παραβιαστεί.
 
13. Κάντε κλικ στο κουμπί **ΠΡΟΒΟΛΉ τελικά ΣΗΜΕΊΑ** τώρα που θα ανοίξει το παράθυρο διαλόγου **Εφαρμογή τελικά σημεία** . 

    ![][13]

14. Από το παράθυρο διαλόγου εφαρμογή τελικά σημεία, αντιγράψτε το **ΔΙΑΚΡΙΤΙΚΌ 2.0 ΔΙΑΚΡΙΤΙΚΟΎ τελικού ΣΗΜΕΊΟΥ**. 

    ![][14]

15. Αυτό το τελικό σημείο θα στη φόρμα όπου το GUID στη διεύθυνση URL είναι σας **TENANT_ID** επομένως, σημειώστε το εξής: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Τώρα θα συνεχίσουμε να ρυθμίσετε τις παραμέτρους των δικαιωμάτων σε αυτήν την εφαρμογή. Για αυτό θα πρέπει να ανοίξετε την [πύλη του Azure](https://portal.azure.com). 

17. Κάντε κλικ στην επιλογή ομάδες **Πόρων** και βρείτε την ομάδα **Mobile δέσμευση** πόρων.  

    ![][15]

18. Κάντε κλικ στην ομάδα **Mobile δέσμευση** πόρων και μεταβείτε εδώ το blade **Ρυθμίσεις** . 

    ![][16]

19. Κάντε κλικ σε **χρήστες** του blade ρυθμίσεις και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη** για να προσθέσετε ένα χρήστη. 

    ![][17]

20. Κάντε κλικ στην **επιλογή ρόλου**

    ![][18]

21. Κάντε κλικ στην επιλογή στον **κάτοχο**

    ![][19]

22. Αναζήτηση για το όνομα της εφαρμογής σας **AD\_Εφαρμογή\_ΌΝΟΜΑ** στο πλαίσιο αναζήτησης. Δεν θα βλέπετε αυτό από προεπιλογή εδώ. Μόλις το βρείτε, επιλέξτε την και κάντε κλικ στην **επιλογή** στο κάτω μέρος του blade. 

    ![][20]

23. Στην blade την **Προσθήκη Access** , θα εμφανίζεται ως **1 ο χρήστης, 0 ομάδες**. Σε αυτό blade για να επιβεβαιώσετε την αλλαγή, κάντε κλικ στο κουμπί **OK** . 

    ![][21]

Τώρα έχετε ολοκληρώσει τις απαιτούμενες ρυθμίσεις AAD και που είναι έτοιμοι για να καλέσετε τα API. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png




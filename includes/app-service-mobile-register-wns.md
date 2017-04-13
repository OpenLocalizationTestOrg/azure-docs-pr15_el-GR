
1. Στην Εξερεύνηση λύσεων Visual Studio, κάντε δεξί κλικ στο έργο εφαρμογή Windows Store, κάντε κλικ στην επιλογή **Αποθήκευση** > **Συσχετίσετε εφαρμογή με το χώρο αποθήκευσης...**.

    ![Συσχετισμός εφαρμογή με το Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. Στον οδηγό, κάντε κλικ στο κουμπί **Επόμενο**, συνδεθείτε με το λογαριασμό Microsoft, πληκτρολογήστε ένα όνομα για την εφαρμογή σας σε **ένα νέο όνομα εφαρμογής αποθεματικού**και κατόπιν κάντε κλικ στην επιλογή **απόθεμα**.

3. Μετά την καταχώρηση εφαρμογής δημιουργήθηκε με επιτυχία, επιλέξτε το νέο όνομα εφαρμογής, κάντε κλικ στο κουμπί **Επόμενο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Συσχέτιση**. Αυτό προσθέτει τις απαιτούμενες πληροφορίες εγγραφής του Windows Store η δήλωση εφαρμογής.

7. Επαναλάβετε τα βήματα 1 και 3 για το έργο εφαρμογής Windows Phone αποθήκευσης με χρήση της ίδιας καταχώρησης που δημιουργήσατε προηγουμένως για την εφαρμογή Windows Store.  

7. Μεταβείτε στο [Κέντρο ανάπτυξης των Windows](https://dev.windows.com/en-us/overview), πραγματοποιήστε είσοδο με το λογαριασμό της Microsoft, κάντε κλικ στην επιλογή νέα εφαρμογή εγγραφές **οι εφαρμογές μου**, στη συνέχεια, αναπτύξτε τις **υπηρεσίες** > **ειδοποιήσεις προώθησης**.

8. Στη σελίδα **ειδοποιήσεις Push** , κάντε κλικ στην επιλογή **τοποθεσία Live υπηρεσιών** στην περιοχή **Microsoft Azure Mobile εφαρμογές και υπηρεσίες ειδοποιήσεων Push των Windows (WNS)**και σημειώστε τις τιμές από το **Αναγνωριστικό πακέτου ΑΣΦΑΛΕΊΑΣ** και την *τρέχουσα* τιμή στην **Εφαρμογή μυστικό**. 

    ![Ρύθμιση εφαρμογών στο Κέντρο για προγραμματιστές](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Το μυστικό εφαρμογής και πακέτου αναγνωριστικό ΑΣΦΑΛΕΊΑΣ είναι τα διαπιστευτήριά σημαντική ασφαλείας. Μην κάνετε κοινή χρήση αυτές τις τιμές με οποιονδήποτε ή διανείμετε μαζί με την εφαρμογή σας.
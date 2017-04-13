Azure πελάτες να ξεκλειδώσετε 25.000 δωρεάν μηνύματα ηλεκτρονικού ταχυδρομείου κάθε μήνα. Αυτά τα 25.000 δωρεάν μηνιαία μηνύματα ηλεκτρονικού ταχυδρομείου θα σάς δίνουν πρόσβαση σε σύνθετες αναφοράς και ανάλυση και [όλα APIs][] (Web, SMTP, συμβάν, ανάλυσης και άλλα). Για πληροφορίες σχετικά με τις πρόσθετες υπηρεσίες που παρέχονται από SendGrid, ανατρέξτε στη σελίδα [Δυνατότητες SendGrid][] .

### <a name="to-sign-up-for-a-sendgrid-account"></a>Για να εγγραφείτε για ένα λογαριασμό SendGrid

1. Συνδεθείτε [πύλη διαχείρισης Azure][].

2. Στο κάτω τμήμα του παραθύρου της πύλης διαχείρισης, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![εντολή-γραμμή-νέα][command-bar-new]

3. Κάντε κλικ στην επιλογή **Marketplace**.

    ![sendgrid store][sendgrid-store]

4. Στο παράθυρο διαλόγου **Επιλογή μιας εφαρμογής και της υπηρεσίας** , επιλέξτε **SendGrid** και κάντε κλικ στο δεξιό βέλος.

5. Στο παράθυρο διαλόγου **Προσαρμογή εφαρμογών και υπηρεσιών** , επιλέξτε το σχέδιο **SendGrid** θέλετε να εγγραφείτε για.

6. Πληκτρολογήστε ένα όνομα για τον προσδιορισμό της υπηρεσίας **SendGrid** στις ρυθμίσεις του Azure σας ή χρησιμοποιήστε την προεπιλεγμένη τιμή του **SendGridEmailDelivery.Simplified.SMTPWebAPI**. Ονόματα πρέπει να είναι μεταξύ 1 και 100 χαρακτήρες και να περιέχουν μόνο αλφαριθμητικούς χαρακτήρες, παύλες, τελείες και χαρακτήρες υπογράμμισης. Το όνομα πρέπει να είναι μοναδικό στη λίστα με τα στοιχεία Store εγγεγραμμένο Azure.

    ![χώρος αποθήκευσης-οθόνη-2][store-screen-2]

7. Επιλέξτε μια τιμή για την περιοχή; Για παράδειγμα, Δυτική US.

8. Κάντε κλικ στο δεξιό βέλος.

9. Στην καρτέλα **Αναθεώρηση αγορά** , εξετάστε το σχέδιο και τις πληροφορίες τιμολόγησης και νομικές τους όρους της. Εάν συμφωνείτε με τους όρους, κάντε κλικ στο σημάδι ελέγχου. Αφού κάνετε κλικ στο σημάδι ελέγχου, ο λογαριασμός σας SendGrid θα ξεκινήσει η [διαδικασία προετοιμασίας SendGrid].

    ![χώρος αποθήκευσης-οθόνη-3][store-screen-3]

10. Μετά την αγορά σας επιβεβαίωση ανακατευθύνεστε στον πίνακα εργαλείων πρόσθετα και θα εμφανιστεί το μήνυμα **SendGrid αγορά**.

    ![sendgrid αγορά μηνύματος][sendgrid-purchasing-message]

    Ο λογαριασμός σας SendGrid παρέχεται αμέσως και θα εμφανιστεί το μήνυμα **με επιτυχία αγοράσει SendGrid πρόσθετο**. Ο λογαριασμός σας και τα διαπιστευτήρια δημιουργούνται τώρα. Είστε έτοιμοι να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου σε αυτό το σημείο. 

    Για να τροποποιήσετε το πρόγραμμά σας συνδρομή ή ανατρέξτε στο θέμα το SendGrid επαφής ρυθμίσεις, κάντε κλικ στο όνομα της υπηρεσίας σας SendGrid για να ανοίξετε τον πίνακα εργαλείων SendGrid Marketplace. 

    ![sendgrid-Προσθήκη-σε-πίνακα εργαλείων][sendgrid-add-on-dashboard]

    Για να στείλετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid, θα πρέπει να δώσετε τα διαπιστευτήρια λογαριασμού σας (όνομα χρήστη και κωδικός πρόσβασης).

### <a name="to-find-your-sendgrid-credentials"></a>Για να βρείτε τα διαπιστευτήριά σας SendGrid ###

1. Κάντε κλικ στην επιλογή **πληροφορίες σύνδεσης**.

    ![sendgrid--πληροφορίες-κουμπί σύνδεσης][sendgrid-connection-info-button]

2. Στο παράθυρο διαλόγου *πληροφορίες σύνδεσης* , αντιγράψτε τον **κωδικό πρόσβασης** και το όνομα χρήστη για να χρησιμοποιήσετε αργότερα σε αυτό το πρόγραμμα εκμάθησης.

    ![sendgrid πληροφοριών σύνδεσης][sendgrid-connection-info]

    Για να ρυθμίσετε το ηλεκτρονικό ταχυδρομείο σας deliverability ρυθμίσεις, κάντε κλικ στο κουμπί **Διαχείριση** . Αυτό θα σας ανακατευθύνουν για τον πίνακα ελέγχου SendGrid. 

    ![Πίνακας ελέγχου sendgrid][sendgrid-control-panel]

    Για περισσότερες πληροφορίες σχετικά με γρήγορα αποτελέσματα με το SendGrid, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα SendGrid][].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[Δυνατότητες SendGrid]: http://sendgrid.com/features
[Πύλη διαχείρισης Azure]: https://manage.windowsazure.com
[SendGrid γρήγορα αποτελέσματα]: http://sendgrid.com/docs
[Διαδικασία προμήθεια SendGrid]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[όλα APIs]: https://sendgrid.com/docs/API_Reference/index.html


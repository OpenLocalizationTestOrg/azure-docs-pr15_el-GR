<properties
    pageTitle="Αντιμετώπιση προβλημάτων την επέκταση του πίνακα πρόσβασης για τον Internet Explorer | Microsoft Azure"
    description="Πώς μπορείτε να χρησιμοποιήσετε την πολιτική ομάδας για να αναπτύξετε το πρόσθετο του Internet Explorer για την πύλη οι εφαρμογές μου."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Αντιμετώπιση προβλημάτων την επέκταση του πίνακα πρόσβασης για τον Internet Explorer

Σε αυτό το άρθρο θα σας βοηθήσει να αντιμετωπίσετε τα ακόλουθα προβλήματα:

- Είστε δεν είναι δυνατή η πρόσβαση σε εφαρμογές σας μέσω της πύλης οι εφαρμογές μου κατά τη χρήση του Internet Explorer.
- Μπορείτε να δείτε το μήνυμα "Εγκατάσταση λογισμικού" ακόμη και αν έχετε ήδη εγκαταστήσει το λογισμικό.

Εάν είστε διαχειριστής, δείτε επίσης: [Πώς να αναπτύξετε την επέκταση του πίνακα πρόσβασης για τον Internet Explorer, χρησιμοποιώντας την πολιτική ομάδας](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Εκτελέστε το εργαλείο διαγνωστικών

Μπορείτε να στη διάγνωση προβλημάτων εγκατάστασης με την επέκταση του πίνακα πρόσβασης κάνοντας λήψη και εκτέλεση του εργαλείου διαγνωστικών πίνακα της Access:

1. [Κάντε κλικ εδώ για να κάνετε λήψη του εργαλείου διαγνωστικών.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Ανοίξτε το αρχείο και πατήστε το κουμπί **Εξαγωγή όλων** .

    ![Πατήστε το πλήκτρο εξαγωγή όλων](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Στη συνέχεια, πατήστε το κουμπί " **Εξαγωγή** " για να συνεχίσετε.

    ![Πατήστε το πλήκτρο εξαγάγετε](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Για να εκτελέσετε το εργαλείο, κάντε δεξί κλικ στο αρχείο με το όνομα **AccessPanelExtensionDiagnosticTool**και, στη συνέχεια, επιλέξτε **Άνοιγμα με την > Microsoft Windows Based Script Host**.

    ![Άνοιγμα με την > βασίζεται στα Microsoft Windows Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Στη συνέχεια, θα δείτε παρακάτω διαγνωστικών παράθυρο, που περιγράφει τι μπορεί να συμβαίνει με την εγκατάσταση.

    ![Ένα δείγμα του παραθύρου διαγνωστικών](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Κάντε κλικ στην επιλογή "**Ναι**" για να αφήσετε το πρόγραμμα να διορθώσετε τα προβλήματα που έχουν εντοπιστεί.

7. Για να αποθηκεύσετε αυτές τις αλλαγές, κλείστε το παράθυρο του Internet Explorer κάθε και, στη συνέχεια, ανοίξτε ξανά τον Internet Explorer.<br />Εάν εξακολουθείτε να έχετε πρόσβαση σε εφαρμογές σας, δοκιμάστε τα παρακάτω βήματα.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Ελέγξτε ότι είναι ενεργοποιημένη η επέκταση του πίνακα πρόσβασης

Για να βεβαιωθείτε ότι είναι ενεργοποιημένη η επέκταση του πίνακα πρόσβαση στον Internet Explorer:

1. Στον Internet Explorer, κάντε κλικ στο **εικονίδιο γραναζιού** στην επάνω δεξιά γωνία του παραθύρου. Στη συνέχεια, επιλέξτε **Επιλογές Internet**.<br />(Σε παλαιότερες εκδόσεις του Internet Explorer, μπορείτε να βρείτε αυτό στην περιοχή **Εργαλεία > Επιλογές Internet**.

    ![Επιλέξτε Εργαλεία > Επιλογές Internet](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Κάντε κλικ στην καρτέλα **προγράμματα** και, στη συνέχεια, κάντε κλικ στο κουμπί **Διαχείριση πρόσθετων** .

    ![Κάντε κλικ στην επιλογή Διαχείριση πρόσθετων](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. Σε αυτό το παράθυρο διαλόγου, επιλέξτε **Επέκταση του πίνακα πρόσβασης** και, στη συνέχεια, κάντε κλικ στο κουμπί **Ενεργοποίηση** .

    ![Κάντε κλικ στην επιλογή Ενεργοποίηση](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Για να αποθηκεύσετε αυτές τις αλλαγές, κλείστε το παράθυρο του Internet Explorer κάθε και, στη συνέχεια, ανοίξτε ξανά τον Internet Explorer.

##<a name="enable-extensions-for-inprivate-browsing"></a>Ενεργοποίηση επεκτάσεις για την Περιήγηση InPrivate

Εάν χρησιμοποιείτε την κατάσταση λειτουργίας περιήγησης InPrivate:

1. Στον Internet Explorer, κάντε κλικ στο **εικονίδιο γραναζιού** στην επάνω δεξιά γωνία του παραθύρου. Στη συνέχεια, επιλέξτε **Επιλογές Internet**.<br />(Σε παλαιότερες εκδόσεις του Internet Explorer, μπορείτε να βρείτε αυτό στην περιοχή **Εργαλεία > Επιλογές Internet**.

    ![Ένα δείγμα του παραθύρου διαγνωστικών](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Μεταβείτε στην καρτέλα **προστασία προσωπικών δεδομένων** , στη συνέχεια, **καταργήστε την επιλογή από** το πλαίσιο ελέγχου με την ετικέτα **Απενεργοποίηση γραμμές εργαλείων και επεκτάσεις κατά την εκκίνηση του Περιήγηση InPrivate**</p>

    ![Καταργήστε την επιλογή Απενεργοποίηση γραμμές εργαλείων και επεκτάσεις κατά την εκκίνηση του Περιήγηση InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Για να αποθηκεύσετε αυτές τις αλλαγές, κλείστε το παράθυρο του Internet Explorer κάθε και, στη συνέχεια, ανοίξτε ξανά τον Internet Explorer.

##<a name="uninstall-the-access-panel-extension"></a>Κατάργηση της εγκατάστασης την επέκταση του πίνακα πρόσβασης

Για να καταργήσετε την επέκταση του πίνακα της Access από τον υπολογιστή σας:

1. Στο πληκτρολόγιό σας, πατήστε το **πλήκτρο των Windows** για να ανοίξετε το μενού "Έναρξη". Όταν είναι ανοιχτό το μενού, μπορείτε να πληκτρολογήσετε κάτι για να κάνετε αναζήτηση. Πληκτρολογήστε "Πίνακας ελέγχου" και, στη συνέχεια, ανοίξτε τον **Πίνακα ελέγχου** , όταν εμφανίζεται στα αποτελέσματα αναζήτησης.

    ![Αναζήτηση για τον πίνακα ελέγχου](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. Στην επάνω δεξιά γωνία του πίνακα ελέγχου, αλλάξτε την επιλογή **προβολής,** σε **μεγάλα εικονίδια**. Στη συνέχεια, βρείτε και κάντε κλικ στο κουμπί **προγράμματα και δυνατότητες** .

    ![Αλλαγή της προβολής για να εμφανίσετε μεγάλα εικονίδια](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Από τη λίστα, επιλέξτε **Επέκταση του πίνακα πρόσβασης**και κάντε κλικ στο κουμπί **Κατάργηση εγκατάστασης** .

    ![Κάντε κλικ στην επιλογή κατάργηση εγκατάστασης](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Στη συνέχεια, μπορείτε να δοκιμάσετε για να εγκαταστήσετε την επέκταση ξανά για να δείτε εάν το ζήτημα έχει επιλυθεί.

Εάν αντιμετωπίσετε προβλήματα με την κατάργηση της εγκατάστασης την επέκταση, μπορείτε επίσης να καταργήσετε χρησιμοποιώντας το εργαλείο [Microsoft διόρθωση It](https://go.microsoft.com/?linkid=9779673) .

## <a name="related-articles"></a>Σχετικά άρθρα

- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
- [Εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Πώς μπορείτε να αναπτύξετε την επέκταση του πίνακα πρόσβασης για τον Internet Explorer, χρησιμοποιώντας την πολιτική ομάδας](active-directory-saas-ie-group-policy.md)

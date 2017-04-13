<properties 
   pageTitle="Azure κινητή δέσμευση οδηγού - APIs αντιμετώπισης προβλημάτων" 
   description="Αντιμετώπιση προβλημάτων οδηγούς για το Azure κινητή δέσμευση - APIs" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Οδηγός αντιμετώπισης προβλημάτων για ζητήματα API

Οι παρακάτω είναι πιθανά ζητήματα που μπορεί να αντιμετωπίσετε με τον τρόπο διαχειριστές αλληλεπίδρασης με δέσμευση Mobile Azure μέσω τα API.

## <a name="syntax-issues"></a>Σύνταξη θέματα

### <a name="issue"></a>Το ζήτημα
- Σύνταξη σφαλμάτων με τη χρήση του API (ή μη αναμενόμενη συμπεριφορά).

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Σύνταξη θέματα:
    - Βεβαιωθείτε ότι για να ελέγξετε τη σύνταξη του συγκεκριμένες API που χρησιμοποιείτε για να επιβεβαιώσετε ότι η επιλογή είναι διαθέσιμη.
    - Ένα κοινό ζήτημα με χρήση API είναι να μπερδέψει κανείς το API επίτευξη και το API Push (περισσότερες εργασίες θα πρέπει να γίνει με το API επίτευξη αντί για το API Push). 
    - Κάποιο άλλο κοινές ζήτημα με ενοποίηση SDK και χρήση API είναι να μπερδέψει κανείς το κλειδί SDK και τον αριθμό-κλειδί API.
    - Δέσμες ενεργειών που συνδέονται με τα API χρειάζεται για την αποστολή δεδομένων τουλάχιστον κάθε 10 λεπτά ή τη σύνδεση θα λήξει το χρονικό όριο (συνηθισμένο σε δέσμες ενεργειών API οθόνη ακρόαση για τα δεδομένα). Για να αποτρέψετε χρονικών ορίων, έχετε δέσμη ενεργειών σας, στείλτε μια εντολή ping XMPP κάθε 10 λεπτά για να διατηρήσετε την περίοδο λειτουργίας ενεργών με το διακομιστή.

### <a name="see-also"></a>Δείτε επίσης
 
- [Τεκμηρίωση API][Link 4]
- [Πληροφορίες πρωτοκόλλου XMPP]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Δεν μπορείτε να χρησιμοποιήσετε το API για να εκτελέσετε την ίδια ενέργεια που είναι διαθέσιμα στο περιβάλλον εργασίας Χρήστη δέσμευση του Azure Mobile

### <a name="issue"></a>Το ζήτημα
- Μια ενέργεια που λειτουργεί από το περιβάλλον εργασίας Χρήστη του Azure Mobile δέσμευση δεν λειτουργεί από το σχετικό API δέσμευση Mobile Azure.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Επιβεβαίωση ότι μπορείτε να εκτελέσετε την ίδια ενέργεια από το περιβάλλον εργασίας Χρήστη του Azure Mobile δέσμευση εμφανίζει ότι έχετε συνδέσει αυτήν τη δυνατότητα του Azure Mobile δέσμευση σωστά με το SDK.

### <a name="see-also"></a>Δείτε επίσης
 
- [Τεκμηρίωση του περιβάλλοντος εργασίας Χρήστη][Link 1]
 
## <a name="error-messages"></a>Μηνύματα σφάλματος

### <a name="issue"></a>Το ζήτημα
- Κωδικοί σφάλματος με χρήση του API εμφανίζεται κατά το χρόνο εκτέλεσης ή στα αρχεία καταγραφής.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Ακολουθεί μια σύνθετη λίστα κοινές API κατάσταση αριθμών κωδικούς για αναφορά και προκαταρκτικού αντιμετώπιση προβλημάτων:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Δείτε επίσης

- [Τεκμηρίωση API - για λεπτομερή σφάλματα σε κάθε συγκεκριμένο API][Link 4]
 
## <a name="silent-failures"></a>Σιωπηρή αποτυχίες

### <a name="issue"></a>Το ζήτημα
- Η ενέργεια API αποτύχει χωρίς κανένα μήνυμα σφάλματος που εμφανίζεται κατά το χρόνο εκτέλεσης ή στα αρχεία καταγραφής.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Πολλά στοιχεία θα απενεργοποιηθεί το περιβάλλον εργασίας χρήστη του Azure Mobile δέσμευση σε περίπτωση που δεν είναι συνδεδεμένο σωστά, αλλά θα αποτύχει σιωπηρά από το API, επομένως, να θυμάστε να ελέγξετε την ίδια λειτουργία από το περιβάλλον εργασίας Χρήστη για να δείτε αν λειτουργεί.
- Azure δέσμευση Mobile και πολλές δυνατότητες για προχωρημένους του Azure δέσμευση Mobile που προσπαθείτε να χρησιμοποιήσετε, πρέπει να μεμονωμένα εμβέλειας στο την εφαρμογή σας με το SDK ως διαφορετικά βήματα πριν να τις χρησιμοποιήσετε.

### <a name="see-also"></a>Δείτε επίσης

- [Οδηγός αντιμετώπισης προβλημάτων - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 

Ενώ είναι δυνατό να επικολλήσετε ένα URI MongoLab μέσα στον κώδικα, συνιστάται να ρυθμίσετε τις παραμέτρους του στο περιβάλλον για διευκόλυνση της διαχείρισης. Με αυτόν τον τρόπο, εάν το URI αλλάξει, μπορείτε να ενημερώσετε το μέσω της πύλης Azure χωρίς να μεταβείτε στον κώδικα.


1. Στην πύλη του Azure, επιλέξτε **Εφαρμογές Web**.
1. Κάντε κλικ στο όνομα της εφαρμογής web στη λίστα εφαρμογών Web.  
![WebAppEntry][entry-website]  
Εμφανίζει τον πίνακα εργαλείων του Web App.

1. Στη γραμμή μενού, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Κάντε κύλιση προς τα κάτω στην ενότητα συμβολοσειρές σύνδεσης.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. **Όνομα**, πληκτρολογήστε MONGOLAB_URI.
1. Για **τιμή**, επικολλήστε τη συμβολοσειρά σύνδεσης που θα σας λαμβάνονται στην προηγούμενη ενότητα.
1. Επιλέξτε **Προσαρμογή** στην αναπτυσσόμενη λίστα **Type** (αντί για την προεπιλεγμένη **SQLAzure**).
1. Στη γραμμή εργαλείων, κάντε κλικ στην επιλογή **Αποθήκευση** .  
![SaveWebApp][button-website-save]

**Σημείωση:** Azure προσθέτει το **CUSTOMCONNSTR\_ ** πρόθεμα σε αυτή τη μεταβλητή, δηλαδή γιατί ο κωδικός παραπάνω αναφορές **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png


<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Παράδειγμα αρχείου ρύθμισης παραμέτρων για την ασφάλεια συμβολοσειρά σύνδεσης


Είναι unsound για να τοποθετήσετε τη συμβολοσειρά σύνδεσης ως λεκτικές σταθερές στον κώδικα C#. Είναι προτιμότερο να τοποθετήσετε τη συμβολοσειρά σύνδεσης σε ένα αρχείο ρύθμισης παραμέτρων. Μπορείτε να επεξεργαστείτε τη συμβολοσειρά οποιαδήποτε στιγμή χωρίς να χρειάζεται να μεταγλωττίσετε ξανά.

Ας υποθέσουμε ότι το μεταγλωττισμένο πρόγραμμα C# ονομάζεται **ConsoleApplication1.exe**και ότι αυτή .exe βρίσκεται σε μια **bin\debug\* * καταλόγου.

Σε αυτό το παράδειγμα, τα περισσότερα τμήματα της συμβολοσειρά σύνδεσης είναι αποθηκευμένες σε ένα αρχείο ρύθμισης παραμέτρων που ονομάζεται ακριβώς **ConsoleApplication1.exe.config**. Αυτό το αρχείο ρύθμισης παραμέτρων επίσης πρέπει να βρίσκονται σε **bin\debug\**.

Στην XML του αρχείου ρύθμισης παραμέτρων που ακολουθεί μπορείτε να δείτε μια συμβολοσειρά σύνδεσης με το όνομα **ConnectionString4NoUserIDNoPassword**. Ο κώδικας C# μοιάζει για αυτήν τη συμβολοσειρά.

Θα πρέπει να επεξεργαστείτε το πραγματικό ονόματα σε για τα σύμβολα κράτησης θέσης:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Για αυτήν την απεικόνιση επιλέγουμε για να παραλείψετε δύο παραμέτρους:

- Αναγνωριστικό χρήστη = {your_userName_here};
- Κωδικός πρόσβασης = {your_password_here};


Μπορείτε να συμπεριλάβετε τους, αλλά μερικές φορές είναι καλύτερα να έχετε το πρόγραμμα γρήγορα τις τιμές από την είσοδο πληκτρολογίου από το χρήστη. Εξαρτάται από το.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->

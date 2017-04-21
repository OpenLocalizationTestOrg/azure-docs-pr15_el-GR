
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Σύγκριση της συμβολοσειράς σύνδεσης


Ο παρακάτω πίνακας συγκρίνει τις συμβολοσειρές σύνδεσης που χρειάζεται το πρόγραμμα C# για να συνδεθείτε με τον SQL Server εσωτερικής εγκατάστασης έναντι της βάσης δεδομένων SQL Azure στο cloud. Οι διαφορές είναι με έντονη γραφή.


| Συμβολοσειρά σύνδεσης για<br/>Βάση δεδομένων SQL Azure | Συμβολοσειρά σύνδεσης για<br/>Microsoft SQL Server |
| :-- | :-- |
| Διακομιστής =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Αναγνωριστικό χρήστη = {your_loginName_here}**@{your_serverName_here}**;<br/>Κωδικός πρόσβασης = {your_password_here};<br/>**Βάση δεδομένων = {your_databaseName_here};**<br/>**Χρονικό όριο σύνδεσης = 30**;<br/>**Κρυπτογράφηση = True**;<br/>**TrustServerCertificate = False**; | Διακομιστής = {your_serverName_here};<br/>Αναγνωριστικό χρήστη = {your_loginName_here};<br/>Κωδικός πρόσβασης = {your_password_here}; |


Το **βάσης δεδομένων =** είναι προαιρετικό για τον SQL Server, αλλά είναι απαραίτητη για βάση δεδομένων SQL.


[Ιδιότητες SqlConnectionStringBuilder ADO .NET](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - ασχολείται με όλες τις παραμέτρους με λεπτομέρειες.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->

<properties
   pageTitle="Azure δεδομένων κατάλογος υποστηριζόμενων προελεύσεων δεδομένων | Microsoft Azure"
   description="Προδιαγραφή από τις προελεύσεις δεδομένων που υποστηρίζονται αυτήν τη στιγμή."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure δεδομένων κατάλογος υποστηριζόμενων προελεύσεων δεδομένων

Οι χρήστες του καταλόγου δεδομένων Azure μπορούν να δημοσιεύουν μετα-δεδομένων με χρήση μιας δημόσιας API, ένα κλικ-μία φορά δήλωσης εργαλείου ή εισάγοντας με μη αυτόματο τρόπο πληροφορίες απευθείας στον κατάλογο δεδομένων web πύλη. Το παρακάτω πλέγμα συνοψίζει όλες τις προελεύσεις που υποστηρίζεται από τον κατάλογο σήμερα και τις δυνατότητες δημοσίευσης για κάθε μία.  Επίσης, παρατίθενται βρίσκονται τα εργαλεία εξωτερικών δεδομένων που μπορεί να ξεκινήσει κάθε προέλευσης από την εμπειρία μας πύλη "Άνοιγμα με". Το δεύτερο πλέγμα στο άρθρο έχει περισσότερες τεχνικές λεπτομέρειες σχετικά με κάθε ιδιότητες σύνδεσης προελεύσεις δεδομένων.


## <a name="list-of-supported-data-sources"></a>Λίστα υποστηριζόμενων προελεύσεων δεδομένων

<table>

    <tr>
       <td><b>Αντικείμενο προέλευσης δεδομένων</b></td>
       <td><b>API</b></td>
       <td><b>Μη αυτόματη καταχώρηση</b></td>
       <td><b>Εργαλείο εγγραφής</b></td>
       <td><b>Άνοιγμα σε εργαλεία</b></td>
       <td><b>Σημειώσεις</b></td>
    </tr>

    <tr>
      <td>Χώρος αποθήκευσης λίμνης δεδομένων Azure καταλόγου</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αρχείο χώρου αποθήκευσης λίμνης δεδομένων Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure χώρο αποθήκευσης αντικειμένων Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Καταλόγου Azure χώρου αποθήκευσης</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας Azure χώρου αποθήκευσης</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS καταλόγου</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αρχείο HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ομάδα πίνακα</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή της ομάδας</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας βάσης δεδομένων Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή βάσης δεδομένων Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Άλλο (γενικό περιουσιακών στοιχείων)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας αποθήκη δεδομένων SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Εργαλεία δεδομένων του Excel, PowerBI, SQL Server</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή αποθήκη δεδομένων SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Εργαλεία δεδομένων του Excel, PowerBI, SQL Server</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Διάσταση υπηρεσιών ανάλυσης του SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Υπηρεσίες ανάλυσης του SQL Server KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services μέτρησης</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας του SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αναφοράς υπηρεσιών αναφοράς του SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Πρόγραμμα περιήγησης</font></td>
      <td><font size=2>Μόνο εγγενή λειτουργία των διακομιστών. Λειτουργία του SharePoint δεν υποστηρίζεται.</font></td>
    </tr>

    <tr>
      <td>Πίνακας του SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Εργαλεία δεδομένων του Excel, PowerBI, SQL Server</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Εργαλεία δεδομένων του Excel, PowerBI, SQL Server</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή Hana SAP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Υπολογισμός και αναλυτικό προβολές μόνο. Προβολές χαρακτηριστικό δεν υποστηρίζονται.</font></td>
    </tr>

    <tr>
      <td>Db2 πίνακα</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αρχείο συστήματος αρχείων</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Κατάλογος FTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αρχείο FTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αναφορά HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP τελικού σημείου</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Αρχείο HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ορισμός οντότητα OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Συνάρτηση OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Πίνακας Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Προβολή Hana SAP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Αντικείμενο Salesforce</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Λίστα του SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Εάν χρειάζεστε υποστήριξη για πρόσθετες προελεύσεις, υποβάλετε μια αίτηση δυνατότητα χρησιμοποιώντας το [Φόρουμ του καταλόγου δεδομένων του Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Προδιαγραφή αναφορά προέλευσης δεδομένων
> [AZURE.NOTE] Στήλη "DSL δομής" στις παρακάτω πίνακα μόνο λίστες ιδιότητες σύνδεσης για τσάντα ιδιότητα "Διεύθυνση" που χρησιμοποιούνται από τον κατάλογο δεδομένων Azure (δηλαδή τσάντα ιδιότητα "Διεύθυνση" μπορεί να περιέχει άλλες ιδιότητες σύνδεσης του αρχείου προέλευσης δεδομένων που εξακολουθεί να εμφανίζεται στον κατάλογο δεδομένων Azure, αλλά δεν χρησιμοποιεί.)
<table>
    <tr>
       <td><b>Τύπος προέλευσης</b></td>
       <td><b>Τύπος περιουσιακών στοιχείων</b></td>
       <td><b>Τύπους αντικειμένου</b></td>
       <td><b>Δομή DSL<b></td>
    </tr>
    <tr>
      <td>Χώρος αποθήκευσης λίμνης δεδομένων Azure</td>
      <td>Κοντέινερ</td>
      <td>Δεδομένα λίμνης</td>
      <td>
        <font size=2>πρωτόκολλο: webhdfs <br>έλεγχος ταυτότητας: {βασικές, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Χώρος αποθήκευσης λίμνης δεδομένων Azure</td>
      <td>Πίνακας</td>
      <td>Κατάλογος, αρχείο</td>
      <td>
        <font size=2>πρωτόκολλο: webhdfs <br>έλεγχος ταυτότητας: {βασικές, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure χώρου αποθήκευσης</td>
      <td>Κοντέινερ</td>
      <td>Κοντέινερ</td>
      <td>
        <font size=2>πρωτόκολλο: azure-αντικείμενα BLOB <br>έλεγχος ταυτότητας: {azure-access-κλειδί} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;τομέα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;λογαριασμός <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;κοντέινερ</font>
      </td>
    </tr>
    <tr>
      <td>Azure χώρου αποθήκευσης</td>
      <td>Πίνακας</td>
      <td>Blob, καταλόγου</td>
      <td>
        <font size=2>πρωτόκολλο: azure-αντικείμενα BLOB <br>έλεγχος ταυτότητας: {azure-access-κλειδί} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;τομέα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;λογαριασμός <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;κοντέινερ <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Όνομα</font>
      </td>
    </tr>
    <tr>
      <td>Azure χώρου αποθήκευσης</td>
      <td>Κοντέινερ</td>
      <td>Κοντέινερ</td>
      <td>
        <font size=2>πρωτόκολλο: πίνακες azure <br>έλεγχος ταυτότητας: {azure-access-κλειδί} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;τομέα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;λογαριασμός</font>
      </td>
    </tr>
    <tr>
      <td>Azure χώρου αποθήκευσης</td>
      <td>Πίνακας</td>
      <td>Πίνακας</td>
      <td>
        <font size=2>πρωτόκολλο: πίνακες azure <br>έλεγχος ταυτότητας: {azure-access-κλειδί} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;τομέα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;λογαριασμός <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Όνομα</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Κοντέινερ</td>
      <td>Εικονικό συμπλέγματος</td>
      <td>
        <font size=2>πρωτόκολλο: cosmos <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Πίνακας</td>
      <td>Ροή, η ροή σύνολο, προβολή</td>
      <td>
        <font size=2>πρωτόκολλο: cosmos <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Κοντέινερ</td>
      <td>Τοποθεσία</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Αναφορά</td>
      <td>Αναφορά, πίνακα εργαλείων</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: db2 <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: db2 <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα</font>
      </td>
    </tr>
    <tr>
      <td>Το σύστημα αρχείων</td>
      <td>Πίνακας</td>
      <td>Αρχείο</td>
      <td>
        <font size=2>πρωτόκολλο: αρχείο <br>έλεγχος ταυτότητας: {κανένα, βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διαδρομή</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Πίνακας</td>
      <td>Κατάλογος, αρχείο</td>
      <td>
        <font size=2>πρωτόκολλο: ftp <br>έλεγχος ταυτότητας: {κανένα, βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Κατανεμημένο σύστημα αρχείων Hadoop</td>
      <td>Κοντέινερ</td>
      <td>Σύμπλεγμα</td>
      <td>
        <font size=2>πρωτόκολλο: webhdfs <br>έλεγχος ταυτότητας: {βασικές, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Κατανεμημένο σύστημα αρχείων Hadoop</td>
      <td>Πίνακας</td>
      <td>Κατάλογος, αρχείο</td>
      <td>
        <font size=2>πρωτόκολλο: webhdfs <br>έλεγχος ταυτότητας: {βασικές, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Ομάδα</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: ομάδα <br>έλεγχος ταυτότητας: {hdinsight, βασικές, το όνομα χρήστη, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Ομάδα</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: ομάδα <br>έλεγχος ταυτότητας: {hdinsight, βασικές, το όνομα χρήστη, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Κοντέινερ</td>
      <td>Τοποθεσία</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Αναφορά</td>
      <td>Αναφορά, πίνακα εργαλείων</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Πίνακας</td>
      <td>Τελικό σημείο, αρχείο</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: mysql <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: mysql <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Κοντέινερ</td>
      <td>Οντότητα κοντέινερ</td>
      <td>
        <font size=2>πρωτόκολλο: odata <br>έλεγχος ταυτότητας: {κανένα, βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Πίνακας</td>
      <td>Ορισμός οντότητα, συνάρτηση</td>
      <td>
        <font size=2>πρωτόκολλο: odata <br>έλεγχος ταυτότητας: {κανένα, βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;πόρων</font>
      </td>
    </tr>
    <tr>
      <td>Βάση δεδομένων της Oracle</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: oracle <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>Βάση δεδομένων της Oracle</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: oracle <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: postgresql <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: postgresql <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Κοντέινερ</td>
      <td>Τοποθεσία</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Αναφορά</td>
      <td>Αναφορά, πίνακα εργαλείων</td>
      <td>
        <font size=2>πρωτόκολλο: http <br>έλεγχος ταυτότητας: {κανένα, basic, windows, διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Πίνακας</td>
      <td>Συνδυαστική εφαρμογή δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: power query <br>έλεγχος ταυτότητας: {διακριτικό} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Πίνακας</td>
      <td>Αντικείμενο</td>
      <td>
        <font size=2>πρωτόκολλο: salesforce com <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;τάξης <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;όνομα στοιχείου</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Κοντέινερ</td>
      <td>Διακομιστή</td>
      <td>
        <font size=2>πρωτόκολλο: sap-hana-sql <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Πίνακας</td>
      <td>Προβολή</td>
      <td>
        <font size=2>πρωτόκολλο: sap-hana-sql <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Του SharePoint</td>
      <td>Πίνακας</td>
      <td>Λίστα</td>
      <td>
        <font size=2>πρωτόκολλο: λίστα του sharepoint <br>έλεγχος ταυτότητας: {βασικές, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL</font>
      </td>
    </tr>
    <tr>
      <td>Αποθήκη δεδομένων του SQL</td>
      <td>Εντολή</td>
      <td>Αποθηκευμένη διαδικασία</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Αποθήκη δεδομένων του SQL</td>
      <td>TableValuedFunction</td>
      <td>Συνάρτηση με πίνακα τιμών</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Αποθήκη δεδομένων του SQL</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>Αποθήκη δεδομένων του SQL</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Εντολή</td>
      <td>Αποθηκευμένη διαδικασία</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Συνάρτηση με πίνακα τιμών</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: κδτ <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;σχήμα <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server πολυδιάστατων</td>
      <td>Κοντέινερ</td>
      <td>Μοντέλο</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server πολυδιάστατων</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server πολυδιάστατων</td>
      <td>Μέτρηση</td>
      <td>Μέτρηση</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {μέτρηση}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server πολυδιάστατων</td>
      <td>Πίνακας</td>
      <td>Διάστασης</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {διάσταση}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server σε μορφή πίνακα</td>
      <td>Κοντέινερ</td>
      <td>Μοντέλο</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server σε μορφή πίνακα</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server σε μορφή πίνακα</td>
      <td>Μέτρηση</td>
      <td>Μέτρηση</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {μέτρηση}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες ανάλυσης SQL Server σε μορφή πίνακα</td>
      <td>Πίνακας</td>
      <td>Πίνακας</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες ανάλυσης <br>έλεγχος ταυτότητας: {windows, βασικός, ανώνυμος, κανένα} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Τύπος_αντικειμένου: {πίνακας}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες αναφοράς του SQL Server</td>
      <td>Κοντέινερ</td>
      <td>Διακομιστή</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες αναφοράς <br>έλεγχος ταυτότητας: {windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;έκδοση: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες αναφοράς του SQL Server</td>
      <td>Αναφορά</td>
      <td>Αναφορά</td>
      <td>
        <font size=2>πρωτόκολλο: υπηρεσίες αναφοράς <br>έλεγχος ταυτότητας: {windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διαδρομή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;έκδοση: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Κοντέινερ</td>
      <td>Βάση δεδομένων</td>
      <td>
        <font size=2>πρωτόκολλο: teradata <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Πίνακας</td>
      <td>Προβολή πίνακα</td>
      <td>
        <font size=2>πρωτόκολλο: teradata <br>έλεγχος ταυτότητας: {πρωτόκολλο, windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διακομιστή <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;βάση δεδομένων <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;αντικείμενο</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες δεδομένων του SQL Server υπόδειγμα</td>
      <td>Κοντέινερ</td>
      <td>Μοντέλο</td>
      <td>
        <font size="2">πρωτόκολλο: mssql mds <br>έλεγχος ταυτότητας: {windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;έκδοση</font>
      </td>
    </tr>
    <tr>
      <td>Υπηρεσίες δεδομένων του SQL Server υπόδειγμα</td>
      <td>Πίνακας</td>
      <td>Οντότητα</td>
      <td>
        <font size="2">πρωτόκολλο: mssql mds <br>έλεγχος ταυτότητας: {windows} <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;διεύθυνση URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;μοντέλο <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;έκδοση <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;οντότητα</font>
      </td>
    </tr>
    <tr>
      <td>Άλλο (όχι ένα από τα παραπάνω)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>πρωτόκολλο: γενικό-περιουσιακών στοιχείων <br>διεύθυνση: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </td>
    </tr>
</table>

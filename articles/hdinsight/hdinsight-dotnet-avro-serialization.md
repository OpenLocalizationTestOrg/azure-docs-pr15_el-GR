<properties
    pageTitle="Σειριοποίηση δεδομένων με τη βιβλιοθήκη Avro Microsoft | Microsoft Azure"
    description="Μάθετε πώς χρησιμοποιεί το Azure HDInsight Avro σειριοποίηση μεγάλο δεδομένων."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Σειριοποίηση δεδομένων σε Hadoop με τη βιβλιοθήκη Avro της Microsoft

Αυτό το θέμα εξηγεί πώς μπορείτε να χρησιμοποιήσετε τη <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Βιβλιοθήκη Avro Microsoft</a> σειριοποίηση αντικειμένων και άλλες δομές δεδομένων σε ροές προκειμένου να παραμένει τους να μνήμη, μια βάση δεδομένων ή ένα αρχείο, καθώς και τον τρόπο αποσειριοποίησης τους για να ανακτήσετε τα αρχικά αντικείμενα.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
Στη <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Βιβλιοθήκη Microsoft Avro</a> εφαρμόζει το σύστημα Apache Avro δεδομένων σειριοποίησης για το περιβάλλον Microsoft.NET. Apache Avro παρέχει μια μορφή ανταλλαγής συμπαγής δυαδικά δεδομένα σειριοποίησης. Χρησιμοποιεί <a href="http://www.json.org" target="_blank">JSON</a> για να ορίσετε ένα σχήμα agnostic γλώσσας που καλύπτουν διαλειτουργικότητα γλώσσας. Μπορείτε να διαβάσετε σειριοποίηση σε μία γλώσσα δεδομένων σε ένα άλλο. Προς το παρόν υποστηρίζονται C, C++, C#, Java, PHP, Python και φωνητικής γραφής. Μπορείτε να βρείτε λεπτομερείς πληροφορίες σχετικά με τη μορφή στην <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro προδιαγραφή</a>. Σημειώστε ότι η τρέχουσα έκδοση της βιβλιοθήκης Avro Microsoft δεν υποστηρίζει το τμήμα κλήσεων (RPC) απομακρυσμένης διαδικασίας της παρούσας προδιαγραφής.

Η σειριακή αναπαράσταση του αντικειμένου στο σύστημα του Avro αποτελείται από δύο μέρη: σχήματος και πραγματική τιμή. Το σχήμα Avro περιγράφει το μοντέλο δεδομένων γλώσσας ανεξάρτητο από τα δεδομένα του αλληλουχίας με JSON. Είναι παρούσα δίπλα-δίπλα με μια δυαδική αναπαράσταση των δεδομένων. Αντιμετωπίζετε το σχήμα ξεχωριστά από τη δυαδική αναπαράσταση σας επιτρέπει να να γράφονται με χωρίς διαφάνειες ανά τιμή, καθιστώντας σειριοποίησης ταχύτητας και η απεικόνιση μικρές κάθε αντικείμενο.

##<a name="the-hadoop-scenario"></a>Το σενάριο Hadoop
Η μορφή σειριοποίησης Apache Avro χρησιμοποιείται ευρέως Azure HDInsight και άλλα Apache Hadoop περιβάλλον. Avro παρέχει έναν εύχρηστο τρόπο για να αναπαραστήσετε δομές σύνθετα δεδομένα μέσα σε μια εργασία Hadoop MapReduce. Η μορφή αρχείων Avro (αρχείο Avro αντικείμενο κοντέινερ) έχει σχεδιαστεί για την υποστήριξη του μοντέλου προγραμματισμού κατανέμεται MapReduce. Η δυνατότητα κλειδιού που επιτρέπει την κατανομή είναι ότι τα αρχεία είναι "Πίνακας Διαίρεση πίνακα" με την έννοια ότι μία μπορεί να αναζητά οποιοδήποτε σημείο σε ένα αρχείο και να ξεκινήσετε ανάγνωσης από ένα συγκεκριμένο μπλοκ.

##<a name="serialization-in-avro-library"></a>Σειριοποίησης στη βιβλιοθήκη Avro
Η βιβλιοθήκη .NET για Avro υποστηρίζει δύο τρόπους σειριοποίησης αντικειμένων:

- **αντανάκλαση** - JSON το σχήμα για τους τύπους δημιουργείται αυτόματα από τα δεδομένα σύμβασης χαρακτηριστικά από τους τύπους .NET για σειριοποίηση.
- **γενικό εγγραφή** - σχήματος A JSON καθορίζεται ρητά σε μια εγγραφή που αναπαρίσταται από την κλάση [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) όταν δεν υπάρχουν τύποι .NET υπάρχουν για να περιγράψετε το σχήμα για τα δεδομένα για να σειριοποιηθεί.

Όταν το σχήμα δεδομένων είναι γνωστό τόσο το writer και στο πρόγραμμα ανάγνωσης της ροής, τα δεδομένα μπορούν να αποσταλούν χωρίς το σχήμα. Στις περιπτώσεις όταν χρησιμοποιείται ένα αρχείο Avro αντικείμενο κοντέινερ, το σχήμα είναι αποθηκευμένο μέσα στο αρχείο. Οι υπόλοιπες παράμετροι, όπως ο κωδικοποιητής που χρησιμοποιήθηκε για τη συμπίεση δεδομένων, μπορεί να καθοριστεί. Αυτά τα σενάρια που περιγράφονται με περισσότερες λεπτομέρειες και με εικόνες στα παρακάτω παραδείγματα κώδικα.


## <a name="install-avro-library"></a>Εγκατάσταση Avro βιβλιοθήκη

Ακολουθούν απαιτείται πριν από την εγκατάσταση της βιβλιοθήκης:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 ή νεότερη έκδοση)

Σημειώστε ότι η εξάρτηση Newtonsoft.Json.dll ληφθεί αυτόματα με την εγκατάσταση της βιβλιοθήκης Avro της Microsoft. Η διαδικασία αυτή παρέχεται στην επόμενη ενότητα.


Στη βιβλιοθήκη Microsoft Avro διανέμεται ως ένα πακέτο NuGet που μπορεί να εγκατασταθεί από το Visual Studio μέσω την ακόλουθη διαδικασία:

1. Επιλέξτε την καρτέλα " **έργο** " -> **Διαχείριση NuGet πακέτων...**
2. Αναζήτηση για "Microsoft.Hadoop.Avro" στο πλαίσιο **Αναζήτησης Online** .
3. Κάντε κλικ στο κουμπί **εγκατάσταση** δίπλα στο στοιχείο **Microsoft Azure HDInsight Avro βιβλιοθήκη**.

Λάβετε υπόψη ότι η Newtonsoft.Json.dll (> = 6.0.4) εξάρτηση είναι επίσης ληφθεί αυτόματα, με τη βιβλιοθήκη Avro της Microsoft.

Εάν θέλετε, μπορείτε να επισκεφθείτε την <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">αρχική σελίδα Microsoft Avro βιβλιοθήκη</a> για να διαβάσετε τις τρέχουσες σημειώσεις έκδοσης.


Η βιβλιοθήκη Avro Microsoft πηγαίος κώδικας είναι διαθέσιμη στη <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">βιβλιοθήκη Microsoft Avro αρχική σελίδα</a>.

##<a name="compile-schemas-using-avro-library"></a>Μεταγλώττιση σχημάτων με χρήση Avro βιβλιοθήκη

Η βιβλιοθήκη Microsoft Avro περιέχει ένα βοηθητικό πρόγραμμα γενιάς κώδικα που επιτρέπει τη δημιουργία C# τύπων αυτόματα με βάση τη διάταξη παλιότερα καθορισμένη JSON. Το βοηθητικό πρόγραμμα δημιουργίας κώδικα δεν διανέμεται ως ένα δυαδικό εκτελέσιμο αρχείο, αλλά μπορεί να δημιουργηθεί εύκολα μέσω την ακόλουθη διαδικασία:

1. Κάντε λήψη του αρχείου .zip με την πιο πρόσφατη έκδοση του HDInsight SDK πηγαίου κώδικα από <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK για Hadoop</a>. (Κάντε κλικ στο εικονίδιο **λήψης** , όχι στην καρτέλα **στοιχεία λήψης** .)

2. Εξαγάγετε το HDInsight SDK σε έναν κατάλογο στον υπολογιστή με το .NET Framework 4 εγκατεστημένη και συνδεδεμένοι στο Internet για τη λήψη πακέτων NuGet εξάρτηση είναι απαραίτητο. Παρακάτω θα υποθέσουμε ότι ότι C:\SDK εξαγωγή της τον πηγαίο κώδικα.

3. Μεταβείτε στο φάκελο C:\SDK\src\Microsoft.Hadoop.Avro.Tools και εκτελέστε build.bat. (Το αρχείο θα καλέσετε MSBuild από την κατανομή 32-bit του .NET Framework. Εάν θέλετε να χρησιμοποιήσετε την έκδοση 64 bit, επεξεργαστείτε build.bat, ακολουθώντας τα σχόλια εντός του αρχείου.) Βεβαιωθείτε ότι η Δόμηση είναι επιτυχής. (Σε ορισμένα συστήματα, MSBuild μπορεί να παράγει προειδοποιήσεων. Αυτές οι προειδοποιήσεις δεν επηρεάζουν το βοηθητικό πρόγραμμα με την προϋπόθεση ότι δεν υπάρχουν σφάλματα Δόμηση.)

4. Βοηθητικού προγράμματος μεταγλωττισμένο βρίσκεται στο C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Για να εξοικειωθείτε με τη σύνταξη γραμμής εντολών, εκτελέστε την ακόλουθη εντολή από το φάκελο όπου βρίσκεται το βοηθητικό πρόγραμμα γενιάς κώδικα:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Για να ελέγξετε το βοηθητικό πρόγραμμα, μπορείτε να δημιουργήσετε κατηγορίες C# από το δείγμα αρχείου σχήματος JSON που παρέχεται με τον πηγαίο κώδικα. Εκτελέστε την ακόλουθη εντολή:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Αυτό είναι πρέπει να δημιουργήσουν δύο C# αρχεία του τρέχοντος καταλόγου: SensorData.cs και Location.cs.

Για να κατανοήσετε της λογικής που χρησιμοποιεί το βοηθητικό πρόγραμμα γενιάς κώδικα κατά τη μετατροπή του σχήματος JSON C# τύπους, ανατρέξτε στο θέμα το αρχείο GenerationVerification.feature βρίσκεται στο C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Σημειώστε ότι οι χώροι ονομάτων εξάγονται από το σχήμα JSON, με χρήση της λογικής που περιγράφονται στο αρχείο που αναφέρονται στην προηγούμενη παράγραφο. Πεδία ονομάτων που έχουν εξαχθεί από το σχήμα προηγούνται ό, τι παρέχονται με την παράμετρο/ν στη γραμμή εντολών βοηθητικού προγράμματος. Εάν θέλετε να παρακάμψετε τα πεδία ονομάτων που περιέχεται μέσα στο σχήμα, χρησιμοποιήστε την παράμετρο /nf. Για παράδειγμα, για να αλλάξετε όλοι οι χώροι ονομάτων από το SampleJSONSchema.avsc my.own.nspace, εκτελέστε την ακόλουθη εντολή:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Δείγματα
Έξι παραδείγματα που παρέχονται σε αυτό το θέμα απεικόνιση διαφορετικά σενάρια που υποστηρίζονται από τη βιβλιοθήκη Avro της Microsoft. Στη βιβλιοθήκη Microsoft Avro έχει σχεδιαστεί για να εργαστείτε με οποιαδήποτε ροή. Σε αυτά τα παραδείγματα, γίνεται χειρισμός των δεδομένων μέσω ροών μνήμης και όχι ροών αρχείων ή σε βάσεις δεδομένων για λόγους απλούστευσης και συνέπειας. Η προσέγγιση που λαμβάνονται σε ένα περιβάλλον παραγωγής θα εξαρτώνται από τις απαιτήσεις ακριβή σενάριο, αρχείο προέλευσης δεδομένων και ένταση, περιορισμών απόδοσης και άλλους παράγοντες.

Τα πρώτα δύο παραδείγματα δείχνουν τον τρόπο σειριοποίηση και αποσειριοποίηση δεδομένων σε buffer ροής μνήμης με χρήση αντανάκλαση και γενικό εγγραφές. Το σχήμα σε αυτές τις δύο περιπτώσεις θεωρείται ότι είναι κοινόχρηστη μεταξύ των προγραμμάτων ανάγνωσης και συντάκτες εκτός εύρους.

Η τρίτη και η τέταρτη παραδείγματα δείχνουν τον τρόπο σειριοποίηση και αποσειριοποίηση δεδομένων, χρησιμοποιώντας τα αρχεία Avro αντικείμενο κοντέινερ. Όταν δεδομένων είναι αποθηκευμένη σε ένα αρχείο κοντέινερ Avro, του σχήματός αποθηκεύεται πάντα με αυτήν, επειδή το σχήμα πρέπει να είναι κοινόχρηστη για αποσειριοποίηση.

Το δείγμα που περιέχει τα πρώτα τέσσερα παραδείγματα μπορούν να ληφθούν από την τοποθεσία <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">δείγματα Azure κώδικα</a> .

Δείχνει το πέμπτο παράδειγμα με τον τρόπο για να χρησιμοποιήσετε ο κωδικοποιητής προσαρμοσμένο συμπίεσης για αρχεία κοντέινερ Avro αντικειμένου. Ένα δείγμα που περιέχει τον κώδικα για αυτό το παράδειγμα μπορεί να ληφθεί από την τοποθεσία <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">δείγματα Azure κώδικα</a> .

Το έκτο δείγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε σειριοποίησης Avro για την αποστολή δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob του Azure και, στη συνέχεια, ανάλυση του χρησιμοποιώντας Hive με ένα σύμπλεγμα HDInsight (Hadoop). Αυτό μπορεί να ληφθεί από την τοποθεσία <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">δείγματα Azure κώδικα</a> .

Εδώ θα βρείτε συνδέσεις για τα έξι δείγματα που περιγράφονται στο θέμα:

 * <a href="#Scenario1">**Σειριοποίησης με αντανάκλαση**</a> - JSON το σχήμα για τους τύπους για σειριοποίηση δημιουργείται αυτόματα από τα δεδομένα σύμβασης χαρακτηριστικά.
 * <a href="#Scenario2">**Σειριοποίησης με εγγραφή γενικής χρήσης**</a> - JSON το σχήμα είναι καθορίζεται ρητά σε μια εγγραφή, όταν δεν υπάρχει τύπος .NET είναι διαθέσιμο για αντανάκλαση.
 * <a href="#Scenario3">**Σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων με αντανάκλαση**</a> - JSON το σχήμα δημιουργηθεί αυτόματα και η κοινή χρήση μαζί με τον αλληλουχίας δεδομένων μέσω ενός αρχείου Avro αντικείμενο κοντέινερ.
 * <a href="#Scenario4">**Σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων με εγγραφή γενικής χρήσης**</a> - JSON το σχήμα που καθορίζεται πριν από την σειριοποίησης ρητά και η κοινή χρήση μαζί με τα δεδομένα μέσω ενός αρχείου Avro αντικείμενο κοντέινερ.
 * <a href="#Scenario5">**Σειριοποίηση με χρήση αρχεία κοντέινερ αντικειμένου με ένα προσαρμοσμένο συμπίεσης κωδικοποιητή**</a> - το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα αρχείο Avro αντικείμενο κοντέινερ με μια προσαρμοσμένη εφαρμογή .NET του κωδικοποιητή συμπίεσης Deflate δεδομένων.
 * <a href="#Scenario6">**Χρήση Avro για την αποστολή δεδομένων για την υπηρεσία Microsoft Azure HDInsight**</a> - το παράδειγμα παρουσιάζει τον τρόπο σειριοποίησης Avro αλληλεπιδρά με την υπηρεσία HDInsight. Μια ενεργή συνδρομή του Azure και την πρόσβαση σε ένα σύμπλεγμα Azure HDInsight απαιτούνται για να εκτελέσετε αυτό το παράδειγμα.

###<a name="Scenario1"></a>Παράδειγμα 1: Σειριοποίησης με αντανάκλαση

Το σχήμα JSON για τους τύπους μπορεί να αυτόματα δημιουργηθεί από τη βιβλιοθήκη Avro Microsoft μέσω αντανάκλαση από τα δεδομένα σύμβασης χαρακτηριστικά τα αντικείμενα C# για να σειριοποιηθεί. Στη βιβλιοθήκη Microsoft Avro δημιουργεί μια [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) για τον προσδιορισμό των πεδίων για σειριοποίηση.

Σε αυτό το παράδειγμα, είναι σειριοποιηθεί αντικείμενα ( **SensorData** κλάση με δομή **θέση** μέλος) σε μια ροή μνήμης και αυτή η ροή είναι αποσειριοποιηθούν με τη σειρά. Το αποτέλεσμα είναι, στη συνέχεια, σε σχέση με την αρχική παρουσία για να επιβεβαιώσετε ότι το αντικείμενο **SensorData** ανάκτησης είναι ίδια με την αρχική.

Το σχήμα σε αυτό το παράδειγμα θεωρείται ότι είναι κοινόχρηστη μεταξύ των προγραμμάτων ανάγνωσης και συντάκτες, ώστε το Avro αντικείμενο κοντέινερ μορφή δεν είναι απαραίτητο. Για ένα παράδειγμα του τρόπου σειριοποίηση και αποσειριοποίηση δεδομένων σε buffer μνήμης με χρήση αντανάκλαση με τη μορφή κοντέινερ αντικείμενο, όταν το σχήμα πρέπει να είναι κοινόχρηστη με τα δεδομένα, ανατρέξτε στο θέμα <a href="#Scenario3">σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων με αντανάκλαση</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Παράδειγμα 2: Σειριοποίησης με μια εγγραφή γενικής χρήσης

Ένα σχήμα JSON μπορεί να ρητά καθοριστεί σε μια εγγραφή γενικής χρήσης όταν δεν μπορούν να χρησιμοποιηθούν αντανάκλαση, επειδή τα δεδομένα δεν είναι δυνατό να αντιπροσωπεύεται μέσω κλάσεις .NET με ένα συμβόλαιο δεδομένων. Αυτή η μέθοδος είναι συνήθως μικρότερη από τη χρήση αντανάκλαση. Σε αυτές τις περιπτώσεις, το σχήμα για τα δεδομένα επίσης να δυναμική, δηλαδή, δεν είναι γνωστή κατά τη μεταγλώττιση. Δεδομένα με τιμές διαχωρισμένες με κόμματα (CSV) αρχεία των οποίων το σχήμα είναι άγνωστη μέχρι να μετατραπεί σε μορφή Avro κατά το χρόνο εκτέλεσης είναι ένα παράδειγμα αυτού του είδους δυναμικής σεναρίου.

Αυτό το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ένα [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) για να καθορίσετε ρητά ένα σχήμα JSON, πώς μπορείτε να συμπληρώσετε το με τα δεδομένα και, στη συνέχεια, πώς να σειριοποίηση και αποσειριοποίηση του. Το αποτέλεσμα είναι, στη συνέχεια, σε σχέση με την αρχική παρουσία για να επιβεβαιώσετε ότι η εγγραφή ανάκτησης είναι ίδιο με το αρχικό.

Το σχήμα σε αυτό το παράδειγμα θεωρείται ότι είναι κοινόχρηστη μεταξύ των προγραμμάτων ανάγνωσης και συντάκτες, ώστε το Avro αντικείμενο κοντέινερ μορφή δεν είναι απαραίτητο. Για ένα παράδειγμα του τρόπου σειριοποίηση και αποσειριοποίηση δεδομένων σε buffer μνήμης με χρήση μιας εγγραφής γενικής χρήσης με τη μορφή κοντέινερ αντικείμενο όταν το σχήμα πρέπει να συμπεριλαμβάνονται με τα δεδομένα του αλληλουχίας, δείτε το παράδειγμα <a href="#Scenario4">σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων με γενικό εγγραφή</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Παράδειγμα 3: Σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων και σειριοποίησης με αντανάκλαση

Αυτό το παράδειγμα είναι παρόμοιο με το σενάριο, στο <a href="#Scenario1">πρώτο παράδειγμα</a>, όπου το σχήμα μη ρητά καθορίζεται με αντανάκλαση. Η διαφορά είναι ότι εδώ, το σχήμα δεν λαμβάνεται είναι γνωστή με το πρόγραμμα ανάγνωσης που deserializes το. Τα αντικείμενα **SensorData** για σειριοποίηση και τους μη ρητά καθορισμένο σχήμα αποθηκεύονται σε ένα αρχείο Avro αντικείμενο κοντέινερ, που αντιπροσωπεύονται από την κλάση [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) .

Τα δεδομένα είναι σειριοποιημένο σε αυτό το παράδειγμα με [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) και Αποσειριοποιημένο με [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Το αποτέλεσμα, στη συνέχεια, συγκρίνεται με το αρχικό παρουσίες για να βεβαιωθείτε ότι ταυτότητα.

Τα δεδομένα στο αρχείο αντικειμένου κοντέινερ είναι συμπιεσμένη μέσω της προεπιλεγμένης [**Deflate**] [ deflate-100] κωδικοποιητής συμπίεσης από το .NET Framework 4. Δείτε το <a href="#Scenario5">πέμπτο παράδειγμα</a> σε αυτό το θέμα για να μάθετε πώς μπορείτε να χρησιμοποιήσετε ένα πιο πρόσφατο ή ανώτερη έκδοση του το [**Deflate**] [ deflate-110] κωδικοποιητής συμπίεσης διαθέσιμες στο διαίρεσης 4,5 .NET Framework.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Δείγμα 4: Σειριοποίηση με χρήση αντικειμένου κοντέινερ αρχείων και σειριοποίησης με εγγραφή γενικής χρήσης

Αυτό το παράδειγμα είναι παρόμοιο με το σενάριο στο <a href="#Scenario2">δεύτερο παράδειγμα</a>, όπου το σχήμα ρητά καθορίζεται με JSON. Η διαφορά είναι ότι εδώ, το σχήμα δεν λαμβάνεται είναι γνωστή με το πρόγραμμα ανάγνωσης που deserializes το.

Το σύνολο δεδομένων δοκιμής συλλέγονται σε μια λίστα των αντικειμένων [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) μέσω ενός ρητά καθορισμένο σχήμα JSON και, στη συνέχεια, είναι αποθηκευμένα σε ένα αρχείο κοντέινερ αντικείμενο που αναπαρίσταται από την κλάση [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) . Αυτό το αρχείο κοντέινερ δημιουργεί ένα πρόγραμμα εγγραφής που χρησιμοποιείται για να σειριοποίηση τα δεδομένα, χωρίς συμπίεση σε ροή μνήμης που είναι αποθηκευμένο στη συνέχεια, σε ένα αρχείο. Η παράμετρος [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) που χρησιμοποιούνται για τη δημιουργία του προγράμματος ανάγνωσης Καθορίζει ότι δεν θα συμπιέζονται αυτών των δεδομένων.

Τα δεδομένα, στη συνέχεια, διαβάστε από το αρχείο και αποσειριοποιηθούν σε μια συλλογή των αντικειμένων. Αυτή η συλλογή είναι σε σύγκριση με το αρχικό λίστα καρτελών Avro για να επιβεβαιώσετε ότι είναι ίδιες.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Δείγμα 5: Σειριοποίησης χρήση αντικειμένου κοντέινερ αρχείων με ο κωδικοποιητής προσαρμοσμένο συμπίεσης

Δείχνει το πέμπτο παράδειγμα με τον τρόπο για να χρησιμοποιήσετε ο κωδικοποιητής προσαρμοσμένο συμπίεσης για αρχεία κοντέινερ Avro αντικειμένου. Ένα δείγμα που περιέχει τον κώδικα για αυτό το παράδειγμα μπορεί να ληφθεί από την τοποθεσία [δείγματα Azure κώδικα](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) .

Η [Προδιαγραφή Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) επιτρέπει χρήση της κωδικοποιητή προαιρετική συμπίεση (εκτός από τις προεπιλεγμένες τιμές **Null** και **Deflate** ). Αυτό το παράδειγμα δεν υλοποιεί μια εντελώς νέα κωδικοποιητή όπως Snappy (που αναφέρεται ως ο κωδικοποιητής υποστηριζόμενες προαιρετικό στην [Προδιαγραφή Avro](http://avro.apache.org/docs/current/spec.html#snappy)). Δείχνει πώς μπορείτε να χρησιμοποιήσετε την εφαρμογή διαίρεσης 4,5 .NET Framework της το [**Deflate**] [ deflate-110] κωδικοποιητή, η οποία παρέχει μια καλύτερη συμπίεση αλγόριθμο με βάση τη βιβλιοθήκη συμπίεσης [zlib](http://zlib.net/) από την προεπιλεγμένη έκδοση .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Δείγμα 6: Χρήση Avro για την αποστολή δεδομένων για την υπηρεσία Microsoft Azure HDInsight

Το έκτο παράδειγμα παρουσιάζει ορισμένες τεχνικές προγραμματισμού που σχετίζονται με την αλληλεπίδραση με την υπηρεσία Azure HDInsight. Ένα δείγμα που περιέχει τον κώδικα για αυτό το παράδειγμα μπορεί να ληφθεί από την τοποθεσία [δείγματα Azure κώδικα](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) .

Το δείγμα κάνει τα εξής:

* Συνδέεται με ένα υπάρχον σύμπλεγμα υπηρεσίας HDInsight.
* Τοποθετεί σειριακά διάφορα αρχεία CSV και αποστέλλει το αποτέλεσμα με το χώρο αποθήκευσης αντικειμένων Blob του Azure. (Αρχεία CSV διανέμονται μαζί με το δείγμα και αντιπροσωπεύουν ένα απόσπασμα μετοχών AMEX δεδομένα ιστορικού διανεμηθεί από [Infochimps](http://www.infochimps.com/) για την περίοδο 1970-2010. Το δείγμα διαβάζει δεδομένα του αρχείου CSV, μετατρέπει τις εγγραφές σε παρουσίες της κλάσης **μετοχών** και, στη συνέχεια, τοποθετεί σειριακά τους χρησιμοποιώντας αντανάκλαση. Ορισμός τύπου μετοχών δημιουργείται από ένα σχήμα JSON μέσω το βοηθητικό πρόγραμμα Microsoft Avro βιβλιοθήκη κώδικα γενιάς.
* Δημιουργείται ένας νέος πίνακας εξωτερικών που ονομάζεται **αποθεμάτων** σε ομάδα, καθώς και συνδέσεις για να τα δεδομένα που έχουν αποσταλεί στο προηγούμενο βήμα.
* Εκτελεί ένα ερώτημα με τη χρήση Hive πάνω από τον πίνακα **αποθεμάτων** .

Επιπλέον, το δείγμα εκτελεί μια διαδικασία εκκαθάρισης πριν και μετά την εκτέλεση λειτουργιών κύρια. Κατά τη διάρκεια του εκκαθάρισης, καταργούνται όλα τα σχετιζόμενα δεδομένα Azure Blob και τους φακέλους και αποτεθεί στον πίνακα της ομάδας. Μπορείτε επίσης να ενεργοποιήσετε τη διαδικασία εκκαθάρισης από το δείγμα γραμμής εντολών.

Το δείγμα περιλαμβάνει τις ακόλουθες προϋποθέσεις:

* Μια ενεργή συνδρομή στο Microsoft Azure και το αναγνωριστικό συνδρομής.
* Ένα πιστοποιητικό διαχείρισης για τη συνδρομή με το αντίστοιχο ιδιωτικό κλειδί. Το πιστοποιητικό πρέπει να εγκατασταθούν με τα τρέχοντα χρήστη ιδιωτικό χώρο αποθήκευσης στον υπολογιστή που χρησιμοποιούνται για να εκτελέσετε το δείγμα.
* Μια ενεργή σύμπλεγμα HDInsight.
* Ένας λογαριασμός αποθήκευσης Azure συνδεθεί στο σύμπλεγμα HDInsight από την προηγούμενη προϋπόθεση, μαζί με το αντίστοιχο πρόσβαση πρωτεύον ή δευτερεύον κλειδί.

Όλες τις πληροφορίες από τις προϋποθέσεις πρέπει να εισάγονται στο αρχείο παραμέτρων δείγμα πριν από την εκτέλεση του δείγματος. Υπάρχουν δύο πιθανές τρόποι για να το κάνετε:

* Επεξεργαστείτε το αρχείο app.config στον ριζικό κατάλογο δείγμα και, στη συνέχεια, δημιουργήστε το δείγμα
* Δημιουργήστε πρώτα το δείγμα και, στη συνέχεια, επεξεργασία AvroHDISample.exe.config στον κατάλογο Δόμηση

Και στις δύο περιπτώσεις θα πρέπει να γίνουν όλες τις αλλαγές με το **<appSettings>** ενότητα ρυθμίσεις. Ακολουθήστε τα σχόλια του αρχείου.
Το δείγμα εκτελείται από τη γραμμή εντολών, εκτελώντας την ακόλουθη εντολή (διαφορετικά, όπου το αρχείο .zip με το δείγμα θεωρήθηκε ότι είναι δυνατή η εξαγωγή C:\AvroHDISample; Εάν χρησιμοποιήσετε τη διαδρομή του αρχείου σχετικές):

    AvroHDISample run C:\AvroHDISample\Data

Για να καθαρίσετε το σύμπλεγμα, εκτελέστε την ακόλουθη εντολή:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx

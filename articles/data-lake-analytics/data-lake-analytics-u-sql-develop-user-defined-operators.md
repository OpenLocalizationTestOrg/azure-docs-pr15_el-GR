<properties 
   pageTitle="Ανάπτυξη τελεστές που ορίζονται από το χρήστη U-SQL για εργασίες ανάλυσης λίμνης δεδομένων Azure | Azure" 
   description="Μάθετε πώς μπορείτε να αναπτυχθούν τελεστές ορίζονται από το χρήστη για να χρησιμοποιηθούν και εκ νέου χρήση τους σε έργα δεδομένων λίμνης ανάλυσης. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Ανάπτυξη τελεστές που ορίζονται από το χρήστη U-SQL για εργασίες ανάλυσης λίμνης δεδομένων Azure

Μάθετε πώς μπορείτε να αναπτυχθούν τελεστές ορίζονται από το χρήστη για να χρησιμοποιηθούν και εκ νέου χρήση τους σε έργα δεδομένων λίμνης ανάλυσης. Θα μπορείτε να αναπτύξετε μια προσαρμοσμένη τελεστή για να μετατρέψετε ονόματα χώρα.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Visual Studio 2015, το Visual Studio 2013 ενημέρωση 4 ή Visual Studio 2012 με εγκατεστημένο Visual C++ 
- Microsoft SDK Azure για το .NET έκδοση 2.5 ή νεότερη έκδοση.  Εγκατάσταση χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web.
- Ένας λογαριασμός ανάλυση λίμνης δεδομένων.  Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυση με Azure πύλη](data-lake-analytics-get-started-portal.md).
- Μεταβείτε στο πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης U SQL Studio](data-lake-analytics-u-sql-get-started.md) .
- Σύνδεση με Azure, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Αποστείλετε το αρχείο προέλευσης δεδομένων, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Ορισμός και χρήση του τελεστή ορίζονται από το χρήστη σε U-SQL

**Για να δημιουργήσετε και να υποβάλετε μια εργασία U-SQL** 

1. Από το μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.
2. Επιλέξτε τον τύπο του **Έργου U-SQL** .

    ![νέο έργο Visual Studio U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Κάντε κλικ στο **κουμπί OK**. Visual studio δημιουργεί μια λύση με ένα αρχείο Script.usql.
4. Από την **Εξερεύνηση λύσεων**, αναπτύξτε Script.usql και, στη συνέχεια, κάντε διπλό κλικ στο **Script.usql.cs**.
5. Επικολλήστε τον ακόλουθο κώδικα στο αρχείο:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Ανοίξτε το Script.usql και επικολλήστε την ακόλουθη δέσμη ενεργειών U-SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. Από την **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στην επιλογή **Script.usql**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία δέσμης ενεργειών**.
6. Από την **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στην επιλογή **Script.usql**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υποβολή δέσμης ενεργειών**.
7. Εάν δεν έχετε σύνδεση με τη συνδρομή σας στο Azure, θα μηνύματος για να εισαγάγετε τα διαπιστευτήριά σας λογαριασμός Azure.
7. Κάντε κλικ στην επιλογή **Υποβολή**. Αποτελέσματα υποβολής και σύνδεση εργασίας είναι διαθέσιμες στο παράθυρο αποτελεσμάτων κατά την υποβολή έχει ολοκληρωθεί.
8. Πρέπει να κάνετε κλικ στο κουμπί Ανανέωση για να δείτε την πιο πρόσφατη κατάσταση της εργασίας και ανανέωση της οθόνης.

**Για να δείτε το αποτέλεσμα του έργου**

1. Από την **Εξερεύνηση Server**, αναπτύξτε **Azure**, ανάπτυξη **Ανάλυσης δεδομένων λίμνης**, αναπτύξτε το λογαριασμό σας ανάλυσης δεδομένων λίμνης, αναπτύξτε **Τους λογαριασμούς χώρου αποθήκευσης**, κάντε δεξιό κλικ στο χώρο αποθήκευσης την προεπιλεγμένη και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξερεύνηση**. 
2. Αναπτύξτε το στοιχείο δείγματα, αναπτύξτε εξόδους και, στη συνέχεια, κάντε διπλό κλικ στο **όνομα Drivers.csv**.


##<a name="see-also"></a>Δείτε επίσης

- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με χρήση του PowerShell](data-lake-analytics-get-started-powershell.md)
- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με την πύλη του Azure](data-lake-analytics-get-started-portal.md)
- [Χρήση εργαλείων λίμνης δεδομένων για το Visual Studio για την ανάπτυξη εφαρμογών U-SQL](data-lake-analytics-data-lake-tools-get-started.md)

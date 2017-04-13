<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Azure Διαγνωστικά (.NET) με τις υπηρεσίες Cloud | Microsoft Azure"
    description="Χρησιμοποιώντας τα Διαγνωστικά του Azure για τη συγκέντρωση δεδομένων από τις υπηρεσίες Azure cloud για τον εντοπισμό σφαλμάτων, μέτρηση επιδόσεων, παρακολούθηση, ανάλυση της κίνησης και πολλά άλλα."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Ενεργοποίηση Azure Διαγνωστικά στις υπηρεσίες Azure Cloud

Ανατρέξτε στο θέμα [Επισκόπηση διαγνωστικών Azure](../azure-diagnostics.md) για ένα φόντο σε διαγνωστικά Azure.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Πώς μπορείτε να ενεργοποιήσετε Διαγνωστικά σε ένα ρόλο εργαζόμενου

Αυτό ματιά περιγράφει πώς μπορείτε να υλοποιήσετε ένα ρόλο Azure εργαζόμενου που εκπέμπει τηλεμετρίας δεδομένων χρησιμοποιώντας την κλάση Προέλευση_συμβάντος .NET. Διαγνωστικά του Azure χρησιμοποιείται για να συλλέξετε τα δεδομένα τηλεμετρίας και αποθηκεύστε το σε ένα λογαριασμό Azure χώρου αποθήκευσης. Όταν δημιουργείτε ένα ρόλο εργαζόμενου Visual Studio ενεργοποιεί αυτόματα την 1.0 Διαγνωστικά ως μέρος της λύσης στο SDK Azure για το .NET 2.4 και παλαιότερες εκδόσεις. Οι παρακάτω οδηγίες περιγράφουν τη διαδικασία για τη δημιουργία του ρόλου εργαζόμενου, απενεργοποίηση 1.0 Διαγνωστικά από τη λύση, και ανάπτυξη Διαγνωστικά 1.2 ή 1.3 στο ρόλο εργασίας.

### <a name="pre-requisites"></a>Προαπαιτούμενα
Σε αυτό το άρθρο προϋποθέτει ότι έχετε μια συνδρομή του Azure και χρησιμοποιούν το Visual Studio 2013 με το Azure SDK. Εάν δεν έχετε μια συνδρομή του Azure, μπορείτε να εγγραφείτε για το [Δωρεάν δοκιμαστικής έκδοσης][]. Βεβαιωθείτε ότι έχετε [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure έκδοση 0.8.7 ή νεότερη έκδοση][].

### <a name="step-1-create-a-worker-role"></a>Βήμα 1: Δημιουργήστε ένα ρόλο εργαζόμενου
1.  Εκκινήστε το **Visual Studio 2013**.
2.  Δημιουργία νέου έργου **Azure υπηρεσία Cloud** από το πρότυπο **Cloud** που προορισμών διαίρεσης 4,5 .NET Framework.  Ονομάστε το έργο "WadExample" και κάντε κλικ στο κουμπί Ok.
3.  Επιλέξτε **Το ρόλο εργασίας** και κάντε κλικ στο κουμπί Ok. Το project θα δημιουργηθεί.
4.  Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ στο αρχείο **WorkerRole1** ιδιότητες.
5.  **Ρύθμιση παραμέτρων** tab καταργήστε την επιλογή **Ενεργοποίηση Διαγνωστικά** για να απενεργοποιήσετε τα Διαγνωστικά 1.0 (Azure SDK 2.4 και eariler).
6.  Δημιουργήστε τη λύση σας για να βεβαιωθείτε ότι έχετε χωρίς σφάλματα.

### <a name="step-2-instrument-your-code"></a>Βήμα 2: Μέσου σας κώδικα
Αντικαταστήστε τα περιεχόμενα του WorkerRole.cs με τον ακόλουθο κώδικα. Τάξη SampleEventSourceWriter, έχουν μεταβιβαστεί από την [Κλάση Προέλευση_συμβάντος][], υλοποιεί τέσσερις μεθόδους καταγραφής: **SendEnums**, **MessageMethod**, **SetOther** και **HighFreq**. Η πρώτη παράμετρος στη μέθοδο **WriteEvent** Καθορίζει το Αναγνωριστικό για το αντίστοιχο συμβάν. Η μέθοδος εκτέλεση υλοποιεί κατάσταση συνεχούς επανάληψης που καλεί κάθε μία από τις μεθόδους καταγραφής υλοποιηθεί στην τάξη **SampleEventSourceWriter** κάθε 10 δευτερόλεπτα.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Βήμα 3: Ανάπτυξη ο ρόλος σας εργασίας
1.  Ανάπτυξη ο ρόλος σας εργαζόμενου Azure από μέσα σε Visual Studio, επιλέγοντας το έργο **WadExample** στο στην Εξερεύνηση λύσεων, στη συνέχεια **Δημοσίευση** από το μενού **Δημιουργία** .
2.  Επιλέξτε τη συνδρομή σας.
3.  Στο παράθυρο διαλόγου **Ρυθμίσεων δημοσίευση Microsoft Azure** επιλέξτε **Δημιουργία νέου...**.
4.  Στο παράθυρο διαλόγου **Δημιουργία υπηρεσία στο Cloud και το λογαριασμό χώρου αποθήκευσης** , εισαγάγετε ένα **όνομα** (για παράδειγμα, "WadExample") και επιλέξτε μια περιοχή ή ομάδα συσχέτισης.
5.  Ορίστε το **περιβάλλον** **ενδιάμεσου σταδίου**.
6.  Τροποποιήστε οποιεσδήποτε άλλες **Ρυθμίσεις** ανάλογα με την περίπτωση και κάντε κλικ στο κουμπί **Δημοσίευση**.
7.  Μετά την ολοκλήρωση της ανάπτυξης επαλήθευση στην πύλη του Azure κλασική ότι την υπηρεσία cloud είναι σε κατάσταση **εκτελείται** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Βήμα 4: Δημιουργήστε το αρχείο ρύθμισης παραμέτρων Διαγνωστικά και εγκαταστήστε την επέκταση
1.  Λήψη ο ορισμός σχήματος αρχείο ρύθμισης παραμέτρων δημόσια εκτελώντας την ακόλουθη εντολή PowerShell:
2.
        (Get-AzureServiceAvailableExtension - ExtensionName 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Εξερχόμενων αρχείων-κωδικοποίηση utf8 - FilePath 'WadConfig.xsd'

2.  Προσθήκη ενός αρχείου XML στο έργο σας **WorkerRole1** κάνοντας δεξί κλικ στο έργο **WorkerRole1** και επιλέξτε **Προσθήκη** -> **Νέο στοιχείο...**  ->  **Visual C# στοιχεία** -> **δεδομένων** -> **Αρχείο XML**. Ονομάστε το αρχείο "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Συσχέτιση του WadConfig.xsd με το αρχείο ρύθμισης παραμέτρων. Βεβαιωθείτε ότι το παράθυρο του προγράμματος επεξεργασίας WadExample.xml είναι το ενεργό παράθυρο. Πατήστε το πλήκτρο **F4** για να ανοίξετε το παράθυρο " **Ιδιότητες** ". Κάντε κλικ στην ιδιότητα **σχήματα** στο παράθυρο **διαλόγου Ιδιότητες** . Κάντε κλικ στην επιλογή **...** στην ιδιότητα **σχήματα** . Κάντε κλικ στην επιλογή της **αναφοράς...** κουμπί και μεταβείτε στη θέση όπου αποθηκεύσατε το αρχείο XSD και επιλέξτε το αρχείο WadConfig.xsd. Κάντε κλικ στο **κουμπί OK**.
4.  Αντικαταστήστε τα περιεχόμενα του αρχείου ρύθμισης παραμέτρων WadExample.xml με το παρακάτω δείγμα XML και αποθηκεύστε το αρχείο. Αυτό το αρχείο ρύθμισης παραμέτρων ορίζει μερικά μετρητές επιδόσεων για τη συλλογή: μία για χρήση της CPU και μία για τη χρήση μνήμης. Στη συνέχεια, τη ρύθμιση παραμέτρων καθορίζει τα τέσσερα συμβάντα που αντιστοιχεί στις μεθόδους στην τάξη SampleEventSourceWriter.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Βήμα 5: Εγκατάσταση Διαγνωστικά σε ο ρόλος σας εργασίας
Είναι τα cmdlet του PowerShell για τη Διαχείριση Διαγνωστικά σε ρόλο web ή εργαζόμενου: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension και κατάργηση AzureServiceDiagnosticsExtension.

1.  Ανοίξτε το Azure PowerShell.
2.  Εκτέλεση της δέσμης ενεργειών για να εγκαταστήσετε Διαγνωστικά σε ρόλο εργαζόμενου (αντικαταστήστε *StorageAccountKey* με το κλειδί λογαριασμού χώρου αποθήκευσης για το λογαριασμό χώρου αποθήκευσης wadexample):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Βήμα 6: Εξετάστε τα δεδομένα σας τηλεμετρίας
Στην **Εξερεύνηση Server** του Visual Studio, μεταβείτε στο λογαριασμό wadexample χώρου αποθήκευσης. Μετά από το cloud υπηρεσίας έχει εκτελεστεί περίπου 5 λεπτά, θα πρέπει να βλέπετε τους πίνακες **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** και **WADSetOtherTable**. Κάντε διπλό κλικ σε έναν από τους πίνακες για να προβάλετε το τηλεμετρίας που έχουν συλλεχθεί.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Σχήμα αρχείου ρύθμισης παραμέτρων

Το αρχείο ρύθμισης παραμέτρων Διαγνωστικά ορίζει τιμές που χρησιμοποιούνται για την προετοιμασία ρυθμίσεων παραμέτρων διαγνωστικών όταν ξεκινά τον παράγοντα Διαγνωστικά. Ανατρέξτε στην [πιο πρόσφατη αναφορά σχήματος](https://msdn.microsoft.com/library/azure/mt634524.aspx) για έγκυρες τιμές και παραδείγματα.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Εάν έχετε προβλήματα, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Διαγνωστικά Azure](../azure-diagnostics-troubleshooting.md) για βοήθεια σχετικά με συνηθισμένα προβλήματα.

## <a name="next-steps"></a>Επόμενα βήματα
[Δείτε μια λίστα με εικονική μηχανή σχετικά άρθρα Διαγνωστικά του Azure](azure-diagnostics.md#cloud-services) για να αλλάξετε τα δεδομένα που συλλέγετε, αντιμετώπιση προβλημάτων ή μάθετε περισσότερα σχετικά με τα Διαγνωστικά Γενικά.


[Προέλευση_συμβάντος τάξης]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Δωρεάν δοκιμαστική έκδοση]: http://azure.microsoft.com/pricing/free-trial/
[Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure έκδοση 0.8.7 ή νεότερη έκδοση]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Azure Διαγνωστικά σε εικονικές μηχανές Windows | Microsoft Azure"
    description="Χρησιμοποιώντας τα Διαγνωστικά του Azure για τη συγκέντρωση δεδομένων από εικονικές μηχανές Windows Azure για τον εντοπισμό σφαλμάτων, μέτρηση επιδόσεων, παρακολούθηση, ανάλυση της κίνησης και πολλά άλλα."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Ενεργοποίηση Διαγνωστικά σε εικονικές μηχανές Windows Azure

Ανατρέξτε στο θέμα [Επισκόπηση διαγνωστικών Azure](azure-diagnostics.md) για ένα φόντο σε διαγνωστικά Azure.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Πώς μπορείτε να ενεργοποιήσετε τα Διαγνωστικά στο μια εικονική μηχανή

Ματιά αυτό περιγράφει τον τρόπο εγκατάστασης από απόσταση Διαγνωστικά σε μια εικονική μηχανή Azure από έναν υπολογιστή ανάπτυξης. Μπορείτε επίσης να μάθετε πώς μπορείτε να υλοποιήσετε μια εφαρμογή που εκτελείται σε αυτόν τον υπολογιστή εικονικές Azure και εκπέμπει τηλεμετρίας δεδομένα χρησιμοποιώντας το.NET [Προέλευση_συμβάντος τάξης][]. Διαγνωστικά του Azure χρησιμοποιείται για τη συλλογή του τηλεμετρίας και αποθηκεύστε το σε ένα λογαριασμό Azure χώρου αποθήκευσης.

### <a name="pre-requisites"></a>Προαπαιτούμενα
Αυτό ματιά προϋποθέτει ότι έχετε μια συνδρομή του Azure και χρησιμοποιούν το Visual Studio 2013 με το Azure SDK. Εάν δεν έχετε μια συνδρομή του Azure, μπορείτε να εγγραφείτε για το [Δωρεάν δοκιμαστικής έκδοσης][]. Βεβαιωθείτε ότι έχετε [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure έκδοση 0.8.7 ή νεότερη έκδοση][].

### <a name="step-1-create-a-virtual-machine"></a>Βήμα 1: Δημιουργήστε μια εικονική μηχανή
1.  Στον υπολογιστή σας ανάπτυξης, ξεκινήστε Visual Studio 2013.
2.  Στην **Εξερεύνηση Server** του Visual Studio ανάπτυξη **Azure**, κάντε δεξί κλικ σε **εικονικές μηχανές** , στη συνέχεια, επιλέξτε **Δημιουργία εικονική μηχανή**.
3.  Επιλέξτε τη συνδρομή σας στο Azure στο παράθυρο διαλόγου **Επιλέξτε μια συνδρομή** και κάντε κλικ στο κουμπί **Επόμενο**.
4.  Επιλέξτε **Windows Server 2012 R2 κέντρο δεδομένων, Νοεμβρίου 2014** στο παράθυρο διαλόγου **Επιλέξτε μια εικονική μηχανή εικόνα** και κάντε κλικ στο κουμπί **Επόμενο**.
5.  Στις **Βασικές ρυθμίσεις εικονική μηχανή**, ορίστε το όνομα εικονική μηχανή να "wadexample". Ορίστε το όνομα χρήστη του διαχειριστή και τον κωδικό πρόσβασης και κάντε κλικ στο κουμπί **Επόμενο**.
6.  Στο παράθυρο διαλόγου **Ρυθμίσεις υπηρεσία Cloud** , δημιουργήστε μια νέα υπηρεσία στο cloud με το όνομα "wadexampleVM". Δημιουργία νέου λογαριασμού χώρου αποθήκευσης με το όνομα "wadexample" και κάντε κλικ στο κουμπί **Επόμενο**.
7.  Κάντε κλικ στην επιλογή **Δημιουργία**.

### <a name="step-2-create-your-application"></a>Βήμα 2: Δημιουργία εφαρμογής σας
1.  Στον υπολογιστή σας ανάπτυξης, ξεκινήστε Visual Studio 2013.
2.  Δημιουργία νέας Visual C# κονσόλα εφαρμογής που ως προορισμό διαίρεσης 4,5 .NET Framework. Ονομάστε το έργο "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Αντικαταστήστε τα περιεχόμενα του Program.cs με τον ακόλουθο κώδικα. Τάξη **SampleEventSourceWriter** υλοποιεί τέσσερις μεθόδους καταγραφής: **SendEnums**, **MessageMethod**, **SetOther** και **HighFreq**. Η πρώτη παράμετρος στη μέθοδο WriteEvent Καθορίζει το Αναγνωριστικό για το αντίστοιχο συμβάν. Η μέθοδος εκτέλεση υλοποιεί κατάσταση συνεχούς επανάληψης που καλεί κάθε μία από τις μεθόδους καταγραφής υλοποιηθεί στην τάξη **SampleEventSourceWriter** κάθε 10 δευτερόλεπτα.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Αποθηκεύστε το αρχείο και επιλέξτε **Δημιουργία λύσης** από το μενού **Δημιουργία** για να δημιουργήσετε τον κωδικό.


### <a name="step-3-deploy-your-application"></a>Βήμα 3: Ανάπτυξη εφαρμογών σας
1.  Κάντε δεξί κλικ στο έργο **WadExampleVM** στην **Εξερεύνηση λύσεων** και επιλέξτε **Άνοιγμα φακέλου στην Εξερεύνηση αρχείων**.
2.  Μεταβείτε στο φάκελο *bin\Debug* και αντιγράψτε όλα τα αρχεία (WadExampleVM.*)
3.  Στην **Εξερεύνηση Server** κάντε δεξί κλικ στον υπολογιστή εικονικές και επιλέξτε **σύνδεση με χρήση απομακρυσμένης επιφάνειας εργασίας**.
4.  Αφού συνδεδεμένο με το Εικονική δημιουργήσετε ένα φάκελο που ονομάζεται WadExampleVM και να επικολλήσετε την εφαρμογή σας αρχεία στο φάκελο.
5.  Ξεκινήστε την εφαρμογή WadExampleVM.exe. Θα πρέπει να βλέπετε ένα παράθυρο κονσόλας κενό.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Βήμα 4: Δημιουργία της ρύθμισης παραμέτρων Διαγνωστικά και εγκαταστήστε την επέκταση
1.  Λήψη ο ορισμός σχήματος αρχείο ρύθμισης παραμέτρων δημόσια στον υπολογιστή σας ανάπτυξης, εκτελώντας την παρακάτω εντολή PowerShell:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Ανοίξτε ένα νέο αρχείο XML στο Visual Studio, είτε σε ένα έργο που ήδη έχετε ανοίξει ή στο Visual Studio μιας παρουσίας με ανοιχτά έργα. Στο Visual Studio, επιλέξτε **Προσθήκη** -> **Νέο στοιχείο...**  ->  **Visual C# στοιχεία** -> **δεδομένων** -> **Αρχείο XML**. Ονομάστε το αρχείο "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Βήμα 5: Απομακρυσμένη εγκατάσταση Διαγνωστικά στην εικονική μηχανή του Azure
Είναι τα cmdlet του PowerShell για τη Διαχείριση Διαγνωστικά σε μια Εικονική: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension και κατάργηση AzureVMDiagnosticsExtension.

1.  Στον υπολογιστή σας για προγραμματιστές, ανοίξτε Azure PowerShell.
2.  Εκτέλεση της δέσμης ενεργειών απομακρυσμένης εγκατάστασης Διαγνωστικά σε σας Εικονική (αντικατάσταση *StorageAccountKey* με το κλειδί λογαριασμού χώρου αποθήκευσης για το λογαριασμό χώρου αποθήκευσης wadexamplevm):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Βήμα 6: Εξετάστε τα δεδομένα σας τηλεμετρίας
Στην **Εξερεύνηση Server** του Visual Studio, μεταβείτε στο λογαριασμό wadexample χώρου αποθήκευσης. Μετά την εικονική Μηχανή έχει εκτελεστεί περίπου 5 λεπτά θα πρέπει να βλέπετε τους πίνακες **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** και **WADSetOtherTable**. Κάντε διπλό κλικ σε έναν από τους πίνακες για να προβάλετε το τηλεμετρίας που έχουν συλλεχθεί.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Σχήμα αρχείου ρύθμισης παραμέτρων

Το αρχείο ρύθμισης παραμέτρων Διαγνωστικά ορίζει τιμές που χρησιμοποιούνται για την προετοιμασία ρυθμίσεων παραμέτρων διαγνωστικών όταν ξεκινά τον παράγοντα Διαγνωστικά. Ανατρέξτε στην [πιο πρόσφατη αναφορά σχήματος](https://msdn.microsoft.com/library/azure/mt634524.aspx) για έγκυρες τιμές και παραδείγματα.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Διαγνωστικά Azure](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Επόμενα βήματα
[Δείτε μια λίστα με εικονική μηχανή σχετικά άρθρα Διαγνωστικά του Azure](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) για να αλλάξετε τα δεδομένα που συλλέγετε, αντιμετώπιση προβλημάτων ή μάθετε περισσότερα σχετικά με τα Διαγνωστικά Γενικά.


[Προέλευση_συμβάντος τάξης]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Δωρεάν δοκιμαστική έκδοση]: http://azure.microsoft.com/pricing/free-trial/
[Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure έκδοση 0.8.7 ή νεότερη έκδοση]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

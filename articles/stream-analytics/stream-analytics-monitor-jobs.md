<properties
    pageTitle="μέσω προγραμματισμού παρακολουθείτε τις εργασίες στη ροή ανάλυση | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να παρακολουθείτε μέσω προγραμματισμού εργασιών ροής ανάλυση δημιουργήσει μέσω API ΥΠΌΛΟΙΠΑ, Azure SDK, ή Powershell."
    keywords="οθόνη .net, οθόνη εργασία, παρακολούθηση εφαρμογής"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Να δημιουργήσετε μέσω προγραμματισμού μια οθόνη ανάλυσης ροής εργασίας
 Σε αυτό το άρθρο παρουσιάζει τον τρόπο για να ενεργοποιήσετε την παρακολούθηση για μια εργασία ροής ανάλυσης. Δεν ανάλυση ροής εργασιών που δημιουργήθηκε μέσω API ΥΠΌΛΟΙΠΑ, Azure SDK, ή Powershell κάντε εποπτεία ενεργοποιημένη από προεπιλογή.  Μπορείτε να με μη αυτόματο τρόπο ενεργοποιήσετε αυτή την πύλη Azure, μεταβαίνοντας στη σελίδα παρακολούθηση του έργου και κάνοντας κλικ στο κουμπί Ενεργοποίηση ή μπορείτε να αυτοματοποιήσετε αυτήν τη διαδικασία, ακολουθώντας τα βήματα σε αυτό το άρθρο. Τα δεδομένα παρακολούθησης θα εμφανίζεται στην καρτέλα "Οθόνη" στην πύλη του Azure για την ανάλυση της ροής εργασίας.

![Παρακολούθηση εργασίας καρτέλα εργασίες](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- Visual Studio 2012 ή 2013.
- Λήψη και εγκατάσταση του [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Μια υπάρχουσα εργασία ροής ανάλυσης που χρειάζεται η παρακολούθηση με δυνατότητα.

## <a name="setup-a-project"></a>Ρύθμιση ενός έργου

1.  Δημιουργία μιας εφαρμογής κονσόλας Visual Studio C# .net.
2.  Στην κονσόλα διαχείρισης πακέτου, εκτελέστε τις ακόλουθες εντολές για να εγκαταστήσετε τα πακέτα NuGet. Το πρώτο είναι το .NET SDK Azure ροή ανάλυση διαχείρισης. Στο δεύτερο παράδειγμα είναι το SDK οθόνη Azure που θα χρησιμοποιηθεί για να ενεργοποιήσετε την παρακολούθηση. Τελευταία αυτό είναι το πρόγραμμα-πελάτη Azure Active Directory που θα χρησιμοποιηθεί για τον έλεγχο ταυτότητας.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Προσθέστε την παρακάτω ενότητα appSettings στο αρχείο App.config.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Αντικατάσταση τιμών για *SubscriptionId* και *ActiveDirectoryTenantId* με τη συνδρομή σας Azure και μισθωτή αναγνωριστικά. Μπορείτε να λάβετε αυτές τις τιμές, εκτελώντας το ακόλουθο cmdlet του PowerShell:

    ```
    Get-AzureAccount
    ```
4.  Προσθέστε τα ακόλουθα χρήση προτάσεων στο αρχείο προέλευσης (Program.cs) στο έργο.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Προσθέστε μια μέθοδο βοηθητική εφαρμογή του ελέγχου ταυτότητας.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Δημιουργία διαχείρισης πελατών
Ο ακόλουθος κώδικας θα ρυθμιστεί τις μεταβλητές είναι απαραίτητο και τα προγράμματα-πελάτες διαχείρισης.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Ενεργοποίηση παρακολούθησης για μια υπάρχουσα εργασία ανάλυσης ροής

Ο ακόλουθος κώδικας θα σας επιτρέψει παρακολούθησης για μια **υπάρχουσα** εργασία ροής ανάλυσης. Το πρώτο μέρος του κώδικα εκτελεί ένα αίτημα GET σε σχέση με την υπηρεσία ανάλυσης ροής για να ανακτήσετε πληροφορίες σχετικά με τη συγκεκριμένη εργασία ροής ανάλυσης. Χρησιμοποιεί την ιδιότητα "Αναγνωριστικό" (ανακτήθηκαν από την αίτηση GET) ως παράμετρο, για τη μέθοδο τοποθέτηση στο δεύτερο μισό του κώδικα που στέλνει μια ΤΟΠΟΘΈΤΗΣΗ αίτηση για την υπηρεσία ιδέες για να ενεργοποιήσετε την παρακολούθηση για την εργασία ροής ανάλυσης.

> [AZURE.WARNING]
> Εάν έχετε ενεργοποιήσει προηγουμένως παρακολούθησης για μια διαφορετική ανάλυση ροή του έργου, είτε μέσω της πύλης Azure ή μέσω προγραμματισμού μέσω του παρακάτω κώδικα, **συνιστάται που παρέχετε το ίδιο όνομα λογαριασμού χώρου αποθήκευσης που κάνατε όταν έχετε ενεργοποιήσει προηγουμένως παρακολούθησης.**
>
> Το λογαριασμό χώρου αποθήκευσης είναι συνδεδεμένο στην περιοχή που δημιουργήσατε εργασίας ροής ανάλυσης στο, όχι συγκεκριμένα για την εργασία στο ίδιο.
>
> Όλα τα αναλυτικά στοιχεία ροής εργασίας (και όλα τα άλλα μέσα Azure) σε αυτήν την ίδια περιοχή κοινή χρήση αυτού του λογαριασμού χώρου αποθήκευσης για την αποθήκευση δεδομένων παρακολούθησης. Εάν παρέχεται ένα λογαριασμό διαφορετικό χώρο αποθήκευσης, αυτό μπορεί να προκαλέσει ανεπιθύμητα επιπτώσεις για την παρακολούθηση των σας άλλες εργασίες ροής ανάλυση ή/και άλλοι πόροι Azure.
>
> Το όνομα λογαριασμού χώρου αποθήκευσης που χρησιμοποιείται για να αντικαταστήσετε ```“<YOUR STORAGE ACCOUNT NAME>”``` παρακάτω πρέπει να ενεργοποιήσετε το παρακολούθησης για ένα λογαριασμό χώρου αποθήκευσης που είναι στην ίδια συνδρομή, όπως τα αναλυτικά στοιχεία ροής εργασίας που.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Λήψη υποστήριξης
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ Azure ροή ανάλυσης](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<properties 
    pageTitle="Χρήση του .NET SDK για πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure" 
    description="Περιγράφει πώς μπορείτε να χρησιμοποιήσετε το κινητό δέσμευση .NET SDK για να αποκτήσετε πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Χρήση του .NET SDK για πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure

Azure δέσμευση Mobile εκθέτει ένα σύνολο API για να διαχειριστείτε συσκευές, Reach/Push εκστρατείες κ.λπ. Για να αλληλεπιδράσουν με αυτά τα API, επίσης, παρέχουμε που [Swagger αρχείου](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) που μπορείτε να χρησιμοποιήσετε με τα εργαλεία για τη δημιουργία SDK για τη γλώσσα που προτιμάτε. Συνιστάται να χρησιμοποιείτε το εργαλείο [AutoRest](https://github.com/Azure/AutoRest) για τη δημιουργία του SDK από το αρχείο Swagger. 

Έχουμε δημιουργήσει ένα .NET SDK με παρόμοιο τρόπο που σας επιτρέπει να αλληλεπιδράσουν με αυτά τα API χρησιμοποιώντας μια περιτυλίγματος C# και δεν χρειάζεται να κάνετε το διακριτικό διαπραγμάτευσης ελέγχου ταυτότητας και ανανέωση στον εαυτό σας.  

Αυτό το δείγμα, έχετε το σύνολο βημάτων για να ακολουθήσετε για να χρησιμοποιήσετε το .NET SDK:

1. Πρώτον, πρέπει να ρυθμίσετε τον έλεγχο ταυτότητας για το API χρησιμοποιώντας το Azure Active Directory ως περιγράφεται [εδώ](mobile-engagement-api-authentication.md#authentication). Στο τέλος της αυτά τα βήματα, θα πρέπει να έχετε ένα έγκυρο **SubscriptionId**, **TenantId**, **αναγνωριστικά εφαρμογής** και **μυστικό**. 

2. Θα χρησιμοποιήσουμε μια απλή εφαρμογή κονσόλας Windows για μια επίδειξη με την εργασία με το .NET SDK με το σενάριο τη δημιουργία μιας εκστρατείας ανακοίνωση. Επομένως, άνοιγμα του Visual Studio και δημιουργία μιας **Εφαρμογής κονσόλας**.   

3. Στη συνέχεια πρέπει να κάνετε λήψη του SDK .NET, το οποίο είναι διαθέσιμο ως **Βιβλιοθήκης διαχείρισης του Microsoft Azure δέσμευση** στη συλλογή Nuget [εδώ](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Εάν εγκαθιστάτε το Nuget από το Visual Studio, πρέπει να βεβαιωθείτε ότι έχετε ελέγχου έχει επισημανθεί την επιλογή **Συμπερίληψη προέκδοση** κατά την αναζήτηση για το πακέτο:

    ![][1]

4. Στο το `Program.cs` αρχείων, προσθέστε τα παρακάτω πεδία ονομάτων:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Στη συνέχεια πρέπει να ορίσετε τις ακόλουθες σταθερές που θα χρησιμοποιήσουμε για τον έλεγχο ταυτότητας και αλληλεπίδραση με την εφαρμογή δέσμευση Mobile με την οποία δημιουργείτε την ανακοίνωση εκστρατεία:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Ορισμός του `EngagementManagementClient` μεταβλητή που θα χρησιμοποιήσουμε για να καλέσετε τις μεθόδους SDK δέσμευση Mobile:

        static EngagementManagementClient engagementClient; 

7. Προσθέστε τα εξής για να σας `Main` μέθοδο:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Ορισμός την ακόλουθη μέθοδο που αναλαμβάνει την προετοιμασία του `EngagementManagementClient` , πρώτα τον έλεγχο ταυτότητας και, στη συνέχεια, συσχετίζοντας ίδια με την εφαρμογή δέσμευση Mobile στην οποία σκοπεύετε να δημιουργήσετε την ανακοίνωση εκστρατεία:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Σημειώστε ότι πρέπει να χρησιμοποιήσετε την **Εφαρμογή όνομα πόρου** που ορίζεται στην πύλη του Azure διαχείρισης για την παράμετρο όνομα_εφαρμογής. 

9. Τέλος, να ορίσετε τη μέθοδο CreateCampaign που θα αναλάβουν την χρησιμοποιώντας το προηγουμένως προετοιμασία EngagementClient για να δημιουργήσετε ένα απλό **οποιαδήποτε στιγμή** & **μόνο για ειδοποίηση** εκστρατείας με τίτλο και το μήνυμα: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Εκτελέστε την εφαρμογή κονσόλας και θα δείτε τα εξής στην επιτυχή δημιουργία της εκστρατείας:

    **Αναγνωριστικό εκστρατείας '159' δημιουργήθηκε με επιτυχία και βρίσκεται σε κατάσταση 'Πρόχειρο'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png

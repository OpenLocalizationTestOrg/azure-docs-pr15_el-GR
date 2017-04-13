<properties 
    pageTitle="Χρησιμοποιώντας το REST API για να αποκτήσετε πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure" 
    description="Περιγράφει τον τρόπο χρήσης τα Mobile δέσμευση REST API για να αποκτήσετε πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure"       
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="wesmc;ricksal" />

#<a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Χρήση ΥΠΌΛΟΙΠΟ για πρόσβαση APIs υπηρεσίας δέσμευση Mobile Azure

Azure δέσμευση Mobile παρέχει το [Azure Mobile δέσμευση REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) για να διαχειριστείτε συσκευές, Reach/Push εκστρατείες κ.λπ. Αυτό το δείγμα θα χρησιμοποιήσει τα REST API απευθείας για να δημιουργήσετε μια ανακοίνωση εκστρατεία, στη συνέχεια, να ενεργοποιήσετε και να προωθήσετε την σε ένα σύνολο των συσκευών. 

Εάν δεν θέλετε να χρησιμοποιήσετε τα ΥΠΌΛΟΙΠΑ API απευθείας, επίσης, παρέχουμε μια [Swagger αρχείου](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) που μπορείτε να χρησιμοποιήσετε με τα εργαλεία για τη δημιουργία SDK για τη γλώσσα που προτιμάτε. Συνιστάται να χρησιμοποιείτε το εργαλείο [AutoRest](https://github.com/Azure/AutoRest) για τη δημιουργία του SDK από το αρχείο Swagger. Έχουμε δημιουργήσει ένα .NET SDK με παρόμοιο τρόπο που σας επιτρέπει να αλληλεπιδράσουν με αυτά τα API χρησιμοποιώντας μια περιτυλίγματος C# και δεν χρειάζεται να κάνετε το διακριτικό διαπραγμάτευσης ελέγχου ταυτότητας και ανανέωση στον εαυτό σας. Εάν θέλετε να ακολουθήσετε ένα δείγμα παρόμοιο με αυτό περιτυλίγματος, ανατρέξτε στο θέμα [Υπηρεσία API .NET SDK δείγματος](mobile-engagement-dotnet-sdk-service-api.md)

Αυτό το δείγμα θα χρησιμοποιήσει τα REST API απευθείας για να δημιουργήσετε και να ενεργοποιήσετε μια ανακοίνωση εκστρατείας. 

## <a name="adding-a-username-appinfo-to-the-mobile-engagement-app"></a>Προσθήκη μιας appInfo όνομα_χρήστη στην εφαρμογή Mobile δέσμευση

Αυτό το δείγμα θα απαιτούν να προστίθεται η εφαρμογή Mobile δέσμευση μια ετικέτα πληροφορίες app. Στην πύλη του δέσμευση για την εφαρμογή σας, μπορείτε να προσθέσετε την ετικέτα, κάνοντας κλικ στην επιλογή **Ρυθμίσεις** > **ετικέτα (πληροφορίες app)** > **νέα ετικέτα (πληροφορίες app)**. Μπορείτε να προσθέσετε τη νέα ετικέτα με το όνομα **όνομα_χρήστη** ως τύπου **συμβολοσειράς** .

![](./media/mobile-engagement-dotnet-rest-service-api/user-name-app-info.png)



Στην περίπτωση που ακολουθήσατε [Γρήγορα αποτελέσματα με το Azure δέσμευση για Windows καθολικής εφαρμογές του Mobile](mobile-engagement-windows-store-dotnet-get-started.md), μπορείτε να ελέγξετε την αποστολή της ετικέτας **όνομα_χρήστη** , απλώς παρακάμπτοντας `OnNavigatedTo()` στο σας `MainPage` τάξης για να στείλετε την ετικέτα πληροφορίες app παρόμοιο με τον ακόλουθο κώδικα:

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        String name = "Wesley"; // Prompt the user to provide this in your client app.

        Dictionary<object, object> info = new Dictionary<object, object>();
        info.Add("user_name", name);
        EngagementAgent.Instance.SendAppInfo(info);
    }
 


## <a name="creating-the-service-api-app"></a>Δημιουργία της εφαρμογής υπηρεσίας API

1. Πρώτον, θα χρειαστείτε τέσσερις παραμέτρους ελέγχου ταυτότητας για χρήση με αυτό το δείγμα. Αυτές οι παράμετροι είναι **SubscriptionId**, **TenantId**, **αναγνωριστικά εφαρμογής** και **μυστικό**. Για να λάβετε αυτές τις παραμέτρους του ελέγχου ταυτότητας, συνιστάται να χρησιμοποιήσετε την προσέγγιση δέσμης ενεργειών του PowerShell που αναφέρονται στην ενότητα *μοναδική εγκατάσταση (με χρήση δέσμης ενεργειών)* στην εκμάθηση [ελέγχου ταυτότητας](mobile-engagement-api-authentication.md#authentication) . 

2. Θα χρησιμοποιήσουμε μια απλή εφαρμογή κονσόλας Windows για μια επίδειξη με την εργασία με τα ΥΠΌΛΟΙΠΑ API υπηρεσίας για να δημιουργήσετε και να ενεργοποιήσετε μια νέα εκστρατεία ανακοίνωση. Επομένως, ανοίξτε του Visual Studio και δημιουργήστε μια νέα **Εφαρμογή κονσόλας**.   

3. Στη συνέχεια, προσθέστε το πακέτο NuGet **Newtonsoft.Json** στο έργο σας.

4. Στο το `Program.cs` αρχείων, προσθέστε τα ακόλουθα `using` προτάσεις για τα παρακάτω πεδία ονομάτων:

        using System.IO;
        using System.Net;
        using Newtonsoft.Json.Linq;
        using Newtonsoft.Json;

5. Στη συνέχεια θα πρέπει να ορίσετε τις ακόλουθες σταθερές στο το `Program` τάξης. Αυτά θα χρησιμοποιείται για τον έλεγχο ταυτότητας και αντιδρώντων με την εφαρμογή δέσμευση Mobile με την οποία δημιουργείτε την ανακοίνωση εκστρατεία:


        // Parameters needed for authentication of API calls.
        // These are returned from the PowerShell script in the authentication tutorial. 
        // https://azure.microsoft.com/documentation/articles/mobile-engagement-api-authentication/
        static String SubscriptionId = "<Your Subscription Id>";
        static String TenantId = "<Your TenantId>";
        static String ApplicationId = "<Your Application Id>";
        static String Secret = "<Your Secret>";

        // The token for authenticating with your Mobile Engagement app.
        static String Token = null;

        // This is the Azure Resource group concept for grouping together resources 
        // See: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        static String ResourceGroup = "MobileEngagement";

        // For Mobile Engagement operations
        // App Collection Name from the Azure portal 
        static String Collection = "<Your App Collection Name>";

        // Application Resource Name - make sure you are using the one as specified in the dashboard of the
        // Azure portal. (This is NOT the App Name)
        static String AppName = "WesmcRestTest-windows";

        // New campaign id returned from creating the new campaign and used to activate and push the campaign
        // a set of devices.
        static String NewCampaignID = null;

        // This list will hold the device Ids we choose to push the campaign to.
        static List<String> deviceList = new List<String>();


6. Προσθέστε τις ακόλουθες δύο μεθόδους για να το `Program` τάξης. Αυτά θα χρησιμοποιηθεί για εκτέλεση ΥΠΌΛΟΙΠΑ APIs και να εμφανίσετε τις απαντήσεις στην κονσόλα.

        private static async Task<HttpWebResponse> ExecuteREST(string httpMethod, string uri, string authToken, WebHeaderCollection headers = null, string body = null, string contentType = "application/json")
        {
            //=== Execute the request 
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(uri);
            HttpWebResponse response = null;

            request.Method = httpMethod;
            request.ContentType = contentType;
            request.ContentLength = 0;

            if (authToken != null)
                request.Headers.Add("Authorization", authToken);

            if (headers != null)
            {
                request.Headers.Add(headers);
            }

            if (body != null)
            {
                byte[] bytes = Encoding.UTF8.GetBytes(body);

                try
                {
                    request.ContentLength = bytes.Length;
                    Stream requestStream = request.GetRequestStream();
                    requestStream.Write(bytes, 0, bytes.Length);
                    requestStream.Close();
                }
                catch (Exception e)
                {
                    Console.WriteLine(e.Message);
                }
            }

            try
            {
                response = (HttpWebResponse)await request.GetResponseAsync();
            }
            catch (WebException we)
            {
                if (we.Response != null)
                {
                    response = (HttpWebResponse)we.Response;
                }
                else
                    Console.WriteLine(we.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

            return response;
        }


        private static void DisplayResponse(dynamic data, HttpWebResponse response)
        {
            Console.WriteLine("HTTP Status " + (int)response.StatusCode + " : " + response.StatusDescription);
            Console.WriteLine(data + "\n");
        }

    }

7. Προσθέστε τον ακόλουθο κώδικα για να σας `Main` μέθοδος για τη δημιουργία ενός διακριτικού ελέγχου ταυτότητας με τις παραμέτρους του ελέγχου ταυτότητας που συλλέγονται:

        //***************************************************************************
        //*** Get a valid authorization token with your authentication parameters ***
        //***************************************************************************

        String methodUrl = "https://login.microsoftonline.com/" + TenantId + "/oauth2/token";
        String requestBody = "grant_type=client_credentials&client_id=" + ApplicationId +
                            "&client_secret=" + Secret +
                            "&resource=https://management.core.windows.net/";
        Console.WriteLine("Getting authorization token...\n");
        HttpWebResponse response = ExecuteREST("POST", methodUrl, null, null, requestBody, null).Result;
        Stream receiveStream = response.GetResponseStream();
        StreamReader readStream = new StreamReader(receiveStream, Encoding.UTF8);
        dynamic data = JObject.Parse(readStream.ReadToEnd());
        readStream.Close();
        receiveStream.Close();
        DisplayResponse(data, response);

        if (response.StatusCode == HttpStatusCode.OK)
        {
            Token = data.token_type + " " + data.access_token;
            Console.WriteLine("Got authorization token...");
            Console.WriteLine("Authorization : " + Token + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to get authorization token. Check your parameters for API calls.\n");
            return;
        }

8. Τώρα που έχουμε μια έγκυρη ελέγχου ταυτότητας διακριτικού μπορούμε να δημιουργήσουμε μια νέα εκστρατεία Reach χρησιμοποιώντας τη [Δημιουργία εκστρατείας](https://msdn.microsoft.com/library/azure/mt683742.aspx) REST API. Η νέα εκστρατεία θα είναι μια απλή **οποιαδήποτε στιγμή** & **μόνο για ειδοποίηση** εκστρατείας ανακοίνωση με τίτλο και το μήνυμα. Αυτό θα είναι μια μη αυτόματη push εκστρατεία, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης. Αυτό σημαίνει ότι το θα μόνο είναι δυνατή η προώθηση χρησιμοποιώντας τα API.


    ![](./media/mobile-engagement-dotnet-rest-service-api/manual-push.png)

    Προσθέστε τον ακόλουθο κώδικα για να σας `Main` μέθοδο για τη δημιουργία της εκστρατείας ανακοίνωση: 

        //*****************************************************************************
        //*** Create a campaign to send a notification using the user-name app-info ***
        //*****************************************************************************

        String newCampaignMethodUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
               "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
               Collection + "/apps/" + AppName + "/campaigns/announcements?api-version=2014-12-01";

        String campaignRequestBody = "{ \"name\": \"BirthdayCoupon\", " +
                                        "\"type\": \"only_notif\", " +
                                        "\"deliveryTime\": \"any\", " +
                                        "\"notificationType\": \"popup\", " +
                                        "\"pushMode\":\"manual\", " +
                                        "\"notificationTitle\": \"Happy Birthday ${user_name}\", " +
                                        "\"notificationMessage\": \"Take extra 10% off on your orders today!\"}";

        Console.WriteLine("Creating new campaign...\n");
        HttpWebResponse newCampaignResponse = ExecuteREST("POST", newCampaignMethodUrl, Token, null, campaignRequestBody).Result;
        Stream receiveCampaignStream = newCampaignResponse.GetResponseStream();
        StreamReader readCampaignStream = new StreamReader(receiveCampaignStream, Encoding.UTF8);
        dynamic newCampaignData = JObject.Parse(readCampaignStream.ReadToEnd());
        readCampaignStream.Close();
        receiveCampaignStream.Close();
        DisplayResponse(newCampaignData, newCampaignResponse);

        if (newCampaignResponse.StatusCode == HttpStatusCode.Created)
        {
            NewCampaignID = newCampaignData.id;
            Console.WriteLine("Created new campaign...");
            Console.WriteLine("New Campaign Id    : " + NewCampaignID + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to create birthday campaign.\n");
            return;
        }


9. Της εκστρατείας πρέπει να ενεργοποιηθεί πριν να προωθηθεί όλες τις συσκευές. Θα σας αποθηκεύονται το Αναγνωριστικό για τη νέα εκστρατεία στο το `NewCampaignID` μεταβλητή. Θα χρησιμοποιήσουμε αυτό ως παράμετρο διαδρομή URI για την ενεργοποίηση της εκστρατείας χρησιμοποιώντας το REST API [Ενεργοποίηση εκστρατείας](https://msdn.microsoft.com/library/azure/mt683745.aspx) . Αυτό πρέπει να αλλάξετε την κατάσταση της εκστρατείας σε **προγραμματισμένη** , ακόμα και αν αυτό θα μόνο είναι δυνατή η προώθηση με μη αυτόματο τρόπο με τα API.

    Προσθέστε τον ακόλουθο κώδικα για να σας `Main` μέθοδο για να ενεργοποιήσετε την ανακοίνωση εκστρατεία: 

        //******************************************
        //*** Activate the new birthday campaign ***
        //******************************************

        String activateCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID +
                   "/activate?api-version=2014-12-01";

        Console.WriteLine("Activating new campaign (ID : " + NewCampaignID + ")...\n");
        HttpWebResponse activateCampaignResponse = ExecuteREST("POST", activateCampaignUrl, Token).Result;
        Stream activateCampaignStream = activateCampaignResponse.GetResponseStream();
        StreamReader activateCampaignReader = new StreamReader(activateCampaignStream, Encoding.UTF8);
        dynamic activateCampaignData = JObject.Parse(activateCampaignReader.ReadToEnd());
        activateCampaignReader.Close();
        activateCampaignStream.Close();
        DisplayResponse(activateCampaignData, activateCampaignResponse);

        if (activateCampaignResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Activated new campaign...");
            Console.WriteLine("New Campaign State : " + activateCampaignData.state + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to activate birthday campaign.\n");
            return;
        }

10. Για να προωθήσετε την εκστρατεία πρέπει να παρέχουμε τη συσκευή ταυτότητες για τους χρήστες που θέλετε να λαμβάνετε ειδοποίηση. Θα χρησιμοποιήσουμε το REST API [συσκευές ερωτήματος](https://msdn.microsoft.com/library/azure/mt683826.aspx) για να λάβετε όλα τα αναγνωριστικά συσκευή. Θα προσθέσουμε κάθε συσκευή αναγνωριστικό στη λίστα εάν έχει συνδεθεί με appInfo **όνομα_χρήστη** .

    Προσθέστε τον ακόλουθο κώδικα για να σας `Main` μέθοδο για τη λήψη όλων των αναγνωριστικών συσκευή και συμπληρώσετε το deviceList:

        //************************************************************************
        //*** Now that the manualPush campaign is activated, get the deviceIds ***
        //*** that you want to trigger the push campaign on.                   ***
        //************************************************************************

        String getDevicesUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/devices?api-version=2014-12-01";

        Console.WriteLine("Getting device IDs...\n");
        HttpWebResponse devicesResponse = ExecuteREST("GET", getDevicesUrl, Token).Result;
        Stream devicesStream = devicesResponse.GetResponseStream();
        StreamReader devicesReader = new StreamReader(devicesStream, Encoding.UTF8);
        dynamic devicesData = JObject.Parse(devicesReader.ReadToEnd());
        devicesReader.Close();
        devicesStream.Close();
        DisplayResponse(devicesData, devicesResponse);

        if (devicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Got devices.");
            foreach (dynamic device in devicesData.value)
            {
                // Just adding all in this example
                if (device.appInfo.user_name != null)
                {
                    Console.WriteLine("Adding device for push campaign...");
                    Console.WriteLine("Device ID    : " + device.deviceId);
                    Console.WriteLine("user_name    : " + device.appInfo.user_name);
                    deviceList.Add((String)device.deviceId);
                }
            }
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine("*** Failed to get devices.\n");
            return;
        }


11. Τέλος, θα σας θα push της εκστρατείας σε όλα τα αναγνωριστικά συσκευή στη λίστα μας χρησιμοποιώντας το REST API [Push εκστρατείας](https://msdn.microsoft.com/library/azure/mt683734.aspx) . Αυτή είναι μια ειδοποίηση **της εφαρμογής** . Επομένως, η εφαρμογή θα πρέπει να να εκτελείται στη συσκευή προκειμένου να λαμβάνονται από το χρήστη.

    Προσθέστε τον ακόλουθο κώδικα για να σας `Main` μέθοδο για να προωθήσετε το campign σε συσκευές στο το deviceList:

        //**************************************************************
        //*** Trigger the manualPush campaign on the desired devices ***
        //**************************************************************

        String pushCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID + 
                   "/push?api-version=2014-12-01";

        Console.WriteLine("Triggering push for new campaign (ID : " + NewCampaignID + ")...\n");
        String deviceIds = "{\"deviceIds\":" + JsonConvert.SerializeObject(deviceList) + "}";
        Console.WriteLine("\n" + deviceIds + "\n");
        HttpWebResponse pushDevicesResponse = ExecuteREST("POST", pushCampaignUrl, Token, null, deviceIds).Result;
        Stream pushDevicesStream = pushDevicesResponse.GetResponseStream();
        StreamReader pushDevicesReader = new StreamReader(pushDevicesStream, Encoding.UTF8);
        dynamic pushDevicesData = JObject.Parse(pushDevicesReader.ReadToEnd());
        pushDevicesReader.Close();
        pushDevicesStream.Close();
        DisplayResponse(pushDevicesData, pushDevicesResponse);

        if (pushDevicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Triggered push on new campaign.\n");
        }
        else
        {
            Console.WriteLine("*** Failed to push campaign to the device list.\n");
        }


12. Δημιουργία και εκτέλεση της εφαρμογής σας κονσόλας. Το αποτέλεσμα πρέπει να είναι παρόμοια με τα εξής:


        C:\Users\Wesley\AzME_Service_API_Rest\test.exe

        Getting authorization token...
        
        HTTP Status 200 : OK
        {
          "token_type": "Bearer",
          "expires_in": "3599",
          "expires_on": "1458085162",
          "not_before": "1458081262",
          "resource": "https://management.core.windows.net/",
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPW
        b3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFh
        NzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0cy53
        MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoiNzJm
        F5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE3OAE
        hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-HASI8
        }
        
        Got authorization token...
        Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNN
        aW5kb3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYt
        Zi1jNzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0
        OGU3MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoi
        iI-oF5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE
        vsf3hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-H
        
        Creating new campaign...
        
        HTTP Status 201 : Created
        {
          "id": 24,
          "state": "draft"
        }
        
        Created new campaign...
        New Campaign Id    : 24
        
        Activating new campaign (ID : 24)...
        
        HTTP Status 200 : OK
        {
          "id": 24,
          "state": "scheduled"
        }
        
        Activated new campaign...
        New Campaign State : scheduled
        
        Getting device IDs...
        
        HTTP Status 200 : OK
        {
          "value": [
            {
              "meta": {
                "lastSeen": 1458080534371,
                "firstSeen": 1458080534371,
                "lastLocation": 1458081389617,
                "lastInfo": 1458080571603
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "1d6208b8f281203ecb49431e2e5ce6b3"
            },
            {
              "meta": {
                "lastSeen": 1457990584698,
                "firstSeen": 1457949384025,
                "lastLocation": 1457990914895,
                "lastInfo": 1457990584597
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "302486644890e26045884ee5aa0619ec"
            }
          ]
        }
        
        Got devices.
        Adding device for push campaign...
        Device ID    : 1d6208b8f281203ecb49431e2e5ce6b3
        user_name    : Wesley
        Adding device for push campaign...
        Device ID    : 302486644890e26045884ee5aa0619ec
        user_name    : Wesley
        
        Triggering push for new campaign (ID : 24)...
        
        
        {"deviceIds":["1d6208b8f281203ecb49431e2e5ce6b3","302486644890e26045884ee5aa0619ec"]}
        
        HTTP Status 200 : OK
        {
          "invalidDeviceIds": []
        }
        
        Triggered push on new campaign.
        


<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png

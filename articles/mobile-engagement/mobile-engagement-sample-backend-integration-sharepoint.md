<properties 
    pageTitle="Azure κινητή δέσμευση - ενοποίησης παρασκηνίου" 
    description="Σύνδεση Azure δέσμευση Mobile με έναν υπολογιστή στο παρασκήνιο του SharePoint για να δημιουργήσετε καμπάνιες από το SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure κινητή δέσμευση - ενοποίησης API

Σε ένα αυτοματοποιημένο σύστημα μάρκετινγκ, τη δημιουργία και την ενεργοποίηση του εκστρατείες μάρκετινγκ επίσης γίνονται αυτόματα. Για το σκοπό - δέσμευση Mobile Azure επιτρέπει τη δημιουργία όπως αυτοματοποιημένη εκστρατείες μάρκετινγκ χρησιμοποιώντας APIs καθώς και. 

Συνήθως πελάτες Χρησιμοποιήστε το περιβάλλον προσκηνίου δέσμευση Mobile για να δημιουργήσετε ανακοινώσεις/ψηφοφορίες κ.λπ ως μέρος τους εκστρατείες μάρκετινγκ. Ωστόσο καθώς το εκστρατείες μάρκετινγκ γίνονται ώριμη, υπάρχει ανάγκη για να αξιοποιήσετε τα δεδομένα κλειδωμένο στα συστήματα παρασκηνίου (όπως οποιαδήποτε CRM CMS το σύστημα ή όπως το SharePoint), ώστε να μπορεί να δημιουργηθεί ένα πλήρως αυτοματοποιημένο διοχέτευσης που δημιουργεί εκστρατείες στο κινητό δέσμευση δυναμικά με βάση τα δεδομένα ροή στο από τα συστήματα παρασκηνίου. 

![][5]

Αυτό το πρόγραμμα εκμάθησης, έχετε ένα τέτοιο σενάριο όπου χρήστης επιχείρησης SharePoint συμπληρώνει μια λίστα του SharePoint με δεδομένα του μάρκετινγκ και μια αυτοματοποιημένη διαδικασία συλλογές ασφαλείας για τα στοιχεία από τη λίστα και συνδέεται με το σύστημα του κινητού δέσμευση χρήση διαθέσιμων REST API για τη δημιουργία μιας εκστρατείας μάρκετινγκ από τα δεδομένα του SharePoint. 
 
> [AZURE.IMPORTANT] Σε γενικές γραμμές, μπορείτε να χρησιμοποιήσετε αυτό το δείγμα ως σημείο εκκίνησης για να κατανοήσετε τον τρόπο για να καλέσετε οποιαδήποτε Mobile δέσμευση REST API όπως λεπτομερώς τις δύο βασικές πτυχές της καλεί τα API - παραμέτρους ελέγχου ταυτότητας και που περνά μέσα. 

## <a name="sharepoint-integration"></a>Ενοποίηση του SharePoint
1. Παρακάτω θα δείτε πώς φαίνεται το δείγμα λίστας του SharePoint. **Τίτλος**, **κατηγορία**, **NotificationTitle**, **μήνυμα** και **διεύθυνση URL** που χρησιμοποιούνται για τη δημιουργία του ανακοίνωση. Υπάρχει μια στήλη που ονομάζεται **IsProcessed** που χρησιμοποιείται από τη διαδικασία αυτοματισμού δείγμα με τη μορφή ενός προγράμματος κονσόλας. Μπορείτε είτε να εκτελέσετε αυτήν την κονσόλα πρόγραμμα ως μια WebJob Azure, έτσι ώστε να μπορείτε να προγραμματίσετε το ή απευθείας, μπορείτε να χρησιμοποιήσετε τη ροή εργασίας του SharePoint σε πρόγραμμα τη δημιουργία και την ενεργοποίηση του ανακοίνωση όταν προστίθεται ένα στοιχείο σε μια λίστα του SharePoint. Σε αυτό το δείγμα χρησιμοποιήσουμε το πρόγραμμα κονσόλας που διέρχεται από τα στοιχεία στη λίστα του SharePoint και δημιουργία ανακοίνωση στο Azure Mobile δέσμευση για κάθε μία από αυτές και, στη συνέχεια, τέλος επισημαίνει τη σημαία **IsProcessed** για να είναι αληθή κατά τη δημιουργία επιτυχής ανακοίνωση.

    ![][1]

2. Χρησιμοποιούμε τον κωδικό από το δείγμα για *απομακρυσμένο έλεγχο ταυτότητας στο SharePoint Online με το μοντέλο αντικειμένων του προγράμματος-πελάτη* [εδώ](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) για τον έλεγχο ταυτότητας με λίστα του SharePoint.
 
3. Αφού γίνει έλεγχος ταυτότητας, θα σας επανάληψη έως τα στοιχεία της λίστας για να βρείτε όλα τα στοιχεία που έχουν δημιουργηθεί πρόσφατα (που θα έχουν **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Ενοποίηση του Mobile δέσμευση
1.  Μόλις μπορούμε να βρούμε ενός στοιχείου που απαιτεί επεξεργασία - μας εξαγάγετε τις πληροφορίες που απαιτούνται για να δημιουργήσετε μια ανακοίνωση από το στοιχείο της λίστας και κλήση `CreateAzMECampaign` για τη δημιουργία του και στη συνέχεια `ActivateAzMECampaign` για να την ενεργοποιήσετε. Αυτά είναι ουσιαστικά REST API κλήσεις κλήση στον υπολογιστή στο παρασκήνιο δέσμευση Mobile. 

2.  Τα Mobile δέσμευση REST API απαιτείται μια **κεφαλίδα HTTP εξουσιοδότησης συνδυασμού βασικές auth** που αποτελείται από το `ApplicationId` και το `ApiKey` που λαμβάνετε από την πύλη του Azure. Βεβαιωθείτε ότι χρησιμοποιείτε το πλήκτρο από την ενότητα **κλειδιά api** και *όχι* από την ενότητα **πλήκτρα sdk** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Για να δημιουργήσετε την ανακοίνωση τύπος εκστρατείας - ανατρέξτε στην [τεκμηρίωση](https://msdn.microsoft.com/library/azure/mt683750.aspx). Πρέπει να βεβαιωθείτε ότι καθορίζετε στην εκστρατεία `kind` ως *ανακοίνωση* και το [φορτίο](https://msdn.microsoft.com/library/azure/mt683751.aspx) και που περνά μέσα ως FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Όταν έχετε ολοκληρώσει την ανακοίνωση που δημιουργήσατε, θα δείτε κάτι παρόμοιο με το εξής στην πύλη δέσμευση Mobile (Σημειώστε ότι η κατάσταση = Πρόχειρο και ενεργοποιημένο = δ/υ)

    ![][3]

5. `CreateAzMECampaign`δημιουργεί μια ανακοίνωση εκστρατεία και επιστρέφει το αναγνωριστικό καλούντος. `ActivateAzMECampaign`απαιτεί αυτό το αναγνωριστικό ως η παράμετρος ενεργοποίησης της εκστρατείας. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Όταν έχετε ολοκληρώσει την ανακοίνωση ενεργοποιηθεί, θα δείτε κάτι παρόμοιο με το εξής στην πύλη δέσμευση Mobile:

    ![][4]

7. Μόλις της εκστρατείας λαμβάνει ενεργοποιημένο, όλες τις συσκευές που πληρούν το κριτήριο σε εκστρατείας θα αρχίσει να βλέπετε ειδοποιήσεις. 

8. Θα παρατηρήσετε επίσης ότι το στοιχείο λίστας έχει επισημανθεί με IsProcessed = false έχει οριστεί στην τιμή True όταν δημιουργηθεί η ανακοίνωση εκστρατείας.  

Αυτό το δείγμα η δημιουργία μιας εκστρατείας ανακοίνωση απλής ακρίβειας που καθορίζει κυρίως τις απαιτούμενες ιδιότητες. Μπορείτε να την προσαρμόσετε έτσι ό, τι μπορείτε να κάνετε από την πύλη, χρησιμοποιώντας τις πληροφορίες [εδώ](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 

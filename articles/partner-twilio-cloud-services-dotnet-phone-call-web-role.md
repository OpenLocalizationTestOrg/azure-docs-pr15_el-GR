<properties 
    pageTitle="Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση από Twilio (.NET) | Microsoft Azure" 
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα κώδικα γραμμένο σε .NET." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση με χρήση Twilio σε ένα ρόλο web στο Azure

Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε Twilio για να πραγματοποιήσετε μια κλήση από μια ιστοσελίδα που φιλοξενείται στο Azure. Η εφαρμογή που προκύπτει ζητά από το χρήστη για τιμές τηλεφωνικής κλήσης, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης.

![Φόρμα Azure κλήσης με χρήση του Twilio και ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Προαπαιτούμενα στοιχεία

Θα πρέπει να κάνετε τα εξής για να χρησιμοποιήσετε τον κωδικό σε αυτό το θέμα:

1. Απόκτηση ένα λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικού. Για να ξεκινήσετε με Twilio, εγγραφείτε στο [https://www.twilio.com/try-twilio][try_twilio]. Μπορείτε να αξιολογήσετε τις τιμές στο [http://www.twilio.com/pricing][twilio_pricing]. Για πληροφορίες σχετικά με το API που παρέχεται από Twilio, ανατρέξτε στο θέμα [http://www.twilio.com/voice/api][twilio_api].
2. Προσθέστε τη βιβλιοθήκη Twilio .NET ο ρόλος σας web. Ανατρέξτε στο θέμα "για να προσθέσετε τις βιβλιοθήκες Twilio στο έργο σας ρόλο web," παρακάτω σε αυτό το θέμα.

Θα πρέπει να εξοικειωμένοι με τη δημιουργία ρόλου βασικά web στην Azure.

## <a name="howtocreateform"></a>Πώς μπορείτε να: Δημιουργία φόρμας web για την πραγματοποίηση κλήσης

<a id="use_nuget"></a>Για να προσθέσετε τις βιβλιοθήκες Twilio στο έργο σας ρόλο web:

1.  Ανοίξτε τη λύση σας στο Visual Studio.
2.  Κάντε δεξί κλικ **αναφορές**.
3.  Κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.
4.  Κάντε κλικ στην επιλογή **Online**.
5.  Στο πλαίσιο ηλεκτρονική αναζήτηση, πληκτρολογήστε *twilio*.
6.  Κάντε κλικ στην επιλογή **εγκατάσταση** του πακέτου Twilio.

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε μια φόρμα web για την ανάκτηση δεδομένων χρήστη για την πραγματοποίηση κλήσης. Σε αυτό το παράδειγμα, δημιουργείται ένα ρόλο web ASP.NET με το όνομα **TwilioCloud** .

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Πώς μπορείτε να: δημιουργία του κώδικα για την πραγματοποίηση της κλήσης
Ο ακόλουθος κώδικας, που ονομάζεται όταν ο χρήστης ολοκληρώσει τη φόρμα, δημιουργεί το μήνυμα κλήσης και την κλήση. Σε αυτό το παράδειγμα, ο κώδικας εκτελείται στο πρόγραμμα χειρισμού συμβάντων onclick του κουμπιού στη φόρμα. (Χρησιμοποιήστε το λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικό αντί για τις τιμές του πλαισίου κράτησης θέσης που έχουν εκχωρηθεί σε **accountSID** και **authToken** τον παρακάτω κώδικα.)

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Η κλήση πραγματοποιείται και εμφανίζονται το τελικό σημείο Twilio, έκδοση API και την κατάσταση της κλήσης. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει το αποτέλεσμα από ένα δείγμα εκτέλεση.

![Απάντηση κλήσης Azure χρησιμοποιώντας Twilio και ASP.NET][twilio_dotnet_basic_form_output]

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με TwiML στο [http://www.twilio.com/docs/api/twiml][twiml]. Περισσότερες πληροφορίες σχετικά με &lt;Καλώς&gt; και άλλα ρήματα Twilio μπορείτε να βρείτε στο [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Επόμενα βήματα
Αυτός ο κωδικός παρέχεται για να σας δείξουν βασικές λειτουργίες χρήση Twilio σε ένα ρόλο web ASP.NET σε Azure. Πριν αναπτύξετε για Azure παραγωγή, ενδέχεται να θέλετε να προσθέσετε περισσότερες χειρισμού σφαλμάτων ή άλλες δυνατότητες. Για παράδειγμα:

* Αντί να χρησιμοποιήσετε μια φόρμα web, μπορείτε να χρησιμοποιήσετε χώρο αποθήκευσης αντικειμένων Blob του Azure ή μια βάση δεδομένων SQL Azure παρουσία για την αποθήκευση αριθμών τηλεφώνου και καλέστε κειμένου. Για πληροφορίες σχετικά με τη χρήση αντικειμένων blob Azure, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure στο .NET][howto_blob_storage_dotnet]. Για πληροφορίες σχετικά με τη χρήση της βάσης δεδομένων SQL, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε τη βάση δεδομένων SQL Azure σε εφαρμογές .NET][howto_sql_azure_dotnet].
* Μπορείτε να χρησιμοποιήσετε RoleEnvironment.getConfigurationSettings για την ανάκτηση του Αναγνωριστικού λογαριασμού Twilio και διακριτικού ελέγχου ταυτότητας από την ανάπτυξη ρυθμίσεις παραμέτρων, αντί για τις τιμές στη φόρμα σας σκληρό κωδικοποίησης. Για πληροφορίες σχετικά με την κλάση RoleEnvironment, ανατρέξτε στο θέμα [Namespace Microsoft.WindowsAzure.ServiceRuntime][azure_runtime_ref_dotnet].
* Διαβάστε τις οδηγίες ασφαλείας Twilio στο [https://www.twilio.com/docs/security][twilio_docs_security].
* Μάθετε περισσότερα σχετικά με Twilio στο [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Δείτε επίσης
* [Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS από το Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx

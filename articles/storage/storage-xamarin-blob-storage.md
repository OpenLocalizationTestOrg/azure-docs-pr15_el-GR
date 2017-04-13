<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από Xamarin | Microsoft Azure"
    description="Στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για Xamarin επιτρέπει στους προγραμματιστές να δημιουργούν iOS, Android και Windows Store εφαρμογές με περιβάλλοντα εργασίας χρήστη εγγενή τους. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Xamarin για να δημιουργήσετε μια εφαρμογή που χρησιμοποιεί το χώρο αποθήκευσης αντικειμένων Blob του Azure."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Επισκόπηση

Οι προγραμματιστές Xamarin παρέχει τη δυνατότητα να χρησιμοποιήσετε ένα κοινόχρηστο C# βάσης κώδικα για να δημιουργήσετε iOS, Android και Windows Store εφαρμογές με περιβάλλοντα εργασίας χρήστη εγγενή τους. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε το χώρο αποθήκευσης αντικειμένων Blob του Azure με μια εφαρμογή Xamarin. Εάν θέλετε να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure, πριν από την καταδύσεις στον κώδικα, ανατρέξτε στο θέμα [Εισαγωγή στο Microsoft Azure αποθήκευσης](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Δημιουργία νέας εφαρμογής Xamarin

Για αυτό γρήγορα αποτελέσματα, θα σας θα δημιουργήσετε μια εφαρμογή που ως προορισμό Android, iOS και Windows. Αυτή η εφαρμογή θα απλώς να δημιουργήσετε ένα κοντέινερ και να αποστείλετε ένα blob σε αυτό το κοντέινερ. Θα χρησιμοποιούμε Visual Studio στα Windows για αυτό γρήγορα αποτελέσματα, αλλά το ίδιο learnings μπορούν να εφαρμοστούν κατά τη δημιουργία μιας εφαρμογής χρησιμοποιώντας Xamarin Studio στο Mac OS.

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε την εφαρμογή σας:

1. Εάν δεν το έχετε κάνει ήδη, κάντε λήψη και εγκατάσταση [Xamarin για το Visual Studio](https://www.xamarin.com/download).
2. Ανοίξτε Visual Studio, και δημιουργήστε μια κενή εφαρμογή (εγγενούς κοινόχρηστο): **Αρχείο > Δημιουργία > έργου > πλατφόρμες > κενό App(Native Shared)**.
3. Κάντε δεξί κλικ στο παράθυρο Εξερεύνηση λύσεων τη λύση σας και επιλέξτε **Διαχείριση πακέτων NuGet για τη λύση**. Αναζήτηση για **WindowsAzure.Storage** και εγκαταστήστε την πιο πρόσφατη έκδοση σταθερό σε όλα τα έργα στη λύση σας.
4. Δημιουργία και εκτέλεση του έργου σας.

Τώρα θα πρέπει να έχετε μια εφαρμογή η οποία σας επιτρέπει να κάνετε κλικ σε ένα κουμπί το οποίο προσαυξάνει μετρητής.

> [AZURE.NOTE] Η βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για Xamarin υποστηρίζει επί του παρόντος τους ακόλουθους τύπους έργου: εγγενούς θέσει σε κοινή χρήση, Xamarin.Forms σε κοινή χρήση, Xamarin.Android και Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Δημιουργία κοντέινερ και αποστολή αντικειμένων blob

Στη συνέχεια, θα προσθέσετε ορισμένες κώδικα στην κοινόχρηστη κλάση `MyClass.cs` που δημιουργεί ένα κοντέινερ και αποστέλλει ένα blob σε αυτό το κοντέινερ. `MyClass.cs`θα πρέπει να είναι ως εξής:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Βεβαιωθείτε ότι για να αντικαταστήσετε "your_account_name_here" και "your_account_key_here" με το όνομα του λογαριασμού πραγματική και κλειδί λογαριασμού. Μπορείτε να χρησιμοποιήσετε αυτήν την κλάση κοινόχρηστο στο iOS, Android και Windows Phone εφαρμογής. Μπορείτε απλώς να προσθέσετε `MyClass.createContainerAndUpload()` για κάθε έργο. Για παράδειγμα:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Τώρα, μπορείτε να εκτελέσετε αυτήν την εφαρμογή σε μια προσομοίωσης Android ή Windows Phone. Μπορείτε επίσης να εκτελέσετε αυτήν την εφαρμογή σε μια προσομοίωσης iOS, αλλά αυτό θα απαιτήσει Mac. Για συγκεκριμένες οδηγίες σχετικά με τον τρόπο για να το κάνετε αυτό, διαβάστε την τεκμηρίωση για [τη σύνδεση του Visual Studio για Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Όταν εκτελείτε την εφαρμογή σας, θα δημιουργήσει το κοντέινερ `mycontainer` στο λογαριασμό σας στο χώρο αποθήκευσης. Θα πρέπει να περιέχει το αντικείμενο blob, `myblob`, που έχει το κείμενο, `Hello, world!`. Μπορείτε να το επαληθεύσετε, χρησιμοποιώντας την [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com/).

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό γρήγορα αποτελέσματα, μάθατε πώς μπορείτε να δημιουργήσετε μια εφαρμογή πλατφόρμες στο Xamarin που χρησιμοποιεί το χώρο αποθήκευσης Azure. Αυτό συγκεκριμένα γρήγορα αποτελέσματα εστιασμένη σε ένα σενάριο στο χώρο αποθήκευσης αντικειμένων Blob. Ωστόσο, μπορείτε να κάνετε πιο συχνά με, όχι μόνο χώρο αποθήκευσης αντικειμένων Blob, αλλά επίσης με πίνακα, το αρχείο και αποθήκευση ουράς. Ελέγξτε τα παρακάτω άρθρα για να μάθετε περισσότερα:
- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω .NET](storage-dotnet-how-to-use-blobs.md)
- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET](storage-dotnet-how-to-use-tables.md)
- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης ουρά Azure μέσω .NET](storage-dotnet-how-to-use-queues.md)
- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αρχείων Azure στα Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

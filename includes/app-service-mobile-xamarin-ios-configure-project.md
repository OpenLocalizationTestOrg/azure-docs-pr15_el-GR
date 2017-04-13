####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Ρύθμιση των παραμέτρων του iOS έργου σε Xamarin Studio

1. Στη Xamarin.Studio, ανοίξτε το **Info.plist**και ενημερώστε το **Αναγνωριστικό πακέτου** με το Αναγνωριστικό πακέτου που δημιουργήσατε προηγουμένως με το νέο αναγνωριστικό εφαρμογής.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Κάντε κύλιση προς τα κάτω για **Λειτουργίες φόντου** και επιλέξτε το πλαίσιο **Ενεργοποίηση λειτουργιών φόντου** και το πλαίσιο **απομακρυσμένο ειδοποιήσεων** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Κάντε διπλό κλικ στο έργο σας στον πίνακα λύση για να ανοίξετε τις **Επιλογές του Project**.

4.  Επιλέξτε **iOS πακέτου είσοδο** στην ενότητα **Δημιουργία**και επιλέξτε την αντίστοιχη **ταυτότητα** και **Provisioning προφίλ** που είχατε ορίσει μόνο για αυτό το έργο. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Αυτό εξασφαλίζει ότι το project χρησιμοποιεί το νέο προφίλ για την υπογραφή κώδικα. Για την επίσημη Xamarin συσκευή προμήθειας τεκμηρίωση, ανατρέξτε στο θέμα [Xamarin συσκευή προμήθειας].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Ρύθμιση παραμέτρων του έργου iOS στο Visual Studio

1. Στο Visual Studio, κάντε δεξί κλικ στο έργο και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**.

2. Στις σελίδες ιδιοτήτων, κάντε κλικ στην καρτέλα **iOS εφαρμογής** και ενημερώστε το **αναγνωριστικό** με το Αναγνωριστικό που δημιουργήσατε νωρίτερα.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Στην καρτέλα **iOS πακέτου σχετικά με την είσοδο** , επιλέξτε την αντίστοιχη **ταυτότητα** και **Provisioning προφίλ** που είχατε ορίσει απλώς για αυτό το έργο. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Αυτό εξασφαλίζει ότι το project χρησιμοποιεί το νέο προφίλ για την υπογραφή κώδικα. Για την επίσημη Xamarin συσκευή προμήθεια τεκμηρίωση, ανατρέξτε στο θέμα [Xamarin προμήθεια συσκευή].

4. Κάντε διπλό κλικ Info.plist για να ανοίξετε και, στη συνέχεια, ενεργοποίηση **RemoteNotifications** στην περιοχή λειτουργίες φόντου. 



[Συσκευή Xamarin προμήθειας]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
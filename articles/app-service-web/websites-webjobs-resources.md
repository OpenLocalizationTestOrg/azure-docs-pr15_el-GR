<properties 
    pageTitle="Azure πόρους τεκμηρίωσης WebJobs" 
    description="Συνιστάται η πόρους για να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure WebJobs και το SDK WebJobs Azure." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure πόρους τεκμηρίωσης WebJobs

## <a name="overview"></a>Επισκόπηση

Σε αυτό το θέμα συνδέσεις για πόρους τεκμηρίωσης σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs Azure. Azure WebJobs παρέχουν έναν εύκολο τρόπο για να εκτελέσετε τις δέσμες ενεργειών ή προγράμματα ως διεργασίες παρασκηνίου στο περιβάλλον μιας [εφαρμογής υπηρεσίας web app, API εφαρμογής, ή εφαρμογή για κινητές συσκευές](../app-service/app-service-value-prop-what-is.md). Μπορείτε να αποστείλετε και να εκτελέσετε ένα εκτελέσιμο αρχείο όπως ως cmd, Μπατ, exe (.NET), ps1, ε, php, γραφή, js και βάζο. Αυτά τα προγράμματα εκτελούνται ως WebJobs σε ένα χρονοδιάγραμμα (cron) ή συνεχώς.

Ο σκοπός του [WebJobs SDK](websites-webjobs-resources.md) είναι για να απλοποιήσετε τον κώδικα που συντάσσετε για τις κοινές εργασίες που ένα WebJob να εκτελέσετε, όπως η επεξεργασία εικόνας, ουρά επεξεργασίας, RSS συνάθροισης, αρχείο συντήρησης και αποστολή μηνυμάτων ηλεκτρονικού ταχυδρομείου. Το SDK WebJobs έχει ενσωματωμένες δυνατότητες για την εργασία με το χώρο αποθήκευσης Azure και Bus υπηρεσίας, για τον προγραμματισμό εργασιών και χειρισμού σφαλμάτων και πολλά άλλα συνηθισμένα σενάρια. Επίσης, είναι σχεδιασμένος για να επεκτάσιμη και δεν υπάρχει ένα [Άνοιγμα αποθετήριο δεδομένων προέλευσης για επεκτάσεις](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Συναρτήσεις Azure](../azure-functions/functions-overview.md) (αυτήν τη στιγμή στην έκδοση preview) βασίζεται σε μια έκδοση του SDK WebJobs που λειτουργεί με το C# δέσμης ενεργειών, Node.js και άλλες γλώσσες. 

Δημιουργία, την ανάπτυξη και τη Διαχείριση WebJobs είναι απρόσκοπτη με ενσωματωμένη tooling στο Visual Studio. Μπορείτε να δημιουργήσετε WebJobs από πρότυπα, δημοσίευση, και διαχείριση (εκτέλεση/διακοπή/οθόνη/εντοπισμός σφαλμάτων) τους. 

Ο πίνακας εργαλείων WebJobs στην πύλη του Azure παρέχει δυνατότητες ισχυρή διαχείρισης που να έχετε πλήρη έλεγχο σε την εκτέλεση της WebJobs, συμπεριλαμβανομένης της δυνατότητας για να καλέσετε μεμονωμένα συναρτήσεων μέσα σε WebJobs. Στον πίνακα εργαλείων εμφανίζει επίσης συνάρτηση χρόνους εκτέλεσης και καταγραφή εξόδου. 

##<a name="getstarted"></a>Γρήγορα αποτελέσματα με το WebJobs και το WebJobs SDK

* [Εισαγωγή στις Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs είναι εκπληκτική και θα πρέπει να ξεκινήσετε να χρησιμοποιείτε τους αυτήν τη στιγμή!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Δημοσίευση ιστολογίου από Αλεξάνδρου Troy.)
* [Δυνατότητες Azure WebJobs](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Τι είναι το SDK WebJobs](websites-dotnet-webjobs-sdk.md)
* [Καθοδήγηση εργασίες φόντου, Microsoft μοτίβα και πρακτικές](/documentation/articles/best-practices-background-jobs/)
* [Ανακοίνωση του 1.1.0 RTM του Microsoft Azure WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Γρήγορα αποτελέσματα με το Azure SDK WebJobs](websites-dotnet-webjobs-sdk-get-started.md)
* [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων του Azure με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Πώς μπορείτε να χρησιμοποιήσετε Azure Service Bus με το SDK WebJobs](websites-dotnet-webjobs-sdk-service-bus.md)
* [Πώς μπορείτε να χρησιμοποιήσετε WebHooks με το SDK WebJobs, με παραδείγματα για GitHub, IFTTT και HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK γρήγορης αναφοράς (λήψη PDF)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [Τεκμηρίωση ρυθμίσεις WebJobs σε GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Βίντεο
    * [WebJobs και το WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Σειρά Azure WebJobs βίντεο στο κανάλι 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Εισαγωγή WebJobs Tooling για το Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling και ο απομακρυσμένος εντοπισμός σφαλμάτων](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure ενημέρωση WebJobs με Pranav Rastogi - επεκτασιμότητα του στην έκδοση 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Ανατρέξτε επίσης στις ακόλουθες ενότητες για [Την ανάπτυξη WebJobs](#deploy) και [δοκιμή και αντιμετώπιση σφαλμάτων WebJobs](#debug).

##<a name="deploy"></a>Για την ανάπτυξη WebJobs

* [Πώς μπορείτε να αναπτύξετε WebJobs Azure χρήση του Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Πώς να αναπτύξετε WebJobs με την πύλη Azure](web-sites-create-web-jobs.md)
* [Ενεργοποίηση της γραμμής εντολών ή συνεχής παράδοση του Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Για την ανάπτυξη μιας εφαρμογής κονσόλας .NET στον Azure χρησιμοποιώντας WebJobs Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Για την ανάπτυξη μιας WebJob F # στον Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Για την ανάπτυξη προσαρμοσμένων υπηρεσιών ως Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Βίντεο
    * [Εισαγωγή WebJobs Tooling για το Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling και ο απομακρυσμένος εντοπισμός σφαλμάτων](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Προγραμματισμός WebJobs

* [Το παράθυρο διαλόγου Azure WebJob προσθήκης](websites-dotnet-deploy-webjobs.md#configure)
* [Δημιουργήστε μια προγραμματισμένη WebJob στην πύλη του Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Διάταξη του χρονοδιαγράμματος εργασίας για μια WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Προγραμματισμός Azure WebJobs με τις παραστάσεις cron](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Προγραμματισμός μεμονωμένα WebJob συναρτήσεις χρησιμοποιώντας το TimerTrigger SDK WebJobs](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Δοκιμή και εντοπισμός σφαλμάτων WebJobs

* [Νέα για προγραμματιστές και τις δυνατότητες εντοπισμού για Azure WebJobs στο Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Προβολή του πίνακα εργαλείων WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Πώς να συντάξετε αρχεία καταγραφής χρησιμοποιώντας το SDK WebJobs και να τα προβάλετε στον πίνακα εργαλείων](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Απομακρυσμένη WebJobs εντοπισμού σφαλμάτων](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Άτομα που συντάξατε το αντικείμενο blob;](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Φιλοξενίας αλληλεπιδραστικών κώδικα στο Cloud](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Προσθήκη παρακολούθησης σε Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Παρακολούθηση, διάγνωση και αντιμετώπιση προβλημάτων χώρος αποθήκευσης Microsoft Azure](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Βίντεο
    * [WebJobs Tooling και ο απομακρυσμένος εντοπισμός σφαλμάτων](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Κλιμάκωση WebJobs

* [Κλιμάκωση σας εφαρμογής Web με Azure τοποθεσίες Web](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure εφαρμογής υπηρεσίας: Εξειδικευμένος μαζικών κλίμακας έτοιμη για επιχειρήσεις Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Εξώφυλλα κλίμακα των εφαρμογών web με WebJobs, συμπεριλαμβανομένου του SDK WebJobs.
* Βίντεο
    * [Κλιμάκωση ανάληψη WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Πρόσθετοι πόροι WebJobs

* [Azure δημοσίευση ιστολογίου WebJobs GA από Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Εκτέλεση εργασιών Powershell Web σε Azure εφαρμογής υπηρεσίας](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Λήψη ειδοποίησης κατά το Azure ενεργοποίησε WebJobs ολοκληρώνει](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Απλή πολιτικής διατήρησης δημιουργίας αντιγράφων ασφαλείας εφαρμογών Web με WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure Web Apps και τις υπηρεσίες Cloud αργές την πρώτη αίτηση](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Δείχνει πώς μπορείτε να χρησιμοποιήσετε WebJobs για να προσομοιώσετε τη δυνατότητα AlwaysOn που είναι διαθέσιμη μόνο για την τυπική σειρά τις πληροφορίες τιμολόγησης.
* [Τερματισμός φυσιολογική WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Τερματισμός φυσιολογική για WebJobs SDK, ανατρέξτε στο θέμα [φυσιολογική τερματισμού](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Αυτοματοποίηση της δημιουργίας αντιγράφων ασφαλείας με το Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Βίντεο
    * [Azure βίντεο WebJobs με Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Σειρά Azure WebJobs βίντεο στο κανάλι 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Πρόσθετοι πόροι WebJobs SDK

* [Σημειώσεις έκδοσης SDK WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK πηγαίου κώδικα](https://github.com/Azure/azure-webjobs-sdk)
* [Επεκτάσεις WebJobs SDK πηγαίος κώδικας](https://github.com/Azure/azure-webjobs-sdk-extensions), με [λεπτομερή οδηγό στο μοντέλο επεκτασιμότητα του](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Πηγαίος κώδικας δέσμης ενεργειών WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-script/) (χρησιμοποιείται για [Τις συναρτήσεις Azure](../azure-functions/functions-overview.md))
* [WebJob για την αποστολή αρχείων FREB στο χώρο αποθήκευσης Azure χρησιμοποιώντας το SDK WebJobs](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Φιλοξενία Azure webjobs εκτός του Azure, με τα πλεονεκτήματα καταγραφής από μια Azure φιλοξενούνται webjob](http://bypassion.dk/?p=510)
* [Δημιουργία ενός εργαλείου εισαγωγής δεδομένων με το Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Επισκόπηση Azure συναρτήσεων](../azure-functions/functions-overview.md)
* Βίντεο
    * [Σειρά Azure WebJobs βίντεο στο κανάλι 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Δείγμα WebJob εφαρμογές

* [Δείγματα εφαρμογών που παρέχεται από την ομάδα WebJobs σε GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Απλή Azure Web App με WebJobs παρασκηνίου χρησιμοποιώντας το SDK WebJobs](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Παρουσιάζει χρήση της WebJobs προγραμματισμένη και βάσει συμβάντων. Ανατρέξτε στην καταχώρηση [φτιάχνετε το SiteMonitR χρησιμοποιώντας Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs)ιστολογίου.

##<a name="blogs"></a>Ιστολόγια

* [Ιστολόγιο του Azure](/blog)
* [Ιστολόγιο της Amit Apple](http://blog.amitapple.com/). Εστίαση στον WebJobs (μην το SDK).
* [Ιστολόγιο του Magnus Mårtensson](http://magnusmartensson.com/)

##<a name="gethelp"></a>Λήψη Βοήθειας με WebJobs

* [StackOverflow για WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow για το SDK WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow για τις συναρτήσεις Azure](http://stackoverflow.com/questions/tagged/azure-functions)
* [Φόρουμ Azure και ASP.NET](http://forums.asp.net/1247.aspx)
* [Azure φόρουμ εφαρμογής υπηρεσίας Web Apps](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure τοποθεσίας Web Apps χρήστη φωνής](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Χρησιμοποιήστε το hashtag #AzureWebJobs.
* [Αναφορά ένα σφάλμα WebJobs ή το ζήτημα](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)


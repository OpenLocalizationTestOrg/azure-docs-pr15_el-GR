<properties
   pageTitle="Τίτλος σελίδας που εμφανίζει το πρόγραμμα περιήγησης tab και τα αποτελέσματα αναζήτησης"
   description="Το άρθρο περιγραφή που θα εμφανίζεται σε σελίδες υποδοχής και στα περισσότερα αποτελέσματα αναζήτησης"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Πρότυπο markdown (άρθρο επικεφαλίδα 1 ή Η1): επικεφαλίδα στο επάνω μέρος αυτού του άρθρου

Για να αντιγράψετε το markdown από αυτό το πρότυπο, αντιγράψτε στο άρθρο στον τοπικό σας repo, ή κάντε κλικ στο κουμπί ανεπεξέργαστα στο περιβάλλον εργασίας Χρήστη του GitHub και αντιγράψτε την markdown.

  ![Εναλλακτικό κείμενο. δεν αφήστε κενό. Περιγραφή εικόνας.][8]

Εισαγωγή παραγράφων: amet dolor Lorem, adipiscing elit. Risus nulla interdum Phasellus, lacinia porta nisl imperdiet sed. Mauris dolor Mauris, tempus sed lacinia nec, μη felis euismod. Nunc semper porta ultrices. Maecenas neque nulla, ipsum σημείωμα condimentum καθίσετε amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Επικεφαλίδα 2 (H2)

Aenean καθίσετε amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, faucibus στο rhoncus στο faucibus sed urna.  volutpat mi αναγνωριστικό purus ultrices iaculis nec μη neque. Το [κείμενο της σύνδεσης για σύνδεση εκτός azure.microsoft.com](http://weblogs.asp.net/scottgu). Dolor dictum Nullam στο aliquam pharetra. Vivamus ac hendrerit mauris [παράδειγμα κείμενο σύνδεσης για σύνδεση σε ένα άρθρο σε ένα φάκελο υπηρεσίας](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Λαμβάνω 10 φορές περισσότερη κυκλοφορία από το [Google]  [ gog] από από [Yahoo]  [ yah] ή [MSN] [msn].

> [AZURE.NOTE] Κείμενο με εσοχή σημείωση.  Το word 'Σημείωση' θα προστεθεί κατά τη διάρκεια της δημοσίευσης. Κοπή ΕΕ pretium lacus. Nullam purus Ανατολική ώρα, πιπέδου εκτιμώμενη sed iaculis, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, ΕΕ condimentum turpis nisi ένα purus.

1. Aenean καθίσετε amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, faucius στο **Rhoncus** στο faucibus sed urna. Suspendisse volutpat mi αναγνωριστικό purus ultrices iaculis nec μη neque.

    ![Εναλλακτικό κείμενο. μην είναι κενό. Αυτοκίνητο συλλογής στο ανίχνευσης κόκκινο χρώμα.][5]

3. Dolor dictum Nullam στο aliquam pharetra. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum και ΔΙΑΦΟΡΩΝ a, vehicula στο nisl.

    ![Εναλλακτικό κείμενο. μην είναι κενό][6]


Suspendisse volutpat mi αναγνωριστικό purus ultrices iaculis nec μη neque. Dolor dictum Nullam στο aliquam pharetra. Vivamus ac hendrerit mauris. Otrus informatus: [1 σύνδεση σε ένα άλλο θέμα τεκμηρίωση azure.microsoft.com](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Επικεφαλίδα (H2)

Κοπή ΕΕ pretium lacus. Nullam purus Ανατολική ώρα, πιπέδου εκτιμώμενη sed iaculis, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, ΕΕ condimentum turpis nisi ένα purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum των προτέρων ipsum primis στο faucibus orci luctus και ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam μη <i>nisl molestie</i> pharetra eget μια εκτιμώμενη. [Σύνδεση 2 σε ένα άλλο θέμα τεκμηρίωση azure.microsoft.com](web-sites-custom-domain-name.md)


Quisque commodo eros πιπέδου lectus euismod auctor eget καθίσετε amet leo. Proin suscipit faucibus tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Επικεφαλίδα 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + SEM

2. Nullam στο hendrerit tempus tellus ΕΕ Τοσκάνης που έχει υποβληθεί.

    ![Εναλλακτικό κείμενο. μην είναι κενό. Παράδειγμα 9 καναλιού βίντεο.][7]

3. Enim felis Quisque, aliquam κοπή fermentum nec, pellentesque pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

Vestibul των προτέρων ipsum primis στο faucibus orci luctus και ultrices posuere cubilia Curae; Ultricies Nullam, hendrerit volutpat σημείωμα ipsum, purus diam pretium eros, σημείωμα tincidunt nulla lorem sed turpis: [3 σύνδεση σε ένα άλλο θέμα τεκμηρίωση azure.microsoft.com](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    

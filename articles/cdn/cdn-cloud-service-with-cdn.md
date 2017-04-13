<properties
    pageTitle="Ενοποίηση μια υπηρεσία cloud με το Azure CDN | Microsoft Azure"
    description="Ένα πρόγραμμα εκμάθησης που σας μαθαίνει πώς να αναπτύξετε μια υπηρεσία cloud που χρησιμοποιείται περιεχόμενο από μια ενσωματωμένη τελικού σημείου Azure CDN"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Ενοποίηση μια υπηρεσία cloud με Azure CDN

Μια υπηρεσία cloud μπορεί να ενσωματωθεί στις CDN Azure, εξυπηρετεί οποιοδήποτε περιεχόμενο από την υπηρεσία cloud θέση. Αυτή η προσέγγιση σάς παρέχει τα εξής πλεονεκτήματα:

- Ανάπτυξη και ενημέρωση των εικόνων, δεσμών ενεργειών και φύλλα στυλ σε καταλόγους έργου υπηρεσία cloud εύκολα
- Αναβάθμιση εύκολα τα πακέτα NuGet στην υπηρεσία cloud, όπως jQuery ή εκκίνησης εκδόσεις
- Διαχείριση της εφαρμογής Web και σας εξυπηρετεί CDN περιεχομένου όλα από το ίδιο περιβάλλον εργασίας του Visual Studio
- Ενοποιημένος χώρος ανάπτυξης ροής εργασίας για την εφαρμογή Web και το περιεχόμενό σας εξυπηρετεί CDN
- Ενοποίηση του ASP.NET δέσμης και minification με Azure CDN

## <a name="what-you-will-learn"></a>Τι θα μάθετε ##

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να:

-   [Ενοποίηση του τελικού σημείου Azure CDN με την υπηρεσία cloud και εξυπηρέτηση στατικό περιεχόμενο στις ιστοσελίδες σας από το Azure CDN](#deploy)
-   [Ρύθμιση παραμέτρων cache για στατικό περιεχόμενο στην υπηρεσία cloud](#caching)
-   [Εξυπηρέτηση περιεχομένου από ενέργειες ελεγκτή μέσω Azure CDN](#controller)
-   [Σερβίρισμα ομαδοποιημένα και minified περιεχομένου μέσω του Azure CDN διατηρώντας τη δέσμη ενεργειών εμπειρία στο Visual Studio εντοπισμού](#bundling)
-   [Ρύθμιση παραμέτρων επιστροφή δέσμες ενεργειών και CSS όταν σας CDN Azure είναι χωρίς σύνδεση](#fallback)

## <a name="what-you-will-build"></a>Τι θα δημιουργήσετε ##

Θα αναπτύξετε ένα ρόλο cloud υπηρεσίας Web, χρησιμοποιώντας το προεπιλεγμένο πρότυπο ASP.NET MVC, Προσθήκη κώδικα για την εξυπηρέτηση περιεχομένου από μια ενσωματωμένη CDN Azure, όπως μια εικόνα, αποτελέσματα ενέργεια ελεγκτή και τα προεπιλεγμένα αρχεία JavaScript και CSS και επίσης σύνταξη κώδικα για τη ρύθμιση παραμέτρων εναλλακτικών μηχανισμό πακέτα που σερβιρίστηκε σε περίπτωση που το CDN είναι εκτός σύνδεσης.

## <a name="what-you-will-need"></a>Τι θα χρειαστείτε ##

Αυτό το πρόγραμμα εκμάθησης περιλαμβάνει τις ακόλουθες προϋποθέσεις:

-   Ενεργό [λογαριασμό Microsoft Azure](/account/)
-   Visual Studio 2015 με [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] Χρειάζεστε ένα λογαριασμό Azure για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης:
> + Μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](/pricing/free-trial/) - λάβετε πιστώσεων μπορείτε να χρησιμοποιήσετε για να δοκιμάσετε την πληρωμή Azure υπηρεσιών και ακόμα και αν χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και χρήση δωρεάν Azure υπηρεσίες, όπως τοποθεσίες Web που διαθέτετε.
> + Μπορείτε να [ενεργοποιήσετε το MSDN συνδρομητών πλεονεκτήματα](/pricing/member-offers/msdn-benefits-details/) - τη συνδρομή σας MSDN παρέχει πιστώσεων κάθε μήνα που μπορείτε να χρησιμοποιήσετε για τις υπηρεσίες του Azure επί πληρωμή.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Ανάπτυξη μια υπηρεσία στο cloud ##

Σε αυτήν την ενότητα, θα αναπτύξετε το προεπιλεγμένο πρότυπο εφαρμογής ASP.NET MVC στο Visual Studio 2015 σε ένα ρόλο cloud υπηρεσίας Web και, στη συνέχεια, την ενοποίηση με ένα νέο τελικό σημείο CDN. Ακολουθήστε τις παρακάτω οδηγίες:

1. Στο Visual Studio 2015, δημιουργήστε μια νέα υπηρεσία Azure cloud από τη γραμμή μενού, μεταβαίνοντας **Αρχείο > Δημιουργία > έργο > Cloud > υπηρεσία Cloud Azure**. Δώσετε ένα όνομα και κάντε κλικ στο **κουμπί OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Επιλέξτε **Το ρόλο Web ASP.NET** και κάντε κλικ στην επιλογή το **>** κουμπί. Κάντε κλικ στο κουμπί OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Επιλέξτε **MVC** και κάντε κλικ στο κουμπί **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Τώρα, μπορείτε να δημοσιεύσετε αυτός ο ρόλος Web σε μια υπηρεσία Azure cloud. Κάντε δεξί κλικ στο έργο υπηρεσιών cloud και επιλέξτε **Δημοσίευση**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Εάν δεν έχετε ακόμη εισέλθει στο Microsoft Azure, κάντε κλικ στην αναπτυσσόμενη λίστα **Προσθήκη λογαριασμού...** και κάντε κλικ στο στοιχείο μενού " **Προσθήκη λογαριασμού** ".

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Στη σελίδα εισόδου, συνδεθείτε με το λογαριασμό Microsoft που χρησιμοποιήσατε για να ενεργοποιήσετε το λογαριασμό σας Azure.
7. Όταν έχετε πραγματοποιήσει είσοδο, κάντε κλικ στο κουμπί **Επόμενο**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Εάν υποθέσουμε ότι δεν έχετε δημιουργήσει ένα λογαριασμό υπηρεσίας ή χώρο αποθήκευσης στο cloud, Visual Studio θα σας βοηθήσει να δημιουργήσετε και τα δύο. Στο παράθυρο διαλόγου **Δημιουργία υπηρεσία στο Cloud και του λογαριασμού** , πληκτρολογήστε το όνομα της υπηρεσίας που θέλετε και επιλέξτε την επιθυμητή περιοχή. Στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Στη σελίδα ρυθμίσεων δημοσίευση, επαληθεύστε τις ρυθμίσεις παραμέτρων και κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Η διαδικασία δημοσίευσης για τις υπηρεσίες cloud διαρκεί πολύ χρόνο. Η ενεργοποίηση Web αναπτύξετε για όλους τους ρόλους επιλογή μπορούν να κάνουν εντοπισμού σας πολύ γρηγορότερα τις υπηρεσία cloud, παρέχοντας γρήγορη (αλλά προσωρινό) ενημερώσεις για ρόλους σας Web. Για περισσότερες πληροφορίες σχετικά με αυτήν την επιλογή, ανατρέξτε στο θέμα [δημοσίευση μια υπηρεσία στο Cloud χρησιμοποιώντας τα εργαλεία Azure](http://msdn.microsoft.com/library/ff683672.aspx).

    Όταν το **Αρχείο καταγραφής του Microsoft Azure δραστηριοτήτων** δείχνει ότι η δημοσίευση κατάσταση είναι **ολοκληρώθηκε**, θα δημιουργήσετε ένα τελικό σημείο CDN που είναι συνδεδεμένο με αυτήν την υπηρεσία cloud.

    >[AZURE.WARNING] Εάν, μετά τη δημοσίευση, την υπηρεσία cloud ανεπτυγμένος εμφανίζει μια οθόνη σφάλματος, είναι πιθανό επειδή την υπηρεσία cloud που έχετε αναπτύξει χρησιμοποιεί [λειτουργικό σύστημα που δεν περιλαμβάνει το .NET 4.5.2 επισκέπτη](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Μπορείτε να επιλύσετε αυτό το ζήτημα, [.NET 4.5.2 ως εργασία εκκίνησης για την ανάπτυξη](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Δημιουργήστε ένα νέο προφίλ CDN

Ένα προφίλ CDN είναι μια συλλογή από τα τελικά σημεία CDN.  Κάθε προφίλ περιέχει μία ή περισσότερες τελικά σημεία CDN.  Ενδέχεται να θέλετε να χρησιμοποιήσετε πολλά προφίλ για να οργανώσετε τα τελικά σημεία σας CDN ανά τομέα internet, εφαρμογή web ή άλλα κριτήρια.

> [AZURE.TIP] Εάν έχετε ήδη ένα προφίλ CDN που θέλετε να χρησιμοποιήσετε για αυτό το πρόγραμμα εκμάθησης, προχωρήστε στο θέμα [Δημιουργία ένα νέο τελικό σημείο CDN](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Δημιουργήστε ένα νέο τελικό σημείο CDN

**Για να δημιουργήσετε ένα νέο τελικό σημείο CDN για το λογαριασμό χώρου αποθήκευσης**

1. Στην [Πύλη διαχείρισης του Azure](https://portal.azure.com), μεταβείτε στο προφίλ σας CDN.  Που μπορεί να έχουν καρφιτσωμένη την στον πίνακα εργαλείων στο προηγούμενο βήμα.  Εάν που δεν, μπορείτε να το βρείτε, κάνοντας κλικ στην επιλογή **Αναζήτηση**, στη συνέχεια, **CDN προφίλ**και, κάνοντας κλικ στην επιλογή το προφίλ που σκοπεύετε να το τελικό σημείο για να προσθέσετε.

    Εμφανίζεται το blade CDN προφίλ.

    ![CDN προφίλ][cdn-profile-settings]

2. Κάντε κλικ στο κουμπί **Προσθήκη τελικού σημείου** .

    ![Προσθήκη κουμπιού τελικού σημείου][cdn-new-endpoint-button]

    Εμφανίζεται το blade **Προσθήκη τελικού σημείου** .

    ![Προσθήκη blade τελικού σημείου][cdn-add-endpoint]

3. Πληκτρολογήστε ένα **όνομα** για αυτό το τελικό σημείο CDN.  Αυτό το όνομα που θα χρησιμοποιηθεί για να αποκτήσετε πρόσβαση στο cache τους πόρους σας στον τομέα `<EndpointName>.azureedge.net`.

4. Στην αναπτυσσόμενη λίστα **Τύπος προέλευσης** , επιλέξτε *μια υπηρεσία Cloud*.  

5. Στην αναπτυσσόμενη λίστα **Origin hostname** , επιλέξτε την υπηρεσία cloud.

6. Αφήστε τις προεπιλογές για **Origin διαδρομή**, **Origin κεφαλίδα κεντρικού υπολογιστή**και **θύρας πρωτόκολλο/Origin**.  Πρέπει να καθορίσετε τουλάχιστον μία protocol (HTTP ή HTTPS).

7. Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε το νέο τελικό σημείο.

8. Αφού δημιουργηθεί το τελικό σημείο, εμφανίζεται σε μια λίστα με τα τελικά σημεία για το προφίλ. Η προβολή λίστας εμφανίζει τη διεύθυνση URL για να χρησιμοποιήσετε για να αποκτήσετε πρόσβαση στο cache περιεχομένου, καθώς και τον τομέα προέλευσης.

    ![Τελικό σημείο CDN][cdn-endpoint-success]

    > [AZURE.NOTE] Το τελικό σημείο δεν θα αμέσως είναι διαθέσιμη για χρήση.  Μπορεί να χρειαστούν έως και 90 λεπτά για την καταχώρηση για τη μετάδοση μέσω του δικτύου CDN. Οι χρήστες που προσπαθούν να χρησιμοποιήσετε το όνομα τομέα CDN αμέσως ενδέχεται να εμφανιστεί ο κωδικός κατάστασης 404 μέχρι το περιεχόμενο είναι διαθέσιμο μέσω το CDN.

## <a name="test-the-cdn-endpoint"></a>Δοκιμάστε το τελικό σημείο CDN

Όταν η κατάσταση δημοσίευσης **ολοκληρώθηκε**, ανοίξτε ένα παράθυρο προγράμματος περιήγησης και μεταβείτε σε * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. Στο πρόγραμμα εγκατάστασης μου, αυτή η διεύθυνση URL είναι:

    http://camservice.azureedge.net/Content/bootstrap.css

Που αντιστοιχεί στην ακόλουθη διεύθυνση URL origin στο το τελικό σημείο CDN:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Όταν μεταβαίνετε σε * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, ανάλογα με το πρόγραμμα περιήγησης, θα σας ζητηθεί να κάνετε λήψη ή ανοίξτε το bootstrap.css που προέρχονται από την εφαρμογή Web της δημοσιευμένης.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Ομοίως μπορείτε να αποκτήσετε πρόσβαση οποιαδήποτε δυνατότητα δημόσιας πρόσβασης διεύθυνση URL στο * *http://*&lt;όνομα_υπηρεσίας >*.cloudapp.net/**, απευθείας από το τελικό σημείο CDN. Για παράδειγμα:

-   Ένα αρχείο .js από τη διαδρομή/Script
-   Οποιοδήποτε αρχείο περιεχομένου από το /Content διαδρομή
-   Οποιαδήποτε ελεγκτή/ενέργεια
-   Εάν είναι ενεργοποιημένη η συμβολοσειρά ερωτήματος σε το τελικό σημείο CDN, οποιαδήποτε διεύθυνση URL με συμβολοσειρές ερωτήματος

Στην πραγματικότητα, με τις παραπάνω ρυθμίσεις παραμέτρων, μπορείτε να φιλοξενήσετε την υπηρεσία cloud ολόκληρο από * *http://*&lt;cdnName >*.azureedge.net/**. Εάν να μεταβείτε σε **http://camservice.azureedge.net/ **, λαμβάνω το αποτέλεσμα ενέργειας από κεντρική/ευρετήριο.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Αυτό δεν σημαίνει, ωστόσο, ώστε να είναι πάντοτε μια καλή ιδέα (ή γενικά καλή ιδέα) για να λειτουργήσει μια υπηρεσία cloud ολόκληρο μέσω Azure CDN. Ορισμένα από τα προειδοποιήσεις είναι οι εξής:

-   Αυτή η προσέγγιση απαιτεί ολόκληρη την τοποθεσία σας να είναι δημόσια, επειδή Azure CDN δεν μπορεί να χρησιμοποιηθεί οποιοδήποτε περιεχόμενο ιδιωτικό αυτήν τη στιγμή.
-   Αν το τελικό σημείο CDN αποσυνδεθεί για οποιονδήποτε λόγο, εάν προγραμματισμένη συντήρηση ή σφάλμα χρήστη, την υπηρεσία cloud ολόκληρο αποσυνδεθεί, εκτός εάν οι πελάτες να ανακατευθύνονται τη διεύθυνση URL origin * *http://*&lt;όνομα_υπηρεσίας >*.cloudapp.net/**.
-   Ακόμα και με τις προσαρμοσμένες ρυθμίσεις Cache-Control (ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων επιλογών προσωρινής αποθήκευσης για στατικά αρχεία στην υπηρεσία cloud](#caching)), ένα τελικό σημείο CDN δεν βελτιώνει τις επιδόσεις της ιδιαίτερα δυναμικό περιεχόμενο. Εάν προσπαθήσατε να φορτώσετε την αρχική σελίδα από το τελικό σημείο CDN ως φαίνεται παραπάνω, παρατηρήστε ότι χρειάστηκαν τουλάχιστον 5 δευτερόλεπτα για να φορτώσετε την πρώτη φορά, η οποία είναι αρκετά απλό σελίδας της προεπιλεγμένης αρχικής σελίδας. Φανταστείτε τι θα συμβεί στην εμπειρία του προγράμματος-πελάτη, εάν αυτή η σελίδα περιέχει δυναμικό περιεχόμενο που πρέπει να ενημερώσετε κάθε λεπτό. Εξυπηρετεί δυναμικό περιεχόμενο από ένα τελικό σημείο CDN απαιτεί σύντομο cache λήξη, η οποία μεταφράζεται στο cache συχνές αποτυχίες κατά το τελικό σημείο CDN. Αυτό πονάει τις επιδόσεις της την υπηρεσία cloud και defeats το σκοπό της ένα CDN.

Η εναλλακτική λύση είναι να καθορίσετε ποιο περιεχόμενο θα χρησιμοποιηθεί από το Azure CDN με βάση κατά περίπτωση στην υπηρεσία cloud. Για το σκοπό που ήδη έχετε δει πώς μπορείτε να αποκτήσετε πρόσβαση σε μεμονωμένα αρχεία περιεχομένου από το τελικό σημείο CDN. Που θα σας δείξουμε πώς να εξυπηρετούν μια ενέργεια συγκεκριμένο ελεγκτή έως το τελικό σημείο CDN στο [λειτουργήσει περιεχόμενο από ενέργειες ελεγκτή μέσω Azure CDN](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Ρύθμιση παραμέτρων επιλογών σε cache για στατικά αρχεία στην υπηρεσία cloud ##

Με ενσωμάτωση του Azure CDN στην υπηρεσία cloud, μπορείτε να καθορίσετε πώς θέλετε να είναι προσωρινά στο το τελικό σημείο CDN στατικό περιεχόμενο. Για να το κάνετε αυτό, ανοίξτε *Web.config* από το έργο σας Web ρόλο (π.χ., WebRole1) και προσθέστε μια `<staticContent>` στοιχείου για `<system.webServer>`. Το παρακάτω XML ρυθμίζει τις παραμέτρους του cache ώστε να λήγει σε 3 ημέρες.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Αφού κάνετε αυτό, όλα τα αρχεία στατικό στην υπηρεσία cloud θα παρατηρήσετε στον ίδιο κανόνα με το cache CDN. Για πιο λεπτομερές έλεγχο του ρυθμίσεις του cache, προσθέστε ένα αρχείο *Web.config* σε ένα φάκελο και εκεί τις ρυθμίσεις σας. Για παράδειγμα, προσθέστε ένα αρχείο *Web.config* στο φάκελο *\Content* και αντικαταστήστε το περιεχόμενο με το παρακάτω δείγμα XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Αυτή η ρύθμιση έχει ως αποτέλεσμα όλα τα στατική αρχεία από το φάκελο *\Content* για να είναι στο cache για 15 ημέρες.

Για περισσότερες πληροφορίες σχετικά με τον τρόπο ρύθμισης των παραμέτρων του `<clientCache>` στοιχείο, ανατρέξτε στο θέμα [Cache του προγράμματος-πελάτη &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

Στο [λειτουργήσει περιεχομένου από ενέργειες ελεγκτή μέσω Azure CDN](#controller), που θα εμφανιστούν και πώς μπορείτε να ρυθμίσετε ρυθμίσεις του cache για ελεγκτή ενέργεια αποτελέσματα στο CDN cache.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Εξυπηρέτηση περιεχομένου από ενέργειες ελεγκτή μέσω Azure CDN ##

Όταν ενσωματώνετε ένα ρόλο cloud υπηρεσίας Web με Azure CDN, είναι σχετικά εύκολο για να εξυπηρετήσει περιεχόμενο από ενέργειες ελεγκτή έως το CDN Azure. Εκτός από το σερβίρισμα την υπηρεσία cloud απευθείας από Azure CDN (επίδειξη παραπάνω), [Balliauw Μαρτίνος](https://twitter.com/maartenballiauw) δείχνει πώς μπορείτε να το κάνετε με μια MemeGenerator ελεγκτή [λανθάνων χρόνος μείωση στο web με το CDN Azure](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)διασκεδαστικό. Να θα απλώς αναπαραγάγετε το εδώ.

Ας υποθέσουμε ότι στην υπηρεσία cloud που θέλετε να δημιουργήσετε memes με βάση μια νέων εικόνα Chuck Norris (φωτογραφία με [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) ως εξής:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Έχετε μια απλή `Index` ενέργεια που επιτρέπει την στους πελάτες για να καθορίσετε το υπερθετικού στην εικόνα, στη συνέχεια, δημιουργεί το meme αφού δημοσιεύσουν με την ενέργεια. Εφόσον είναι Chuck Norris, που θα περιμένατε αυτήν τη σελίδα για να γίνετε τεράστιες δημοφιλείς καθολικά. Αυτό είναι ένα καλό παράδειγμα εξυπηρετεί ημι-δυναμικό περιεχόμενο με Azure CDN.

Ακολουθήστε τα παραπάνω βήματα για να ρυθμίσετε αυτή η ενέργεια του ελεγκτή:

1. Στο φάκελο *\Controllers* , δημιουργήστε ένα νέο αρχείο .cs που ονομάζεται *MemeGeneratorController.cs* και να αντικαταστήσετε το περιεχόμενο με τον ακόλουθο κώδικα. Φροντίστε να αντικαταστήσετε το επισημασμένο τμήμα με το όνομα CDN σας.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Κάντε δεξί κλικ στην προεπιλεγμένη `Index()` ενέργεια και επιλέξτε **Προσθήκη προβολής**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Αποδεχτείτε τις παρακάτω ρυθμίσεις και κάντε κλικ στην επιλογή **Προσθήκη**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Ανοίξτε το νέο *Views\MemeGenerator\Index.cshtml* και αντικαταστήστε το περιεχόμενο με το παρακάτω απλό HTML για την υποβολή του υπερθετικού:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Δημοσιεύστε ξανά την υπηρεσία cloud και μεταβείτε στις επιλογές * *http://*&lt;όνομα_υπηρεσίας >*.cloudapp.net/MemeGenerator/Index** στο πρόγραμμα περιήγησης.

Όταν υποβάλετε τις τιμές της φόρμας για να `/MemeGenerator/Index`, η `Index_Post` ενέργεια μέθοδος επιστρέφει μια σύνδεση προς το `Show` μέθοδο ενέργεια με το αντίστοιχο αναγνωριστικό εισόδου. Όταν κάνετε κλικ στη σύνδεση, να φτάσετε τον ακόλουθο κώδικα:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Εάν το τοπικό πρόγραμμα εντοπισμού σφαλμάτων είναι συνδεδεμένο, θα λάβετε την εμπειρία εντοπισμού σφαλμάτων κανονική με ένα τοπικό redirect. Εάν εκτελείται στην υπηρεσία cloud, στη συνέχεια, θα ανακατευθύνει σε:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Που αντιστοιχεί στην ακόλουθη διεύθυνση URL origin σε το τελικό σημείο CDN:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Μπορείτε να χρησιμοποιήσετε το `OutputCacheAttribute` χαρακτηριστικών σε το `Generate` μέθοδο για να καθορίσετε πώς το αποτέλεσμα ενέργεια θα πρέπει να είναι στο cache, που θα εκτελέσει Azure CDN. Ο παρακάτω κώδικας Καθορίστε μια λήξης cache 1 ώρα (3.600 δευτερόλεπτα).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Παρομοίως, που μπορεί να χρησιμοποιηθεί το περιεχόμενο από οποιαδήποτε ενέργεια ελεγκτή στην υπηρεσία cloud μέσω του Azure CDN, με την επιλογή που θέλετε σε cache.

Στην επόμενη ενότητα, που θα σας δείξει πώς για να εξυπηρετήσει τα ομαδοποιημένες και minified δεσμών ενεργειών και CSS μέσω Azure CDN.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Ενοποίηση του ASP.NET δέσμης και minification με Azure CDN ##

Οι δέσμες ενεργειών και CSS φύλλα στυλ δεν αλλάζουν συχνά και είναι δάνεια υποψήφιοι για το cache Azure CDN. Σερβίρισμα του ολόκληρο ρόλου Web μέσω του Azure CDN είναι ο ευκολότερος τρόπος για να ενοποιήσετε δέσμης και minification με Azure CDN. Ωστόσο, ενδέχεται να δεν θέλετε να το κάνετε αυτό, που θα σας δείξει πώς να το κάνετε ενώ διατηρώντας την επιθυμητή develper εμπειρία της δέσμης ASP.NET και minification, όπως:

-   Εμπειρία σε λειτουργία εξαιρετική εντοπισμού σφαλμάτων
-   Βελτιωμένο ανάπτυξης
-   Άμεση ενημερώσεις για προγράμματα-πελάτες για αναβαθμίσεις έκδοσης δέσμης ενεργειών/CSS
-   Μηχανισμός εναλλακτικών όταν αποτύχει η το τελικό σημείο CDN
-   Ελαχιστοποίηση τροποποίηση κώδικα

Στο έργο **WebRole1** που δημιουργήσατε στο [ενσωματώσουν τελικού σημείου Azure CDN με το Azure τοποθεσιών Web και σερβίρισμα στατικό περιεχόμενο στις ιστοσελίδες σας από το Azure CDN](#deploy), ανοίξτε το *App_Start\BundleConfig.cs* και Ρίξτε μια ματιά το `bundles.Add()` κλήσεων της μεθόδου.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Την πρώτη `bundles.Add()` δήλωση προσθέτει μια δέσμη ενεργειών στο στον εικονικό κατάλογο `~/bundles/jquery`. Στη συνέχεια, ανοίξτε *Views\Shared\_Layout.cshtml* για να δείτε τον τρόπο απόδοσης την ετικέτα δέσμη ενεργειών. Θα πρέπει να μπορείτε να βρείτε την ακόλουθη γραμμή Razor κώδικα:

    @Scripts.Render("~/bundles/jquery")

Κατά την εκτέλεση αυτόν τον κωδικό Razor στο ρόλο Azure Web, θα απόδοση ενός `<script>` ετικέτα για το πακέτο δέσμης ενεργειών με την εξής:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Ωστόσο, όταν εκτελείται στο Visual Studio, πληκτρολογώντας `F5`, αυτό θα απόδοση κάθε αρχείο δέσμης ενεργειών στο τη δέσμη μεμονωμένα (στην περίπτωση που παραπάνω, μόνο μία δέσμη ενεργειών αρχείο έχει τη δέσμη):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Αυτό σας επιτρέπει να εντοπισμού σφαλμάτων στον κώδικα JavaScript στο περιβάλλον ανάπτυξής σας ενώ μείωση συνδέσεις ταυτόχρονες προγράμματος-πελάτη (δέσμης) και τη βελτίωση της αρχείο λήψη επιδόσεων (minification) παραγωγής. Είναι μια εξαιρετική δυνατότητα για να διατηρήσετε με ενσωμάτωση του Azure CDN. Επιπλέον, επειδή το πακέτο αποδίδονται περιέχει ήδη μια συμβολοσειρά που δημιουργείται αυτόματα την έκδοση, που θέλετε να αναπαραγάγετε αυτήν τη λειτουργικότητα έτσι ώστε το κάθε φορά που ενημερώνετε την έκδοση jQuery έως NuGet, μπορεί να ενημερωθεί στην πλευρά του προγράμματος-πελάτη το συντομότερο δυνατό.

Ακολουθήστε τα παρακάτω βήματα για να δέσμης ASP.NET ενοποίηση και minification με το τελικό σημείο CDN.

1. Επιστροφή στο *App_Start\BundleConfig.cs*, τροποποιήστε το `bundles.Add()` μεθόδους για να χρησιμοποιήσετε μια διαφορετική [δέσμη κατασκευή](http://msdn.microsoft.com/library/jj646464.aspx), ένα που καθορίζει μια διεύθυνση CDN. Για να το κάνετε αυτό, αντικαταστήστε το `RegisterBundles` μέθοδο ορισμού με τον ακόλουθο κώδικα:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Φροντίστε να αντικαταστήσετε `<yourCDNName>` με το όνομα του σας CDN Azure.

    Με απλά λόγια, ορίζετε `bundles.UseCdn = true` και προσθέσει μια προσεκτικά γραμμένη διεύθυνση URL CDN για κάθε πακέτο. Για παράδειγμα, η πρώτη κατασκευή στον κώδικα:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    είναι η ίδια με:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Αυτήν την κατασκευή ενημερώνει δέσμης ASP.NET και minification απόδοση αρχεία μεμονωμένα δέσμης ενεργειών όταν διορθώνεται τοπικά, αλλά χρησιμοποιήστε τη συγκεκριμένη διεύθυνση CDN για να αποκτήσετε πρόσβαση στη δέσμη ενεργειών εν λόγω. Ωστόσο, σημειώστε δύο σημαντικά χαρακτηριστικά με αυτήν τη διεύθυνση URL προσεκτικά γραμμένη CDN:

    -   Η προέλευση για αυτήν τη διεύθυνση URL CDN είναι `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, που είναι στην πραγματικότητα στον εικονικό κατάλογο της τη δέσμη ενεργειών στην υπηρεσία cloud.
    -   Επειδή χρησιμοποιείτε CDN κατασκευή, την ετικέτα δέσμης ενεργειών CDN για το πακέτο δεν περιέχει πλέον τη συμβολοσειρά που δημιουργείται αυτόματα την έκδοση του αποδίδονται διεύθυνσης URL. Χρειάζεται να δημιουργήσετε μια μοναδική έκδοση συμβολοσειρά με μη αυτόματο τρόπο κάθε φορά που τροποποιείται τη δέσμη ενεργειών για να επιβάλετε μια αποτυχημένων cache στο σας CDN Azure. Την ίδια στιγμή, αυτή η συμβολοσειρά μοναδική έκδοση πρέπει να παραμένει σταθερή έως τη διάρκεια ζωής της ανάπτυξης για να μεγιστοποιήσετε αιτήσεις στο cache στο σας CDN Azure μετά την ανάπτυξη της δέσμης.
    -   Η συμβολοσειρά ερωτήματος v = < W.X.Y.Z > συγκεντρώνει από *Properties\AssemblyInfo.cs* στο έργο σας Web ρόλο. Μπορείτε να έχετε μια ροή εργασίας ανάπτυξης που περιλαμβάνει τα βηματικής την έκδοση συγκρότησης κάθε φορά που δημοσιεύετε σε Azure. Ή, μπορείτε απλώς να τροποποιήσετε *Properties\AssemblyInfo.cs* στο έργο σας να αυξάνονται αυτόματα τη συμβολοσειρά έκδοσης κάθε φορά που δημιουργείτε, χρήση του χαρακτήρα μπαλαντέρ "*". Για παράδειγμα:

            [assembly: AssemblyVersion("1.0.0.*")]

        Τυχόν άλλες στρατηγικής για να βελτιστοποιήσετε δημιουργεί μια μοναδική συμβολοσειρά για τη διάρκεια ζωής μιας ανάπτυξης λειτουργούν εδώ.

3. Δημοσιεύστε ξανά την υπηρεσία cloud και μεταβείτε στην αρχική σελίδα.

4. Προβάλετε τον κώδικα HTML για τη σελίδα. Θα πρέπει να μπορείτε να δείτε τη διεύθυνση URL CDN απόδοσης, με μια συμβολοσειρά μοναδικό έκδοσης κάθε φορά που αναδημοσιεύετε αλλαγές για την υπηρεσία cloud. Για παράδειγμα:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Στο Visual Studio, εντοπισμός σφαλμάτων την υπηρεσία cloud στο Visual Studio, πληκτρολογώντας `F5`.,

6. Προβάλετε τον κώδικα HTML για τη σελίδα. Εξακολουθείτε να βλέπετε κάθε αρχείο δέσμης ενεργειών μεμονωμένα απόδοσης, έτσι ώστε να μπορείτε να έχετε μια συνεπή εντοπισμού σφαλμάτων εμπειρία στο Visual Studio.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Μηχανισμός εναλλακτικών για διευθύνσεις URL CDN ##

Όταν το τελικό σημείο Azure CDN αποτύχει για οποιονδήποτε λόγο, θέλετε την ιστοσελίδα σας να είναι αρκετά Έξυπνες για να αποκτήσετε πρόσβαση σε διακομιστή Web origin ως εναλλακτική επιλογή για τη φόρτωση JavaScript ή εκκίνησης. Είναι αρκετά σοβαρά ώστε να χάσετε εικόνων στην τοποθεσία Web λόγω μη διαθεσιμότητα CDN, αλλά πολύ πιο σημαντική απώλεια λειτουργικότητας κρίσιμης σημασίας σελίδας που παρέχεται από τις δέσμες ενεργειών και φύλλα στυλ.

Η κλάση [πακέτου](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) περιέχει μια ιδιότητα που ονομάζεται [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) που σας επιτρέπει να ρυθμίσετε τις παραμέτρους εναλλακτικών μηχανισμό CDN αποτυχία. Για να χρησιμοποιήσετε αυτήν την ιδιότητα, ακολουθήστε τα παρακάτω βήματα:

1. Στο έργο σας ρόλο Web, ανοίξετε *App_Start\BundleConfig.cs*, όπου έχετε προσθέσει μια διεύθυνση URL CDN σε κάθε [πακέτο κατασκευή](http://msdn.microsoft.com/library/jj646464.aspx), και να κάνετε τις ακόλουθες αλλαγές επισήμανση για να προσθέσετε εναλλακτικών μηχανισμό τα πακέτα προεπιλεγμένη:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Όταν `CdnFallbackExpression` είναι δεν είναι null, δέσμη ενεργειών έχει εισαχθεί στην HTML για να ελέγξετε εάν έχει φορτωθεί με επιτυχία τη δέσμη και, εάν όχι, πρόσβασης τη δέσμη απευθείας από το διακομιστή Web προέλευσης. Αυτή η ιδιότητα πρέπει να οριστούν σε μια παράσταση JavaScript που ελέγχει εάν το αντίστοιχο πακέτο CDN φορτώνεται σωστά. Η παράσταση που χρειάζεται για να ελέγξετε κάθε πακέτου διαφέρει ανάλογα με το περιεχόμενο. Για το παραπάνω προεπιλεγμένες ομάδες:

    -   `window.jquery`έχει οριστεί στο .js jquery-{έκδοση}
    -   `$.validator`έχει οριστεί στο jquery.validate.js
    -   `window.Modernizr`έχει οριστεί στο .js modernizer-{έκδοση}
    -   `$.fn.modal`έχει οριστεί στο bootstrap.js

    Που ίσως έχετε παρατηρήσει ότι που δεν έχει ορίσει CdnFallbackExpression για το `~/Cointent/css` πακέτου. Αυτό συμβαίνει επειδή αυτήν τη στιγμή υπάρχει ένα [σφάλμα σε System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) που εισάγει μια `<script>` ετικέτα για τα CSS εναλλακτικών αντί για την αναμενόμενη `<link>` ετικέτα.

    Ωστόσο, υπάρχει μια καλή [Επιστροφή πακέτου στυλ](https://github.com/EmberConsultingGroup/StyleBundleFallback) που παρέχεται από την [Ομάδα συμβουλευτικές Ember](https://github.com/EmberConsultingGroup).

2. Για να χρησιμοποιήσετε τη λύση CSS, δημιουργήστε ένα νέο αρχείο .cs στο έργο σας Web ρόλο *App_Start* φάκελο που ονομάζεται *StyleBundleExtensions.cs*και αντικαταστήστε το περιεχόμενο με τον [κωδικό από GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. Στο *App_Start\StyleFundleExtensions.cs*, Μετονομασία χώρου ονομάτων στο όνομα του ρόλου σας στο Web (π.χ. **WebRole1**).

3. Επιστρέψτε στο `App_Start\BundleConfig.cs` και την τελευταία τροποποίηση `bundles.Add` δήλωση με τον ακόλουθο κώδικα επισήμανση:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Αυτή η νέα μέθοδος επέκταση χρησιμοποιεί το ίδιο ιδέα για την εισαγωγή δέσμης ενεργειών στο HTML για να ελέγξετε το DOM για το ένα αντίστοιχο όνομα κλάσης όνομα κανόνα και τιμή κανόνα που ορίζεται στο πακέτο CSS και εμπίπτει στο διακομιστή Web προέλευσης, εάν αποτύχει για να βρείτε τη συμφωνία.

4. Δημοσιεύστε ξανά την υπηρεσία cloud και μεταβείτε στην αρχική σελίδα.

5. Προβάλετε τον κώδικα HTML για τη σελίδα. Πρέπει να βρείτε έχει εισαχθεί δέσμες ενεργειών παρόμοιο με το εξής:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Σημειώστε ότι έχει εισαχθεί δέσμη ενεργειών για τη δέσμη CSS περιέχει ακόμη το errant υπόλειμμα από το `CdnFallbackExpression` ιδιότητα στη γραμμή:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Αλλά από το πρώτο τμήμα του το || παράσταση θα επιστρέφει πάντα true (στη γραμμή ακριβώς επάνω από που), η συνάρτηση document.write() θα εκτελεστεί ποτέ.

## <a name="more-information"></a>Περισσότερες πληροφορίες ##
- [Επισκόπηση του δικτύου Azure παροχής περιεχομένου (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Χρήση του Azure CDN](cdn-create-new-endpoint.md)
- [Δέσμης ASP.NET και Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
